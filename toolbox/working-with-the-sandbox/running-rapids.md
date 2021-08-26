# Запуск RAPIDS

[RAPIDS](https://rapids.ai/) - наюор инструментов от NVIDIA, позволяющий подготавливать и запускать end-to-end пайплайны по анализу данных на GPU. Он имплементирован в виде Python-библиотеки.

To use RAPIDS on Neu.ro, you can run it in conjunction with Jupyter.

## Быстрый запуск

Мы создали [заранее настроенный проект](https://github.com/neuro-inc/ml-recipe-rapids),  который вы можете скачать и использовать для запуска RAPIDS.

Просто скопируйте репозиторий и запустите следующую команду в CLI:

```text
$ neuro-flow run jupyter
```

Это откроет Jupyter в вашем браузере, и в нём вы сможете начать работу с RAPIDS.

## Запуск RAPIDS в вашем проекте

Чтобы запустить RAPIDS в вашем проекте, добавьте следующее описание действия в файле `.neuro/live.yml` file:

```text
kind: live
title: <имя_действия>

jobs:
  jupyter:
    title: "<имя_задания>"
    image: rapidsai/rapidsai:cuda10.1-runtime-ubuntu18.04-py3.8
    preset: gpu-v100-small
    http_port: 8888
    http_auth: false
    browse: True
    detach: True
    life_span: 3h
```

Теперь с помощью команды `neuro-flow run jupyter` можно будет запустить экземпляр Jupyter, содержащий нобходимые блокноты RAPIDS из образа `rapidsai/rapidsai:cuda10.1-runtime-ubuntu18.04-py3.8`.

