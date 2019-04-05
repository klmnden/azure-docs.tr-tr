---
title: Azure IOT Hub iletişim protokolleri ve bağlantı noktaları | Microsoft Docs
description: Geliştirici Kılavuzu - desteklenen iletişim protokolleri CİHAZDAN buluta ve bulut-cihaz iletişim ve açık bir bağlantı noktası numaraları açıklanmaktadır.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 7082ebc4ca3066f84ca9790797cfa04e437f78a3
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59051064"
---
# <a name="reference---choose-a-communication-protocol"></a>Başvuru - iletişim protokolü seçme

IOT Hub cihaz tarafı iletişimleri için şu protokoller cihazların sağlar:

* [MQTT](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf)
* WebSockets üzerinden MQTT
* [AMQP](https://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
* WebSockets üzerinden AMQP
* HTTPS

Bu protokollerin belirli IOT hub'ı özelliklerinden nasıl desteklediği hakkında daha fazla bilgi için bkz: [CİHAZDAN buluta iletişim Kılavuzu](iot-hub-devguide-d2c-guidance.md) ve [bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md).

Aşağıdaki tabloda, Protokolü seçiminize yönelik üst düzey öneriler verilmiştir:

| Protokol | Bu protokol seçtiğinizde |
| --- | --- |
| MQTT <br> WebSocket üzerinden MQTT |Birden çok (her biri kendi cihaz başına kimlik bilgilerini) aynı TLS bağlantısı üzerinden bağlıyorsunuz gerekmez, tüm cihazlarda kullanın. |
| AMQP <br> WebSocket üzerinden AMQP |Cihaz üzerinden çoğullama bağlantı yararlanmak için alan ve bulut ağ geçidi'ni kullanın. |
| HTTPS |Diğer protokoller destekleyemez cihazlar için kullanın. |

Aygıt tarafı iletişimleri protokolünüzü seçtiğinizde aşağıdaki noktaları göz önünde bulundurun:

* **Bulut-cihaz deseni**. HTTPS sunucusu gönderimi uygulamak için etkin bir yönteme sahip değil. Bu nedenle, HTTPS kullanırken, cihazlar IOT Hub için bulut-cihaz iletilerini yoklar. Bu yaklaşım, hem cihaz hem de IOT Hub için yetersizdir. Geçerli HTTPS kılavuzları her cihaz için iletileri her 25 dakika veya daha fazla yoklama. MQTT ve AMQP sunucu gönderimi bulut-cihaz iletilerini alırken destekler. Anında bildirim iletileri IOT hub'dan cihaza tanırlar. Teslim gecikme bir konudur, MQTT veya AMQP kullanmak için en iyi protokolleri demektir. Nadiren bağlı cihazlar için HTTPS de çalışır.

* **Alan ağ geçitleri**. MQTT ve HTTPS kullanarak, birden çok cihazı (her biri kendi cihaz başına kimlik bilgilerini) bağlanamıyor aynı TLS bağlantısını kullanarak. İçin [alan ağ geçidi senaryoları](iot-hub-devguide-endpoints.md#field-gateways) bağlı her cihaz için bir alan ağ geçidi ve IOT hub'ı arasında bir TLS bağlantı gerektirir, bu protokolleri yetersiz.

* **Düşük kaynak cihazları**. MQTT ve HTTPS kitaplıkları AMQP kitaplıkları değerinden daha küçük bir kaplama alanı vardır. Cihaz kaynaklarını (örneğin, küçük 1 MB RAM) sınırlıdır, bu nedenle, bu protokolleri yalnızca protokol uygulaması olabilir.

* **Ağ geçişi**. Bağlantı noktası 5671 standart AMQP protokolünü kullanan ve MQTT 8883 numaralı bağlantı noktasında dinler. Bu bağlantı noktalarının kullanımını HTTPS dışındaki protokoller için kapalı ağlardaki sorunlara neden olabilir. MQTT WebSockets, AMQP WebSockets veya HTTPS üzerinden bu senaryoda kullanın.

* **Yükü boyutu**. MQTT ve AMQP HTTPS değerinden daha küçük yüklerini neden ikili, kurallarıdır.

> [!WARNING]
> HTTPS kullanarak, her cihaz her 25 dakika veya daha fazla bulut buluttan cihaza iletiler için yoklama. Ancak, geliştirme sırasında her 25 dakikadan daha sık yoklamak için kabul edilebilir.

## <a name="port-numbers"></a>Bağlantı noktası numaraları

Cihazlar, çeşitli protokoller kullanarak Azure IOT Hub ile iletişim kurabilir. Genellikle, Protokol seçimi belirli gereksinimlerine çözüm tarafından yönetilir. Aşağıdaki tabloda, bir cihazın belirli bir protokol kullanabilmek için açık olmalıdır giden bağlantı noktaları listelenmektedir:

| Protokol | Bağlantı noktası |
| --- | --- |
| MQTT |8883 |
| WebSockets üzerinden MQTT |443 |
| AMQP |5671 |
| WebSockets üzerinden AMQP |443 |
| HTTPS |443 |

Bir Azure bölgesinde bir IOT hub'ı oluşturduğunuzda, IOT hub'ı, IOT hub'ı ömrü boyunca aynı IP adresini tutar. Microsoft IOT hub hizmet kalitesini korumak için farklı ölçek birimi için taşır, ancak daha sonra yeni bir IP adresi atanır.

## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı ve MQTT protokolünü nasıl uyguladığı hakkında daha fazla bilgi için bkz: [ve MQTT protokolünü kullanarak IOT hub ile iletişim Kur](iot-hub-mqtt-support.md).
