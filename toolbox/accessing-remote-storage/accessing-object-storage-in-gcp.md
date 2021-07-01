# Доступ к Object Storage в GCP

## Введение

В данном руководстве показано, как получить доступ к облачному хранилищу Google Cloud Storage с платформы. Мы создадим новый проект на GCP, учетную запись и bucket, а затем сделаем этот bucket доступным для задания на платформе.

Убедитесь, что у Вас установлен [Neu.ro CLI](accessing-object-storage-in-gcp.md).

### Создание проектов на платформе и GCP

Для создания проекта выполните команду:

```bash
neuro project init
cd <project-slug>
neuro-flow build myimage
```

Хорошей практикой является ограничение доступа к конкретному проекту GCP. Чтобы создать новый проект GCP выполните команду:

```bash
PROJECT_ID=${PWD##*/}  # name of the current directory
gcloud projects create $PROJECT_ID
gcloud config set project $PROJECT_ID
```

Убедитесь, что Вы настроили платежную учетную запись для Вашего GCP проекта [set billing account](https://cloud.google.com/billing/docs/how-to/modify-project). Для подробной информации см. [Creating and Managing Projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects).

### Создание учетной записи и загрузка ключа

Сначала создайте учетную запись для задания:

```bash
SA_NAME="neuro-job"
gcloud iam service-accounts create $SA_NAME  \
    --description "Neuro Platform Job Service Account" \
    --display-name "Neuro Platform Job"
```

Для подробной информации см. [Creating and managing service accounts](https://cloud.google.com/iam/docs/creating-managing-service-accounts#iam-service-accounts-create-gcloud).

Затем загрузите ключ аккаунта:

```bash
gcloud iam service-accounts keys create ~/$SA_NAME-key.json \
  --iam-account $SA_NAME@$PROJECT_ID.iam.gserviceaccount.com
```

Убедитесь, что вновь созданный ключ находится в `~/`.

Создайте секрет для этого файла:

```bash
neuro secret add gcp-key @~/$SA_NAME-key.json
```

Откройте `.neuro/live.yaml`, найдите в нём секцию `remote_debug` внутри `jobs` и добавьте следующие строки в её конце:

```bash
jobs:
  remote_debug:
     ...
     secret_files: '["secret:gcp-key:/var/secrets/gcp.json"]'
     additional_env_vars: '{"GOOGLE_APPLICATION_CREDENTIALS": "/var/secrets/gcp.json"}'
```

### Создание Bucket и предоставление доступа

Теперь создайте новый bucket. Помните: имена bucket глобально уникальны \(см. дополнительную информацию [bucket naming conventions](https://cloud.google.com/storage/docs/naming)\).

```bash
BUCKET_NAME="my-neuro-bucket-42"
gsutil mb gs://$BUCKET_NAME/
```

Предоставьте доступ к созданному bucket:

```bash
# Permissions for gsutil:
PERM="storage.objectAdmin"
gsutil iam ch serviceAccount:$SA_NAME@$PROJECT_ID.iam.gserviceaccount.com:roles/$PERM gs://$BUCKET_NAME

# Permissions for client APIs:
PERM="storage.legacyBucketOwner"
gsutil iam ch serviceAccount:$SA_NAME@$PROJECT_ID.iam.gserviceaccount.com:roles/$PERM gs://$BUCKET_NAME
```

### Тестирование

Создайте файл и загрузите его в Google Cloud Storage Bucket:

```bash
echo "Hello World" | gsutil cp - gs://$BUCKET_NAME/hello.txt
```

Измените настройку по умолчанию на `cpu-small` и `.neuro/live.yaml`, чтобы избежать использования GPU для этого теста:

```bash
defaults:
  preset: cpu-small
```

Запустите задание и подключитесь к нему через оболочку shell:

```bash
neuro-flow run remote_debug
```

Активируйте сервисный аккаунт в оболочке задания:

```bash
gcloud auth activate-service-account --key-file $GOOGLE_APPLICATION_CREDENTIALS
```

Для доступа к Вашему bucket попробуйте использовать `gsutil`:

```bash
gsutil cat gs://my-neuro-bucket-42/hello.txt
```

Обратите внимание, что при `develop`, `train` и работе с `jupyter` переменная окружения `GOOGLE_APPLICATION_CREDENTIALS` указывает на ваш файл ключа. Таким образом, вы можете использовать его для [аутентификации других библиотек](https://cloud.google.com/storage/docs/reference/libraries).

Например, вы можете получить доступ к своему bucket через Python API, предоставляемый пакетом `google-cloud-storage`:

```text
>>> from google.cloud import storage
>>> bucket = storage.Client().get_bucket("my-neuro-bucket-42")
>>> text = bucket.get_blob("hello.txt").download_as_string()
>>> print(text)
b'Hello World\n'
```

Чтобы закрыть сеанс удаленного терминала, нажмите `^D` или введите `exit`.

Пожалуйста, не забудьте прекратить работу задания, если оно Вам больше не нужно:

```bash
neuro-flow kill remote_debug
```

