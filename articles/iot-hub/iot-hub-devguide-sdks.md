---
title: Azure IOT SDK'ları anlama | Microsoft Docs
description: Geliştirici Kılavuzu - ve cihaz uygulamaları ve arka uç uygulamaları oluşturmak için kullanabileceğiniz çeşitli Azure IOT cihaz ve hizmet SDK'lar bağlantılar hakkında bilgi.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/12/2018
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aec2126369f45a89050dbd8b2d3cae7e00ccb8ed
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="understand-and-use-azure-iot-sdks"></a>Anlamak ve Azure IOT SDK'ları kullanın

Yazılım geliştirme setleri (SDK) IOT Hub ile çalışmaya yönelik üç kategoriye ayrılır:

* **Cihaz SDK'ları** IOT cihazlarınızı üzerinde çalışan uygulamalar geliştirme olanak sağlar. Bu uygulamaları IOT hub'ınıza telemetri göndermek ve isteğe bağlı olarak, IOT hub'ından iletileri alacak.

* **Hizmet SDK'ları** IOT hub'ınızı yönetmenize olanak tanıyan ve isteğe bağlı olarak IOT cihazlarınızı ileti gönderin.

* **Azure IOT kenar** desteklenen protokollerden birini kullanmayan cihazlar için ağ geçitleri oluşturmanıza olanak sağlar. Ağ geçitleri de kenar iletilerde işleyebilir.

SDK'ları, birden fazla programlama dili desteklemek için sağlanır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>Azure IOT cihaz SDK'ları

Microsoft Azure IOT cihaz SDK'ları oluşturma aygıtlar ve bağlanmak ve Azure IOT Hub Hizmetleri tarafından yönetilen uygulamalar kolaylaştıran kodu içerir.

Aşağıdaki Azure IOT cihaz SDK'ları Github'dan indirilebilir:

* [.NET için Azure IOT cihaz SDK'sı][lnk-dotnet-device-sdk]
* [Java için Azure IOT cihaz SDK'sı][lnk-java-device-sdk]
* [Node.js için Azure IOT cihaz SDK'sı][lnk-node-device-sdk]
* [Python için Azure IOT cihaz SDK'sı][lnk-python-device-sdk]
* [C için Azure IOT cihaz SDK'sı] [ lnk-c-device-sdk] taşınabilirlik ve geniş platform uyumluluğu için ANSI C (C99) yazılır. C, alt düzey için iki cihaz istemci Kitaplığı **iothub_client** ve **seri hale getirici**.

> [!NOTE]
> GitHub depolarının readme dosyalarında ikili dosyaları ve bağımlılıkları geliştirme makinenizde yüklemek için dil ve platforma özgü paket yöneticileri kullanma hakkında bilgi için bkz.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>İşletim sistemi platformu ve donanım uyumluluğu

Belirli donanım aygıtları ile SDK uyumluluğu hakkında daha fazla bilgi için bkz: [Azure IOT cihaz katalog için onaylanmıştır][lnk-certified].

## <a name="azure-iot-service-sdks"></a>Azure IOT hizmeti SDK'ları

Azure IOT hizmeti SDK'ları cihazları ve güvenliği yönetmek için etkileşime uygulamaları oluşturma IOT Hub ile doğrudan kolaylaştırmak için kod içerir.

Aşağıdaki Azure IOT hizmeti SDK'ları Github'dan indirilebilir:

* [.NET için Azure IOT hizmeti SDK'sı][lnk-dotnet-service-sdk]
* [Java için Azure IOT hizmeti SDK'sı][lnk-java-service-sdk]
* [Node.js için Azure IOT hizmeti SDK'sı][lnk-node-service-sdk]
* [Python için Azure IOT hizmeti SDK'sı][lnk-python-service-sdk]
* [C için Azure IOT hizmeti SDK'sı][lnk-c-service-sdk]

> [!NOTE]
> GitHub depolarının readme dosyalarında ikili dosyaları ve bağımlılıkları geliştirme makinenizde yüklemek için dil ve platforma özgü paket yöneticileri kullanma hakkında bilgi için bkz.

## <a name="azure-iot-edge"></a>Azure IoT Edge

Azure IOT kenar altyapısı ve IOT ağ geçidi çözümler oluşturmak için modüller içerir. IOT bir uçtan uca senaryonuz için özel olarak hazırlanmış ağ geçitleri oluşturmak için kenar genişletebilirsiniz.

İndirebilirsiniz [Azure IOT kenar] [ lnk-iot-edge] github'dan.

## <a name="online-api-reference-documentation"></a>Çevrimiçi API başvuru belgeleri

Aşağıdaki liste, Azure IOT cihaz, hizmet ve ağ geçidi kitaplıkları için çevrimiçi API başvuru belgeleri bağlantılarını içerir:

* [Nesnelerin interneti (IOT) .NET][lnk-dotnet-ref]
* [Java için Azure IOT cihaz SDK'sı][lnk-java-ref]
* [Java için Azure IOT hizmeti SDK'sı][lnk-java-service-ref]
* [Node.js için Azure IOT cihaz SDK'sı][lnk-node-ref]
* [Node.js için Azure IOT hizmeti SDK'sı][lnk-node-service-ref]
* [C için Azure IOT cihaz SDK'sı][lnk-c-ref]
* [IOT hub'ı REST][lnk-rest-ref]
* [Azure IoT Edge][lnk-gateway-ref]

## <a name="next-steps"></a>Sonraki adımlar

Bu IOT Hub Geliştirici Kılavuzu'ndaki diğer başvuru konuları içerir:

* [IOT Hub uç noktaları][lnk-devguide-endpoints]
* [Cihaz çiftlerini, işler ve ileti yönlendirme için IOT hub'ı sorgulama dili][lnk-devguide-query]
* [Kotalar ve azaltma][lnk-devguide-quotas]
* [IOT Hub MQTT desteği][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-c-service-sdk]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_service_client
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/device
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/service
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/service
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/service
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device
[lnk-python-service-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-iot-edge]: https://github.com/Azure/iot-edge

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service
[lnk-node-service-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-gateway-ref]: http://azure.github.io/iot-edge/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
