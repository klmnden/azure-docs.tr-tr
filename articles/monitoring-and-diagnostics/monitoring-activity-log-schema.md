---
title: Azure etkinlik günlüğü olay şeması
description: Etkinlik günlüğüne yayılan veriler için olay şemasının anlama
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 4/12/2018
ms.author: dukek
ms.component: activitylog
ms.openlocfilehash: 123ae27310d70812918f3c81ac3b9a71959a6c2c
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917236"
---
# <a name="azure-activity-log-event-schema"></a>Azure etkinlik günlüğü olay şeması
**Azure etkinlik günlüğü** Azure'da gerçekleşen herhangi bir abonelik düzeyindeki olayların sağlayan günlüktür. Bu makalede veri kategorisini başına olay şeması. Portal, PowerShell, CLI veya karşı REST API aracılığıyla doğrudan veri okunuyorsa veri şeması bağlı olarak farklı [veri depolama veya günlük profilini kullanarak Event Hubs akış](./monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile). Aşağıdaki örnekler, portal, PowerShell, CLI ve REST API kullanıma sunulan teklifinizle şema gösterir. Bu özellikler için bir eşleme [Azure tanılama günlükleri şema](./monitoring-diagnostic-logs-schema.md) makalenin sonunda sağlanır.

## <a name="administrative"></a>Yönetim
Bu kategoride tüm kaydı oluşturma, güncelleştirme, silme ve eylem işlemlerine Resource Manager aracılığıyla gerçekleştirilir. Görmek Bu kategoride olay türlerini örnekleri arasında "sanal makine oluşturma" ve "bir kullanıcı ya da Resource Manager kullanarak uygulama tarafından gerçekleştirilen her eylemi modellenmiş bir işlemi belirli bir kaynak türü olarak ağ güvenlik grubunu sil". İşlem türü, yazma, silme veya eylem ise, hem Başlangıç hem de başarılı kayıtlar veya bu işlemin başarısız yönetim kategorisi kaydedilir. Yönetim kategorisi, bir abonelikte rol tabanlı erişim denetimi değişiklikleri de içerir.

### <a name="sample-event"></a>Örnek olay
```json
{
    "authorization": {
        "action": "Microsoft.Network/networkSecurityGroups/write",
        "scope": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG"
    },
    "caller": "rob@contoso.com",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.core.windows.net/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "_claim_names": "{\"groups\":\"src1\"}",
        "_claim_sources": "{\"src1\":{\"endpoint\":\"https://graph.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/users/f409edeb-4d29-44b5-9763-ee9348ad91bb/getMemberObjects\"}}",
        "http://schemas.microsoft.com/claims/authnclassreference": "1",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
        "appid": "355249ed-15d9-460d-8481-84026b065942",
        "appidacr": "2",
        "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "10845a4d-ffa4-4b61-a3b4-e57b9b31cdb5",
        "e_exp": "262800",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Robertson",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Rob",
        "ipaddr": "111.111.1.111",
        "name": "Rob Robertson",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "onprem_sid": "S-1-5-21-4837261184-168309720-1886587427-18514304",
        "puid": "18247BBD84827C6D",
        "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "rob@contoso.com",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "rob@contoso.com",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2018-01-29T20:42:31.3810679Z",
    "id": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG/events/d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d/ticks/636528553513810679",
    "level": "Informational",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Network/networkSecurityGroups/write",
        "localizedValue": "Microsoft.Network/networkSecurityGroups/write"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Network",
        "localizedValue": "Microsoft.Network"
    },
    "resourceType": {
        "value": "Microsoft.Network/networkSecurityGroups",
        "localizedValue": "Microsoft.Network/networkSecurityGroups"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-01-29T20:42:50.0724829Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "statusCode": "Created",
        "serviceRequestId": "a4c11dbd-697e-47c5-9663-12362307157d",
        "responseBody": "",
        "requestbody": ""
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| Yetkilendirme |BLOB RBAC özelliklerinin olay. Genellikle, "action", "rolü" ve "scope" özelliklerini içerir. |
| çağıran |İşlem, UPN Talebi veya SPN talep kullanılabilirliğine göre gerçekleştiren kullanıcının e-posta adresi. |
| kanallar |Aşağıdaki değerlerden biri: "Yönetici", "İşlem" |
| Talep |Kullanıcı veya uygulama Kaynak Yöneticisi'nde bu işlemi gerçekleştirmek için kimlik doğrulaması için Active Directory tarafından kullanılan JWT belirteci. |
| correlationId |Genellikle bir GUID dize biçiminde. Bir Correlationıd paylaşan olayları aynı uber eyleme ait. |
| açıklama |Olay açıklaması statik metin. |
| eventDataId |Olayın benzersiz tanımlayıcısı. |
| HTTP isteği |Http isteği açıklayan blob. Genellikle "Clientrequestıd'ye", "clientIpAddress" ve "method" (HTTP yöntemi. içerir For example, PUT). |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error"Uyarı",", "Bilgilendirici" ve "Ayrıntılı" |
| resourceGroupName |Etkilenen kaynak için kaynak grubunun adı. |
| resourceProviderName |Etkilenen kaynak için kaynak sağlayıcısının adı |
| resourceId |Etkilenen kaynak kaynak kimliği. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). |
| durum |İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı. |
| alt durumu |Genellikle karşılık gelen REST, HTTP durum kodu diyebilirsiniz, ama bu ortak değerleri gibi bir alt açıklayan diğer dizeleri de içerebilir: Tamam (HTTP durum kodu: 200), oluşturulan (HTTP durum kodu: 201), kabul edilen (HTTP durum kodu: 202), içerik yok (HTTP durum Kodu: 204), hatalı istek (HTTP durum kodu: 400) bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503), ağ geçidi zaman aşımı (HTTP durum kodu: 504). |
| eventTimestamp |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

## <a name="service-health"></a>Hizmet durumu
Bu kategori, Azure'da gerçekleşen tüm hizmet durumu olayları kaydını içerir. Bu kategoride göreceğiniz olay türünü "SQL Azure Doğu ABD, kesinti yaşanıyor." örneğidir Hizmet durumu olayları beş çeşit olarak gelir: eylem gerekli, Kurtarma Yardımlı, olay, bakım, bilgi veya güvenlik, olaydan etkilenen aboneliğindeki bir kaynak varsa yalnızca görünür.

### <a name="sample-event"></a>Örnek olay
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/<subscription ID>/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/<subscription ID>",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "<subscription ID>",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```
Başvurmak [hizmet durumu bildirimlerini](./monitoring-service-notifications.md) makale özelliklerinde değerler hakkındaki belgeleri.

