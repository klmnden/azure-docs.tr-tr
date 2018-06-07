---
title: Azure IOT hub'ı iletişim protokolleri ve bağlantı noktaları | Microsoft Docs
description: Geliştirici Kılavuzu - cihaz Bulut ve bulut-cihaz iletişimi ve açık bağlantı noktası numaraları için desteklenen iletişim protokolleri açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 0fe3dd719877dac23410ff1ca00d559636a5ed60
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633011"
---
# <a name="reference---choose-a-communication-protocol"></a>Başvuru - iletişim protokolü seçin

IOT Hub cihaz tarafındaki iletişimleri için aşağıdaki protokolleri cihazların sağlar:

* [MQTT][lnk-mqtt]
* WebSockets üzerinden MQTT
* [AMQP][lnk-amqp]
* WebSockets üzerinden AMQP
* HTTPS

Bu protokollerin belirli IOT Hub özellikleri nasıl desteği hakkında daha fazla bilgi için bkz: [cihaz bulut iletişimleri Kılavuzu] [ lnk-d2c-guidance] ve [bulut-cihaz iletişimi Kılavuzu] [lnk-c2d-guidance].

Aşağıdaki tabloda, tercih ettiğiniz Protokolü yönelik üst düzey öneriler sağlar:

| Protokol | Bu protokol seçtiğinizde |
| --- | --- |
| MQTT <br> WebSocket üzerinden MQTT |Birden çok aygıt (her kendi cihaz başına kimlik bilgileriyle) aynı TLS bağlantısı üzerinden bağlanmak için gerekli değil tüm cihazlarda kullanın. |
| AMQP <br> AMQP WebSocket üzerinden |Aygıtlarda çoğullama bağlantı yararlanmak için alan ve bulut ağ geçidi kullanın. |
| HTTPS |Diğer protokolleri destekleyemez cihazlar için kullanın. |

Cihaz tarafındaki iletişimleri için protokol seçtiğinizde aşağıdaki noktaları göz önünde bulundurun:

* **Bulut cihaz düzeni**. HTTPS server itme uygulamak için etkili bir yol yok. Bu nedenle, HTTPS kullanırken, cihazlar IOT Hub için bulut-cihaz iletilerini yoklar. Bu yaklaşım aygıt ve IOT Hub için yetersiz olduğunu. Geçerli HTTPS yönergeleri altında her aygıt için iletileri 25 dakikada bir veya daha fazla yoklama. MQTT ve AMQP sunucu itme bulut-cihaz iletilerini alırken destekler. Bunlar, anında bildirim iletilerinin IOT hub'dan cihaz için etkinleştirin. Teslim gecikme ilgili bir sorun varsa, MQTT veya AMQP kullanmak için en iyi protokolleri edilir. Nadiren bağlanan cihazlar için HTTPS de çalışır.
* **Alan ağ geçitleri**. MQTT ve HTTPS kullanırken, birden çok aygıt (her kendi cihaz başına kimlik bilgileriyle) bağlanamıyor aynı TLS bağlantısı kullanarak. İçin [alan ağ geçidi senaryoları] [ lnk-azure-gateway-guidance] bağlı her cihaz için alan ağ geçidi ve IOT hub'ı arasında bir TLS bağlantısı gerektirir, bu protokollere yetersiz.
* **Düşük kaynak aygıtları**. MQTT ve HTTPS kitaplıkları AMQP kitaplıkları daha küçük bir yer vardır. Cihaz kaynakları (örneğin, değerinden 1 MB RAM) sınırlıdır, bu nedenle, bu protokollere yalnızca protokol uygulanması olabilir.
* **Ağ geçişi**. Bağlantı noktası 5671 standart AMQP protokolünü kullanır ve MQTT 8883 bağlantı noktasını dinler. Bu bağlantı noktalarının kullanımını HTTPS dışındaki protokoller için kapalı ağlarında sorunlara neden olabilir. MQTT WebSockets, AMQP WebSockets veya HTTPS üzerinden bu senaryoda kullanın.
* **Yükü boyutu**. MQTT ve AMQP HTTPS değerinden daha küçük yüklerini sonucunda ikili protokoller şunlardır.

> [!WARNING]
> HTTPS kullanırken, her cihaz için bulut-cihaz iletilerini 25 dakikada bir veya daha fazla yoklama. Ancak, geliştirme sırasında 25 dakikada daha sık yoklamak için kabul edilebilir.

## <a name="port-numbers"></a>Bağlantı noktası numaraları

Aygıtları çeşitli protokoller kullanarak Azure IOT Hub ile iletişim kurabilir. Genellikle, Protokol seçimi çözümü belirli gereksinimleri tarafından yönetilir. Aşağıdaki tabloda, belirli bir iletişim kuralı kullanabilmek bir cihaz için açık olmalıdır giden bağlantı noktalarını listeler:

| Protokol | Bağlantı noktası |
| --- | --- |
| MQTT |8883 |
| WebSockets üzerinden MQTT |443 |
| AMQP |5671 |
| WebSockets üzerinden AMQP |443 |
| HTTPS |443 |

Bir Azure bölgesinde bir IOT hub'ı oluşturduğunuzda, IOT hub'ı bu IOT hub'ın ömrü boyunca aynı IP adresini tutar. Ancak, Microsoft hizmet kalitesini korumak için farklı ölçek birimi için IOT hub'ı taşınırsa, yeni bir IP adresi atanmış olduğu.


## <a name="next-steps"></a>Sonraki adımlar

IOT hub'ı MQTT Protokolü nasıl uyguladığını hakkında daha fazla bilgi için bkz: [MQTT protokolünü kullanarak, IOT hub ile iletişim kurun][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
