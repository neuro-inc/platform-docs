# Обучение и обработка с помощью Label Studio и Pachyderm

Вы можете создать эффективную цепочку процессов обработки изображений и обучения модели, используя платформу вместе с [Label Studio](https://labelstud.io/) и [Pachyderm](https://www.pachyderm.com/) в sandbox-пространстве.

Для этого вам нужно будет создать пайплайн Pachyderm, активирующий процесс обучения или переобучения модели при каждом обновлении датасета, задевающем маркировку изображений. Таким образом, каждый раз, когда вы будете обрабатывать изображения в Label Studio, ваша модель будет автоматечески переобучена.

Когда ваше sandbox-среда будет настроено и у вас будет запущенный кластер Pachyderm, вам нужно будет создать пайплайн Pachyderm:

```text
$ neuro-flow run create_pipeline --param mlflow_storage $MLFLOW_STORAGE --param mlflow_uri $MLFLOW_URI
```

Далее, вам нужно будет загрузить датасет на дисковое пространство платформы, выполняя следующую команду:

```text
$ neuro-flow run prepare_remote_dataset
```

Выберите некоторое количество изображений из датасета и добавьте их в Pachyderm:

```text
$ neuro-flow run extend_data --param extend_dataset_by <количество_изображений>
```

Теперь вы можете проверить работу цепочки, запуская Label Studio в браузере:

```text
$ neuro-flow run label_studio
```

Как только изображения будут обработаны, Label Studio автоматически закроется и создаст новую версию датасета.

Это, в свою очередь, активирует пайплайн Pachyderm и начнёт обучение модели. Вы можете проследить за процессом в pipeline-логах Pachyderm:

```text
pachctl config update context default --pachd-address <Pachyderm server address>
pachctl logs -f -p train
```

