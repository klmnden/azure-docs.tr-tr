---
title: "Azure etkinlik günlüğü olay şeması | Microsoft Docs"
description: "Etkinlik günlüğü yayılan veriler için olay şeması anlama"
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2017
ms.author: johnkem
ms.openlocfilehash: 91129da9ef7791a506292d9e13e386a25ee341a8
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="azure-activity-log-event-schema"></a>Azure etkinlik günlüğü olay şeması
**Azure etkinlik günlüğü** Azure'da oluşan herhangi bir abonelik düzeyi olayı bir anlayış sağlar günlüktür. Bu makalede veri kategorisi başına olay şema açıklanmaktadır.

## <a name="administrative"></a>Yönetim
Bu kategorideki tüm kaydını içerir oluşturma, güncelleştirme, silme ve eylem işlemlerine Resource Manager aracılığıyla gerçekleştirilir. Bkz Bu kategoride olay türlerini örneklerindendir "sanal makine oluşturma" ve "belirli bir kaynak türü üzerinde bir işlem olarak bir kullanıcı veya Kaynak Yöneticisi'ni kullanarak uygulama tarafından gerçekleştirilen her eylem Modellenen ağ güvenlik grubunu sil". İşlem türü, yazma, silme veya eylemi ise, yönetim kategorisi kayıtları hem Başlangıç hem de başarılı veya başarısız bu işlemin kaydedilir. Yönetim kategorisi, rol tabanlı erişim denetimi yapılan değişikliklerin bir abonelikte de içerir.

### <a name="sample-event"></a>Örnek olayı
```json
{
  "authorization": {
    "action": "microsoft.support/supporttickets/write",
    "role": "Subscription Admin",
    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
  },
  "caller": "admin@contoso.com",
  "channels": "Operation",
  "claims": {
    "aud": "https://management.core.windows.net/",
    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "iat": "1421876371",
    "nbf": "1421876371",
    "exp": "1421880271",
    "ver": "1.0",
    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
    "puid": "20030000801A118C",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
    "name": "John Smith",
    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
    "appidacr": "2",
    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
    "http://schemas.microsoft.com/claims/authnclassreference": "1"
  },
  "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "description": "",
  "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
  "eventName": {
    "value": "EndRequest",
    "localizedValue": "End request"
  },
  "httpRequest": {
    "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
    "clientIpAddress": "192.168.35.115",
    "method": "PUT"
  },
  "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
  "level": "Informational",
  "resourceGroupName": "MSSupportGroup",
  "resourceProviderName": {
    "value": "microsoft.support",
    "localizedValue": "microsoft.support"
  },
  "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
  "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "operationName": {
    "value": "microsoft.support/supporttickets/write",
    "localizedValue": "microsoft.support/supporttickets/write"
  },
  "properties": {
    "statusCode": "Created"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": "Created",
    "localizedValue": "Created (HTTP Status Code: 201)"
  },
  "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
  "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
  "subscriptionId": "s1"
}
```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| Yetkilendirme |BLOB olay RBAC özelliklerinin. Genellikle "eylem", "rol" ve "scope" özellikleri içerir. |
| Arayan |İşlem, UPN Talebi veya kullanılabilirliğine göre SPN talep yürüttü kullanıcının e-posta adresi. |
| Kanalları |Aşağıdaki değerlerden birini: "Yönetici", "İşlem" |
| Talepleri |Kullanıcı veya Kaynak Yöneticisi'nde bu işlemi gerçekleştirmek için uygulama kimliğini doğrulamak için Active Directory tarafından kullanılan JWT belirteci. |
| correlationId |Genellikle bir GUID dize biçiminde. Bir correlationıd değeri paylaşan olayları aynı uber eyleme ait. |
| açıklama |Olay açıklaması statik metin. |
| eventDataId |Bir olay benzersiz tanımlayıcısı. |
| httpRequest |Http isteği açıklayan blob. Genellikle "clientRequestId", "clientIpAddress" ve "yöntemi" (HTTP yöntemi. içerir For example, PUT). |
| düzeyi |Olay düzeyi. Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" ve "Ayrıntılı" |
| resourceGroupName |Etkilenen kaynak kaynak grubu adı. |
| resourceProviderName |Etkilenen kaynak için kaynak sağlayıcısının adı |
| resourceId |Etkilenen kaynağının kaynak kimliği. |
| Operationıd |Tek bir işleme karşılık gelen olayları arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). |
| durum |İşlemin durumunu açıklayan dize. Bazı genel değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı. |
| alt durum |Genellikle HTTP durum kodu karşılık gelen REST çağrısı, ancak bu ortak değerleri gibi bir alt durum açıklayan diğer dizeleri de içerir: Tamam (HTTP durum kodu: 200), oluşturulan (HTTP durum kodu: 201), kabul edilen (HTTP durum kodu: 202), Hayır içeriği (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400), bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu : 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503), ağ geçidi zaman aşımı (HTTP durum kodu: 504). |
| eventTimestamp |Olay işleme olay karşılık gelen isteği Azure hizmeti tarafından oluşturulan zaman damgası. |
| submissionTimestamp |Olay sorgulama için kullanılabilir duruma zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

