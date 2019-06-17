---
title: Yönlendirme olayları ve iletileri Azure dijital çiftleri ile | Microsoft Docs
description: Yönlendirme olayları ve iletileri Azure dijital çiftleri ile hizmet uç noktalarına genel bakış
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: alinast
ms.openlocfilehash: 7dfda00aca403f7f95f0c56d1db28c3c609bebd2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080650"
---
# <a name="routing-events-and-messages"></a>Yönlendirme olayları ve iletileri

IOT çözümleri, depolama, analiz ve daha fazlasını içeren çeşitli güçlü Hizmetleri genellikle birleştiren. Bu makalede, Azure dijital İkizlerini uygulamaları daha ayrıntılı Öngörüler ve İşlevler sağlamak için Azure depolama analizi ve yapay ZEKA hizmetlerine bağlanmak açıklar.

## <a name="route-types"></a>Yol türü  

Azure dijital çiftleri, IOT olaylar, diğer Azure hizmetlerine veya iş kolu uygulamaları ile bağlanmak için iki yol sunar:

* **Azure dijital İkizlerini olaylarını yönlendirme**: Uzamsal nesneyi graf alındığında, telemetri verilerini bu değişiklikleri veya önceden tanımlanmış koşullara göre bir uyarı oluşturan bir kullanıcı tanımlı işlev Azure dijital İkizlerini olayları tetikleyebilir. Kullanıcılara bu olayları gönderme [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure Service Bus konu başlıklarını](https://azure.microsoft.com/services/service-bus/), veya [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) daha fazla işleme için.

* **Yönlendirme cihaz telemetrisi**: Yönlendirme olaylarına ilaveten, Azure dijital İkizlerini aynı zamanda ham cihaz telemetri iletilerini Event Hubs'a daha fazla öngörü ve analizler için yönlendirebilirsiniz. Bu tür iletileri Azure dijital İkizlerini tarafından işlenen değil. Ve olay hub'ına yalnızca iletilen.

Kullanıcılar, olayları veya iletme iletileri göndermek için bir veya daha fazla çıkış uç noktası belirtebilir. Olayları ve iletileri bu önceden tanımlanmış yönlendirme tercihlerini göre uç noktalarına gönderilir. Diğer bir deyişle, kullanıcılar, grafik işlem olaylarını almak için bir belirli uç noktasına başka bir cihaz telemetri olaylarını almak ve benzeri belirtebilir.

![Azure dijital İkizlerini olaylarını yönlendirme][1]

Event Hubs için üretim telemetri iletilerini gönderilme sırasını korur. Bu nedenle başlangıçta alındıkları gibi aynı sıradaki uç noktasında gelen. Event Grid ve Service Bus uç noktaları aynı sırada, gerçekleşen olayları alacaksınız garanti etmez. Ancak, olay şeması uç noktasında olayları geldikten sonra sırasını belirlemek için kullanılan bir zaman damgası içerir.

## <a name="route-implementation"></a>Rota uygulaması

Azure dijital İkizlerini hizmeti şu anda aşağıdaki desteklemektedir **EndpointTypes**:

* **EventHub** Event Hubs bağlantı dizesi uç noktadır.
* **ServiceBus** Service Bus bağlantı dizesi uç noktadır.
* **EventGrid** Event Grid bağlantı dizesi uç noktadır.

Azure dijital İkizlerini şu anda aşağıdaki destekler **EventTypes** seçilen uç noktasına gönderilir:

* **DeviceMessages** telemetri iletilerini kullanıcıların cihazlarından gönderilir ve sistem tarafından iletilir.
* **TopologyOperation** grafik veya graf meta verileri değiştiren bir işlemdir. Bir örnek ekleme veya bir boşluk gibi bir varlık siliniyor.
* **SpaceChange** gelen cihaz telemetrisi iletiyi sonuçları hesaplanan değeri boşlukla ait bir değişikliktir.
* **SensorChange** gelen cihaz telemetrisi iletiyi sonuçları bir algılayıcının hesaplanan değeri bir değişikliktir.
* **UdfCustom** kullanıcı tanımlı bir işlevden bir özel bildirimidir.

> [!IMPORTANT]  
> Tüm **EndpointTypes** tüm destek **EventTypes**.
> İçin aşağıdaki tabloya bakın **EventTypes** izin verilen her biri için **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| EventHub|     X          |         X         |     X       |      X       |   X       |
| ServiceBus|              |         X         |     X       |      X       |   X       |
| EventGrid|               |         X         |     X       |      X       |   X       |

>[!NOTE]  
>Uç noktaları ve olay ' şema örnekleri nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [çıkış ve uç noktaları](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Sonraki adımlar

- Önizleme sınırları Azure dijital İkizleri hakkında bilgi edinmek için [genel Önizleme hizmet sınırları](concepts-service-limits.md).

- Bir Azure dijital İkizlerini örnek denemek için bkz [kullanılabilir odaları bulmak için Hızlı Başlangıç](quickstart-view-occupancy-dotnet.md).

<!-- Images -->
[1]: media/concepts/digital-twins-events-routing.png
