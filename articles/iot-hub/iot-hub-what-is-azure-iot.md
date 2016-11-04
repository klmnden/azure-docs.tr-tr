---
title: Nesnelerin İnterneti için Azure çözümleri | Microsoft Docs
description: Örnek bir çözüm mimarisi ve bu mimarinin Azure IoT Hub, cihaz SDK'ları ve önceden yapılandırılmış çözümlerle olan ilişkisini içeren, Azure'daki IoT'ye genel bakış
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2016
ms.author: dobett

---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## Sonraki adımlar
Azure IoT Hub, uygulamanızın arka ucu ile milyonlarca cihaz arasında düzgün ve güvenli çift yönlü iletişimler sağlayan bir Azure hizmetidir. Şunları yapmak için uygulama arka ucu sağlar:

* Cihazlarınızdan ölçekleyerek telemetri alma.
* Verileri cihazlarınızdan akış olayı işlemcisine yönlendirme.
* Cihazlardan dosya yüklemeleri alma.
* Belirli cihazlara, buluttan cihaza komutları gönderme.

Kendi çözüm arka ucunuzu uygulamak için IoT Hub'ınızı kullanabilirsiniz. Buna ek olarak IoT Hub cihazları, güvenlik kimlik bilgilerini ve hub'a bağlanma haklarını sağlamak için kullanılan bir cihaz kimlik kayıt defterini içerir. IoT Hub hakkında daha fazla bilgi almak için bkz. [IoT Hub nedir?][lnk-iot-hub].

Cihazlarınızı uzaktan yönetmeniz, yapılandırmanız ve güncelleştirmeniz için Azure IoT Hub’ın standartlara dayalı IoT cihaz yönetimini nasıl etkinleştirdiği hakkında bilgi almak için bkz. [Azure IoT Hub cihaz yönetimine genel bakış][lnk-device-management].

Çok çeşitli cihaz donanım platformları ve işletim sistemlerinde istemci uygulamalarını uygulamak için IoT cihaz SDK'larını kullanabilirsiniz. IoT cihaz SDK'ları, bir IoT hub'ına telemetri göndermeyi ve bulut-cihaz komutlarını almayı gerçekleştiren kitaplıkları içerir. SDK'ları kullandığınızda IoT Hub ile iletişim kurmak için birçok ağ protokolü arasından seçim yapabilirsiniz. Daha fazla bilgi için bkz. [Cihaz SDK'ları hakkında bilgi][lnk-device-sdks].

Bazı kodları yazmaya ve bazı örnekleri çalıştırmaya başlamak için [IoT Hub ile çalışmaya başlama][lnk-getstarted] öğreticisine bakın.

Önceden yapılandırılmış çözümler koleksiyonu olan [Azure IoT Paketi][lnk-iot-suite] de ilginizi çekebilir. IoT Paketi, hızlı bir şekilde kullanmaya başlamanızı ve uzaktan izleme, varlık yönetimi ve tahmine dayalı bakım gibi ortak IoT senaryolarını ele almak üzere IoT projelerini ölçeklendirmenizi sağlar.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md


<!--HONumber=Oct16_HO3-->


