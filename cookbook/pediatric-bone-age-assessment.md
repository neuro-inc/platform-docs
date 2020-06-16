# Оценка костного возраста детей

[Запуск на Neu.ro](https://apps.neu.ro/ml-recipes/bone-age)

[Репозиторий на GitHub](https://github.com/neuromation/ml-recipe-bone-age)

В этом проекте мы изучаем проблему оценки костного возраста у детей. В ходе развития организма кости скелета меняются в размерах и форме. Разница, между определенным у ребенка костным возрастом и хронологическим возрастом может указывать на проблему роста. Врачи используют оценку костного возраста для оценки зрелости скелетной системы ребенка.

Оценка костного возраста обычно начинается с получения рентгеновского снимка левой руки от запястья до кончиков пальцев. Традиционно кости на рентгенограмме сравниваются с изображениями в стандартизированном атласе развития костей. Этот рецепт представляет собой основной подход, описанный в _"Paediatric Bone Age Assessment Using Deep Convolutional Neural Networks" by V. Iglovikov, A. Rakhlin, A. Kalinin and A. Shvets_, [link 1](https://link.springer.com/chapter/10.1007%2F978-3-030-00889-5_34), [2](https://www.biorxiv.org/content/biorxiv/early/2018/06/20/234120.full.pdf).

Мы проверяем эффективность метода, используя данные конкурса «Pediatric Bone Age Challenge 2017», организованного Радиологическим обществом Северной Америки \(RSNA\). Набор данных предоставлен 3 медицинскими центрами - Стэнфордского университета, Университета Колорадо и Калифорнийского университета в Лос-Анджелесе. Первоначально набор данных был предоставлен Центром AIMI Стэнфордского университета и теперь он свободно доступен на [платформе Kaggle](https://kaggle.com/kmader/rsna-bone-age). Для простоты мы пропускаем интенсивные этапы предварительной обработки, как это описано в оригинальной работе и предоставляем рентгенограммы с уже удаленным фоном и единообразными изображениями рук.

![Original radiograph of a hand of 82 month old \(approx. 7 y.o.\) girl](../.gitbook/assets/1381_original.png)

![Preprocessed radiograph of a hand of 82 month old \(approx. 7 y.o.\) girl](../.gitbook/assets/1381_preprocessed.png)

### Технологии

* `Catalyst` как конвейер для задач глубокого обучения. Эта новая и быстро развивающаяся [библиотека](https://github.com/catalyst-team/catalyst) может значительно сократить объем стандартного кода. Если вы знакомы с экосистемой TensorFlow, вы можете думать о Catalyst как о Keras для PyTorch. Этот фреймворк интегрирован с системами логирования, такими как хорошо известный [TensorBoard](https://www.tensorflow.org/tensorboard) и новый [Weights & biases](https://www.wandb.com/).

## Начало работы

#### 0. Зарегистрироваться

#### 1. Установить CLI и зайти

```text
pip install -U neuromation
neuro login
```

#### 2. Запустить рецепт

```text
git clone git@github.com:neuromation/ml-recipe-bone-age.git
cd ml-recipe-bone-age
make setup
make jupyter
```

#### 3. Обучить модель

Загрузить набор данных в demo notebook и выполнить команду:

```text
make training
```

