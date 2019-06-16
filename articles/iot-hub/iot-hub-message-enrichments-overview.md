---
title: Azure IOT Hub ileti zenginleştirmelerinin genel bakış
description: Azure IOT Hub ileti için ileti zenginleştirmelerinin genel bakış
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: 13e35ab93fc37541548785c6355489eaf3a3efc2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66754555"
---
# <a name="message-enrichments-for-device-to-cloud-iot-hub-messages-preview"></a>İleti zenginleştirmelerinin CİHAZDAN buluta IOT hub'ı iletileri (Önizleme)

*İleti zenginleştirmelerinin* IOT Hub'ına özelliğidir *damga* iletileri belirlenen uç noktasına gönderilmeden önce ek bilgilerle iletileri. İleti zenginleştirmelerinin kullanmak için bir neden, aşağı akış işleme basitleştirmek için kullanılan veri eklemektir. Örneğin, cihaz ikizi etiketi ile cihaz telemetri iletilerini zenginleştirme, müşterilerin, cihaz ikizi için bu bilgileri API çağrıları yapmak için yük azaltabilir.

![İleti zenginleştirmelerinin akışı](./media/iot-hub-message-enrichments-overview/message-enrichments-flow.png)

Bir ileti zenginleştirme, üç anahtar öğelerin sahiptir:

* Zenginleştirme adı veya anahtar

* Bir değer

* Bir veya daha fazla [uç noktaları](iot-hub-devguide-endpoints.md) için zenginleştirme uygulanmalıdır.

Herhangi bir dize olabilir.

Değer aşağıdaki örnekler hiçbirini olabilir:

* Herhangi bir statik dize. Koşullar, mantıksal, işlemler ve işlevler gibi dinamik değerler izin verilmez. Örneğin, çeşitli müşteriler tarafından kullanılan bir SaaS uygulaması geliştiriyorsanız, her müşteri için bir tanımlayıcı atayın ve tanımlayıcı uygulamada kullanılabilir hale getirebilirsiniz. Uygulama çalışırken, IOT Hub cihaz telemetri iletilerini müşterinin tanımlayıcısına sahip iletileri her müşteri için farklı şekilde işlemek üzere edinerek damgasının.

* Cihaz ikizini yolu gibi bilgileri. Örnekler olacak *$twin.tags.field* ve *$twin.tags.latitude*.

* İletiyi gönderen IOT hub'ın adı. Bu değer *$iothubname*.

## <a name="applying-enrichments"></a>Zenginleştirmelerinin uygulanıyor

İletileri tarafından desteklenen herhangi bir veri kaynağından gelebilir [IOT Hub ileti yönlendirme](iot-hub-devguide-messages-d2c.md), aşağıdaki örnekler de dahil olmak üzere:

* Sıcaklık veya baskısı gibi cihaz telemetrisi
* cihaz ikizi değişiklik bildirimleri--cihaz ikizinde değişiklikleri
* cihazın ne zaman oluşturulduğu veya silindiği gibi cihaz yaşam döngüsü olayları

Bir IOT Hub'ın yerleşik uç noktaya giden iletileri ya da Azure Blob Depolama, Service Bus kuyruğu veya Service Bus konu gibi özel uç noktalar için yönlendirilmiş iletiler zenginleştirmelerinin ekleyebilirsiniz.

Event Grid, Event Grid uç nokta seçerek yayımlanan iletileri zenginleştirmelerinin ekleyebilirsiniz. Daha fazla bilgi için [IOT Hub ve Event Grid](iot-hub-event-grid.md).

Zenginleştirmelerinin uç noktası uygulanır. Belirli bir uç nokta için damgalı için beş zenginleştirmelerinin belirtirseniz, bu uç noktaya gitmesiyle tüm iletiler aynı beş zenginleştirmelerinin ile içerdiği.

İletisi zenginleştirmelerinin denemek hakkında bilgi için bkz [ileti zenginleştirmelerinin Öğreticisi](tutorial-message-enrichments.md)

## <a name="limitations"></a>Sınırlamalar

* Bu hub'ları için IOT hub'ı başına en fazla 10 zenginleştirmelerinin standart veya temel katmanında ekleyebilirsiniz. IOT hub'ları için ücretsiz katmanda en fazla 2 zenginleştirmelerinin ekleyebilirsiniz.

* Bir değeri bir etiket veya cihaz ikizindeki özelliği ayarlanmış bir zenginleştirme uyguluyorsanız bazı durumlarda, değeri dize değeri olarak damgalı. İçin $twin.tags.field zenginleştirme değeri ayarlarsanız, örneğin, iletileri "$twin.tags.field" dizesi ile bu alanın değeri yerine ikizinden damgalı. Bu, aşağıdaki durumlarda gerçekleşir:

   * IOT Hub'ınıza temel katmanında ' dir. Cihaz ikizlerini temel katman IOT hub'ları desteklemez.

   * IOT Hub'ınıza standart katmanda, ancak iletiyi göndermeyi cihaz hiçbir cihaz ikizi.

   * IOT Hub'ınıza, standart katmanda olmakla birlikte zenginleştirme değeri için kullanılan cihaz ikizi yolu yok. Örneğin, zenginleştirme değeri $twin.tags.location için ayarlanır ve cihaz ikizinde etiketleri altında bir konum özelliği yoktur ileti "$twin.tags.location" dizesi ile damgalandı. 

* Cihaz ikizi güncelleştirmeleri karşılık gelen zenginleştirme değeri yansıtılması beş dakika kadar sürebilir.

* Zenginleştirmelerinin dahil olmak üzere toplam ileti boyutu 256 KB aşamaz. İleti boyutu 256 KB aşarsa, IOT Hub ileti kaldıracağız. Kullanabileceğiniz [IOT hub'ı ölçümleri](iot-hub-metrics.md) bırakılan iletiler, hatalarını ayıklamanıza ve tanımlamak için. Örneğin, d2c.telemetry.egress.invalid izleyebilirsiniz.

## <a name="pricing"></a>Fiyatlandırma

İleti zenginleştirmelerinin için ek ücret ödemeden kullanılabilir. Şu anda, bir IOT Hub'ına ileti gönderirken ücretlendirilir. İleti için birden fazla uç nokta aşsa bile yalnızca bir kez bu ileti için ücretlendirilirsiniz.

## <a name="availability"></a>Kullanılabilirlik

Bu özellik Önizleme aşamasındadır ve Doğu ABD, Batı ABD, Batı Avrupa dışındaki tüm bölgelerde kullanılabilir [Azure kamu](/azure/azure-government/documentation-government-welcome), [Azure Çin 21Vianet](/azure/china), ve [Azure Almanya](https://azure.microsoft.com/global-infrastructure/germany/).

## <a name="next-steps"></a>Sonraki adımlar

Bir IOT hub'ına ileti yönlendirme konusunda daha fazla bilgi için şu makalelere göz atın:

* [Farklı uç noktalar için CİHAZDAN buluta iletileri göndermek için IOT Hub ileti yönlendirme kullanın](iot-hub-devguide-messages-d2c.md)

* [Öğretici: IOT Hub'ın Yönlendirme](tutorial-routing.md)