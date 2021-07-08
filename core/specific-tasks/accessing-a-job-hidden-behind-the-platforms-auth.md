# Как получить доступ к заданию за Auth платформы

Здесь описаны основные шаги по запуску задания с последующим доступом к нему через Auth платформы.

### Генерация сервисного аккаунта

Сгенерируйте сервисный аккаунт с помощью команды `neuro service-account create --name <имя-sa-аккаунта>`. Сохраните **Роль** и **Auth-токен** - вы будете использовать их для аутентификации запросов.

#### Пример:

```text
    $ neuro service-account create --name test
     Id               service-account-b41aa732-4bb5-45e4-94c1-a078ca013255
     Name             test
     Role             janedoe/service-accounts/test
     Owner            janedoe
     Default cluster  neuro-compute
     Created at       now
     
    Full token with cluster and API url embedded (this value can be used as NEURO_PASSED_CONFIG environment variable):
    eyJ0b2tlbi<hidden>SJ9
    
    Just auth token (this value can be passed to neuro config login-with-token):
    eyJhbGciOi<hidden>Np0
    
    Save it to some secure place, you will be unable to retrieve it later!
```

В этом случае, `janedoe/service-accounts/test` - нужная роль, а  
`eyJhbGciOi<hidden>Np0` - нужный auth-токен.

### Запуск задания

Запустите задание, к которому хотите получить доступ.

#### Пример:

```text
$ neuro run --http 8080 --name mytestjob python python -m http.server --cgi 8080
```

### Предоставление доступа к заданию сервисному аккаунту 

Дайте роли сервисного аккаунта из шага 1 доступ к заданию с помощью команды   
`neuro acl grant job:<id-или-имя-задания> <имя-роли> read`.

#### Пример:

```text
$ neuro acl grant job:mytestjob janedoe/service-accounts/test read
```

### Использование auth-токена

Используйте auth-токен из шага 1, чтобы аутентифицировать запрос с haeder'ом. Для этого запустите команду `"cookie: sat=<token-here>;"`.

#### Пример:

```text
    $ curl https://mytestjob--janedoe.jobs.neuro-compute.org.neu.ro -H "cookie: sat=eyJhbGciOi<hidden>Np0"
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

