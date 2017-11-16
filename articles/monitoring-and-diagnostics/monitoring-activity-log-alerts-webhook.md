---
title: "Etkinlik günlüğü uyarıları kullanılan Web kancası şema anlama | Microsoft Docs"
description: "Bir etkinlik günlüğü uyarı etkinleştirdiğinde, bir Web kancası URL'si gönderilen JSON şeması hakkında bilgi edinin."
author: johnkemnetz
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: b0e301f58ec0b5a14254935d6c269cc8006f4eff
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Azure etkinlik günlüğü uyarılar için Web kancaları
Bir eylem grubu tanımının bir parçası olarak, etkinlik günlüğü uyarı bildirimlerini almak için Web kancası uç noktalarını yapılandırabilirsiniz. Web kancası ile işlem sonrası ya da özel eylemler için diğer sistemlere bu bildirimler yönlendirebilirsiniz. Bu makalede, bir Web kancası için HTTP POST için yükü nasıl göründüğünü gösterir.

Etkinlik günlüğü uyarılar hakkında daha fazla bilgi için bkz: nasıl yapılır [Azure etkinlik günlüğü uyarı oluşturma](monitoring-activity-log-alerts.md).

Eylem grupları hakkında daha fazla bilgi için bkz: nasıl yapılır [Eylem grupları oluşturma](monitoring-action-groups.md).

## <a name="authenticate-the-webhook"></a>Web kancası kimlik doğrulaması
Web kancası kimlik doğrulaması için isteğe bağlı olarak belirteç tabanlı bir yetkilendirme kullanabilirsiniz. URI kaydedilir belirteci Kimliğine sahip, örneğin, Web kancası `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>Yükü şeması
POST işleminde yer alan JSON yükü yükü 's data.context.activityLog.eventSource alana göre farklılık gösterir.

###<a name="common"></a>Common
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a>Yönetim
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a>ServiceHealth
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
    "status": "Activated",
    "context": {
        "activityLog": {
        "channels": "Admin",
        "correlationId": "bbac944f-ddc0-4b4c-aa85-cc7dc5d5c1a6",
        "description": "Active: Virtual Machines - Australia East",
        "eventSource": "ServiceHealth",
        "eventTimestamp": "2017-10-18T23:49:25.3736084+00:00",
        "eventDataId": "6fa98c0f-334a-b066-1934-1a4b3d929856",
        "level": "Informational",
        "operationName": "Microsoft.ServiceHealth/incident/action",
        "operationId": "bbac944f-ddc0-4b4c-aa85-cc7dc5d5c1a6",
        "properties": {
            "title": "Virtual Machines - Australia East",
            "service": "Virtual Machines",
            "region": "Australia East",
            "communication": "Starting at 02:48 UTC on 18 Oct 2017 you have been identified as a customer using Virtual Machines in Australia East who may receive errors starting Dv2 Promo and DSv2 Promo Virtual Machines which are in a stopped &quot;deallocated&quot; or suspended state. Customers can still provision Dv1 and Dv2 series Virtual Machines or try deploying Virtual Machines in other regions, as a possible workaround. Engineers have identified a possible fix for the underlying cause, and are exploring implementation options. The next update will be provided as events warrant.",
            "incidentType": "Incident",
            "trackingId": "0NIH-U2O",
            "impactStartTime": "2017-10-18T02:48:00.0000000Z",
            "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"}],\"ServiceName\":\"Virtual Machines\"}]",
            "defaultLanguageTitle": "Virtual Machines - Australia East",
            "defaultLanguageContent": "Starting at 02:48 UTC on 18 Oct 2017 you have been identified as a customer using Virtual Machines in Australia East who may receive errors starting Dv2 Promo and DSv2 Promo Virtual Machines which are in a stopped &quot;deallocated&quot; or suspended state. Customers can still provision Dv1 and Dv2 series Virtual Machines or try deploying Virtual Machines in other regions, as a possible workaround. Engineers have identified a possible fix for the underlying cause, and are exploring implementation options. The next update will be provided as events warrant.",
            "stage": "Active",
            "communicationId": "636439673646212912",
            "version": "0.1.1"
        },
        "status": "Active",
        "subscriptionId": "45529734-0ed9-4895-a0df-44b59a5a07f9",
        "submissionTimestamp": "2017-10-18T23:49:28.7864349+00:00"
        }
    },
    "properties": {}
    }
}
```

