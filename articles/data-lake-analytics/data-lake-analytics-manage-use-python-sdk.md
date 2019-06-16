---
title: Python kullanarak Azure Data Lake Analytics'i yönetme
description: Bu makale, Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işlerini yönetmek için Python kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: d4213a19-4d0f-49c9-871c-9cd6ed7cf731
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: 82007c780a0c9ff3bb2e1a50a4826499f9df9c9f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60811713"
---
# <a name="manage-azure-data-lake-analytics-using-python"></a>Python kullanarak Azure Data Lake Analytics'i yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Bu makalede, Python kullanarak Azure Data Lake Analytics hesaplarını, veri kaynakları, kullanıcılar ve işlerini yönetmek açıklar.

## <a name="supported-python-versions"></a>Desteklenen Python sürümleri

* Python 64-bit sürümünü kullanın.
* Python dağıtım bulunamadı, standart kullanabileceğiniz  **[Python.org indirir](https://www.python.org/downloads/)** . 
* Birçok geliştiricinin kullanmak uygun bulma  **[Anaconda Python dağıtım](https://www.anaconda.com/download/)** .  
* Bu makalede Python 3.6 sürümünü standart Python dağıtım kullanılarak yazılmıştır

## <a name="install-azure-python-sdk"></a>Azure Python SDK’yı yükleme

Aşağıdaki modüller yükleyin:

* **Azure-mgmt-resource** modülü, Active Directory gibi şeyler için diğer Azure modüllerini içerir.
* **Azure-datalake-store** modülü Azure Data Lake Store dosya sistemi işlemlerini içerir. 
* **Azure-mgmt-datalake-store** modülü Azure Data Lake Store hesap yönetim işlemlerini içerir.
* **Azure-mgmt-datalake-analytics** modülü Azure Data Lake Analytics işlemlerini içerir. 

İlk olarak, en son olduğundan emin olun `pip` aşağıdaki komutu çalıştırarak:

```
python -m pip install --upgrade pip
```

Bu belge kullanılarak yazılmıştır `pip version 9.0.1`.

Aşağıdaki `pip` modülleri komut satırından yüklemek için komutları:

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a>Yeni bir Python betiği oluşturma

Betiğe aşağıdaki kodu yapıştırın:

```python
## Use this only for Azure AD service-to-service authentication
#from azure.common.credentials import ServicePrincipalCredentials

## Use this only for Azure AD end-user authentication
#from azure.common.credentials import UserPassCredentials

## Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

## Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

## Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

## Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInformation

## Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

## Required for Azure Data Lake Analytics catalog management
from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

## Use these as needed for your application
import logging, getpass, pprint, uuid, time
```

Modülleri içeri doğrulamak için bu betiği çalıştırın.

## <a name="authentication"></a>Kimlik Doğrulaması

### <a name="interactive-user-authentication-with-a-pop-up"></a>Etkileşimli kullanıcı kimlik doğrulaması ile bir açılır pencere

Bu yöntem desteklenmiyor.

### <a name="interactive-user-authentication-with-a-device-code"></a>Bir cihaz kodu ile etkileşimli kullanıcı kimlik doğrulaması

```python
user = input('Enter the user to authenticate with that has permission to subscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a>Etkileşimli olmayan kimlik doğrulaması ile SPI ve gizlilik

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a>API ve bir sertifika ile etkileşimli olmayan kimlik doğrulaması

Bu yöntem desteklenmiyor.

## <a name="common-script-variables"></a>Ortak komut dosyası değişkenleri

Bu değişkenler örnekleri kullanılır.

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-the-clients"></a>İstemcileri oluşturma

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a>Azure Kaynak Grubu oluşturma

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Önce bir depolama hesabı oluşturun.

```python
adlsAcctResult = adlsAcctClient.account.create(
    rg,
    adls,
    DataLakeStoreAccount(
        location=location)
    )
).wait()
```
Daha sonra bu depoyu kullanır bir ADLA hesabı oluşturun.

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInformation(name=adls)]
    )
).wait()
```

## <a name="submit-a-job"></a>Bir iş gönderdiniz

```python
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

## <a name="wait-for-a-job-to-end"></a>Sonlandırmak işin tamamlanmasını bekleyin

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a>Liste işlem hatları ve tekrarlar
İşlerinizi bağlı ardışık düzen veya yineleme meta verilerine de sahip olup olmadığını bağlı olarak, işlem hatları ve tekrarlar listeleyebilirsiniz.

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a>İşlem ilkelerini yönetme

DataLakeAnalyticsAccountManagementClient nesnesi, bir Data Lake Analytics hesabı için işlem ilkeleri yönetmek için yöntemler sağlar.

### <a name="list-compute-policies"></a>Liste işlem ilkeleri

Aşağıdaki kod, bir Data Lake Analytics hesabı için işlem ilkelerinin bir listesini alır.

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a>Yeni bir işlem ilkesi oluşturma

Aşağıdaki kod kullanılabilir maksimum AUS değerini belirtilen kullanıcı için 50 ve 250 en düşük iş önceliği ayarını bir Data Lake Analytics hesabı için yeni bir işlem ilkesi oluşturur.

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a>Sonraki adımlar

- Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
- U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
- Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).

