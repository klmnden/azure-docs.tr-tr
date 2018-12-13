---
title: Azure Event Hubs ve Azure Service Bus verilerinizi dışarı aktarma | Microsoft Docs
description: Azure Event Hubs'a ve Azure Service Bus için Azure IOT Central uygulamanızdan veri dışarı aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 14b51f109ca76661ac10c99d42002dda45bc0500
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318714"
---
# <a name="export-your-data-in-azure-iot-central"></a>Azure IOT Central verilerinizi dışarı aktarma

*Bu konu, Yöneticiler için geçerlidir.*

Bu makalede daha ayrıntılı sürekli veri dışarı aktarma özelliği verilerinizi kendi değerlerinizle dışarı aktarmak için Azure IOT Central içinde kullanmak nasıl türlerine geçiyor **Azure Event Hubs**, ve **Azure Service Bus** örnekleri. Dışarı aktarabilirsiniz **ölçümleri**, **cihazları**, ve **cihaz şablonları** Orta Gecikmeli kanaldan öngörü ve analizler için kendi hedef. Bu, Azure Stream analytics'te özel kurallar tetikleme, Azure Logic apps'te özel iş akışları tetikleyen veya veri dönüştürme ve Azure işlevleri aracılığıyla iletmeden içerir. 

> [!Note]
> Bir kez daha, verileri sürekli dışarı aktarma üzerinde etkinleştirdiğinizde, ileriye doğru o andan itibaren yalnızca verileri alın. Şu anda, verileri sürekli dışarı aktarma kapalıydı ne zaman bir kez verileri alınamıyor. Daha fazla geçmiş verileri korumak için verileri sürekli dışarı aktarma üzerinde erken açın.


## <a name="prerequisites"></a>Önkoşullar

- IOT Central uygulamanızda yönetici olmanız gerekir

## <a name="export-to-azure-event-hubs-and-azure-service-bus"></a>Azure Event Hubs'a ve Azure hizmet veri yolu dışarı aktarma

Olay hub'ı veya Service Bus kuyruğuna veya konusuna neredeyse gerçek zamanlı ölçümler, cihazları ve cihaz şablonları veri aktarılır. Dışarı aktarılan ölçümleri veri iletinin tamamen IOT Central için gönderilen cihazlarınızı yalnızca ölçüleri değerlerini içerir. Dışarı aktarılan cihazlar veri değişiklikleri tüm cihazların ayarları ve özellikleri içerir ve dışarı aktarılan cihaz şablonları tüm cihaz şablonlarına yapılan değişiklikleri içerir. Dışarı aktarılan veriler "body" özelliğine ve JSON biçimindedir.

> [!NOTE]
> Service Bus, kuyruklar ve konular dışa aktarma hedefi seçerken **oturumları veya yinelenen algılama etkin olmamalıdır**. Bu seçeneklerden birini etkinse, bazı iletiler kuyruğuna veya konusuna geldiğinde olmaz.

### <a name="measurements"></a>Ölçümler

IOT Central bir CİHAZDAN ileti aldıktan sonra yeni bir ileti hızlı bir şekilde aktarılır. Service Bus Event Hubs, dışarı aktarılan her ileti tam ileti JSON biçiminde "body" özelliğine gönderilen cihaz içerir. 

> [!NOTE]
> Ölçümler gönderen cihazları (aşağıdaki bölümlere bakın) cihaz kimlikleri tarafından temsil edilir. Cihazları adlarını almak için cihaz verilerini dışarı aktarma ve her iletinin kullanarak ilişkilendirin **connectionDeviceId** eşleşen **DeviceID** cihaz ileti.

Aşağıdaki örnekte gösterildiği olay hub'ı veya Service Bus kuyruğuna veya konusuna ölçümleri veriler hakkında bir ileti alındı.

```json
{
  "body": {
    "humidity": 29.06963648666288,
    "temp": 8.4503795661685,
    "pressure": 1075.8334910110093,
    "magnetometerX": 408.6966458887116,
    "magnetometerY": -532.8809796603962,
    "magnetometerZ": 174.70653875528205,
    "accelerometerX": 1481.546749013788,
    "accelerometerY": -1139.4316656437406,
    "accelerometerZ": 811.6928695575307,
    "gyroscopeX": 442.19879163299856,
    "gyroscopeY": 123.23710975717177,
    "gyroscopeZ": 708.5397575786151,
    "deviceState": "DANGER"
  },
  "annotations": {
    "iothub-connection-device-id": "<connectionDeviceId>",
    "iothub-connection-auth-method": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id": "<generationId>",
    "iothub-enqueuedtime": 1539381029965,
    "iothub-message-source": "Telemetry",
    "x-opt-sequence-number": 25325,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539381030200
  },
  "sequenceNumber": 25325,
  "enqueuedTimeUtc": "2018-10-12T21:50:30.200Z",
  "offset": "<offset>",
  "properties": {
    "content_type": "application/json",
    "content_encoding": "utf-8"
  }
}
```

