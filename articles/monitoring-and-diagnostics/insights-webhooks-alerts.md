---
title: Web kancası kullanarak bir Azure sistem bilgisini Klasik ölçüm uyarısı sahip
description: Diğer'i, Azure dışı sistemlere Azure ölçüm uyarıları yeniden yönlendirme hakkında bilgi edinin.
author: snehithm
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/03/2017
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 74a4066e3d30b1e91fe558fcfeb6f39220e41c02
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887353"
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Azure bir ölçüm uyarısında Web kancası yapılandırma
İşlem Sonrası veya özel eylemler için diğer sistemlere Azure bir uyarı bildirimine yönlendirmek için Web kancaları kullanabilirsiniz. Sohbet veya Mesajlaşma Hizmetleri aracılığıyla veya diğer çeşitli eylemler için bir takıma bildirin hataları oturum, SMS iletileri göndermek Hizmetleri yönlendirmek için bir uyarısında Web kancası kullanabilirsiniz. 

Bu makalede, Azure bir ölçüm uyarısında Web kancası ayarlama açıklanır. Ayrıca, bir Web kancası HTTP POST yükü nasıl göründüğünü gösterir. Azure etkinlik için şema ve kurulumu hakkında bilgi için günlük uyarı (uyarı) olaylarına bakın [bir Azure etkinlik günlüğü uyarısında Web kancası çağırma](insights-auditlog-to-webhook-email.md).

Azure uyarıları, uyarı içeriği JSON biçiminde bir Web kancası uyarı oluştururken sağladığınız URI göndermek için HTTP POST kullanın. Şema, bu makalenin sonraki bölümlerinde tanımlanır. URI geçerli bir HTTP veya HTTPS uç noktası olmalıdır. Azure, uyarı etkinleştirildiğinde, istek başına tek bir giriş gönderir.

## <a name="configure-webhooks-via-the-azure-portal"></a>Azure Portalı aracılığıyla Web kancalarını yapılandırma
Eklemek veya Web kancası URI güncelleştirmek için [Azure portalında](https://portal.azure.com/)Git **Create/Update uyarılar**.

![Bir uyarı kuralı bölmesi ekleme](./media/insights-webhooks-alerts/Alertwebhook.png)

Kullanarak bir Web kancası için URI gönderilecek bir uyarı yapılandırabilirsiniz [Azure PowerShell cmdlet'lerini](insights-powershell-samples.md#create-metric-alerts), [platformlar arası CLI](insights-cli-samples.md#work-with-alerts), veya [Azure İzleyici REST API'leri](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>Web kancası kimlik doğrulaması
Web kancası, belirteç tabanlı yetkilendirme kullanarak kimlik doğrulaması yapabilirsiniz. Web kancası belirteci bir kimliğe sahip URI kaydedilir Örneğin, `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Yükü şeması
GÖNDERME işlemi, tüm ölçüm tabanlı uyarılar için şema ve aşağıdaki JSON yükü içerir:

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
| durum |E |Etkin, çözülmüş |Belirlediğiniz koşullara göre uyarı durumu. |
| bağlam |E | |Uyarı bağlamı. |
| timestamp |E | |Uyarının tetiklenme zamanı. |
| id |E | |Her uyarı kuralı benzersiz bir kimliğe sahiptir. |
| ad |E | |Uyarı adı. |
| açıklama |E | |Uyarı açıklaması. |
| Koşul türü |E |Ölçüm, olay |İki uyarı türleri desteklenir: ölçüm ve olay. Ölçüm uyarıları, bir ölçüm koşuluyla ilgili temel alır. Olay uyarıları bir etkinlik günlüğü olayında temel alır. Uyarı bir ölçüm veya olaya göre denetlemek için bu değeri kullanın. |
| koşul |E | |Denetlenecek belirli alanları temel **koşul türü** değeri. |
| metricName |Ölçüm uyarıları | |Ne kuralı izler tanımlayan ölçüm adı. |
| metricUnit |Ölçüm uyarıları |Bayt cinsinden BytesPerSecond, Count, CountPerSecond, yüzde, saniye |Ölçümde izin birimi. Bkz: [izin verilen değerler](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Ölçüm uyarıları | |Uyarıya neden ölçümü gerçek değeri. |
| Eşik |Ölçüm uyarıları | |Eşik değeri, uyarı etkinleştirilir. |
| pencereboyutu |Ölçüm uyarıları | |Süre, eşiğine dayalı uyarı etkinliğini izlemek için kullanılır. Değer 5 dakika ile 1 gün arasında olmalıdır. Değer ISO 8601 süre biçiminde olmalıdır. |
| timeAggregation |Ölçüm uyarıları |Ortalama, en son, maksimum, Minimum, yok, toplam |Toplanan veriler, zaman içinde nasıl birleştirilmelidir. Ortalama varsayılan değerdir. Bkz: [izin verilen değerler](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| işleci |Ölçüm uyarıları | |Geçerli ölçüm verileri için ayarlanan eşik ile karşılaştırmak için kullanılan işleç. |
| subscriptionId |E | |Azure abonelik kimliği |
| resourceGroupName |E | |Etkilenen kaynak kaynak grubunun adı. |
| resourceName |E | |Etkilenen kaynak kaynak adı. |
| Kaynak türü |E | |Etkilenen kaynak kaynak türü. |
| resourceId |E | |Etkilenen kaynak kaynak kimliği. |
| resourceRegion |E | |Bölgeyi veya etkilenen kaynak konumu. |
| portalLink |E | |Portal kaynak özet sayfasına doğrudan bir bağlantı. |
| properties |N |İsteğe bağlı |Olayla ilgili ayrıntıları içeren bir anahtar/değer çiftleri kümesi. Örneğin, `Dictionary<String, String>`. Özellikler alanı isteğe bağlıdır. Özel kullanıcı Arabirimi veya mantıksal uygulama tabanlı iş akışı, kullanıcı yükü geçirilebilir anahtar/değer çiftleri girebilirsiniz. Web kancası kendisi (olarak URI sorgu parametreleri) aracılığıyla özel özellikler Web kancası geri geçirmek için alternatif bir yolu var. |

> [!NOTE]
> Ayarlayabileceğiniz **özellikleri** kullanarak yalnızca alan [Azure İzleyici REST API'leri](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Sonraki adımlar
* Azure uyarıları ve Web kancaları videoda hakkında daha fazla bilgi [PagerDuty ile tümleştirerek Azure uyarıları](http://go.microsoft.com/fwlink/?LinkId=627080).
* Bilgi edinmek için nasıl [Azure uyarılarda Azure Otomasyon betikleri (runbook'lar) yürütmek](http://go.microsoft.com/fwlink/?LinkId=627081).
* Bilgi nasıl [Azure uyarıdan Twilio aracılığıyla SMS iletisi göndermek için bir mantıksal uygulama kullanmak](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Bilgi edinmek için nasıl [Azure uyarıdan bir Slack iletisi göndermek için bir mantıksal uygulama kullanma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Bilgi edinmek için nasıl [Azure uyarıdan bir Azure kuyruğuna bir ileti göndermek için bir mantıksal uygulama kullanma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).
