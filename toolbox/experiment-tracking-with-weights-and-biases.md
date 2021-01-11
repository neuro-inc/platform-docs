# Отслеживание экспериментов с Weights & Biases

### Введение

В данном руководстве мы покажем, как настроить отслеживание эксперимента через Weights & Biases на платформе Neuro, используя шаблон проекта Neuro.

### Создание проекта Neuro

Для создания нового проекта Neuro выполните команду:

```bash
neuro project init
cd <project-slug>
neuro-flow build myimage
```

### Аутентификация на W&B

Теперь можно подключать [Weights & Biases](https://www.wandb.com/) к вашему проекту:

1. [Зарегистрируйтесь на W&B](https://app.wandb.ai/login?signup=true)
2. Найдите ваш API-ключ \(также называемый _токен доступа_\) на[ странице настроек W&B](https://app.wandb.ai/settings) \(секция “API keys”\). Он дожен выглядеть подобным образом: `cf23df2207d99a74fbe169e3eba035e633b65d94`.
3. Сохраните API-ключ \(токен\) как файл в локальной домашней директории `~` и защитите его, выставляя соответствующий уровень доступа, чтобы W&B был доступен на платформе Neu.ro:

```text
export WANDB_SECRET_FILE=wandb-token.txt
echo "cf23df2207d99a74fbe169e3eba035e633b65d94" > ~/$WANDB_SECRET_FILE
chmod 600 ~/$WANDB_SECRET_FILE
```

После этого создайте секрет Neu.ro:

```text
neuro secrets add wandb-token @~/$WANDB_SECRET_FILE
```

Откройте `.neuro/live.yaml`, найдите в нём секцию `remote_debug` внутри `jobs` и добавьте следующие строки в её конце:

```bash
jobs:
  remote_debug:
     ...
     additional_env_vars: '{"WANDB_API_KEY": "secret:wandb-token"}'
```

Теперь Вы можете использовать W&B API в Вашем коде.

### Тестирование

Измените настройку по умолчанию на `cpu-small` в `.neuro/live.yaml`, чтобы избежать использования GPU для этого теста:

```bash
defaults:
  preset: cpu-small
```

Запустите задание и подключитесь к нему через оболочку shell:

```bash
neuro-flow run remote_debug
```

Попробуйте использовать команду `wandb`:

```bash
wandb status
```

Вы должны увидеть такой вывод:

```bash
Logged in? True
Current Settings
{
  "anonymous": "false",
  "base_url": "https://api.wandb.ai",
  "entity": null,
  "git_remote": "origin",
  "ignore_globs": [],
  "project": null,
  "run": "latest",
  "section": "default"
}
```

Вы также можете найти другие примеры использования отслеживания экспериментов и других функций W&B [в официальном репозитории примеров W&B](https://github.com/wandb/examples).

Чтобы закрыть сеанс удаленного терминала, нажмите `^D` или введите`exit`.

Пожалуйста, не забудьте прекратить работу задания, если оно Вам больше не нужно:

```bash
neuro-flow kill remote_debug
```

