# Доступ к Object Storage на AWS

## Введение

В данном руководстве показано, как получить доступ к AWS S3 из платформы. Вы создадите новый проект, создадите S3 bucket и сделаете его доступным из заданий платформы.

Убедитесь, что у Вас установлен [Neu.ro CLI](accessing-object-storage-in-aws.md).

## Создание проекта

Для создания проекта выполните команду:

```bash
neuro project init
cd <project-slug>
neuro-flow build myimage
```

## Создание пользователя AWS IAM

Следуйте инструкциям [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html).

В Консоли AWS перейдите в раскрывающийся список "Services", "IAM" \(Identity and Access Management\). На левой панели выберите "Access management" -&gt; "Users", нажмите синюю кнопку "Add user" и пройдите мастер создания пользователя. В результате у Вас будет создан новый пользователь:

![](../../.gitbook/assets/1_add_user.png)

Убедитесь, что данный пользователь имеется в списке разрешений "AmazonS3FullAccess".

Затем, Вам нужно создать ключ доступа для созданного пользователя. Для этого перейдите в описание пользователя, вкладка "Security credentials", нажмите кнопку "Create access key":

![](../../.gitbook/assets/2_create_key.png)

Поместите эти учетные данные в локальный файл в каталоге вашего проекта `~/aws-credentials.txt`. Например:

```text
[default]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

Установите соответствующие разрешения для файла с секретом:

```bash
chmod 600 ~/aws-credentials.txt
```

Настройте платформу для использования этого файла и убедитесь, что проект на платформе обнаружил этот файл:

```bash
neuro secret add aws-key @~/aws-credentials.txt
```

Откройте `.neuro/live.yaml`, найдите в нём секцию `remote_debug` внутри `jobs` и добавьте следующие строки в её конце:

```bash
jobs:
  remote_debug:
     ...
     secret_files: '["secret:aws-key:/var/secrets/aws.txt"]'
     additional_env_vars: '{"AWS_CONFIG_FILE": "/var/secrets/aws.txt"}'
```

## Создание bucket и предоставление доступа

Теперь создайте новый S3 bucket. Помните: имена bucket глобально уникальны.

```bash
BUCKET_NAME="my-neuro-bucket-42"
aws s3 mb s3://$BUCKET_NAME/
```

## Тестирование

Создайте файл и загрузите его в S3 Bucket:

```bash
echo "Hello World" | aws s3 cp - s3://$BUCKET_NAME/hello.txt
```

Измените настройку по умолчанию на `cpu-small` в `.neuro/live.yaml`, чтобы избежать использования GPU для этого теста:

```bash
defaults:
  preset: cpu-small
```

Запустите задание и подключитесь к оболочке shell:

```bash
neuro-flow run remote_debug
```

В оболочке shell Вашего задания попробуйте использовать `s3` для доступа к Вашему bucket:

```bash
aws s3 cp s3://my-neuro-bucket-42/hello.txt -
```

Чтобы закрыть сеанс удаленного терминала, нажмите `^D` или введите `exit`.

Пожалуйста, не забудьте прекратить работу задания, если оно Вам больше не нужно:

```bash
make kill-develop
```

