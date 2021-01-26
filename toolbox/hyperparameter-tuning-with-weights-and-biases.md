# Настройка гиперпараметров с Weights & Biases

Платформа позволяет проводить обучение модели параллельно с различными комбинациями гиперпараметров через интеграцию с [Weights & Biases](https://www.wandb.com/). W&B - это инструмент отслеживания экспериментов для глубокого обучения. Инженеру ML нужно только инициировать процесс: подготовить код для обучения модели, настроить пространство гиперпараметров и начать поиск только одной командой. Все остальное сделает платформа.

Чтобы увидеть данное руководство в действии, ознакомьтесь с нашим [рецептом](https://github.com/neuromation/ml-recipe-hyperparam-wandb), в котором мы применяем настройку гиперпараметров W&B к задаче классификации изображений.

### Создание проекта

Шаблон проекта содержит интеграцию с Weights and Biases. Чтобы создать новый проект из шаблона, необходимо выполнить несколько шагов.

Сначала [зарегистрируйтесь](https://neu.ro/) и [установите клиент CLI](https://docs.neu.ro/getting-started#installing-cli)

Затем создайте проект:

```text
neuro project init
```

Эта команда задаст Вам несколько простых вопросов:

```text
project_name \[Neuro Project]: Hyperparameter tuning test
project_slug \[hyperparameter-tuning-test]:
code_directory \[modules]:
```

Нажмите `Enter`, если вы не хотите менять предложенное значение.

Затем поменяйте рабочую директорию:

```text
cd hyperparameter-tuning-test
```

### Подключение Weights & Biases

Теперь подключите Ваш проект к [Weights & Biases](https://www.wandb.com/):

* [Зарегистрируйтесь на W&B](https://app.wandb.ai/login?signup=true).
* На странице [W&B’s settings page](https://app.wandb.ai/settings) \(раздел “API keys”\) найдите Ваш API key \(он также называется _токеном_\). Он должен выглядеть таким образом: `cf23df2207d99a74fbe169e3eba035e633b65d94`.
* Сохраните ваш API key \(токен\) в файле в локальном каталоге `~` и добавьте его как секрет на платформу:

```text
export WANDB_SECRET_FILE=wandb-token.txt
echo "cf23df2207d99a74fbe169e3eba035e633b65d94" > ~/$WANDB_SECRET_FILE
neuro secret add wandb-token @~/$WANDB_SECRET_FILE
```

* Загрузите `hypertrain.yml` [отсюда](https://github.com/neuro-inc/ml-recipe-hyperparam-wandb/blob/master/.neuro/hypertrain.yml) в `.neuro/hypertrain.yml` в папке Вашего проекта.

Вы можете узнать больше об использовании Weights & Biases в коде из [документации W&B](https://docs.wandb.com/library/api/examples) и [примеров проектов ](https://github.com/wandb/examples)[W](https://github.com/wandb/examples)[&B](https://github.com/wandb/examples).

### Использование Weights & Biases для настройки гиперпараметров

Если Вы выполнили предыдущие части данного руководства, W&B готов к использованию. Чтобы запустить настройку гиперпараметров для модели, Вам необходимо:

* определить список гиперпараметров \(в файле `config/wandb-sweep.yaml`\), и
* отправлять метрики в W&B после каждого запуска \(используя команду `neuro-flow bake hypertrain`\).

`.neuro/hypertrain.yml` и `config/wandb-sweep.yaml` ссылаются на `train.py`. Если Вы хотите запустить `hypertrain` для другого скрипта, вы можете изменить характеристику `program` в `config/wandb-sweep.yaml` \(см. ниже\). Скрипт должен содержать описание модели и цикл обучения.

Скрипт Python также должен получать параметры в качестве аргументов командной строки с такими же именами, как указано в `config/wandb-sweep.yaml` и использовать их для обучения/оценки модели. Например, Вы можете использовать параметры командной строки, как в модуле Python [argparse](https://docs.python.org/3/library/argparse.html).

Более подробная информация:

`train.py` - это файл, содержащий код для обучения модели. В нем должны логироваться метрики из W&B, например, в нашем случае:

```text
wandb.log({'accuracy': 0.9})
```

`config/wandb-sweep.yaml` имеет следующую структуру:

```text
program: /project/src/train.py
method: grid
metric:
  name: accuracy01/valid
  goal: maximize
parameters:
  lr:
    values: [0.1, 0.01, 0.001]
  optimizer:
    values: ['sgd', 'adam']
  scheduler:
    values: ['const', 'cosine']
```

* Line 1: `/../train.py` - путь по умолчанию к файлу с обучающим кодом модели.
* Line 2: метод, который используется для настройки гиперпараметров. Для получения дополнительной информации см. документацию W&B.
* Lines 4, 5: имя метрики, которая должна быть оптимизирована. Инженер ML может изменить метрику в соответствии с заданием.
* Lines 6 -12: настройки гиперпараметров. Их необходимо использовать в файле `train.py`. Имена, значения и диапазоны могут быть изменены.

Имя файла `wandb-sweep.yaml` и путь к нему можно изменить в `.neuro/hypertrain.yml` \(найдите секцию`WANDB_SWEEPS_FILE=...` внутри определения задания `start_sweep`\).

### Настройка гиперпараметров

Теперь, когда Вы настроили платформу и W&B и подготовили скрипт для обучения модели, можно попробовать сделать настройку гиперпараметров. Для этого необходимо выполнить следующую команду:

```text
neuro-flow bake hypertrain --param token_secret_name wandb-token
```

Данная команда параллельно выполняет задания, которые приводят в действие скрипт `train.py` \(или скрипт с любым другим выбранным Вами именем\), с различными наборами гиперпараметров. По умолчанию одновременно выполняется только 2 задания. Вы можете изменить это число, изменив список `id` внутри определения задания `worker_...` в файле `.neuro/hypertrain.yml`

```text
tasks:
...
  - id: worker_$[[ matrix.id ]]
    image: $[[ images.myimage.ref ]]
    preset: gpu-k80-small-p
    needs: [start_sweep]
    strategy:
      matrix:
        id: [1, 2] # <- e.g., replace with [1, 2, 3, 4, 5]
```

Чтобы отслеживать процесс настройки гиперпараметров, перейдите по ссылке, которую W&B предоставляет в начале работы.

Если вы хотите остановить настройку гиперпараметров, прекратите все связанные задания:

```text
neuro-flow kill hypertrain
```

Убедитесь, что задания остановлены, выполнив команду `neuro-flow ps`.

