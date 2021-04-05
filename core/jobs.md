# Задания

Задание - это простейший исполняемый элемент. Вы можете запустить задание в заданном рабочем окружении с заданным набором ресурсов и с заданным объемом дискового пространства для хранения данных. Задания являются строительными блоками Вашего проекта и должны быть тщательно спланированы для оптимального использования ресурсов.

Прежде чем запустить задание, необходимо определить:

* Образ Docker, используемый для запуска задания. Обратите внимание, что задание завершается, если запуск контейнера Docker завершается сбоем, исключая случаи, когда для задания используется специальная политика перезапуска.
* Предустановку \(далее пресет\) - комбинацию ресурсов CPU, GPU и памяти для использования в задании.

В сложных проектах Вы можете иметь несколько заданий, запущенных с разными пресет, которые лучше всего подходят для работы конкретного задания.

### Что такое пресет?

Платформа позволяет запускать задание в рабочем окружении с заданным пресет с несколькими томами для хранения данных. Здесь под пресет понимается комбинация ресурсов CPU, GPU и памяти.

Вы должны определить и установить количество ресурсов CPU, GPU и памяти, которые Вы хотите использовать для работы. По умолчанию используется пресет cpu-small. Данные ограничения гарантируют лучшее использование ресурсов в вычислительном кластере.

Например, в данной команде мы используем пресет cpu-small, поскольку задание не требует большой вычислительной мощности.

**Пример команды:**

```text
(base) C:\Projects>neuro run --preset cpu-small --name test ubuntu echo Hello, World! 
√ Job ID: job-aba12927-08f0-402c-8ed0-0a014bbdf87b
√ Name: test
- Status: pending Creating
- Status: pending Scheduling
√ Http URL: https://test--jane-doe.jobs.neuro-compute.org.neu.ro
√ The job will die in a day. See --life-span option documentation for details.
√ Status: succeeded
√ =========== Job is running in terminal mode ===========
√ (If you don't see a command prompt, try pressing enter)
√ (Use Ctrl-P Ctrl-Q key sequence to detach from the job)
Hello, World!
```

Платформа имеет набор пресетов, которые подходят для выполнения различных видов рабочих нагрузок. Некоторые задания могут также потребовать ресурсов GPU. Вы можете просмотреть список доступных пресетов, используя команду `neuro config show`.

```text
(base) C:\Projects>neuro config show
User Configuration:
 User Name            jane-doe
 Current Cluster      neuro-compute
 API URL              https://staging.neu.ro/api/v1
 Docker Registry URL  https://registry.neuro-compute.org.neu.ro
Resource Presets:
 Name               #CPU   Memory   Preemptible   Preemptible Node   GPU                     Jobs Avail
────────────────────────────────────────────────────────────────────────────────────────────────────────
 cpu-small             1     4.0G        ×               ×                                           65
 cpu-medium            3    11.0G        ×               ×                                           18
 cpu-large             7    26.0G        ×               ×                                            8
 gpu-k80-small         5    48.0G        ×               ×           1 x nvidia-tesla-k80            25
 gpu-k80-small-p     5.0    48.0G        √               √           1 x nvidia-tesla-k80            10
 gpu-v100-small        5    95.0G        ×               ×           1 x nvidia-tesla-v100           10
 gpu-v100-small-p    5.0    95.0G        √               √           1 x nvidia-tesla-v100           10
```

Команда выводит список доступных пресетов и их конфигураций. Например, пресет **cpu-small** включает в себя 1 CPU, 4 ГБ памяти, без GPU. С другой стороны, **gpu-k80-small** включает 5 CPU, 48 ГБ памяти и графический процессор nvidia-tesla-k80.

### Как запустить задание?

Чтобы запустить задание в CLI, Вы можете использовать команду `neuro run`. Эта команда принимает множество различных аргументов, большинство из них описаны в этом и следующих разделах.

Каждое задание имеет уникальный идентификатор ID. Для Вашего удобства Вы можете дать имя заданию. Может быть только одно с заданным именем в статусе PENDING или RUNNING.

Каждое задание имеет доступ к своему временному дисковому пространству \(которое, по сути, является частью SSD на физической машине, на которой выполняется это задание\). Данный тип хранилища быстрый, но не постоянный: как только вы завершаете задание, данные теряются.

Чтобы сохранить данные, Вы можете подключить к заданию тома для хранения данных. Этот тип хранения немного медленнее и имеет некоторые ограничения. Например, обучение модели на данных из примонтированной папки обычно выполняется на 10-20% медленнее. Кроме того, случайные операции записи \(например, разархивирование\) очень медленны и крайне не рекомендуются.

**Пример команды:**