## <a name="service-health"></a>Hizmet durumu
Bu kategori, Azure'da oluşan herhangi bir hizmet durumu olay kaydını içerir. Bu kategorideki görür olay türü "SQL Azure Doğu ABD kesinti yaşanıyor." örneğidir Hizmet sistem durumu olayları beş çeşit olarak gelir: eylem gerekli, Destekli kurtarma, olay, bakım, bilgi veya güvenlik ve olay için etkilenen Abonelikteki bir kaynağınız varsa yalnızca görünür.

### <a name="sample-event"></a>Örnek olayı
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
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
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
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
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

### <a name="property-descriptions"></a>Özellik açıklamaları
Öğe adı | Açıklama
-------- | -----------
Kanalları | Aşağıdaki değerlerden biridir: "Yönetici", "İşlem"
correlationId | Genellikle bir GUID dize biçiminde değil. Olaylar, ile ait aynı uber eylemi genellikle aynı correlationıd değeri paylaşın.
açıklama | Olay açıklaması.
eventDataId | Bir olay benzersiz tanımlayıcısı.
EventName | Olay başlığı.
düzeyi | Olay düzeyi. Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" ve "Ayrıntılı"
resourceProviderName | Etkilenen kaynak için kaynak sağlayıcısının adı. Bilinmiyor, bu boş olacaktır.
Kaynak türü| Etkilenen kaynağın kaynak türü. Bilinmiyor, bu boş olacaktır.
alt durum | Genellikle hizmet sistem durumu olayları için null.
eventTimestamp | Günlük olayı oluşturulur ve etkinlik günlüğü gönderilen zaman damgası.
submissionTimestamp |   Olay etkinlik günlüğünde kullanılabilir duruma zaman damgası.
subscriptionId | Bu olayın günlüğe yazıldığı Azure aboneliği.
durum | İşlemin durumunu açıklayan dize. Bazı genel değerler şunlardır: etkin, çözüldü.
operationName | İşlemin adı. Genellikle Microsoft.ServiceHealth/incident/action.
category | "ServiceHealth"
resourceId | Etkilenen kaynağının biliniyorsa kaynak kimliği. Abonelik kimliği aksi sağlanır.
Properties.Title | Bu iletişim için yerelleştirilmiş başlık. İngilizce varsayılan dildir.
Properties.Communication | HTML biçimlendirmesi iletişimi yerelleştirilmiş ayrıntıları. İngilizce varsayılandır.
Properties.incidentType | Olası değerler: AssistedRecovery, ActionRequired, bilgi, olay, bakım, güvenlik
Properties.trackingId | Bu olay ile ilişkili olay tanımlar. Bir olaya ilgili olayları ilişkilendirmek için bunu kullanın.
Properties.impactedServices | Hizmetlerin ve olaydan etkilenen bölgeler açıklar bir kaçış karakterli JSON blobu. Her biri bir ServiceName ve her biri bir RegionName sahip ImpactedRegions listesini sahip hizmetlerin listesini.
Properties.defaultLanguageTitle | İngilizce dilinde iletişim
Properties.defaultLanguageContent | Html biçimlendirmesi veya düz metin olarak İngilizce dilinde iletişim
Properties.Stage | AssistedRecovery, ActionRequired, bilgi, olay, güvenlik için olası değerler: etkin, olan çözümlendi. Bakım için oldukları: etkin, planlanmış, devam ediyor, iptal edildi, Rescheduled, çözümlenmiş, tamamlandı
Properties.communicationId | Bu olay iletişimi ilişkilidir.

