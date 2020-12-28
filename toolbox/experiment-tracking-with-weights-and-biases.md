---
description: >-
  Данная страница немного устарела. Наша команда технических писателей работает
  над ней.
---

# Отслеживание экспериментов с Weights & Biases

## Введение

В данном руководстве мы покажем, как настроить отслеживание эксперимента через Weights & Biases на платформе Neuro, используя шаблон проекта Neuro.

## Создание проекта Neuro

Для создания нового проекта Neuro выполните команду:

```bash
neuro project init
cd <project-slug>
make setup
```

## Аутентификация на W&B

Зарегистрируйте учетную запись на [W&B](https://app.wandb.ai/login) \(примечание: W&B предлагает _бесплатный ограниченный аккаунт_ для индивидуальных исследователей, см. [W&B pricing policy](https://www.wandb.com/pricing)\). Затем загрузите API key на странице [W&B settings page](https://app.wandb.ai/settings) и сохраните его в локальный файл `./config/wandb-token.txt` \(Вы можете использовать другое имя файла, но в этом случае Вам нужно будет экспортировать имя файла в переменную окружения: `export WANDB_SECRET_FILE=<file-name>`\).

Убедитесь, что проект Neuro обнаружил файл ключа:

```bash
make wandb-check-auth
```

В случае успеха вывод команды должен быть таким: `Weights & Biases will be authenticated via key file: '/project-path/config/wandb-token.txt'`. Теперь Вы можете выполнять задания `develop`, `train` или `jupyter`, Neuro будет аутентифицировать W&B через Ваш API key, запуская при старте задания `wandb login`.

## Тестирование

Запустите задание и подключитесь к заданию через оболочку shell:

```bash
export PRESET=cpu-small  # to avoid consuming GPU for this test
make develop
make connect-develop
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

Вы можете найти реальный пример использования W&B на платформе Neuro в наших рецептах по ML для Hierarchical Attention. Вы также можете найти другие примеры использования отслеживания экспериментов и других функций W&B [в официальном репозитории примеров W&B](https://github.com/wandb/examples).

Чтобы закрыть сеанс удаленного терминала, нажмите `^D` или введите`exit`.

Пожалуйста, не забудьте прекратить работу задания, если оно Вам больше не нужно:

```bash
make kill-develop
```

