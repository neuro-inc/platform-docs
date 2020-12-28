---
description: >-
  Данная страница немного устарела. Наша команда технических писателей работает
  над ней.
---

# Рабочее окружение \(образы Docker\)

Neu.ro использует Docker-контейнеры для выполнения заданий в изолированной среде. Чтобы запустить контейнер, необходимо использовать образ Docker, представляющий собой шаблон, содержащий приложение и все зависимости для запуска данного приложения. Таким образом, Docker-контейнер это запущенный экземпляр образа Docker.

Neu.ro позволяет использовать образы Docker либо из публичного репозитория \(например, Docker Hub\) либо из репозитория кластера платформы \(называемого также приватным репозиторием\). Каждый кластер имеет один репозиторий, поэтому вы не можете использовать образы из одного кластера в другом кластере.

Есть несколько способов добавить пользовательские образы в репозиторий платформы. Эти образы могут быть открыты для общего пользования Вашим коллегами по команде.

![&#x423;&#x43F;&#x440;&#x430;&#x432;&#x43B;&#x435;&#x43D;&#x438;&#x435; &#x440;&#x430;&#x431;&#x43E;&#x447;&#x438;&#x43C; &#x43E;&#x43A;&#x440;&#x443;&#x436;&#x435;&#x43D;&#x438;&#x435;&#x43C;](../.gitbook/assets/environments-2.png)

В данном руководстве для лучшего понимания доступных функций мы будем использовать тестовый проект - Nero-Assistant. Перед тем, как приступить к работе в рабочем окружении, убедитесь, что Вы создали новый проект \(см. руководство по началу работы\).

## **Как выполнить задание в контейнере из образа публичного репозитория?**

Вы можете запустить задание как из образа публичного репозитория, так и из образа репозитория платформы. Чтобы запустить задание необходимо выполнить команду neuro run:

`neuro run [OPTIONS] IMAGE [CMD]`

**Параметры**

| Наименование | Описание |
| :--- | :--- |
| IMAGE | Полный URI образа, где вы хотите выполнить команду. |
| \[CMD\] | Команды, которые вы хотите передать в контейнер. |

**Образец вывода**

```text
> neuro run -n job369 -s cpu-small ubuntu
Job ID: job-1712b7de-0072-48f5-bcc1-f64d3256730e Status: pending
Name: job369
Http URL: https://job369--clarytyllc.jobs.neuro-public.org.neu.ro
Shortcuts:
  neuro status job369     # check job status
  neuro logs job369       # monitor job stdout
  neuro top job369        # display real-time job telemetry
  neuro exec job369 bash  # execute bash shell to the job
  neuro kill job369       # kill job
Status: pending Creating
Status: pending Scheduling
Status: succeeded
Terminal is attached to the remote job, so you receive the job's output.
Use 'Ctrl-C' to detach (it will NOT terminate the job), or restart the
job with `--detach` option.
```

Замечание. Имя задания должно содержать только строчные буквы, цифры или дефисы и должно начинаться с буквы.

Когда задание создано, оно добавляется в очередь и его статус устанавливается как Pending. Как только задание запускается, ему присваивается ID и статус меняется на Running. Все текущие задания приведены в веб-интерфейсе Neu.ro.

Для более подробной информации смотри [справка по CLI для запуска](../references/cli-reference/).

Чтобы посмотреть список заданий, выполняющихся в данный момент, необходимо выполнить команду: `neuro ps`. Будет показан список заданий и их ID. Можно также завершить выполнение задания, выполнив команду: `neuro kill <ID задания>`.

## **Как можно посмотреть список всех доступных для меня образов?**

Neu.ro позволяет создавать много образов и просматривать информацию о Ваших образах или к которым у Вас имеется доступ. Для просмотра списка всех образов необходимо выполнить команду:

`neuro image ls [OPTIONS]`

**Образец вывода**

```text
> neuro image ls
image:neuromation-neuro-tutorial
image:neuromation-nero-assistant
```

Чтобы просмотреть теги образов, необходимо выполнить команду:

`neuro image tags [OPTIONS] IMAGE`

**Образец вывода**

```text
> neuro image tags image://neuro-public/clarytyllc/neuromation-nero-assistant
image://neuro-public/clarytyllc/neuromation-nero-assistant:v1.5.1
```

Обратите внимание, что для просмотра тегов образов необходимо указать полный URI. Вы можете добавлять теги, когда загружаете образ или сохраняете его. Для получения дополнительной информации см. [Загрузка пользовательского образа в репозиторий платформы](environments-docker-images.md#how-can-i-upload-a-custom-image-to-the-platform-registry).

## **Как я могу загрузить пользовательский образ в репозиторий платформы?**

Neu.ro предоставляет базовый публичный образ Docker на основе deepo. Вы можете изменить базовый образ, установив новые пакеты или изменив настройки с помощью механизмов докера. Произвести настройку образа можно при помощи файла Dockerfiles.

Neu.ro позволяет загружать пользовательские образы на платформу, которые затем можно использовать для запуска заданий. Вы также можете предоставить доступ к Вашим образам своим коллегам. Рекомендуется добавлять теги к образу для лучшего отслеживания.

Чтобы загрузить пользовательский образ, выполните команду:

`neuro image push [OPTIONS] LOCAL_IMAGE [REMOTE_IMAGE]`

**Параметры**

| Наименование | Описание |
| :--- | :--- |
| LOCAL\_IMAGE | Пользовательский образ, который Вы хотите загрузить. Название образа не должно содержать “image://”. |
| REMOTE\_IMAGE | Удаленный образ, который Вы хотите загрузить. |

**Образец вывода**

```text
> neuro push neuromation-nero-assistant image:nero-assistant:v2
Pushing image neuromation-nero-assistant => image://neuro-public/mrsmariyadavydova/nero-assistant:v2
> b7f3b88ae387: Pushed
> e6a8e7191cdf: Pushing [=> ]               2.736MB/93.82MB
> 9dfa40a0da3b: Pushing [===============> ] 1.219MB/3.966MB
```

После загрузки образа, чтобы проверить, сработала ли успешно передача, выполните команду neuro image ls. Если загрузка прошла успешно, то Вы увидите Ваш образ, а также образ платформы.

```text
> neuro images
image:neuro-public/mrsmariyadavydova/nero-assistant:latest
image:neuromation-neuro-tutorial
image:neuromation-nero-assistant
```

## **Как я могу сохранить текущее задание как пользовательский образ?**

Существует несколько способов создания пользовательских образов. Вы можете создать пользовательский образ из работающего задания. Прежде чем сохранить задание, необходимо узнать id или имя задания, которое Вы хотите сохранить.

Для сохранения задания в качестве образа надо использовать команду neuro job save:

`neuro job save [JOB] [IMAGE]`

**Параметры**

| Наименование | Описание |
| :--- | :--- |
| JOB | ID или имя задания, которое Вы хотите сохранить как пользовательский образ. |
| IMAGE | Команды, которые вы хотите передать в контейнер. |

**Образец вывода**

```text
> neuro job save job363 image:ubuntu-custom
Saving job-16339fe4-9559-4c4c-9437-e6e7d5d0721e -> image://neuro-public/clarytyllc/ubuntu-custom:latest
Creating image image://neuro-public/clarytyllc/ubuntu-custom:latest image from the job container
Image created
Pushing image clarytyllc/ubuntu-custom:latest => image://neuro-public/clarytyllc/ubuntu-custom:latest
8891751e0a17: Pushed
2a19bd70fcd4: Pushed
9e53fd489559: Pushed
7789f1a3d4e9: Pushed
image://neuro-public/clarytyllc/ubuntu-custom:latest
```

Мы сохранили задание **job363** как пользовательский образ **ubuntu-custom**.

## **Как мне использовать образ из репозитория платформы?**

Neu.ro позволяет работать с заданиями, рабочим окружением и дисковым пространством. Задания выполняются в рабочем окружении \(или контейнерах\), которые изолированы вместе с собственным дисковым пространством, CPU и памятью. Вы можете запускать задания как в публичном образе Docker, так и в образе из репозитория платформы. Чтобы лучше управлять ресурсами, Вы можете указать CPU и объем памяти, которые будут использоваться заданием.

Для запуска задания используется команда neuro run:

`neuro run [OPTIONS] IMAGE [CMD]`

**Параметры**

| Наименование | Описание |
| :--- | :--- |
| IMAGE | Полный URI образа, где вы хотите выполнить команду. |
| \[CMD\] | Команды, которые вы хотите передать в контейнер. |

**Образец вывода**

```text
> neuro run -n job363 -s cpu-small image:ubuntu-patched:v2 echo Hello World
Job ID: job-21beb932-1cdb-4b55-b286-10a99752a9f1 Status: pending
Name: job363
Http URL: https://job363--clarytyllc.jobs.neuro-public.org.neu.ro
Shortcuts:
  neuro status job363     # check job status
  neuro logs job363       # monitor job stdout
  neuro top job363        # display real-time job telemetry
  neuro exec job363 bash  # execute bash shell to the job
  neuro kill job363       # kill job
Status: pending Creating
Status: pending Scheduling
Status: succeeded
Terminal is attached to the remote job, so you receive the job's output.
Use 'Ctrl-C' to detach (it will NOT terminate the job), or restart the
job with `--detach` option.                
Hello World
```

Мы запустили задание с именем **job363** , используя ресурсы cpu-small на пропатченом образе ubuntu.

## **Как я могу загрузить образ из репозитория платформы?**

Neu.ro позволяет вам загрузить образ из репозитория платформы. Вы можете указать тег образа, чтобы загрузить определенный тег, в противном случае будет загружен образ с последним тегом.

Для загрузки образа с платформы выполните команду:

`neuro image pull [OPTIONS] REMOTE_IMAGE [LOCAL_IMAGE]`

**Parameters**

| Наименование | Описание |
| :--- | :--- |
| REMOTE\_IMAGE | Образ, который Вы хотите загрузить. |
| \[LOCAL\_IMAGE\] | Локальный пользовательский образ, который Вы хотите загрузить. Название образа не должно содержать “image://”. |

**Образец вывода**

```text
> neuro pull image:ubuntu-patched:v1
> Pulling image image://neuro-public/mrsmariyadavydova/ubuntu-patched:v1 => ubuntu-patched:v1
> d51af753c3d3: Downloading [========>                   ]  4.972MB/28.56MB
> fc878cd0a91c: Download complete
> 6154df8ff988: Download complete
> fee5db0ff82f: Download complete
> 8d6a6e6d0908: Download complete
```

## Как я могу предоставить доступ к моему пользовательскому образу другим членам команды?

Чтобы поделиться образом с Вашей командой нет необходимости загружать образ на платформу. Если Вы хотите поделиться Вашим образом с большой аудиторией, Вы можете загрузить образ в какой-либо публичный репозиторий.

Для предоставления доступа к Вашему образу выполните команду:

`neuro share [OPTIONS] URI USER [read|write|manage]`

Разрешения для образа, которые вы можете предоставить пользователю USER:

* Read: пользователь может только просматривать образ и не может вносить никаких изменений.
* Write: пользователь может вносить изменения в образ.
* Manage: пользователь может предоставить доступ к образу.

**Пример команды**

`neuro share image://neuro-public/clarytyllc/neuromation-nero-assistant mrsmariyadavydova manage`

