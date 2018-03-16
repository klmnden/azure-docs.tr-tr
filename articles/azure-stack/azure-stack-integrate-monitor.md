---
title: "Dış izleme çözümü Azure yığın ile tümleştirme | Microsoft Docs"
description: "Dış bir izleme çözümü, veri merkezinizdeki Azure yığın tümleştirileceğini öğrenin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 856738a7-1510-442a-88a8-d316c67c757c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 3435ada40afb9f1c6e57be64d1b9086d0cdaefd9
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2018
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Dış izleme çözümü Azure yığın ile tümleştirme

Dış Azure yığın altyapı izleme için Azure yığın yazılım, fiziksel bilgisayarları ve fiziksel ağ anahtarları izlemeniz gerekir. Bu alanların her biri, sistem durumu ve uyarı bilgilerini almak için bir yöntem sunar:

- Azure yığın yazılım sistem durumu ve uyarıları almak için REST tabanlı bir API sunar. (Depolama alanları doğrudan gibi yazılım tanımlı teknolojileri kullanımı ile depolama durumunu ve Uyarıları yazılım izleme bir parçasıdır.).
- Fiziksel bilgisayar sistem durumunu ve uyarı bilgilerini temel kart yönetim denetçileri (BMC) aracılığıyla kullanılabilir hale getirebilirsiniz.
- Fiziksel ağ aygıtlarını sistem durumu ve uyarı bilgileri SNMP protokolü aracılığıyla kullanılabilir hale getirebilirsiniz.

Her Azure yığın çözüm donanım yaşam döngüsü ana bilgisayar ile birlikte gelir. Bu ana bilgisayar özgün donanım üreticisi (OEM) donanım satıcısının izleme yazılım fiziksel sunucuları ve ağ aygıtları için çalışır. İsterseniz, bu çözümleri izleme atlayabilir ve doğrudan, veri merkezinizde mevcut izleme çözümleriyle tümleştirin.

> [!IMPORTANT]
> Kullandığınız dış izleme çözümü aracısız olması gerekir. Azure yığın bileşenleri içindeki üçüncü taraf aracılar yükleyemezsiniz.

Aşağıdaki diyagramda bir Azure tümleşik yığını sistem, donanım yaşam döngüsü konak, dış bir izleme çözümü ve bir dış raporlama/veri toplama sistemi arasındaki trafik akışını gösterir.

