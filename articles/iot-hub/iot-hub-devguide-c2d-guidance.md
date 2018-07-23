---
title: Azure IOT hub'ı bulut-cihaz seçenekleri | Microsoft Docs
description: Geliştirici Kılavuzu - yönergeler ne zaman doğrudan yöntemler, cihaz ikizinin istenen özellikleri veya Bulut-cihaz iletilerini bulut-cihaz iletişimi için kullanılır.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: 2cc9bd39371741caaa3ae025df494e225dc754b0
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187045"
---
# <a name="cloud-to-device-communications-guidance"></a>Bulut buluttan cihaza iletişim Kılavuzu
IOT hub'ı, arka uç uygulaması için işlevselliği göstermek cihaz uygulamaları için üç seçenek sunulur:

* [Doğrudan yöntemler] [ lnk-methods] sonucun anında onay gerektiren iletişimler için. Doğrudan yöntemler genellikle cihazların fan üzerinde kapatma gibi etkileşimli denetimi için kullanılır.
* [İkiz özellikleri istenen] [ lnk-twins] cihaz belirli bir yerleştirmek için amaçlanan uzun süren komutları durumu istenen için. Örneğin, telemetri gönderme aralığı 30 dakikaya ayarlayın.
* [Bulut-cihaz iletilerini] [ lnk-c2d] cihaz uygulaması için tek yönlü bildirimleri.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Çeşitli bulut-cihaz iletişim seçenekleri ayrıntılı bir karşılaştırması aşağıdadır.

|  | Doğrudan yöntemler | İkizinin istenen özellikleri | Bulut-cihaz iletilerini |
| ---- | ------- | ---------- | ---- |
| Senaryo | Fan üzerinde kapatma gibi anında onay gerektiren komutlar içerir. | Uzun süre çalışan komutlar, cihaz bir belirli istenen duruma getirmeyi yöneliktir. Örneğin, telemetri gönderme aralığı 30 dakikaya ayarlayın. | Cihaz uygulaması için tek yönlü bildirimleri. |
| Veri akışı | İki yönlü. Cihaz uygulamasını yönteme hemen yanıt verebilir. Çözüm arka ucu sonucu bağlamsal isteği alır. | Tek yönlü. Cihaz uygulamasını özellik değişikliği içeren bir bildirim alır. | Tek yönlü. Cihaz uygulamasını iletiyi alır.
| Dayanıklılık | Bağlantısı kesilmiş cihazlar kurulmadı. Çözüm arka ucu, cihaz bağlı değil olarak bildirilir. | Özellik değerleri, cihaz ikizinde korunur. Cihaz, sonraki yeniden bağlanma sırasında okuyun. Özellik değerleri ile alınabilir [IOT Hub sorgu dili][lnk-query]. | İletileri IOT Hub tarafından 48 saate kadar saklanabilir. |
| Hedefler | Tek bir cihaz kullanarak **DeviceID**, veya birden çok cihazı kullanarak [işleri][lnk-jobs]. | Tek bir cihaz kullanarak **DeviceID**, veya birden çok cihazı kullanarak [işleri][lnk-jobs]. | Tek bir cihaz tarafından **DeviceID**. |
| Boyut | En fazla bir doğrudan yöntem yükünün boyutu 128 KB ' dir. | En fazla 8 KB'lık özellikleri boyutudur istenen. | En fazla 64 KB iletileri. |
| Sıklık | Yüksek. Daha fazla bilgi için [IOT hub'ı sınırlar][lnk-quotas]. | Orta. Daha fazla bilgi için [IOT hub'ı sınırlar][lnk-quotas]. | Düşük. Daha fazla bilgi için [IOT hub'ı sınırlar][lnk-quotas]. |
| Protokol | MQTT veya AMQP kullanarak kullanılabilir. | MQTT veya AMQP kullanarak kullanılabilir. | Tüm protokoller kullanılabilir. Cihaz, HTTPS kullanırken yoklaması gerekir. |

Aşağıdaki öğreticilerde doğrudan yöntemler, istenen özellikleri ve bulut-cihaz iletilerini kullanmayı öğrenin:

* [Doğrudan yöntemler kullanma][lnk-methods-tutorial], doğrudan yöntemler için;
* [Cihazları yapılandırmak için istenen özellikleri kullanın][lnk-twin-properties]için cihaz ikizinin istenen özellikleri; 
* [Bulut-cihaz iletilerini göndermek][lnk-c2d-tutorial], bulut-cihaz iletilerini için.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: quickstart-control-device-node.md
[lnk-twin-properties]: tutorial-device-twins.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md
