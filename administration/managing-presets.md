# Управление пресетами

Как только пулы узлов будут настроены на Вашем кластере, вы можете управлять пресетами ресурсов, которые будут доступны Вам и Вашей команде во время работы. 

### Проверка информации о пресетах кластера

Вы можете просмотреть пресеты Вашего текущего кластера, выполняя команду `neuro config show` и сверяясь с секцией **Resource Presets** в её выводе:

```text
Resource Presets:
 Name               #CPU   Memory   Preemptible   Preemptible Node   GPU                     Jobs Avail
────────────────────────────────────────────────────────────────────────────────────────────────────────
 cpu-small             1     4.0G        ×               ×                                           63
 cpu-medium            3    11.0G        ×               ×                                           18
 cpu-large             7    26.0G        ×               ×                                            7
 gpu-k80-small         5    48.0G        ×               ×           1 x nvidia-tesla-k80            28
 gpu-k80-small-p     5.0    48.0G        √               √           1 x nvidia-tesla-k80            10
 gpu-v100-small        5    95.0G        ×               ×           1 x nvidia-tesla-v100           10
 gpu-v100-small-p    5.0    95.0G        √               √           1 x nvidia-tesla-v100           10
```

### Модификация и добавление пресетов

Вы можете изменять и добавлять пресеты ресурсов с помощью команды `neuro admin update-resource-preset`. 

Например, чтобы изменить количество памяти, доступное в существующем пресете **cpu-large**, на 32ГБ, запустите следующую команду:

```bash
> neuro admin update-resource-preset -m 32G company-cluster cpu-large
```

Чтобы добавить новый пресет, пропишите его название и параметры в команде `neuro admin update-resource-preset`. Вы можете узнать больше об использовании этой команды [здесь](https://neu-ro.gitbook.io/neu-ro-cli-reference/commands/admin).

### Удаление пресетов

Вы можете удалять пресеты ресурсов, используя команду `neuro admin remove-resource-preset`. Например:

```bash
> neuro admin remove-resource-preset company-cluster cpu-medium
```