* **Выполнить короткое задание без монтажа дискового пространства:**

```text
(base) C:\Projects>neuro run --preset cpu-small --name job230 ubuntu echo Hello, World!
√ Job ID: job-3ef0d955-bd2e-491a-aaea-f17b418fd4e8
√ Name: job230
- Status: pending Creating
- Status: pending Scheduling
√ Http URL: https://job230--jane-doe.jobs.neuro-compute.org.neu.ro
√ The job will die in a day. See --life-span option documentation for details.
√ Status: succeeded
√ =========== Job is running in terminal mode ===========
√ (If you don't see a command prompt, try pressing enter)
√ (Use Ctrl-P Ctrl-Q key sequence to detach from the job)
Hello, World!
```

* **Выполнить длительное задание с монтажом томов для хранения данных**

```text
(base) C:\Projects>neuro run --name job303 --volume storage:nero-assistant/ModelCode/:/code:rw --preset cpu-small neuromation/base python /code/train.py -d /data
√ Job ID: job-5a4942de-06ac-489d-8ac8-399640904991
√ Name: job303
- Status: pending Creating
- Status: pending Scheduling
√ Http URL: https://job303--jane-doe.jobs.neuro-compute.org.neu.ro
√ The job will die in a day. See --life-span option documentation for details.
√ Status: succeeded
√ =========== Job is running in terminal mode ===========
√ (If you don't see a command prompt, try pressing enter)
√ (Use Ctrl-P Ctrl-Q key sequence to detach from the job)
Your training script here. Data directory: /data
```

### Как можно увидеть список текущих выполняемых заданий?

Вы можете использовать команду `neuro ps` для просмотра списка заданий, которые выполняются в данный момент. В данной команде можно использовать различные параметры для фильтрации списка заданий по статусу, владельцу или по имени. Чтобы узнать информацию о конкретном задании, необходимо использовать команду `neuro job status`.

**Пример команд:**

1. **Посмотреть список всех работающих в данный момент заданий**

```text
(base) C:\Projects>neuro ps
ID                                       NAME   STATUS  WHEN           IMAGE          OWNER  
job-3erw4f2e-cc57-4e4b-af04-c795b76d9ca8 job363 running 6 seconds ago  ubuntu:latest  <you>  
job-d2c04f2e-cc57-4e4b-af04-c795b76d9ca8 job390 pending 26 seconds ago ubuntu:latest  <you>  
```

1. **Посмотреть список заданий в статусе pending**

```text
(base) C:\Projects>neuro ps -s pending
ID                                        NAME   STATUS   WHEN          IMAGE         OWNER   
job-d2c04f2e-cc57-4e4b-af04-c795b76d9ca8  job390 running  3 minutes ago ubuntu:latest <you>
```

### Могу ли я подключиться к заданию, когда оно работает?

При выполнении задания иногда может потребоваться подключиться к нему и выполнить какую-либо команду. Вы можете использовать команду `neuro job exec` для подключения к работающему заданию.

**Пример команды:**

* **Выполнение простой команды list в контейнере, в котором размещено задание**

```text
(base) C:\Projects>neuro job exec job363 ls
bin dev home lib32 libx32 mnt proc run srv tmp varboot etc lib lib64 media opt root sbin sys usr
Connection to ssh-auth.neuro-public.org.neu.ro closed.
```

* **Предоставление bash-терминала для контейнера, в котором размещено задание**

```text
(base) C:\Projects>neuro job exec job363 /bin/bash
root@job-36d59977-84d2-40e5-9475-e4af25a06b6c:/# echo "Hello, World!"
Hello, World!
root@job-36d59977-84d2-40e5-9475-e4af25a06b6c:/# exit
exit 
Connection to ssh-auth.neuro-public.org.neu.ro closed.
```

Терминал bash позволяет работать в контейнере во время выполнения задания.

### Какое состояние имеет задание?

Задание - это простейший исполняемый элемент, который выполняется до завершения или до тех пор, пока не будет уничтожено. Задание, пока не завершится или не потерпит неудачу, проходит через множество состояний. Вы можете просмотреть текущее состояние задания с помощью команды `neuro job status`.

**Пример команды:**

