---
title: "Bir Web kancası Azure etkinlik günlüğü uyarılar çağrı | Microsoft Docs"
description: "Rota etkinlik günlüğü olaylarını özel eylemler için diğer hizmetler. Örneğin SMS gönder, hatalar oturum veya sohbet ve mesajlaşma hizmeti üzerinden bir takım bildirin."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 341ab32ad0ec691285fbf1537ee298ab30156a5d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>Bir Web kancası Azure etkinlik günlüğü uyarılar çağırın
Web kancası işlem sonrası ya da özel eylemler için diğer sistemlere Azure bir uyarı bildirimine yol olanak sağlar. SMS gönder, hatalar oturum, sohbet ve mesajlaşma Servisleri üzerinden bir takım bildirmek veya başka eylemler herhangi bir sayıda yapmak hizmetlere yönlendirmek için bir uyarı durumunda bir Web kancası kullanın. Bu makalede, Azure etkinlik günlüğü uyarı ateşlenir olduğunda çağrılacak bir Web kancası ayarlanacağını açıklar. Ayrıca, bir Web kancası için HTTP POST için yükü nasıl göründüğünü gösterir. Kurulum ve bir Azure ölçüm uyarı şeması hakkında bilgi için [bunun yerine bu sayfaya bakın](insights-webhooks-alerts.md). Etkinleştirildiğinde bir e-posta göndermek için bir etkinlik günlüğü alarm de ayarlayabilirsiniz.

> [!NOTE]
> Bu özellik şu anda önizlemede ve belirli bir noktada gelecekte kaldırılacaktır.
>
>

Bir etkinlik günlüğü uyarı kullanarak ayarlayabilirsiniz [Azure PowerShell cmdlet'leri](insights-powershell-samples.md#create-metric-alerts), [platformlar arası CLI](insights-cli-samples.md#work-with-alerts), veya [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx). Şu anda bir Azure portalını kullanarak ayarlanamıyor.

## <a name="authenticating-the-webhook"></a>Web kancası kimlik doğrulaması
Web kancası bu yöntemlerden birini kullanarak doğrulayabilir:

1. **Belirteç tabanlı bir yetkilendirme** -URI kaydedilir belirteci Kimliğine sahip, örneğin, Web kancası`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2. **Temel yetkilendirme** -URI kaydedilir bir kullanıcı adı ve parola, örneğin, Web kancası`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Yükü şeması
GÖNDERME işlemini aşağıdaki JSON yükü ve tüm etkinlik günlüğü tabanlı uyarılar için şema içerir. Bu şemayı ölçüm tabanlı uyarılar tarafından kullanılan benzer.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| Öğe adı | Açıklama |
| --- | --- |
| durum |Ölçüm uyarılar için kullanılır. Her zaman "etkin" için etkinlik günlüğü Uyarıları ayarlayın. |
| bağlam |Olayın bağlamı. |
| resourceProviderName |Etkilenen kaynağının kaynak sağlayıcısı. |
| Koşul türü |Her zaman "olayını." |
| ad |Uyarı kuralı adı. |
| id |Uyarının kaynak kimliği. |
| Açıklama |Uyarı oluşturulması sırasında uyarı açıklaması olarak ayarla. |
| subscriptionId |Azure abonelik kimliği |
| timestamp |Olay istek işlenmeden Azure hizmeti tarafından oluşturulduğu saat. |
| resourceId |Etkilenen kaynağının kaynak kimliği. |
| resourceGroupName |Etkilenen kaynak için kaynak grubunun adı |
| properties |Kümesi `<Key, Value>` çiftleri (yani `Dictionary<String, String>`) olay ayrıntılarını içerir. |
| Olay |Olayla ilgili meta verileri içeren öğe. |
| Yetkilendirme |Olay RBAC özellikleri. Bunlar genellikle "eylem", "rol" ve "scope." içerir |
| category |Olay kategorisi. Desteklenen değerler: yönetici, uyarı, güvenlik, ServiceHealth, öneri. |
| Arayan |İşlem, UPN Talebi veya kullanılabilirliğine göre SPN talep gerçekleştiren kullanıcı e-posta adresi. Belirli sistem çağrıları için null olabilir. |
| correlationId |Genellikle bir GUID dize biçiminde. Correlationıd değeri olaylarla aynı büyük eyleme ait ve genellikle bir correlationıd değeri paylaşın. |
| eventDescription |Olay açıklaması statik metin. |
| eventDataId |Olay için benzersiz tanımlayıcı. |
| EventSource |Azure hizmet veya olayı oluşturan altyapı adı. |
| httpRequest |Genellikle "clientRequestId", "clientIpAddress" ve "yöntemi" içerir (örneğin PUT HTTP yöntemi). |
| düzeyi |Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" ve "Ayrıntılı." |
| Operationıd |Tek bir işlem için karşılık gelen olayları arasında paylaşılan genellikle bir GUID. |
| operationName |İşlemin adı. |
| properties |Olay Özellikleri. |
| durum |Dize. İşlem durumu. Genel değerler şunlardır: "Başlatıldı", "Sürüyor", "Başarılı", "Başarısız", "Active", "Çözülmüş". |
| alt durum |Genellikle, karşılık gelen REST çağrısı HTTP durum kodunu içerir. Ayrıca, bir alt durum açıklayan diğer dizeleri de içerebilir. Ortak alt durum değerleri şunları içerir: Tamam (HTTP durum kodu: 200), oluşturulan (HTTP durum kodu: 201), kabul edilen (HTTP durum kodu: 202), Hayır içeriği (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400), bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503), ağ geçidi zaman aşımı (HTTP durum kodu: 504) |

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğü hakkında daha fazla bilgi edinin](monitoring-overview-activity-logs.md)
* [Azure Otomasyonu komut dosyaları (Runbook'lar) Azure uyarılar yürütme](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Mantıksal uygulama Twilio aracılığıyla bir SMS gelen Azure uyarı göndermek için kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Bu örnek için ölçüm uyarılar, ancak bir etkinlik günlüğü uyarı ile çalışması için değiştirilmesi.
* [Bir Azure uyarıdan Slack ileti göndermek için mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Bu örnek için ölçüm uyarılar, ancak bir etkinlik günlüğü uyarı ile çalışması için değiştirilmesi.
* [Bir Azure uyarıdan bir Azure kuyruğuna ileti göndermek için mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Bu örnek için ölçüm uyarılar, ancak bir etkinlik günlüğü uyarı ile çalışması için değiştirilmesi.
