---
title: Azure IOT hub'ı bulut-cihaz seçenekleri | Microsoft Docs
description: Geliştirici Kılavuzu - yönergeler ne zaman doğrudan yöntemler, cihaz ikizinin istenen özellikleri veya Bulut-cihaz iletilerini bulut-cihaz iletişimi için kullanılır.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: 4b738f34ae75478c0120832e7ad2b6a6a83dbf69
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224787"
---
# <a name="cloud-to-device-communications-guidance"></a>Bulut buluttan cihaza iletişim Kılavuzu

IOT hub'ı, arka uç uygulaması için işlevselliği göstermek cihaz uygulamaları için üç seçenek sunulur:

* [Doğrudan yöntemler](iot-hub-devguide-direct-methods.md) sonucun anında onay gerektiren iletişimler için. Doğrudan yöntemler genellikle cihazların fan üzerinde kapatma gibi etkileşimli denetimi için kullanılır.

* [İkiz özellikleri istenen](iot-hub-devguide-device-twins.md) cihaz belirli bir yerleştirmek için amaçlanan uzun süren komutları durumu istenen için. Örneğin, telemetri gönderme aralığı 30 dakikaya ayarlayın.

* [Bulut-cihaz iletilerini](iot-hub-devguide-messages-c2d.md) cihaz uygulaması için tek yönlü bildirimleri.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Çeşitli bulut-cihaz iletişim seçenekleri ayrıntılı bir karşılaştırması aşağıdadır.

|  | Doğrudan yöntemler | İkizinin istenen özellikleri | Bulut-cihaz iletilerini |
| ---- | ------- | ---------- | ---- |
| Senaryo | Fan üzerinde kapatma gibi anında onay gerektiren komutlar içerir. | Uzun süre çalışan komutlar, cihaz bir belirli istenen duruma getirmeyi yöneliktir. Örneğin, telemetri gönderme aralığı 30 dakikaya ayarlayın. | Cihaz uygulaması için tek yönlü bildirimleri. |
| Veri akışı | İki yönlü. Cihaz uygulamasını yönteme hemen yanıt verebilir. Çözüm arka ucu sonucu bağlamsal isteği alır. | Tek yönlü. Cihaz uygulamasını özellik değişikliği içeren bir bildirim alır. | Tek yönlü. Cihaz uygulamasını iletiyi alır.
| Dayanıklılık | Bağlantısı kesilmiş cihazlar kurulmadı. Çözüm arka ucu, cihaz bağlı değil olarak bildirilir. | Özellik değerleri, cihaz ikizinde korunur. Cihaz, sonraki yeniden bağlanma sırasında okuyun. Özellik değerleri ile alınabilir [IOT Hub sorgu dili](iot-hub-devguide-query-language.md). | İletileri IOT Hub tarafından 48 saate kadar saklanabilir. |
| Hedefler | Tek bir cihaz kullanarak **DeviceID**, veya birden çok cihazı kullanarak [işleri](iot-hub-devguide-jobs.md). | Tek bir cihaz kullanarak **DeviceID**, veya birden çok cihazı kullanarak [işleri](iot-hub-devguide-jobs.md). | Tek bir cihaz tarafından **DeviceID**. |
| Boyut | En fazla bir doğrudan yöntem yükünün boyutu 128 KB ' dir. | En fazla 8 KB'lık özellikleri boyutudur istenen. | En fazla 64 KB iletileri. |
| Sıklık | Yüksek. Daha fazla bilgi için [IOT hub'ı sınırlar](iot-hub-devguide-quotas-throttling.md). | Orta. Daha fazla bilgi için [IOT hub'ı sınırlar](iot-hub-devguide-quotas-throttling.md). | Düşük. Daha fazla bilgi için [IOT hub'ı sınırlar](iot-hub-devguide-quotas-throttling.md). |
| Protokol | MQTT veya AMQP kullanarak kullanılabilir. | MQTT veya AMQP kullanarak kullanılabilir. | Tüm protokoller kullanılabilir. Cihaz, HTTPS kullanırken yoklaması gerekir. |

Aşağıdaki öğreticilerde doğrudan yöntemler, istenen özellikleri ve bulut-cihaz iletilerini kullanmayı öğrenin:

* [Doğrudan yöntemler kullanma](quickstart-control-device-node.md)
* [Cihazları yapılandırmak için istenen özellikleri kullanın](tutorial-device-twins.md) 
* [Bulut-cihaz iletilerini gönder](iot-hub-node-node-c2d.md)