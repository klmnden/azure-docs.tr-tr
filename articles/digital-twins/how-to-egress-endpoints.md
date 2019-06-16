---
title: Çıkış ve Azure dijital İkizlerini uç noktaların | Microsoft Docs
description: Azure ile dijital İkizlerini uç noktaları oluşturma hakkında yönergeler.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: alinast
ms.openlocfilehash: 478fe1859dd9067e8097df0384657793602c1378
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071453"
---
# <a name="egress-and-endpoints"></a>Çıkış ve uç noktaları

Azure dijital İkizlerini *uç noktaları* bir kullanıcının Azure aboneliğinde bir ileti veya olay Aracısı temsil eder. Azure Event Hubs, Azure Event Grid ve Azure Service Bus konuları için olayları ve iletileri gönderilebilir.

Olayları, önceden tanımlanmış yönlendirme tercihlerini göre Uç noktalara yönlendirilir. Kullanıcılar geçidime hangi *olay türleri* her uç nokta alabilirsiniz.

Olaylar hakkında daha fazla bilgi için Yönlendirme ve olay türlerini başvurmak [yönlendirme olayları ve iletileri Azure dijital İkizlerini](./concepts-events-routing.md).

## <a name="events"></a>Olaylar

Olayları Azure ileti ve olay aracıları tarafından işlenmek IOT nesneler (örneğin, cihazlar ve algılayıcılar) tarafından gönderilir. Olaylar, şunlarla tanımlanır [Azure Event Grid olay şema başvurusu](../event-grid/event-schema.md).

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "subject": "ExtendedPropertyKey",
  "data": {
    "SpacesToNotify": [
      "3a16d146-ca39-49ee-b803-17a18a12ba36"
    ],
    "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
    "AccessType": "Create"
  },
  "eventType": "TopologyOperation",
  "eventTime": "2018-04-17T17:41:54.9400177Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Öznitelik | Tür | Açıklama |
| --- | --- | --- |
| id | string | Olayın benzersiz tanımlayıcısı. |
| subject | string | Yayımcı tarafından tanımlanan olay konu yolu. |
| data | object | Kaynak sağlayıcıya özel olay verileri. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |
| topic | string | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |

Event Grid olay şeması hakkında daha fazla bilgi için:

