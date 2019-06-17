---
title: Azure etkinlik günlüğü olay şeması
description: Etkinlik günlüğüne yayılan veriler için olay şemasının anlama
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 1/16/2019
ms.author: dukek
ms.subservice: logs
ms.openlocfilehash: ba5e0f696f54f46fb14086b542dc3b2e64155975
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244945"
---
# <a name="azure-activity-log-event-schema"></a>Azure etkinlik günlüğü olay şeması
**Azure etkinlik günlüğü** Azure'da gerçekleşen herhangi bir abonelik düzeyindeki olayların sağlayan günlüktür. Bu makalede veri kategorisini başına olay şeması. Portal, PowerShell, CLI veya karşı REST API aracılığıyla doğrudan veri okunuyorsa veri şeması bağlı olarak farklı [veri depolama veya günlük profilini kullanarak Event Hubs akış](activity-log-export.md). Aşağıdaki örnekler, portal, PowerShell, CLI ve REST API kullanıma sunulan teklifinizle şema gösterir. Bu özellikler için bir eşleme [Azure tanılama günlükleri şema](diagnostic-logs-schema.md) makalenin sonunda sağlanır.

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
| authorization |BLOB RBAC özelliklerinin olay. Genellikle, "action", "rolü" ve "scope" özelliklerini içerir. |
| Çağıran |İşlem, UPN Talebi veya SPN talep kullanılabilirliğine göre gerçekleştiren kullanıcının e-posta adresi. |
| Kanallar |Aşağıdaki değerlerden biri: "Yönetici", "İşlem" |
| claims |Kullanıcı veya uygulama Kaynak Yöneticisi'nde bu işlemi gerçekleştirmek için kimlik doğrulaması için Active Directory tarafından kullanılan JWT belirteci. |
| correlationId |Genellikle bir GUID dize biçiminde. Bir Correlationıd paylaşan olayları aynı uber eyleme ait. |
| description |Olay açıklaması statik metin. |
| eventDataId |Olayın benzersiz tanımlayıcısı. |
| EventName | Yönetim olayı kolay adı. |
| category | "Yönetici" her zaman |
| httpRequest |Http isteği açıklayan blob. Genellikle "Clientrequestıd'ye", "clientIpAddress" ve "method" (HTTP yöntemi. içerir For example, PUT). |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error", "Uyarı" ve "Bilgilendirici" |
| resourceGroupName |Etkilenen kaynak için kaynak grubunun adı. |
| resourceProviderName |Etkilenen kaynak için kaynak sağlayıcısının adı |
| Kaynak türü | Yönetici bir olay tarafından etkilenen kaynak türü. |
| resourceId |Etkilenen kaynak kaynak kimliği. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). |
| status |İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır: Çalışmaya, sürüyor, başarılı, başarısız, etkin, çözümlenmiş. |
| alt durumu |Genellikle karşılık gelen REST, HTTP durum kodu diyebilirsiniz, ama bu ortak değerleri gibi bir alt açıklayan diğer dizeleri de içerebilir: Tamam (HTTP durum kodu: 200) oluşturuldu (HTTP durum kodu: 201) kabul edildi (HTTP durum kodu: 202), içerik yok (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400) bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503), ağ geçidi zaman aşımı (HTTP durum kodu: 504). |
| eventTimestamp |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

## <a name="service-health"></a>Hizmet durumu
Bu kategori, Azure'da gerçekleşen tüm hizmet durumu olayları kaydını içerir. Bu kategoride göreceğiniz olay türünü "SQL Azure Doğu ABD, kesinti yaşanıyor." örneğidir Hizmet durumu olayları beş çeşit olarak bulunur: Yardımlı kurtarma, olay, bakım, bilgi veya güvenlik, gerekli, eylem ve olaydan etkilenen aboneliğindeki bir kaynak varsa yalnızca görünür.

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
Başvurmak [hizmet durumu bildirimlerini](./../../azure-monitor/platform/service-notifications.md) makale özelliklerinde değerler hakkındaki belgeleri.

