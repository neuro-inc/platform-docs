---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Hierarchical Attention for Sentiment Classification

[Run on Neu.ro](https://apps.neu.ro/ml-recipes/hier-attention)

[Explore on GitHub](https://github.com/neuromation/ml-recipe-hier-attention)

Our recipe is based on a frequently cited paper, [Hierarchical Attention Networks for Document Classification](https://arxiv.org/abs/1608.07775) \(Z. Yang et al.\), published in 2017. We will classify the IMDB's reviews as positive and negative \(25k reviews for training and the same number for testing\). The proposed neural network’s architecture makes two steps:

1. It encodes **sentences**. The attention mechanism predicts the importance of each **word** in the final embedding of a **sentence**.
2. It encodes **texts**. The attention mechanism predicts the importance of each **sentence** in the final embedding of a **text**.

This architecture is interesting because it allows us to create an illustration to better understand which words and sentences were important for prediction. More information can be found in the original article.

The architecture of the Hierarchical Attention Network \(HAN\):

![](../.gitbook/assets/scheme.png)

This recipe includes two scenarios:

* You can **train the model** yourself from scratch with the ability to make changes in data processing or architecture, it isn't tricky. 
* You can **play with a trained model** in the Jupyter notebook; write your review or pick a random one from the test set, then visualize the model’s predictions.

![](../.gitbook/assets/visualization.png)

### Technologies

* `Catalyst` as pipeline runner for deep learning tasks. This new and rapidly developing [library](https://github.com/catalyst-team/catalyst) can significantly reduce the amount of boilerplate code. If you are familiar with the TensorFlow ecosystem, you can think of Catalyst as Keras for PyTorch. This framework is integrated with logging systems such as the well-known [TensorBoard](https://www.tensorflow.org/tensorboard) and the new [Weights & biases](https://www.wandb.com/).
* `Pytorch` and `Torchtext` as main frameworks for deep learning. 
* `NLTK` for data preprocessing.

## Quick Start

#### 0. Sign up

#### 1. Install CLI and log in

```text
pip install -U neuromation
neuro login
```

#### 2. Run the recipe

```text
git clone git@github.com:neuromation/ml-recipe-hier-attention.git
cd ml-recipe-hier-attention
make setup
make jupyter
```

