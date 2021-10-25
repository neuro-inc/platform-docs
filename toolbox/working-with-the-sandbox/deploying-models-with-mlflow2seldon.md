# Выкладка моделей с помощью MLflow2Seldon

[MLFlow2Seldon Deployer](https://github.com/neuro-inc/mlops-k8s-mlflow2seldon) - интеграционный сервис для выкладки [зарегистрированных MLFlow-моделей](https://www.mlflow.org/docs/latest/model-registry.html) в виде REST/GRPC API на кластер Kubernetes с помощью [Seldon-core](https://www.seldon.io/tech/products/core/).

## Как работает MLFlow2Seldon

Этот сервис запускается внутри кластера Kubernetes с выложенным Seldon-core. Он синхронизирует состояние MLFlow с Seldon-core внутри кластера Kubernetes, постоянно получая зарегистрированные сервером MLFlow модели,  запущенные как задания на платформе с помощью MLFlow Python SDK.

Например, если версия зарегистрированной MLFlow модели присваивается статус staging/production, соответствующий бинарный файл модели выкладывается из MLFlow в кластер K8s как **SeldonDeployment** (с открытыми REST/GRPC APIs). Если статус модели меняется или удаляется, её SeldonDeployment изменяется соответствующе.

Таким образом, всё взаимодействие с сервисом происходит в неявном виде через состояние MLFlow-спервера. There is no need to Запускать конкретные команды/нагрузки напрямую не требуется.

## Предварительные требования

Перед тем, как запускать MLflow2Seldon, вам потребуется настроить MLflow и Seldon следующим образом:

### MLflow

* [ ] Запущен как [задание на платформе](https://github.com/neuro-actions/mlflow)
* [ ] Версия сервера не меньше 1.11.0
* [ ] SSO платформы отключена
* [ ] Место зхранения артефактов примотировано как локальный путь на дисковом пространстве платформы

### Seldon

* [ ] Образ контейнера SeldonDeployment ([model wrapper](https://docs.seldon.io/projects/seldon-core/en/stable/python/python\_wrapping\_docker.html)) хранится на реестре платформы на тои же кластере, что и MLFlow
* [ ] Версия** seldon-core-operator** не меньше 1.5.0
* [ ] Инструмент `kubectl` аутентифицирован для коммуникации с кластером Kubernetes, на котором выложен Seldon

## Выкладка

### С помощью CLI-команды

Чтобы выложить модель через MLflow2Seldon с помощью CLI, запустите следующую команду:

```
$ make helm_deploy
```

Эта команда спросит у вас некоторую дополнительную информацию - например, MLFlow URL, какой кластер нужно использовать, и т.д.&#x20;

### С помощью переменных среды

Укажите следующие переменные среды, чтобы использовать MLflow2Seldon:

* `M2S_MLFLOW_HOST` - hostname сервера MLFlow (например: [_https://mlflow--user.jobs.cluster.org.neu.ro_](https://mlflow--user.jobs.cluster.org.neu.ro)).
* `M2S_MLFLOW_STORAGE_ROOT` - корневой путь к артефактам на дисковом пространстве платформы (например: _storage:myproject/mlruns_).
* `M2S_SELDON_NEURO_DEF_IMAGE` - docker-образ, хранящийся в реестре платформы, который будет использоваться для выкладки модели (например: _image:myproject/seldon:v1_). Вы также можете настроить сервис на использование другого образа с платформы для для выкладки, отмечая соответствующую зарегистрированную _модель_ (но не _версию модели_) тегом, названным как значение параметра чарта `M2S_MLFLOW_DEPLOY_IMG_TAG`. Например, тег "_deployment-image_" и значение "_image:myproject/seldon:v2"_.
* `M2S_SRC_NEURO_CLUSTER` - Кластер, на котором размещены образ, артефакты MLflow и сам MLFlow (например: _demo\_cluster_).

### С помощью helm-чарта

Можно использовать helm-чарт напрямую, но это может быть менее удобно - вся информация, требуемая для makefile, должна быть передана в значениях чарта.

## Очистка

Если вы закончили работать с MLflow2Seldon и хотите очистить пространство, которое он использовал, запустите следующую команду:

```
$ make helm_delete
```

&#x20;Эта команда удалит:

* Все ресурсы, требуемые для сервиса и созданные helm-чартом, и сам сервис
* Namespace Kubernetes, в котором были созданы SeldonDeployments (M2S\_SELDON\_DEPLOYMENT\_NS)
