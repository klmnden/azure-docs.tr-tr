---
title: Azure IOT Hub Mesajlaşma anlama | Microsoft Docs
description: Geliştirici Kılavuzu - CİHAZDAN buluta ve bulut-cihaz IOT Hub ile ileti gönderme. İleti biçimleri ve desteklenen protokolleri hakkında bilgi içerir.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: b0667f820145f16c75a07ebe1849e20d2de36cc7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185518"
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>CİHAZDAN buluta ve bulut-cihaz IOT Hub ile ileti gönderme

IOT Hub, cihazlarınız tarafından ile iletişim kurmak için Mesajlaşma kullanın:

* Gönderme [CİHAZDAN buluta] [ lnk-d2c] çözümünüze cihazlarınızdan gelen iletileri arka ucu.
* Gönderme [bulut-cihaz] [ lnk-c2d] iletileri çözüm arka ucu cihazlarınıza.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Çekirdek özellikleri IOT Hub Mesajlaşma işlevlerin güvenilirlik ve dayanıklılık iletilerinin ' dir. Bu özellikler cihaz tarafında ve olay işleme bulut tarafta artış yüklemek için aralıklı bağlantı dayanıklılığı sağlar. IOT hub'ı uygulayan *en az bir kez* teslim hem bulut-cihaz, hem de CİHAZDAN buluta iletileri için güvence altına alır.

Giriş, IOT Hub için özelliklere bakın [Azure IOT Hub hizmetine genel bakış][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>IOT Hub Mesajlaşma kullanıldığı durumlar

Zaman serisi telemetri ve uyarılar cihaz uygulamanızdan göndermek için CİHAZDAN buluta iletileri ve bulut-cihaz iletilerini cihaz uygulamanız için tek yönlü bildirimler için kullanın.

* Başvurmak [CİHAZDAN buluta iletişim Kılavuzu] [ lnk-d2c-guidance] CİHAZDAN buluta iletileri, bildirilen özellikleri veya karşıya dosya yükleme'nın kullanımı arasındaki şüpheli ise.
* Başvurmak [bulut buluttan cihaza iletişim Kılavuzu] [ lnk-c2d-guidance] bulut-cihaz iletilerini, istenen özellikleri veya doğrudan yöntemler kullanma arasında şüpheli ise.

## <a name="next-steps"></a>Sonraki adımlar

* IOT hub'ı hakkında bilgi edinin [CİHAZDAN buluta ileti][lnk-d2c].
* IOT hub'ı hakkında bilgi edinin [bulut-cihaz Mesajlaşma][lnk-c2d].

[lnk-azure-iot]: ../iot-fundamentals/index.yml
[lnk-iot-hub-overview]: about-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md