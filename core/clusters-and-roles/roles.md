# Роли

Контроль доступа на платформе основывается на **ролях**. Каждая роль содержит в себе список разрешений доступа к различными сущностям и действиям. Таким образом, пользователи с разными ролями будут иметь разные полномочия на платформе. По умолчанию на каждом кластере есть три роли: **User**, **Manager** и **Admin** \(Вы можете узнать о них больше [здесь](../../administration/cluster-management/managing-users-and-quotas.md#kakie-roli-polzovatelei-sushestvuyut)\). Пользователи также могут создавать новые роли и предоставлять их другим пользователям.

## Создание новых ролей

Для создания ролей используется команда `neuro acl add-role {username}/roles/{rolename}`. Например:

```text
> neuro acl add-role alice/roles/newrole
```

Созданная роль будет называться `newrole` и её список разрешений будет пуст. Чтобы добавить в этот набор новые ресурсы, используйте команду`neuro acl grant {URI} {username}/roles/{rolename}`. Например:

```text
> neuro acl grant job:job363 alice/roles/newrole
```

Это добавит разрешение доступа к заданию `job363` в роль `newrole`.

## Использование ролей

### Предоставление ролей

Для предоставление ролей пользователям используется команда `neuro acl grant role://{username}/roles/{rolename} {username2}`:

```text
> neuro acl grant role://alice/roles/newrole bob
```

Это предоставит роль `newrole` пользователю Bob. Это значит, что у пользователя Bob будет доступ ко всем сущностям, перечисленным в списке разрешений этой роли.

### Отозвание ролей

Для отозвания ролей у пользователей используется команда `neuro acl revoke`. Например:

```text
> neuro acl revoke role://alice/roles/newrole bob
```

### Удаление ролей

Для удаления ролей используется команда `neuro acl remove-role`. Например:

```text
> neuro acl remove-role alice/roles/newrole
```

Вы можете найти больше информации об использовании команды `neuro acl` [здесь](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/acl).

