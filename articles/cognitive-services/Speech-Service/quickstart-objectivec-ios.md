---
title: "Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sı kullanarak iOS Objective-C, konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler konuşma SDK'sı kullanarak iOS üzerinde Objective-c Konuşma tanımayı öğrenmesine
services: cognitive-services
author: chlandsi
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 10/12/2018
ms.author: chlandsi
ms.openlocfilehash: ce9979d8d300f2308a4b7a22791c242409f2c988
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341183"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-cognitive-services-speech-sdk"></a>Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sı kullanarak iOS Objective-C, konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Objective-C ses dosyası ile kaydedilen Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sını kullanarak bir iOS uygulaması oluşturma konusunda bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir Mac 9.4.1 iOS geliştirme ortamı olarak yüklü Xcode ile. Bu öğreticide, iOS sürümleri 11.4 hedefler. Xcode yoksa henüz buradan yükleyebilirsiniz, [App Store](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12).

## <a name="get-the-speech-sdk-for-ios"></a>İOS için Speech SDK'sı Al

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.0.1`.

Bilişsel hizmetler konuşma SDK'sı Mac ve iOS için şu anda bir Cocoa çerçeve dağıtılır.
Dan indirilebilir https://aka.ms/csspeech/iosbinary. Dosyayı giriş dizininize yükleyin.

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluştur 

Xcode başlatın ve tıklayarak yeni bir proje başlatın **dosya** > **yeni** > **proje**.
Şablon Seçimi iletişim kutusunda, seçin "iOS tek görünüm uygulamanızı" şablonu.

İletişim kutularındaki, izleyin, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri iletişim kutusu
    1. Hızlı Başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Apple Geliştirici hesabınız zaten varsa, bir uygun kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla gibi herhangi bir ad yalnızca seçebilir `testorg`. Uygulamayı imzalamak için de uygun bir sağlama profili gerekir. Lütfen [Apple Geliştirici sitesine](https://developer.apple.com/) Ayrıntılar için.
    1. Objective-C proje dili olarak seçilmiş olduğundan emin olun.
    1. Testleri ve temel veri tüm onay kutularını devre dışı bırakın.
    ![Proje ayarları](media/sdk/qs-objectivec-project-settings.png)
1. Proje dizini seçin
    1. Proje yerleştirmek için giriş dizininizi seçin. Bu oluşturacak bir `helloworld` dizin giriş dizininizde Xcode projesi için tüm dosyaları içerir.
    1. Bu örnek proje için bir Git deposu oluşturma devre dışı bırakın.
    1. SDK'yı yolları Ayarla *proje ayarları*.
        1. İçinde **genel** sekmesinde altında **katıştırılmış ikili dosyalar** üst bilgi bir çerçeve SDK'sı kitaplığı Ekle: **Ekle katıştırılmış ikili dosyalar** > **diğer Ekle ...**  > Giriş dizinine gidin ve dosyayı seçin `MicrosoftCognitiveServicesSpeech.framework`. Bu da otomatik olarak üst SDK'sı kitaplığı ekler **bağlı çerçeve ve kitaplıklar**.
        ![Eklenen Framework](media/sdk/qs-objectivec-framework.png)
        1. Git **Build Settings** sekme ve etkinleştirme **tüm** ayarları.
        1. Dizin Ekle `$(SRCROOT)/..` için *çerçeve arama yollarını* altında **arama yollarını** başlığı.
        ![Çerçeve arama yolu ayarını](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>UI'yi Kur

Örnek uygulamayı çok basit bir kullanıcı Arabirimi olacaktır: dosya veya mikrofon girişi ve sonucu görüntülemek için bir metin etiketi konuşma tanıma başlatmak için iki düğme.
Kullanıcı arabirimini ayarlanır `Main.storyboard` projenin bir parçası.
Film şeridi XML görünümünü açın, sağ tıklayarak `Main.storyboard` projenin giriş ağacı ve seçme **farklı Aç...**   >  **Kaynak kodu**.
Otomatik olarak oluşturulan XML şununla değiştirin:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. İndirme [örnek wav dosyası](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) bağlantıya tıklayıp seçme **Hedefi Farklı Kaydet...** . Wav dosyası, projeye bir Bulucu penceresinden Proje Görünümü ' kök düzeye sürükleyerek bir kaynak olarak ekleyin.
Tıklayın **son** ayarları değiştirmeden aşağıdaki iletişim.
1. Otomatik olarak oluşturulan içeriklerini `ViewController.m` tarafından dosya:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.
1. Mikrofon erişimi için istek ekleyin. Sağ tıklayın `Info.plist` girişi seçin ve proje ağacı **farklı Aç...**   >  **Kaynak kodu**. İçine aşağıdaki satırları ekleyin `<dict>` bölümüne ve ardından dosyayı kaydedin.
    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="building-and-running-the-sample"></a>Oluşturma ve çalıştırma örneği

1. Hata ayıklama çıkışını görünür hale getirmek (**görünümü** > **hata ayıklama alan** > **etkinleştirme konsol**).
1. İOS simülatörü veya listeden bir uygulama için hedef olarak geliştirme makinenize bağlı bir iOS cihazı seçin **ürün** -> **hedef** menüsü.
1. Derlemek ve örnek kod seçerek iOS simulator'da çalıştırmak **ürün** -> **çalıştırma** menüsünden veya **Play** düğmesi.
Şu anda Speech SDK'sı, yalnızca 64 bit iOS platformlarını destekler.
1. "Tanı (dosya)" düğmesine tıkladıktan sonra "Gibi bir hava durumu nedir?" ses dosyasının içeriğini görmeniz gerekir ekranın alt bölümünde üzerinde.

 ![Benzetimli iOS uygulaması](media/sdk/qs-objectivec-simulated-app.png)

1. Uygulamayı "Tanı (mikrofon)" düğmesine tıklayın ve birkaç sözcük söyleyin sonra ekranın alt bölümünde, konuşulan metnin görmeniz gerekir.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/objectivec-ios` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneklerimizi Al](speech-sdk.md#get-the-samples)

