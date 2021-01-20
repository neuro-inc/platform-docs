# Обучение Вашей первой модели

## Введение

В данном руководстве описывается рекомендуемый способ обучения простой модели машинного обучения на платформе. Так как наши инженеры предпочитают PyTorch другим инструментам ML, мы показываем обучение и оценку на примере PyTorch.

Мы предполагаем, что вы уже зарегистрировались на платформе, установили Neuro CLI и залогинились \(смотри [Начало работы](getting-started.md)\).

Наш пример основывается на [Classifying Names with a Character-Level RNN](https://pytorch.org/tutorials/intermediate/char_rnn_classification_tutorial.html).

### Создание нового проекта

Чтобы упростить работу с платформой и помочь внедрить лучшие практики ML, мы предоставляем шаблон проекта. Этот шаблон состоит из рекомендуемых каталогов и файлов. Он предназначен для удобной работы с нашим [базовым рабочим окружением](https://hub.docker.com/r/neuromation/base).

Создайте новый проект на основе шаблона:

```text
neuro project init
```

Данная команда запрашивает несколько вопросов относительно Вашего проекта:

```text
project_name [Name of the project]: Neuro Tutorial
project_slug [neuro-tutorial]: 
code_directory [modules]: rnn
```

### Структура проекта

После выполнения вышеприведенной команды Вы получите следующую структуру:

```text
neuro-tutorial
├── .neuro/             <- live.yaml file with commands for manipulating training environment
├── data/               <- training and testing datasets (we do not keep it under source control)
├── notebooks/          <- Jupyter notebooks
├── rnn/                <- source code of models
├── .gitignore          <- default .gitignore for a Python project
├── README.md           <- auto-generated informational file
├── apt.txt             <- list of system packages to be installed in the training environment 
├── requirements.txt    <- list Python dependencies to be installed in the training environment     
└── setup.cfg           <- linter settings (Python code quality checking)
```

Когда Вы запустите задание \(например, при помощи команды `neuro-flow run jupyter`\), каталоги примонтируются к заданию следующим образом:

| Точка монтирования | Описание | URI хранения данных |
| :--- | :--- | :--- |
| `/project/data/` | Данные обучения / тестирования | `storage:neuro-tutorial/data/` |
| `/project/rnn/` | Пользовательский код Python | `storage:neuro-tutorial/rnn/` |
| `/project/notebooks/` | Пользовательский Jupyter notebook | `storage:neuro-tutorial/notebooks/` |
| `/project/results/` | Файлы логирования и результат | `storage:neuro-tutorial/results/` |

Данное сопоставление определено в переменных в верхней части файла `Makefile` и при необходимости может быть изменено.

### Заполнение проекта

Теперь необходимо наполнить вновь созданный проект контентом:

* Меняем рабочую директорию:

```text
cd neuro-tutorial
```

* Копируем [модель](https://github.com/pytorch/tutorials/blob/master/intermediate_source/char_rnn_classification_tutorial.py) в папку `rnn`:

```text
curl https://raw.githubusercontent.com/pytorch/tutorials/master/intermediate_source/char_rnn_classification_tutorial.py -o rnn/char_rnn_classification_tutorial.py
```

* Добавим следующие строки в файл `requirements.txt` в корневой папке проекта:

```text
sphinx==1.8.2
sphinx-gallery==0.3.1
sphinx-copybutton
tqdm
numpy
matplotlib
torch
torchtext
PyHamcrest
bs4

# PyTorch Theme
-e git+git://github.com/pytorch/pytorch_sphinx_theme.git#egg=pytorch_sphinx_theme

ipython

# to run examples
pandas
# pillow >= 4.2 will throw error when trying to write mode RGBA as JPEG,
# this is a workaround to the issue.
pillow
wget

```

* Загрузим [отсюда](https://download.pytorch.org/tutorial/data.zip) данные, распакуем ZIP содержимое и поместим его в папку `data`:

```text
curl https://download.pytorch.org/tutorial/data.zip -o data/data.zip && unzip data/data.zip && rm data/data.zip
```

### Обучение и оценка модели

Когда вы начинаете работать с проектом на платформе, основной процесс выглядит следующим образом: настраиваете удаленное рабочее окружение, загружаете данные и код на дисковое пространство, проводите обучение и оцениваете результат.

Чтобы настроить удаленное рабочее окружение, запустите команду

```text
neuro-flow build myimage
```

Данная команда запускает задание \(через команду `neuro run`\), загружает файлы зависимостей `apt.txt` и `requirements.txt` \(через команду `neuro cp`\), устанавливает зависимости \(используя `neuro exec`\), выполняет другие подготовительные действия, а затем создает базовый образ из данного задания и загружает его на платформу \(с помощью функции `neuro save`, которая работает аналогично `docker commit`\).

Чтобы загрузить данные и код на Ваш диск, запустите команду

```text
neuro-flow upload ALL
```

Чтобы запустить обучение, необходимо указать команду для запуска в файле `.neuro/live.yaml`, а затем выполнить `neuro-flow run train`:

* откройте в редакторе файл `.neuro/live.yaml`,
* найдите следующую запись в секции `train`:

```text
    bash: |
        cd $[[ flow.workspace ]]
        python -u $[[ volumes.code.mount ]]/train.py --data $[[ volumes.data.mount ]]
```

* и замените ее на нижеприведенную запись: 

```text
    bash: |
        cd $[[ flow.workspace ]]
        python -u $[[ volumes.code.mount ]]/char_rnn_classification_tutorial.py
```

Теперь можно запустить команду

```text
neuro-flow run train
```

и наблюдать за выводом. Вы увидите, как выполняются некоторые проверки в начале скрипта, а затем происходит обучение модели и оценка:

```text
['data/names/German.txt', 'data/names/Polish.txt', 'data/names/Irish.txt', 'data/names/Vietnamese.txt', 
'data/names/French.txt', 'data/names/Japanese.txt', 'data/names/Spanish.txt', 'data/names/Chinese.txt', 
'data/names/Korean.txt', 'data/names/Czech.txt', 'data/names/Arabic.txt', 'data/names/Portuguese.txt', 
'data/names/English.txt', 'data/names/Italian.txt', 'data/names/Russian.txt', 'data/names/Dutch.txt', 
'data/names/Scottish.txt', 'data/names/Greek.txt']
Slusarski
['Abandonato', 'Abatangelo', 'Abatantuono', 'Abate', 'Abategiovanni']
tensor([[0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0.,
         0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 1.,
         0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0.,
         0., 0., 0.]])
torch.Size([5, 1, 57])
tensor([[-2.8248, -2.9118, -2.8999, -2.9170, -2.8916, -2.9699, -2.8785, -2.9273,
         -2.8397, -2.8539, -2.8764, -2.9278, -2.8638, -2.9310, -2.9546, -2.9008,
         -2.8295, -2.8441]], grad_fn=<LogSoftmaxBackward>)
('German', 0)
category = Vietnamese / line = Vu
category = Chinese / line = Che
category = Scottish / line = Fraser
category = Arabic / line = Abadi
category = Russian / line = Adabash
category = Vietnamese / line = Cao
category = Greek / line = Horiatis
category = Portuguese / line = Pinho
category = Vietnamese / line = To
category = Scottish / line = Mcintosh
5000 5% (0m 19s) 2.7360 Ho / Portuguese ✗ (Vietnamese)
10000 10% (0m 38s) 2.0606 Anderson / Russian ✗ (Scottish)
15000 15% (0m 58s) 3.5110 Marqueringh / Russian ✗ (Dutch)
20000 20% (1m 17s) 3.6223 Talambum / Arabic ✗ (Russian)
25000 25% (1m 35s) 2.9651 Jollenbeck / Dutch ✗ (German)
30000 30% (1m 54s) 0.9014 Finnegan / Irish ✓
35000 35% (2m 13s) 0.8603 Taverna / Italian ✓
40000 40% (2m 32s) 0.1065 Vysokosov / Russian ✓
45000 45% (2m 52s) 3.6136 Blanxart / French ✗ (Spanish)
50000 50% (3m 11s) 0.0969 Bellincioni / Italian ✓
55000 55% (3m 30s) 3.1383 Roosa / Spanish ✗ (Dutch)
60000 60% (3m 49s) 0.6585 O'Kane / Irish ✓
65000 65% (4m 8s) 4.7300 Satorie / French ✗ (Czech)
70000 70% (4m 27s) 0.9765 Mueller / German ✓
75000 75% (4m 46s) 0.7882 Attia / Arabic ✓
80000 80% (5m 5s) 2.1131 Till / Irish ✗ (Czech)
85000 85% (5m 25s) 0.5304 Wei / Chinese ✓
90000 90% (5m 44s) 1.6258 Newman / Polish ✗ (English)
95000 95% (6m 2s) 3.2015 Eberhardt / Irish ✗ (German)
100000 100% (6m 21s) 0.2639 Vamvakidis / Greek ✓

> Dovesky
(-0.77) Czech
(-1.11) Russian
(-2.03) English

> Jackson
(-0.92) English
(-1.65) Czech
(-1.85) Scottish

> Satoshi
(-1.32) Italian
(-1.81) Arabic
(-2.14) Japanese
```

