# Развертывание моделей с TorchServe

### Введение

В этом руководстве Вы узнаете об использовании [TorchServe](https://pytorch.org/serve/) \(гибкий инструмент для развёртывания моделей PyTorch\) на платформе. 

Перед тем, как приступать к следующим шагам, убедитесь, что у Вас установлен [Neu.ro CLI](../getting-started.md#installing-cli).

### Подготовка локальных файлов

Для начала, склонируйте [репозиторий TorchServe](https://github.com/pytorch/serve):

```text
> git clone git@github.com:pytorch/serve.git
```

Вы также можете использовать такой вариант этой команды:

```text
> git clone https://github.com/pytorch/serve.git
```

Затем, скопируйте модель, которую вы собираетесь развёртывать, в локальный репозиторий. В данном примере мы будем использовать [DenseNet](https://pytorch.org/hub/pytorch_vision_densenet/):

```text
> curl -o serve/examples/image_classifier/densenet161-8d451a50.pth https://download.pytorch.org/models/densenet161-8d451a50.pth
```

Теперь можно скопировать фалы из локального репозитория на дисковое пространство платформы:

```text
> neuro cp -r serve storage:
```

### Развёртывание модели

Так как все требуемые файлу уже находятся на дисковом пространстве платформы, Вы можете монтировать их к Вашим заданиям, как том. Запустите следующую команду:

```text
> neuro run --name serve --volume storage:serve:/home/serve:rw --preset gpu-small --http 8080 --no-http-auth --detach pytorch/torchserve:0.1.1-cuda10.1-cudnn7-runtime
```

Теперь войдите в терминал bash из только что запущенного задания:

```text
> neuro exec serve bash
```

Создайте папку `model-store`:

```text
> mkdir /home/serve/model-store
```

Запустите [Torch Model Archiver](https://pypi.org/project/torch-model-archiver/) на Вашей модели:

```text
> torch-model-archiver --model-name densenet161 --version 1.0 --model-file /home/serve/examples/image_classifier/densenet_161/model.py --serialized-file /home/serve/examples/image_classifier/densenet161-8d451a50.pth --export-path /home/serve/model-store --extra-files /home/serve/examples/image_classifier/index_to_name.json --handler image_classifier
```

Перед развёртыванием модели, запустите:

```text
> torchserve --stop
```

Теперь Вы можете развёртывать модель. Следующие команды помогут развернуть `densenet161`:

```text
>torchserve --start --ncs --model-store /home/serve/model-store --models densenet161.mar
 curl -O https://raw.githubusercontent.com/pytorch/serve/master/docs/images/kitten_small.jpg
 curl -v https://serve--your-user-name.jobs.neuro-compute.org.neu.ro/predictions/densenet161 -T kitten_small.jpg
```

### Доступ к API управления

Для доступа к API управления модели, сначала пробросьте порт к заданию:

```text
> neuro port-forward serve 8081:8081
```

После этого, Вы можете скопировать папку `densenet161` на Ваш локальный компьютер:

```text
> curl http://localhost:8081/models/densenet161
```