![İzleme ve raporlama çözümü yığını, Azure arasındaki trafiği gösteren diyagram.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

Bu makalede, Azure yığın System Center Operations Manager ve Nagios gibi dış izleme çözümü ile tümleştirmek açıklanmaktadır. Ayrıca, uyarılarla programlı olarak REST API çağrıları aracılığıyla veya PowerShell kullanarak nasıl çalışılacağı içerir.

## <a name="integrate-with-operations-manager"></a>Operations Manager ile tümleştirme

Operations Manager dış Azure yığınını izlemek için kullanabilirsiniz. Microsoft Azure yığın için System Center Yönetim Paketi tek bir Operations Manager örneği birden çok Azure yığın dağıtım izlemenize olanak sağlar. Yönetim Paketi, Azure yığın ile iletişim kurmak için sistem durumu kaynak sağlayıcısı ve güncelleştirme kaynak sağlayıcısı REST API'lerini kullanır. Donanım yaşam döngüsü konakta çalışan yazılım izleme OEM geçiş yapmayı planlıyorsanız, fiziksel sunucuları izlemek için satıcı yönetim paketleri yükleyebilirsiniz. Operations Manager ağ aygıtı bulma, ağ anahtarları izlemek için de kullanabilirsiniz.

Azure yığını için yönetim paketi aşağıdaki yetenekleri sağlar:

- Birden çok Azure yığın dağıtımlarını yönetebilir
- Azure Active Directory (Azure AD) ve Active Directory Federasyon Hizmetleri (AD FS) için destek
- Alabilir ve Uyarıları Kapat
- Bir sistem durumu ve kapasite Panosu
- Düzeltme eki ve güncelleştirme (P & U) olduğunda ediyor için otomatik Bakım modu algılama içerir
- Dağıtım ve bölge için zorla güncelleştirme görevleri içerir
- Bir bölgeye özel bilgiler ekleyebilirsiniz.
- Destekler bildirim ve Raporlama

System Center Yönetim Paketi için Microsoft Azure yığın ve ilişkili indirebilirsiniz [Kullanıcı Kılavuzu](https://www.microsoft.com/en-us/download/details.aspx?id=55184), ya da Operations Manager'dan doğrudan.

Bilet çözümü için Operations Manager System Center Service Manager ile tümleştirebilirsiniz. Tümleşik ürün Bağlayıcısı'nı bir hizmet isteği Hizmet Yöneticisi'nde çözdükten sonra Azure yığını ve Operations Manager bir uyarıyı kapatmak izin veren çift yönlü iletişim sağlar.

Aşağıdaki diyagram, mevcut bir System Center dağıtım ile Azure yığınının tümleştirme gösterir. Hizmet Yöneticisi daha fazla ile System Center Orchestrator veya Service Management Automation (SMA) Azure yığınında işlemlerini çalıştırmak için otomatik hale getirebilirsiniz.

![OM, Service Manager ve SMA ile tümleştirme gösteren diyagram.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Nagios ile tümleştirme

Eklentisi izleme Nagios izin veren ücretsiz yazılım lisansı altında – MIT (Massachusetts Teknoloji Enstitüsü'nün) kullanılabilir olan iş ortağı Cloudbase çözümleriyle birlikte geliştirilmiştir.

Eklenti Python içinde yazılmış ve sistem durumu kaynak sağlayıcısı REST API kullanır. Almak ve Azure yığın içindeki uyarıları Kapat temel işlevleri sunar. System Center Yönetim Paketi gibi birden çok Azure yığın dağıtımları eklemek ve bildirimleri göndermek için sağlar.

Eklenti Nagios Enterprise ve Nagios çekirdek ile çalışır. Karşıdan yükleyebileceğiniz [burada](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). İndirme site ayrıca yükleme ve yapılandırma ayrıntılarını içerir.

### <a name="plugin-parameters"></a>Eklentisi parametreleri

Eklenti dosyası "Azurestack_plugin.py" aşağıdaki parametrelerle yapılandırın:

| Parametre | Açıklama | Örnek |
|---------|---------|---------|
| *arm_endpoint* | Azure Kaynak Yöneticisi (Yönetici) uç noktası |https://adminmanagement.local.azurestack.external |
| *api_endpoint* | Azure Kaynak Yöneticisi (Yönetici) uç noktası  | https://adminmanagement.local.azurestack.external |
| *Tenant_id* | Yönetici abonelik kimliği | Yönetici portalı veya PowerShell aracılığıyla Al |
| *User_name* | İşleç abonelik kullanıcı adı | operator@myazuredirectory.onmicrosoft.com |
| *User_password* | İşleç abonelik parola | İstanbul |
| *Client_id* | İstemci | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417* |
| *region* |  Azure yığın bölge adı | yerel |
|  |  |

* PowerShell sağlanan Evrensel GUID'dir. Her dağıtım için kullanabilirsiniz.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>İzleyici sistem durumu ve Uyarılar için PowerShell kullanın

Operations Manager, Nagios veya Nagios tabanlı bir çözümü kullanmıyorsanız, Azure yığın ile tümleştirmek için çözümlerini izleme geniş etkinleştirmek için PowerShell kullanabilirsiniz.
 
1. PowerShell kullanmak için bilgisayarınızda yüklü olduğundan emin olun [PowerShell yüklenmiş ve yapılandırılmış](azure-stack-powershell-configure-quickstart.md) Azure yığın işleci ortamı için. PowerShell Kaynak Yöneticisi (Yönetici) uç noktası ulaşabilir yerel bir bilgisayara yükleme (https://adminmanagement. [ Bölge]. [External_FQDN]).

2. Bir Azure yığın işleç olarak Azure yığın ortama bağlanmak için aşağıdaki komutları çalıştırın:

   ```PowerShell
   Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Login-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```
3. Değiştirme yüklediğiniz dizine [Azure yığın Araçları](https://github.com/Azure/AzureStack-Tools) PowerShell yükleme, örneğin, c:\azurestack-tools-master bir parçası olarak. Ardından, altyapı dizinine değiştirin ve altyapı modülü içeri aktarmak için aşağıdaki komutu çalıştırın:

   ```PowerShell
   Import-Module .\AzureStack.Infra.psm1
    ```
4. Uyarılarla çalışma için aşağıdaki örneklerde olduğu gibi komutları kullanın:
   ```PowerShell
   #Retrieve all alerts
   Get-AzsAlert -location [Region]

   #Filter for active alerts
   $Active=Get-AzsAlert -location [Region] | Where {$_.State -eq "active"}
   $Active

   #Close alert
   Close-AzsAlert -location [Region] -AlertID "ID"

   #Retrieve resource provider health
   Get-AzsResourceProviderHealths -location [Region]

   #Retrieve infrastructure role instance health
   Get-AzsInfrastructureRoleHealths -location [Region]
   ```

## <a name="use-the-rest-api-to-monitor-health-and-alerts"></a>İzleyici durum ve Uyarılar için REST API kullanın

REST API çağrıları, uyarıları alma, uyarıları kapatın ve kaynak sağlayıcıları durumunu almak için kullanabilirsiniz.

### <a name="get-alert"></a>Uyarı alma

**İstek**

İstek varsayılan sağlayıcı abonelik için tüm etkin ve kapalı uyarıları alır. İstek gövdesinde yok.


|Yöntem  |İstek URI'si  |
|---------|---------|
|AL     |   https://{armendpoint}/subscriptions/{subId}/resourceGroups/system.{RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/Alerts?api-version=2016-05-01"      |
|     |         |

**Bağımsız değişkenler**

|Bağımsız değişken  |Açıklama  |
|---------|---------|
|armendpoint     |  Azure Resource Manager uç noktasını, Azure yığın ortamınızda biçimi https://adminmanagement. {RegionName}. {Dış FQDN}. Örneğin, dış FQDN ise *azurestack.external* ve bölge adı *yerel*, kaynak yöneticisi uç noktası ise https://adminmanagement.local.azurestack.external.       |
|subid     |   Çağrıyı yapan kullanıcının abonelik kimliği. Sorgu için bu API, yalnızca varsayılan sağlayıcı abonelik izni olan bir kullanıcı ile kullanabilirsiniz.      |
|RegionName     |    Azure yığın dağıtımına bölge adı.     |
|API sürümü     |  Bu isteği yapmak için kullanılan protokol sürümü. 2016-05-01 kullanmanız gerekir.      |
|     |         |

**Yanıt**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/Alerts?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Active",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},

…
```

**Yanıt Ayrıntıları**


|  Bağımsız değişken  |Açıklama  |
|---------|---------|
|*id*     |      Uyarı benzersiz kimliği.   |
|*Adı*     |     Uyarı iç adı.   |
|*Türü*     |     Kaynak tanımı.    |
|*konum*     |       Bölge adı.     |
|*etiketler*     |   Kaynak etiketleri.     |
|*closedtimestamp*    |  UTC saati ne zaman uyarı kapatıldı.    |
|*createdtimestamp*     |     Uyarının oluşturulduğu UTC saati.   |
|*Açıklama*     |    Uyarı açıklaması.     |
|*faultid*     | Etkilenen bileşeni.        |
|*alertid*     |  Uyarı benzersiz kimliği.       |
|*faulttypeid*     |  Hatalı bileşen benzersiz türü.       |
|*lastupdatedtimestamp*     |   Uyarı bilgileri en son güncelleştirildiği UTC saati.    |
|*healthstate*     | Genel sistem durumu.        |
|*Adı*     |   Belirli uyarı adı.      |
|*fabricname*     |    Hatalı bileşen kayıtlı doku adı.   |
|*Açıklama*     |  Kayıtlı fabric bileşeni açıklaması.   |
|*servicetype*     |   Kayıtlı doku hizmetin türü.   |
|*remediation*     |   Düzeltme adımları önerilir.    |
|*Türü*     |   Uyarı türü.    |
|*resourceRegistrationid*    |     Etkilenen kayıtlı kaynak kimliği.    |
|*resourceProviderRegistrationID*   |    Etkilenen bileşeninin kayıtlı kaynak sağlayıcısı kimliği.  |
|*serviceregistrationid*     |    Kayıtlı hizmet kimliği.   |
|*Önem derecesi*     |     Uyarı önem derecesi.  |
|*Durumu*     |    Uyarı durumu.   |
|*title*     |    Uyarı başlığı.   |
|*impactedresourceid*     |     Etkilenen kaynak kimliği.    |
|*ImpactedresourceDisplayName*     |     Etkilenen kaynağın adı.  |
|*closedByUserAlias*     |   Uyarı kapatan kullanıcı.      |

### <a name="close-alert"></a>Uyarıyı kapat

**İstek**

İstek kendi benzersiz kimliğe göre bir uyarıyı kapatır

|Yöntem    |İstek URI'si  |
|---------|---------|
|PUT     |   https://{armendpoint}/subscriptions/{subId}/resourceGroups/system.{RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/Alerts/alertid?api-version=2016-05-01"    |

**Bağımsız değişkenler**


|Bağımsız değişken  |Açıklama  |
|---------|---------|
|*armendpoint*     |   Kaynak Yöneticisi uç noktası biçimi https://adminmanagement içinde Azure yığın ortamınızın. {RegionName}. {Dış FQDN}. Örneğin, dış FQDN ise *azurestack.external* ve bölge adı *yerel*, kaynak yöneticisi uç noktası ise https://adminmanagement.local.azurestack.external.      |
|*subid*     |    Çağrıyı yapan kullanıcının abonelik kimliği. Sorgu için bu API, yalnızca varsayılan sağlayıcı abonelik izni olan bir kullanıcı ile kullanabilirsiniz.     |
|*RegionName*     |   Azure yığın dağıtımına bölge adı.      |
|*api-version*     |    Bu isteği yapmak için kullanılan protokol sürümü. 2016-05-01 kullanmanız gerekir.     |
|*alertid*     |    Uyarı benzersiz kimliği.     |

**Gövde**

```json

{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"2017-08-10T20:18:58.1584868Z",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Closed",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},
```
**Yanıt**

```http
PUT https://adminmanagement.local.azurestack.external//subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Closed",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},
```

**Yanıt Ayrıntıları**


|  Bağımsız değişken  |Açıklama  |
|---------|---------|
|*id*     |      Uyarı benzersiz kimliği.   |
|*Adı*     |     Uyarı iç adı.   |
|*Türü*     |     Kaynak tanımı.    |
|*konum*     |       Bölge adı.     |
|*etiketler*     |   Kaynak etiketleri.     |
|*closedtimestamp*    |  UTC saati ne zaman uyarı kapatıldı.    |
|*createdtimestamp*     |     Uyarının oluşturulduğu UTC saati.   |
|*Açıklama*     |    Uyarı açıklaması.     |
|*faultid*     | Etkilenen bileşeni.        |
|*alertid*     |  Uyarı benzersiz kimliği.       |
|*faulttypeid*     |  Hatalı bileşen benzersiz türü.       |
|*lastupdatedtimestamp*     |   Uyarı bilgileri en son güncelleştirildiği UTC saati.    |
|*healthstate*     | Genel sistem durumu.        |
|*Adı*     |   Belirli uyarı adı.      |
|*fabricname*     |    Hatalı bileşen kayıtlı doku adı.   |
|*Açıklama*     |  Kayıtlı fabric bileşeni açıklaması.   |
|*servicetype*     |   Kayıtlı doku hizmetin türü.   |
|*remediation*     |   Düzeltme adımları önerilir.    |
|*Türü*     |   Uyarı türü.    |
|*resourceRegistrationid*    |     Etkilenen kayıtlı kaynak kimliği.    |
|*resourceProviderRegistrationID*   |    Etkilenen bileşeninin kayıtlı kaynak sağlayıcısı kimliği.  |
|*serviceregistrationid*     |    Kayıtlı hizmet kimliği.   |
|*Önem derecesi*     |     Uyarı önem derecesi.  |
|*Durumu*     |    Uyarı durumu.   |
|*title*     |    Uyarı başlığı.   |
|*impactedresourceid*     |     Etkilenen kaynak kimliği.    |
|*ImpactedresourceDisplayName*     |     Etkilenen kaynağın adı.  |
|*closedByUserAlias*     |   Uyarı kapatan kullanıcı.      |

### <a name="get-resource-provider-health"></a>Kaynak sağlayıcı durumu Al

**İstek**

İstek, tüm kayıtlı kaynak sağlayıcıları için sistem durumunu alır.


|Yöntem  |İstek URI'si  |
|---------|---------|
|AL    |   https://{armendpoint}/subscriptions/{subId}/resourceGroups/system.{RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/serviceHealths?api-version=2016-05-01"   |


**Bağımsız değişkenler**


|Bağımsız Değişkenler  |Açıklama  |
|---------|---------|
|*armendpoint*     |    Kaynak Yöneticisi uç noktasını, Azure yığın ortamınızda biçimi https://adminmanagement. {RegionName}. {Dış FQDN}. Örneğin, dış FQDN azurestack.external ve bölge adını yerel ise, kaynak yöneticisi uç noktası ise https://adminmanagement.local.azurestack.external.     |
|*subid*     |     Çağrıyı yapan kullanıcının abonelik kimliği. Sorgu için bu API, yalnızca varsayılan sağlayıcı abonelik izni olan bir kullanıcı ile kullanabilirsiniz.    |
|*RegionName*     |     Azure yığın dağıtımına bölge adı.    |
|*api-version*     |   Bu isteği yapmak için kullanılan protokol sürümü. 2016-05-01 kullanmanız gerekir.      |


**Yanıt**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths?api-version=2016-05-01
```

```json
{
"value":[
{
"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/03ccf38f-f6b1-4540-9dc8-ec7b6389ecca",
"name":"03ccf38f-f6b1-4540-9dc8ec7b6389ecca",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/serviceHealths",
"location":"local",
"tags":{},
"properties":{
"registrationId":"03ccf38f-f6b1-4540-9dc8-ec7b6389ecca",
"displayName":"Key Vault",
"namespace":"Microsoft.KeyVault.Admin",
"routePrefix":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.KeyVault.Admin/locations/local",
"serviceLocation":"local",
"infraURI":"/subscriptions/4aa97de3-6b83-4582-86e1-65a5e4d1295b/resourceGroups/system.local/providers/Microsoft.KeyVault.Admin/locations/local/infraRoles/Key Vault",
"alertSummary":{"criticalAlertCount":0,"warningAlertCount":0},
"healthState":"Healthy"
}
}

…
```
**Yanıt Ayrıntıları**


|Bağımsız değişken  |Açıklama  |
|---------|---------|
|*Kimlik*     |   Uyarı benzersiz kimliği.      |
|*Adı*     |  Uyarı iç adı.       |
|*Türü*     |  Kaynak tanımı.       |
|*konum*     |  Bölge adı.       |
|*etiketler*     |     Kaynak etiketleri.    |
|*registrationId*     |   Benzersiz kayıt kaynak sağlayıcısı için.      |
|*displayName*     |Kaynak sağlayıcının görünen adı.        |
|*Namespace*     |   API ad alanı kaynak sağlayıcısını uygular.       |
|*routePrefix*     |    Kaynak sağlayıcısı ile etkileşim kurmak için URI.     |
|*serviceLocation*     |   Bu kaynak sağlayıcısı kayıtlı bölgesi.      |
|*infraURI*     |   Bir altyapı rolü olarak listelendiğini kaynak sağlayıcısı URI'si.      |
|*alertSummary*     |   Kaynak sağlayıcı ile ilişkili kritik ve uyarı uyarı özeti.      |
|*healthState*     |    Kaynak sağlayıcısı sistem durumu.     |


### <a name="get-resource-health"></a>Kaynak durumu Al

İstek belirli kaynak sağlayıcısı için sistem durumunu alır.

**İstek**

|Yöntem  |İstek URI'si  |
|---------|---------|
|AL     |     https://{armendpoint}/subscriptions/{subId}/resourceGroups/system.{RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/serviceHealths/{RegistrationID}/resourceHealths?api-version=2016-05-01"    |

**Bağımsız değişkenler**

|Bağımsız Değişkenler  |Açıklama  |
|---------|---------|
|*armendpoint*     |    Kaynak Yöneticisi uç noktasını, Azure yığın ortamınızda biçimi https://adminmanagement. {RegionName}. {Dış FQDN}. Örneğin, dış FQDN azurestack.external ve bölge adını yerel ise, kaynak yöneticisi uç noktası ise https://adminmanagement.local.azurestack.external.     |
|*subid*     |Çağrıyı yapan kullanıcının abonelik kimliği. Sorgu için bu API, yalnızca varsayılan sağlayıcı abonelik izni olan bir kullanıcı ile kullanabilirsiniz.         |
|*RegionName*     |  Azure yığın dağıtımına bölge adı.       |
|*api-version*     |  Bu isteği yapmak için kullanılan protokol sürümü. 2016-05-01 kullanmanız gerekir.       |
|*RegistrationID* |Belirli kaynak sağlayıcısı için kayıt kimliği. |

**Yanıt**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/03ccf38f-f6b1-4540-9dc8-ec7b6389ecca /resourceHealths?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":
[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/472aaaa6-3f63-43fa-a489-4fd9094e235f/resourceHealths/028c3916-ab86-4e7f-b5c2-0468e607915c",
"name":"028c3916-ab86-4e7f-b5c2-0468e607915c",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/serviceHealths/resourceHealths",
"location":"local",
"tags":{},
"properties":
{"registrationId":"028c3916-ab86-4e7f-b5c2 0468e607915c","namespace":"Microsoft.Fabric.Admin","routePrefix":"/subscriptions/4aa97de3-6b83-4582-86e1 65a5e4d1295b/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local",
"resourceType":"infraRoles",
"resourceName":"Privileged endpoint",
"usageMetrics":[],
"resourceLocation":"local",
"resourceURI":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/Privileged endpoint",
"rpRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"alertSummary":{"criticalAlertCount":0,"warningAlertCount":0},"healthState":"Unknown"
}
}
…
```

**Yanıt Ayrıntıları**

|Bağımsız değişken  |Açıklama  |
|---------|---------|
|*Kimlik*     |   Uyarı benzersiz kimliği.      |
|*Adı*     |  Uyarı iç adı.       |
|*Türü*     |  Kaynak tanımı.       |
|*konum*     |  Bölge adı.       |
|*etiketler*     |     Kaynak etiketleri.    |
|*registrationId*     |   Benzersiz kayıt kaynak sağlayıcısı için.      |
|*resourceType*     |Kaynak türü.        |
|*resourceName*     |   Kaynak adı.   |
|*usageMetrics*     |    Kaynak için kullanım ölçümü.     |
|*resourceLocation*     |   Bölge adını dağıtılan burada.      |
|*resourceURI*     |   Kaynak için URI.   |
|*alertSummary*     |   Kritik özetini ve Uyarıları, sistem durumu.     |

## <a name="learn-more"></a>Daha fazla bilgi edinin

Yerleşik sistem durumu izleme hakkında daha fazla bilgi için bkz: [sistem durumu ve Uyarıları Azure yığınında izleme](azure-stack-monitor-health.md).


## <a name="next-steps"></a>Sonraki adımlar

[Güvenlik tümleştirme](azure-stack-integrate-security.md)