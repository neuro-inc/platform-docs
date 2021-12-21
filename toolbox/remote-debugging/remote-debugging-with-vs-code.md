# Удаленная отладка с помощью VS Code

### Введение

В данном руководстве мы покажем, как настроить удаленную отладку с помощью VS Code на платформе, используя шаблон проекта.

### Создание нового проекта

Во-первых, убедитесь, что у вас установлен и настроен клиент CLI и [пакет **cookiecutter**](https://github.com/cookiecutter/cookiecutter):

```bash
$ pipx install neuro-all cookiecutter
$ neuro login
```

Затем создайте пустой проект:

```bash
$ cookiecutter gh:neuro-inc/cookiecutter-neuro-project --checkout release
```

Эта команда задаст несколько вопросов о вашем проекте:

```
project_name [Name of the project]: Neuro VSCode
project_dir [neuro-vscode]:
project_id [neuro-vscode]:
code_directory [modules]: 
preserve Neuro Flow template hints [yes]:
```

### Настройка проекта

Добавьте `debugpy` в файл `requirements.txt` (он расположен в корневой папке Вашего проекта):&#x20;

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
$ neuro-flow build myimage
```

Когда образ будет собран, Вы можете загрузить свой код из локального файла на дисковое пространство платформы:

```
$ neuro-flow upload code
```

По умолчанию загрузит все файлы из папки `modules` Вашего проекта в папку `storage:<your_project_id>/modules` на дисковом пространстве платформы. Для изменения исходной и целевой папок этой команды, найдите секцию `code` в разделе`volumes` файла `.neuro/live.yml` Вашего проекта:

```
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

```
$ neuro-flow run train
```

Как только задание запустится, отключитесь от него в терминале, используя сочетания клавиш **Ctrl+P, Ctrl+Q,** и выполните такую команду:

```
$ neuro job port-forward <job-id> 5678:5678
```

Это даст Вам возможность получить доступ к заданию через порт `5678`.

### Отладка&#x20;

Откройте Ваш файл с кодом в VS Code и нажмите **Run > Start Debugging** или **F5**:

![](<../../.gitbook/assets/image (89).png>)

Выберите **Remote Attach**:

![](<../../.gitbook/assets/image (88) (1).png>)

Введите **localhost** в качестве имени хоста и номер порта задания (в данном примере - **5678**):

![](<../../.gitbook/assets/image (87).png>)

![](<../../.gitbook/assets/image (91) (1).png>)

После этого Вы можете выбрать точку прерывания и начать отладку.