## <a name="alert"></a>Uyarı
Bu kategorideki tüm etkinleştirmeleri Azure uyarı kaydı içerir. Bu kategorideki görür olay türü "myVM CPU % 80'den son 5 dakika için bırakıldı." örneğidir Azure sistemleri çeşitli sahip bir uyarı verme kavramı--bir kural çeşit tanımlayabilir ve bu kural için koşullara uyan bir bildirim alıyorsunuz. Her bir desteklenen Azure uyarı türü 'etkinleştirir,' veya bir bildirim oluşturmak için koşullar, bir kayıt etkinleştirme etkinlik günlüğü bu kategoriyi de gönderilir.

### <a name="sample-event"></a>Örnek olayı

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
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
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
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
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| Arayan | Her zaman Microsoft.Insights/alertRules |
| Kanalları | Her zaman "Yönetici, işlemi" |
| Talepleri | JSON blob uyarı altyapısı SPN (hizmet asıl adı) ya da kaynak türüne sahip. |
| correlationId | Dize biçimindeki bir GUID. |
| açıklama |Uyarı Olay açıklaması statik metin. |
| eventDataId |Uyarı olay benzersiz tanımlayıcısı. |
| düzeyi |Olay düzeyi. Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" ve "Ayrıntılı" |
| resourceGroupName |Ölçüm uyarı ise etkilenen kaynak kaynak grubu adı. Diğer uyarı türleri için uyarıyı içeren kaynak grubunun adını budur. |
| resourceProviderName |Etkilenen kaynak ölçüm uyarı ise kaynak sağlayıcısı adı. Diğer uyarı türleri için bu kaynak sağlayıcısı uyarının adıdır. |
| resourceId | Etkilenen kaynak ölçüm uyarı ise kaynak kimliği adı. Diğer uyarı türleri için bu kaynak uyarı kaynak kimliğidir. |
| Operationıd |Tek bir işleme karşılık gelen olayları arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). |
| durum |İşlemin durumunu açıklayan dize. Bazı genel değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı. |
| alt durum | Genellikle uyarılar için null. |
| eventTimestamp |Olay işleme olay karşılık gelen isteği Azure hizmeti tarafından oluşturulan zaman damgası. |
| submissionTimestamp |Olay sorgulama için kullanılabilir duruma zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

### <a name="properties-field-per-alert-type"></a>Uyarı türü başına özellikleri alanı
Özellikler alanı uyarı olay kaynağı bağlı olarak farklı değerler içerir. İki ortak uyarı Olay Etkinlik günlüğü uyarıları ve ölçüm uyarıları sağlayıcılarıdır.

#### <a name="properties-for-activity-log-alerts"></a>Etkinlik günlüğü uyarıların özellikleri
| Öğe adı | Açıklama |
| --- | --- |
| properties.subscriptionId | Abonelik kimliği etkinlik günlüğü olaydan etkinleştirilmesi bu etkinliği günlük uyarı kuralı neden. |
| properties.eventDataId | Olay verileri etkinleştirilmesi bu etkinliği günlük uyarı kuralı neden etkinlik günlüğü olay Kimliğinden. |
| properties.resourceGroup | Kaynak grubu etkinlik günlüğü olaydan etkinleştirilmesi bu etkinliği günlük uyarı kuralı neden. |
| properties.resourceId | Etkinleştirilmesi bu etkinliği günlük uyarı kuralı neden etkinlik günlüğü olay kaynağı kimliği. |
| properties.eventTimestamp | Etkinleştirilmesi bu etkinliği günlük uyarı kuralı neden etkinlik günlüğü olayı olay zaman damgası. |
| properties.operationName | Etkinleştirilmesi bu etkinliği günlük uyarı kuralı neden etkinlik günlüğü olayı işlem adı. |
| Properties.Status | Etkinleştirilmesi bu etkinliği günlük uyarı kuralı neden etkinlik günlüğü olay durumu.|