```text
(base) C:\Projects>neuro job status filebrowser-49249
Job                      job-d31c2ce9-f27b-4de0-9b60-b619ff6ff2af
Name                     filebrowser-49249
Tags                     kind:web-widget, target:filebrowser
Owner                    jane-doe
Cluster                  neuro-compute
Status                   running
Image                    filebrowser/filebrowser:v2-alpine
Command                  --noauth --root /var/storage
Resources                 Memory              4.0G
                          CPU                  1.0
                          Extended SHM space  True
Life span                1d
TTY                      False
Volumes                   /var/storage         storage:
                          /var/storage/.neuro  storage:.neuro/
Internal Hostname        job-d31c2ce9-f27b-4de0-9b60-b619ff6ff2af.platform-jobs
Internal Hostname Named  filebrowser-49249--jane-doe.platform-jobs
Http URL                 https://filebrowser-49249--jane-doe.jobs.neuro-compute.org.neu.ro
Http port                80
Http authentication      True
Environment               NEURO_STEAL_CONFIG   /var/storage/.neuro/85f68be9-6230-40ca-9c07-80d43275ee94-cfg
                          NEURO_PASSED_CONFIG  eyJ0b2tlbiI6ICJleUpoYkdjaU9pSklVekkxTmlJc0luUjVjQ0k2SWtwWFZDSjkuZXlK…
Created                  2021-01-12T19:36:37.468683+00:00
Started                  2021-01-12T19:36:52.733308+00:00
```

Задание может иметь одно из следующих состояний:

* Pending: Когда задание создано и ресурсы распределены.
* Running: Когда задание выполняется.
* Complete: Когда задание завершено.
* Failed: Когда задание не выполняется и завершено с кодом ошибки.

### Как выставить HTTP-сервер, работающий в задании?

Многие приложения, которые Вы запускаете на платформе, имеют некоторый веб-интерфейс, например, Jupyter Notebooks, TensorBoard и другие. Когда Вы запускаете задание, содержащее такое приложение, Вы можете получить доступ к этому веб-интерфейсу в Вашем браузере. Для этого Вам нужно передать порт, который должен быть открыт, через опцию `--port` \(по умолчанию 80\).

Чтобы открыть выставленный интерфейс в браузере, есть несколько вариантов:

* Передайте `--browse` в качестве параметра команде `neuro run`. В этом случае после запуска задания будет открыт веб-браузер, который установлен в операционной системе по умолчанию;
* Выполните `neuro job browse <NAME or ID>`, когда задание уже запущено;
* Нажмите на HTTP URL для Вашего задания на панели инструментов Neu.ro.

Все выполняемые задания по умолчанию скрыты за SSO. Это означает, что если Вы поделитесь ссылкой на веб-интерфейс задания с кем-либо, то ему придется войти на платформу и иметь разрешение на доступ к заданию \(см. раздел ниже\). Чтобы разрешить доступ всем, нужно передать `--no-http-auth` в команду `neuro run`. Мы настоятельно рекомендуем избегать этой опции, если Вы не уверены, что хотите пропустить проверку безопасности SSO.

Пример:

```text
neuro run --name filebrowser-demo --preset cpu-small --http 8085 --no-http-auth --browse --volume storage::/srv:rw filebrowser/filebrowser --noauth --port 8085
```

Данная команда запускает экземпляр FileBrowser на порту 8085, открывает этот порт, удаляет проверку SSO и открывает веб-интерфейс в браузере по умолчанию.

### Как контролировать продолжительность работы задания?

Вы можете контролировать продолжительность выполнения заданий, используя параметр конфигурации `life-span`. Вы можете обновить параметр `life-span` в разделе \[job\] файла глобальной конфигурации. Файл глобальной конфигурации находится по стандартному пути конфигурации платформы. Neu.ro CLI по умолчанию использует папку `~/.neuro`, а путь к файлу глобальной конфигурации `~/.neuro/user.toml`.

Параметр ограничивает время выполнения задания и имеет строковый формат. Например, значение 2d3h20min ограничит время выполнения задания 2 днями, 3 часами и 20 минутами.

**Пример:**

```text
# jobs section
[job]
life-span = "2d3h20min"
```

Этот параметр также можно выставлять отдельным заданиям с помощью опции `life-span`: 

```text
$ neuro run --life-span 2h <имя-задания>
```

Дополнительно есть возможность увеличить продолжительность жизни задания с помощью `bump-life-span`:

```text
$ neuro job bump-life-span <имя-задания> 2d
```

Это добавит 2 дня к текущей продолжительности жизни задания `<имя-задания>`.

### Как завершить задание?

Можно завершить любое задание, используя команду `neuro job kill`. Для завершения задания необходимо знать имя задания или его ID.

**Пример команды:**

```text
(base) C:\Projects>neuro job kill filebrowser-49249
job-d31c2ce9-f27b-4de0-9b60-b619ff6ff2af
```

### Возможно ли предоставить доступ к заданию другим пользователям?

Да, вы можете делиться текущими заданиями с коллегами по команде. Вы можете получить все детали текущих выполняемых заданий, используя команду `neuro ps`. Эта команда выводит список всех заданий, которыми вы владеете и к которым Вы имеете доступ.

