---
title: Bir Web kancası Azure etkinlik günlüğü uyarıdaki (Klasik) çağırın
description: Etkinlik günlüğü olaylarını diğer hizmetlere özel eylemler için rota öğrenin. Örneğin, SMS iletileri göndermek, hatalar oturum veya sohbet veya ileti sistemi hizmeti aracılığıyla bir takım bildirin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/23/2017
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: e825d0f2487c20c8c7f3d210d7180b07742d7173
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35262469"
---
# <a name="call-a-webhook-on-an-azure-activity-log-alert"></a>Bir Web kancası bir Azure etkinlik günlüğü uyarı çağırın
Web kancası diğer sistemlere özel eylemler veya sonrası işleme için Azure bir uyarı bildirimine yönlendirmek için kullanabilirsiniz. Sohbet veya Mesajlaşma Hizmetleri aracılığıyla veya diğer çeşitli eylemler için bir takım bildirmek için hatalar, oturum, SMS iletileri göndermek Hizmetleri yönlendirmek için bir uyarı durumunda bir Web kancası kullanabilirsiniz. Bir uyarı etkinleştirildiğinde, e-posta göndermek için bir etkinlik günlüğü alarm de ayarlayabilirsiniz.

Bu makalede, bir Azure etkinlik günlüğü uyarı ateşlenir olduğunda çağrılacak bir Web kancası ayarlamak açıklar. Ayrıca, bir Web kancası için HTTP POST için yükü nasıl göründüğünü gösterir. Kurulum ve Azure ölçüm uyarı şeması hakkında daha fazla bilgi için bkz: [bir Web kancası Azure ölçüm uyarıyı yapılandırmak](insights-webhooks-alerts.md). 

> [!NOTE]
> Şu anda Azure etkinlik günlüğü uyarı üzerinde bir Web kancası çağırma destekleyen Önizleme özelliğidir.
>
>

Kullanarak bir etkinlik günlüğü alarm kurabilirsiniz [Azure PowerShell cmdlet'lerini](insights-powershell-samples.md#create-metric-alerts), [platformlar arası CLI](insights-cli-samples.md#work-with-alerts), veya [Azure İzleyici REST API'lerini](https://msdn.microsoft.com/library/azure/dn933805.aspx). Şu anda bir etkinlik günlüğü alarm ayarlamak için Azure Portalı'nı kullanamazsınız.

## <a name="authenticate-the-webhook"></a>Web kancası kimlik doğrulaması
Web kancası bu yöntemlerden birini kullanarak kimlik doğrulaması:

* **Belirteç tabanlı bir yetkilendirme**. Web kancası belirteci bir kimliğe sahip URI kaydedilir Örneğin, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
* **Temel yetkilendirme**. Web kancası URI, bir kullanıcı adı ve parola ile kaydedilir. Örneğin: `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Yükü şeması
GÖNDERME işlemini aşağıdaki JSON yükü ve tüm etkinlik günlüğü tabanlı uyarılar için şema içerir. Bu şemayı ölçüm tabanlı uyarılar için kullanılan bir benzer.

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
| durum |Ölçüm uyarılar için kullanılır. Etkinlik günlüğü uyarılar için her zaman için etkinleştirildi ayarlayın.|
| bağlam |Olay bağlamı. |
| activityLog | Olay günlüğü özellikleri.|
| Yetkilendirme |Olay rol tabanlı erişim denetimi (RBAC) özellikleri. Bu özellikleri genellikle dahil **eylem**, **rol**, ve **kapsam**. |
| action | Uyarı tarafından yakalanan eylem. |
| scope | (Kaynak) uyarı kapsamı.|
| kanallar | İşlem. |
| Talepleri | Şekliyle bilgilerin toplanması için talep ilişkilendirir. |
| çağıran |İşlemi, UPN Talebi veya kullanılabilirliğine göre SPN talep gerçekleştiren kullanıcının kullanıcı adı veya GUID. Bazı sistem çağrıları için boş bir değer olabilir. |
| correlationId |Genellikle bir GUID dize biçiminde. Olaylarla **correlationıd değeri** aynı büyük eyleme ait. Bunlar genellikle aynı olan **correlationıd değeri** değeri. |
| açıklama |Uyarı oluşturulduğunda ayarlandı uyarı açıklaması. |
| EventSource |Azure hizmet veya olayı oluşturan altyapısı adı. |
| eventTimestamp |Olayın gerçekleştiği süre. |
| eventDataId |Olay benzersiz tanımlayıcısı. |
| düzey |Aşağıdaki değerlerden birini: Kritik hata, uyarı, bilgilendirici veya ayrıntılı. |
| operationName |İşlemin adı. |
| operationId |Genellikle olaylar arasında paylaşılan bir GUID. GUID, genellikle tek bir işleme karşılık gelir. |
| resourceId |Etkilenen kaynağı kaynak kimliği. |
| resourceGroupName |Etkilenen kaynağı için kaynak grubu adı. |
| resourceProviderName |Etkilenen kaynağı kaynak sağlayıcısı. |
| durum |İşlemin durumunu gösteren bir dize değeri. Ortak değerleri başlatıldı, devam eden, başarılı, başarısız, etkin ve Çözümlenmiş içerir. |
| alt durum |Genellikle, karşılık gelen REST çağrısı HTTP durum kodunu içerir. Ayrıca, bir alt durum açıklayan diğer dizeleri de içerebilir. Ortak substatus değerler Tamam (HTTP durum kodu: 200), oluşturulan (HTTP durum kodu: 201), kabul edilen (HTTP durum kodu: 202), Hayır içeriği (HTTP durum kodu: 204), hatalı istek (HTTP durum kodu: 400), bulunamadı (HTTP durum kodu: 404), çakışma (HTTP durum kodu: 409), iç sunucu hatası (HTTP durum kodu: 500), hizmet kullanılamıyor (HTTP durum kodu: 503) ve ağ geçidi zaman aşımı (HTTP durum kodu : 504). |
| subscriptionId |Azure abonelik kimliği |
| submissionTimestamp |Olay istek işlenmeden Azure hizmeti tarafından oluşturulduğu saat. |
| Kaynak türü | Olayı oluşturan kaynak türü.|
| properties |Olay ayrıntılarını olan anahtar/değer çiftleri kümesi. Örneğin, `Dictionary<String, String>`. |

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [etkinlik günlüğü](monitoring-overview-activity-logs.md).
* Bilgi edinmek için nasıl [Azure Otomasyon betikleri (runbook'lar) Azure uyarılar yürütme](http://go.microsoft.com/fwlink/?LinkId=627081).
* Bilgi edinmek için nasıl [Azure bir uyarıdan Twilio aracılığıyla SMS iletisi göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Bu örnek, ölçüm uyarılar için olmakla birlikte, bir etkinlik günlüğü uyarı ile çalışacak şekilde değiştirebilirsiniz.
* Bilgi edinmek için nasıl [Azure bir uyarıdan Slack ileti göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Bu örnek, ölçüm uyarılar için olmakla birlikte, bir etkinlik günlüğü uyarı ile çalışacak şekilde değiştirebilirsiniz.
* Bilgi edinmek için nasıl [Azure bir uyarıdan bir Azure kuyruğuna ileti göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Bu örnek, ölçüm uyarılar için olmakla birlikte, bir etkinlik günlüğü uyarı ile çalışacak şekilde değiştirebilirsiniz.
