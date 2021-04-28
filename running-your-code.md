# Запуск вашего кода

Часто вы не начинаете проект с нуля. Вместо этого вы используете свой или сторонний старый код и разрабатываете свое решение на его основе. В этом руководстве показано, как взять существующую базу кода, преобразовать ее в проект и начать разработку на платформе.

## Предусловия

1. Удостоверьтесь, что вы установили Neuro CLI вошли в веб-интерфейс платформы.
2. Установите пакет `neuro-flow`:

```bash
$ pip install -U neuro-flow
```

## Настройка

В качестве примера мы будем использовать [репозиторий](https://github.com/songyouwei/ABSA-PyTorch) GitHub, который содержит PyTorch-реализации для моделей аспектно-ориентированного анализа эмоциональной окраски \(подробнее см. [Attentional Encoder Network for Targeted Sentiment Classification](https://paperswithcode.com/paper/attentional-encoder-network-for-targeted)\). 

Сначала клонируем репозиторий и перейдем в созданную папку.:

```bash
$ git clone git@github.com:songyouwei/ABSA-PyTorch.git
$ cd ABSA-PyTorch
```

Теперь добавим несколько файлов:

* `Dockerfile` содержит очень простую конфигурацию образа Docker. Этот файл нужен нам для создания собственного образа Docker, основанного на общедоступных образах `pytorch/pytorch` и содержащего требования данного репозитория \(которые перечислены в файле `requirements.txt`\):

{% code title="Dockerfile" %}
```bash
FROM pytorch/pytorch
COPY . /cfg
RUN pip install --progress-bar=off -U --no-cache-dir -r /cfg/requirements.txt
```
{% endcode %}

* `.neuro/live.yml` содержит минимальное количество конфигурационной информации, позволяющей запускать скрипты из данного репозитория прямо на платформе с помощью коротких команд:

{% code title=".neuro/live.yml" %}
```yaml
kind: live
title: Sentiment Analysis Training
id: absa

volumes:
  project:
    remote: storage:${{ flow.id }}
    mount: /project
    local: .

images:
  pytorch:
    ref: image:${{ flow.id }}:v1.0
    dockerfile: ${{ flow.workspace }}/Dockerfile
    context: ${{ flow.workspace }}

jobs:
  train:
    image: ${{ images.pytorch.ref }}
    preset: gpu-small
    volumes:
      - ${{ volumes.project.ref_rw }}
    bash: |
        cd ${{ volumes.project.mount }}
        python train.py --model_name bert_spc --dataset restaurant
```
{% endcode %}

Эту конфигурационную информацию можно кратко разъяснить так:

*  секция `volumes` содержит объявления связей между файловой системой вашего компьютера и хранилищем платформы; здесь мы заявляем, что хотим, чтобы вся папка проекта была загружена в папку `storage:absa` внутри хранилища и примонтирована внутри заданий `/project`;
* секция `images` содержит объявления образов Docker, созданных внутри проекта; здесь мы объявляем наш образ, описанный в файле `Dockerfile`;
* в секции `jobs` происходит большая часть работы; здесь мы объявляем задание `train`, запускающее наш скрипт по обучению с несколькими параметрами.

## Запуск кода

Теперь можно запустить несколько команд, производящих настройку среды проекта и начинающих обучение.

* Сначала создайте тома и загрузите проект на хранилище платформы:

```text
$ neuro-flow mkvolumes
$ neuro-flow upload ALL
```

* Затем соберите образ:

```text
$ neuro-flow build pytorch
```

* Запустите обучение:

```text
$ neuro-flow run train
```

Вы можете запустить `neuro-flow --help`, чтобы получить сведения о доступных командах. 

