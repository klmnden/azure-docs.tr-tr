---
title: "Azure IOT Hub cihaz bulut Mesajlaşma anlama | Microsoft Docs"
description: "Geliştirici Kılavuzu - IOT Hub ile cihaz bulut Mesajlaşma kullanma. Telemetri ve telemtry olmayan veri gönderme ve iletileri sunmak için yönlendirmeyi kullanma hakkında bilgiler içerir."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 48b904818c80b9175d45b88345634f11cf4a4812
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="send-device-to-cloud-messages-to-iot-hub"></a>IOT hub'a cihaz-bulut iletileri gönder

Zaman serisi telemetri ve uyarılar, çözüm arka ucuna cihazlarınızdan göndermek için cihaz bulut iletilerini cihazınızın IOT hub'ınıza gönderin. IOT Hub tarafından desteklenen diğer cihaz bulut seçenekleri tartışma için bkz [cihaz bulut iletişimleri Kılavuzu][lnk-d2c-guidance].

Bir aygıt'e yönelik uç noktası aracılığıyla cihaz bulut iletilerini gönder (**/devices/ {DeviceID} / iletileri/olayları**). Yönlendirme kuralları ardından, IOT hub'ındaki service'e yönelik uç noktalardan biri iletilerinizi rotaya. Yönlendirme kuralları, onları yönlendirmek nereye belirlemek için üstbilgiler ve cihaz bulut iletilerini gövdesi kullanın. Varsayılan olarak, yerleşik service'e yönelik uç noktasına iletileri yönlendirilir (**iletileri/olayları**), diğer bir deyişle uyumlu [Event Hubs][lnk-event-hubs]. Bu nedenle, standart kullanabilirsiniz [olay hub'ları tümleştirme ve SDK] [ lnk-compatible-endpoint] çözüm arka ucu cihaz-bulut iletileri almak için.

IOT Hub cihaz bulut akış Mesajlaşma düzeni kullanarak ileti uygular. IOT Hub'ın cihaz bulut iletilerini gibi daha fazla [Event Hubs] [ lnk-event-hubs] *olayları* daha [Service Bus] [ lnk-servicebus] *iletileri* birden çok okuyucular tarafından okunabilir hizmeti aracılığıyla geçirme olayları hacmi yüksek olmasını durumunda.

Cihaz bulut IOT Hub ile Mesajlaşma aşağıdaki özelliklere sahiptir:

* Cihaz bulut iletilerini sağlam ve bir IOT hub'ın varsayılan saklama **iletileri/olayları** endpoint yedi gündür.
* Cihaz bulut iletilerini en fazla 256 KB olabilir ve gönderir iyileştirmek için toplu olarak gruplandırılabilir. Toplu işlemler en fazla 256 KB olabilir.
* İçinde anlatıldığı gibi [IOT Hub'ına erişim denetim] [ lnk-devguide-security] bölüm, IOT Hub cihaz başına kimlik doğrulama ve erişim denetimi sağlar.
* IOT Hub, en fazla 10 özel uç noktaları oluşturmanıza olanak sağlar. İletiler, IOT hub'ına yapılandırılmış yolları göre Uç noktalara teslim edilir. Daha fazla bilgi için bkz: [yönlendirme kuralları](#routing-rules).
* IOT Hub, milyonlarca eş zamanlı cihazı sağlar (bkz [kotalar ve azaltma][lnk-quotas]).
* IOT hub'ı rasgele bölümleme izin vermiyor. Cihaz bulut iletilerini bölümlenmiş kendi oluşturan bağlı **DeviceID**.

IOT Hub ve Event Hubs arasındaki farklar hakkında daha fazla bilgi için bkz: [karşılaştırma Azure IOT Hub ve Azure Event Hubs][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Telemetri olmayan trafiği Gönder

Genellikle, telemetri ek olarak, iletileri ve ayrı yürütme ve çözüm arka ucu işlemede gerektiren isteklerinin aygıtları gönderin. Arka uçtaki belirli bir eylemi tetikleyen gerekir Örneğin, kritik uyarılar. Yazma bir [yönlendirme kuralı] [ lnk-devguide-custom] ya da bir başlığına bir ileti ya da bir değeri ileti gövdesi göre bunların işlenmesini adanmış bir uç nokta ileti türlerinden göndermek için.

Bu tür bir iletiyi işlemek için en iyi yolu hakkında daha fazla bilgi için bkz: [Öğreticisi: IOT Hub cihaz bulut iletilerini işlemek nasıl] [ lnk-d2c-tutorial] Öğreticisi.

## <a name="route-device-to-cloud-messages"></a>Rota cihaz-bulut iletileri

Arka uç uygulamalarınızı cihaz-bulut iletileri yönlendirmek için iki seçeneğiniz vardır:

* Yerleşik kullanmak [Event Hub ile uyumlu uç nokta] [ lnk-compatible-endpoint] arka uç uygulamaların hub tarafından alınan cihaz bulut iletilerini okumanızı sağlamak için. Yerleşik Event Hub ile uyumlu uç noktası hakkında bilgi edinmek için [yerleşik uç noktasından cihaz bulut iletilerini okumanızı][lnk-devguide-builtin].
* IOT hub'ınızdaki özel uç noktaları iletileri göndermek için yönlendirme kurallarını kullanın. Olay hub'ları, Service Bus kuyruklarını veya Service Bus konu başlıklarını kullanarak cihaz bulut iletilerini okumanızı arka uç uygulamalarınızı özel uç noktaları etkinleştirin. Yönlendirme ve özel uç noktalar hakkında bilgi edinmek için [özel uç noktaları ve yönlendirme kuralları için cihaz bulut iletilerini kullanmak][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Yanıltma özellikleri

Damgalar tüm cihaz IOT Hub, cihaz bulut iletilerini kimlik sahtekarlığı önlemek için aşağıdaki özelliklere sahip iletileri:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

İlk iki içeren **DeviceID** ve **Generationıd** kaynak cihazın göre [aygıt kimlik özellikleri][lnk-device-properties].

**ConnectionAuthMethod** özelliği, aşağıdaki özelliklere sahip bir JSON serileştirilmiş nesne içerir:

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Cihaz bulut iletilerini göndermek için kullanabileceğiniz SDK'ları hakkında bilgi için bkz: [Azure IOT SDK'ları][lnk-sdks].

[Get Started] [ lnk-get-started] öğreticileri sanal ve fiziksel aygıtlardan cihaz bulut iletilerini göndermek nasıl gösterir. Daha fazla ayrıntı için [yolları kullanma işlemi IOT Hub cihaz bulut iletilerini] [ lnk-d2c-tutorial] Öğreticisi.

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
