---
title: Çıkış ve Azure dijital İkizlerini uç noktaların | Microsoft Docs
description: Azure ile dijital İkizlerini uç noktaları oluşturma yönergeleri
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: c94d29f16c011a9ff9951d064d7496d3a87f70ef
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636314"
---
# <a name="egress-and-endpoints"></a>Çıkış ve uç noktaları

Azure dijital İkizlerini kavramını destekler **uç noktaları**. Her uç nokta, kullanıcının Azure aboneliğinde bir ileti veya olay Aracısı temsil eder. Azure Event Hubs, Azure Event Grid ve Azure Service Bus konuları için olayları ve iletileri gönderilebilir.

Olaylar, önceden tanımlanmış yönlendirme tercihlerini göre uç noktalarına gönderilir. Kullanıcı, hangi uç noktaya aşağıdaki olaylardan herhangi biri alması gereken belirtebilirsiniz: 

- TopologyOperation
- UdfCustom
- SensorChange
- SpaceChange
- DeviceMessage

Yönlendirme olayları ve olay türleri temel anlamak için bkz [yönlendirme olayları ve iletileri](concepts-events-routing.md).

## <a name="event-types-description"></a>Olay türleri açıklaması

Olay biçimlerini olay türlerinin her biri için aşağıdaki bölümlerde açıklanmıştır.

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

Uç nokta yönetim uç noktalarını API aracılığıyla Uygula. Aşağıdaki örnekler, farklı desteklenen uç noktalar yapılandırmak nasıl ekleyebileceğiniz gösterilmektedir. Yönlendirme için uç nokta tanımlar gibi özel olay türleri dizisi dikkat edin:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
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
