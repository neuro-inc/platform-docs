---
description: >-
  Данная страница немного устарела. Наша команда технических писателей работает
  над ней.
---

# Кластеры \(Ресурсы\)

Кластер является набором ресурсов - вычислительных, дисковых, репозиториев. Когда вы регистрируетесь на Neu.ro, мы предоставляем Вам доступ к нашему общедоступному кластеру neuro-public. Вы можете создавать новые кластеры, однако для этого потребуется дополнительный доступ. Для получения дополнительной информации обращайтесь [team@neu.ro](mailto:team@neu.ro).

Вы можете использовать один кластер, а затем переключиться на другой. Данное руководство поможет вам понять, как Вы можете управлять ресурсами.

## **Как посмотреть информацию о моем текущем кластере?**

Каждый кластер имеет свой собственный набор настроек, дисковое пространство и репозиторий. Вы можете использовать команду `neuro config show` для просмотра Вашего текущего кластера и детальной информации о нем.

```text
> neuro config show
User Configuration:
  User Name: clarytyllc
  Current Cluster: neuro-public
  API URL: https://staging.neu.ro/api/v1
  Docker Registry URL: https://registry.neuro-public.org.neu.ro
  Resource Presets:
    Name         #CPU  Memory  Preemptible  GPU
    cpu-small     1.0    2.0G       No
    cpu-large     7.0   28.0G       No
    gpu-small     3.0   57.0G       No      1 x nvidia-tesla-k80
    gpu-small-p   3.0   57.0G      Yes      1 x nvidia-tesla-k80
    gpu-large     7.0   57.0G       No      1 x nvidia-tesla-v100
    gpu-large-p   7.0   57.0G      Yes      1 x nvidia-tesla-v100
```

Команда отображает конфигурацию текущего кластера, такую как имя кластера, API URL, URL-адрес репозитория Docker и список настроек, доступных для кластера.

## **Как просмотреть все мои кластеры и переключиться с текущего кластера?**

Вы можете просмотреть информацию о кластерах, к которым у Вас есть доступ. Для этого надо использовать команду `neuro config get-clusters`.

```text
> neuro config get-clusters
Fetch the list of available clusters...
Available clusters:
* Name: neuro-public
  Presets:
    Name         #CPU  Memory  Preemptible  GPU
    cpu-small       1    2.0G       No
    cpu-large       7   28.0G       No
    gpu-small       3   57.0G       No      1 x nvidia-tesla-k80
    gpu-small-p     3   57.0G      Yes      1 x nvidia-tesla-k80
    gpu-large       7   57.0G       No      1 x nvidia-tesla-v100
    gpu-large-p     7   57.0G      Yes      1 x nvidia-tesla-v100
  Name: onprem
  Presets:
    Name       #CPU  Memory  Preemptible  GPU                          
    cpu-small     1    4.0G       No                                   
    cpu-large     4   10.0G       No                                   
    gpu-small    23   60.0G       No      1 x nvidia-geforce-rtx-2080ti
    gpu-large    46  120.0G       No      2 x nvidia-geforce-rtx-2080ti
```

Чтобы перейти с одного кластера на другой необходимо использовать команду `neuro config switch-cluster`. После ввода команды Вам будет предложено ввести имя кластера, на который Вы хотите перейти. Переключение происходит после ввода имени.

```text
> neuro config switch-cluster
Fetch the list of available clusters...
Available clusters:
* Name: neuro-public
  Presets:
    Name         #CPU  Memory  Preemptible  GPU
    cpu-small       1    2.0G       No
    cpu-large       7   28.0G       No
    gpu-small       3   57.0G       No      1 x nvidia-tesla-k80
    gpu-small-p     3   57.0G      Yes      1 x nvidia-tesla-k80
    gpu-large       7   57.0G       No      1 x nvidia-tesla-v100
    gpu-large-p     7   57.0G      Yes      1 x nvidia-tesla-v100
  Name: onprem
  Presets:
    Name       #CPU  Memory  Preemptible  GPU                          
    cpu-small     1    4.0G       No                                   
    cpu-large     4   10.0G       No                                   
    gpu-small    23   60.0G       No      1 x nvidia-geforce-rtx-2080ti
    gpu-large    46  120.0G       No      2 x nvidia-geforce-rtx-2080ti

Select cluster to switch [neuro-public]: onprem
The current cluster is onprem
```

## **Как посмотреть мои квоты вычислительных ресурсов?**

При регистрации на neu.ro Вам дается квота вычислений в 100 часов работы GPU. Эта квота используется всякий раз, когда вы запускаете задания. Neu.ro предоставляет неограниченные часы работы CPU, которые вы можете использовать для своей работы.

Вы можете просмотреть оставшуюся квоту вычислительных ресурсов на панели инструментов.

![Neu.ro Dashboard](../.gitbook/assets/cluster_dashboard.jpg)

Оставшуюся квоту также можно посмотреть при помощи команды neuro config show-quota.

```text
> neuro config show-quota
GPU: spent: 35h 54m (quota: 100h 00m, left: 64h 06m)
CPU: spent: 168h 49m (quota: infinity)
```

## Как запросить дополнительную квоту вычислений?

Вы можете пополнить квоту вычислений, нажав кнопку **Top Up** в панели инструментов.

![Top Up button](../.gitbook/assets/cluster_topup.jpg)

Вы можете написать на [team@neu.ro](mailto:team@neu.ro), чтобы узнать о последних скидках и акциях, а так же запросить пополнение.

## Как я могу создать новый кластер?

Neu.ro позволяет создавать новые кластеры для лучшего управления и координации ресурсов. Однако Вы должны помнить, что для нового кластера возможно потребуется большая квота вычислительных ресурсов. Прежде чем создавать кластер, Вы должны выбрать настройки, дисковое пространство и репозиторий, которые Вы хотите назначить для нового кластера. Вы можете создать кластер, написав нам по адресу [team@neu.ro](mailto:team@neu.ro).

