---
title: 'Hızlı Başlangıç: Konuşma tanıma, konuşma Hizmetleri Swift - tanıması'
titleSuffix: Azure Cognitive Services
description: Speech SDK'sı kullanarak iOS Swift de Konuşma tanımayı öğrenmesine
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 98c37d4a39235e5ed1a82df72886d196cbf19e49
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605033"
---
# <a name="quickstart-recognize-speech-in-swift-on-ios-using-the-speech-sdk"></a>Hızlı Başlangıç: Speech SDK'sı kullanarak iOS swift'te konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne bir mikrofondan kaydedilen özelliği Bilişsel hizmetler konuşma SDK'sı kullanarak swift'te iOS uygulaması oluşturulacağını öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce önkoşullarının listesi aşağıda verilmiştir:

* A [abonelik anahtarı](get-started.md) konuşma hizmeti için.
* Bir macOS makineyle [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri ve [CocoaPods](https://cocoapods.org/) yüklü.

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.6.0`. Bu öğretici SDK'sının daha eski sürümleri için değişiklik yapmadan çalışmaz unutmayın.

Bilişsel hizmetler konuşma SDK'sı iOS için bir çerçeve paket dağıtılır.
Xcode projelerinde kullanılabilir bir [CocoaPod](https://cocoapods.org/), veya'ndan indirilmiş https://aka.ms/csspeech/macosbinary ve el ile bağlanır. Bu kılavuzda bir CocoaPod kullanır.

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluştur

Xcode’u başlatın ve **Dosya** > **Yeni** > **Proje** seçeneklerine tıklayarak yeni bir proje başlatın.
Şablon seçimi iletişim kutusunda, “iOS Tek Görünüm Uygulaması” şablonunu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Apple Geliştirici hesabınız zaten varsa, uygun bir kuruluş adı ve bir kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profili gerekir. Başvurmak [Apple Geliştirici sitesine](https://developer.apple.com/) Ayrıntılar için.
    1. Swift proje dili olarak seçilmiş olduğundan emin olun.
    1. Görsel Taslaklar kullanın ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı bırakın. Basit kullanıcı Arabirimi için örnek uygulamayı programlı olarak oluşturulur.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
1. Proje dizini seçin
    1. Proje yerleştirmek için bir dizin seçin. Bu, oluşturur bir `helloworld` Xcode projesi için tüm dosyaları içeren seçtiğiniz dizine bir dizin.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Uygulamanın mikrofon kullanımını bildirmek de gerekir. `Info.plist` dosya. Genel Bakış dosyasında tıklayın ve "Mikrofon konuşma tanıma için gerektiği gibi" değeri "Gizlilik – mikrofon kullanım açıklaması" anahtarını ekleyin.
    ![Info.plist dosyasındaki ayarları](media/sdk/qs-swift-ios-info-plist.png)
1. Xcode projesi kapatın. CocoaPods ayarlandıktan sonra daha sonra farklı bir örneğine bunu kullanır.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Adlı yeni bir üstbilgi dosyası yerleştirin `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` içine `helloworld` dizin içinde helloworld proje ve içine aşağıdaki kodu yapıştırın: [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]
1. Göreli yolu Ekle `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` Swift için köprü oluşturma üst bilgisi için proje ayarları helloworld hedefi *Objective-C köprü oluşturma üst bilgisi* alan ![üstbilgi özellikleri](media/sdk/qs-swift-ios-bridging-header.png)
1. Otomatik olarak oluşturulan içeriklerini `AppDelegate.swift` tarafından dosya: [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/AppDelegate.swift#code)]
1. Otomatik olarak oluşturulan içeriklerini `ViewController.swift` tarafından dosya: [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/ViewController.swift#code)]
1. İçinde `ViewController.swift`, dize değiştirin `YourSubscriptionKey` abonelik.
1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

## <a name="install-the-sdk-as-a-cocoapod"></a>Bir CocoaPod SDK'sını yükleyin

1. CocoaPod bağımlılık Yöneticisi bölümünde anlatılan şekilde yükleyin, [yükleme yönergeleri](https://guides.cocoapods.org/using/getting-started.html).
1. Örnek uygulamanızın dizine gidin (`helloworld`). Bir metin dosyası adıyla yerleştirmek `Podfile` ve aşağıdaki içeriği dizindeki: [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/Podfile)]
1. Gidin `helloworld` terminal ve şu komutu çalıştırın, dizin `pod install`. Bu oluşturacak bir `helloworld.xcworkspace` hem örnek uygulamasını ve Speech SDK'sı bağımlılık olarak içeren bir Xcode çalışma alanı. Bu çalışma alanı aşağıdaki kullanılır.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Açık `helloworld.xcworkspace` Xcode çalışma.
1. Hata ayıklama çıkışını görünür hale getirin (**Görünüm** > **Hata Ayıklama Alanı** > **Konsolu Etkinleştir**).
1. İOS simülatörü veya listeden bir uygulama için hedef olarak geliştirme makinenize bağlı bir iOS cihazını seçin **ürün** > **hedef** menüsü.
1. Menüden **Ürün** > **Çalıştır** seçeneklerini belirleyerek veya **Oynat** düğmesine tıklayarak iOS simülatöründe örnek kodu derleyin ve çalıştırın.
1. Uygulamayı "Tanı" düğmesine tıklayın ve birkaç sözcük söyleyin sonra ekranın alt bölümünde, konuşulan metnin görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da örneklerimizi keşfedin](https://aka.ms/csspeech/samples)