#### <a name="properties-for-metric-alerts"></a>Ölçüm uyarıların özellikleri
| Öğe adı | Açıklama |
| --- | --- |
| Özellikler. RuleUri | Ölçüm uyarı kuralı kendisini kaynak kimliği. |
| Özellikler. RuleName | Ölçüm uyarı kuralı adı. |
| Özellikler. RuleDescription | (Uyarı kuralı tanımlanan) ölçüm uyarı kuralı açıklaması. |
| Özellikler. Eşik | Ölçüm uyarı kuralı hesaplanmasında kullanılan eşik değeri. |
| Özellikler. WindowSizeInMinutes | Ölçüm uyarı kuralı hesaplanmasında kullanılan pencere boyutu. |
| Özellikler. Toplama | Ölçüm uyarı kuralda tanımlanan toplama türü. |
| Özellikler. İşleci | Ölçüm uyarı kuralı hesaplanmasında kullanılan koşullu işleç. |
| Özellikler. MetricName | Ölçüm uyarı kuralı hesaplanmasında kullanılan ölçüm ölçüm adı. |
| Özellikler. MetricUnit | Ölçüm uyarı kuralı hesaplanmasında kullanılan ölçüm ölçü birimi. |

## <a name="autoscale"></a>Otomatik Ölçeklendirme
Bu kategori, aboneliğinizde tanımladığınız herhangi bir otomatik ölçeklendirme ayarı göre otomatik ölçeklendirme altyapısı işlemi ile ilgili olayları kaydını içerir. Bu kategorideki görür olayın türünü, "Otomatik ölçeklendirme ölçek büyütme eylemi başarısız oldu." örneğidir Otomatik ölçeklendirme'ni kullanarak, otomatik olarak ölçeğini veya ölçeklendirin desteklenen kaynak türü örneği sayısı bir otomatik ölçeklendirme ayarı kullanarak gün ve/veya yük (ölçüm) verileri zamanında temel. Ne zaman koşulları Ölçekle yukarı veya aşağı, başlangıç sınamadan ve başarılı veya başarısız olayları bu kategorideki kaydedilmez.

### <a name="sample-event"></a>Örnek olayı
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
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
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
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
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a>Özellik açıklamaları
| Öğe adı | Açıklama |
| --- | --- |
| Arayan | Her zaman Microsoft.Insights/autoscaleSettings |
| Kanalları | Her zaman "Yönetici, işlemi" |
| Talepleri | JSON blob otomatik ölçeklendirme altyapısı SPN (hizmet asıl adı) ya da kaynak türüne sahip. |
| correlationId | Dize biçimindeki bir GUID. |
| açıklama |Otomatik ölçeklendirme Olay açıklaması statik metin. |
| eventDataId |Otomatik ölçeklendirme olay benzersiz tanımlayıcısı. |
| düzeyi |Olay düzeyi. Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" ve "Ayrıntılı" |
| resourceGroupName |Otomatik ölçeklendirme ayarında kaynak grubunun adı. |
| resourceProviderName |Otomatik ölçeklendirme ayarında kaynak sağlayıcısı adı. |
| resourceId |Otomatik ölçeklendirme ayarında kaynak kimliği. |
| Operationıd |Tek bir işleme karşılık gelen olayları arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). |
| Özellikler. Açıklama | Otomatik ölçeklendirme altyapısı yapmakta olduğu ayrıntılı açıklaması. |
| Özellikler. ResourceName | Etkilenen kaynağının kaynak kimliği (kaynak üzerinde ölçek eylemi gerçekleştirilir) |
| Özellikler. OldInstancesCount | Otomatik ölçeklendirme eylemi etkisi sürdü önce örneği sayısı. |
| Özellikler. NewInstancesCount | Otomatik ölçeklendirme eylemi etkisi sürdü sonra örneği sayısı. |
| Özellikler. LastScaleActionTime | Otomatik ölçeklendirme eylemi gerçekleştiği damgası. |
| durum |İşlemin durumunu açıklayan dize. Bazı genel değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı. |
| alt durum | Genellikle otomatik ölçeklendirme için null. |
| eventTimestamp |Olay işleme olay karşılık gelen isteği Azure hizmeti tarafından oluşturulan zaman damgası. |
| submissionTimestamp |Olay sorgulama için kullanılabilir duruma zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

