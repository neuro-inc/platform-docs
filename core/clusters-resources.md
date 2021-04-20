# Кластеры \(Ресурсы\)

Кластер является набором ресурсов - вычислительных, дисковых, и репозиториев. Когда вы регистрируетесь на платформе, мы предоставляем Вам доступ к нашему общедоступному кластеру neuro-compute. Вы можете создавать новые кластеры, однако для этого потребуется дополнительный доступ. Для получения дополнительной информации обращайтесь [team@neu.ro](mailto:team@neu.ro).

Вы можете использовать один кластер, а затем переключиться на другой. Данное руководство поможет вам понять, как Вы можете управлять ресурсами.

### **Как посмотреть информацию о моем текущем кластере?**

Каждый кластер имеет свой собственный набор настроек, дисковое пространство и репозиторий. 

Просмотреть информацию о текущем кластере можно как в CLI, так и в Веб-интерфейсе:

{% tabs %}
{% tab title="CLI" %}
Вы можете использовать команду `neuro config show` для просмотра имени текущего кластера, его API URL, URL-адреса репозитория Docker и списка доступных пресетов.

```text
> neuro config show
User Configuration:
 User Name            john-doe
 Current Cluster      neuro-compute
 API URL              https://staging.neu.ro/api/v1
 Docker Registry URL  https://registry.neuro-compute.org.neu.ro
Resource Presets:
 Name               #CPU   Memory   Preemptible   Preemptible Node   GPU                     Jobs Avail
────────────────────────────────────────────────────────────────────────────────────────────────────────
 cpu-small             1     4.0G        ×               ×                                           49
 cpu-medium            3    11.0G        ×               ×                                           14
 cpu-large             7    26.0G        ×               ×                                            6
 gpu-k80-small         5    48.0G        ×               ×           1 x nvidia-tesla-k80            26
 gpu-k80-small-p     5.0    48.0G        √               √           1 x nvidia-tesla-k80            30
 gpu-v100-small        5    95.0G        ×               ×           1 x nvidia-tesla-v100           10
 gpu-v100-small-p    5.0    95.0G        √               √           1 x nvidia-tesla-v100           10
```
{% endtab %}

{% tab title="Веб-интерфейс" %}
Секция **Информация** содержит имя текущего кластера, список доступных пресетов, а также вашу роль на нём:

![](../.gitbook/assets/image%20%28150%29.png)
{% endtab %}
{% endtabs %}

### **Как просмотреть все мои кластеры и переключиться с текущего кластера?**

{% tabs %}
{% tab title="CLI" %}
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
* Name: neuro-compute
  Resource Presets:
   Name               #CPU   Memory   Preemptible   Preemptible Node   GPU
  ───────────────────────────────────────────────────────────────────────────────────────────
   cpu-small             1     4.0G        ×               ×
   cpu-medium            3    11.0G        ×               ×
   cpu-large             7    26.0G        ×               ×
   gpu-k80-small         5    48.0G        ×               ×           1 x nvidia-tesla-k80
   gpu-k80-small-p     5.0    48.0G        √               √           1 x nvidia-tesla-k80
   gpu-v100-small        5    95.0G        ×               ×           1 x nvidia-tesla-v100
   gpu-v100-small-p    5.0    95.0G        √               √           1 x nvidia-tesla-v100
  Name: onprem-poc
  Resource Presets:
   Name         #CPU   Memory   Preemptible   Preemptible Node   GPU
  ─────────────────────────────────────────────────────────────────────────────────────────────
   cpu-nano      0.2     1.0G        ×               ×
   cpu-small       1     4.0G        ×               ×
   cpu-large       4    10.0G        ×               ×
   gpu-small      23    60.0G        ×               ×           1 x nvidia-geforce-rtx-2080ti
   gpu-large      47   120.0G        ×               ×           2 x nvidia-geforce-rtx-2080ti
   gpu-1x3090   23.0    60.0G        ×               ×           1 x nvidia-geforce-rtx-3090
   gpu-2x3090   47.0   120.0G        ×               ×           2 x nvidia-geforce-rtx-3090
Select cluster to switch [neuro-compute]: onprem-poc
The current cluster is onprem-poc
```
{% endtab %}

{% tab title="Веб-интерфейс" %}
Используйте выпадающий список **Кластер** для просмотра доступных кластеров и переключения между ними:

![](../.gitbook/assets/image%20%28154%29.png)
{% endtab %}
{% endtabs %}

### **Как посмотреть мои квоты вычислительных ресурсов?**

При регистрации на платформе Вам дается квота вычислений в 100 кредитов. Эта квота используется всякий раз, когда вы запускаете задания. 

Вы можете просмотреть оставшуюся квоту вычислительных ресурсов на панели инструментов веб-интерфейса.

Оставшуюся квоту также можно посмотреть при помощи команды `neuro config show-quota`.

### Как запросить дополнительную квоту вычислений?

Вы можете пополнить квоту вычислений, нажав **ПОПОЛНИТЬ СЕЙЧАС** в панели инструментов веб-интерфейса.

Вы можете написать на [team@neu.ro](mailto:team@neu.ro), чтобы узнать о последних скидках и акциях, а также запросить пополнение.

### Как я могу создать новый кластер?

Платформа позволяет создавать новые кластеры для лучшего управления и координации ресурсов. Однако Вы должны помнить, что для нового кластера возможно потребуется большая квота вычислительных ресурсов. Прежде чем создавать кластер, Вы должны выбрать настройки, дисковое пространство и репозиторий, которые Вы хотите назначить для нового кластера. Вы можете создать кластер, написав нам по адресу [team@neu.ro](mailto:team@neu.ro).

