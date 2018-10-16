---
title: Yönlendirme olayları ve iletileri Azure dijital çiftleri ile | Microsoft Docs
description: Yönlendirme olayları ve iletileri Azure dijital çiftleri ile hizmet uç noktalarına genel bakış
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 9b861f0991b11637c7b27b645d4506e658ea53b3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324382"
---
# <a name="routing-events-and-messages"></a>Yönlendirme olayları ve iletileri

IOT çözümleri, depolama, analiz ve daha fazlası dahil olmak üzere çeşitli güçlü Hizmetleri genellikle birleştiren. Bu makalede, Azure dijital İkizlerini uygulamaları daha ayrıntılı Öngörüler ve İşlevler zenginleştirmek için Azure depolama analizi ve yapay ZEKA hizmetlerine bağlanmak açıklar.

## <a name="route-types"></a>Yol türü

Dijital çiftleri, IOT olayları diğer Azure Hizmetleri veya iş uygulamalarınızla tümleştirmek için iki yol sunar:

* **Yönlendirme dijital İkizlerini olayları**: Azure dijital İkizlerini olayları tetiklenen telemetri verilerini alındığında veya kullanıcı tanımlı işlev, uzamsal grafiği değişiklikleri bir nesne oluşturur, önceden tanımlanmış koşullara göre bir bildirim. Kullanıcılara bu olayları gönderme [Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Service Bus konu başlıklarını](https://azure.microsoft.com/services/service-bus/), veya [olay Kılavuzlar](https://azure.microsoft.com/services/event-grid/) daha fazla işleme için.

* **Yönlendirme cihaz telemetrisi**: Yönlendirme olaylarına ilaveten, Azure dijital İkizlerini aynı zamanda ham cihaz telemetri iletilerini Event Hubs'a daha fazla öngörü ve analizler için yönlendirebilirsiniz. Bu tür iletileri dijital İkizlerini tarafından işlenen değildir ve bunlar yalnızca iletilir **olay hub'ı**.

Kullanıcılar, olayları veya iletme iletileri göndermek için bir veya daha fazla çıkış uç noktası belirtebilir. Olayları ve iletileri bu önceden tanımlanmış yönlendirme tercihlerini göre uç noktalarına gönderilir. Diğer bir deyişle, kullanıcılar, grafik işlem olaylarını almak için belirli bir uç nokta başka bir cihaz telemetri olaylarını almak ve benzeri belirtebilirsiniz.

![Dijital İkizlerini olaylarını yönlendirme][1]

Yönlendirme **Event Hubs** siparişin hangi telemetriyi iletileri gönderilir, böylece özgün alındıkları gibi aynı sıradaki uç noktasında ulaştıkları korur. **Event Grid** ve **Service Bus** uç noktalar aynı sırada, gerçekleşen olayları alacaksınız garanti etmez. Ancak, olay şeması uç noktasında olayları geldikten sonra sırasını belirlemek için kullanılan bir zaman damgası içerir.

## <a name="route-implementation"></a>Rota uygulaması

Azure dijital İkizlerini hizmeti şu anda aşağıdaki desteklemektedir **EndpointTypes**:

* **EventHub**: olay hub'ı bağlantı dizesi uç noktadır.
* **ServiceBus**: Service Bus bağlantı dizesi uç noktadır.
* **EventGrid**: Event Grid bağlantı dizesi uç noktadır.

Azure dijital İkizlerini şu anda aşağıdaki destekler **EventTypes** seçilen uç noktasına gönderilir:

* **DeviceMessages**: telemetri iletilerini kullanıcıların cihazlarından gönderilir ve sistem tarafından iletilir.
* **TopologyOperation**: grafik veya graf meta verileri değiştirme işlemleri. Örneğin, ekleme veya silme gibi bir alan bir varlık.
* **SpaceChange**: olan bir alana ait değişiklikleri hesaplanan değer sonucunda bir cihaz telemetri iletisi.
* **SensorChange**: olan bir algılayıcının değişiklikleri hesaplanan bir cihaz telemetrisi ileti sonucu olarak değer.
* **UdfCustom**: kullanıcı tanımlı bir işlevden özel bildirimleri.

> [!IMPORTANT]
> Tüm **EndpointTypes** tüm destek **EventTypes**.
> Aşağıdaki tabloya bakın **EventTypes** izin verilen her biri için **EndpointType**.

|             | DeviceMessages | TopologyOperation | SpaceChange | SensorChange | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| **eventHub**|     X          |         X         |     X       |      X       |   X       |
| **ServiceBus**|              |         X         |     X       |      X       |   X       |
| **EventGrid**|               |         X         |     X       |      X       |   X       |

>[!NOTE]
>Uç noktaları ve olay ' şema örnekleri oluşturma hakkında daha fazla ayrıntı için lütfen bkz [uç noktaları ve çıkış](how-to-egress-endpoints.md).

## <a name="next-steps"></a>Sonraki adımlar

Genel Önizleme sınırları hakkında bilgi edinmek için [Azure dijital İkizlerini Önizleme sınırları](concepts-service-limits.md).

Bir Azure dijital İkizlerini örnek denemek için okuma [kullanılabilir odaları bulmak için Hızlı Başlangıç](quickstart-view-occupancy-dotnet.md).

<!-- Images -->
[1]: media/concepts/digital-twins-events-routing.png