**Пример команды для просмотра всех работающих заданий:**

```text
(base) C:\Projects>neuro ps
ID                                       NAME   STATUS   WHEN           IMAGE            OWNER
job-7c384fe1-af22-4514-9b06-e9445df46143 job390 pending  11 seconds ago pytorch:latest  <you>   
job-0b8dc223-8d18-498b-a511-a1d643262e95 job363 pending  5 seconds ago  ubuntu:latest   <you>   
```

Прежде чем открыть доступ к заданию, Вы должны знать ID данного задания. После определения ID, чтобы предоставить доступ, необходимо использовать команду `neuro share job`.

**Пример команды для предоставления доступа:**

```text
(base) C:\Projects>neuro share job:job363 mrsmariyadavydova manage
```

Данная команда предоставляет доступ к заданию job363 для пользователя mrsmariyadavydova и обеспечивает доступ с уровнем manage. Вы можете предоставить партнеру доступ для чтения, записи или управления задания. Теперь Ваш коллега может использовать команду `neuro ps` для просмотра данного задания в своем списке доступных заданий.

### Где можно найти логи задания?

Полные логи задания можно посмотреть, используя команду `neuro job logs` \[имя задания или id\]. Команда отображает логи для указанного задания.

Логи также отображается, если Вы не передали опцию `--detach` при запуске задания. Опция `--detach` гарантирует, что задание не подключено к логгированию и не ожидает кода exit.

**Пример команды:** 

```text
(base) C:\Projects>neuro job logs filebrowser-49249
2021/01/12 19:36:52 Using config file: /.filebrowser.json
2021/01/12 19:36:52 Listening on [::]:80
2021/01/12 19:36:55 /: 404 10.60.0.10 <nil>
2021/01/12 21:11:23 Caught signal terminated: shutting down.
2021/01/12 21:11:23 accept tcp [::]:80: use of closed network connection
```

### Возможно ли управлять заданиями из веб-интерфейса?

Платформа предоставляет интуитивно понятный интерфейс, который позволяет Вам управлять заданиями. На странице с заданиями веб-интерфейса платформы перечислены все задания.

![&#x421;&#x442;&#x440;&#x430;&#x43D;&#x438;&#x446;&#x430; &quot;&#x417;&#x430;&#x434;&#x430;&#x43D;&#x438;&#x44F;&quot;](../.gitbook/assets/image%20%2851%29.png)

Вы можете просмотреть веб-интерфейс задания, нажав **Ссылка** в выпадающем списке, доступном справа от задания.

![&#x424;&#x443;&#x43D;&#x43A;&#x446;&#x438;&#x44F; &quot;&#x421;&#x441;&#x44B;&#x43B;&#x43A;&#x430;&quot;](../.gitbook/assets/image%20%2835%29.png)

Чтобы просмотреть журнал логов и другие сведения о задании, нажмите на его ID.

![&#x414;&#x435;&#x442;&#x430;&#x43B;&#x438; &#x437;&#x430;&#x434;&#x430;&#x43D;&#x438;&#x44F;](../.gitbook/assets/image%20%2896%29.png)

Чтобы просмотреть только работающие задания, установите флажок **Только выполняющиеся**.

![&#x418;&#x441;&#x43F;&#x43E;&#x43B;&#x44C;&#x437;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x435; &#x444;&#x43B;&#x430;&#x436;&#x43A;&#x430; &quot;&#x422;&#x43E;&#x43B;&#x44C;&#x43A;&#x43E; &#x432;&#x44B;&#x43F;&#x43E;&#x43B;&#x43D;&#x44F;&#x44E;&#x449;&#x438;&#x435;&#x441;&#x44F;&quot;](../.gitbook/assets/image%20%28104%29.png)

Вы можете искать задания по имени, ID или тегам, используя поле **Поиск**. 

![&#x41F;&#x43E;&#x438;&#x441;&#x43A; &#x437;&#x430;&#x434;&#x430;&#x43D;&#x438;&#x439;](../.gitbook/assets/image%20%2828%29.png)

Пользовательский интерфейс также позволяет завершить или перезапустить задание, нажав **ОСТАНОВИТЬ** или **ПЕРЕЗАПУСТИТЬ** в выпадающем списке справа от задания.

![&#x41E;&#x43F;&#x446;&#x438;&#x438; &#x41E;&#x421;&#x422;&#x410;&#x41D;&#x41E;&#x412;&#x418;&#x422;&#x42C; &#x438; &#x41F;&#x415;&#x420;&#x415;&#x417;&#x410;&#x41F;&#x423;&#x421;&#x422;&#x418;&#x422;&#x42C;](../.gitbook/assets/image%20%28105%29.png)

