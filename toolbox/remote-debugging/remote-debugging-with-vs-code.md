# Удаленная отладка с помощью VS Code

### Введение

В данном руководстве мы покажем, как настроить удаленную отладку с помощью VS Code на платформе, используя шаблон проекта.

### Создание нового проекта

Во-первых, убедитесь, что у вас установлен и настроен клиент платформы: 

```bash
pip install -U neuro-cli neuro-extras neuro-flow
neuro login
```

Затем создайте пустой проект:

```bash
neuro project init
```

Если при запуске этой команды возникнет вопрос о загрузке проекта `\.cookiecutters\cookiecutter-neuro-project`, загрузите его заново.

Команда инициализации проекта задаст несколько вопросов о Вашем проекте:

```text
project_name [Name of the project]: Neuro VSCode
project_slug [neuro-vscode]: 
code_directory [modules]:
```

### Настройка проекта

Добавьте `debugpy` в файл `requirements.txt` \(он расположен в корневой папке Вашего проекта\): 

```bash
neuro-flow

flake8
mypy
isort
black
debugpy
```

Добавьте следующие строки в начало вашего файла с кодом. В этом примере, мы работаем с файлом `modules/train.py`.

```bash
import debugpy
debugpy.listen(("0.0.0.0", 5678))
debugpy.wait_for_client()
```

Далее, настройте рабочую среду проекта на платформе:

```bash
neuro-flow build myimage
```

Когда образ будет собран, Вы можете загрузить свой код из локального файла на дисковое пространство платформы:

```text
neuro-flow upload code
```

По умолчанию загрузит все файлы из папки `modules` Вашего проекта в папку `storage:<your_project_id>/modules` на дисковом пространстве платформы. Для изменения исходной и целевой папок этой команды, найдите секцию `code` в разделе`volumes` файла `.neuro/live.yml` Вашего проекта:

```text
volumes:
  data:
    remote: storage:$[[ flow.project_id ]]/data
    mount: /project/data
    local: data
  code:
    remote: storage:$[[ flow.project_id ]]/modules
    mount: /project/modules
    local: modules
  config:
    remote: storage:$[[ flow.project_id ]]/config
    mount: /project/config
    local: config
    read_only: True
```

Здесь Вы можете указать локальную папку с кодом и папку на дисковом пространстве платформы, в которую Вы хотите загрузить этот код.

### Запуск Вашего кода

В этом примере мы запустим задание по обучению, основанное на коде из файла `train.py`, который мы только что загрузили на дисковое пространство платформы. Для запуска задания выполните следующую команду:

```text
neuro-flow run train
```

Как только задание запустится, отключитесь от него в терминале, используя сочетания клавиш **Ctrl+P, Ctrl+Q,** и выполните такую команду:

```text
neuro job port-forward <job-id> 5678:5678
```

Это даст Вам возможность получить доступ к заданию через порт `5678`.

### Отладка 

Откройте Ваш файл с кодом в VS Code и нажмите **Run &gt; Start Debugging** или **F5**:

![](../../.gitbook/assets/image%20%2889%29.png)

Выберите **Remote Attach**:

![](../../.gitbook/assets/image%20%2888%29%20%281%29.png)

Введите **localhost** в качестве имени хоста и номер порта задания \(в данном примере - **5678**\):

![](../../.gitbook/assets/image%20%2887%29.png)

![](../../.gitbook/assets/image%20%2891%29%20%281%29.png)

После этого Вы можете выбрать точку прерывания и начать отладку.

