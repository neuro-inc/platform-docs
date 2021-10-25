# Использование MLFlow

MLFlow - open-source платформа для управления жизненным циклом машинного обучения. Она фокусируется на таких основных аспектах:

* **Отслеживание экспериментов**
* **Проекты**
* **Модели**
* **Реестр моделей**

Вы можете использовать MLFlow вместе с нашей платформой для создания эффективных пайплайнов.

Мы создали [репозитроий с проектом](https://github.com/neuro-inc/mlops-demo-oss-dogs), который поможет вам быстро проверить, как это работает.&#x20;

## Подключение MLFlow к платформе

для начала вам потребуется создать папку в секции `volumes` вашего проекта -  эта папка послужит для хранения артефактов и данных SQLite. В нашем примере мы назовём её `mlruns`:

```
mlruns:
  remote: storage:${{ flow.flow_id }}/mlruns
  mount: /usr/local/share/mlruns
```

Далее, добавьте действие `mlflow` в секции `jobs`. Это позволит вам запустить MLFlow в своём проекте, автоматически активируя соответствующий Github action из нашего репозитория без потребности настраивать его вручную.

```
mlflow:
  action: gh:neuro-actions/mlflow@v1.17.0
  args:
    backend_store_uri: sqlite:///${{ volumes.mlruns.mount }}/mlflow.db
    default_artifact_root: ${{ volumes.mlruns.mount }}
    volumes: "${{ to_json( [volumes.mlruns.ref_rw] ) }}"
```

Теперь примонтируйте созданную папку (в нашем случае, `mlruns`) в секции `volumes` описания задания `train`:

```
volumes:
  - $[[ upload(volumes.data).ref_ro ]]
  - $[[ upload(volumes.code).ref_ro ]]
  - $[[ volumes.mlruns.ref_rw ]]
```

После этого передайте MLFLow URI в скрипт обучения, чтобы синхронизировать его с MLFlow, запущенным на нашей платформе. Для этого добавьте следующую строку в секцию `bash` описания задания`train`:

```
--mlflow_uri http://${{ inspect_job('mlflow').internal_hostname_named }}:5000 \
```

Чтобы распарсить аргумент `mlflow_uri`, используйте такой код:

```
parser.add_argument(
    '--mlflow_uri',
    type=str,
    default='http//localhost:5000',
    help='Mlflow URI.'
)
```

После этого вы сможете использовать функциональность MLFlow в скриптах вашего проекта.
