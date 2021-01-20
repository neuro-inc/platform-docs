---
description: >-
  Данная страница немного устарела. Наша команда технических писателей работает
  над ней.
---

# Настройка гиперпараметров с NNI

## Введение

В данном руководстве Вы узнаете, как использовать [NNI](https://github.com/microsoft/nni) \(open-source инструмент от Microsoft\) для настройки гиперпараметров на платформе. Вы создадите новый проект , интегрируете его с NNI и, чтобы ускорить поиск, запустите несколько процессов по настройке.

{% embed url="https://vimeo.com/417956858" caption="Hyperparameter Tuning with NNI" %}

Убедитесь, что у Вас установлен [Neu.ro CLI](../getting-started.md#installing-cli).

### Создание проекта Neu.ro

Для создания проекта, выполните команду:

```bash
neuro project init
cd <project-slug>
```

{% hint style="info" %}
Убедитесь, что Вы еще не выполняли команду `make setup`. Мы собираемся изменить несколько файлов перед созданием образа для эксперимента.
{% endhint %}

### Подготовка кода для эксперимента и интеграция с Neu.ro

Мы собираемся использовать пример кода [NNI example code](https://github.com/microsoft/nni/tree/master/examples/trials/mnist-tfv2) с набором данных MNIST. Поместите файл [mnist.py](https://github.com/microsoft/nni/blob/master/examples/trials/mnist-tfv2/mnist.py) в папку `modules`, а файл [search\_space.json](https://github.com/microsoft/nni/blob/master/examples/trials/mnist-tfv2/search_space.json) в папку `config`.

Затем добавьте следующие записи в файл `requirements.txt`:

```bash
# Required for Hyper-parameter search
nni==1.5
# Required by Neu.ro NNI integration scripts
Jinja2>=2.11.2
aiodns>=2.0.0
```

Теперь мы готовы собрать наш образ:

```bash
make setup
```

Пока Docker создает наш образ, мы можем продолжить настройку интеграции NNI:

Поместите [nni.mk](https://github.com/neuromation/ml-recipe-nni/blob/master/nni.mk) в корень Вашего проекта и добавьте следующую строку _в конец_ `Makefile`:

```bash
include nni.mk
```

Это добавит несколько новых целей make и перезапишет существующие для работы с NNI.

Наконец, поместите [`nni-config-template.yml`](https://github.com/neuromation/ml-recipe-nni/blob/master/config/nni-config-template.yml) и [`prepare-nni-config.py`](https://github.com/neuromation/ml-recipe-nni/blob/master/config/prepare-nni-config.py) в папку `config`.

После завершения работы команды `make setup` проект готов к запуску на Neu.ro.

### Запуск заданий по настройке

Осталось выполнить команду

```bash
make hypertrain
```

Данная команда делает:

* Запускает 3 ноды с предустановкой `gpu-small`. Оба параметра могут быть настроены через переменные `N_JOBS` и `PRESET` соответственно.
* Запускает мастер ноду с предустановкой `cpu-small`.
* Автоматически генерирует файл конфигурации NNI для мастер ноды, указывающий на процессы.
* Запускает процесс обучения и автоматически открывает веб-интерфейс NNI в Вашем браузере.

С помощью веб-интерфейса вы можете отслеживать ход эксперимента и промежуточные результаты. Когда процессы завершатся, Вы можете получить окончательные значения гиперпараметра и загрузить файлы логирования, если это необходимо.

![NNI Hyperparameter Tuning GUI](../.gitbook/assets/screen-shot-2020-05-12-at-12.43.02-pm.png)

Когда Вы закончите работу, Вы можете завершить оба процесса и мастер ноду с помощью команды:

```bash
make kill-hypertrain
```