Belirli şeması hakkında ayrıntılı bilgi için hizmet sistem durumu bildirimi etkinlik günlüğü uyarıları görmek [hizmet durumu bildirimlerine](monitoring-service-notifications.md). Ayrıca, bilgi nasıl [mevcut sorun yönetimi çözümleriyle hizmet sistem durumu Web kancası bildirimleri yapılandırmak](../service-health/service-health-alert-webhook-guide.md).

Belirli şeması hakkında ayrıntılı bilgi için diğer tüm etkinlik günlüğü uyarıları görmek [Azure etkinlik günlüğü'ne genel bakış](monitoring-overview-activity-logs.md).

| Öğe adı | Açıklama |
| --- | --- |
| durum |Ölçüm uyarılar için kullanılır. Her zaman "etkinlik günlüğü uyarıları için etkinleştirilmiş" olarak ayarlayın. |
| bağlam |Olayın bağlamı. |
| resourceProviderName |Etkilenen kaynağının kaynak sağlayıcısı. |
| Koşul türü |Her zaman "olayını." |
| ad |Uyarı kuralı adı. |
| id |Uyarının kaynak kimliği. |
| açıklama |Uyarı açıklaması uyarı oluşturulduğunda ayarlayın. |
| subscriptionId |Azure abonelik kimliği |
| timestamp |Olay istek işlenmeden Azure hizmeti tarafından oluşturulduğu saat. |
| resourceId |Etkilenen kaynağının kaynak kimliği. |
| resourceGroupName |Etkilenen kaynak kaynak grubu adı. |
| properties |Kümesi `<Key, Value>` çiftleri (diğer bir deyişle, `Dictionary<String, String>`) olay ayrıntılarını içerir. |
| Olay |Olay hakkında meta veriler içeren öğe. |
| Yetkilendirme |Olay rol tabanlı erişim denetimi özellikleri. Bu özellikler, eylem, rolü ve kapsamı genellikle içerir. |
| category |Olay kategorisi. Yönetim, uyarı, güvenlik, ServiceHealth ve öneri desteklenen değerler içerir. |
| Arayan |İşlem, UPN Talebi veya kullanılabilirliğine göre SPN talep gerçekleştiren kullanıcı e-posta adresi. Belirli sistem çağrıları için null olabilir. |
| correlationId |Genellikle bir GUID dize biçiminde. Correlationıd değeri olaylarla aynı büyük eyleme ait ve genellikle bir correlationıd değeri paylaşın. |
| eventDescription |Olay açıklaması statik metin. |
| eventDataId |Olay için benzersiz tanımlayıcı. |
| EventSource |Azure hizmet veya olayı oluşturan altyapı adı. |
| httpRequest |İstek genellikle clientRequestId, clientIpAddress ve HTTP yöntemini içerir (örneğin, PUT). |
| düzeyi |Aşağıdaki değerlerden birini: Kritik hata, uyarı, bilgilendirici ve ayrıntılı. |
| Operationıd |Tek bir işlem için karşılık gelen olayları arasında paylaşılan genellikle bir GUID. |
| operationName |İşlemin adı. |
| properties |Olay Özellikleri. |
| durum |Dize. İşlem durumu. Ortak değerleri başlatıldı, devam eden, başarılı, başarısız, etkin ve Çözümlenmiş içerir. |
| alt durum |Genellikle, karşılık gelen REST çağrısı HTTP durum kodunu içerir. Ayrıca, bir alt durum açıklayan diğer dizeleri de içerebilir. Ortak substatus değerler Tamam (HTTP durum kodu: 200), oluşturulan (HTTP durum kodu: 201), kabul edilen (HTTP durum kodu: 202), Hayır içeriği (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400), bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503) ve ağ geçidi zaman aşımı (HTTP durum kodu : 504). |

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğü hakkında daha fazla bilgi](monitoring-overview-activity-logs.md).
* [Azure Otomasyon betikleri (Runbook'lar) Azure uyarılar yürütme](http://go.microsoft.com/fwlink/?LinkId=627081).
* [Twilio aracılığıyla bir SMS gelen Azure uyarı göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Bu örnek için ölçüm uyarıları olmakla birlikte, bir etkinlik günlüğü uyarı ile çalışmak için değiştirilebilir.
* [Bir Azure uyarıdan Slack ileti göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Bu örnek için ölçüm uyarıları olmakla birlikte, bir etkinlik günlüğü uyarı ile çalışmak için değiştirilebilir.
* [Bir Azure uyarıdan bir Azure kuyruğuna ileti göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Bu örnek için ölçüm uyarıları olmakla birlikte, bir etkinlik günlüğü uyarı ile çalışmak için değiştirilebilir.
