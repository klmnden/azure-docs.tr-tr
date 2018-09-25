---
title: "Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sı kullanarak iOS Objective-C, konuşma tanıma"
titleSuffix: Microsoft Cognitive Services
description: Bilişsel hizmetler konuşma SDK'sı kullanarak iOS üzerinde Objective-c Konuşma tanımayı öğrenmesine
services: cognitive-services
author: chlandsi
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 09/24/2018
ms.author: chlandsi
ms.openlocfilehash: e21348ccd694baf6b7eccf2787ec0a9f21a73b11
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985665"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-cognitive-services-speech-sdk"></a>Hızlı Başlangıç: Bilişsel hizmetler konuşma SDK'sı kullanarak iOS Objective-C, konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Objective-C ses dosyası ile kaydedilen Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sını kullanarak bir iOS uygulaması oluşturma konusunda bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir abonelik anahtarı. Bkz: [konuşma hizmeti ücretsiz olarak deneyin](get-started.md).
* Bir Mac 9.4.1 iOS geliştirme ortamı olarak yüklü Xcode ile. Bu öğreticide, iOS sürümleri 11.4 hedefler. Xcode yoksa henüz buradan yükleyebilirsiniz, [App Store](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12).

## <a name="get-the-speech-sdk-for-ios"></a>İOS için Speech SDK'sı Al

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel hizmetler konuşma SDK'ın geçerli sürümü `1.0.0`.

Bilişsel hizmetler konuşma SDK'sı Mac ve iOS için şu anda bir Cocoa çerçeve dağıtılır.
Dan indirilebilir https://aka.ms/csspeech/iosbinary. Dosyayı giriş dizininize yükleyin.


## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluştur 

Xcode başlatın ve tıklayarak yeni bir proje başlatın **dosya** > **yeni** > **proje**.
Şablon Seçimi iletişim kutusunda, seçin "iOS tek görünüm uygulamanızı" şablonu.

İletişim kutularındaki, izleyin, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri iletişim kutusu
    1. Hızlı Başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Bir kuruluş adı girin `TestOrg`ve bir kuruluş tanımlayıcısı gibi `testorg`.
    1. Objective-C proje dili olarak seçilmiş olduğundan emin olun.
    1. Testleri ve temel veri tüm onay kutularını devre dışı bırakın.
    ![Proje ayarları](media/sdk/qs-objectivec-project-settings.png)
1. Proje dizini seçin
    1. Proje yerleştirmek için giriş dizininizi seçin. Bu oluşturacak bir `helloworld` dizin giriş dizininizde Xcode projesi için tüm dosyaları içerir.
    1. Bu örnek proje için bir Git deposu oluşturma devre dışı bırakın.
    1. SDK'yı yolları Ayarla *proje ayarları*.
        1. İçinde **genel** sekmesinde altında **bağlantılı çerçeveler ve kitaplıklar** üst bilgi bir çerçeve SDK'sı kitaplığı Ekle: **Add framework** > **Ekle Diğer...**  > Giriş dizinine gidin ve dosyayı seçin `MicrosoftCognitiveServicesSpeech.framework`.
        ![Eklenen Framework](media/sdk/qs-objectivec-framework.png)
        1. Git **Build Settings** sekme ve etkinleştirme **tüm** ayarları.
        1. Dizin Ekle `$(SRCROOT)/..` için *çerçeve arama yollarını* altında **arama yollarını** başlığı.
        ![Çerçeve arama yolu ayarını](media/sdk/qs-objectivec-framework-search-paths.png)
        1. Dizin Ekle `$(SRCROOT)/..` için *Runpath arama yollarını* altında **bağlama** başlığı.
        ![Runpath arama yolu ayarı](media/sdk/qs-objectivec-runpaths.png)


## <a name="set-up-the-ui"></a>UI'yi Kur

Örnek uygulamayı çok basit bir kullanıcı Arabirimi olacaktır: dosya ve sonucu görüntülemek için bir metin etiketi işlemeye başlamak için bir düğme.
Kullanıcı arabirimini ayarlanır `Main.storyboard` projenin bir parçası.
Film şeridi XML görünümünü açın, sağ tıklayarak `Main.storyboard` projenin giriş ağacı ve seçme **farklı Aç...**   >  **Kaynak kodu**.
Otomatik olarak oluşturulan XML şununla değiştirin:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Örnek kod ekleyin

1. İndirme [örnek wav dosyası](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) bağlantıya tıklayıp seçme **Hedefi Farklı Kaydet...** . Wav dosyası, projeye bir Bulucu penceresinden Proje Görünümü ' kök düzeye sürükleyerek bir kaynak olarak ekleyin.
Tıklayın **son** ayarları değiştirmeden aşağıdaki iletişim.
1. Otomatik olarak oluşturulan içeriklerini `ViewController.m` tarafından dosya:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. Dize değiştirin `YourSubscriptionKey` abonelik.

1. Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili (örneğin, `westus` ücretsiz deneme aboneliği için).


## <a name="building-and-running-the-sample"></a>Oluşturma ve çalıştırma örneği

1. Hata ayıklama çıkışını görünür hale getirmek (**görünümü** > **hata ayıklama alan** > **etkinleştirme konsol**).
1. Derlemek ve örnek kod seçerek iOS simulator'da çalıştırmak **ürün** -> **çalıştırma** menüsünden veya **Play** düğmesi.
1. "Tanı!" düğmesine tıkladıktan sonra uygulamada, düğme görmeniz gerekir "Gibi bir hava durumu nedir?" içeriğini ses dosyası Benzetimli ekranın alt bölümünde üzerinde.

 ![Benzetimli iOS uygulaması](media/sdk/qs-objectivec-simulated-app.png)

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Bu örnekte arayın `quickstart/objectivec-ios` klasör.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneklerimizi Al](speech-sdk.md#get-the-samples)
