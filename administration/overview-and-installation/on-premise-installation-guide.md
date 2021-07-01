# Инструкция по локальной установке

## Требования:

* Кластер Kubernetes:
  * Должен иметь возможность использовать **OpenEBS Cstor**. Диски должны быть подсоединены к узлам Kubernetes, непримонтированы и неформатированы.
  * Если будет отсутствовать доступ к интернету, образ **busybox:latest** должен быть извлечён в каждый узел.
* Linux VM:
  * Должна быть доступна кластеру Kubernetes \(на эту VM будет установлен реестр docker и сервисы **chartmuseum** и **devpi**, которые нужны для работы платформы\).
  * Должна быть доступна кластеру Kubernetes.
  * Должны быть установлены следующие утилиты: **docker, kubectl, jq.**

## Структура .tar-архива

**/chartmuseum**

* Папка со всеми необходимыми helm-чартами. Она будет примонтирована как том к контейнеру **chartmuseum**.

**/registry**

* Папка со всеми необходимыми docker-образами. Она будет примонтирована как том к контейнеру **registry**.

**/devpi**

* Папка с python-пакетом **neuro-cli** и его зависимостями. Она будет примонтирована как том к контейнеру **devpi**.

**registry.tar**

* Сохранённый образ **registry:2**.

**chartmuseum.tar**

* Сохранённый образ **chartmuseum/chartmuseum:latest**.

**devpi.tar**

* Сохранённый образ **devpi**.

**jq.tar**

* Сохранённый образ **imega/jq:latest**, обработчик JSON для командной строки.

**yq.tar**

* Сохранённый образ **mikefarah/yq:latest**, обработчик YAML для командной строки.

**k8s/\*.yaml**

* Ресурсы Kubernetes, которые будут созданы на кластере.

**\*.sh**

* Установочные скрипты.

## Установка платформы

Подключитесь к Linux VM и удостоверьтесь, что **kubectl** может подсоединиться к кластеру Kubernetes:

```text
kubectl get nodes
```

Примонтируйте USB или другое устройство внешнего хранения и распакуйте архив **neuro.tar:**

```text
mkdir –p $HOME/neuro
tar -xvf neuro.tar -C $HOME/neuro
```

Подготовьте конфигурационный файл \(см. [пример ниже](on-premise-installation-guide.md#config-file-example)\), запустите установочный скрипт и дождитесь момента, когда все поды будут в статусе Running:

```text
$HOME/neuro/install.sh $CONFIG_FILE_PATH
```

По умолчанию, если в конфигурационном файле не указан сертификат Ingress, установочный скрипт сгенерирует самозаверенный сертификат. Этот самозаверенный сертификат должен быть добавлен в trust store сертификатов в среде разработки пользователя платформы.

### Настройка DNS-сервера

Настройте A-записи к доменам платформы **\*.neu.ro**, **default.org.neu.ro**, **\*.default.org.neu.ro**, **\*.jobs.default.org.neu.ro** таким образом, чтобы они указывали на все IPv4-адреса кластера Kubernetes.

## Пример конфигурационного файла

```text
 server:
  ip: "10.240.0.8"
ui:
  type: minzdrav
ingress_ssl:
  cert_path: "/path/to/ingress.crt" # optional
  cert_key_path: "/path/to/ingress.key" # optional
postgres:
  password: changeme
  size: 10Gi
redis:
  password: changeme
  size: 10Gi
keycloak:
  username: admin
  password: changeme
auth:
  jwt_secret: changeme
registry:
  size: 10Gi
storage:
  size: 10Gi
blob_storage:
  size: 10Gi
metrics:
  size: 10Gi
node_pools:
- name: cpu
  cpu: 8
  memory_gb: 6
  disk_size_gb: 6
  nodes:
  - aks-agentpool-36699122-vmss000002
- name: gpu
  cpu: 8
  memory_gb: 6
  disk_size_gb: 6
  gpu: 1
  gpu_model: nvidia-tesla-k80
  nodes:
  - aks-agentpool-36699122-vmss000002
```

## Настройка среды разработки

### Добавление сертификата в trust store \(если был сгенерирован самозаверенный сертификат\)

* Загрузите сертификат Ingress:

```text
openssl s_client -connect app.neu.ro:443 -showcerts </dev/null > ingress.crt
```

* Добавьте его в Ваш trust store.

### Установка Neuro CLI

Запустите следующую команду для установки Neuro CLI:

```text
pip install -i http://$SERVER_IP/root/pypi neuro-cli
```

