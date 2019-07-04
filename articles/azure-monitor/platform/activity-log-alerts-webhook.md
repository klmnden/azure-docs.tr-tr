---
title: Etkinlik günlüğü uyarıları kullanılan Web kancası şeması anlama
description: Bir etkinlik günlüğü uyarısı etkinleştirirken, bir Web kancası URL'sine gönderilen JSON şeması hakkında bilgi edinin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: johnkem
ms.subservice: alerts
ms.openlocfilehash: c91c1badaa4b1bc055859d700857cfd4d062babd
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491509"
---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Azure etkinlik günlüğü uyarıları için Web kancaları
Bir eylem grubu tanımının bir parçası olarak, etkinlik günlüğü uyarı bildirimleri almak için Web kancası uç noktaları yapılandırabilirsiniz. Web kancaları sayesinde işlem sonrası veya özel eylemler için diğer sistemlere bu bildirimleri yönlendirebilirsiniz. Bu makalede bir Web kancası HTTP POST yükü nasıl göründüğünü gösterir.

Etkinlik günlüğü Uyarıları hakkında daha fazla bilgi için bkz. nasıl [Azure etkinlik günlüğü uyarıları oluşturma](activity-log-alerts.md).

Eylem grupları hakkında daha fazla bilgi için bkz. nasıl [Eylem grupları oluşturma](../../azure-monitor/platform/action-groups.md).

