---
title: Azure Event Grid Service Bus olay şeması
description: Service Bus olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: babanisa
ms.openlocfilehash: f44d2c1c5be6ac895b6f5ea9feca29c0f8ed09f3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561770"
---
# <a name="azure-event-grid-event-schema-for-service-bus"></a>Service Bus için Azure Event Grid olay şeması

Bu makale, Service Bus olayları için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

Örnek betikler ve öğreticiler listesi için bkz: [Service Bus olay kaynağı](event-sources.md#service-bus).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Service Bus, aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners | Bir kuyruk veya abonelik ve dinleme gerçekleştirmiyorsa etkin ileti olduğunda oluşturulur. |
| Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener | Sahipsiz sıra ve hiçbir etkin dinleyiciler etkin iletileriniz olduğunda oluşturulur. |

## <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, olay dinleyicileri ile etkin iletileriniz şemasını gösterir:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Sahipsiz Sıra olayın şeması benzer:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| topic | string | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| subject | string | Yayımcı tarafından tanımlanan olay konu yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | string | Olayın benzersiz tanımlayıcısı. |
| data | object | BLOB Depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| namespaceName | string | Service Bus ad alanı kaynak bulunmaktadır. |
| requestUri | string | Belirli bir kuyruk veya olay yayma abonelik URI'si. |
| entityType | string | Olayları (kuyruk veya abonelik) yayınlama Service Bus varlık türü. |
| queueName | string | Bir kuyruğa abone olursa active iletilerle kuyruk. Konuları kullanıyorsanız null değer / abonelikler. |
| topicName | string | Konu etkin iletileriniz ile Service Bus aboneliği aittir. Bir kuyruk kullanma değilse null değeri. |
| subscriptionName | string | Etkin iletiler ile Service Bus aboneliği. Bir kuyruk kullanma değilse null değeri. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
* Service Bus ile Azure Event Grid kullanma hakkında daha fazla bilgi için bkz [Service Bus-Event Grid tümleştirmesine genel bakış](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md).
* Deneyin [işlevler veya Logic Apps ile Service Bus olayları alma](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json).
