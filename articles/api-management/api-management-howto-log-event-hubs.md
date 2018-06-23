---
title: Azure Event hubs'a Azure API Management olaylarını günlüğe kaydedecek şekilde nasıl | Microsoft Docs
description: Azure Event hubs'a Azure API Management'te olayları günlüğe kaydetme hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 88f6507d-7460-4eb2-bffd-76025b73f8c4
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: apimpm
ms.openlocfilehash: 2334aefdfb442054226ef6d7d55a8c097a433565
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316332"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Azure Event hubs'a Azure API Management'te olayları günlüğe kaydetme hakkında
Azure Event Hubs, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki verileri işleyip analiz edebilmeniz için saniye başına milyonlarca olayı işleyebilen ileri düzeyde ölçeklenebilir bir veri alım sistemidir. Event Hubs bir olay komut zincirinin "ön kapı" olarak görev yapan ve veriler bir event hub'ına toplandıktan sonra dönüştürülebilir ve tüm gerçek zamanlı analiz sağlayıcısı veya toplu işlem/depolama bağdaştırıcısı kullanılarak saklanır. Event Hubs olay akışı üretimlerini bu olayların tüketilmesinden ayırır, böylece olay tüketicileri olaylara kendi zamanlamalarında erişebilir.

Bu makalede bir yardımcı olan [olay hub'ları ile Azure API Management tümleştirmek](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video ve Azure Event Hubs kullanarak API Management olaylarını günlüğe kaydedecek şekilde açıklar.

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub’ı oluşturma

Bir olay hub'ı oluşturma ve olay hub'ı gelen ve giden olayları alıp göndermek için gereken bağlantı dizeleri alma hakkında ayrıntılı adımlar için bkz: [bir olay hub'ları ad alanı oluşturup Azure portalını kullanarak bir event hub](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-api-management-logger"></a>Bir API Management Günlükçü oluşturma
Bir Event Hub sahip olduğunuza göre sonraki adıma yapılandırmaktır bir [Günlükçü](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) , API Management hizmeti bu olayları Event Hub'ına oturum açabilir.

API Management günlükçüleri kullanılarak yapılandırılmış olan [API Management REST API](http://aka.ms/smapi). REST API ilk kez kullanmadan önce gözden [Önkoşullar](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#Prerequisites) ve olduğundan emin olun [REST API erişim etkin](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

Günlükçü oluşturmak için aşağıdaki URL şablonu kullanarak bir HTTP PUT İsteği olun:

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2017-03-01`

* Değiştir `{your service}` API Management hizmet örneği adı.
* Değiştir `{new logger name}` yeni Günlükçü için istenen adı ile. Yapılandırdığınızda bu adına başvuruda [günlük eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) İlkesi

Aşağıdaki üst bilgiler isteği ekleyin:

* Content-Type: uygulama/json
* Yetkilendirme: SharedAccessSignature 58...
  * Oluşturma yönergeleri için `SharedAccessSignature` bkz [Azure API Management REST API kimlik doğrulama](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Aşağıdaki şablonu kullanarak istek gövdesini belirtin:

```json
{
  "loggerType" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the Azure Classic Portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `loggerType` ayarlanmalıdır `AzureEventHub`.
* `description` günlükçünün isteğe bağlı bir açıklama sağlar ve isterseniz sıfır uzunluğunda bir dize olabilir.
* `credentials` içeren `name` ve `connectionString` Azure olay hub'ınızın.

Yaptığınızda isteği durum kodu Günlükçü oluşturduysanız `201 Created` döndürülür.

> [!NOTE]
> Diğer olası dönüş kodları ve bunların nedenleri için bkz: [Günlükçü oluşturma](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). Liste, güncelleştirme ve silme gibi diğer işlemler gerçekleştirme hakkında bilgi için bkz: [Günlükçü](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) varlık belgeleri.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Günlük eventhubs ilkeleri yapılandırma

API Management'te, Günlükçü yapılandırıldıktan sonra günlük eventhubs ilkelerinizi istenen olaylarını günlüğe kaydedecek şekilde yapılandırabilirsiniz. Günlük eventhubs ilkesi gelen ilke bölümünü veya giden ilke bölümünü kullanılabilir.

1. APIM örneğinize göz atın.
2. API sekmesini seçin.
3. İlke eklemek istediğiniz API seçin. Bu örnekte, biz bir ilkeye eklediğiniz **Echo API'si** içinde **sınırsız** ürün.
4. **Tüm işlemler**’i seçin.
5. Ekranın üst kısmında Tasarım sekmesini seçin.
6. Gelen veya giden işlem penceresinde üçgen (yanındaki kalem) tıklatın.
7. Kod Düzenleyicisi'ni seçin. Daha fazla bilgi için bkz: [nasıl ayarlayacağınız veya ilkeleri düzenleme](set-edit-policies.md).
8. İmleç Konumlandır `inbound` veya `outbound` ilke bölümü.
9. Sağdaki penceresinde seçin **ilkeleri Gelişmiş** > **EventHub günlüğüne**. Bu ekler `log-to-eventhub` ilke deyimi şablonu.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
Değiştir `logger-id` önceki adımda yapılandırdığınız API Management günlükçü adı.

Değeri olarak bir dize döndürür. herhangi bir ifade kullanabileceğiniz `log-to-eventhub` öğesi. Bu örnekte, tarih ve saat, hizmet adı, istek kimliği, istek IP adresi ve işlem adı içeren bir dize günlüğe kaydedilir.

Tıklatın **kaydetmek** güncelleştirilmiş ilke yapılandırmasını kaydetmek için. Kaydedilmeden hemen ilkesi etkindir ve belirlenen olay Hub'ına olayları kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Event Hubs hakkında daha fazla bilgi edinin
  * [Azure Event Hubs ile çalışmaya başlama](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost bulunan iletiler alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Management ve Event Hubs ile tümleştirme hakkında daha fazla bilgi edinin
  * [Günlükçü varlık başvurusu](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [Günlük eventhub ilke başvurusu](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Azure API Management, olay hub'ları ve Runscope Apı'lerinizi izleme](api-management-log-to-eventhub-sample.md)  
* Daha fazla bilgi edinmek [Azure Application Insights ile tümleştirme](api-management-howto-app-insights.md)

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
