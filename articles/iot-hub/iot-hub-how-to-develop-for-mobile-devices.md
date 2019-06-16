---
title: Azure IOT SDK'larını kullanarak mobil cihazlar için geliştirme | Microsoft Docs
description: Geliştirici Kılavuzu - Azure IOT Hub SDK'larını kullanarak mobil cihazlar için geliştirme hakkında bilgi edinin.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: yizhon
ms.openlocfilehash: 5256a58a2b68584888abcac915392d8e389e9772
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60399377"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Azure IOT SDK'larını kullanarak mobil cihazlar için geliştirme

Değişen bir özellik ile çok çeşitli cihazları için nesnelerin interneti şeyler başvurabilir: algılayıcılar, denetleyicilere, akıllı cihazlar, endüstriyel ağ geçitleri ve hatta mobil cihazları.  Bir mobil cihazı, CİHAZDAN buluta telemetri gönderen ve bulut tarafından yönetilen bir IOT cihazı olabilir.  Ayrıca, diğer IOT cihazları yöneten bir arka uç hizmeti uygulaması çalıştıran cihaz de olabilir.  Her iki durumda da [Azure IOT Hub SDK'ları](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) mobil cihazlarda çalışan uygulamalar geliştirmek için kullanılabilir.  

## <a name="develop-for-native-ios-platform"></a>Yerel iOS platform için geliştirin

Azure IOT Hub SDK'ları ile Azure IOT Hub C SDK'sını yerel iOS platform desteği sağlar.  Bu, bir iOS Swift veya Objective C XCode projenizde birleştirebilirsiniz SDK'sı olarak düşünebilirsiniz.  C SDK'sı iOS kullanmanın iki yolu vardır:

* XCode projesi CocoaPod kitaplıkları doğrudan kullanın.  
* C SDK'sı için kaynak kodunu indirebilir ve iOS platformu şu için derleme [yönerge yapı](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) MacOS için.  

Azure IOT Hub C SDK'sı, çeşitli platformlar için en fazla taşınabilir C99 yazılır.  Taşıma işlemi için burada bulunabilir platforma özgü bileşenler için ince benimseme katmanındaki yazma içerir [iOS](https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx).  C SDK'sı içindeki özellikleri, desteklenen Azure IOT hub'ı temelleri dahil olmak üzere iOS platformunda faydalanacaklarını ve gibi yeniden deneme ilkesi ağ güvenilirlik SDK özgü özellikler.  İOS arabirimi SDK ayrıca Azure IOT Hub C SDK'sı arabirimi benzer.  

Bu belgeler, bir cihaz uygulaması veya bir iOS cihazında hizmet uygulaması geliştirme konusunda rehberlik:

* [Hızlı Başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-ios.md)  
* [Cihazınızı IOT hub ile buluttan iletiler gönderme](iot-hub-ios-swift-c2d.md) 

### <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Azure IOT hub'ı CocoaPod kitaplıkları ile geliştirin

Azure IOT Hub SDK'ları, iOS geliştirme için Objective-C CocoaPod kitaplıkları kümesini serbest bırakır.  CocoaPod kitaplıklarının en son listesi için bkz [için Microsoft Azure IOT CocoaPods](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md).  İlgili kitaplıkları XCode projenize dahil edilir sonra IOT Hub ilgili kod yazmak için iki yolu vardır:

* Objective C işlevi: Objective-C içinde projenizin yazılmışsa, Azure IOT Hub C SDK'sından API'lerini doğrudan çağırabilir.  Projeniz Swift yazılmışsa çağırabilirsiniz `@objc func` önce işlevinizi oluşturma ve C ya da Objective-C kodu kullanarak Azure IOT Hub ile ilgili tüm logics yazmaya devam edin.  Her ikisini de gösteren örnekler kümesi bulunabilir [örnek depoyu](https://github.com/Azure-Samples/azure-iot-samples-ios).  

* C örnekleri dahil: C cihaz uygulama yazdıysanız, XCode projenizde doğrudan başvurabilirsiniz:
    * Sample.c dosyayı Xcode'dan XCode projenize ekleyin.  
    * Üstbilgi dosyası, bağımlılığı ekleyin.  Bir üstbilgi dosyasını dahil [örnek depoyu](https://github.com/Azure-Samples/azure-iot-samples-ios) örnek olarak. Daha fazla bilgi için lütfen için Apple'nın belgeleri sayfasını ziyaret edin [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="develop-for-android-platform"></a>Android platformuna yönelik geliştirme
Azure IOT Hub Java SDK'sı, Android platformunu destekler.  Sınanan belirli API sürümü için lütfen bizim [platform destek sayfasını](iot-hub-device-sdk-platform-support.md) en son güncelleştirmesi.

Bu belgeler, bir cihaz uygulaması veya hizmet uygulaması Gradle ve Android Studio kullanarak bir Android cihazında geliştirme konusunda rehberlik:

* [Hızlı Başlangıç: Bir IOT hub'ına bir CİHAZDAN telemetri gönderme](quickstart-send-telemetry-android.md)  
* [Hızlı Başlangıç: Cihazı denetleme](quickstart-control-device-android.md) 

## <a name="next-steps"></a>Sonraki adımlar

* [IOT Hub REST API Başvurusu](https://docs.microsoft.com/rest/api/iothub/)
* [Azure IOT C SDK'sı kaynak kodu](https://github.com/Azure/azure-iot-sdk-c)