## <a name="alert"></a>Uyarı
Bu kategorideki tüm etkinleştirmeleri Azure uyarıları kaydını içerir. Bu kategoride göreceğiniz olay türünü "myVM üzerindeki CPU % 80'den önceki 5 dakika boyunca bırakıldı." örneğidir Çeşitli Azure sistemleri sahip bir uyarı verme kavramı--tür bir kural tanımlamak ve bu kuralda veya ek koşullarla eşleşen bir bildirim alırsınız. Her bir desteklenen Azure uyarı türü 'etkinleştirir,' veya bir bildirim oluşturmak için koşullar karşılandığında, bir kaydı etkinleştirme etkinlik günlüğü, bu kategoriye de gönderilir.

### <a name="sample-event"></a>Örnek olay

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "<subscription ID>"
}
```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| çağıran | Her zaman Microsoft.Insights/alertRules |
| kanallar | Her zaman "Yöneticisi işlemi" |
| Talep | JSON blob uyarı altyapısının SPN (hizmet asıl adı) ya da kaynak türüne sahip. |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Uyarı olayının açıklaması statik metin. |
| eventDataId |Uyarı olayı benzersiz tanımlayıcısı. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error"Uyarı",", "Bilgilendirici" ve "Ayrıntılı" |
| resourceGroupName |Etkilenen kaynak ölçüm uyarısı ise kaynak grubunun adı. Diğer uyarı türleri için bu uyarıyı içeren kaynak grubunun adıdır. |
| resourceProviderName |Etkilenen kaynak ölçüm uyarısı ise kaynak sağlayıcı adı. Diğer uyarı türleri için uyarı için kaynak sağlayıcısının adını budur. |
| resourceId | Etkilenen kaynak ölçüm uyarısı ise kaynak kimliği adı. Diğer uyarı türleri için uyarı kaynağın kaynak kimliği budur. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). |
| durum |İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı. |
| alt durumu | Genellikle, uyarılar için null. |
| eventTimestamp |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

### <a name="properties-field-per-alert-type"></a>Özellikler alanı uyarı türünün başına
Özellikler alanı uyarı olayının kaynağı, bağlı olarak farklı değerler içerir. İki ortak uyarı olayının etkinlik günlüğü uyarılarına ve ölçüm uyarıları sağlayıcılarındandır.

#### <a name="properties-for-activity-log-alerts"></a>Etkinlik günlüğü uyarıların özellikleri
| Öğe adı | Açıklama |
| --- | --- |
| properties.subscriptionId | Abonelik kimliği etkinleştirilmesi bu etkinlik günlük uyarı kuralı nedeniyle etkinlik günlüğü olay. |
| properties.eventDataId | Olay verileri etkinleştirilmesi bu etkinlik günlük uyarı kuralı nedeniyle etkinlik günlüğü olay kimliği. |
| properties.resourceGroup | Kaynak grubu etkinleştirilmesi bu etkinlik günlük uyarı kuralı nedeniyle etkinlik günlüğü olay. |
| properties.resourceId | Kaynak Kimliği etkinleştirilmesi bu etkinlik günlük uyarı kuralı nedeniyle etkinlik günlüğü olay. |
| properties.eventTimestamp | Etkinleştirilecek bu etkinlik günlük uyarı kuralı neden etkinlik günlüğü olayının olay zaman damgası. |
| properties.operationName | İşlem adı etkinleştirilmesi bu etkinlik günlük uyarı kuralı nedeniyle etkinlik günlüğü olay. |
| Properties.Status | Etkinleştirilecek bu etkinlik günlük uyarı kuralı nedeniyle etkinlik günlüğü olayında durumu.|

#### <a name="properties-for-metric-alerts"></a>Ölçüm uyarıları özellikleri
| Öğe adı | Açıklama |
| --- | --- |
| özellikleri. RuleUri | Ölçüm uyarısı kuralının kendisini kaynak kimliği. |
| özellikleri. RuleName | Ölçüm uyarısı kuralının adı. |
| özellikleri. RuleDescription | Ölçüm uyarısı kuralının (uyarı kuralı tanımlanan) açıklaması. |
| özellikleri. Eşik | Ölçüm uyarı kuralının veriyi değerlendirmede kullanılan eşik değeri. |
| özellikleri. WindowSizeInMinutes | Ölçüm uyarı kuralının veriyi değerlendirmede kullanılan pencere boyutu. |
| özellikleri. Toplama | Ölçüm uyarı kuralda tanımlanan toplama türü. |
| özellikleri. İşleci | Ölçüm uyarı kuralının veriyi değerlendirmede kullanılan koşullu işleç. |
| özellikleri. MetricName | Ölçüm uyarı kuralının veriyi değerlendirmede kullanılan ölçümü ölçüm adı. |
| özellikleri. MetricUnit | Ölçüm uyarı kuralının veriyi değerlendirmede kullanılan ölçüm için ölçüm birimi. |

## <a name="autoscale"></a>Otomatik Ölçeklendirme
Bu kategori, kayıt işlemi herhangi bir otomatik ölçeklendirme ayarı, aboneliğinizde tanımladığınız dayalı otomatik ölçeklendirme altyapısı ilgili olayların içerir. Bu kategoride göreceğiniz olay türünü, "Otomatik ölçek ölçeği artırma eylemi başarısız oldu." örneğidir Otomatik ölçeklendirme kullanarak, otomatik olarak ölçeği genişletme veya ölçeklendirme desteklenen kaynak türü örneği sayısını gün ve/veya yük (ölçüm) verileri kullanarak bir otomatik ölçeklendirme ayarı zamanında temel. Ne zaman koşulları yukarı veya aşağı başlangıç ölçek karşılanması ve başarılı veya başarısız olaylar bu kategorideki kaydedilir.

### <a name="sample-event"></a>Örnek olay
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "<subscription ID>"
}

```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| çağıran | Her zaman Microsoft.Insights/autoscaleSettings |
| kanallar | Her zaman "Yöneticisi işlemi" |
| Talep | JSON blob SPN (hizmet asıl adı) ya da kaynak türü ile otomatik ölçeklendirme altyapısı. |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Otomatik ölçeklendirme olayının statik metin açıklaması. |
| eventDataId |Otomatik ölçeklendirme olayının benzersiz tanımlayıcısı. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error"Uyarı",", "Bilgilendirici" ve "Ayrıntılı" |
| resourceGroupName |Otomatik ölçeklendirme ayarı için kaynak grubunun adı. |
| resourceProviderName |Otomatik ölçeklendirme ayarı için kaynak sağlayıcısının adı. |
| resourceId |Otomatik ölçeklendirme ayarı kaynak kimliği. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). |
| özellikleri. Açıklaması | Otomatik ölçeklendirme altyapısı yapmakta olduğu ayrıntılı açıklaması. |
| özellikleri. ResourceName | Etkilenen kaynak kaynak kimliği (kaynak üzerinde ölçeklendirme eylemi gerçekleştirilmedi) |
| properties.OldInstancesCount | Etkin otomatik ölçeklendirme eylemin önce örnek sayısı. |
| özellikleri. NewInstancesCount | Etkin otomatik ölçeklendirme eylemin sonra örnek sayısı. |
| özellikleri. LastScaleActionTime | Otomatik ölçeklendirme eylemi gerçekleştiği, zaman damgası. |
| durum |İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı. |
| alt durumu | Genellikle, otomatik ölçeklendirme için null. |
| eventTimestamp |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