> [!NOTE]
> Ayrıca [ortak uyarı şeması](https://aka.ms/commonAlertSchemaDocs), Genişletilebilir tek bir avantajı sağlar ve birleşik uyarı yük boyunca tüm uyarı Hizmetleri Azure İzleyici'de, Web kancası tümleştirmeleri için. [Ortak uyarı şema tanımları hakkında bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)


## <a name="authenticate-the-webhook"></a>Web kancası kimlik doğrulaması
Web kancası, isteğe bağlı olarak kimlik doğrulaması için belirteç tabanlı yetkilendirme kullanabilirsiniz. URI kaydedildiğinde bir belirteç Kimliğiyle, örneğin, Web kancası `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>Yükü şeması
POST işlemi içinde yer alan JSON yükü yükü'nın data.context.activityLog.eventSource alanı göre farklılık gösterir.

### <a name="common"></a>Common

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

### <a name="administrative"></a>Yönetim

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

### <a name="security"></a>Güvenlik

```json
{
    "schemaId":"Microsoft.Insights/activityLogs",
    "data":{"status":"Activated",
        "context":{
            "activityLog":{
                "channels":"Operation",
                "correlationId":"2518408115673929999",
                "description":"Failed SSH brute force attack. Failed brute force attacks were detected from the following attackers: [\"IP Address: 01.02.03.04\"].  Attackers were trying to access the host with the following user names: [\"root\"].",
                "eventSource":"Security",
                "eventTimestamp":"2017-06-25T19:00:32.607+00:00",
                "eventDataId":"Sec-07f2-4d74-aaf0-03d2f53d5a33",
                "level":"Informational",
                "operationName":"Microsoft.Security/locations/alerts/activate/action",
                "operationId":"Sec-07f2-4d74-aaf0-03d2f53d5a33",
                "properties":{
                    "attackers":"[\"IP Address: 01.02.03.04\"]",
                    "numberOfFailedAuthenticationAttemptsToHost":"456",
                    "accountsUsedOnFailedSignInToHostAttempts":"[\"root\"]",
                    "wasSSHSessionInitiated":"No","endTimeUTC":"06/25/2017 19:59:39",
                    "actionTaken":"Detected",
                    "resourceType":"Virtual Machine",
                    "severity":"Medium",
                    "compromisedEntity":"LinuxVM1",
                    "remediationSteps":"[In case this is an Azure virtual machine, add the source IP to NSG block list for 24 hours (see https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/)]",
                    "attackedResourceType":"Virtual Machine"
                },
                "resourceId":"/subscriptions/12345-5645-123a-9867-123b45a6789/resourceGroups/contoso/providers/Microsoft.Security/locations/centralus/alerts/Sec-07f2-4d74-aaf0-03d2f53d5a33",
                "resourceGroupName":"contoso",
                "resourceProviderName":"Microsoft.Security",
                "status":"Active",
                "subscriptionId":"12345-5645-123a-9867-123b45a6789",
                "submissionTimestamp":"2017-06-25T20:23:04.9743772+00:00",
                "resourceType":"MICROSOFT.SECURITY/LOCATIONS/ALERTS"
            }
        },
        "properties":{}
    }
}
```

### <a name="recommendation"></a>Öneri

```json
{
    "schemaId":"Microsoft.Insights/activityLogs",
    "data":{
        "status":"Activated",
        "context":{
            "activityLog":{
                "channels":"Operation",
                "claims":"{\"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress\":\"Microsoft.Advisor\"}",
                "caller":"Microsoft.Advisor",
                "correlationId":"123b4c54-11bb-3d65-89f1-0678da7891bd",
                "description":"A new recommendation is available.",
                "eventSource":"Recommendation",
                "eventTimestamp":"2017-06-29T13:52:33.2742943+00:00",
                "httpRequest":"{\"clientIpAddress\":\"0.0.0.0\"}",
                "eventDataId":"1bf234ef-e45f-4567-8bba-fb9b0ee1dbcb",
                "level":"Informational",
                "operationName":"Microsoft.Advisor/recommendations/available/action",
                "properties":{
                    "recommendationSchemaVersion":"1.0",
                    "recommendationCategory":"HighAvailability",
                    "recommendationImpact":"Medium",
                    "recommendationName":"Enable Soft Delete to protect your blob data",
                    "recommendationResourceLink":"https://portal.azure.com/#blade/Microsoft_Azure_Expert/RecommendationListBlade/recommendationTypeId/12dbf883-5e4b-4f56-7da8-123b45c4b6e6",
                    "recommendationType":"12dbf883-5e4b-4f56-7da8-123b45c4b6e6"
                },
                "resourceId":"/subscriptions/12345-5645-123a-9867-123b45a6789/resourceGroups/contoso/providers/microsoft.storage/storageaccounts/contosoStore",
                "resourceGroupName":"CONTOSO",
                "resourceProviderName":"MICROSOFT.STORAGE",
                "status":"Active",
                "subStatus":"",
                "subscriptionId":"12345-5645-123a-9867-123b45a6789",
                "submissionTimestamp":"2017-06-29T13:52:33.2742943+00:00",
                "resourceType":"MICROSOFT.STORAGE/STORAGEACCOUNTS"
            }
        },
        "properties":{}
    }
}
```

### <a name="servicehealth"></a>ServiceHealth

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

Belirli şeması hakkında ayrıntılı bilgi için hizmet sistem durumu bildirimi etkinlik günlüğü uyarıları görmek [hizmet durumu bildirimlerini](../../azure-monitor/platform/service-notifications.md). Ayrıca, bilgi nasıl [mevcut sorun yönetimi çözümleriyle hizmet sistem durumu Web kancası bildirimleri yapılandırma](../../service-health/service-health-alert-webhook-guide.md).

### <a name="resourcehealth"></a>ResourceHealth

```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Admin, Operation",
                "correlationId": "a1be61fd-37ur-ba05-b827-cb874708babf",
                "eventSource": "ResourceHealth",
                "eventTimestamp": "2018-09-04T23:09:03.343+00:00",
                "eventDataId": "2b37e2d0-7bda-4de7-ur8c6-1447d02265b2",
                "level": "Informational",
                "operationName": "Microsoft.Resourcehealth/healthevent/Activated/action",
                "operationId": "2b37e2d0-7bda-489f-81c6-1447d02265b2",
                "properties": {
                    "title": "Virtual Machine health status changed to unavailable",
                    "details": "Virtual machine has experienced an unexpected event",
                    "currentHealthStatus": "Unavailable",
                    "previousHealthStatus": "Available",
                    "type": "Downtime",
                    "cause": "PlatformInitiated"
                },
                "resourceId": "/subscriptions/<subscription Id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<resource name>",
                "resourceGroupName": "<resource group>",
                "resourceProviderName": "Microsoft.Resourcehealth/healthevent/action",
                "status": "Active",
                "subscriptionId": "<subscription Id>",
                "submissionTimestamp": "2018-09-04T23:11:06.1607287+00:00",
                "resourceType": "Microsoft.Compute/virtualMachines"
            }
        }
    }
}
```

| Öğe adı | Açıklama |
| --- | --- |
| status |Ölçüm uyarıları için kullanılır. Her zaman "için etkinlik günlüğü uyarıları etkin" olarak ayarlayın. |
| context |Olayın bağlamı. |
| resourceProviderName |Etkilenen kaynak kaynak sağlayıcısı. |
| conditionType |Her zaman "olay." |
| name |Uyarı kuralı adı. |
| id |Uyarının kaynak kimliği. |
| description |Uyarı açıklaması uyarı oluşturulduğunda ayarlayın. |
| subscriptionId |Azure abonelik kimliği |
| timestamp |Olay isteği işleyen Azure hizmeti tarafından oluşturulduğu zaman. |
| resourceId |Etkilenen kaynak kaynak kimliği. |
| resourceGroupName |Etkilenen kaynak için kaynak grubunun adı. |
| properties |Kümesi `<Key, Value>` çiftleri (diğer bir deyişle, `Dictionary<String, String>`), olay hakkındaki ayrıntıları içerir. |
| olay |Olay hakkında meta veriler içeren öğe. |
| authorization |Olay rol tabanlı erişim denetimi özellikleri. Bu özellikler genellikle eylemi, rolü ve kapsamı içerir. |
| category |Olayın kategorisi. Desteklenen değerler, yönetim, uyarı, güvenlik, ServiceHealth ve öneri içerir. |
| Çağıran |İşlem, UPN Talebi veya SPN talep kullanılabilirliğine göre gerçekleştiren kullanıcının e-posta adresi. Belirli sistem çağrıları için null olabilir. |
| correlationId |Genellikle bir GUID dize biçiminde. Correlationıd olaylarla aynı büyük eyleme ait ve genellikle bir Correlationıd paylaşın. |
| eventDescription |Olay açıklaması statik metin. |
| eventDataId |Olayın benzersiz tanımlayıcısı. |
| EventSource |Azure hizmeti veya olayı oluşturan altyapı adı. |
| httpRequest |İstek Clientrequestıd'ye clientIpAddress ve HTTP yöntemi genellikle içerir (örneğin, PUT). |
| level |Aşağıdaki değerlerden biri: Kritik hata, uyarı ve bilgilendirici. |
| operationId |Tek işlem için karşılık gelen olaylar arasında paylaşılan genellikle bir GUID. |
| operationName |İşlemin adı. |
| properties |Olay Özellikleri. |
| status |dize. İşlemin durumu. Başlarken, sürüyor, başarılı, başarısız, etkin ve Çözümlenmiş ortak değerlerini içerir. |
| alt durumu |Genellikle, karşılık gelen REST çağrısı HTTP durum kodunu içerir. Ayrıca, alt açıklayan diğer dizeleri de içerebilir. Ortak substatus değerler arasında Tamam (HTTP durum kodu: 200) oluşturuldu (HTTP durum kodu: 201) kabul edildi (HTTP durum kodu: 202), içerik yok (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400) bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503) ve ağ geçidi zaman aşımı (HTTP durum kodu: 504). |

Belirli şeması hakkında ayrıntılı bilgi için diğer tüm etkinlik günlüğü uyarıları görmek [Azure etkinlik günlüğü'ne genel bakış](../../azure-monitor/platform/activity-logs-overview.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğü hakkında daha fazla bilgi](../../azure-monitor/platform/activity-logs-overview.md).
* [Azure uyarılarını Azure Otomasyon betikleri (Runbook'lar) yürütme](https://go.microsoft.com/fwlink/?LinkId=627081).
* [Azure bir uyarıdan Twilio aracılığıyla SMS göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Bu örnek için ölçüm uyarıları, ancak bir etkinlik günlüğü uyarısı ile çalışacak şekilde değiştirilebilir.
* [Azure bir uyarıdan bir Slack iletisi göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Bu örnek için ölçüm uyarıları, ancak bir etkinlik günlüğü uyarısı ile çalışacak şekilde değiştirilebilir.
* [Azure bir uyarıdan bir Azure kuyruğuna bir ileti göndermek için bir mantıksal uygulama kullanma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Bu örnek için ölçüm uyarıları, ancak bir etkinlik günlüğü uyarısı ile çalışacak şekilde değiştirilebilir.

