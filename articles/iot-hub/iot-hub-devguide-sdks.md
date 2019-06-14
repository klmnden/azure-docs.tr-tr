---
title: Azure IOT SDK'ları anlama | Microsoft Docs
description: Geliştirici Kılavuzu - ilgili bilgiler ve cihaz uygulamalarını hem de arka uç uygulamaları oluşturmak için kullanabileceğiniz çeşitli Azure IOT cihaz ve hizmet SDK'ları için bağlantılar.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: e51313bbed21459de9f717edd123887caed18f4b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60400669"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Anlama ve Azure IOT Hub SDK'ları kullanın

IOT Hub ile çalışmaya yönelik yazılım geliştirme setleri (SDK'lar) iki kategorisi vardır:

* **IOT Hub cihazı SDK'ları** , cihaz istemcisi veya modül istemcisi kullanarak IOT cihazlarında çalışan uygulamalar oluşturmanıza olanak tanır. Bu uygulamalar, IOT hub'ına telemetri gönderme ve isteğe bağlı olarak, IOT hub'ından iletiler, iş, yöntemi veya ikizi güncelleştirmeleri alırsınız.  Modül istemci yazmak için kullanabileceğiniz [modülleri](../iot-edge/iot-edge-modules.md) için [Azure IOT Edge çalışma zamanı](../iot-edge/about-iot-edge.md).

* **IOT Hub hizmeti SDK'ları** , IOT hub'ınızı yönetme ve isteğe bağlı olarak göndermek için arka uç uygulamaları oluşturmak için işleri zamanlama, doğrudan metotları çağırma veya istenen özellik güncelleştirmeleri, IOT cihazları veya modülleri Gönder etkinleştir.

Ayrıca, ayrıca bir dizi SDK'ları ile çalışmak için bildirimde [cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md).
* **Cihaz SDK'ları sağlama** , iletişim kurmak için cihaz sağlama hizmeti ile IOT cihazlarınızı üzerinde çalışan uygulamalar oluşturmanıza olanak tanır.

* **Sağlama hizmeti SDK'ları** , cihaz sağlama hizmeti, kayıtlarınızı yönetmek için arka uç uygulamaları oluşturmanıza olanak tanır.

Hakkında bilgi edinin [avantajları, Azure IOT SDK'larını kullanarak geliştirme](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]


### <a name="os-platform-and-hardware-compatibility"></a>İşletim sistemi platformu ve donanım uyumluluğu

SDK'ları için desteklenen platformları bulunabilir [Azure IOT SDK'ları Platform Desteği](iot-hub-device-sdk-platform-support.md).

Belirli donanım cihazlarına SDK uyumluluğu hakkında daha fazla bilgi için bkz. [IOT cihaz kataloğu için Azure sertifikalı](https://catalog.azureiotsolutions.com/) veya tek tek depo.

## <a name="azure-iot-hub-device-sdks"></a>Azure IOT Hub cihaz SDK'ları

Microsoft Azure IOT cihaz SDK'ları bağlanın ve Azure IOT Hub Hizmetleri tarafından yönetilen uygulamaları oluşturma kolaylaştıran kodu içerir.

.NET için Azure IOT Hub cihazı SDK'sı: 

* İndirmesine [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/).  IOT Hub cihaz istemcilerine (DeviceClient, ModuleClient) içeren Microsoft.Azure.Devices.Clients ad alanıdır.
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-csharp)
* [API başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Modül başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

C (ANSI C - C99) için Azure IOT Hub cihazı SDK'sı:

* Yüklersiniz [apt-get, MBED, Arduino IDE veya iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-c)
* [C cihaz SDK'sını derleme](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md#compiling-the-c-device-sdk)
* [API başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
* [Modül başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-h)
* [Diğer platformlar için C SDK'sı taşıma](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
* [Geliştirici belgeleri](https://github.com/Azure/azure-iot-sdk-c/tree/master/doc) çapraz derleme, farklı platformlarda çalışmaya başlama hakkında bilgi için vs.
* [Azure IOT Hub C SDK'sı kaynak tüketimi bilgilerini](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/c_sdk_resource_information.md)

Java için Azure IOT Hub cihazı SDK'sı: 

* Ekleme [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk) proje
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-java)
* [API başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Modül başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.moduleclient?view=azure-java-stable)

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

## <a name="azure-iot-hub-service-sdks"></a>Azure IOT Hub hizmeti SDK'ları

Azure IOT hizmeti SDK'ları, cihazları ve güvenliği yönetmek için etkileşim kuran uygulamaları oluşturma IOT hub'ı ile doğrudan kolaylaştırmak için kod içerir.

.NET için Azure IOT Hub hizmeti SDK'sı:

* İndirmesine [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices/).  IOT Hub hizmeti istemcilerine (RegistryManager, ServiceClients) içeren Microsoft.Azure.Devices ad alanıdır.
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-csharp)
* [API başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

Java için Azure IOT Hub hizmeti SDK'sı: 

* Ekleme [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk) proje
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

## <a name="microsoft-azure-provisioning-sdks"></a>Microsoft Azure sağlama SDK'ları

**Microsoft Azure sağlama SDK'ları** kullanarak IOT hub cihaz sağlama sağlayan [cihaz sağlama hizmeti](../iot-dps/about-iot-dps.md).

Azure sağlama cihaz ve hizmet SDK'ları C# için:

* İndirmesine [cihaz SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Client/) ve [hizmet SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) nuget'ten.
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-csharp/)
* [API başvurusu](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.provisioning.client?view=azure-dotnet)

C için Azure sağlama cihaz ve hizmet SDK

* Yüklersiniz [apt-get, MBED, Arduino IDE veya iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md#packages-and-libraries)
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [API başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

Azure sağlama cihaz ve hizmet SDK'ları Java için:

* Ekleme [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk) proje
* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning)
* [API başvurusu](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.provisioning.device?view=azure-java-stable)

Azure sağlama cihaz ve hizmet SDK'ları Node.js için:

* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning)
* [API başvurusu](https://docs.microsoft.com/javascript/api/overview/azure/iothubdeviceprovisioning?view=azure-node-latest)
* İndirme [cihaz SDK'sı](https://badge.fury.io/js/azure-iot-provisioning-device) ve [hizmeti SDK'sı](https://badge.fury.io/js/azure-iot-provisioning-service) npm

Azure sağlama cihaz ve hizmet SDK'ları Python için:

* [Kaynak kod](https://github.com/Azure/azure-iot-sdk-python)
* İndirme [cihaz SDK'sı](https://pypi.org/project/azure-iot-provisioning-device-client/) ve [hizmeti SDK'sı](https://pypi.org/project/azure-iothub-provisioningserviceclient/) pip gelen

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT SDK'ları ile geliştirmeye yardımcı olmak için araçlar kümesi de sağlar:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): IOT Hub ile bağlantı ilgili sorunların tanılanmasına yardımcı olmak için bir platformlar arası komut satırı aracı.
* [cihaz Gezgini](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): IOT Hub'ınıza bağlanmak için bir Windows masaüstü uygulaması.

İlgili belgeleri, Azure IOT SDK'larını kullanarak geliştirme ile ilgili:
* Hakkında bilgi edinin [bağlantı ve güvenilir Mesajlaşma yönetme](iot-hub-reliability-features-in-sdks.md) IOT Hub SDK'larını kullanarak.
* Hakkında bilgi [mobil platformlar için geliştirme](iot-hub-how-to-develop-for-mobile-devices.md) iOS ve Android gibi.
* [Azure IOT SDK'sı platform desteği](iot-hub-device-sdk-platform-support.md)


Bu IOT Hub Geliştirici Kılavuzu'nda olan diğer başvuru konularını içerir:

* [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md)
* [Cihaz ikizleri, işler ve ileti yönlendirme için IOT Hub sorgu dili](iot-hub-devguide-query-language.md)
* [Kotalar ve azaltma](iot-hub-devguide-quotas-throttling.md)
* [IOT hub'ı MQTT desteği](iot-hub-mqtt-support.md)
* [IOT Hub REST API Başvurusu](/rest/api/iothub/)
