---
title: Azure IOT SDK'larını kullanarak mobil cihazlar için geliştirme | Microsoft Docs
description: Geliştirici Kılavuzu - Azure IOT Hub SDK'larını kullanarak mobil cihazlar için geliştirme hakkında bilgi edinin.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: yizhon
ms.openlocfilehash: 4a94abe69b525dc1b03fe2c1ae9593f3c6399f56
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339762"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Azure IOT SDK'larını kullanarak mobil cihazlar için geliştirme

Değişen bir özellik ile çok çeşitli cihazları için nesnelerin interneti şeyler başvurabilir: algılayıcılar, denetleyicilere, akıllı cihazlar, endüstriyel ağ geçitleri ve hatta mobil cihazları.  Bir mobil cihazı, CİHAZDAN buluta telemetri gönderen ve bulut tarafından yönetilen bir IOT cihazı olabilir.  Ayrıca, diğer IOT cihazları yöneten bir arka uç hizmeti uygulaması çalıştıran cihaz de olabilir.  Her iki durumda da [Azure IOT Hub SDK'ları](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) mobil cihazlarda çalışan uygulamalar geliştirmek için kullanılabilir.  

## <a name="develop-for-native-ios-platform"></a>Yerel iOS platform için geliştirin

Azure IOT Hub SDK'ları ile Azure IOT Hub C SDK'sını yerel iOS platform desteği sağlar.  Bu, bir iOS Swift veya Objective C XCode projenizde birleştirebilirsiniz SDK'sı olarak düşünebilirsiniz.  C SDK'sı iOS kullanmanın iki yolu vardır:

* XCode projesi CocoaPod kitaplıkları doğrudan kullanın.  

* C SDK'sı için kaynak kodunu indirebilir ve iOS platformu şu için derleme [yönerge yapı](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) MacOS için.  

Azure IOT Hub C SDK'sı, çeşitli platformlar için en fazla taşınabilir C99 yazılır.  Taşıma işlemi için burada bulunabilir platforma özgü bileşenler için ince benimseme katmanındaki yazma içerir [iOS](https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx).  C SDK'sı içindeki özellikleri, desteklenen Azure IOT hub'ı temelleri dahil olmak üzere iOS platformunda faydalanacaklarını ve gibi yeniden deneme ilkesi ağ güvenilirlik SDK özgü özellikler.  İOS arabirimi SDK ayrıca Azure IOT Hub C SDK'sı arabirimi benzer.  

Bu belgeler, bir cihaz uygulaması veya bir iOS cihazında hizmet uygulaması geliştirme konusunda rehberlik:

* [Hızlı Başlangıç: Bir cihazdan IoT hub’a telemetri gönderme](quickstart-send-telemetry-ios.md)  

* [Cihazınızı IOT hub ile buluttan iletiler gönderme](iot-hub-ios-swift-c2d.md) 

## <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Azure IOT hub'ı CocoaPod kitaplıkları ile geliştirin

Azure IOT Hub SDK'ları, iOS geliştirme için Objective-C CocoaPod kitaplıkları kümesini serbest bırakır.  CocoaPod kitaplıklarının en son listesi için bkz [için Microsoft Azure IOT CocoaPods](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md).  İlgili kitaplıkları XCode projenize dahil edilir sonra IOT Hub ilgili kod yazmak için iki yolu vardır:

* Objective C işlevi: Objective-C içinde projenizin yazılmışsa, API'leri Azure IOT Hub C SDK'sından doğrudan çağırabilirsiniz.  Projeniz Swift yazılmışsa çağırabilirsiniz `@objc func` önce işlevinizi oluşturma ve C ya da Objective-C kodu kullanarak Azure IOT Hub ile ilgili tüm logics yazmaya devam edin.  Her ikisini de gösteren örnekler kümesi bulunabilir [örnek depoyu](https://github.com/Azure-Samples/azure-iot-samples-ios).  

* C örnekleri dahil edilip derecelendirilir: C cihaz uygulama yazdıysanız, XCode projenizde doğrudan başvurabilirsiniz:

    * Sample.c dosyayı Xcode'dan XCode projenize ekleyin.  
    
    * Üstbilgi dosyası, bağımlılığı ekleyin.  Bir üstbilgi dosyasını dahil [örnek depoyu](https://github.com/Azure-Samples/azure-iot-samples-ios) örnek olarak. Daha fazla bilgi için lütfen için Apple'nın belgeleri sayfasını ziyaret edin [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="next-steps"></a>Sonraki adımlar

* [IOT Hub REST API Başvurusu](https://docs.microsoft.com/rest/api/iothub/)
* [Azure IOT C SDK'sı kaynak kodu](https://github.com/Azure/azure-iot-sdk-c)