### <a name="devices"></a>Cihazlar

Cihaz verilerini içeren iletileri, olay hub'ı veya Service Bus kuyruğuna veya konusuna birkaç dakikada bir kez gönderilir. Bu, birkaç dakikada hakkında ile verileri toplu olarak mesaj ulaşır anlamına gelir.
- Eklenen yeni cihazları
- Cihazları değiştirilmiş özellik ve değerlerini ayarlama

Her ileti için bir cihaz son dışarı aktarılan yapılacak itibaren bir veya daha fazla değişiklik temsil eder. Her iletiye gönderilen bilgiler içerir:
- `id` IOT Central'ın aygıt
- `name` Aygıt
- `deviceId` gelen [cihaz sağlama hizmeti](https://aka.ms/iotcentraldocsdps)
- Cihaz şablon bilgisi
- Özellik değerleri
- Ayar değerleri

> [!NOTE]
> Cihazları son toplu olmayan dışarı beri silindi. Şu anda yok gösterge silinen cihazlar için verilen iletileri.
>
> Her bir cihaza ait cihaz şablonu cihaz şablon kimliği ile temsil edilir Cihaz şablonunun adını almak için çok cihaz şablonu verileri dışarı aktarma emin olun.

Aşağıdaki örnek, olay hub'ı veya Service Bus kuyruğuna veya konusuna cihaz verileri hakkında bir ileti gösterir:


```json
{
  "body": {
    "id": "<id>",
    "name": "<deviceName>",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
      "id": "<templateId>",
      "version": "1.0.0"
    },
    "properties": {
      "cloud": {
        "location": "Seattle"
      },
      "device": {
        "dieNumber": 5445.5862873026645
      }
    },
    "settings": {
      "device": {
        "fanSpeed": 0
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "devices",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 39740,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274959654
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 39740,
  "enqueuedTimeUtc": "2018-10-11T16:22:39.654Z",
  "offset": "<offset>",
}
```

### <a name="device-templates"></a>Cihaz şablonları

Cihaz şablonları verileri içeren iletileri, olay hub'ı veya Service Bus kuyruğuna veya konusuna birkaç dakikada bir kez gönderilir. Bu, birkaç dakikada hakkında ile verileri toplu olarak mesaj ulaşır anlamına gelir.
- Eklenen yeni cihaz şablonları
- Cihaz şablonlarıyla değiştirilen ölçümleri, özellik ve tanımları ayarlama

Her ileti için cihaz şablonu son dışarı aktarılan yapılacak itibaren bir veya daha fazla değişiklik temsil eder. Her iletiye gönderilen bilgiler içerir:
- `id` cihaz şablonu
- `name` cihaz şablonu
- `version` cihaz şablonu
- Ölçüm veri türleri ve en düşük/en yüksek değerleri
- Özellik veri türleri ve varsayılan değerleri
- Veri türleri ve varsayılan değerleri ayarlama

> [!NOTE]
> Bu yana son toplu iş silindi cihaz şablonlarını dışa aktarılmaz. Şu anda yok gösterge silinen cihaz şablonları dışarı aktarılan iletileri.

Aşağıdaki örnek, olay hub'ı veya Service Bus kuyruğuna veya konusuna cihaz şablonları verileri hakkında bir ileti gösterir:

```json
{
  "body": {
    "id": "<id>",
    "version": "1.0.0",
    "name": "<templateName>",
    "measurements": {
      "telemetry": {
        "humidity": {
          "dataType": "double",
          "name": "humidity"
        },
        "pressure": {
          "dataType": "double",
          "name": "pressure"
        },
        "temp": {
          "dataType": "double",
          "name": "temperature"
        }
      }
    },
    "properties": {
      "cloud": {
        "location": {
          "dataType": "string",
          "name": "Location"
        }
      },
      "device": {
        "dieNumber": {
          "dataType": "double",
          "name": "Die Number"
        }
      }
    },
    "settings": {
      "device": {
        "fanSpeed": {
          "dataType": "double",
          "name": "Fan Speed",
          "initialValue": 0
        }
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "deviceTemplates",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 25315,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274985085
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 25315,
  "enqueuedTimeUtc": "2018-10-11T16:23:05.085Z",
  "offset": "<offset>",
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Event Hubs'a ve Azure Service Bus için verilerinizi dışarı aktarma artık bildiğinize göre sonraki adıma devam edin:

> [!div class="nextstepaction"]
> [Azure işlevleri'ni tetikleme](howto-trigger-azure-functions.md)
