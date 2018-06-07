---
title: Cihaz Bağlantısı'nda Azure IOT Merkezi | Microsoft Docs
description: Bu makale Azure IOT Merkezi içindeki cihaz bağlantısı ile ilgili temel kavramları tanıtır
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: dc9fe144c2258f33ce59c61ce63c15835cc3fa53
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628343"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Azure IOT Merkezi içindeki cihaz bağlantısı

Bu makalede, Microsoft Azure IOT Merkezi içindeki cihaz bağlantısı ile ilgili temel kavramları tanıtır.

Azure IOT merkezi uygulamanıza bağlanan her bir aygıtı bağlandığı [uç noktaları](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints) Azure IOT merkezi kullanır IOT hub tarafından sunulur. IOT hub'ı bağlı cihazlarınızdan ölçeklenebilir, güvenli ve güvenilir bağlantılar sağlar.

## <a name="sdk-support"></a>SDK'sı desteği

Sizin için Azure cihaz SDK'ları teklif en kolay yolu, Azure IOT merkezi uygulamanıza bağlayan cihazlarınızda kodu uygulayın. Aşağıdaki cihaz SDK'ları mevcuttur:

- [C için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-c)
- [Python için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-python)
- [Node.js için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-node)
- [Java için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-java)
- [.NET için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-csharp)

Her aygıt, aygıtın tanımlayan bir benzersiz bağlantı dizesi kullanarak bağlanır. Bir cihazı yalnızca, kayıtlı olduğu IOT hub'ına bağlanabilir. Azure IOT merkezi uygulamanızda gerçek bir aygıt oluşturduğunuzda, uygulamayı kullanabilmeniz için bir bağlantı dizesi oluşturur.

## <a name="sdk-features-and-iot-hub-connectivity"></a>SDK özelliklerinin ve IOT Hub bağlantı

IOT Hub ile tüm cihaz iletişimi aşağıdaki IOT Hub bağlantı seçenekleri kullanır:

- [Cihaz bulut Mesajlaşma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Cihaz çiftlerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

Aşağıdaki tabloda, nasıl Azure IOT merkezi cihaz özelliklerini açın IOT Hub özellikleri eşlemek özetlenmektedir:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Ölçüm: Telemetri | Cihaz bulut Mesajlaşma |
| Cihaz özellikleri | Cihaz çifti özellikleri bildirdi |
| Ayarlar | Cihaz çifti istenen ve Özellikler bildirdi |

Cihaz SDK'ları kullanma hakkında daha fazla bilgi edinmek için örnek kod bir aşağıdaki makalelere bakın:

- [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)
- [Azure IOT merkezi uygulamanıza Raspberry Pi'yi aygıtı bağlayın](howto-connect-raspberry-pi-python.md)
- [Azure IOT merkezi DevDiv Seti aygıt bağlamak](howto-connect-devkit.md).

Aşağıdaki videoda gerçek bir cihazı Azure IOT merkezi bağlanmak nasıl bir bakış sunar:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Connect-real-devices-to-Microsoft-IoT-Central/Player]

## <a name="protocols"></a>Protokoller

Cihaz SDK'ları, bir IOT hub'ına bağlanmak için aşağıdaki ağ protokollerini destekler:

- MQTT
- AMQP
- HTTPS

Bir seçme hakkında bilgi için bu fark protokolleri ve rehberlik, bkz: [iletişim protokolü seçin](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Cihazınızı Desteklenen protokoller hiçbirini kullanamıyorsanız, dönüştürme protokolü için Azure IOT kenar kullanabilirsiniz. IOT kenar işleme kenarına Azure IOT merkezi uygulamadan boşaltmak için diğer Intelligence üzerinde--uç senaryolarını destekler.

## <a name="security"></a>Güvenlik

Aygıtları ve, Azure IOT merkezi arasında alınıp tüm veriler şifrelenir. IOT Hub cihaz dönük IOT Hub uç noktaları birine bağlayan bir aygıttan her isteğin kimliğini doğrular. Kimlik bilgileri kablo üzerinden değişimi önlemek için kimlik doğrulaması için imzalanmış belirteçleri bir aygıt kullanır. Daha fazla bilgi için bkz: [IOT Hub'ına erişim denetim](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Şu anda Azure IOT merkezi bağlanan aygıtları SAS belirteçleri kullanmanız gerekir. X.509 sertifikaları Azure IOT merkezi bağlanan cihazlar için desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Merkezi içindeki cihaz bağlantısı hakkında öğrendiniz, önerilen sonraki adımlar şunlardır:

- [Hazırlama ve DevKit aygıtı bağlayın](howto-connect-devkit.md)
- [Raspberry Pi'yi hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)
- [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)
