---
title: 'Hızlı Başlangıç: Konuşma tanıma Objective-C - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Speech SDK'sı kullanarak iOS üzerinde Objective-c Konuşma tanımayı öğrenmesine
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: chlandsi
ms.openlocfilehash: 347969ac129faa9cbe841be097e2bc7fd66c6b8e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65020560"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-speech-sdk"></a>Hızlı Başlangıç: Speech SDK'sı kullanarak iOS Objective-C, konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Objective-C konuşma mikrofon veya kaydedilmiş sesli bir dosyadan metin özelliği, Bilişsel hizmetler konuşma SDK'sı kullanarak bir iOS uygulaması oluşturma konusunda bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce önkoşullarının listesi aşağıda verilmiştir:

* A [abonelik anahtarı](get-started.md) konuşma hizmeti
* Bir macOS makineyle [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri
* Hedef sürüm iOS 9.3 veya üzerini ayarlayın

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.5.0`.

Bilişsel hizmetler konuşma SDK'sı iOS için şu anda bir Cocoa çerçeve dağıtılır.
Dan indirilebilir [burada](https://aka.ms/csspeech/iosbinary). Dosyayı giriş dizininize indirin.

## <a name="create-an-xcode-project"></a>Xcode Projesi oluşturma

Xcode’u başlatın ve **Dosya** > **Yeni** > **Proje** seçeneklerine tıklayarak yeni bir proje başlatın.
Şablon seçimi iletişim kutusunda, “iOS Tek Görünüm Uygulaması” şablonunu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Zaten bir Apple geliştirici hesabınız varsa uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profili gerekir. Başvurmak [Apple Geliştirici sitesine](https://developer.apple.com/) Ayrıntılar için.
    1. Proje dili olarak Objective-C seçildiğinden emin olun.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
    ![Proje Ayarları](media/sdk/qs-objectivec-project-settings.png)
1. Proje dizini seçin
    1. Projeyi yerleştirmek için giriş dizininizi seçin. Bu, oluşturur bir `helloworld` dizin giriş dizininizde Xcode projesi için tüm dosyaları içerir.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
    1. *Proje Ayarları*’nda SDK yollarını ayarlayın.
        1. İçinde **genel** sekmesinde altında **katıştırılmış ikili dosyalar** üst bilgi bir çerçeve SDK'sı kitaplığı ekleyin: **Katıştırılmış ikili dosyalar ekleme** > **diğer Ekle...**  > Giriş dizinine gidin ve dosyayı seçin `MicrosoftCognitiveServicesSpeech.framework`. Bu SDK'sı kitaplığı üstbilgiye ekler **bağlı çerçeve ve kitaplıklar** otomatik olarak.
        ![Eklenen Çerçeve](media/sdk/qs-objectivec-framework.png)
        1. **Derleme Ayarları** sekmesine gidin ve **Tümü** ayarını etkinleştirin.
        1. `$(SRCROOT)/..` dizinini **Arama Yolları** başlığı altında *Çerçeve Arama Yolları*’na ekleyin.
        ![Çerçeve Arama Yolu ayarı](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Kullanıcı arabirimini ayarlama

Örnek uygulama, çok basit bir kullanıcı Arabirimi vardır: Konuşma tanıma dosyasından veya mikrofon girişi ve sonucu görüntülemek için bir metin etiketi başlatmak için iki düğme.
Kullanıcı arabirimi projenin `Main.storyboard` bölümünde ayarlanır.
Proje ağacında `Main.storyboard` girdisine sağ tıklayıp **Farklı Aç...** > **Kaynak Kod** seçeneklerini belirleyerek görsel taslağın XML görünümünü açın.
Otomatik olarak oluşturulan XML şu kodla değiştirin:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Bağlantıya sağ tıklayıp **Hedefi farklı kaydet...** seçeneğini belirleyerek [örnek wav dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) indirin. Wav dosyasını bir Bulucu penceresinden Proje görünümünün kök düzeyine sürükleyerek projeye bir kaynak olarak ekleyin.
   Aşağıdaki iletişim kutusunda ayarları değiştirmeden **Son**’a tıklayın.
1. Aşağıdaki şekilde otomatik oluşturulan `ViewController.m` dosyasının içeriğini değiştirin:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.
1. Mikrofon erişimi için isteği ekleyin. Sağ `Info.plist` girişi seçin ve proje ağacı **farklı Aç...**   >  **Kaynak kodu**. Aşağıdaki satırları `<dict>` bölümüne ekleyin ve ardından dosyayı kaydedin.
    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="building-and-running-the-sample"></a>Örneği Derleme ve Çalıştırma

1. Hata ayıklama çıkışını görünür hale getirin (**Görünüm** > **Hata Ayıklama Alanı** > **Konsolu Etkinleştir**).
1. İOS simülatörü veya listeden bir uygulama için hedef olarak geliştirme makinenize bağlı bir iOS cihazını seçin **ürün** -> **hedef** menüsü.
1. Menüden **Ürün** -> **Çalıştır** seçeneklerini belirleyerek veya **Oynat** düğmesine tıklayarak iOS simülatöründe örnek kodu derleyin ve çalıştırın.
   Konuşma SDK’sı şu anda yalnızca 64 bit iOS platformlarını desteklemektedir.
1. Uygulamada "Tanı (Dosya)" düğmesine tıkladığınızda, ekranın alt kısmında "What's the weather like?" ses dosyasının içeriklerini görmeniz gerekir.

   ![iOS Uygulaması Simülasyonu](media/sdk/qs-objectivec-simulated-app.png)

1. Uygulamada “Tanı (Mikrofon)” düğmesine tıklayıp birkaç sözcük söyledikten sonra, konuştuğunuz metni ekranın alt bölümünde görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde Objective-C örneklerini keşfedin](https://aka.ms/csspeech/samples)

