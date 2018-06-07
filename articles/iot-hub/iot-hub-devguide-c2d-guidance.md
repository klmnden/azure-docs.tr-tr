---
title: Azure IOT hub'ı bulut aygıt seçenekleri | Microsoft Docs
description: Geliştirici Kılavuzu - Kılavuzu ne zaman doğrudan yöntemleri, cihaz çifti'nin istediğiniz özellikler veya Bulut-cihaz iletilerini bulut-cihaz iletişimi için kullanılır.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: ff81be4bbf6d297c623c5d98b5dc22a540112fcc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634446"
---
# <a name="cloud-to-device-communications-guidance"></a>Bulut-cihaz iletişimi Kılavuzu
IOT Hub cihaz uygulamaların bir arka uç uygulamasının işlevselliğini göstermek üç seçenek sunar:

* [Doğrudan yöntemleri] [ lnk-methods] sonucunun hemen onay gerektiren iletişimleri için. Doğrudan yöntemleri genellikle üzerinde fan kapatma gibi aygıtların etkileşimli denetimi için kullanılır.
* [Twin özellikleri istenen] [ lnk-twins] cihaz belirli bir yerleştirilecek hedeflenen uzun süre çalışan komutlar durumu istenen için. Örneğin, telemetri gönderme aralığı 30 dakikaya ayarlayın.
* [Bulut-cihaz iletilerini] [ lnk-c2d] cihaz uygulaması için tek yönlü bildirimler için.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Aşağıda, çeşitli bulut-cihaz iletişimi seçenekleri ayrıntılı karşılaştırması verilmiştir.

|  | Doğrudan yöntemler | Twin'ın istenen özellikleri | Bulut-cihaz iletilerini |
| ---- | ------- | ---------- | ---- |
| Senaryo | Fan üzerinde kapatma gibi hemen onay gerektiren komutlar. | Uzun süre çalışan komutlar, cihaz bir belirli istenen duruma amacı. Örneğin, telemetri gönderme aralığı 30 dakikaya ayarlayın. | Cihaz uygulaması tek yönlü bildirimleri. |
| Veri akışı | İki yönlü. Cihaz uygulaması yönteme hemen yanıt verebilir. Çözüm arka ucu bağlam için isteğin sonucu alır. | Tek yönlü. Cihaz uygulaması özellik değişikliği içeren bir bildirim alır. | Tek yönlü. Cihaz uygulaması iletiyi alır
| Dayanıklılık | Bağlantısı kesilmiş aygıtları temas değil. Çözüm arka ucu cihaz bağlanmamış bildirilir. | Özellik değerlerini cihaz çiftine korunur. Aygıt, sonraki yeniden bağlanma sırasında okuyun. Özellik değerleri ile alınabilir [IOT hub'ı sorgu dili][lnk-query]. | İletilerini 48 saate kadar IOT Hub tarafından korunabilir. |
| Hedefler | Tek bir cihazla **DeviceID**, veya birden çok cihazı kullanarak [işleri][lnk-jobs]. | Tek bir cihazla **DeviceID**, veya birden çok cihazı kullanarak [işleri][lnk-jobs]. | Tek aygıt tarafından **DeviceID**. |
| Boyut | En fazla doğrudan yöntemi yükü boyutu 128 KB'tır. | Maksimum özellikleri boyutu 8 KB istenen. | En fazla 64 KB iletileri. |
| Sıklık | Yüksek. Daha fazla bilgi için bkz: [IOT hub'ı sınırlar][lnk-quotas]. | Orta. Daha fazla bilgi için bkz: [IOT hub'ı sınırlar][lnk-quotas]. | Düşük. Daha fazla bilgi için bkz: [IOT hub'ı sınırlar][lnk-quotas]. |
| Protokol | MQTT veya AMQP kullanarak kullanılabilir. | MQTT veya AMQP kullanarak kullanılabilir. | Tüm protokoller kullanılabilir. Aygıt, HTTPS kullanırken yoklaması gerekir. |

Aşağıdaki öğreticilerde doğrudan yöntemler, istenen özellikler ve bulut-cihaz iletilerini kullanmayı öğrenin:

* [Doğrudan yöntemleri kullanın][lnk-methods-tutorial], doğrudan yöntemleri;
* [Cihazları yapılandırmak için istenen özellikleri kullanmak][lnk-twin-properties], cihaz çifti özellikleri; istenen için 
* [Bulut-cihaz iletilerini göndermek][lnk-c2d-tutorial], bulut cihaz iletileri için.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md
