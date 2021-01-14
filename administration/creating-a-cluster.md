# Создание кластеров

### Введение

Neu.ro позволяет Вам создать кластер у любого из трёх крупнейших провайдеров облачных служб - [Amazon Web Services](https://aws.amazon.com/), [Google Cloud Platform](https://cloud.google.com/) и [Azure](https://azure.microsoft.com/en-in/). После регистрации аккаунта у одного из этих провайдеров, Вам нужно будет предоставить информацию о своём сервисном аккаунте и желаемой конфигурации кластера нашей команде. После этого мы создадим для Вас кластер и установим Neu.ro на вашем облачном хранилище.

Чтобы установить кластер Neu.ro, Вам нужно:

1. Настроить обачную среду:
   * GCP: [Создайте проект](https://cloud.google.com/appengine/docs/standard/nodejs/building-app/creating-project) и [сервисный аккаунт](https://cloud.google.com/iam/docs/creating-managing-service-accounts#creating).
   * AWS: [Создайте VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html#getting-started-create-vpc) и [сервисный аккаунт](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html).
   * Azure: [Создайте группу ресурсов](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups) и [сервисный аккаунт](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal).
2. Приготовьте YAML-файл конфигурации кластера.
3. Отправьте этот YAML-файл нам. Как только мы получим этот файл, мы настроим и запустим кластер \(обычно это занимает не больше 1 дня\). Вы можете начать добавлять пользователей в кластер, даже когда он находится в процессе настройки.

Кроме вышеупомянутого процесса, есть другие методы установки:

* Установите Neu.ro на существующий кластер AWS, GCP или Azure. Это потребует настройки пула узлов. 
* Установите Neu.ro на существующий кластер, предоставленный другими провайдерами облачных сервисов.
* Установите Neu.ro на своей собственной инфраструктуре \(так называемая установка bare metal\).

Для дальнейшей помощи с этими способами установки, [свяжитесь с нашей командой](mailto:team@neu.ro).

### YAML-файл конфигурации кластера 

Вам нужно создать проект/VPC/группу ресурсов и сервисный аккаунт со всеми требуемыми разрешениями прежде чем начинать готовить YAML-файл конфигурации кластера. YAML-файл требуется команде Neu.ro для настройки и запуска кластера. Команда `neuro admin generate-cluster-config` используется для генерации YAML-файла. Это интерактивный инструмент, генерирующий файл конфигурации, настройка узлов по умолчанию в котором устанавливается на основе ваших ответов.

Эта команда спрашивает следующую информацию для того, чтобы сгенерировать YAML-файл:

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>&#x417;&#x430;&#x43F;&#x440;&#x43E;&#x441;</b>
      </th>
      <th style="text-align:left"><b>&#x41E;&#x43F;&#x438;&#x441;&#x430;&#x43D;&#x438;&#x435;</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Cluster Type</td>
      <td style="text-align:left">&#x423;&#x43A;&#x430;&#x436;&#x438;&#x442;&#x435; &#x43F;&#x440;&#x43E;&#x432;&#x430;&#x439;&#x434;&#x435;&#x440;&#x430;
        &#x43E;&#x431;&#x43B;&#x430;&#x447;&#x43D;&#x44B;&#x445; &#x441;&#x435;&#x440;&#x432;&#x438;&#x441;&#x43E;&#x432;
        - AWS, GCP, Azure</td>
    </tr>
    <tr>
      <td style="text-align:left">GCP Project Name &#x438;&#x43B;&#x438; AWS VPC ID &#x438;&#x43B;&#x438;
        Azure subscription ID</td>
      <td style="text-align:left">
        <ul>
          <li>&#x414;&#x43B;&#x44F; GCP, &#x443;&#x43A;&#x430;&#x436;&#x438;&#x442;&#x435;
            &#x43D;&#x430;&#x437;&#x432;&#x430;&#x43D;&#x438;&#x435; &#x43F;&#x440;&#x43E;&#x435;&#x43A;&#x442;&#x430;.</li>
          <li>&#x414;&#x43B;&#x44F; AWS, &#x443;&#x43A;&#x430;&#x436;&#x438;&#x442;&#x435;
            ID VPC, &#x43A;&#x43E;&#x442;&#x43E;&#x440;&#x44B;&#x439; &#x432;&#x44B;
            &#x441;&#x43E;&#x437;&#x434;&#x430;&#x43B;&#x438; &#x43D;&#x430; &#x43F;&#x440;&#x435;&#x434;&#x44B;&#x434;&#x443;&#x449;&#x435;&#x43C;
            &#x448;&#x430;&#x433;&#x435;.</li>
          <li>&#x414;&#x43B;&#x44F; Azure, &#x443;&#x43A;&#x430;&#x436;&#x438;&#x442;&#x435;
            ID &#x43F;&#x43E;&#x434;&#x43F;&#x438;&#x441;&#x43A;&#x438; Azure, &#x43A;&#x43E;&#x442;&#x43E;&#x440;&#x443;&#x44E;
            &#x432;&#x44B; &#x441;&#x43E;&#x437;&#x434;&#x430;&#x43B;&#x438; &#x43D;&#x430;
            &#x43F;&#x440;&#x435;&#x434;&#x44B;&#x434;&#x443;&#x449;&#x435;&#x43C;
            &#x448;&#x430;&#x433;&#x435;.</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Service Account Key File (.json)</p>
        <p>&#x438;&#x43B;&#x438;</p>
        <p>AWS Profile Name</p>
        <p>&#x438;&#x43B;&#x438; Azure information</p>
      </td>
      <td style="text-align:left">
        <ul>
          <li>&#x414;&#x43B;&#x44F; GCP, &#x443;&#x43A;&#x430;&#x436;&#x438;&#x442;&#x435;
            &#x43F;&#x443;&#x442;&#x44C; &#x43A; &#x444;&#x430;&#x439;&#x43B;&#x443;-&#x43A;&#x43B;&#x44E;&#x447;&#x443;
            &#x43E;&#x442; &#x441;&#x435;&#x440;&#x432;&#x438;&#x441;&#x43D;&#x43E;&#x433;&#x43E;
            &#x430;&#x43A;&#x43A;&#x430;&#x443;&#x43D;&#x442;&#x430;. &#x414;&#x43B;&#x44F;
            &#x434;&#x43E;&#x43F;&#x43E;&#x43B;&#x43D;&#x438;&#x442;&#x435;&#x43B;&#x44C;&#x43D;&#x44B;&#x445;
            &#x441;&#x432;&#x435;&#x434;&#x435;&#x43D;&#x438;&#x439; &#x43E; &#x441;&#x435;&#x440;&#x432;&#x438;&#x441;&#x43D;&#x44B;&#x445;
            &#x430;&#x43A;&#x43A;&#x430;&#x443;&#x43D;&#x442;&#x430;&#x445;, &#x441;&#x43C;.
            <a
            href="https://cloud.google.com/iam/docs/understanding-service-accounts">Understanding service accounts</a>.</li>
          <li>&#x414;&#x43B;&#x44F; AWS, &#x443;&#x43A;&#x430;&#x436;&#x438;&#x442;&#x435;
            &#x438;&#x43C;&#x44F; &#x43F;&#x440;&#x43E;&#x444;&#x438;&#x43B;&#x44F;
            AWS. &#x414;&#x43B;&#x44F; &#x434;&#x43E;&#x43F;&#x43E;&#x43B;&#x43D;&#x438;&#x442;&#x435;&#x43B;&#x44C;&#x43D;&#x44B;&#x445;
            &#x441;&#x432;&#x435;&#x434;&#x435;&#x43D;&#x438;&#x439;, &#x441;&#x43C;.
            <a
            href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html">Managing Access Keys</a>.</li>
          <li>&#x414;&#x43B;&#x44F; Azure, &#x443;&#x43A;&#x430;&#x436;&#x438;&#x442;&#x435;
            &#x442;&#x430;&#x43A;&#x443;&#x44E; &#x438;&#x43D;&#x444;&#x43E;&#x440;&#x43C;&#x430;&#x446;&#x438;&#x44E;
            &#x434;&#x43E;&#x441;&#x442;&#x443;&#x43F;&#x430;:
            <ul>
              <li><a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret">ID &#x43A;&#x43B;&#x438;&#x435;&#x43D;&#x442;&#x430; Azure</a>
              </li>
              <li><a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret">ID &#x442;&#x435;&#x43D;&#x430;&#x43D;&#x442;&#x430; Azure</a>
              </li>
              <li><a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret">&#x421;&#x435;&#x43A;&#x440;&#x435;&#x442; &#x43A;&#x43B;&#x438;&#x435;&#x43D;&#x442;&#x430; Azure</a>
              </li>
              <li><a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application">&#x413;&#x440;&#x443;&#x43F;&#x43F;&#x443; &#x440;&#x435;&#x441;&#x443;&#x440;&#x441;&#x43E;&#x432; Azure</a>
              </li>
              <li>&#x420;&#x430;&#x437;&#x43C;&#x435;&#x440; &#x445;&#x440;&#x430;&#x43D;&#x438;&#x43B;&#x438;&#x449;&#x430;
                Azure Files (&#x432; &#x413;&#x411;)</li>
            </ul>
          </li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Ниже приведен пример команды для GCP:

```text
> neuro admin generate-cluster-config
Select cluster type (aws, gcp, azure): gcp
GCP project name: My Project
Service Account Key File (.json): GCP_User.json
Cluster config cluster.yml is generated.
```

Эта команда создаёт файл `cluster.yml`, включающий информацию, необходимую команде Neu.ro для настройки вашего кластера. Ниже пример файла `cluster.yml`, сгенерированного для GCP:

```text
type: gcp
location_type: multi_zonal
region: us-central1
zones:
- us-central1-a
- us-central1-c
project: My Project
credentials:
 auth_provider_x509_cert_url: https://www.googleapis.com/oauth2/v1/certs
 auth_uri: https://accounts.google.com/o/oauth2/auth
 client_email: johndoe@intricate-web-236410.iam.gserviceaccount.com
 client_id: '105087309181394151560'
 client_x509_cert_url: https://www.googleapis.com/robot/v1/metadata/x509/johndoe%40intricate-web-236410.iam.gserviceaccount.com
 private_key: ...
 private_key_id: ...
 project_id: intricate-web-236410
 token_uri: https://oauth2.googleapis.com/token
 type: service_account
node_pools:
- id: n1_highmem_8
 min_size: 1
 max_size: 4
- id: n1_highmem_8_1x_nvidia_tesla_k80
 min_size: 1
 max_size: 4
- id: n1_highmem_8_1x_nvidia_tesla_v100
 min_size: 0
 max_size: 1
storage:
 id: standard
 capacity_tb: 4
```

Этот файл содержит установку пула узлов по умолчанию, которая используется как стартовая точка кластера:

* 1 вытесняемый узел с K80,
* 1 вытесняемый узел с V100,
* 4 вытесняемых узла отличного от GPU типа,
* 4 ТБ стандартного дискового пространства.

Этот файл создаст кластер со следующими пресетами:

* cpu-small,
* cpu-large,
* gpu-small \(узел с K80\),
* gpu-large \(узел с V100\).

Чтобы получить информацию о доступных опциях для каждого из провайдеров облачных сервисов, запустите:

`neuro admin show-cluster-options` .

Вы также можете дополнять и обновлять файл `cluster.yml` в соответствии с вашими требованиями, прежде чем отсылать его нам. Если у вас возникнут проблемы м обновлением этого файла, [свяжитесь с нами](mailto:team@neu.ro). Как только вы закончите дополнять конфигурационный файл, отошлите его команде Neu.ro для последующей настройки кластера, используя команду:

`neuro admin add-cluster <path/to/config>`

Как только мы получим файл YAML, мы настроим и запустим кластер \(обычно это занимает не больше дня\). После запуска предыдущей команды вы становитесь администратором кластера. Вы также можете начать добавлять новых пользователей сразу после этого.