## <a name="resource-health"></a>Kaynak durumu
Bu kategori, Azure kaynaklarınıza ortaya çıkan herhangi bir kaynak sistem durumu olayları kaydını içerir. Bu kategoride göreceğiniz olay türünü, "sanal makine sistem durumu kullanılamaz değiştirildi." örneğidir Kaynak sistem durumu olayları dört durum durumlardan birini temsil edebilir: Kullanılabilir, yok, düşürülmüş ve bilinmeyen. Ayrıca, kaynak sistem durumu olayları, Platform tarafından başlatılan veya kullanıcı tarafından başlatılan olacak şekilde sınıflandırılabilir.

### <a name="sample-event"></a>Örnek olay

```json
{
    "channels": "Admin, Operation",
    "correlationId": "28f1bfae-56d3-7urb-bff4-194d261248e9",
    "description": "",
    "eventDataId": "a80024e1-883d-37ur-8b01-7591a1befccb",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "ResourceHealth",
        "localizedValue": "Resource Health"
    },
    "eventTimestamp": "2018-09-04T15:33:43.65Z",
    "id": "/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>/events/a80024e1-883d-42a5-8b01-7591a1befccb/ticks/636716720236500000",
    "level": "Critical",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Resourcehealth/healthevent/Activated/action",
        "localizedValue": "Health Event Activated"
    },
    "resourceGroupName": "<resource group>",
    "resourceProviderName": {
        "value": "Microsoft.Resourcehealth/healthevent/action",
        "localizedValue": "Microsoft.Resourcehealth/healthevent/action"
    },
    "resourceType": {
        "value": "Microsoft.Compute/virtualMachines",
        "localizedValue": "Microsoft.Compute/virtualMachines"
    },
    "resourceId": "/subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-09-04T15:36:24.2240867Z",
    "subscriptionId": "<subscription ID>",
    "properties": {
        "stage": "Active",
        "title": "Virtual Machine health status changed to unavailable",
        "details": "Virtual machine has experienced an unexpected event",
        "healthStatus": "Unavailable",
        "healthEventType": "Downtime",
        "healthEventCause": "PlatformInitiated",
        "healthEventCategory": "Unplanned"
    },
    "relatedEvents": []
}
```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| Kanallar | Her zaman "Yöneticisi işlemi" |
| correlationId | Dize biçiminde bir GUID. |
| description |Uyarı olayının açıklaması statik metin. |
| eventDataId |Uyarı olayı benzersiz tanımlayıcısı. |
| category | Her zaman "ResourceHealth" |
| eventTimestamp |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error", "Uyarı", "Bilgilendirici" ve "Ayrıntılı" |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| resourceGroupName |Kaynağın bulunduğu kaynak grubunun adı. |
| resourceProviderName |Her zaman "Microsoft.Resourcehealth/healthevent/action". |
| Kaynak türü | Kaynak durumu olay tarafından etkilenen kaynak türü. |
| resourceId | Etkilenen kaynak adı kaynak kimliği. |
| status |Sistem durumu olayı durumunu açıklayan bir dize. Değerleri olabilir: Etkin, çözülmüş, sürüyor, güncelleştirildi. |
| alt durumu | Genellikle, uyarılar için null. |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük).|
| Properties.Title | Kaynak sistem durumunu açıklayan kullanıcı dostu bir dize. |
| Properties.details | Daha fazla olay hakkında ayrıntılar açıklanmaktadır kullanıcı dostu bir dize. |
| properties.currentHealthStatus | Kaynağın geçerli sistem durumu. Aşağıdaki değerlerden biri: "Kullanılabilir olma", "Kullanılamıyor", "Düşük" ve "Bilinmeyen". |
| properties.previousHealthStatus | Kaynak önceki sistem durumu. Aşağıdaki değerlerden biri: "Kullanılabilir olma", "Kullanılamıyor", "Düşük" ve "Bilinmeyen". |
| Properties.Type | Kaynak sistem durumu olay türü açıklaması. |
| properties.cause | Kaynak sistem durumu olayı nedenini açıklaması. "UserInitiated" ve "PlatformInitiated". |


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
| Çağıran | Her zaman Microsoft.Insights/alertRules |
| Kanallar | Her zaman "Yöneticisi işlemi" |
| claims | JSON blob uyarı altyapısının SPN (hizmet asıl adı) ya da kaynak türüne sahip. |
| correlationId | Dize biçiminde bir GUID. |
| description |Uyarı olayının açıklaması statik metin. |
| eventDataId |Uyarı olayı benzersiz tanımlayıcısı. |
| category | Her zaman "uyarı" |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error", "Uyarı" ve "Bilgilendirici" |
| resourceGroupName |Etkilenen kaynak ölçüm uyarısı ise kaynak grubunun adı. Diğer uyarı türleri için uyarıyı içeren kaynak grubunun adıdır. |
| resourceProviderName |Etkilenen kaynak ölçüm uyarısı ise kaynak sağlayıcı adı. Diğer uyarı türleri için kaynak sağlayıcısı için uyarı adıdır. |
| resourceId | Etkilenen kaynak ölçüm uyarısı ise kaynak kimliği adı. Diğer uyarı türleri için kaynak uyarı kaynak kimliğidir. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). |
| status |İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır: Çalışmaya, sürüyor, başarılı, başarısız, etkin, çözümlenmiş. |
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
| Çağıran | Her zaman Microsoft.Insights/autoscaleSettings |
| Kanallar | Her zaman "Yöneticisi işlemi" |
| claims | JSON blob SPN (hizmet asıl adı) ya da kaynak türü ile otomatik ölçeklendirme altyapısı. |
| correlationId | Dize biçiminde bir GUID. |
| description |Otomatik ölçeklendirme olayının statik metin açıklaması. |
| eventDataId |Otomatik ölçeklendirme olayının benzersiz tanımlayıcısı. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error", "Uyarı" ve "Bilgilendirici" |
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
| status |İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır: Çalışmaya, sürüyor, başarılı, başarısız, etkin, çözümlenmiş. |
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
| Kanallar | Her zaman "işlem" |
| correlationId | Dize biçiminde bir GUID. |
| description |Güvenlik olayı açıklaması statik metin. |
| eventDataId |Güvenlik olayı benzersiz tanımlayıcısı. |
| EventName |Güvenlik olayı kolay adı. |
| category | Her zaman "güvenlik" |
| id |Güvenlik olayı benzersiz bir kaynak tanımlayıcısı. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error", "Uyarı" veya "Bilgilendirici" |
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| resourceProviderName |Azure Güvenlik Merkezi kaynak sağlayıcısının adı. Her zaman "Microsoft.Security". |
| Kaynak türü |"Microsoft.Security/locations/alerts" gibi bir güvenlik olayı oluşturulan kaynak türü |
| resourceId |Güvenlik Uyarısı kaynak kimliği. |
| operationId |Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). Bu özellikler, güvenlik uyarısı türüne bağlı olarak değişir. Bkz: [bu sayfayı](../../security-center/security-center-alerts-type.md) gelen güvenlik Merkezi'nden uyarı türlerini açıklaması. |
| özellikleri. Önem derecesi |Önem düzeyi. Olası değerler şunlardır: "Yüksek" "Orta" veya "Düşük." |
| status |İşlemin durumunu açıklayan bir dize. Sık karşılaşılan bazı değerler şunlardır: Çalışmaya, sürüyor, başarılı, başarısız, etkin, çözümlenmiş. |
| alt durumu | Genellikle, güvenlik olaylarında null. |
| eventTimestamp |Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

