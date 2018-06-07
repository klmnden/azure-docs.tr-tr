---
title: Azure IOT SDK'ları anlama | Microsoft Docs
description: Geliştirici Kılavuzu - ve cihaz uygulamaları ve arka uç uygulamaları oluşturmak için kullanabileceğiniz çeşitli Azure IOT cihaz ve hizmet SDK'lar bağlantılar hakkında bilgi.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: dobett
ms.openlocfilehash: 31fed4ad1c9a790f771f8943775804976070fc0b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633392"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Anlamak ve Azure IOT Hub SDK'ları kullanın

Yazılım geliştirme setleri (SDK) IOT Hub ile çalışmaya yönelik iki kategorisi vardır:

* **Cihaz SDK'ları** IOT cihazlarınızı üzerinde çalışan uygulamalar geliştirme olanak sağlar. Bu uygulamaları IOT hub'ınıza telemetri gönderebilir ve isteğe bağlı olarak iletiler, iş, yöntemi veya twin güncelleştirmeleri IOT hub'ından alabilirsiniz.

* **Hizmet SDK'ları** IOT hub'ınızı yönetmek ve isteğe bağlı olarak iletileri göndermek, işleri, doğrudan yöntemleri çağırma veya IOT cihazlarınızı istenen özelliği güncellemelerin olanak sağlar.

Azure IOT SDK'ları kullanarak geliştirme avantajları hakkında bilgi edinin [burada][lnk-benefits-blog].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Azure IOT cihaz SDK'ları

Microsoft Azure IOT cihaz SDK'ları oluşturma aygıtlar ve bağlanmak ve Azure IOT Hub Hizmetleri tarafından yönetilen uygulamalar kolaylaştıran kodu içerir.

.NET için Azure IOT Hub cihaz SDK'sı: 
* Yüklemenize [Nuget][lnk-nuget-csharp-device]
* [Kaynak kodu][lnk-dotnet-sdk]
* [API Başvurusu][lnk-dotnet-ref]

ANSI C (C99) taşınabilirlik ve geniş platform uyumluluğu için yazılmış C: Azure IOT Hub cihaz SDK'sı
* Yüklemenize [get apt, MBED, Arduino IDE veya Nuget][lnk-c-package]
* [Kaynak kodu][lnk-c-sdk]
* [API Başvurusu][lnk-c-ref]

Java için Azure IOT Hub cihaz SDK'sı: 
* Ekleme [Maven] [ lnk-maven-device] proje
* [Kaynak kodu][lnk-java-sdk]
* [API Başvurusu][lnk-java-ref]

Node.js için Azure IOT Hub cihaz SDK'sı: 
* Yüklemenize [npm][lnk-npm-device]
* [Kaynak kodu][lnk-node-sdk]
* [API Başvurusu][lnk-node-ref]

Python için Azure IOT Hub cihaz SDK'sı: 
* Yüklemenize [PIP][lnk-pip-device]
* [Kaynak kodu][lnk-python-sdk]

> [!NOTE]
> GitHub depolarının readme dosyalarında ikili dosyaları ve bağımlılıkları geliştirme makinenizde yüklemek için dil ve platforma özgü paket yöneticileri kullanma hakkında bilgi için bkz.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>İşletim sistemi platformu ve donanım uyumluluğu

Belirli donanım aygıtları ile SDK uyumluluğu hakkında daha fazla bilgi için bkz: [Azure IOT cihaz katalog için onaylanmıştır] [ lnk-certified] veya bireysel deposu.

## <a name="azure-iot-service-sdks"></a>Azure IOT hizmeti SDK'ları

Azure IOT hizmeti SDK'ları cihazları ve güvenliği yönetmek için etkileşime uygulamaları oluşturma IOT Hub ile doğrudan kolaylaştırmak için kod içerir.

.NET için Azure IOT Hub hizmeti SDK:
* İndirin [Nuget][lnk-nuget-csharp-service]
* [Kaynak kodu][lnk-dotnet-sdk]
* [API Başvurusu][lnk-dotnet-service-ref]

Java için Azure IOT Hub hizmeti SDK: 
* Ekleme [Maven] [ lnk-maven-service] proje
* [Kaynak kodu][lnk-java-sdk]
* [API Başvurusu][lnk-java-service-ref]

Node.js için Azure IOT Hub hizmeti SDK: 
* İndirin [npm][lnk-npm-service]
* [Kaynak kodu][lnk-node-sdk]
* [API Başvurusu][lnk-node-service-ref]

Python için Azure IOT Hub hizmeti SDK: 
* İndirin [PIP][lnk-pip-service]
* [Kaynak kodu][lnk-python-sdk]

C: için Azure IOT Hub hizmeti SDK'sı 
* İndirin [get apt, MBED, Arduino IDE veya Nuget][lnk-c-package]
* [Kaynak kodu][lnk-c-sdk]

> [!NOTE]
> GitHub depolarının readme dosyalarında ikili dosyaları ve bağımlılıkları geliştirme makinenizde yüklemek için dil ve platforma özgü paket yöneticileri kullanma hakkında bilgi için bkz.


## <a name="next-steps"></a>Sonraki adımlar

Bu IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları][lnk-devguide-endpoints]
* [Cihaz çiftlerini, işler ve ileti yönlendirme için IOT hub'ı sorgulama dili][lnk-devguide-query]
* [Kotalar ve azaltma][lnk-devguide-quotas]
* [IOT Hub MQTT desteği][lnk-devguide-mqtt]
* [IOT hub'ı REST API Başvurusu][lnk-rest-ref]

<!-- Links and images -->

[lnk-c-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-dotnet-sdk]: https://github.com/Azure/azure-iot-sdk-csharp
[lnk-java-sdk]: https://github.com/Azure/azure-iot-sdk-java
[lnk-node-sdk]: https://github.com/Azure/azure-iot-sdk-node
[lnk-python-sdk]: https://github.com/Azure/azure-iot-sdk-python
[lnk-certified]: https://catalog.azureiotsuite.com/

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet
[lnk-dotnet-service-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service
[lnk-node-service-ref]: https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest

[lnk-maven-device]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk
[lnk-maven-service]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
[lnk-npm-device]: https://www.npmjs.com/package/azure-iot-device
[lnk-npm-service]: https://www.npmjs.com/package/azure-iothub
[lnk-nuget-csharp-device]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-csharp-service]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-c-package]: https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md
[lnk-pip-device]: https://pypi.python.org/pypi/azure-iothub-device-client/
[lnk-pip-service]: https://pypi.python.org/pypi/azure-iothub-service-client/


[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-benefits-blog]: https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/
