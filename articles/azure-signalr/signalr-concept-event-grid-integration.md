---
title: Azure SignalR hizmeti olaylara tepki verme
description: Azure SignalR hizmeti olaylarına abone olmak için Azure Event grid'i kullanın.
services: azure-signalr,event-grid
author: chenyl
ms.author: chenyl
ms.reviewer: zhshang
ms.date: 06/12/2019
ms.topic: conceptual
ms.service: azure-signalr
ms.openlocfilehash: 02f88c5953d499b30f2ea3408318f70a72f42b0f
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789195"
---
# <a name="reacting-to-azure-signalr-service-events"></a>Azure SignalR hizmeti olaylara tepki verme

Azure SignalR hizmeti olaylarını uygulamaları modern sunucusuz mimarileri kullanarak bağlantısı kesilmiş veya bağlı istemci bağlantıları için tepki vermek izin verin. Bunu karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gerek kalmadan yapar.  Bunun yerine, olayların gönderilmesini [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) gibi abonelere [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), ve hatta kendi özel http dinleyicisi ve yalnızca kullandığınız kadarı için ödeme yaparsınız.

Azure SignalR hizmeti olaylarını güvenilir bir şekilde zengin bir deneyimle uygulamalarınızda güvenilir teslim hizmetleri yeniden deneme ilkeleri ve teslim edilemeyen sağlayan Event Grid hizmetine gönderilir. Daha fazla bilgi için bkz. [Event Grid iletiyi teslim ve yeniden deneme](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

![Event Grid modeli](https://docs.microsoft.com/azure/event-grid/media/overview/functional-model.png)

## <a name="serverless-state"></a>Sunucusuz durumu
Azure SignalR hizmeti olaylarını etkin için istemci bağlantıları sunucusuz durumdadır. Bir istemci bir hub sunucusuna yönlendirmek değil, genel olarak bakıldığında, sunucusuz durumuna geçtiğinde. Yalnızca istemci bağlantıları bağlandığı hub, hub sunucusu yoksa Klasik modu iş. Ancak, bazı sorundan kaçınmak için sunucusuz modu önerilir. Bilgi hizmeti modu hakkında daha fazla ayrıntı için bkz [hizmeti modu seçme](https://github.com/Azure/azure-signalr/blob/dev/docs/faq.md#what-is-the-meaning-of-service-mode-defaultserverlessclassic-how-can-i-choose).

## <a name="available-azure-signalr-service-events"></a>Azure SignalR hizmeti olayları kullanılabilir
Event grid'i kullanır [olay abonelikleri](../event-grid/concepts.md#event-subscriptions) abonelere olay iletileri yönlendirmek için. Azure SignalR hizmeti olay abonelikleri iki olay türlerini destekler:  

|Olay adı|Açıklama|
|----------|-----------|
|`Microsoft.SignalRService.ClientConnectionConnected`|İstemci bağlantısı bağlı olduğunda oluşturulur.|
|`Microsoft.SignalRService.ClientConnectionDisconnected`|İstemci bağlantısı kesildiğinde oluşturulur.|

## <a name="event-schema"></a>Olay şeması
Azure SignalR hizmeti olaylarını verilerinizdeki değişiklikleri yanıtlamak için gereken tüm bilgileri içerir. Azure SignalR hizmeti olay eventType özelliği ile başlar "Microsoft.SignalRService" ile tanımlayabilirsiniz. Event Grid olay özelliklerinin kullanımı hakkında ek bilgi konumunda belgelenmiştir [Event Grid olay şeması](../event-grid/event-schema.md).  

İstemci bağlantı bağlı olayının bir örnek aşağıda verilmiştir:
```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Daha fazla bilgi için [SignalR hizmet olaylar şeması](../event-grid/event-schema-azure-signalr.md).

## <a name="next-steps"></a>Sonraki adımlar

Event Grid hakkında daha fazla bilgi edinin ve Azure SignalR hizmeti olaylarını deneyin:

> [!div class="nextstepaction"]
> [Azure SignalR hizmeti ile bir örnek Event Grid tümleştirmesini deneyin](./signalr-howto-event-grid-integration.md)
> [Event Grid hakkında](../event-grid/overview.md)