- Gözden geçirme [Azure Event Grid olay şema başvurusu](../event-grid/event-schema.md).
- Okuma [Azure EventGrid Node.js SDK'sı EventGridEvent başvuru](https://docs.microsoft.com/javascript/api/azure-eventgrid/eventgridevent?view=azure-node-latest).

## <a name="event-types"></a>Olay türleri

Olay türleri olay doğasını sınıflandırmak ve ayarlanan **eventType** alan. Kullanılabilir olay türleri tarafından aşağıdaki listede verilmiştir:

- TopologyOperation
- UdfCustom
- SensorChange
- SpaceChange
- DeviceMessage

Her bir olay türü için olay biçimlerini aşağıdaki alt bölümlerde daha ayrıntılı açıklanmaktadır.

### <a name="topologyoperation"></a>TopologyOperation

**TopologyOperation** grafiği değişiklikleri uygular. **Konu** özelliği, etkilenen nesne türünü belirtir. Aşağıdaki nesne türlerini, bu olay tetikleyebilir:

- Cihaz
- DeviceBlobMetadata
- DeviceExtendedProperty
- ExtendedPropertyKey
- ExtendedType
- Anahtar deposu
- Rapor
- RoleDefinition
- Algılayıcı
- SensorBlobMetadata
- SensorExtendedProperty
- Uzay
- SpaceBlobMetadata
- SpaceExtendedProperty
- SpaceResource
- SpaceRoleAssignment
- Sistem
- Kullanıcı
- UserBlobMetadata
- UserExtendedProperty

#### <a name="example"></a>Örnek

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "subject": "ExtendedPropertyKey",
  "data": {
    "SpacesToNotify": [
      "3a16d146-ca39-49ee-b803-17a18a12ba36"
    ],
    "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
    "AccessType": "Create"
  },
  "eventType": "TopologyOperation",
  "eventTime": "2018-04-17T17:41:54.9400177Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOPIC_NAME | Özelleştirilmiş, konu adı |

### <a name="udfcustom"></a>UdfCustom

**UdfCustom** kullanıcı tanımlı işlev tarafından (UDF) gönderilen bir olay.
  
> [!IMPORTANT]  
> Bu olay, açıkça UDF'yi gönderilmelidir.

#### <a name="example"></a>Örnek

```JSON
{
  "id": "568fd394-380b-46fa-925a-ebb96f658cce",
  "subject": "UdfCustom",
  "data": {
    "TopologyObjectId": "7c799bfc-1bff-4b9e-b15a-669933969d20",
    "ResourceType": "Space",
    "Payload": "\"Room is not available or air quality is poor\"",
    "CorrelationId": "568fd394-380b-46fa-925a-ebb96f658cce"
  },
  "eventType": "UdfCustom",
  "eventTime": "2018-10-02T06:50:15.198Z",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOPIC_NAME | Özelleştirilmiş, konu adı |

### <a name="sensorchange"></a>SensorChange

**SensorChange** telemetri değişikliklere dayalı bir algılayıcının durumu bir güncelleştirmedir.

#### <a name="example"></a>Örnek

```JSON
{
  "id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
  "subject": "SensorChange",
  "data": {
    "Type": "Classic",
    "DataType": "Motion",
    "Id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
    "Value": "False",
    "PreviousValue": "True",
    "EventTimestamp": "2018-04-17T17:46:15.4964262Z",
    "MessageType": "sensor",
    "Properties": {
      "ms-client-request-id": "c9e576b7-5eea-4f61-8617-92a57add5179",
      "ms-activity-id": "ct22YwXEGJ5u.605.0"
    }
  },
  "eventType": "SensorChange",
  "eventTime": "2018-04-17T17:46:18.5452993Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOPIC_NAME | Özelleştirilmiş, konu adı |

### <a name="spacechange"></a>SpaceChange

**SpaceChange** boşlukla ait durum telemetri değişikliklere dayalı bir güncelleştirmedir.

#### <a name="example"></a>Örnek

```JSON
{
  "id": "42522e10-b1aa-42ff-a5e7-7181788ffc4b",
  "subject": "SpaceChange",
  "data": {
    "Type": null,
    "DataType": "AvailableAndFresh",
    "Id": "7c799bfc-1bff-4b9e-b15a-669933969d20",
    "Value": "Room is not available or air quality is poor",
    "PreviousValue": null,
    "RawData": null,
    "transactionId": null,
    "EventTimestamp": null,
    "MessageType": null,
    "Properties": null,
    "CorrelationId": "42522e10-b1aa-42ff-a5e7-7181788ffc4b"
  },
  "eventType": "SpaceChange",
  "eventTime": "2018-10-02T06:50:20.128Z",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Değer | Şununla değiştir |
| --- | --- |
| YOUR_TOPIC_NAME | Özelleştirilmiş, konu adı |

### <a name="devicemessage"></a>DeviceMessage

Kullanarak **DeviceMessage**, belirtebileceğiniz bir **EventHub** için ham telemetri olayları yönlendirilir de Azure dijital İkizlerini bağlantı.

> [!NOTE]
> - **DeviceMessage** combinable ile yalnızca olan **EventHub**. Birleştirmeniz mümkün olmayacaktır **DeviceMessage** herhangi bir olay türü.
> - Tek bir uç nokta türü bileşimi belirtebilirsiniz **EventHub** veya **DeviceMessage**.

## <a name="configure-endpoints"></a>Uç noktaları yapılandırma

Uç nokta yönetim uç noktalarını API aracılığıyla Uygula.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

Aşağıdaki örnekler, desteklenen uç noktalar yapılandırmak nasıl ekleyebileceğiniz gösterilmektedir.

>[!IMPORTANT]
> Dikkatli dikkat **eventTypes** özniteliği. Hangi olay türleri bitiş noktası tarafından işlenir ve bu nedenle, akışı belirler ve tanımlar.

Kimliği doğrulanmış bir HTTP POST isteği

```plaintext
YOUR_MANAGEMENT_API_URL/endpoints
```

- Service Bus olay türleri için rota **SensorChange**, **SpaceChange**, ve **TopologyOperation**:

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME"
  }
  ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_NAMESPACE | Uç noktanız için ad alanı |
    | YOUR_PRIMARY_KEY | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | YOUR_SECONDARY_KEY | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | YOUR_TOPIC_NAME | Özelleştirilmiş, konu adı |

- Event Grid olay türleri için rota **SensorChange**, **SpaceChange**, ve **TopologyOperation**:

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME.westus-1.eventgrid.azure.net"
  }
  ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_PRIMARY_KEY | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi|
    | YOUR_SECONDARY_KEY | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | YOUR_TOPIC_NAME | Özelleştirilmiş, konu adı |

- Event Hubs olay türleri için rota **SensorChange**, **SpaceChange**, ve **TopologyOperation**:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_NAMESPACE | Uç noktanız için ad alanı |
    | YOUR_PRIMARY_KEY | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | YOUR_SECONDARY_KEY | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | YOUR_EVENT_HUB_NAME | Olay hub'ınızın adı |

- Event Hubs olay türü için rota **DeviceMessage**. Eklenmesi, `EntityPath` içinde **connectionString** zorunludur:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Değer | Şununla değiştir |
    | --- | --- |
    | YOUR_NAMESPACE | Uç noktanız için ad alanı |
    | YOUR_PRIMARY_KEY | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | YOUR_SECONDARY_KEY | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | YOUR_EVENT_HUB_NAME | Olay hub'ınızın adı |

> [!NOTE]  
> Yeni bir uç nokta oluşturulduktan sonra en fazla 5 uç noktasında olayları almaya başlaması için 10 dakika sürebilir.

## <a name="primary-and-secondary-connection-keys"></a>Birincil ve ikincil bağlantı anahtarları

Birincil bağlantı anahtar yetkisiz duruma geldiğinde, sistem otomatik olarak ikincil bağlantı anahtar çalışır. Bir yedekleme sağlar ve düzgün bir şekilde kimlik doğrulaması ve API uç noktaları aracılığıyla birincil anahtarı güncelleştirmek olanağı sağlar.

Birincil ve ikincil bağlantı anahtarları yetkisiz ise sistem 30 dakikaya kadar bir üstel geri alma bekleme süresini girer. Olayları, geri alma tetiklenen bekleme her seferinde bırakılır.

Sistem olduğunda bekleme durumu bir geri alma, güncelleştirme bağlantıları anahtarları uç API'sinden etkili 30 dakika kadar sürebilir.

## <a name="unreachable-endpoints"></a>Erişilemeyen uç noktaları

Bir uç nokta ulaşılamaz duruma gelmediği sistem 30 dakikaya kadar bir üstel geri alma bekleme süresini girer. Olayları, geri alma tetiklenen bekleme her seferinde bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi [Azure dijital Swagger İkizlerini kullanma](how-to-use-swagger.md).

- Daha fazla bilgi edinin [yönlendirme olayları ve iletileri](concepts-events-routing.md) Azure dijital İkizlerini içinde.