## <a name="recommendation"></a>Öneri
Bu kategori, hizmetlerinizi için oluşturulan yeni önerisi kaydını içerir. Bir öneri örneği "kullanımı kullanılabilirlik kümeleri için hataya dayanıklılık." olacaktır Oluşturulabilecek öneri olayları dört tür vardır: Yüksek kullanılabilirlik, performans, güvenlik ve maliyet iyileştirme. 

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
| Kanallar | Her zaman "işlem" |
| correlationId | Dize biçiminde bir GUID. |
| description |Öneri Olay açıklaması statik metin |
| eventDataId | Öneri etkinliğin benzersiz tanımlayıcısı. |
| category | Her zaman "öneri" |
| id |Öneri olayın benzersiz bir kaynak tanımlayıcısı. |
| düzey |Olay düzeyi. Aşağıdaki değerlerden biri: "Kritik", "Error", "Uyarı" veya "Bilgilendirici" |
| operationName |İşlemin adı.  Her zaman "Microsoft.Advisor/generateRecommendations/action"|
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| resourceProviderName |"Gibi MICROSOFT.COMPUTE" için bu önerinin geçerli kaynak için kaynak sağlayıcısının adı |
| Kaynak türü |Kaynak türü için "Gibi MICROSOFT.COMPUTE/virtualmachines" için bu önerinin geçerli kaynak adı |
| resourceId |Önerinin geçerli kaynağının kaynak kimliği |
| status | Her zaman "etkin" |
| submissionTimestamp |Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId |Azure abonelik kimliği |
| properties |Kümesi `<Key, Value>` öneri ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük).|
| properties.recommendationSchemaVersion| Etkinlik günlüğü girişinde öneri özelliklerini şema sürümü yayımlandı |
| properties.recommendationCategory | Öneri kategorisi. "Yüksek kullanılabilirlik", "Performans", "Güvenlik" ve "Maliyet" olası değerler şunlardır: |
| properties.recommendationImpact| Öneri etkisini. Olası değerler şunlardır: "Yüksek", "Orta", "Düşük" |
| properties.recommendationRisk| Risk öneri. Olası değerler şunlardır: "Error"Uyarı",", "None" |

