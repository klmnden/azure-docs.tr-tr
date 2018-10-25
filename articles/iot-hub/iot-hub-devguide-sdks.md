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
ms.openlocfilehash: d58c86c17cdab360f37a09b28bdf705cb781a620
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50023844"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Anlama ve Azure IOT Hub SDK'ları kullanın

IOT Hub ile çalışmaya yönelik yazılım geliştirme setleri (SDK'lar) üç kategoriye ayrılır:

* **Cihaz SDK'ları** , cihaz istemcisi veya modül istemcisi kullanarak IOT cihazlarında çalışan uygulamalar oluşturmanıza olanak tanır. Bu uygulamalar, IOT hub'ına telemetri gönderme ve isteğe bağlı olarak, IOT hub'ından iletiler, iş, yöntemi veya ikizi güncelleştirmeleri alırsınız.  Modül istemci yazmak için kullanabileceğiniz [modülleri](../iot-edge/iot-edge-modules.md) için [Azure IOT Edge çalışma zamanı](../iot-edge/about-iot-edge.md).

* **Hizmet SDK'ları** IOT hub'ınızı yönetme ve isteğe bağlı olarak ileti göndermek, zamanlama işleri, doğrudan metotları çağırma veya istenen özellik güncelleştirmeleri, IOT cihazları veya modülleri Gönder olanak sağlar.

* **Cihaz sağlama SDK'ları** kullanarak IOT hub cihaz sağlama sağlayan [cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md).

Hakkında bilgi edinin [avantajları, Azure IOT SDK'larını kullanarak geliştirme](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Azure IOT cihaz SDK'ları

Microsoft Azure IOT cihaz SDK'ları oluşturma aygıtlar ve bağlanın ve Azure IOT Hub Hizmetleri tarafından yönetilen uygulamalar kolaylaştıran kodu içerir.

.NET için Azure IOT Hub cihazı SDK'sı: 

* Yüklersiniz [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-csharp)
* [API başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Modül başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

C'de ANSI C (C99) taşınabilirlik ve geniş platform uyumluluğu için yazılan, Azure IOT Hub cihazı SDK'sı:

* Yüklersiniz [apt-get, MBED, Arduino IDE veya Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-c)
* [API başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
* [Modül başvurusu](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_module_client.h)

Java için Azure IOT Hub cihazı SDK'sı: 

* Ekleme [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk) proje
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-java)
* [API başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Modül başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._module_client?view=azure-java-stable)

Node.js için Azure IOT Hub cihazı SDK'sı: 

* Yüklersiniz [npm](https://www.npmjs.com/package/azure-iot-device)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-node)
* [API başvurusu](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [Modül başvurusu](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

Python için Azure IOT Hub cihazı SDK'sı: 

* Yüklersiniz [pip](https://pypi.python.org/pypi/azure-iothub-device-client/)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-python)
* API Başvurusu: bkz [C API Başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

İOS için Azure IOT Hub cihazı SDK'sı: 

* Yüklersiniz [CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient)
* [Örnekler](https://github.com/Azure-Samples/azure-iot-samples-ios)
* API Başvurusu: bkz [C API Başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

> [!NOTE]
> GitHub depolarının readme dosyalarında dil ve platforma özgü paket yöneticileri, geliştirme makinenizde ikili dosyaları ve bağımlılıklarını yüklemek için kullanma hakkında bilgi için bkz.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>İşletim sistemi platformu ve donanım uyumluluğu

SDK'ları için desteklenen platformları bulunabilir [Azure IOT SDK'ları Platform Desteği](iot-hub-device-sdk-platform-support.md).

Belirli donanım cihazlarına SDK uyumluluğu hakkında daha fazla bilgi için bkz. [IOT cihaz kataloğu için Azure sertifikalı](https://catalog.azureiotsuite.com/) veya tek tek depo.

## <a name="azure-iot-service-sdks"></a>Azure IOT hizmeti SDK'ları

Azure IOT hizmeti SDK'ları, cihazları ve güvenliği yönetmek için etkileşim kuran uygulamaları oluşturma IOT hub'ı ile doğrudan kolaylaştırmak için kod içerir.

.NET için Azure IOT Hub hizmeti SDK'sı:

* İndirmesine [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices/)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-csharp)
* [API başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

Java için Azure IOT Hub hizmeti SDK'sı: 

* Ekleme [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
) proje
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-java)
* [API başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service)

Node.js için Azure IOT Hub hizmeti SDK'sı: 

* İndirmesine [npm](https://www.npmjs.com/package/azure-iothub)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-node)
* [API başvurusu](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

Python için Azure IOT Hub hizmeti SDK'sı: 

* İndirmesine [pip](https://pypi.python.org/pypi/azure-iothub-service-client/)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-python)

C için Azure IOT Hub hizmeti SDK'sı 

* İndirmesine [apt-get, MBED, Arduino IDE veya Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-c)

İOS için Azure IOT Hub hizmeti SDK'sı: 

* Yüklersiniz [CocoaPod](https://cocoapods.org/pods/AzureIoTHubServiceClient)
* [Örnekler](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> GitHub depolarının readme dosyalarında dil ve platforma özgü paket yöneticileri, geliştirme makinenizde ikili dosyaları ve bağımlılıklarını yüklemek için kullanma hakkında bilgi için bkz.

## <a name="device-provisioning-sdks"></a>Cihaz SDK'ları sağlama

**Microsoft Azure sağlama SDK'ları** kullanarak IOT hub cihaz sağlama sağlayan [cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md).

Azure sağlama cihaz ve hizmet SDK'ları C# için:

* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/device)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/service)

Azure sağlama cihaz ve hizmet SDK'ları Java için:

* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-device-client)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-service-client)

Azure sağlama cihaz ve hizmet SDK'ları Node.js için:

* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/device)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service)

Azure sağlama cihaz ve hizmet SDK'ları Python için:

* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-python/blob/master/provisioning_device_client)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client)

C için Azure sağlama cihaz ve hizmet SDK

* [Cihaz istemci SDK'sı sağlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [Sağlama hizmeti istemci SDK'sı](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_service_client)

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT SDK'ları ile geliştirmeye yardımcı olmak için araçlar kümesi de sağlar:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): IOT Hub ile bağlantı ilgili sorunların tanılanmasına yardımcı olmak için bir platformlar arası komut satırı aracı.
* [cihaz Gezgini](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): IOT Hub'ınıza bağlanmak için bir Windows masaüstü uygulaması.

Bu IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md)
* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md)
* [Kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md)
* [IOT hub'ı MQTT desteği](iot-hub-mqtt-support.md)
* [IOT Hub REST API Başvurusu](/rest/api/iothub/)
* [Azure IOT SDK'sı platform desteği](iot-hub-device-sdk-platform-support.md)