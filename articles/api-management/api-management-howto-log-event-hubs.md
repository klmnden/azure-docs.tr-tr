---
title: Azure Event hubs'a, Azure API Management'ta olayları günlüğe nasıl | Microsoft Docs
description: Azure Event hubs'a, Azure API Management'ta olayları günlüğe kaydetme hakkında bilgi edinin.
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
ms.openlocfilehash: 14f84b5380a1c106114cdab425de7f69f4e19825
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58793569"
---
# <a name="how-to-log-events-to-azure-event-hubs-in-azure-api-management"></a>Azure Event hubs'a, Azure API Management'ta olayları günlüğe kaydetme hakkında
Azure Event Hubs, bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki verileri işleyip analiz edebilmeniz için saniye başına milyonlarca olayı işleyebilen ileri düzeyde ölçeklenebilir bir veri alım sistemidir. Event Hubs bir olay işlem hattının "ön kapısı" olarak görev yapar ve veriler bir olay hub'ına toplandıktan sonra dönüştürülebilir ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işleme/depolama bağdaştırıcısı kullanılarak depolanabilir. Event Hubs olay akışı üretimlerini bu olayların tüketilmesinden ayırır, böylece olay tüketicileri olaylara kendi zamanlamalarında erişebilir.

Bu makale için bir yardımcı olan [Event Hubs ile Azure API Management tümleştirme](https://azure.microsoft.com/documentation/videos/integrate-azure-api-management-with-event-hubs/) video ve Azure Event Hubs'ı kullanarak API Management olay günlüğünün nasıl tutulduğunu açıklar.

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub’ı oluşturma

Bir olay hub'ı oluşturma ve olay hub'ı gelen ve olayları alıp göndermek için gereken bağlantı dizelerini almak ayrıntılı adımları için bkz: [bir Event Hubs ad alanı ve Azure portalını kullanarak bir olay hub'ı oluşturma](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

## <a name="create-an-api-management-logger"></a>Bir API Management Günlükçü oluşturma
Bir olay hub'ı olduğuna göre sonraki adıma yapılandırmaktır bir [Günlükçü](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) API Yönetimi'nde hizmet, olayları olay Hub'ına oturum açabilir.

API Management günlükçüleri kullanarak yapılandırılmış [API Management REST API](https://aka.ms/smapi). REST API ilk kez kullanmadan önce gözden [önkoşulları](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest) ve sahip olduğunuzdan emin olun [REST API erişimi etkinleştirilmiş](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest#EnableRESTAPI).

Günlükçü oluşturmak için aşağıdaki URL şablonu kullanarak bir HTTP PUT İsteği olun:

`https://{your service}.management.azure-api.net/loggers/{new logger name}?api-version=2017-03-01`

* Değiştirin `{your service}` API Management hizmet örneğinizin adı.
* Değiştirin `{new logger name}` , yeni bir Günlükçü için istenen ad ile. Yapılandırdığınızda bu ada başvuru [günlük eventhub](/azure/api-management/api-management-advanced-policies#log-to-eventhub) İlkesi

Aşağıdaki üst bilgileri isteğe ekleyin:

* İçerik türü: application/json
* Yetkilendirme: SharedAccessSignature 58...
  * Oluşturma yönergeleri için `SharedAccessSignature` bkz [Azure API Management REST API kimlik doğrulaması](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-authentication).

Aşağıdaki şablonu kullanarak istek gövdesini belirtin:

```json
{
  "loggerType" : "AzureEventHub",
  "description" : "Sample logger description",
  "credentials" : {
    "name" : "Name of the Event Hub from the portal",
    "connectionString" : "Endpoint=Event Hub Sender connection string"
    }
}
```

* `loggerType` ayarlanmalıdır `AzureEventHub`.
* `description` günlükçünün isteğe bağlı bir açıklama sunar ve istenen sıfır uzunlukta bir dize olabilir.
* `credentials` içeren `name` ve `connectionString` Azure olay hub'ınızın.

Yaptığınızda, istek Günlükçü oluşturuldu, durum kodu `201 Created` döndürülür. Yukarıdaki örnek isteği temel alarak bir örnek yanıt aşağıda gösterilmiştir.

```json
{
    "id": "/loggers/{new logger name}",
    "loggerType": "azureEventHub",
    "description": "Sample logger description",
    "credentials": {
        "name": "Name of the Event Hub from the Portal",
        "connectionString": "{{Logger-Credentials-xxxxxxxxxxxxxxx}}"
    },
    "isBuffered": true,
    "resourceId": null
}
```

> [!NOTE]
> Diğer olası dönüş kodları ve bunların nedenleriyle bkz [bir Günlükçü oluşturma](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity#PUT). Liste, güncelleştirme ve silme gibi diğer işlemlerini nasıl gerçekleştireceğinizi görmek için bkz: [Günlükçü](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity) entity belgeleri.
>
>

## <a name="configure-log-to-eventhubs-policies"></a>Günlük eventhubs ilkelerini yapılandırma

API Yönetimi'nde, Günlükçü yapılandırıldıktan sonra günlük eventhubs ilkelerinizi istenen olaylarını günlüğe kaydedecek şekilde yapılandırabilirsiniz. Günlük eventhubs ilke, gelen bir ilke bölümü veya giden bir ilke bölümü içinde kullanılabilir.

1. APIM örneğinize göz atın.
2. API sekmesini seçin.
3. İlkeyi eklemek istediğiniz API'yi seçin. Bu örnekte, bir ilkesi ekliyoruz **Echo API'si** içinde **sınırsız** ürün.
4. **Tüm işlemler**’i seçin.
5. Ekranın üst kısmında, Tasarım sekmesini seçin.
6. Gelen veya giden işleme penceresinde (kalemin yanındaki) üçgene tıklayın.
7. Kod Düzenleyicisi'ni seçin. Daha fazla bilgi için [ilkeleri ayarlama veya düzenleme nasıl](set-edit-policies.md).
8. İmlecinizi, `inbound` veya `outbound` ilke bölümü.
9. Sağdaki pencerede seçin **Gelişmiş İlkeler** > **EventHub günlüğüne**. Bu ekler `log-to-eventhub` ilke deyimi şablonu.

```xml
<log-to-eventhub logger-id ='logger-id'>
  @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name))
</log-to-eventhub>
```
Değiştirin `logger-id` için kullandığınız değerle `{new logger name}` Günlükçü önceki adımda oluşturmak için URL.

Bir dize değeri olarak döndüren herhangi bir ifadeyi kullanabilirsiniz `log-to-eventhub` öğesi. Bu örnekte, tarih ve saat, hizmet adı, istek kimliği, istek IP adresi ve işlem adı içeren bir dize günlüğe kaydedilir.

Tıklayın **Kaydet** güncelleştirilmiş ilke yapılandırmasını kaydetmek için. Kaydedildikten hemen sonra ilkeyi etkin ve belirlenen olay Hub'ına olayları günlüğe kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Event Hubs hakkında daha fazla bilgi edinin
  * [Azure Event Hubs ile çalışmaya başlama](../event-hubs/event-hubs-c-getstarted-send.md)
  * [EventProcessorHost bulunan iletiler alma](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md)
  * [Event Hubs programlama kılavuzu](../event-hubs/event-hubs-programming-guide.md)
* API Management ve olay hub'ları ile tümleştirme hakkında daha fazla bilgi edinin
  * [Günlükçü varlık başvurusu](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-logger-entity)
  * [Günlük eventhub ilke başvurusu](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#log-to-eventhub)
  * [Azure API Management, Event hubs'ı ve Moesif API'leri izleme](api-management-log-to-eventhub-sample.md)  
* Daha fazla bilgi edinin [Azure Application Insights ile tümleştirme](api-management-howto-app-insights.md)

[publisher-portal]: ./media/api-management-howto-log-event-hubs/publisher-portal.png
[create-event-hub]: ./media/api-management-howto-log-event-hubs/create-event-hub.png
[event-hub-connection-string]: ./media/api-management-howto-log-event-hubs/event-hub-connection-string.png
[event-hub-dashboard]: ./media/api-management-howto-log-event-hubs/event-hub-dashboard.png
[receiving-policy]: ./media/api-management-howto-log-event-hubs/receiving-policy.png
[sending-policy]: ./media/api-management-howto-log-event-hubs/sending-policy.png
[event-hub-policy]: ./media/api-management-howto-log-event-hubs/event-hub-policy.png
[add-policy]: ./media/api-management-howto-log-event-hubs/add-policy.png