## <a name="policy"></a>İlke

Bu kategoride işlemlerinin tarafından gerçekleştirilen tüm etkin eylem kayıtları [Azure İlkesi](../../governance/policy/overview.md). Örnekler görmek Bu kategoride olay türlerini _denetim_ ve _Reddet_. İlke tarafından gerçekleştirilen her eylemi, bir kaynak üzerinde bir işlem olarak modellenir.

### <a name="sample-policy-event"></a>Örnek ilke olayı

```json
{
    "authorization": {
        "action": "Microsoft.Resources/checkPolicyCompliance/read",
        "scope": "/subscriptions/<subscriptionID>"
    },
    "caller": "33a68b9d-63ce-484c-a97e-94aef4c89648",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.azure.com/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "appid": "1d78a85d-813d-46f0-b496-dd72f50a3ec0",
        "appidacr": "2",
        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "description": "",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Policy",
        "localizedValue": "Policy"
    },
    "eventTimestamp": "2019-01-15T13:19:56.1227642Z",
    "id": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy/events/13bbf75f-36d5-4e66-b693-725267ff21ce/ticks/636831551961227642",
    "level": "Warning",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Authorization/policies/audit/action",
        "localizedValue": "Microsoft.Authorization/policies/audit/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Sql",
        "localizedValue": "Microsoft SQL"
    },
    "resourceType": {
        "value": "Microsoft.Resources/checkPolicyCompliance",
        "localizedValue": "Microsoft.Resources/checkPolicyCompliance"
    },
    "resourceId": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/contososqlpolicy",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-01-15T13:20:17.1077672Z",
    "subscriptionId": "<subscriptionID>",
    "properties": {
        "isComplianceCheck": "True",
        "resourceLocation": "westus2",
        "ancestors": "72f988bf-86f1-41af-91ab-2d7cd011db47",
        "policies": "[{\"policyDefinitionId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.
            Authorization/policyDefinitions/5775cdd5-d3d3-47bf-bc55-bb8b61746506/\",\"policyDefiniti
            onName\":\"5775cdd5-d3d3-47bf-bc55-bb8b61746506\",\"policyDefinitionEffect\":\"Deny\",\"
            policyAssignmentId\":\"/subscriptions/<subscriptionID>/providers/Microsoft.Authorization
            /policyAssignments/991a69402a6c484cb0f9b673/\",\"policyAssignmentName\":\"991a69402a6c48
            4cb0f9b673\",\"policyAssignmentScope\":\"/subscriptions/<subscriptionID>\",\"policyAssig
            nmentParameters\":{}}]"
    },
    "relatedEvents": []
}
```

### <a name="policy-event-property-descriptions"></a>İlke olay özellik açıklamaları

