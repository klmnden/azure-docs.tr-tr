---
title: Azure IOT Hub cihaz bulut seçeneklerini | Microsoft Docs
description: Geliştirici Kılavuzu - Kılavuzu ne zaman cihaz bulut iletilerini, bildirilen özellikleri veya karşıya dosya yükleme bulut-cihaz iletişimi için kullanılır.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: a1881b74ba3f4f66e9e47b24d2ee8c8c17c5f05d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634225"
---
# <a name="device-to-cloud-communications-guidance"></a>Cihaz bulut iletişimleri Kılavuzu
Ne zaman çözüm arka ucu, IOT Hub cihaz uygulaması gönderen bilgileriyle üç seçenek sunar:

* [Cihaz bulut iletilerini] [ lnk-d2c] zaman serisi telemetri ve Uyarılar için.
* [Cihaz çifti özellikleri bildirilen] [ lnk-twins] kullanılabilir özellikleri, koşullar veya uzun süre çalışan iş akışları durumu gibi cihaz durumu bilgilerini raporlama. Örneğin, yapılandırma ve yazılım güncelleştirmeleri.
* [Dosya yüklemeleri] [ lnk-fileupload] ortam dosyaları ve büyük telemetri toplu aralıklı bağlı cihazlar tarafından karşıya veya bant genişliğini korumak amacıyla sıkıştırılmış.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Aşağıda, çeşitli cihaz bulut iletişim seçenekleri ayrıntılı karşılaştırması verilmiştir.

|  | Cihazdan buluta iletiler | Cihaz çifti'nın bildirilen özellikleri | Dosya yüklemeleri |
| ---- | ------- | ---------- | ---- |
| Senaryo | Telemetri zaman serisi ve Uyarıları. Örneğin, 5 dakikada 256 KB algılayıcı veri yığını gönderdi. | Kullanılabilir özellikler ve koşulları. Örneğin, geçerli cihaz bağlantı modunu cep gibi veya WiFi. Uzun süre çalışan iş akışları, yapılandırma ve yazılım güncelleştirmeleri gibi eşitleniyor. | Ortam dosyaları. Büyük (genellikle sıkıştırılmış) telemetri toplu işler. |
| Depolama ve alma | Geçici olarak IOT Hub tarafından en fazla 7 gün için depolanır. Yalnızca sıralı okuma. | IOT Hub tarafından cihaz çiftine depolanır. Alınabilir kullanarak [IOT hub'ı sorgu dili][lnk-query]. | Kullanıcı tarafından sağlanan Azure depolama hesabında depolanır. |
| Boyut | En fazla 256 KB iletileri. | En fazla bildirilen özellikleri boyutu 8 KB'tır. | Azure Blob Storage tarafından desteklenen en büyük dosya boyutu. |
| Sıklık | Yüksek. Daha fazla bilgi için bkz: [IOT hub'ı sınırlar][lnk-quotas]. | Orta. Daha fazla bilgi için bkz: [IOT hub'ı sınırlar][lnk-quotas]. | Düşük. Daha fazla bilgi için bkz: [IOT hub'ı sınırlar][lnk-quotas]. |
| Protokol | Tüm protokoller kullanılabilir. | MQTT veya AMQP kullanarak kullanılabilir. | Cihazda HTTPS gerektirir ancak herhangi bir protokolünü kullanarak olduğunda kullanılabilir. |

Uygulama bilgileri hem de bir telemetri zaman serisi veya uyarı olarak göndermek ve cihaz çiftine kullanılabilmesini gerekebilir. Bu senaryoda, aşağıdaki seçeneklerden birini seçebilirsiniz:

* Cihaz uygulaması cihaz bulut ileti gönderir ve özellik değişikliği raporlar.
* İleti aldığında, çözüm arka ucu cihaz çifti'nın etiketlerinde bilgilerini depolayabilir.

Cihaz çifti güncelleştirmeleri daha bir çok daha yüksek verimlilik cihaz bulut iletilerini etkinleştir olduğundan, bazen her cihaz bulut ileti cihaz çiftinin güncelleştirme önlemek için önerilir.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
