---
title: Çıkış ve Azure dijital İkizlerini uç noktaların | Microsoft Docs
description: Azure ile dijital İkizlerini uç noktaları oluşturma Kılavuzu
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: c09ee84cda5f0a9747d3ee1f8f1b37d1323f2cc2
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212259"
---
# <a name="egress-and-endpoints"></a>Çıkış ve uç noktaları

Azure dijital İkizlerini kavramını destekler _uç noktaları_ her uç nokta kullanıcının Azure aboneliğinde bir ileti/olay Aracısı burada temsil eder. Olayları ve iletileri göndermenin **olay hub'ı**, **Event Grid**, ve **hizmet veri yolu konuları**.

Olaylar, önceden tanımlı yönlendirme tercihlerini göre uç noktalarına gönderilir: kullanıcı, hangi uç noktaya aşağıdaki olaylardan herhangi biri alması gereken belirtebilirsiniz: **TopologyOperation**, **UdfCustom**, **SensorChange**, **SpaceChange**, veya **DeviceMessage**.

Yönlendirme olayları ve olay türleri temel anlamak için bkz [yönlendirme olayları ve iletileri](concepts-events-routing.md).

## <a name="event-types-description"></a>Olay türleri açıklaması

Olay türlerinin her biri için olay biçimler şunlardır:

- **TopologyOperation**

  Graf değişiklikleri uygular. *Konu* özelliği, etkilenen nesne türünü belirtir. Bu olay tetikleyebilir nesne türleri şunlardır: **cihaz**, **DeviceBlobMetadata**, **DeviceExtendedProperty**, **ExtendedPropertyKey**, **ExtendedType**, **KeyStore**, **rapor**, **RoleDefinition**, **algılayıcısı**, **SensorBlobMetadata**, **SensorExtendedProperty**, **alanı**, **SpaceBlobMetadata**,  **SpaceExtendedProperty**, **SpaceResource**, **SpaceRoleAssignment**, **sistem**, **kullanıcı**, **UserBlobMetadata**, **UserExtendedProperty**.

  Örnek:

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
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Özel öznitelik adı | Değiştirin |
    | --- | --- |
    | *yourTopicName* | Özelleştirilmiş, konu adı |

- **UdfCustom**

  Kullanıcı tanımlı işlev tarafından (UDF) gönderilen bir olay. 
  
  > [!IMPORTANT]
  > Bu olay UDF'yi açıkça gönderilmesi gerekir.

  Örnek:

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
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Özel öznitelik adı | Değiştirin |
    | --- | --- |
    | *yourTopicName* | Özelleştirilmiş, konu adı |

- **SensorChange**

  Bir algılayıcının durumu telemetri değişikliklere dayalı bir güncelleştirme.

  Örnek:

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
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Özel öznitelik adı | Değiştirin |
    | --- | --- |
    | *yourTopicName* | Özelleştirilmiş, konu adı |

- **SpaceChange**

  Telemetri değişikliklere dayalı bir alanı'nın durumunu güncelleştirme.

  Örnek:

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
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Özel öznitelik adı | Değiştirin |
    | --- | --- |
    | *yourTopicName* | Özelleştirilmiş, konu adı |

- **DeviceMessage**

  Belirtmenizi sağlar bir **EventHub** için ham telemetri olayları yönlendirilir de Azure dijital İkizlerini bağlantı.

> [!NOTE]
> - **DeviceMessage** combinable ile yalnızca olan **EventHub**; birleştirmek mümkün olmayacaktır **DeviceMessage** herhangi bir olay türü.
> - Tek bir uç nokta türü bileşimi belirlemek mümkün olacaktır **EventHub** veya **DeviceMessage**.

## <a name="configuring-endpoints"></a>Uç noktalarını yapılandırma

Uç nokta yönetim uç noktalarını API aracılığıyla Uygula. Farklı desteklenen uç noktalarını yapılandırma hakkında bazı örnekleri aşağıda verilmiştir. Yönlendirme için uç nokta tanımlar gibi özel olay türleri dizisi dikkat edin:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
```

- Yönlendirme **Service Bus** olay türleri: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourTopicName"
  }
  ```

    | Özel öznitelik adı | Değiştirin |
    | --- | --- |
    | *AdAlanınız* | Uç noktanız için ad alanı |
    | *yourPrimaryKey* | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | *yourSecondaryKey* | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | *yourTopicName* | Özelleştirilmiş, konu adı |

- Yönlendirme **Event Grid** olay türleri: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "yourPrimaryKey",
    "secondaryConnectionString": "yourSecondaryKey",
    "path": "yourTopicName.westus-1.eventgrid.azure.net"
  }
  ```

    | Özel öznitelik adı | Değiştirin |
    | --- | --- |
    | *yourPrimaryKey* | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi|
    | *yourSecondaryKey* | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | *yourTopicName* | Özelleştirilmiş, konu adı |

- Yönlendirme **olay hub'ı** olay türleri: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourEventHubName"
  }
  ```

    | Özel öznitelik adı | Değiştirin |
    | --- | --- |
    | *AdAlanınız* | Uç noktanız için ad alanı |
    | *yourPrimaryKey* | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | *yourSecondaryKey* | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | *yourEventHubName* | Adı, **olay hub'ı** |

- Yönlendirme **olay hub'ı** olay türü: **DeviceMessage**. Eklenmesi, `EntityPath` içinde **connectionString** zorunludur.

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey;EntityPath=yourEventHubName",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey;EntityPath=yourEventHubName",
    "path": "yourEventHubName"
  }
  ```

    | Özel öznitelik adı | Değiştirin |
    | --- | --- |
    | *AdAlanınız* | Uç noktanız için ad alanı |
    | *yourPrimaryKey* | Kimlik doğrulaması için kullanılan birincil bağlantı dizesi |
    | *yourSecondaryKey* | Kimlik doğrulaması için kullanılan ikincil bağlantı dizesi |
    | *yourEventHubName* | Adı, **olay hub'ı** |

> [!NOTE]
> Yeni bir uç nokta oluşturulduktan sonra en fazla 5 uç noktasında olayları almaya başlaması için 10 dakika sürebilir.

## <a name="primary-and-secondary-connection-keys"></a>Birincil ve ikincil bağlantı anahtarları

Birincil bağlantı anahtar yetkisiz duruma geldiğinde, sistem otomatik olarak ikincil bağlantı anahtar çalışır. Bir yedekleme sağlar ve düzgün bir şekilde kimlik doğrulaması ve API uç noktaları aracılığıyla birincil anahtarı güncelleştirmek olanağı sağlar.

Birincil ve ikincil bağlantı anahtarları yetkisiz, sistemin bir üstel geri alma bekleme süresi en fazla 30 dakika girer. Olayları, geri alma tetiklenen bekleme her seferinde bırakılır.

Sistem olduğunda bekleme durumu bir geri alma, güncelleştirme bağlantıları anahtarları uç API'sinden etkili 30 dakika kadar sürebilir.

## <a name="unreachable-endpoints"></a>Erişilemeyen uç noktaları

Bir uç nokta ulaşılamaz duruma gelmediği, sistemin bir üstel geri alma bekleme süresi en fazla 30 dakika girer. Olayları, geri alma tetiklenen bekleme her seferinde bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini Swagger kullanmayı öğrenmek için [Azure İkizlerini dijital Swagger](how-to-use-swagger.md).

Yönlendirme iletilerini Azure dijital çiftleri ve olaylar hakkında daha fazla bilgi edinmek için [yönlendirme olayları ve iletileri](concepts-events-routing.md).
