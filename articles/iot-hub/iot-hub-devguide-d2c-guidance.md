---
title: Azure IOT Hub CİHAZDAN buluta seçenekleri | Microsoft Docs
description: Geliştirici Kılavuzu - yönergeler ne zaman CİHAZDAN buluta iletileri, bildirilen özellikleri veya dosyayı karşıya yükleme bulut-cihaz iletişimi için kullanılır.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: fffa064b912a96b05feb901d1d2d44533c4681b7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60885525"
---
# <a name="device-to-cloud-communications-guidance"></a>CİHAZDAN buluta iletişim Kılavuzu

Ne zaman çözüm arka ucu, IOT Hub cihaz uygulamasından gönderme bilgilerini üç seçenek sunar:

* [CİHAZDAN buluta iletileri](iot-hub-devguide-messages-d2c.md) zaman serisi telemetri ve Uyarılar için.

* [Cihaz çiftinin bildirilen özelliklerini](iot-hub-devguide-device-twins.md) kullanılabilir yetenekler, koşullar veya uzun süre çalışan iş akışları durumu gibi cihaz durumu bilgilerini raporlama. Örneğin, yapılandırma ve yazılım güncelleştirmeleri.

* [Dosya karşıya yüklemeleri](iot-hub-devguide-file-upload.md) medya için dosyaların ve büyük telemetri toplu işler tarafından aralıklı olarak bağlanan cihazlarla karşıya veya bant genişliğinden tasarruf etmek sıkıştırılmış.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Çeşitli cihaz-bulut iletişim seçenekleri ayrıntılı bir karşılaştırması aşağıdadır.

|  | Cihazdan buluta iletiler | Cihaz ikizinin bildirilen özellikleri | Dosya yüklemeleri |
| ---- | ------- | ---------- | ---- |
| Senaryo | Zaman serisi telemetri ve uyarılar. Örneğin, her 5 dakikada 256 KB'lık algılayıcı veri yığını gönderilir. | Kullanılabilir yetenekler ve koşullar. Örneğin, geçerli cihaz bağlantı modunu hücresel gibi veya WiFi. Yapılandırma ve yazılım güncelleştirmeleri gibi uzun süre çalışan iş akışları eşitleniyor. | Medya dosyaları. Büyük (genellikle sıkıştırılmış) telemetri toplu işler. |
| Depolama ve alma | Geçici olarak IOT Hub tarafından en fazla 7 gün olarak depolanır. Yalnızca sıralı okuma. | IOT Hub tarafından cihaz çiftine depolanır. Alınabilir kullanarak [IOT Hub sorgu dili](iot-hub-devguide-query-language.md). | Kullanıcı tarafından sağlanan Azure depolama hesabında depolanır. |
| Boyut | En fazla 256 KB'lık iletileri. | En çok bildirilen özellikler, 8 KB'lık boyutudur. | Azure Blob Depolama tarafından desteklenen en büyük dosya boyutu. |
| Sıklık | Yüksek. Daha fazla bilgi için [IOT hub'ı sınırlar](iot-hub-devguide-quotas-throttling.md). | Orta. Daha fazla bilgi için [IOT hub'ı sınırlar](iot-hub-devguide-quotas-throttling.md). | Düşük. Daha fazla bilgi için [IOT hub'ı sınırlar](iot-hub-devguide-quotas-throttling.md). |
| Protokol | Tüm protokoller kullanılabilir. | MQTT veya AMQP kullanarak kullanılabilir. | Cihazda gerektirir ancak herhangi bir protokolünü kullanarak HTTPS olduğunda kullanılabilir. |

Bir uygulamanın telemetri zaman serisi veya uyarı olarak hem bilgi göndermek ve cihaz ikizinde kullanılabilir hale getirmek gerekebilir. Bu senaryoda, aşağıdaki seçeneklerden birini seçebilirsiniz:

* Cihaz uygulaması, bir CİHAZDAN buluta ileti gönderir ve bir özellik değişiminin raporlar.
* İleti aldığında çözüm arka ucu, cihaz ikizinin etiketleri bilgileri depolayabilirsiniz.

Cihaz ikizi güncelleştirmeleri daha bir çok daha yüksek performans CİHAZDAN buluta iletileri etkinleştirme olduğundan, bazen her CİHAZDAN buluta ileti için cihaz ikizi güncelleştirme önlemek için tercih edilir.