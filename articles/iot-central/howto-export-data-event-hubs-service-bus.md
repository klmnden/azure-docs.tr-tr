---
title: Azure Event Hubs ve Azure Service Bus verilerinizi dışarı aktarma | Microsoft Docs
description: Azure Event Hubs'a ve Azure Service Bus için Azure IOT Central uygulamanızdan veri dışarı aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 03/20/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 78edeb0c418f5c426771d241464d389f8a632e96
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65463946"
---
# <a name="export-your-data-in-azure-iot-central"></a>Azure IOT Central verilerinizi dışarı aktarma

*Bu konu, Yöneticiler için geçerlidir.*

Bu makalede sürekli veri dışa aktarma Özelliği Azure IOT Central verilerinizi kendi değerlerinizle dışarı aktarmak için nasıl kullanılacağını açıklar **Azure Event Hubs**, ve **Azure Service Bus** örnekleri. Dışarı aktarabilirsiniz **ölçümleri**, **cihazları**, ve **cihaz şablonları** Orta Gecikmeli kanaldan öngörü ve analizler için kendi hedef. Bu, Azure Stream analytics'te özel kurallar tetikleme, Azure Logic apps'te özel iş akışları tetikleyen veya veri dönüştürme ve Azure işlevleri aracılığıyla iletmeden içerir. 

> [!Note]
> Bir kez daha, verileri sürekli dışarı aktarma üzerinde etkinleştirdiğinizde, ileriye doğru o andan itibaren yalnızca verileri alın. Şu anda, verileri sürekli dışarı aktarma kapalıydı ne zaman bir kez verileri alınamıyor. Daha fazla geçmiş verileri korumak için verileri sürekli dışarı aktarma üzerinde erken açın.


## <a name="prerequisites"></a>Önkoşullar

- IOT Central uygulamanızda yönetici olmanız gerekir

## <a name="set-up-export-destination"></a>Dışarı aktarma hedef ayarlayın

Vermek için mevcut bir olay hub'ları / Service Bus sahip değilseniz, şu adımları izleyin:

## <a name="create-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

1. Oluşturma bir [Azure portalında yeni Event Hubs ad alanı](https://ms.portal.azure.com/#create/Microsoft.EventHub). Daha fazla bilgi [Azure Event Hubs belgeleri](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).
2. Bir abonelik seçin. 

    > [!Note] 
    > Artık olan diğer abonelikler için verileri dışarı aktarabilirsiniz **aynı** bir Kullandıkça Öde IOT Central uygulamanız için. Bu durumda bir bağlantı dizesi kullanarak bağlanır.
3. Event Hubs ad alanınız içinde bir olay hub'ı oluşturun. Ad alanınıza gidin ve seçin **+ olay hub'ı** en üstünde bir olay hub'ı örneği oluşturulamadı.

## <a name="create-service-bus-namespace"></a>Service Bus ad alanı oluşturma

1. Oluşturma bir [Azure portalında yeni hizmet veri yolu ad alanı](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5) . Daha fazla bilgi [Azure Service Bus belgeleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal).
2. Bir abonelik seçin. 

    > [!Note] 
    > Artık olan diğer abonelikler için verileri dışarı aktarabilirsiniz **aynı** bir Kullandıkça Öde IOT Central uygulamanız için. Bu durumda bir bağlantı dizesi kullanarak bağlanır.

3. Service Bus ad alanınıza gidin ve seçin **+ kuyruk** veya **+ konu** en üstünde bir kuyruk veya konuda vermek için oluşturulacak.


## <a name="set-up-continuous-data-export"></a>Verileri sürekli dışarı aktarma ayarlayın

Verileri dışarı aktarmak için bir olay hub'ları / Service Bus hedef olduğuna göre verileri sürekli dışarı aktarma ' için bu adımları izleyin. 

1. IOT Central uygulamanız için oturum açın.

2. Sol menüde **verileri sürekli dışarı aktarma**.

    > [!Note]
    > Verileri sürekli dışarı aktarma sol taraftaki menüde görmüyorsanız, yöneticinin uygulamanızda değildir. Verileri dışarı aktarma ' için yöneticinin konuşun.

    ![Yeni değerinde olay hub'ı oluşturma](media/howto-export-data/export_menu1.png)

3. Seçin **+ yeni** sağ üst köşesindeki düğme. Birini **Azure Event Hubs** veya **Azure Service Bus** dışarı aktarma hedefi olarak. 

    > [!NOTE] 
    > Dışarı aktarmalar uygulama başına en fazla sayısı beştir. 

    ![Yeni verileri sürekli dışarı aktarma oluştur](media/howto-export-data/export_new1.png)

4. Aşağı açılan liste kutusunda, **Event Hubs ad alanı/Service Bus ad alanı**. Son seçenek, listeden seçebilirsiniz **bir bağlantı dizesi girin**. 

    > [!NOTE] 
    > Depolama hesapları/Event Hubs ad alanlarını/hizmet veri yolu ad alanları yalnızca göreceksiniz **IOT Central uygulamanız ile aynı abonelikte**. Bu abonelik dışında bir hedefe dışarı aktarmak istiyorsanız seçin **bir bağlantı dizesi girin** ve 5. adıma bakın.

    > [!NOTE] 
    > 7 günlük deneme uygulamaları, verileri sürekli yapılandırmak için tek yolu dışarı aktarmak için bir bağlantı dizesidir. 7 günlük deneme uygulamalar, ilişkili Azure aboneliği olmadığı için budur.

    ![Yeni değerinde olay hub'ı oluşturma](media/howto-export-data/export_create1.png)

5. (İsteğe bağlı) Seçerseniz, **bir bağlantı dizesi girin**, bağlantı dizenizi yapıştırmak için yeni kutusu görünür. Bağlantı dizesini almak için:
    - Olay hub'ları veya Service Bus, Azure portalında ad alanına gidin.
        - Altında **ayarları**seçin **paylaşılan erişim ilkeleri**
        - Varsayılan seçin **RootManageSharedAccessKey** veya yeni bir tane oluşturun
        - Birincil veya ikincil bağlantı dizesini kopyalayın
 
6. Bir olay hub'ı / kuyruk veya konuda aşağı açılan liste kutusundan seçin.

7. Altında **dışarı aktarmak için veri**, her tür ayarlayarak dışarı aktarmak için veri türü belirtin **üzerinde**.

6. Verileri sürekli dışarı aktarma üzerinde etkinleştirmek için emin **verileri dışarı aktarma** olduğu **üzerinde**. **Kaydet**’i seçin.

    ![Yapılandırma verileri sürekli dışarı aktarma](media/howto-export-data/export_list1.png)

7. Birkaç dakika sonra verilerinizi, seçtiğiniz hedef olarak görünür.


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
