---
title: "Azure IOT hub'ı Azure Event hubs'a karşılaştırma | Microsoft Docs"
description: "İşlevsel farklılıklar ve kullanım örnekleri vurgulama IOT Hub ve olay hub'ları Azure Hizmetleri karşılaştırması. Desteklenen protokoller, cihaz yönetimi, izleme, karşılaştırma içerir ve dosyayı yükler."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: b515e05d16dda83c7d865113d5d3578c44be084f
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Azure IOT Hub ve Azure Event Hubs karşılaştırması
Ana kullanım durumları için IOT hub'ı telemetri aygıtlardan toplamak için biridir. Bu nedenle, IOT hub'ı genellikle karşılaştırılır [Azure Event Hubs][Azure Event Hubs]. IOT Hub gibi olay hub'ları düşük gecikme süreli ve yüksek güvenilirlikle bulutta büyük ölçekte olay ve telemetri girişi sağlayan hizmetini işleme bir olaydır.

Ancak, Hizmetleri aşağıdaki tabloda ayrıntılı birçok farklar vardır:

| Alan | IoT Hub’ı | Event Hubs |
| --- | --- | --- |
| Kimlik doğrulaması desenleri | Etkinleştirir [cihaz bulut iletişimleri] [ lnk-d2c-guidance] (yüklemeleri ve bildirilen özellikleri Mesajlaşma, dosya) ve [bulut-cihaz iletişimi] [ lnk-c2d-guidance] (doğrudan yöntemleri, Mesajlaşma istenen özellikleri). |Yalnızca olay giriş (genellikle cihaz bulut senaryoları için kabul) sağlar. |
| Cihaz durumu bilgileri | [Cihaz çiftlerini] [ lnk-twins] depolayabilir ve sorgu aygıt durum bilgileri. | Aygıt durum bilgisi depolanabilir. |
| Cihaz protokol desteği |MQTT, MQTT WebSockets, AMQP, WebSockets ve HTTPS üzerinden AMQP üzerinden destekler. Ayrıca, IOT Hub ile çalışır [Azure IOT protokolü ağ geçidini][lnk-azure-protocol-gateway], özel protokoller desteklemek üzere özelleştirilebilir protokol ağ geçidi uygulanması. |AMQP, AMQP WebSockets ve HTTPS üzerinden destekler. |
| Güvenlik |Cihaz başına kimlik ve iptal edilebilir erişim denetimi sağlar. Bkz: [IOT Hub Geliştirici Kılavuzu'nun güvenlik bölümüne]. |Olay hub'ları genelinde sağlar [paylaşılan erişim ilkeleri][Event Hubs - security], ile sınırlı iptalini desteklemek aracılığıyla [publisher'ın ilkeleri][Event Hubs publisher policies]. IOT çözümleri genellikle cihaz başına kimlik bilgilerini ve ölçüleri yanıltma desteklemek için özel bir çözümü uygulamak için gereklidir. |
| İşlemleri izleme |Cihaz kimlik yönetimi ve bağlantı olayları tek tek cihaz kimlik doğrulama hataları, azaltma ve hatalı biçim özel durumlar gibi zengin bir dizi abone olmak IOT çözümleri sağlar. Bu olayları tek tek cihaz düzeyinde bağlantısı sorunları hızla tanımlamanıza olanak sağlar. |Yalnızca toplama ölçümleri kullanıma sunar. |
| Ölçek |Eş zamanlı aygıtların milyonlarca desteği için optimize edilmiştir. |Bağlantıları göre meters [Azure Event Hubs kotaları][Azure Event Hubs quotas]. Diğer taraftan, olay hub'ları gönderilen her ileti için bölüm belirtmenize olanak sağlar. |
| Cihaz SDK'ları |Sağlar [cihaz SDK'ları] [ Azure IoT SDKs] büyük çeşitli platformlar ve doğrudan MQTT, AMQP ve HTTPS API'leri ek dil için. |Ayrıca .NET, Java ve C AMQP ve HTTPS gönderme arabirimleri için desteklenir. |
| Karşıya dosya yükleme |IOT çözümleri bulut aygıtlardan dosyaları yüklemesine olanak sağlar. İş akışı tümleştirme ve hata ayıklama desteği için kategori izleme işlemleri için bir dosya bildirim uç noktası içerir. | Desteklenmiyor. |
| İletileri yönlendirmek için birden fazla uç noktası | En fazla 10 özel uç noktaları desteklenir. Kurallar, iletileri özel uç noktaları için nasıl yönlendirileceğini belirler. Daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine][lnk-devguide-messaging]. | Yazılan ve ileti gönderme için barındırılan için ek kod gerektirir. |

Yalnızca kullanım örneği cihaz bulut telemetri giriş olsa bile Özet olarak, IOT hub'ı IOT cihaz bağlantısı için tasarlanmış bir hizmet sağlar. Bu senaryolar IOT özgü özelliklerle değer önermeleri genişletmek devam eder. Event Hubs olay Giriş bir büyük ölçekte, ağlar arası veri merkezi ve içi veri merkezi senaryoları bağlamında her ikisi için tasarlanmıştır.

IOT Hub ve Event Hubs aynı çözüm içinde kullanmak için seyrek değil. Olay hub'ları aşama sonraki olay giriş gerçek zamanlı işleme altyapısı işler ve IOT Hub cihaz bulut iletişimi işler.

### <a name="next-steps"></a>Sonraki adımlar
IOT hub'ı dağıtımınızı planlama hakkında daha fazla bilgi için bkz: [ölçeklendirme, HA ve DR][lnk-scaling].

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [AI ile Azure IOT kenar sınır cihazları için dağıtma][lnk-iotedge]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[IOT Hub Geliştirici Kılavuzu'nun güvenlik bölümüne]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-features.md#event-publishers
[Azure Event Hubs quotas]: ../event-hubs/event-hubs-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
