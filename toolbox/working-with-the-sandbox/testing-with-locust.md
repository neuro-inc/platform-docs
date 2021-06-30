# Тестирование моделей с помощью Locust

[Locust](https://locust.io/) - open-source инструмент для нагрузочного тестирования. Его можно использовать в комбинации с платформой, чтобы тестировать модели и проверять стабильность их работы перед выкладкой в production-среду.

## Использование Locust с платформой

### Предварительные шаги

Перед тем, как тестировать модель в Locust, вам потребуется сделать тестовую выкладку с помощью MLflow. Для этого используется следующая команда:

```text
$ neuro-flow run deploy_inference_platform --param run_id <run-id>
```

Получить требуемый **run ID** можно в MLflow:

![](../../.gitbook/assets/image%20%28224%29.png)

Запуская задание `deploy_inference_platform`, вы получаете доступ к бинарному файлу модели и выкладываете его на кластере платформы.

После запуска это задание сгенерирует предсказуемый URI, который позднее можно использовать для доступа к модели в Locust.

### Запуск Locust

Вы можете указать, как платформа будет взаимодействовать с Locust в файле `.neuro/live.yml`, находящемся в корневом каталоге проекта.

В секции `jobs` файла `live.yml` вы найдёте стандартное описание задания `locust`, выглядящее следующим образом:

```text
locust:
    image: locustio/locust:1.4.1
    name: $[[ flow.title ]]-locust
    http_port: 8080
    http_auth: False
    life_span: 1d
    detach: True
    browse: True
    params:
      endpoint_url: 
        default: ~
        descr: |
          Examples:
          https://demo-oss-dogs-test-inference--<user-name>.jobs.<cluster-name>.org.neu.ro/api/v1.0/predictions - if model deployed as platform job
          http://seldon.<cluster-name>.org.neu.ro/seldon/seldon/<model-name>-<model-stage>/api/v1.0/predictions -if model is deployed in Seldon
    volumes:
      - $[[ upload(volumes.src).ref_ro ]]
      - $[[ upload(volumes.config).ref_ro ]]
      - $[[ volumes.remote_dataset.ref_ro ]]
    env:
      DOG_IDS: "n02085936, n02088094"
      IMGS_DIR: $[[ volumes.remote_dataset.mount ]]/images/Images/
      PYTHONPATH: $[[ volumes.src.mount ]]/..
    cmd: |
      -f $[[ volumes.src.mount ]]/locust.py --web-port 8080 -H $[[ params.endpoint_url ]]
```

Вы всегда можете исправить необходимые характеристики задания, изменяя значение параметров - например, `http_port` или `life_span` - в описании.

Используйте следующую команду для запуска описанного задания `locust`:

```text
$ neuro-flow run locust
```

Это задание использует файл`locust.py`, описанный в секции `cmd` определения задания. Этот файл должен описывать, какие тесты будет выполнять Locust . Вы можете [узнать больше о написании Locust-файлов здесь](https://docs.locust.io/en/stable/writing-a-locustfile.html).

Как только задание Locust будет запущено, вы можете открыть Locust в браузере, используя URI, сгенерированный в предыдущих шагах. 

Далее, введите URL-адрес задания Locust на платформе в поле **Host** и нажмите **Start swarming**:

![](../../.gitbook/assets/image%20%28223%29.png)

После этого вы сможете изменять количество симулируемых пользователей и частоту их создания, а также наблюдать за процессом тестирования с помощью многочисленных графиков, обновляемых в реальном времени:

![](../../.gitbook/assets/image%20%28225%29.png)

