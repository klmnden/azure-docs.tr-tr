---
title: 'Hızlı Başlangıç: Konuşma tanıma Objective-C - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'sını kullanarak macOS üzerinde Objective-c Konuşma tanımayı öğrenmesine
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/03/2019
ms.author: chlandsi
ms.openlocfilehash: 55fc671d926880375b0420e0eafb6dc63f170ba6
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012202"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Hızlı Başlangıç: Objective C'de konuşma Speech SDK'sı kullanarak Macos'ta tanıması

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne bir mikrofondan kaydedilen özelliği Bilişsel hizmetler konuşma SDK'sı kullanarak Objective-C içinde bir macOS uygulaması oluşturulacağını öğrenin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce önkoşullarının listesi aşağıda verilmiştir:

* A [abonelik anahtarı](get-started.md) konuşma hizmeti
* Bir macOS makineyle [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri ve macOS 10.13 veya üzeri

## <a name="get-the-speech-sdk-for-macos"></a>MacOS için Speech SDK'sı Al

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.4.0`.

Bilişsel hizmetler konuşma SDK'sı Mac için bir çerçeve paket dağıtılır.
Xcode projelerinde kullanılabilir bir [CocoaPod](https://cocoapods.org/), veya'ndan indirilmiş https://aka.ms/csspeech/macosbinary ve el ile bağlanır. Bu kılavuzda bir CocoaPod kullanır.

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluştur

Xcode’u başlatın ve **Dosya** > **Yeni** > **Proje** seçeneklerine tıklayarak yeni bir proje başlatın.
Şablon Seçimi iletişim kutusunda, "Cocoa uygulaması" şablonu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Apple Geliştirici hesabınız zaten varsa, uygun bir kuruluş adı ve bir kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profili gerekir. Başvurmak [Apple Geliştirici sitesine](https://developer.apple.com/) Ayrıntılar için.
    1. Proje dili olarak Objective-C seçildiğinden emin olun.
    1. Görsel Taslaklar kullanın ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı bırakın. Basit kullanıcı Arabirimi için örnek uygulamayı programlı olarak oluşturulur.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
    ![Proje ayarları](media/sdk/qs-objectivec-macos-project-settings.png)
1. Proje dizini seçin
    1. Proje yerleştirmek için bir dizin seçin. Bu, oluşturur bir `helloworld` dizin giriş dizininizde Xcode projesi için tüm dosyaları içerir.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Ağ ve mikrofon erişimi için yetkilendirmeleri ayarlama. Genel bakış için uygulama yapılandırmasını almak için sol taraftaki ilk satırında uygulama adına tıklayın ve ardından "Özellikler" sekmesini seçin.
    1. Uygulama için "Uygulama korumalı alanı" ayarını etkinleştirin.
    1. "Giden bağlantılar" ve "Mikrofon" erişim onay kutularını etkinleştirin.
    ![Korumalı alan ayarları](media/sdk/qs-objectivec-macos-sandbox.png)
1. Uygulamanın mikrofon kullanımını bildirmek de gerekir. `Info.plist` dosya. Genel Bakış dosyasında tıklayın ve "Mikrofon konuşma tanıma için gerektiği gibi" değeri "Gizlilik – mikrofon kullanım açıklaması" anahtarını ekleyin.
    ![Info.plist dosyasındaki ayarları](media/sdk/qs-objectivec-macos-info-plist.png)
1. Xcode projesi kapatın. CocoaPods ayarlandıktan sonra daha sonra farklı bir örneğine bunu kullanır.

## <a name="install-the-sdk-as-a-cocoapod"></a>Bir CocoaPod SDK'sını yükleyin

1. CocoaPod bağımlılık Yöneticisi bölümünde anlatılan şekilde yükleyin, [yükleme yönergeleri](https://guides.cocoapods.org/using/getting-started.html).
1. Örnek uygulamanızın dizine gidin (`helloworld`). Bir metin dosyası adıyla yerleştirmek `Podfile` ve aşağıdaki içeriği dizindeki:
    ```
    target 'helloworld' do
        platform :osx, '10.13'
        pod 'MicrosoftCognitiveServicesSpeech-macOS', '~> 1.4.0'
    end
    ```
1. Gidin `helloworld` terminal ve şu komutu çalıştırın, dizin `pod install`. Bu oluşturacak bir `helloworld.xcworkspace` hem örnek uygulamasını ve Speech SDK'sı bağımlılık olarak içeren bir Xcode çalışma alanı. Bu çalışma alanı aşağıdaki kullanılır.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Açık `helloworld.xcworkspace` Xcode çalışma.
1. Otomatik olarak oluşturulan içeriklerini `AppDelegate.m` tarafından dosya: [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/helloworld/AppDelegate.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Hata ayıklama çıkışını görünür hale getirin (**Görünüm** > **Hata Ayıklama Alanı** > **Konsolu Etkinleştir**).
1. Derleme ve seçerek örnek kodu çalıştırma **ürün** -> **çalıştırmak** menüsünden veya **Play** düğmesi.
1. Düğmesine tıklayın ve birkaç sözcük söyleyin sonra ekranın alt bölümünde, konuşulan metnin görmeniz gerekir. Uygulamayı ilk kez çalıştırdığınızda, bilgisayarınızın mikrofon için uygulama erişimi vermek için istenecektir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde Objective-C örneklerini keşfedin](https://aka.ms/csspeech/samples)

