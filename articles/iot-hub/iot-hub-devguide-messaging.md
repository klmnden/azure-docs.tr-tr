---
title: Azure IOT Hub Mesajlaşma anlama | Microsoft Docs
description: Geliştirici Kılavuzu - cihaz Bulut ve bulut-cihaz IOT Hub ile ileti. İleti biçimleri ve desteklenen protokolleri hakkında bilgi içerir.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 451a8226bbc52727dad562a4be352e352925bd0b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632715"
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Cihaz Bulut ve bulut-cihaz IOT Hub ile Mesajlaşma

IOT Hub'ın cihazlar tarafından ile iletişim kurmak için Mesajlaşma kullanın:

* Gönderme [cihaz bulut] [ lnk-d2c] cihazlarınızdan gelen iletileri çözümünüze son yedekleme.
* Gönderme [bulut cihaz] [ lnk-c2d] çözümü iletilerden aygıtlarınıza son yedekleme.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Çekirdek IOT hub'ı Mesajlaşma işlevleri güvenilirlik ve dayanıklılık iletilerinin özellikleridir. Bu özellikleri aralıklı bağlantısı cihaz tarafındaki ve olay bulut tarafında işleme ani yüklemek için esneklik sağlar. IOT hub'ı uygulayan *en az bir kez* teslim cihaz Bulut ve bulut-cihaz Mesajlaşma güvence altına alır.

Giriş IOT hub için özelliklere bakın [Azure IOT Hub hizmetine genel bakış][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>IOT Hub'ın Mesajlaşma kullanma zamanı

Zaman serisi telemetri ve uyarılar, cihaz uygulamanızdan göndermek için cihaz bulut iletilerini ve bulut-cihaz iletilerini cihaz uygulamanız için tek yönlü bildirimler için kullanın.

* Başvurmak [cihaz bulut iletişimi Kılavuzu] [ lnk-d2c-guidance] IF cihaz bulut iletilerini, bildirilen özellikleri veya karşıya dosya yükleme kullanarak arasında şüpheli.
* Başvurmak [bulut-cihaz iletişimi Kılavuzu] [ lnk-c2d-guidance] IF bulut-cihaz iletilerini, istenen özelliklerini veya doğrudan yöntemlerini kullanarak arasında şüpheli.

## <a name="next-steps"></a>Sonraki adımlar

* IOT Hub hakkında bilgi edinin [cihaz bulut Mesajlaşma][lnk-d2c].
* IOT Hub hakkında bilgi edinin [bulut cihaz Mesajlaşma][lnk-c2d].

[lnk-azure-iot]: ../iot-fundamentals/index.yml
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md