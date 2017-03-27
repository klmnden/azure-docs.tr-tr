---
title: "Nesnelerin İnterneti (IoT Paketi) için Azure çözümleri | Microsoft Docs"
description: "Örnek bir IoT çözümü mimarisi ile cihazlar, Azure IoT Hub hizmeti, Azure IoT cihaz SDK&quot;ları, Azure IoT hizmeti SDK&quot;ları ve diğer Azure hizmetleriyle olan ilişkisi."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: e95d02e706b87eaa355951a34bbaa3abf2ac2370
ms.lasthandoff: 01/24/2017


---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Hub, çözümünüzün arka ucu ile milyonlarca cihaz arasında düzgün ve güvenli çift yönlü iletişimler sağlayan bir Azure hizmetidir. Şunları yapmak için çözüm arka ucu sağlar:

* Cihazlarınızdan ölçekleyerek telemetri alma.
* Verileri cihazlarınızdan akış olayı işlemcisine yönlendirme.
* Cihazlardan dosya yüklemeleri alma.
* Belirli cihazlara, buluttan cihaza iletileri gönderme.

Kendi çözüm arka ucunuzu uygulamak için IoT Hub'ınızı kullanabilirsiniz. Buna ek olarak IoT Hub; cihazları, güvenlik kimlik bilgilerini ve IoT hub'ına bağlanma haklarını sağlamak için kullanılan bir kimlik kayıt defterini içerir. IoT Hub hakkında daha fazla bilgi almak için bkz. [IoT Hub nedir?][lnk-iot-hub].

Cihazlarınızı uzaktan yönetmeniz, yapılandırmanız ve güncelleştirmeniz için Azure IoT Hub’ın standartlara dayalı cihaz yönetimini nasıl etkinleştirdiği hakkında bilgi almak üzere bkz. [IoT Hub’ı ile cihaz yönetimine genel bakış][lnk-device-management].

İstemci uygulamalarını çok sayıda cihaz donanımı platformunda ve işletim sisteminde uygulamak için Azure IoT cihaz SDK'larını kullanabilirsiniz. Cihaz SDK'ları, bir IoT hub'ına telemetri gönderme ve buluttan cihaza iletilerini alma işlemlerini gerçekleştiren kitaplıkları içerir. Cihaz SDK'larını kullandığınızda IoT Hub ile iletişim kurmak için birçok ağ protokolü arasından seçim yapabilirsiniz. Daha fazla bilgi için bkz. [Cihaz SDK'ları hakkında bilgi][lnk-device-sdks].

Bazı kodları yazmaya ve bazı örnekleri çalıştırmaya başlamak için [IoT Hub ile çalışmaya başlama][lnk-getstarted] öğreticisine bakın.

Önceden yapılandırılmış çözümler koleksiyonu olan [Azure IoT Paketi][lnk-iot-suite] de ilginizi çekebilir. IoT Paketi, hızlı bir şekilde kullanmaya başlamanızı ve uzaktan izleme, varlık yönetimi ve tahmine dayalı bakım gibi ortak IoT senaryolarını ele almak üzere IoT projelerini ölçeklendirmenizi sağlar.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md

