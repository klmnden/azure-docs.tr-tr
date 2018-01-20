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
ms.openlocfilehash: 08467aed4e1601b32598fc42515d9c38b601a9d4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
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

```json
{
    "WebhookName": "Alert1515526229589",
    "RequestBody": {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "authorization": {
                        "action": "Microsoft.Compute/virtualMachines/deallocate/action",
                        "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1"
                    },
                    "channels": "Operation",
                    "claims": {
                        "aud": "https://management.core.windows.net/",
                        "iss": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "iat": "1234567890",
                        "nbf": "1234567890",
                        "exp": "1234567890",
                        "aio": "Y2NgYBD8ZLlhu27JU6WZsXemMIvVAAA=",
                        "appid": "00000000-0000-0000-0000-000000000000",
                        "appidacr": "2",
                        "e_exp": "262800",
                        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "http://schemas.microsoft.com/identity/claims/objectidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.microsoft.com/identity/claims/tenantid": "00000000-0000-0000-0000-000000000000",
                        "uti": "XnCk46TrDkOQXwo49Y8fAA",
                        "ver": "1.0"
                    },
                    "caller": "00000000-0000-0000-0000-000000000000",
                    "correlationId": "00000000-0000-0000-0000-000000000000",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-01-09T20:11:25.8410967+00:00",
                    "eventDataId": "00000000-0000-0000-0000-000000000000",
                    "level": "Informational",
                    "operationName": "Microsoft.Compute/virtualMachines/deallocate/action",
                    "operationId": "00000000-0000-0000-0000-000000000000",
                    "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
                    "resourceGroupName": "ContosoVM",
                    "resourceProviderName": "Microsoft.Compute",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "00000000-0000-0000-0000-000000000000",
                    "submissionTimestamp": "2018-01-09T20:11:40.2986126+00:00",
                    "resourceType": "Microsoft.Compute/virtualMachines"
                }
            },
            "properties": {}
        }
    },
    "RequestHeader": {
        "Expect": "100-continue",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "IcMBroadcaster/1.0",
        "X-CorrelationContext": "RkkKACgAAAACAAAAEADlBbM7x86VTrHdQ2JlmlxoAQAQALwazYvJ/INPskb8S5QzgDk=",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

| Öğe adı | Açıklama |
| --- | --- |
| durum |Ölçüm uyarılar için kullanılır. Her zaman "etkin" için etkinlik günlüğü Uyarıları ayarlayın. |
| bağlam |Olayın bağlamı. |
| activityLog | Olay günlüğü özellikleri.|
| Yetkilendirme |Olay RBAC özellikleri. Bunlar genellikle "eylem", "rol" ve "scope." içerir |
| action | Uyarı tarafından yakalanan eylem. |
| Kapsam | Uyarı (yani kapsamı Kaynak).|
| kanallar | İşlem |
| Talepleri | Bir bilgi koleksiyonudur adresindeki ilişkili talep. |
| çağıran |GUID veya işlemi, UPN Talebi veya kullanılabilirliğine göre SPN talep gerçekleştiren kullanıcının kullanıcı adı. Belirli sistem çağrıları için null olabilir. |
| correlationId |Genellikle bir GUID dize biçiminde. Correlationıd değeri olaylarla aynı büyük eyleme ait ve genellikle bir correlationıd değeri paylaşın. |
| açıklama |Uyarı oluşturulması sırasında uyarı açıklaması olarak ayarla. |
| eventSource |Azure hizmet veya olayı oluşturan altyapı adı. |
| eventTimestamp |Olayın gerçekleştiği süre. |
| eventDataId |Olay için benzersiz tanımlayıcı. |
| düzey |Aşağıdaki değerlerden birini: "Kritik", "Error"Uyarı",", "Bilgi" ve "Ayrıntılı." |
| operationName |İşlemin adı. |
| operationId |Tek bir işlem için karşılık gelen olayları arasında paylaşılan genellikle bir GUID. |
| resourceId |Etkilenen kaynağının kaynak kimliği. |
| resourceGroupName |Etkilenen kaynak için kaynak grubunun adı |
| resourceProviderName |Etkilenen kaynağının kaynak sağlayıcısı. |
| durum |Dize. İşlem durumu. Genel değerler şunlardır: "Başlatıldı", "Sürüyor", "Başarılı", "Başarısız", "Active", "Çözülmüş". |
| alt durum |Genellikle, karşılık gelen REST çağrısı HTTP durum kodunu içerir. Ayrıca, bir alt durum açıklayan diğer dizeleri de içerebilir. Ortak alt durum değerleri şunları içerir: Tamam (HTTP durum kodu: 200), oluşturulan (HTTP durum kodu: 201), kabul edilen (HTTP durum kodu: 202), Hayır içeriği (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400), bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503), ağ geçidi zaman aşımı (HTTP durum kodu: 504) |
| subscriptionId |Azure abonelik kimliği |
| submissionTimestamp |Olay istek işlenmeden Azure hizmeti tarafından oluşturulduğu saat. |
| resourceType | Olayı oluşturan kaynak türü.|
| properties |Kümesi `<Key, Value>` çiftleri (yani `Dictionary<String, String>`) olay ayrıntılarını içerir. |

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğü hakkında daha fazla bilgi edinin](monitoring-overview-activity-logs.md)
* [Azure Otomasyonu komut dosyaları (Runbook'lar) Azure uyarılar yürütme](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Mantıksal uygulama Twilio aracılığıyla bir SMS gelen Azure uyarı göndermek için kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Bu örnek için ölçüm uyarılar, ancak bir etkinlik günlüğü uyarı ile çalışması için değiştirilmesi.
* [Bir Azure uyarıdan Slack ileti göndermek için mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Bu örnek için ölçüm uyarılar, ancak bir etkinlik günlüğü uyarı ile çalışması için değiştirilmesi.
* [Bir Azure uyarıdan bir Azure kuyruğuna ileti göndermek için mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Bu örnek için ölçüm uyarılar, ancak bir etkinlik günlüğü uyarı ile çalışması için değiştirilmesi.
