# Experiment Tracking with TensorBoard

### Introduction

[TensorBoard](https://www.tensorflow.org/tensorboard) is a tool that allows you to measure and visualize your machine learning workflow. It's based on the [TensorFlow](https://www.tensorflow.org/) open-source platform for machine learning. TensorFlow lets you easily acquire data, train models, serve predictions, and refine experiment results. \
TensorBoard, in turn, lets you measure, visualize, and share your experiment results. It also provides functionality for creating dataflow graphs that describe how data moves through a graph or series of nodes. All of this is provided through [python](https://www.python.org/), Java, Go, and JavaScript.

Apolo includes TensorBoard that lets you train ML models. If you're a beginner, then you can also use TensorBoard via Jupyter Notebooks without installing any additional components. You can run TensorFlow training processes using either CLI or JupyterLab. This guide will take you through a sample ML training task using TensorFlow and viewing the experiment in TensorBoard.

In this example, we will create a training model, deploy the model, and review the results. You must note that the logs of the project are saved on the platform storage. This lets you run or stop TensorBoard whenever required. Whenever you're done with the experiment, you should terminate the job to limit the amount of consumed GPU hours. Our example is based on the [Displaying image data in TensorBoard](https://www.tensorflow.org/tensorboard/image\_summaries) guide.

### Creating Training Using CLI

This training lets you log tensors and arbitrary images and view them in TensorBoard. We will use a sample image from the public Fashion MNIST dataset, convert it into an image, and visualize it in TensorBoard.

To create the training:

* Create a new flow:&#x20;

```
> cookiecutter gh:neuro-inc/cookiecutter-neuro-project --checkout release
project_name [Neuro Project]: imagesummary
project_dir [imagesummary]:
project_id [imagesummary]:
code_directory [modules]:
preserve Neuro Flow template hints [yes]:
```

Once the project is initialized, we will build the code to run our model. The next steps will guide you through creating the `train.py` file that will include the code.

* In the `<project directory>/modules` directory (`image/modules` in our example), add the following lines to the `train.py` file:

```python
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

* Next, we will download and load our data from the [Fashion-MNIST](https://research.zalando.com/welcome/mission/research-projects/fashion-mnist/) dataset:

```python
# Download the data. The data is already divided into train and test.
# The labels are integers representing classes.
fashion_mnist = keras.datasets.fashion_mnist
(train_images, train_labels), (test_images, test_labels) = fashion_mnist.load_data()
# Names of the integer classes, i.e., 0 -> T-short/top, 1 -> Trouser, etc.
class_names = ['T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat',
               'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot']
```

After the dataset is loaded, we will have a list of [matplotlib plots](https://matplotlib.org/). These have to be converted to tensors before you can start visualizing them.&#x20;

* To convert [matplotlib plots](https://matplotlib.org/) to images, add the following code:

```python
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

Next, we will use the data that we have to build an example. We will create an image classifier to classify the [Fashion-MNIST](https://research.zalando.com/welcome/mission/research-projects/fashion-mnist/) dataset that we have converted into tensors.

* To build the classifier, add the following code:

```python
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

* We will have to track how the classifier is performing through a confusion matrix. To create a confusion matrix, we will use the [Scikit-learn](https://scikit-learn.org/stable/auto\_examples/model\_selection/plot\_confusion\_matrix.html) function and then plot using the matplotlib:

```python
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

* Now that we have created the classifier and its confusion matrix, we need to log the basic metrics and the confusion matrix at the end of every cycle. Note that we have selected `results` as the log directory. You can select other directories too, if required.

<pre class="language-python"><code class="lang-python">logdir = "results/image/" + datetime.now().strftime("%Y%m%d-%H%M%S")
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

<strong>    # Log the confusion matrix as an image summary.
</strong>    with file_writer_cm.as_default():
        tf.summary.image("Confusion Matrix", cm_image, step=epoch)
</code></pre>

* Finally, let's train the classifier:

```python
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

Now that you have created the training code, use the following commands to run the classifier:

1. `apolo-flow build train`. This creates the required framework for the experiment before the job is executed.
2. `apolo-flow run train`. You need to press CTRL+C to detach from the process. This starts the job required for training.
3. `apolo-flow run tensorboard`. This starts a TensorBoard instance that visualizes the experiment.

TensorBoard automatically updates every 30 seconds, or you can manually refresh the page to view the latest results. The `results` subfolder of a project is saved on the platform storage. This lets you run and stop TensonBoard as often as you want.&#x20;

The TensorBoard interface includes the following tabs:

* Scalars
* Images
* Graphs

#### Scalars&#x20;

The **Scalars** dashboard shows how the accuracy and loss change with each epoch. You can use it to track training speed, learning rate, and other metrics. You can move your mouse over the graph to view more details.

![](../../.gitbook/assets/scalar.gif)

You can download the scalar information as a CSV or JSON file. To download, select **Show data download links** and then select the required file format.

#### Images

The **Images** tab displays the confusion matrix for the current training. For our current training (in which we are classifying images into categories of clothing), the **Images** tab shows the confusion matrix for various clothing types.&#x20;

![](../../.gitbook/assets/images\_tab.gif)

#### Graphs

The **Graphs** tab visualizes the computation of your model, such as a neural network mode. The Graph visualization lets you easily see what's happening in your model and detect any issues. \


![](<../../.gitbook/assets/graph\_tab (1) (1).gif>)

You can double-click on a code unit to open its visualization.
