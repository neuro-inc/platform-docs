---
description: >-
  This page is slightly outdated. Our technical writers are updating it right
  now.
---

# Deep Q-Learning \(DQN\)

[Run on Neu.ro](https://apps.neu.ro/ml-recipes/mountain-car)

[Explore on GitHub](https://github.com/neuromation/ml-recipe-mountain-car)

The first deep learning model to successfully learn control policies directly from high-dimensional sensory input using reinforcement learning was presented by [Mnih et. al.](https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf).

The model was a convolutional neural network, trained with a variant of Q-learning, whose input is raw pixels and whose output is a value function estimating future rewards.

![Car](https://user-images.githubusercontent.com/1387585/70574334-c090ab80-1b58-11ea-988d-f40afb6642a2.jpg)

We apply the approach discovered in that paper to one of the traditional [OpenAi gym](https://gym.openai.com/) environments - [Mountain Car](https://gym.openai.com/envs/MountainCar-v0/).

Classic DQN represents a rather simplistic approach, but at the same time, the recipe is an excellent starting point for diving into Deep Reinforcement Learning.

## Quick Start

#### 0. Sign up

#### 1. Install CLI and log in

```text
pip install -U neuro-cli neuro-extras neuro-flow
neuro login
```

#### 2. Run the recipe

```text
git clone git@github.com:neuromation/ml-recipe-mountain-car.git
cd ml-recipe-mountain-car
make setup
make jupyter
```

