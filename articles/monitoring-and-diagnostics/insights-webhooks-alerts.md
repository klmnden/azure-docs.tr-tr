---
title: "Web kancası Azure ölçüm uyarılarını yapılandırma | Microsoft Docs"
description: "Azure uyarıları diğer Azure dışı sistemlere yeniden yönlendir."
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
ms.openlocfilehash: 1a885166e5c71f13da222bfc22b0fc579096c52f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Bir Web kancası Azure ölçüm uyarıyı yapılandırın
Web kancası işlem sonrası ya da özel eylemler için diğer sistemlere Azure bir uyarı bildirimine yol olanak sağlar. SMS gönder, hatalar oturum, sohbet ve mesajlaşma Servisleri üzerinden bir takım bildirmek veya başka eylemler herhangi bir sayıda yapmak hizmetlere yönlendirmek için bir uyarı durumunda bir Web kancası kullanın. Bu makalede, bir Web kancası bir Azure ölçüm uyarı ayarlama ve bir Web kancası için HTTP POST için yükü benzer açıklanmaktadır. Kurulum ve Azure etkinlik günlüğü uyarı (uyarı) olayları için şema hakkında bilgi için [bunun yerine bu sayfaya bakın](insights-auditlog-to-webhook-email.md).

Azure uyarıları HTTP POST JSON uyarı içeriğini biçimi, aşağıda bir Web kancası uyarı oluştururken sağladığınız URI tanımlanan şema. Bu URI geçerli bir HTTP veya HTTPS uç noktası olması gerekir. Bir uyarı etkinleştirildiğinde azure istek başına bir giriş gönderir.

## <a name="configuring-webhooks-via-the-portal"></a>Web kancası portalı üzerinden yapılandırma
Ekleyebilir veya Create/Update uyarıları ekranında URI Web kancası güncelleştirme içinde [portal](https://portal.azure.com/).

![Bir uyarı kuralı Ekle](./media/insights-webhooks-alerts/Alertwebhook.png)

Kullanarak bir Web kancası URI göndermek için bir uyarı da yapılandırabilirsiniz [Azure PowerShell cmdlet'leri](insights-powershell-samples.md#create-metric-alerts), [platformlar arası CLI](insights-cli-samples.md#work-with-alerts), veya [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Web kancası kimlik doğrulaması
Web kancası belirteci tabanlı bir yetkilendirme kullanarak kimlik doğrulaması yapabilir. Web kancası URI bir belirteç Kimliğiyle ör kaydedilir. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Yükü şeması
GÖNDERME işlemini aşağıdaki JSON yükü ve tüm ölçüm tabanlı uyarılar için şema içerir.

```JSON
{
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
```


| Alan | Zorunlu | Sabit değer kümesi | Notlar |
|:--- |:--- |:--- |:--- |
| durum |E |"Etkin", "Çözülmüş" |Dışına koşullara uyarı durumu, ayarlamanız gerekir. |
| bağlam |E | |Uyarı bağlamı. |
| timestamp |E | |Hangi uyarının başlatıldığı zaman. |
| id |E | |Her uyarı kuralı benzersiz bir kimliği var. |
| ad |E | |Uyarı adı. |
| açıklama |E | |Uyarı açıklaması. |
| Koşul türü |E |"Ölçüm", "Olay" |Uyarı iki türleri desteklenir. Ölçüm bir koşula göre bir ve diğer etkinlik günlüğünde bir olay tabanlı. Uyarı ölçüm veya olay göre varsa denetlemek için bu değeri kullanın. |
| Koşul |E | |Koşul türü üzerinde temel denetlemek için belirli alanları. |
| metricName |Ölçüm uyarıları | |Hangi kural izler tanımlar ölçüm adı. |
| metricUnit |Ölçüm uyarıları |"Bayt sayısı", "BytesPerSecond", "Count", "CountPerSecond", "Yüzde", "Saniye" |Ölçümde izin birimi. [Değerleri burada listelenen izin verilen](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Ölçüm uyarıları | |Uyarıya neden ölçüm gerçek değeri. |
| Eşik |Ölçüm uyarıları | |Uyarının etkin eşik değeri. |
| pencereboyutu |Ölçüm uyarıları | |Süre, eşiğine dayalı uyarı etkinliğini izlemek için kullanılır. 5 dakika ile 1 gün arasında olmalıdır. ISO 8601 süre biçimi. |
| timeAggregation |Ölçüm uyarıları |"Ortalama", "Son", "En", "Minimum", "None", "Toplam" |Toplanan veriler zamanla nasıl birleştirilmelidir. Ortalama varsayılan değerdir. [Değerleri burada listelenen izin verilen](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| işleci |Ölçüm uyarıları | |Geçerli ölçüm verileri için ayarlanan eşikle karşılaştırmak için kullanılan işleci. |
| subscriptionId |E | |Azure abonelik kimliği |
| resourceGroupName |E | |Etkilenen kaynak kaynak grubu adı. |
| resourceName |E | |Etkilenen kaynağının kaynak adı. |
| Kaynak türü |E | |Etkilenen kaynağın kaynak türü. |
| resourceId |E | |Etkilenen kaynağının kaynak kimliği. |
| resourceRegion |E | |Bölge veya etkilenen kaynağın konumu. |
| portalLink |E | |Portal kaynak Özet sayfasında doğrudan bağlantı. |
| properties |N |İsteğe bağlı |Kümesi `<Key, Value>` çiftleri (yani `Dictionary<String, String>`) olay ayrıntılarını içerir. Özellikler alanı isteğe bağlıdır. Özel kullanıcı Arabirimi veya mantığı uygulama tabanlı iş akışlarında, kullanıcıların yükü geçirilen anahtar/değer girebilirsiniz. Özel özellikler geri Web kancası geçirmek için alternatif Web kancası URI kendisini (gibi sorgu parametrelerini) aracılığıyla yoludur |

> [!NOTE]
> Özellikler alanı yalnızca kullanılarak ayarlanabilir [Azure İzleyici REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Sonraki adımlar
* Azure uyarıları ve Web kancalarını videoda hakkında daha fazla bilgi [Azure uyarılarla tümleştirmek PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
* [Azure Otomasyonu komut dosyaları (Runbook'lar) Azure uyarılar yürütme](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Mantıksal uygulama Twilio aracılığıyla bir SMS gelen Azure uyarı göndermek için kullanın](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
* [Mantıksal uygulama Azure bir uyarıdan Slack ileti göndermek için kullanın](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
* [Mantıksal uygulama Azure bir uyarıdan bir Azure kuyruğuna ileti göndermek için kullanın](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
