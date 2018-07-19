---
title: Azure IOT Hub CİHAZDAN buluta iletileri anlama | Microsoft Docs
description: Geliştirici Kılavuzu - IOT Hub ile cihaz-bulut Mesajlaşma kullanma. Telemetri hem telemtry olmayan veri göndermeye başlamak ve yönlendirme iletileri ulaştırmak üzere kullanma hakkında bilgi içerir.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: be87b00f27f0d0b25cd77a0634ab1c653a85e5ac
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126451"
---
# <a name="send-device-to-cloud-messages-to-iot-hub"></a>IOT Hub'ına CİHAZDAN buluta ileti gönderme

Zaman serisi telemetri ve Uyarılar için çözüm arka ucunuz cihazlarınızdan göndermek için IOT hub'ınıza CİHAZDAN buluta iletileri cihazınızdan gönderin. IOT Hub tarafından desteklenen diğer cihaz-bulut seçeneklerinin bir açıklaması için bkz. [CİHAZDAN buluta iletişim Kılavuzu][lnk-d2c-guidance].

Bir cihaz'e yönelik uç noktası aracılığıyla CİHAZDAN buluta iletileri gönderme (**/devices/ {DeviceID} / iletiler/olaylar**). Yönlendirme kuralları ardından IOT hub'ınızdaki hizmet dönük uç noktalardan biri için iletiler yönlendirebilirsiniz. Yönlendirme kuralları, üst bilgiler ve CİHAZDAN buluta ileti gövdesini bunları nereye belirlemek için kullanın. Varsayılan olarak, iletiler yerleşik hizmet'e yönelik uç noktaya yönlendirilir (**iletiler/olaylar**), diğer bir deyişle uyumlu [Event Hubs][lnk-event-hubs]. Bu nedenle, standart kullanabilirsiniz [Event Hubs tümleştirme ve SDK'ları] [ lnk-compatible-endpoint] çözüm arka ucu CİHAZDAN buluta iletileri almak için.

IOT Hub CİHAZDAN buluta ileti gönderme akış bir Mesajlaşma deseni kullanılarak uygular. IOT Hub'ınızın CİHAZDAN buluta iletileri gibi daha fazla [Event Hubs] [ lnk-event-hubs] *olayları* daha [Service Bus] [ lnk-servicebus] *iletileri* olayları birden çok okuyucular tarafından okunan hizmeti üzerinden geçen yüksek hacimli olmasını durumunda.

CİHAZDAN buluta iletileri IOT Hub ile aşağıdaki özelliklere sahiptir:

* CİHAZDAN buluta iletileri, dayanıklı ve bir IOT hub'ın varsayılan saklama **iletiler/olaylar** yedi güne kadar uç noktası.
* CİHAZDAN buluta iletileri en fazla 256 KB olabilir ve gönderen iyileştirmek için toplu olarak gruplandırılabilir. Toplu en fazla 256 KB olabilir.
* İçinde anlatıldığı gibi [IOT hub'a erişimi denetleme] [ lnk-devguide-security] bölümde, IOT Hub cihaz başına kimlik doğrulaması ve erişim denetimi sağlar.
* IOT Hub, en fazla 10 özel uç noktaları oluşturmanıza olanak sağlar. İletileri, IOT hub'ınızda yapılandırılmış rota tabanlı uç noktalarına gönderilir. Daha fazla bilgi için [yönlendirme kuralları](iot-hub-devguide-query-language.md#device-to-cloud-message-routes-query-expressions).
* IOT Hub, eşzamanlı olarak bağlanan milyonlarca sağlar (bkz [kotalar ve azaltma][lnk-quotas]).
* IOT hub'ı rastgele bölümleme izin vermez. CİHAZDAN buluta iletileri bölümlenmiş kendi kaynak tabanlı **DeviceID**.

IOT Hub ile Event Hubs arasındaki farklar hakkında daha fazla bilgi için bkz. [karşılaştırma Azure IOT Hub ve Azure Event Hubs][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Telemetri dışı trafiği gönderme

Genellikle, telemetri yanı sıra cihazları, iletileri ve ayrı yürütme ve çözüm arka ucu işlemede gerektiren istekleri gönderir. Örneğin, belirli bir eylemi geri tetiklemelidir kritik uyarılar sonlandırın. Yazabileceğiniz bir [yönlendirme kuralı] [ lnk-devguide-custom] ya da ileti üzerinde bir üst bilgi veya bir değeri ileti gövdesi göre kendi işlemeye ayrılmış bir uç nokta bu tür iletileri göndermek için.

Bu tür bir iletiyi işlemek için en iyi yolu hakkında daha fazla bilgi için bkz: [Öğreticisi: IOT Hub CİHAZDAN buluta iletiler nasıl işlenir] [ lnk-d2c-tutorial] öğretici.

## <a name="route-device-to-cloud-messages"></a>CİHAZDAN buluta iletiler

Arka uç uygulamalarınızı CİHAZDAN buluta iletileri yönlendirmek için iki seçeneğiniz vardır:

* Yerleşik kullanın [Event Hub ile uyumlu uç nokta] [ lnk-compatible-endpoint] hub tarafından alınan CİHAZDAN buluta iletileri okumak arka uç uygulamalarını etkinleştirmek için. Yerleşik Event Hub ile uyumlu uç nokta hakkında bilgi edinmek için [CİHAZDAN buluta iletilerini yerleşik uç noktadan okuma][lnk-devguide-builtin].
* Özel uç noktalar IOT hub'ınızdaki iletileri göndermek için yönlendirme kuralları kullanın. Özel uç noktalar, Event Hubs, Service Bus kuyrukları ve Service Bus konu başlıklarını kullanarak CİHAZDAN buluta iletileri okumak arka uç uygulamalarınızı etkinleştirin. Yönlendirme ve özel uç noktaları hakkında bilgi edinmek için [özel uç noktalar ve yönlendirme kuralları için CİHAZDAN buluta iletileri kullanın][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Sahtekarlığına karşı koruma özellikleri

Tüm Damgalar cihaz IOT hub'ı, CİHAZDAN buluta iletileri kimlik sahtekarlığı önlemek için aşağıdaki özelliklere sahip iletileri:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

İlk iki içeren **DeviceID** ve **Generationıd** kaynak cihazın olarak başına [cihaz kimlik özelliklerini][lnk-device-properties].

**ConnectionAuthMethod** özelliği, aşağıdaki özelliklere sahip bir seri hale getirilmiş JSON nesnesi içerir:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Sonraki adımlar

CİHAZDAN buluta iletileri göndermek için kullanabileceğiniz SDK'lar hakkında bilgi için bkz. [Azure IOT SDK'ları][lnk-sdks].

[Hızlı Başlangıçlar] [ lnk-get-started] sanal cihazlardan CİHAZDAN buluta ileti gönderme işlemini gösterir. Daha fazla ayrıntı için [yolları kullanarak işlem IOT Hub CİHAZDAN buluta iletileri] [ lnk-d2c-tutorial] öğretici.

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: quickstart-send-telemetry-node.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: tutorial-routing.md