## <a name="security"></a>Güvenlik
Bu kategori, Azure Güvenlik Merkezi tarafından oluşturulan herhangi bir uyarı kaydını içerir. Bu kategoride göreceğiniz olay türünü, "şüpheli çift uzantılı dosya yürütüldü." örneğidir

### <a name="sample-event"></a>Örnek olay
```json
{
    "channels": "Operation",
    "correlationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "description": "Suspicious double extension file executed. Machine logs indicate an execution of a process with a suspicious double extension.\r\nThis extension may trick users into thinking files are safe to be opened and might indicate the presence of malware on the system.",
    "eventDataId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "eventName": {
        "value": "Suspicious double extension file executed",
        "localizedValue": "Suspicious double extension file executed"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2017-10-18T06:02:18.6179339Z",
    "id": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/events/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/ticks/636439033386179339",
    "level": "Informational",
    "operationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Microsoft.Security/locations/alerts/activate/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/<subscription ID>/providers/Microsoft.Security/locations/centralus/alerts/2518939942613820660_a48f8653-3fc6-4166-9f19-914f030a13d3",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": null
    },
    "submissionTimestamp": "2017-10-18T06:02:52.2176969Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "accountLogonId": "0x2r4",
        "commandLine": "c:\\mydirectory\\doubleetension.pdf.exe",
        "domainName": "hpc",
        "parentProcess": "unknown",
        "parentProcess id": "0",
        "processId": "6988",
        "processName": "c:\\mydirectory\\doubleetension.pdf.exe",
        "userName": "myUser",
        "UserSID": "S-3-2-12",
        "ActionTaken": "Detected",
        "Severity": "High"
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| kanallar | Her zaman "işlem" |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Güvenlik olayı açıklaması statik metin. |
| eventDataId |Güvenlik olayı benzersiz tanımlayıcısı. |
| EventName |Güvenlik olayı kolay adı. |
| id |Güvenlik olayı benzersiz bir kaynak tanımlayıcısı. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error"Uyarı",", "Bilgilendirici" veya "Ayrıntılı" |
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| resourceProviderName |Azure Güvenlik Merkezi kaynak sağlayıcısının adı. Her zaman "Microsoft.Security". |
| Kaynak türü |"Microsoft.Security/locations/alerts" gibi bir güvenlik olayı oluşturulan kaynak türü |
| resourceId |Güvenlik Uyarısı kaynak kimliği. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). Bu özellikler, güvenlik uyarısı türüne bağlı olarak değişir. Bkz: [bu sayfayı](../security-center/security-center-alerts-type.md) gelen güvenlik Merkezi'nden uyarı türlerini açıklaması. |
| özellikleri. Önem derecesi |Önem düzeyi. Olası değerler şunlardır: "Yüksek" "Orta" veya "Düşük." |
| durum |İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı. |
| alt durumu | Genellikle, güvenlik olaylarında null. |
| eventTimestamp |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

## <a name="recommendation"></a>Öneri
Bu kategori, hizmetlerinizi için oluşturulan yeni önerisi kaydını içerir. Bir öneri örneği "kullanımı kullanılabilirlik kümeleri için hataya dayanıklılık." olacaktır Oluşturulabilecek öneri olayları 4 tür vardır: yüksek kullanılabilirlik, performans, güvenlik ve maliyet iyileştirmesi. 

### <a name="sample-event"></a>Örnek olay
```json
{
    "channels": "Operation",
    "correlationId": "92481dfd-c5bf-4752-b0d6-0ecddaa64776",
    "description": "The action was successful.",
    "eventDataId": "06cb0e44-111b-47c7-a4f2-aa3ee320c9c5",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Recommendation",
        "localizedValue": "Recommendation"
    },
    "eventTimestamp": "2018-06-07T21:30:42.976919Z",
    "id": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM/events/06cb0e44-111b-47c7-a4f2-aa3ee320c9c5/ticks/636640038429769190",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Advisor/generateRecommendations/action",
        "localizedValue": "Microsoft.Advisor/generateRecommendations/action"
    },
    "resourceGroupName": "MYRESOURCEGROUP",
    "resourceProviderName": {
        "value": "MICROSOFT.COMPUTE",
        "localizedValue": "MICROSOFT.COMPUTE"
    },
    "resourceType": {
        "value": "MICROSOFT.COMPUTE/virtualmachines",
        "localizedValue": "MICROSOFT.COMPUTE/virtualmachines"
    },
    "resourceId": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.COMPUTE/VIRTUALMACHINES/MYVM",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-06-07T21:30:42.976919Z",
    "subscriptionId": "<Subscription ID>",
    "properties": {
        "recommendationSchemaVersion": "1.0",
        "recommendationCategory": "Security",
        "recommendationImpact": "High",
        "recommendationRisk": "None"
    },
    "relatedEvents": []
}

