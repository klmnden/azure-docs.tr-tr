---
title: "Web kancası Azure ölçüm uyarılarını yapılandırma | Microsoft Docs"
description: "Azure uyarıları diğer'i, Azure dışı sistemlere yeniden yönlendir öğrenin."
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.openlocfilehash: 049803e7701c68559103d9b1fa5dfacf820d0548
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Bir Web kancası Azure ölçüm uyarıyı yapılandırın
İşlem sonrası ya da özel eylemler için diğer sistemlere Azure bir uyarı bildirimine yönlendirmek için Web kancası kullanabilirsiniz. Sohbet veya Mesajlaşma Hizmetleri aracılığıyla veya diğer çeşitli eylemler için bir takım bildirmek için hatalar, oturum, SMS iletileri göndermek Hizmetleri yönlendirmek için bir uyarı durumunda bir Web kancası kullanabilirsiniz. 

Bu makalede, bir Web kancası Azure ölçüm uyarıyı ayarlamak açıklar. Ayrıca, bir Web kancası için HTTP POST için yükü nasıl göründüğünü gösterir. Kurulum ve Azure aktivite şeması hakkında bilgi için günlük uyarı (uyarı) olaylarına bakın [bir Web kancası bir Azure etkinlik günlüğü uyarı çağrı](insights-auditlog-to-webhook-email.md).

Azure uyarıları, uyarı içeriği JSON biçiminde bir Web kancası uyarı oluştururken sağladığınız URI göndermek için HTTP POST kullanın. Şema, bu makalenin sonraki bölümlerinde tanımlanır. URI geçerli bir HTTP veya HTTPS uç noktası olması gerekir. Bir uyarı etkinleştirildiğinde azure istek başına bir giriş gönderir.

## <a name="configure-webhooks-via-the-azure-portal"></a>Azure Portalı aracılığıyla Web kancalarını yapılandırın
Ekleyin veya URI, Web kancası güncelleştirmek için [Azure portal](https://portal.azure.com/)gidin **oluştur/güncelleştir uyarıları**.

![Bir uyarı kuralı bölmesi ekleme](./media/insights-webhooks-alerts/Alertwebhook.png)

Web kancası için URI kullanarak göndermek için bir uyarı da yapılandırabilirsiniz [Azure PowerShell cmdlet'lerini](insights-powershell-samples.md#create-metric-alerts), [platformlar arası CLI](insights-cli-samples.md#work-with-alerts), veya [Azure İzleyici REST API'lerini](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>Web kancası kimlik doğrulaması
Web kancası belirteci tabanlı bir yetkilendirme kullanılarak doğrulanabilir. Web kancası belirteci bir kimliğe sahip URI kaydedilir Örneğin, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Yükü şeması
GÖNDERME işlemini aşağıdaki JSON yükü ve tüm ölçüm tabanlı uyarılar için şema içerir:

```JSON
{
    "WebhookName": "Alert1515515157799",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                "metricName": "Requests",
                "metricUnit": "Count",
                "metricValue": "10",
                "threshold": "10",
                "windowSize": "15",
                "timeAggregation": "Average",
                "operator": "GreaterThanOrEqual"
            },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
        },
        "properties": {
            "key1": "value1",
            "key2": "value2"
        }
    }
}
```


| Alan | Zorunlu | Sabit değer kümesi | Notlar |
|:--- |:--- |:--- |:--- |
| durum |E |Etkin, Çözümlendi |Ayarladığınız koşullara göre uyarı durumu. |
| bağlam |E | |Uyarı bağlamı. |
| timestamp |E | |Hangi uyarının başlatıldığı zaman. |
| id |E | |Her uyarı kuralı benzersiz bir kimliği var. |
| ad |E | |Uyarı adı. |
| açıklama |E | |Uyarı açıklaması. |
| conditionType |E |Ölçüm, olay |Uyarı iki türleri desteklenir: ölçüm ve olay. Ölçüm uyarılar ölçüm bir koşula temel alır. Olay uyarıları etkinlik günlüğünde olay dayanır. Uyarı bir ölçüm veya olaya dayanan denetlemek için bu değeri kullanın. |
| koşul |E | |Denetlenecek belirli alanlar temel **koşul türü** değeri. |
| metricName |Ölçüm uyarıları | |Hangi kural izler tanımlar ölçüm adı. |
| metricUnit |Ölçüm uyarıları |Bayt cinsinden BytesPerSecond, Count, CountPerSecond, yüzde, saniye |Ölçümde izin birimi. Bkz: [izin verilen değerler](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Ölçüm uyarıları | |Uyarıya neden ölçüm gerçek değeri. |
| Eşik |Ölçüm uyarıları | |Uyarının etkin eşik değeri. |
| windowSize |Ölçüm uyarıları | |Süre, eşiğine dayalı uyarı etkinliğini izlemek için kullanılır. Değer 5 dakika ile 1 gün arasında olmalıdır. Değer ISO 8601 süre biçiminde olmalıdır. |
| timeAggregation |Ölçüm uyarıları |Ortalama, en son, maksimum, Minimum, None, toplam |Toplanan veriler zamanla nasıl birleştirilmelidir. Ortalama varsayılan değerdir. Bkz: [izin verilen değerler](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| işleci |Ölçüm uyarıları | |Geçerli ölçüm verileri için ayarlanan eşikle karşılaştırmak için kullanılan işleci. |
| subscriptionId |E | |Azure abonelik kimliği |
| resourceGroupName |E | |Etkilenen kaynağı için kaynak grubu adı. |
| resourceName |E | |Etkilenen kaynağı kaynak adı. |
| resourceType |E | |Etkilenen kaynağı kaynak türü. |
| resourceId |E | |Etkilenen kaynağı kaynak kimliği. |
| resourceRegion |E | |Bölgeyi veya etkilenen kaynağın konumu. |
| portalLink |E | |Portal kaynak Özet sayfasında doğrudan bağlantı. |
| properties |N |İsteğe bağlı |Olay ayrıntılarını olan anahtar/değer çiftleri kümesi. Örneğin, `Dictionary<String, String>`. Özellikler alanı isteğe bağlıdır. Özel kullanıcı Arabirimi veya mantığı uygulama tabanlı iş akışı, kullanıcıların yükü geçirilen anahtar/değer çiftlerinin girebilirsiniz. Özel özellikler geri Web kancası geçirmek için alternatif Web kancası URI kendisini (gibi sorgu parametrelerini) aracılığıyla bir yoludur. |

> [!NOTE]
> Ayarlayabileceğiniz **özellikleri** kullanarak yalnızca alan [Azure İzleyici REST API'lerini](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Sonraki adımlar
* Azure uyarıları ve Web kancalarını videoda hakkında daha fazla bilgi [PagerDuty tümleştirmek Azure uyarılarla](http://go.microsoft.com/fwlink/?LinkId=627080).
* Bilgi edinmek için nasıl [Azure Otomasyon betikleri (runbook'lar) Azure uyarılar yürütme](http://go.microsoft.com/fwlink/?LinkId=627081).
* Bilgi edinmek için nasıl [Azure bir uyarıdan Twilio aracılığıyla SMS iletisi göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Bilgi edinmek için nasıl [Azure bir uyarıdan Slack ileti göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Bilgi edinmek için nasıl [Azure bir uyarıdan bir Azure kuyruğuna ileti göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).
