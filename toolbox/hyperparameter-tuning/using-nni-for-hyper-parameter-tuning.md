---
description: >-
  Данная страница немного устарела. Наша команда технических писателей работает
  над ней.
---

# Настройка гиперпараметров с NNI

## Введение

В данном руководстве Вы узнаете, как использовать [NNI](https://github.com/microsoft/nni) \(open-source инструмент от Microsoft\) для настройки гиперпараметров на платформе. Вы создадите новый проект, интегрируете его с NNI и, чтобы ускорить поиск, запустите несколько процессов по настройке.

Перед тем, как приступать к следующим шагам, убедитесь, что у Вас установлен [CLI](../../first-steps/getting-started.md#installing-cli).

## Создание проекта

Для создания проекта, выполните команду:

```bash
neuro project init
cd <project-slug>
```

## Подготовка кода для эксперимента и интеграция с платформой

Мы собираемся использовать пример кода [NNI example code](https://github.com/microsoft/nni/tree/master/examples/trials/mnist-tfv2) с набором данных MNIST. Поместите файл [mnist.py](https://github.com/microsoft/nni/blob/master/examples/trials/mnist-tfv2/mnist.py) в папку `modules`, а файл [search\_space.json](https://github.com/microsoft/nni/blob/master/examples/trials/mnist-tfv2/search_space.json) в папку `config`.

Затем добавьте следующие записи в файл `requirements.txt`:

```bash
nni==2.0 # Required for Hyper-parameter search
neuro-sdk # Required by Neu.ro NNI integration script
Jinja2>=2.11.2 # Required by Neu.ro NNI integration script
```

Теперь мы готовы собрать наш образ:

```bash
neuro-flow build myimage
```

Пока Docker создает наш образ, мы можем продолжить настройку интеграции NNI.

Добавьте следующие записи _в конец_ файла `.neuro/live.yml`:

```bash
  worker:
    image: $[[ images.myimage.ref ]]
    life_span: 1d
    multi: true
    detach: true
    volumes:
      - $[[ volumes.code.ref_ro ]]
      - $[[ volumes.config.ref_ro ]]
      - $[[ volumes.project.ref ]]
    env:
      EXPOSE_SSH: "yes"
    bash: |
      sleep infinity

  master:
    image: $[[ images.myimage.ref ]]
    life_span: 1d
    http_port: 8080
    http_auth: false
    pass_config: true
    detach: true
    browse: true
    volumes:
      - $[[ volumes.code.ref_ro ]]
      - $[[ volumes.config.ref ]]
      - $[[ volumes.project.ref ]]
    bash: |
      cd $[[ volumes.config.mount ]] && python3 prepare-nni-config.py 
      cd $[[ volumes.project.mount ]] && USER=root nnictl create --config $[[ volumes.config.mount ]]/nni-config.yml -f
```

Это добавит несколько новых заданий для работы с NNI.

Наконец, поместите [`nni-config-template.yml`](https://github.com/neuromation/ml-recipe-nni/blob/master/config/nni-config-template.yml) и [`prepare-nni-config.py`](https://github.com/neuromation/ml-recipe-nni/blob/master/config/prepare-nni-config.py) в папку `config` и [Makefile](https://github.com/neuro-inc/ml-recipe-nni/blob/master/Makefile) в корневую папку Вашего проекта.

После завершения работы команды `neuro-flow build myimage` проект готов к запуску на платформе.

## Запуск заданий по настройке

Осталось выполнить команду

```bash
make hypertrain
```

Данная команда делает следующее:

* Запускает 3 узла с предустановкой `gpu-k80-small-p`. Оба параметра могут быть настроены через переменные `N_JOBS` в файле Makefile и `preset` в файле `.config/live.yaml` соответственно.
* Запускает мастер-узел с пресетом `cpu-small`.
* Автоматически генерирует файл конфигурации NNI для мастер-узла, указывающий на процессы.
* Запускает процесс обучения и автоматически открывает веб-интерфейс NNI в Вашем браузере.

С помощью веб-интерфейса вы можете отслеживать ход эксперимента и промежуточные результаты. Когда процессы завершатся, Вы можете получить окончательные значения гиперпараметра и загрузить файлы логирования, если это необходимо.

![NNI Hyperparameter Tuning GUI](../../.gitbook/assets/screen-shot-2020-05-12-at-12.43.02-pm.png)

Когда Вы закончите работу, Вы можете завершить оба процесса и мастер-узел с помощью команды:

```bash
neuro-flow kill ALL
```

