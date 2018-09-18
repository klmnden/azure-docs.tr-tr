---
title: Azure IOT SDK'ları anlama | Microsoft Docs
description: Geliştirici Kılavuzu - ilgili bilgiler ve cihaz uygulamalarını hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz çeşitli Azure IOT cihaz ve hizmet SDK'ları için bağlantılar.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dobett
ms.openlocfilehash: ba06617762650afc8cd3eecb2fcddda6d24f4228
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734999"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Anlama ve Azure IOT Hub SDK'ları kullanın

IOT Hub ile çalışmaya yönelik yazılım geliştirme setleri (SDK'lar) üç kategoriye ayrılır:

* **Cihaz SDK'ları** , cihaz istemcisi veya modül istemcisi kullanarak IOT cihazlarında çalışan uygulamalar oluşturmanıza olanak tanır. Bu uygulamalar, IOT hub'ına telemetri gönderme ve isteğe bağlı olarak, IOT hub'ından iletiler, iş, yöntemi veya ikizi güncelleştirmeleri alırsınız.  Modül istemci yazmak için kullanabileceğiniz [modülleri](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules) için [Azure IOT Edge çalışma zamanı](https://docs.microsoft.com/en-us/azure/iot-edge/about-iot-edge).

* **Hizmet SDK'ları** IOT hub'ınızı yönetme ve isteğe bağlı olarak ileti göndermek, zamanlama işleri, doğrudan metotları çağırma veya istenen özellik güncelleştirmeleri, IOT cihazları veya modülleri Gönder olanak sağlar.

* **Cihaz sağlama SDK'ları** kullanarak IOT hub cihaz sağlama sağlayan [cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md).

Azure IOT SDK'larını kullanarak geliştirme avantajları hakkında bilgi [burada][lnk-benefits-blog].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Azure IOT cihaz SDK'ları

Microsoft Azure IOT cihaz SDK'ları oluşturma aygıtlar ve bağlanın ve Azure IOT Hub Hizmetleri tarafından yönetilen uygulamalar kolaylaştıran kodu içerir.

.NET için Azure IOT Hub cihazı SDK'sı: 
* Yüklersiniz [Nuget][lnk-nuget-csharp-device]
* [Kaynak kodu][lnk-dotnet-sdk]
* [API Başvurusu][lnk-dotnet-ref]
* [Modül başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

Taşınabilirlik ve geniş platform uyumluluğu için ANSI C (C99) yazılmış için Azure IOT Hub cihaz SDK'sı
* Yüklersiniz [apt-get, MBED, Arduino IDE veya Nuget][lnk-c-package]
* [Kaynak kodu][lnk-c-sdk]
* [API Başvurusu][lnk-c-ref]
* [Modül başvurusu](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_module_client.h)

Java için Azure IOT Hub cihazı SDK'sı: 
* Ekleme [Maven] [ lnk-maven-device] proje
* [Kaynak kodu][lnk-java-sdk]
* [API Başvurusu][lnk-java-ref]
* [Modül başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._module_client?view=azure-java-stable)

Node.js için Azure IOT Hub cihazı SDK'sı: 
* Yüklersiniz [npm][lnk-npm-device]
* [Kaynak kodu][lnk-node-sdk]
* [API Başvurusu][lnk-node-ref]
* [Modül başvurusu](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

Python için Azure IOT Hub cihazı SDK'sı: 
* Yüklersiniz [pip][lnk-pip-device]
* [Kaynak kodu][lnk-python-sdk]
* API Başvurusu: bkz [C API Başvurusu][lnk-c-ref]

İOS için Azure IOT Hub cihazı SDK'sı: 
* Yüklersiniz [CocoaPod][lnk-cocoa-device]
* [Örnekleri][lnk-ios-sample]
* API Başvurusu: bkz [C API Başvurusu][lnk-c-ref]

> [!NOTE]
> GitHub depolarının readme dosyalarında dil ve platforma özgü paket yöneticileri, geliştirme makinenizde ikili dosyaları ve bağımlılıklarını yüklemek için kullanma hakkında bilgi için bkz.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>İşletim sistemi platformu ve donanım uyumluluğu

Bu SDK'ları için desteklenen platformları bulunabilir [belge](iot-hub-device-sdk-platform-support.md).
Belirli donanım cihazlarına SDK uyumluluğu hakkında daha fazla bilgi için bkz. [IOT cihaz kataloğu için Azure sertifikalı] [ lnk-certified] veya tek tek depo.

## <a name="azure-iot-service-sdks"></a>Azure IOT hizmeti SDK'ları

Azure IOT hizmeti SDK'ları, cihazları ve güvenliği yönetmek için etkileşim kuran uygulamaları oluşturma IOT hub'ı ile doğrudan kolaylaştırmak için kod içerir.

.NET için Azure IOT Hub hizmeti SDK'sı:
* İndirmesine [Nuget][lnk-nuget-csharp-service]
* [Kaynak kodu][lnk-dotnet-sdk]
* [API Başvurusu][lnk-dotnet-service-ref]

Java için Azure IOT Hub hizmeti SDK'sı: 
* Ekleme [Maven] [ lnk-maven-service] proje
* [Kaynak kodu][lnk-java-sdk]
* [API Başvurusu][lnk-java-service-ref]

Node.js için Azure IOT Hub hizmeti SDK'sı: 
* İndirmesine [npm][lnk-npm-service]
* [Kaynak kodu][lnk-node-sdk]
* [API Başvurusu][lnk-node-service-ref]

Python için Azure IOT Hub hizmeti SDK'sı: 
* İndirmesine [pip][lnk-pip-service]
* [Kaynak kodu][lnk-python-sdk]

C için Azure IOT Hub hizmeti SDK'sı 
* İndirmesine [apt-get, MBED, Arduino IDE veya Nuget][lnk-c-package]
* [Kaynak kodu][lnk-c-sdk]

İOS için Azure IOT Hub hizmeti SDK'sı: 
* Yüklersiniz [CocoaPod][lnk-cocoa-service]
* [Örnekleri][lnk-ios-sample]

> [!NOTE]
> GitHub depolarının readme dosyalarında dil ve platforma özgü paket yöneticileri, geliştirme makinenizde ikili dosyaları ve bağımlılıklarını yüklemek için kullanma hakkında bilgi için bkz.

## <a name="device-provisioning-sdks"></a>Cihaz SDK'ları sağlama

**Microsoft Azure sağlama SDK'ları** kullanarak IOT hub cihaz sağlama sağlayan [cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md).

Azure sağlama cihaz ve hizmet SDK'ları C# için:
* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/device)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/service)

Azure sağlama cihaz ve hizmet SDK'ları Java için:
* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning-device-client)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-service-client)

