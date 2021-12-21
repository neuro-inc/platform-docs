# FAQ

## Как загрузить и выгрузить данные

Вы можете загрузить свои наборы данных на платформу, используя Neuro CLI. Neuro CLI поддерживает базовые операции с файловой системой для копирования и перемещения файлов на дисковое пространство платформы и из нее.

Перейдите в терминале в каталог, содержащий набор данных и выполните команду:

```
$ neuro cp -r data/ storage:data/
```

URI `storage:data/` указывает, что местом назначения является платформа. Таким же образом,

```
$ neuro cp -r storage:data/ data/
```

загружает данные в ваш локальный каталог.

Вы можете получить доступ к своему набору данных из контейнера, указав `--volume storage:data/:/var/storage/data/:rw` в `neuro run`, когда запускаете задание.

## Как просматривать данные в Filebrowser

[Filebrowser](https://github.com/filebrowser/filebrowser) - это веб-интерфейс управления файлами. Вы можете использовать его для просмотра и управления вашим данными на платформе. Чтобы запустить задание и открыть инструмент в браузере, выполните следующую команду:

```
$ neuro run \
    --name filebrowser \
    --preset cpu-small \
    --http 80 \
    --detach \
    --browse \
    --volume storage::/srv:rw \
    filebrowser/filebrowser
```

FileBrowser требует аутентификации; учетные данные по умолчанию - `admin`/`admin`. Для более сложной настройки пользователя, пожалуйста, обратитесь к [документации Filebrowser](https://filebrowser.xyz).

## Как подключиться к работающему заданию

Чтобы работать с вашим набором данных из контейнера, для устранения неполадок модели или для получения доступа shell к экземпляру GPU, Вы можете выполнить команду внутри запущенного задания в интерактивном режиме.

Для этого необходимо узнать ID запущенного задания (можно сделать `neuro ps`, чтобы увидеть список) и выполнить команду:

```
$ neuro exec <job-id or job-name> bash
```

Например:

```
$ neuro exec training bash
```

Эта команда запускает bash внутри запущенного задания и подключает к нему ваш терминал.

## Как запустить задание в пользовательском рабочем окружении

Предполагается, что на вашем локальном компьютере создан образ Docker с именем `helloworld`. Вы можете отправить его на платформу, выполнив:

```
$ neuro push helloworld
```

После этого вы можете запустить задание:

```
$ neuro run image:helloworld
```

## Как завершить все работающие задания

Чтобы завершить все задания, которые в данный момент выполняются от вашего имени, выполните следующую команду:

```
 $ neuro kill `neuro -q ps -o <user>`
```

Например:

```
 $ neuro kill `neuro -q ps -o mariyadavydova`
```

## Как выполнить две или более команд в задании

Иногда необходимо выполнить две или три команды в задании, не подключаясь к нему. Например, чтобы изменить рабочий каталог и запустить обучение. Чтобы это сделать, Вам нужно обернуть Ваши команды в вызов `”bash -c ‘<commands>’”`, например:

```
"bash -c 'cd /project && python mnist/train.py'"
```

## Как получить результат с работающего задания

Есть два способа получить результат вашего текущего задания:

* Запустите его без опции `--detach`.
* Подключиться к выходу работающего задания с помощью `neuro log <JOB>`, где JOB это либо id, либо имя Вашего задания.

В некоторых случаях Python кеширует выходные данные, так что Вы не получите вывода, пока работа не будет завершена. Чтобы преодолеть эту проблему, используйте опцию `-u` для `python`, например:

```
"bash -c 'cd /project && python -u mnist/train.py'"
```

## Как получить доступ к заданию за Auth платформы

### Сгенерируйте сервисный аккаунт

Сгенерируйте сервисный аккаунт с помощью команды `neuro service-account create --name <имя-sa-аккаунта>`. Сохраните **Роль** и **Auth-токен** - вы будете использовать их для аутентификации запросов.

#### Пример:

```
    $ neuro service-account create --name test
     Id               service-account-b41aa732-4bb5-45e4-94c1-a078ca013255
     Name             test
     Role             janedoe/service-accounts/test
     Owner            janedoe
     Default cluster  default
     Created at       now
     
    Full token with cluster and API url embedded (this value can be used as NEURO_PASSED_CONFIG environment variable):
    eyJ0b2tlbi<hidden>SJ9
    
    Just auth token (this value can be passed to neuro config login-with-token):
    eyJhbGciOi<hidden>Np0
    
    Save it to some secure place, you will be unable to retrieve it later!
```

В этом случае, `janedoe/service-accounts/test` - нужная роль, а\
`eyJhbGciOi<hidden>Np0` - нужный auth-токен.

### Запустите ваше задание

Запустите задание, к которому хотите получить доступ.

#### Пример:

```
$ neuro run --http 8080 --name mytestjob python python -m http.server --cgi 8080
```

### Дайте сервисному аккаунту доступ к заданию

Дайте роли сервисного аккаунта из шага 1 доступ к заданию с помощью команды \
`neuro acl grant job:<id-или-имя-задания> <имя-роли> read`.

#### Пример:

```
$ neuro acl grant job:mytestjob janedoe/service-accounts/test read
```

### Используйте auth-токен

Используйте auth-токен из шага 1, чтобы аутентифицировать запрос с haeder'ом. Для этого запустите команду `"cookie: sat=<token-here>;"`.

#### Пример:

```
    $ curl https://mytestjob--janedoe.jobs.default.org.neu.ro -H "cookie: sat=eyJhbGciOi<hidden>Np0"
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Directory listing for /</title>
    </head>
    <body>
    <h1>Directory listing for /</h1>
    <hr>
    <ul>
    <li><a href=".dockerenv">.dockerenv</a></li>
    <li><a href="bin/">bin/</a></li>
    <li><a href="boot/">boot/</a></li>
    ....
```

## Как проверить использование дискового пространства

Для того, чтобы увидеть текущее использование дискового пространства, запустите такую команду:

```
$ neuro run -v storage://:/var/storage ubuntu du -h -d 1 /var/storage
```

Если вы хотите проверить использование дискового пространства в конкретной папке, пропишите её название в этой команде следующим образом:

```
$ neuro run -v storage:FOLDER_NAME:/var/storage ubuntu du -h -d 1 /var/storage
```