## <a name="security"></a>Güvenlik
Bu kategori, Azure Güvenlik Merkezi tarafından oluşturulan tüm uyarıları kayıt içeriyor. Bu kategorideki görür olayın türünü, "yürütülen şüpheli çift uzantısının." örneğidir

### <a name="sample-event"></a>Örnek olayı
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
    "id": "/subscriptions/d4742bb8-c279-4903-9653-9858b17d0c2e/providers/Microsoft.Security/locations/centralus/alerts/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/events/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/ticks/636439033386179339",
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
    "resourceId": "/subscriptions/d4742bb8-c279-4903-9653-9858b17d0c2e/providers/Microsoft.Security/locations/centralus/alerts/2518939942613820660_a48f8653-3fc6-4166-9f19-914f030a13d3",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": null
    },
    "submissionTimestamp": "2017-10-18T06:02:52.2176969Z",
    "subscriptionId": "d4742bb8-c279-4903-9653-9858b17d0c2e",
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
| Kanalları | Her zaman "işlem" |
| correlationId | Dize biçimindeki bir GUID. |
| açıklama |Güvenlik Olay açıklaması statik metin. |
| eventDataId |Güvenlik olayı benzersiz tanımlayıcısı. |
| EventName |Güvenlik olayı kolay adı. |
| id |Güvenlik olayı benzersiz kaynak tanımlayıcısı. |
| düzeyi |Olay düzeyi. Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" veya "Ayrıntılı" |
| resourceGroupName |Kaynak için kaynak grubunun adı. |
| resourceProviderName |Azure Güvenlik Merkezi için kaynak sağlayıcısının adı. Her zaman "Microsoft.Security". |
| Kaynak türü |"Microsoft.Security/locations/alerts" gibi güvenlik olayı oluşturulan kaynak türü |
| resourceId |Güvenlik Uyarısı kaynak kimliği. |
| Operationıd |Tek bir işleme karşılık gelen olayları arasında paylaşılan bir GUID. |
| operationName |İşlemin adı. |
| properties |Kümesi `<Key, Value>` olay ayrıntılarını açıklayan çiftleri (diğer bir deyişle, bir sözlük). Bu özellikler, güvenlik uyarısı türüne bağlı olarak değişir. Bkz: [bu sayfayı](../security-center/security-center-alerts-type.md) gelen güvenlik Merkezi'nden uyarı türlerini açıklaması. |
| Özellikler. Önem derecesi |Önem düzeyi. Olası değerler şunlardır: "Yüksek" "Orta" veya "Düşük." |
| durum |İşlemin durumunu açıklayan dize. Bazı genel değerler şunlardır:, ilerleme, başarılı, başarısız, etkin, çözümlenmiş başlatıldı. |
| alt durum | Genellikle güvenlik olayları için null. |
| eventTimestamp |Olay işleme olay karşılık gelen isteği Azure hizmeti tarafından oluşturulan zaman damgası. |
| submissionTimestamp |Olay sorgulama için kullanılabilir duruma zaman damgası. |
| subscriptionId |Azure abonelik kimliği |

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğü (önceki adıyla denetim günlüklerini) hakkında daha fazla bilgi edinin](monitoring-overview-activity-logs.md)
* [Akış Event hubs'a Azure etkinlik günlüğü](monitoring-stream-activity-logs-event-hubs.md)
