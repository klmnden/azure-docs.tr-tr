---
title: 'Hızlı Başlangıç: konuşma tanıma Objective-C - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Konuşma Tanıma Hizmeti SDK’sını kullanarak iOS üzerinde Objective-C’de konuşma tanımayı öğrenme
services: cognitive-services
author: chlandsi
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/06/2018
ms.author: chlandsi
ms.openlocfilehash: eaa44f942082c6bd062599dbdd0401fe4505daf4
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53090222"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-using-the-speech-service-sdk"></a>Hızlı Başlangıç: Konuşma Tanıma Hizmeti SDK’sını kullanarak iOS üzerinde Objective-C’de konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, kayıtlı konuşmayı metne dönüştürme ile bir ses dosyasının dökümünü almak için Bilişsel Hizmetler Konuşma SDK’sını kullanarak iOS içinde bir Objective-C uygulaması oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce önkoşullarının listesi aşağıda verilmiştir:

* A [abonelik anahtarı](get-started.md) konuşma hizmeti
* Bir macOS makineyle [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri
* Hedef iOS sürüm 11.4 veya sonraki sürümünü ayarlayın

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.1.0`.

Mac ve iOS için Bilişsel Hizmetler Konuşma SDK’sı şu anda bir Cocoa Framework olarak dağıtılmaktadır.
https://aka.ms/csspeech/iosbinary konumundan indirilebilir. Dosyayı giriş dizininize indirin.

## <a name="create-an-xcode-project"></a>Xcode Projesi oluşturma

Xcode’u başlatın ve **Dosya** > **Yeni** > **Proje** seçeneklerine tıklayarak yeni bir proje başlatın.
Şablon seçimi iletişim kutusunda, “iOS Tek Görünüm Uygulaması” şablonunu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Zaten bir Apple geliştirici hesabınız varsa uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için, ayrıca uygun bir sağlama profili gerekir. Lütfen ayrıntılar için [Apple geliştirici sitesine](https://developer.apple.com/) bakın.
    1. Proje dili olarak Objective-C seçildiğinden emin olun.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
    ![Proje Ayarları](media/sdk/qs-objectivec-project-settings.png)
1. Proje dizini seçin
    1. Projeyi yerleştirmek için giriş dizininizi seçin. Bu, giriş dizininizde Xcode projesi için tüm dosyaları içeren bir `helloworld` dizini oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
    1. *Proje Ayarları*’nda SDK yollarını ayarlayın.
        1. **Katıştırılmış İkili Dosyalar** üst bilgisi altında **Genel** sekmesinde, SDK kitaplığını çerçeve olarak ekleyin: **Katıştırılmış ikili dosya ekle** > **Başka ekle...** > Giriş dizininize gidin ve `MicrosoftCognitiveServicesSpeech.framework` dosyasını seçin. Bu ayrıca SDK kitaplığını otomatik olarak **Bağlı Çerçeve ve Kitaplıklar**’a ekler.
        ![Eklenen Çerçeve](media/sdk/qs-objectivec-framework.png)
        1. **Derleme Ayarları** sekmesine gidin ve **Tümü** ayarını etkinleştirin.
        1. `$(SRCROOT)/..` dizinini **Arama Yolları** başlığı altında *Çerçeve Arama Yolları*’na ekleyin.
        ![Çerçeve Arama Yolu ayarı](media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Kullanıcı arabirimini ayarlama

Örnek uygulama çok basit bir kullanıcı arabirimine sahip olacaktır: Dosyadan veya mikrofon girişinden konuşma tanımayı başlatmak için iki düğme ve sonucu görüntülemek için bir metin etiketi.
Kullanıcı arabirimi projenin `Main.storyboard` bölümünde ayarlanır.
Proje ağacında `Main.storyboard` girdisine sağ tıklayıp **Farklı Aç...** > **Kaynak Kod** seçeneklerini belirleyerek görsel taslağın XML görünümünü açın.
Otomatik oluşturulan XML’i şununla değiştirin:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Bağlantıya sağ tıklayıp **Hedefi farklı kaydet...** seçeneğini belirleyerek [örnek wav dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) indirin. Wav dosyasını bir Bulucu penceresinden Proje görünümünün kök düzeyine sürükleyerek projeye bir kaynak olarak ekleyin.
Aşağıdaki iletişim kutusunda ayarları değiştirmeden **Son**’a tıklayın.
1. Aşağıdaki şekilde otomatik oluşturulan `ViewController.m` dosyasının içeriğini değiştirin:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.
1. Mikrofon erişimi için isteği ekleyin. Proje ağacında `Info.plist` girdisine sağ tıklayın ve **Farklı Aç...** > **Kaynak Kodu** seçeneğini belirleyin. Aşağıdaki satırları `<dict>` bölümüne ekleyin ve ardından dosyayı kaydedin.
    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="building-and-running-the-sample"></a>Örneği Derleme ve Çalıştırma

1. Hata ayıklama çıkışını görünür hale getirin (**Görünüm** > **Hata Ayıklama Alanı** > **Konsolu Etkinleştir**).
1. **Ürün** -> **Hedef** menüsündeki listede bir iOS simülatorünü veya geliştirme cihazınıza bağlı bir iOS cihazını uygulamanız için hedef olarak seçin.
1. Menüden **Ürün** -> **Çalıştır** seçeneklerini belirleyerek veya **Oynat** düğmesine tıklayarak iOS simülatöründe örnek kodu derleyin ve çalıştırın.
Konuşma SDK’sı şu anda yalnızca 64 bit iOS platformlarını desteklemektedir.
1. Uygulamada "Tanı (Dosya)" düğmesine tıkladığınızda, ekranın alt kısmında "What's the weather like?" ses dosyasının içeriklerini görmeniz gerekir.

 ![iOS Uygulaması Simülasyonu](media/sdk/qs-objectivec-simulated-app.png)

1. Uygulamada “Tanı (Mikrofon)” düğmesine tıklayıp birkaç sözcük söyledikten sonra, konuştuğunuz metni ekranın alt bölümünde görmeniz gerekir.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
`quickstart/objectivec-ios` klasöründe bu örneği arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneklerimizi alın](speech-sdk.md#get-the-samples)
