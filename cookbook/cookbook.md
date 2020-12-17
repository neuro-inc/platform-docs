---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Cookbook

What you will find here is a curated collection of ML Recipes: ready-to-use examples, best practices, and learning materials available to you in the form of interactive environments that run on Neuro Platform.

Using ML Recipes, you can:

* Run state of the art \(SOTA\) deep learning models for a variety of applied tasks.
* Interact with the results and extend the solution.
* Upload data and download the results using a desktop or mobile device.
* Make your experiments reproducible.

You have two options of how you might wish to use our recipes. You can sign up as [free tier user](https://neu.ro/) and experiment with the recipes within our managed installation of the platform. You can also install Neuro into your AWS, GCP, or Azure cloud account or on-perm GPU rack and use the recipes there.

## What's Inside?

### Vision

[Object detection](object-detection.md). An end to end object detection pipeline for objects represented in the [Common Objects in Context \(COCO\)](http://cocodataset.org) dataset that allows you to evaluate performance on real life images and to add new object classes.

[Pediatric Bone Age Assessment](pediatric-bone-age-assessment.md). This recipe demonstrates the estimation of the age of the child from the X-ray image of the left hand from the wrist to the fingertips. It is based on an approach described in _"Paediatric Bone Age Assessment Using Deep Convolutional Neural Networks" by V. Iglovikov, A. Rakhlin, A. Kalinin and A. Shvets_, [1](https://link.springer.com/chapter/10.1007%2F978-3-030-00889-5_34), [2](https://www.biorxiv.org/content/biorxiv/early/2018/06/20/234120.full.pdf).

### Language

[Hierarchical Attention](hierarchical-attention-for-sentiment-classification.md). Based on highly cited paper [Hierarchical Attention Networks for Document Classification](https://arxiv.org/abs/1608.07775) \(Z. Yang et al.\), published in 2017, this recipe demonstrates how to apply this two-step architecture to sentiment classification.

[MIDI Generator](midi-generator.md). This recipe demonstrates how to generate MIDI files from scratch and continue existing MIDI files. Warning: the recipe contains a very catchy notebook. Don't forget to watch your surroundings!

### Reinforcement Learning

[Deep Q-Learning](deep-q-learning-dqn.md). This recipe shows a rather simplistic approach to reinforcement learning \(DQN\) in one of the traditional environment: [Mountain Car](https://gym.openai.com/envs/MountainCar-v0/). It is an excellent starting point to dive into Deep Reinforcement Learning.

## Prerequisites

* Familiarity with working with data in Python.
* Familiarity with machine learning concepts \(such as training and test sets\).
* Some experience with PyTorch and neural networks is helpful.

## Getting Started

### Install the Neuro CLI

To work with ML Recipes, you need to install the Neuro Platform CLI.

```text
pip install -U neuromation
neuro login
```

Paste the above in a macOS, Windows, or Linux terminal prompt. This command automatically installs the Neuro CLI and brings you to the login screen of Neuro Platform.

The Neuro Platform CLI requires Python 3.7 to be installed. We suggest installing Anaconda Python 3.7 Distribution. On some distributions, you might have to run `pip3 install -U neuromation`.

### Run ML Recipe

Choose an ML Recipe from the list above, clone the corresponding repository and follow the instructions from that recipe's README.

