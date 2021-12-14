# Отслеживание экспериментов с TensorBoard

### Введение

[TensorBoard](https://www.tensorflow.org/tensorboard) - инструмент, позволяющий измерять и визуализировать Ваш процесс машинного обучения. Он основан на open-source платформе для машинного обучения  [TensorFlow](https://www.tensorflow.org). TensorFlow позволяет легко получать данные, обучать модели, давать прогнозы, и улучшать результаты экспериментов. \
TensorBoard, в свою очередь, позволяет измерять и визуализировать ваши эксперименты и делиться их результатами. Он также обладает функциональностью для создания графиков, описывающих поток данных. Эти возможности предоставляются с помощью [Python](https://www.python.org), Java, Go и JavaScript.

Платформа включает в себя TensorBoard, что позволяет Вам обучать ML-модели. Вы также можете использовать TensorBoard через Jupyter Notebooks без установки дополнительных компонентов. Процессы обучения TensorFlow можно запускать через CLI или JupyterLab. Далее мы разберём пример задачи по ML-обучению, использующей TensorFlow, и просмотр результатов эксперимента в TensorBoard.

В этом примере мы созданим модель для обучения, развернём её и проверим результаты. Заметьте, что логи проекта сохраняются на хранилище платформы. Это позволяет Вам запускать и останавливать TensorBoard в любой удобный момент. Как только Вы закончите работать с экспериментом, желательно остановить соответствующее задание для уменьшения количества часов работы GPU. Наш пример основан на [Displaying image data in TensorBoard](https://www.tensorflow.org/tensorboard/image\_summaries).

### Подготовка и запуск обучения в CLI

Это обучение позволяет логировать тензоры и произвольные изображения и просматривать их в TensorBoard. Мы используем данные из публичного набора данных Fashion MNIST, преобразим их в изображение, и визуализируем их в TensorBoard.

Чтобы создать обучение:

* Создайте новый проект, используя следующую команду:

```
(base) C:\Projects>neuro project init
project_name [Neuro Project]: imagesummary
project_slug [imagesummary]: image
code_directory [modules]:
```

Как только новый проект будет создан, можно начинать собирать код, который запустит нашу модель. Далее описаны шаги по созданию файла `train.py`, содержащего этот код.

* Добавьте следующие строки в файл `train.py`, находящийся в папке `<project directory>/modules` (в нашем случае это `image/modules`):

```
from datetime import datetime
import io
import itertools
from six.moves import range

import tensorflow as tf
from tensorflow import keras

import matplotlib.pyplot as plt
import numpy as np
import sklearn.metrics
```

* Далее, мы загрузим данные из набора данных [Fashion-MNIST](https://research.zalando.com/welcome/mission/research-projects/fashion-mnist/):

```
# Download the data. The data is already divided into train and test.
# The labels are integers representing classes.
fashion_mnist = keras.datasets.fashion_mnist
(train_images, train_labels), (test_images, test_labels) = fashion_mnist.load_data()
# Names of the integer classes, i.e., 0 -> T-short/top, 1 -> Trouser, etc.
class_names = ['T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat',
               'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot']
```

Когда данные загрузятся, мы получим список [графиков matplotlib](https://matplotlib.org). Перед визуализацией их требуется преобразовать в тензоры.&#x20;

* Для преобразования [графиков matplotlib](https://matplotlib.org) в изображения используем такой код:

```
def plot_to_image(figure):
"""Converts the matplotlib plot specified by 'figure' to a PNG image and
returns it. The supplied figure is closed and inaccessible after this call."""
# Save the plot to a PNG in memory.
buf = io.BytesIO()
plt.savefig(buf, format='png')
# Closing the figure prevents it from being displayed directly #inside the notebook.
plt.close(figure)
buf.seek(0)
# Convert PNG buffer to TF image
image = tf.image.decode_png(buf.getvalue(), channels=4)
# Add the batch dimension
image = tf.expand_dims(image, 0)
return image
```

Далее, мы используем имеющиеся данные для наглядного примера. Мы создадим классификатор изображений, который поможет классифицировать набор данных [Fashion-MNIST](https://research.zalando.com/welcome/mission/research-projects/fashion-mnist/), который мы ранее преобразовали в тензоры.

* Чтобы создать классификатор, используйте следующий код:

```
model = keras.models.Sequential([
    keras.layers.Flatten(input_shape=(28, 28)),
    keras.layers.Dense(32, activation='relu'),
    keras.layers.Dense(10, activation='softmax')
])

model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)
```

* Следить за работой классификатора мы будем, используя матрицу неточностей. Для создания матрицы неточностей нам понадобится функция [Scikit-learn](https://scikit-learn.org/stable/auto\_examples/model\_selection/plot\_confusion\_matrix.html), а для графика используем matplotlib:

```
def plot_confusion_matrix(cm, class_names):
    """
    Returns a matplotlib figure containing the plotted confusion matrix.

    Args:
      cm (array, shape = [n, n]): a confusion matrix of integer classes
      class_names (array, shape = [n]): String names of the integer classes
    """
    figure = plt.figure(figsize=(8, 8))
    plt.imshow(cm, interpolation='nearest', cmap=plt.cm.Blues)
    plt.title("Confusion matrix")
    plt.colorbar()
    tick_marks = np.arange(len(class_names))
    plt.xticks(tick_marks, class_names, rotation=45)
    plt.yticks(tick_marks, class_names)

    # Normalize the confusion matrix.
    cm = np.around(cm.astype('float') / cm.sum(axis=1)[:, np.newaxis], decimals=2)

    # Use white text if squares are dark; otherwise black.
    threshold = cm.max() / 2.
    for i, j in itertools.product(range(cm.shape[0]), range(cm.shape[1])):
        color = "white" if cm[i, j] > threshold else "black"
        plt.text(j, i, cm[i, j], horizontalalignment="center", color=color)

    plt.tight_layout()
    plt.ylabel('True label')
    plt.xlabel('Predicted label')
    return figure
```

* Классификатор и матрица неточностей созданы, и теперь нам нужно логировать базовые метрики и матрицу неточностей в конце каждого цикла. Заметьте, что мы выбрали папку `results` для логирования результатов. Вы можете выбрать другую папку для этих целей, если потребуется.

```
logdir = "results/image/" + datetime.now().strftime("%Y%m%d-%H%M%S")
# Define the basic TensorBoard callback.
tensorboard_callback = keras.callbacks.TensorBoard(log_dir=logdir)
file_writer_cm = tf.summary.create_file_writer(logdir + '/cm')

def log_confusion_matrix(epoch, logs):
# Use the model to predict the values from the validation dataset.
test_pred_raw = model.predict(test_images)
test_pred = np.argmax(test_pred_raw, axis=1)

# Calculate the confusion matrix.
cm = sklearn.metrics.confusion_matrix(test_labels, test_pred)
# Log the confusion matrix as an image summary.
figure = plot_confusion_matrix(cm, class_names=class_names)
cm_image = plot_to_image(figure)

# Log the confusion matrix as an image summary.
with file_writer_cm.as_default():
tf.summary.image("Confusion Matrix", cm_image, step=epoch)
```

* Теперь можно написать код для обучения классификатора:

```
# Define the per-epoch callback.
cm_callback = keras.callbacks.LambdaCallback(on_epoch_end=log_confusion_matrix)

# Train the classifier.
model.fit(
      train_images,
train_labels,
epochs=25,
verbose=0,  # Suppress chatty output
callbacks=[tensorboard_callback, cm_callback],
validation_data=(test_images, test_labels),
)
```

Так как код для обучения теперь написан, можно запускать классификатор, используя следующие команды:

1. `make setup`. Это создаст требуемую среду для эксперимента перед его запуском.
2. `make train`. Эта команда запустит задание, требуемое для обучения. Используйте CTRL+C, чтобы оставить задание выполняться в фоновом режиме.
3. `make tensorboard`. Это запустит экземпляр TensorBoard, визуализирующий эксперимент.

```
(base) C:\Projects\image>make tensorboard
neuro run  \
        --name tensorboard-image \
        --preset cpu-small \
        --tag "target:tensorboard" --tag "kind:project" --tag "project:image" --tag "project-id:neuro-project-d2c1fffe" \
        --http 6006 \
        --http-auth \
        --browse \
        --life-span=1d \
        --volume storage:image/results://project/results:ro \
        tensorflow/tensorflow:latest \
        tensorboard --host=0.0.0.0 --logdir=//project/results
Job ID: job-650959b2-3f85-41fc-b423-07d48cf460c2 Status: pending
Name: tensorboard-image
Http URL: https://tensorboard-image--clarytyllc.jobs.neuro-public.org.neu.ro
Shortcuts:
  neuro status tensorboard-image     # check job status
  neuro logs tensorboard-image       # monitor job stdout
  neuro top tensorboard-image        # display real-time job telemetry
  neuro exec tensorboard-image bash  # execute bash shell to the job
  neuro kill tensorboard-image       # kill job
Status: pending Creating
Status: pending Scheduling
Status: pending ContainerCreating
Status: running
Terminal is attached to the remote job, so you receive the job's output.
Use 'Ctrl-C' to detach (it will NOT terminate the job), or restart the
job with `--detach` option.
TensorBoard 2.2.1 at http://0.0.0.0:6006/ (Press CTRL+C to quit)
```

TensorBoard автоматически обновляется каждые 30 секунд. Вы также можете обновить страницу вручную для получения последних результатов. Подпапка `results` текущего проекта сохраняется на хранилище платформы, что позволяет Вам запускать и останавливать TensonBoard в любой момент.&#x20;

Интерфейс TensorBoard состоит из следующих вкладок:

* Scalars
* Images
* Graphs

#### Scalars&#x20;

Вкладка **Scalars** показывает, как точность и потери изменяются с каждой эпохой. Здесь можно следить за скоростью обучения, темпом обучаемости и другими метриками. Вы можете увидеть больше информации, наводя курсор на график.

![](<../../.gitbook/assets/scalar (1).gif>)

Вы можете загрузить скалярную информацию как файл CSV или JSON. Для этого активируйте опцию **Show data download links** и выберите нужный формат.

#### Images

Вкладка **Images** показывает матрицу неточностей для текущего обучения. В нашем случае (так как мы классифицируем изображения в разные категории одежды), вкладка **Images** показывает матрицу неточностей для разных типов одежды.&#x20;

![](<../../.gitbook/assets/images\_tab (1).gif>)

#### Graphs

Вкладка **Graphs** визуализирует вычисление Вашей модели - например, в режиме нейросети. Эта визуализация позволяет легко отслеживать, что происходит с вашей моделью и выявлять проблемы. \


![](<../../.gitbook/assets/graph\_tab (1).gif>)

Двойной клик по элементу кода открывает его визуализацию.