Azure sağlama cihaz ve hizmet SDK'ları Node.js için:
* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/device)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service)

Azure sağlama cihaz ve hizmet SDK'ları Python için:
* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-python/blob/master/provisioning_device_client)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client)

C için Azure sağlama cihaz ve hizmet SDK
* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning/service)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT SDK'ları ile geliştirmeye yardımcı olmak için araçlar kümesi de sağlar:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): IOT Hub ile bağlantı ilgili sorunların tanılanmasına yardımcı olmak için bir platformlar arası komut satırı aracı.
* [cihaz Gezgini](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): IOT Hub'ınıza bağlanmak için bir Windows masaüstü uygulaması.

Bu IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IOT Hub uç noktaları][lnk-devguide-endpoints]
* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili][lnk-devguide-query]
* [Kotalar ve azaltma][lnk-devguide-quotas]
* [IOT hub'ı MQTT desteği][lnk-devguide-mqtt]
* [IOT Hub REST API Başvurusu][lnk-rest-ref]
* [Azure IOT SDK'sı platform desteği](iot-hub-device-sdk-platform-support.md)

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
[lnk-c-package]: https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md
[lnk-pip-device]: https://pypi.python.org/pypi/azure-iothub-device-client/
[lnk-pip-service]: https://pypi.python.org/pypi/azure-iothub-service-client/


[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-benefits-blog]: https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/
[lnk-cocoa-device]: https://cocoapods.org/pods/AzureIoTHubClient
[lnk-ios-sample]: https://github.com/Azure-Samples/azure-iot-samples-ios
[lnk-cocoa-service]: https://cocoapods.org/pods/AzureIoTHubServiceClient
