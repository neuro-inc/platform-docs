# Глубокое Q-обучение \(DQN\)

[Запуск на Neu.ro](https://apps.neu.ro/ml-recipes/mountain-car)

[Репозиторий на GitHub](https://github.com/neuromation/ml-recipe-mountain-car)

Первая модель глубокого обучения, позволяющая успешно применять политику контроля непосредственно из многомерного чуствительного ввода с использованием обучения с подкреплением была представлена [Mnih et. al.](https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf).

Модель представляла собой сверточную нейронную сеть, обученную по варианту Q-обучения, чей входной сигнал представляет собой необработанные пиксели, а выходной - функция вознаграждения, оценивающая будущие вознаграждения.

![Car](https://user-images.githubusercontent.com/1387585/70574334-c090ab80-1b58-11ea-988d-f40afb6642a2.jpg)

Мы применяем подход, описанный в этой статье, к одной из традиционных [OpenAi gym](https://gym.openai.com/) сред - [Mountain Car](https://gym.openai.com/envs/MountainCar-v0/).

Классический DQN представляет собой довольно упрощенный подход, но в то же время этот рецепт является отличной отправной точкой для более глубокого изучения обучения с подкреплением.

## Начало работы

### 0. Зарегистрироваться

### 1. Установить CLI и зайти

```text
pip install -U neuromation
neuro login
```

### 2. Запустить рецепт

```text
git clone git@github.com:neuromation/ml-recipe-mountain-car.git
cd ml-recipe-mountain-car
make setup
make jupyter
```

