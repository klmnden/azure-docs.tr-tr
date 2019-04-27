---
title: Azure IOT Hub Mesajlaşma anlama | Microsoft Docs
description: Geliştirici Kılavuzu - CİHAZDAN buluta ve bulut-cihaz IOT Hub ile ileti gönderme. İleti biçimleri ve desteklenen protokolleri hakkında bilgi içerir.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: f56332fa7f53c729ffaa28ea375f043d1b4a3678
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60626256"
---
# <a name="send-device-to-cloud-and-cloud-to-device-messages-with-iot-hub"></a>IOT hub'ına CİHAZDAN buluta ve bulut-cihaz iletilerini göndermek

IOT hub'ı cihazlarınızla için çift yönlü iletişim sağlar. Kullanım IOT Hub'ın cihazlarınıza gelen iletiler göndererek cihazlarınızla iletişim kurmak için Mesajlaşma çözümlerinizi geri sona IOT komut gönderme çözüm arka ucu ve cihazlarınıza. Daha fazla bilgi edinin [IOT Hub ileti biçimi](iot-hub-devguide-messages-construct.md).

## <a name="sending-device-to-cloud-messages-to-iot-hub"></a>IOT Hub'ına CİHAZDAN buluta ileti gönderme

IOT Hub, cihazlarınızdan telemetri iletilerini okumak için arka uç Hizmetleri tarafından kullanılan bir yerleşik hizmet uç noktası vardır. Bu uç nokta ile uyumlu [Event Hubs](https://docs.microsoft.com/azure/event-hubs/) ve standart IOT Hub SDK'ları için kullanabileceğiniz [bu yerleşik uç noktadan okuma](iot-hub-devguide-messages-read-builtin.md).

IOT hub'ı da destekler [özel uç noktalar](iot-hub-devguide-endpoints.md#custom-endpoints) kullanarak Azure Hizmetleri için cihaz telemetri verileri ve olayları göndermek için kullanıcılar tarafından tanımlanabilir [ileti yönlendirme](iot-hub-devguide-messages-d2c.md).

## <a name="sending-cloud-to-device-messages-from-iot-hub"></a>IOT Hub'ından bulut buluttan cihaza iletileri gönderme

Gönderebilirsiniz [bulut-cihaz](iot-hub-devguide-messages-c2d.md) iletileri çözüm arka ucu cihazlarınıza.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Çekirdek özellikleri IOT Hub Mesajlaşma işlevlerin güvenilirlik ve dayanıklılık iletilerinin ' dir. Bu özellikler cihaz tarafında ve olay işleme bulut tarafta artış yüklemek için aralıklı bağlantı dayanıklılığı sağlar. IOT hub'ı uygulayan *en az bir kez* teslim hem bulut-cihaz, hem de CİHAZDAN buluta iletileri için güvence altına alır.

## <a name="choosing-the-right-type-of-iot-hub-messaging"></a>IOT Hub Mesajlaşma doğru türünü seçme

Zaman serisi telemetri ve uyarılar cihaz uygulamanızdan göndermek için CİHAZDAN buluta iletileri ve bulut-cihaz iletilerini cihaz uygulamanız için tek yönlü bildirimler için kullanın.

* Başvurmak [CİHAZDAN buluta iletişim Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-d2c-guidance) CİHAZDAN buluta iletileri arasında bildirilen özelliklerini seçin ya da karşıya dosya.

* Başvurmak [bulut buluttan cihaza iletişim Kılavuzu](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-c2d-guidance) bulut-cihaz iletilerini, istenen özellikleri veya doğrudan yöntemler arasından seçme.

## <a name="next-steps"></a>Sonraki adımlar

* IOT hub'ı hakkında bilgi edinin [ileti yönlendirme](iot-hub-devguide-messages-d2c.md).

* IOT hub'ı hakkında bilgi edinin [bulut-cihaz Mesajlaşma](iot-hub-devguide-messages-c2d.md).