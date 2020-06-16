# Доступ к Object Storage на AWS

### Введение

В данном руководстве показано, как получить доступ к AWS S3 из платформы Neuro. Вы создадите новый проект Neuro, создадите S3 bucket и сделаете его доступным из заданий платформы Neuro.

Убедитесь, что у Вас установлен [Neu.ro CLI](../references/cli-reference/).

### Создание проекта Neuro

Для создания проекта Neuro выполните команду:

```bash
neuro project init
cd <project-slug>
make setup
```

### Создание пользователя AWS IAM

Следуйте инструкциям [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html).

В Консоли AWS перейдите в раскрывающийся список "Services", "IAM" \(Identity and Access Management\). На левой панели выберите "Access management" -&gt; "Users", нажмите синюю кнопку "Add user", пройдите мастер и в результате у Вас будет создан новый пользователь:

![](../.gitbook/assets/1_add_user.png)

Убедитесь, что данный пользователь имеется в списке разрешений "AmazonS3FullAccess".

Затем, Вам нужно создать ключ доступа для вновь созданного пользователя. Для этого перейдите в описание пользователя, вкладка "Security credentials", нажмите кнопку "Create access key":

![](../.gitbook/assets/2_create_key.png)

Поместите эти учетные данные в локальный файл в каталоге вашего проекта `./config/aws-credentials.txt`, например:

```text
[default]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

Установите соответствующие разрешения для файла с доступом:

```bash
chmod 600 ./config/aws-credentials.txt
```

Настройте Neuro Platform для использования этого файла и убедитесь, что проект Neuro обнаружил этот файл:

```bash
export AWS_SECRET_FILE=aws-credentials.txt
make aws-check-auth
```

Вывод данной команды: \`AWS will be authenticated via user account credentials file: '/project-path/config/aws-credentials.txt".

### Создание Bucket предоставление доступа

Теперь создайте новый S3 bucket. Помните: имена bucket глобально уникальны.

```bash
BUCKET_NAME="my-neuro-bucket-42"
aws s3 mb s3://$BUCKET_NAME/
```

### Проверка

Создайте файл и загрузите его в S3 Bucket:

```bash
echo "Hello World" | aws s3 cp - s3://$BUCKET_NAME/hello.txt
```

Запустите задание и подключитесь к оболочке shell:

```bash
export PRESET=cpu-small  # to avoid consuming GPU for this test
make develop
make connect-develop
```

В оболочке shell Вашего задания попробуйте использовать s3 для доступа к Вашему bucket:

```bash
aws s3 cp s3://my-neuro-bucket-42/hello.txt -
```

Чтобы закрыть сеанс удаленного терминала, нажмите `^D` или введите `exit`.

Пожалуйста, не забудьте прекратить работу задания, если оно Вам больше не нужно:

```bash
make kill-develop
```

