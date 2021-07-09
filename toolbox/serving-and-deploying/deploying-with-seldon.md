# Выкладка моделей с Seldon

## Введение

В этом руководстве показано, как обучить и выложить [MNIST](http://yann.lecun.com/exdb/mnist/)-модель с помощью [Seldon](https://www.seldon.io/) - здесь мы основываемся на [базовом примере PyTorch](https://github.com/pytorch/examples/tree/master/mnist). 

Мы выполним следующие шаги:

1. Подготовим и обучим базовую модель на платформе;
2. Обернём модель в HTTP-сервер для системы выводов;
3. Проверим работу системы выводов на платформе;
4. Запустим систему выводов на существующий экземпляр Seldon Core.

## Предварительные требования

Для начала, на вашей системе должны быть установлены `neuro-cli` и `neuro-extras`:

```text
# Установка `neuro-cli` и 'neuro-extras'
$ pip install neuro-cli neuro-extras
```

Также удостоверьтесь, что вы вошли в аккаунт платформы:

```text
$ neuro login
```

Вам также потребуется работающий экземпляр [Seldon Core](https://www.seldon.io/tech/products/core/). Предполагается, что у вас локально настроен `kubectl` для того, чтобы создать все необходимые ресурсы K8S.

## Обучение

Сначала потребуется скопировать два файла из репозитория, упомянутого в введении:

```text
$ curl -O "https://raw.githubusercontent.com/pytorch/examples/master/mnist/main.py"
$ curl -O "https://raw.githubusercontent.com/pytorch/examples/master/mnist/requirements.txt"
```

Проверяя содержание файла `main.py`, можно увидеть, что путь к конечной сериализованной модели прописан прямо в коде:

```text
    if args.save_model:
        torch.save(model.state_dict(), "mnist_cnn.pt")
```

Это не даёт достаточно гибкости для наших целей, так что в идеале следовало бы изменить код и прописать дополнительную опцию, позволяющую указывать желаемый путь в команде. Но, в случае этого руководства, для простоты мы сделаем иначе - скопируем конечную модель, как только закончится процесс обучения.

Сериализованная модель должна быть скопирована в примонтированный том дискового пространства по выбранному пути. Для этого нам придётся написать Dockerfile для задания по обучению и сохранить его как `train.Dockerfile` для последующего использования. Наш образ будет основан на заранее собранном образе PyTorch. Полный список таких образов может быть найден на [PyTorch DockerHub](https://hub.docker.com/r/pytorch/pytorch/tags).

```text
FROM pytorch/pytorch:1.5-cuda10.1-cudnn7-runtime

ENV MODEL_PATH=/var/storage/model.pkl

COPY main.py .

CMD bash -c "python main.py --save-model; mv mnist_cnn.pt $MODEL_PATH"
```

Как можно увидеть, CMD выполняет две операции: 

1. Обучает модель и сохраняет ее по стандартному пути
2. Копирует сохранённую модель в примонтированный том дискового пространства

Теперь сожно собирать образ. Следующая команда не требует локально запущенного Docker Engine. Процесс сборки будет проходить на платформе.

```text
$ neuro-extras image build -f train.Dockerfile . image:examples/mnist:train
...
INFO: Successfully built image:examples/mnist:train
```

Эта команда даёт платформе инструкцию копировать контекст сборки \(в данном случае, текущую папку `.`\), использовать шаги сборки из `train.Dockerfile` и сохранить полученный образ `image:examples/mnist:train` на реестре платформы. Проверить содержимое реестра можно с помощью такой команды:

```text
$ neuro image tags image:examples/mnist 
image:examples/mnist:train
```

Так как образ теперь доступен внутри платформы, можно запустить задание по обучению.

```text
$ neuro run -s gpu-small -v storage:examples/mnist:/var/storage \
    -e MODEL_PATH=/var/storage/model.pkl image:examples/mnist:train
...
Test set: Average loss: 0.0265, Accuracy: 9910/10000 (99%)
```

Заметьте, что здесь мы явно указываем переменную среды `MODEL_PATH`, которая ведёт к примонтированному тому дискового пространства. Это задание занимает около 4 минут при запуске на пресете `gpu-k80-small`.

Теперь можно проверить, попала ли сериализованная модель на дисковое пространство:

```text
$ neuro ls -l storage:examples/mnist
-m 4800957 2020-05-04 15:21:18 model.pkl
```

## Выкладка модели

Так как модель прошла процесс обучения, можно начать её использовать.

### Сборка сервера

`neuro-extras` предоставляет хорошую основу для обёртывания кода модели в функциональный HTTP-сервер для системы выводов, который можно запускать на платформе или любой среде контейнеров.

```text
$ neuro-extras seldon init-package .

ls -l
-rw-r--r--  1 user  group  2375 May  4 15:33 README.md
-rw-r--r--  1 user  group  5136 May  4 14:36 main.py
-rw-r--r--  1 user  group    18 May  4 14:37 requirements.txt
-rwx------  1 user  group   517 Apr 30 18:21 seldon.Dockerfile
-rw-r--r--  1 user  group  1193 Apr 30 18:22 seldon_model.py
-rw-r--r--  1 user  group   176 May  4 15:12 train.Dockerfile
```

Эта команда создаёт два файла:

1. `seldon_model.py` - модуль Python, ответственный за имплементацию интерфейса, ожидаемого Seldon Core. Обычно этот модуль считывает сериализованную модель из примонтированного тома дискового пространства, десериализует эту модель и использует её для предсказания на входящих элементах информации.
2. `seldon.Dockerfile` - заранее установленный Dockerfile, собирающий ваш код и HTTP-сервер выводов Seldon Core для последующего использования.

Сначала реализуем интерфейс. Требуется добавить некоторые отсутствующие imports в верхней части файла `seldon_model.py`:

```text
import io

import torch
from PIL import Image
from torchvision import transforms

from .main import Net
```

После этого надо заменить существующий метод пустого конструктора процедурой загрузки модели.

```text
    def __init__(self):
        self._model = Net()
        self._model.load_state_dict(
            torch.load("/storage/model.pkl", map_location=torch.device("cpu"))
        )
        self._model.eval()
```

Заметьте, что вышеприведенный код предполагает, что модель находится по конкретному пути. Следует примонтировать том дискового пространства в `/storage`, чтобы код работал корректно. Ещё один момент, который стоит упомянуть - это то, что мы будем запускать систему выводов на CPU.

Аналогично, нам потребуется изменить метод `predict`. Так как в этом примере мы рассматриваем проблему классификации изображений, нам потребуется, чтобы HTTP-сервер выводов мог получать изображения как двоичные данный в `bytes`, предсказывать классы и возвращать JSON-документ с конечными оценками.

```text
    def predict(self, X, features_names):
        data = transforms.ToTensor()(Image.open(io.BytesIO(X)))
        return self._model(data[None, ...]).detach().numpy()
```

Теперь можно собрать образ для системы выводов по аналогии с образом для обучения:

```text
$ neuro-extras image build -f seldon.Dockerfile . image:examples/mnist:seldon
```

```text
$ neuro image tags image:examples/mnist 
image:examples/mnist:train
image:examples/mnist:seldon
```

Как видим, на реестре появился новый образ.

### Запуск сервера на платформе

Перед выгрузкой собранной и обученной модели на production, посмотрим, как она работает в рамках платформы. Нам потребуется запустить задание, открывающее порт `5000` \(стандартный порт для HTTP-сервера Seldon Core\) и монтирующее том дискового пространства с сериализованной моделью по пути, упомянутому ранее.

```text
$ neuro run -n example-mnist --http 5000 --no-http-auth --detach -v storage:examples/mnist:/storage:ro image:examples/mnist:seldon
```

Обратите внимание на результат этой команды. Скопируйте значение поля HTTP URL для последующего формирования команды `curl`.

После запуска заданифя и выгрузки модели, можно начать давать ей тестовые данные. Мы используем `.jpg`-изображение из набора данных MNIST: [![img\_103.jpg](https://github.com/neuro-inc/neuro-examples/raw/master/mnist/img_103.jpg)](https://github.com/neuro-inc/neuro-examples/blob/master/mnist/img_103.jpg)

Предполагая, что значение HTTP URL было `https://example-mnist--user.jobs.neuro-ai-public.org.neu.ro/`, мы можем составить команду `curl`. HTTP-сервер Seldon Core ожидает двоичные данные, посланные в форме `binData`:

```text
$ curl -F binData=@img_103.jpg https://example-mnist--user.jobs.neuro-ai-public.org.neu.ro/predict
{"data":{"names":["t:0","t:1","t:2","t:3","t:4","t:5","t:6","t:7","t:8","t:9"],"tensor":{"shape":[1,10],"values":[-3.3279592990875244,-2.659998655319214,-0.39225172996520996,-2.8468592166900635,-5.054018020629883,-5.108893394470215,-4.198001861572266,-2.7248833179473877,-2.8381588459014893,-4.701035499572754]}},"meta":{}}
```

элемент массива с индексом `2` имеет наибольшую оценку, как и ожидалось.

Если задание по обучению больше не нужно, его можно остановить, освобождая таким образом ранее занятые ресурсы:

```text
$ neuro kill example-mnist
```

### Запуск сервера на Seldon Core

После успешной проверки нашего сервера выводов, нужно будет выгрузить результат в production.

Так как ваш развёрнутый экземпляр Seldon Core обычно находится за пределами платформы \(например, на вашем локальном кластере K8S\), нам нужно дать инструкцию K8S и Seldon загрузить новый образ модели и соответствующую сериализованную модель на кластер. `neuro-extras` позволяет быстро создать нужные ресурсы:

```text
# Creating a dedicated namespace to keep all the related resources together
$ kubectl create namespace seldon

# Creating a registry secret for acccessing Neu.ro registry from within K8S
$ neuro-extras k8s generate-registry-secret | kubectl -n seldon apply -f -

# Creating a secret for accessing Neu.ro (Storage etc) from within K8S
$ neuro-extras k8s generate-secret | kubectl -n seldon apply -f -

# Creating a Seldon deployment using the URIs we prepared beforehand
$ neuro-extras seldon generate-deployment image:examples/mnist:seldon \
    storage:examples/mnist/model.pkl | kubectl -n seldon apply -f -
```

Теперь можно проверить наш production-среду. Используем такую же `curl`-команду, как и ранее, но изменим URL. Он должен вести к вашему шлюзу K8S ingress.

```text
$ curl -F binData=@img_103.jpg "http://<GATEWAY>/seldon/seldon/neuro-model/api/v1.0/predictions"
{"data":{"names":["t:0","t:1","t:2","t:3","t:4","t:5","t:6","t:7","t:8","t:9"],"tensor":{"shape":[1,10],"values":[-3.3279592990875244,-2.659998655319214,-0.39225172996520996,-2.8468592166900635,-5.054018020629883,-5.108893394470215,-4.198001861572266,-2.7248833179473877,-2.8381588459014893,-4.701035499572754]}},"meta":{}}
```

Как и ранее, можем видеть, что результат - `2` и среда работает корректно.

