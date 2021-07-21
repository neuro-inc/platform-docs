# Использование MLflow

[MLflow](https://mlflow.org) - open-source инструмент, позволяющий управлять циклами машинного обучения от начала до конца. При использовании с платформой, MLflow может помочь с отслеживанием экспериментов и выкладкой моделей.

## Запуск MLflow на платформе

Для того, чтобы упростить процесс запуска MLflow на платформе, мы создали [действие MLflow](https://github.com/neuro-actions/mlflow) для [neuro-flow](https://neu-ro.gitbook.io/neuro-flow/), запускающее экземпляр [отслеживающего сервера MLFlow](https://www.mlflow.org/docs/latest/tracking.html).

Есть два обязательных параметра, которые нужно указать для корректной работы действия MLflow:

* **`backend_store_uri`**

Этот параметр указывает URI пространства, которое будет использовано для хранения экспериментов, их метрик, зарегистрированных моделей и т.д. Вы можете указать сервер Postgres, экземпляр SQLite, файл и др. Больше информации об этом параметре [здесь](https://mlflow.org/docs/latest/tracking.html#backend-stores).

* **`default_artifact_root`**

Этот параметр указывает место хранения артефактов MLFlow. Этот путь также должен быть доступен из основного задания по обучению. Больше информации об этом параметре [здесь](https://mlflow.org/docs/latest/tracking.html#artifact-stores).

Пример действия MLflow в его самом простом виде:

```text
jobs:
  mlflow_server:
    action: gh:neuro-actions/mlflow@v1.0.0
    args:
      backend_store_uri: sqlite:///${{ volumes.mlflow_storage.mount }}/mlflow.db
      default_artifact_root: ${{ volumes.mlflow_artifacts.mount }}
```

Вы можете прочитать о всех поддерживаемых параметрах действия [здесь](https://github.com/neuro-actions/mlflow).