| Öğe adı | Açıklama |
| --- | --- |
| authorization | RBAC olay özelliklerinin dizisi. Yeni kaynaklar için eylem ve değerlendirme tetiklenen isteğinin kapsamı budur. Var olan kaynaklar için "Microsoft.Resources/checkPolicyCompliance/read" bir eylemdir. |
| Çağıran | Yeni kaynaklar için bir dağıtım tarafından başlatılan kimlik. Var olan kaynaklar için Microsoft Azure İlkesi Insights RP GUİD'si. |
| Kanallar | İlke olaylarını yalnızca "İşlem" kanal kullanın. |
| claims | Kullanıcı veya uygulama Kaynak Yöneticisi'nde bu işlemi gerçekleştirmek için kimlik doğrulaması için Active Directory tarafından kullanılan JWT belirteci. |
| correlationId | Genellikle bir GUID dize biçiminde. Bir Correlationıd paylaşan olayları aynı uber eyleme ait. |
| description | İlke olaylarını Bu alan boştur. |
| eventDataId | Olayın benzersiz tanımlayıcısı. |
| EventName | "BeginRequest" veya "EndRequest". "BeginRequest" Gecikmeli auditIfNotExists ve Deployıfnotexists değerlendirmeleri ve Deployıfnotexists etkili bir şablon dağıtımı başladığı için kullanılır. Tüm diğer işlemler "EndRequest" döndürür. |
| category | Etkinlik günlüğü olayında "ilkesi" olarak bildirir. |
| eventTimestamp | Olay karşılık gelen isteği işlemeye Azure hizmeti tarafından bir olay oluşturulduğunda zaman damgası. |
| id | Belirli kaynak olayı benzersiz tanımlayıcısı. |
| düzey | Olay düzeyi. Denetim "Uyarı" ve "Error" Reddet kullanır. AuditIfNotExists veya Deployıfnotexists bir hata önem derecesine bağlı olarak, "Uyarı" veya "Error" oluşturabilirsiniz. Diğer tüm ilke olaylarını "Bilgilendirici" kullanın. |
| operationId | Tek bir işleme karşılık gelen olaylar arasında paylaşılan bir GUID. |
| operationName | İlke etkisi'için işlem adını ve doğrudan ilişkilendirir. |
| resourceGroupName | Değerlendirilen kaynağı için kaynak grubunun adı. |
| resourceProviderName | Değerlendirilen kaynağın kaynak sağlayıcısının adı. |
| Kaynak türü | Yeni kaynaklar için değerlendirilen türüdür. Var olan kaynaklar için "Microsoft.Resources/checkPolicyCompliance" döndürür. |
| resourceId | Değerlendirilen kaynağının kaynak kimliği. |
| status | İlke değerlendirme sonucu durumunu açıklayan bir dize. "Başarılı" çoğu ilke değerlendirmeleri döndürür, ancak reddetme etkisi "Başarısız" döndürür. Hataları auditIfNotExists veya Deployıfnotexists ayrıca "Başarısız" döndürür. |
| alt durumu | İlke olaylarını alan boştur. |
| submissionTimestamp | Olay sorgulamak için kullanılabilen kalktığında zaman damgası. |
| subscriptionId | Azure abonelik kimliği |
| properties.isComplianceCheck | Yeni bir kaynak dağıtılır veya var olan bir kaynağın kaynak yöneticisi özellikleri güncelleştirildi "False" döndürür. Diğer tüm [değerlendirme Tetikleyicileri](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) neden "True". |
| properties.resourceLocation | Değerlendirilen kaynak Azure bölgesi. |
| Properties.ancestors | Üst yönetim grupları virgülle ayrılmış bir listesi için en uzak dizinleriyle doğrudan üst öğeden sıralı. |
| Properties.Policies | İlke tanımı, atama, etkili ve bu ilke değerlendirmesi sonucudur parametreleri hakkındaki ayrıntıları içerir. |
| relatedEvents | İlke olaylarını Bu alan boştur. |

## <a name="mapping-to-diagnostic-logs-schema"></a>Tanılama günlükleri şemaya eşleme

Azure etkinlik günlüğünün Event Hubs ad alanı veya bir depolama hesabını akış, verileri izleyen [Azure tanılama günlükleri şema](./diagnostic-logs-schema.md). Şemayı özelliklerinden tanılama günlükleri şemaya eşleme şu şekildedir:

| Tanılama günlükleri şema özelliği | Etkinlik günlüğü REST API şema özelliği | Notlar |
| --- | --- | --- |
| time | eventTimestamp |  |
| resourceId | resourceId | Subscriptionıd, resourceType, resourceGroupName tüm çıkarılan ResourceId. |
| operationName | operationName.value |  |
| category | İşlem adının parçası | "Yazma" / "Sil" işlemi türü - kırılımı / "Action" |
| resultType | Status.Value | |
| resultSignature | SubStatus.Value | |
| resultDescription | description |  |
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
* [Etkinlik günlüğü hakkında daha fazla bilgi edinin](activity-logs-overview.md)
* [Azure depolama veya olay hub'larına Etkinlik günlüğünü dışarı aktarma](activity-log-export.md)

