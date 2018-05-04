---
title: Azure IOT SDK'ları kullanarak mobil cihazlar için geliştirme | Microsoft Docs
description: Geliştirici Kılavuzu - Azure IOT Hub SDK'ları kullanarak mobil cihazlar için geliştirme hakkında bilgi edinin.
services: iot-hub
documentationcenter: ''
author: yzhong94
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/16/2018
ms.author: yizhon
ms.openlocfilehash: 824d577a4709a388bd858bdda6ef5f11f53dde58
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Azure IOT SDK'ları kullanarak mobil cihazlar için geliştirme

Nesnelerin interneti şeyler değişen özelliğine sahip çok çeşitli aygıtları başvurabilir: algılayıcılar, mikrodenetleyici, akıllı aygıtlar, endüstriyel ağ geçitleri ve hatta mobil aygıtlar.  Bir mobil cihaz burada cihaz bulut telemetri gönderen ve bulut tarafından yönetilen bir IOT cihaz olabilir.  Ayrıca, diğer IOT cihazları yöneten bir arka uç hizmeti uygulaması çalıştıran aygıtları da olabilir.  Her iki durumda da [Azure IOT Hub SDK'ları] [ lnk-sdk-overview] mobil cihazlar için çalışan uygulamalar geliştirmek için kullanılır.  

## <a name="develop-for-native-ios-platform"></a>Yerel iOS platformu için geliştirme

Azure IOT Hub SDK'ları Azure IOT Hub C SDK'sı aracılığıyla yerel iOS platform desteği sağlar.  Bunu, bir iOS Swift veya Objective C XCode projenizde dahil edebilirsiniz SDK'sı olarak düşünebilirsiniz.  C SDK'sı iOS kullanmanın iki yolu vardır:
- XCode projesi CocoaPod kitaplıklarında doğrudan kullanın.  
- C SDK'sı için kaynak kodunu indirebilir ve iOS platformu aşağıdaki yapı [yönerge yapı] [ lnk-c-devbox] MacOS için.  

Azure IOT Hub C SDK'sı C99 maksimum taşınabilirlik çeşitli platformlar için yazılmıştır.  Bağlantı noktası oluşturma işlemi için burada bulunabilir platforma özgü bileşenleri için ince benimseme katman yazma içerir [iOS][lnk-ios-pal].  C SDK'sı özelliklerinde desteklenen Azure IOT Hub temelleri dahil olmak üzere iOS platformunda yararlanılabilir ve SDK özgü özellikler gibi ağ güvenilirlik için ilke yeniden deneyin.  İOS için arabirimi SDK ayrıca Azure IOT Hub C SDK'sı arabirimi benzer.  

Bir cihaz uygulaması veya bir iOS cihazında hizmet uygulaması geliştirmeyi aracılığıyla bu belgeleri yol:
- [Hızlı Başlangıç: telemetri bir CİHAZDAN bir IOT hub'ına Gönder][lnk-device-ios-quickstart]  
- [Cihazınızı IOT hub ile buluttan iletileri gönder][lnk-service-ios-quickstart]  

### <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Azure IOT Hub CocoaPod kitaplıklarıyla geliştirin

Azure IOT Hub SDK'ları iOS geliştirme için Objective-C CocoaPod bir dizi serbest bırakır.  CocoaPod kitaplıkları en son listesini görmek için bkz: [Microsoft Azure IOT CocoaPods][lnk-cocoapod-list].  İlgili kitaplıkları XCode projenize eklenen sonra IOT Hub ile ilgili kod yazmak için iki yolu vardır:
- Objective C işlevi: projenizi Objective-C yazılmışsa, API'ları Azure IOT Hub C SDK doğrudan çağırabilirsiniz.  Projenizi içinde Swift yazılmışsa çağırabilirsiniz ``@objc func`` önce işlevinizi oluşturma ve C ya da Objective-C kodunu kullanarak Azure IOT Hub için ilgili tüm logics yazmaya devam edin.  Her ikisi de gösteren örnekler kümesi bulunabilir [örnek depo][lnk-ios-samples-repo].  
- C örnekleri dahil: C cihaz uygulama yazdıysanız, XCode projenizde doğrudan başvurusu yapabilir:
    - Sample.c dosyasını XCode XCode projenize ekleyin.  
    - Üst bilgi dosyasını, bağımlılık ekleyin.  Bir üst bilgi dosyasını dahil [örnek depo] [ lnk-ios-samples-repo] örnek olarak.  Daha fazla bilgi için lütfen için Apple'nın belgeleri sayfasını ziyaret edin [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="next-steps"></a>Sonraki adımlar
* [IOT hub'ı REST API Başvurusu][lnk-rest-ref]
* [Azure IOT C SDK'sı kaynak kodu][lnk-c-sdk]

[lnk-sdk-overview]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks
[lnk-c-devbox]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-device-ios-quickstart]:https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-ios
[lnk-service-ios-quickstart]: https://docs.microsoft.com/azure/iot-hub/iot-hub-ios-swift-c2d
[lnk-cocoapod-list]: https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md
[lnk-ios-samples-repo]: https://github.com/Azure-Samples/azure-iot-samples-ios
[lnk-ios-pal]: https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx
[lnk-c-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