```
### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| kanallar | Her zaman "işlem" |
| correlationId | Dize biçiminde bir GUID. |
| açıklama |Öneri Olay açıklaması statik metin |
| eventDataId | Öneri etkinliğin benzersiz tanımlayıcısı. |
| category | Her zaman "öneri" |
| id |Öneri olayın benzersiz bir kaynak tanımlayıcısı. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error"Uyarı",", "Bilgilendirici" veya "Ayrıntılı" |
| operationName |İşlemin adı.  Her zaman "Microsoft.Advisor/generateRecommendations/action"|
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| resourceProviderName |"Gibi MICROSOFT.COMPUTE" için bu önerinin geçerli kaynak için kaynak sağlayıcısının adı |
| Kaynak türü |Kaynak türü için "Gibi MICROSOFT.COMPUTE/virtualmachines" için bu önerinin geçerli kaynak adı |
| resourceId |Önerinin geçerli kaynağının kaynak kimliği |
| durum | Her zaman "etkin" |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |
| properties |Kümesi `<Key, Value>` öneri ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük).|
| properties.recommendationSchemaVersion| Etkinlik günlüğü girişinde öneri özelliklerini şema sürümü yayımlandı |
| properties.recommendationCategory | Öneri kategorisi. "Yüksek kullanılabilirlik", "Performans", "Güvenlik" ve "Maliyet" olası değerler şunlardır: |
| properties.recommendationImpact| Öneri etkisini. Olası değerler şunlardır: "Yüksek", "Orta", "Düşük" |
| properties.recommendationRisk| Risk öneri. Olası değerler şunlardır: "Error"Uyarı",", "None" |

## <a name="mapping-to-diagnostic-logs-schema"></a>Tanılama günlükleri şemaya eşleme

Azure etkinlik günlüğünün Event Hubs ad alanı veya bir depolama hesabını akış, verileri izleyen [Azure tanılama günlükleri şema](./monitoring-diagnostic-logs-schema.md). Şemayı özelliklerinden tanılama günlükleri şemaya eşleme şu şekildedir:

| Tanılama günlükleri şema özelliği | Etkinlik günlüğü REST API şema özelliği | Notlar |
| --- | --- | --- |
| time | eventTimestamp |  |
| resourceId | resourceId | Subscriptionıd, resourceType, resourceGroupName tüm çıkarılan ResourceId. |
| operationName | operationName.value |  |
| category | İşlem adının parçası | "Yazma" / "Sil" işlemi türü - kırılımı / "Action" |
| resultType | Status.Value | |
| resultSignature | SubStatus.Value | |
| resultDescription | açıklama |  |
| durationMs | Yok | Her zaman 0 |
| callerIpAddress | httpRequest.clientIpAddress |  |
| correlationId | correlationId |  |
| identity | Talepler ve yetkilendirme özellikleri |  |
| Düzey | Düzey |  |
| location | Yok | Olay işleneceği konumu. *Bu kaynak konumu değildir, ancak bunun yerine burada olay işlendi. Bu özellik gelecek bir güncelleştirmede kaldırılacak.* |
| Özellikler | properties.eventProperties |  |
| properties.eventCategory | category | Properties.eventCategory mevcut değilse, "Yönetim" kategorisidir |
| properties.eventName | EventName |  |
| properties.operationId | operationId |  |
| properties.eventProperties | properties |  |


## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğü'nü (eski adıyla denetim günlükleri) hakkında daha fazla bilgi edinin](monitoring-overview-activity-logs.md)
* [Azure etkinlik günlüğünün Event Hubs'a Stream](monitoring-stream-activity-logs-event-hubs.md)
