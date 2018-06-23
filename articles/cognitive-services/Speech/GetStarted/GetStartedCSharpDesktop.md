---
title: C# Masaüstü kitaplığını kullanarak Microsoft konuşma tanıma API'si ile çalışmaya başlama | Microsoft Docs
description: Konuşma ses metne dönüştürmek için Microsoft konuşma tanıma API'si kullanan temel Windows uygulamaları geliştirin.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/27/2017
ms.author: zhouwang
ms.openlocfilehash: e59b0e25401fb5182edd52f82985ffed9052286d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352246"
---
# <a name="get-started-with-the-speech-recognition-api-in-c35-for-net-on-windows"></a>C'de konuşma tanıma API'si ile çalışmaya başlama&#35; .NET Windows için

Bu sayfa konuşulan sesi metne dönüştürmek için konuşma tanıma API'si kullanan temel bir Windows uygulama geliştirmeyi gösterir. İstemci kitaplığı kullanılarak gerçek zamanlı akış için istemci uygulamanızı hizmete ses gönderdiğinde, aynı anda ve zaman uyumsuz kısmi tanıma sonuçları geri aldığı, yani sağlar.

Herhangi bir cihazda çalıştırma uygulamalardan konuşma hizmetini kullanmak isteyen geliştiriciler C# Masaüstü kitaplığını kullanabilirsiniz. Kitaplık kullanmak için yükleme [NuGet paketi Microsoft.ProjectOxford.SpeechRecognition x86](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x86/) 32-bit platformu için ve [NuGet paketi Microsoft.ProjectOxford.SpeechRecognition x64](https://www.nuget.org/packages/Microsoft.ProjectOxford.SpeechRecognition-x64/) için bir 64-bit platformu. İstemci Kitaplığı API Başvurusu bkz [Microsoft konuşma C# Masaüstü Kitaplığı](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html).

Aşağıdaki bölümlerde, yükleme, yapı ve C# Masaüstü kitaplığını kullanarak C# örnek uygulamayı çalıştırın açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Aşağıdaki örnek Windows 8 + ve .NET Framework 4.5 + için kullanarak geliştirilmiştir [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Örnek uygulamayı Al

Örnekten kopyalama [konuşma C# Masaüstü Kitaplığı örneği](https://github.com/microsoft/cognitive-speech-stt-windows) deposu.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olmak ve ücretsiz deneme aboneliği anahtarı alma

Konuşma API Bilişsel hizmetler (daha önce proje Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alma [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma API seçtikten sonra Seç **alma API anahtarı** anahtarı alınamadı. Birincil ve ikincil anahtar döndürür. Her iki anahtar kullanabilmek için her iki anahtarı aynı kotasını bağlıdır.

> [!IMPORTANT]
> * Abonelik anahtarı edinin. Konuşma istemci kitaplıkları kullanmadan önce bilmeniz gereken bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
> * Abonelik anahtarınızı kullanın. Örneği çalıştırdığınızda sağlanan C# Masaüstü örnek uygulama ile abonelik anahtarınızı metin kutusuna yapıştırın. Daha fazla bilgi için bkz: [örnek uygulamayı çalıştırın](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>1. adım: örnek uygulama yükleme

1. Visual Studio 2015'ı başlatın ve seçin **dosya** > **açık** > **proje/çözüm**.

2. İndirilen konuşma tanıma API'si dosyalarını kaydettiğiniz klasöre göz atın. Seçin **konuşma** > **Windows**ve örnek WP klasörü seçin.

3. SpeechToText WPF Samples.sln adlı Visual Studio 2015 çözümü (.sln) dosyasını açmak için çift tıklayın. Visual Studio'da Çözüm açılır.

## <a name="step-2-build-the-sample-application"></a>2. adım: örnek uygulama oluşturma

1. Kullanmak istiyorsanız, *amacıyla tanıma*, ilk kaydolmak gereken [dil anlama akıllı hizmet (HALUK)](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). Anahtar değerini ayarlamak için uç nokta URL'sini HALUK uygulamanızın kullanmak `LuisEndpointUrl` samples/SpeechRecognitionServiceExample klasöründeki app.config dosyasında. HALUK uygulama uç nokta URL'sini hakkında daha fazla bilgi için bkz: [uygulamanızı yayınlama](../../luis/luis-get-started-create-app.md#publish-your-app).

   > [!TIP]
   > Değiştir `&` HALUK uç nokta URL'si ile karakter `&amp;` URL XML Ayrıştırıcı tarafından doğru bir şekilde yorumlanır emin olmak için.

2. Ctrl + Shift + B tuşuna basın veya seçin **yapı** Şerit menüsünde. Ardından **yapı çözümü**.

## <a name="step-3-run-the-sample-application"></a>3. adım: örnek uygulamayı çalıştırma

1. Yapı tamamlandıktan sonra F5 tuşuna basın veya seçin **Başlat** örneği çalıştırmak için Şerit menüsünde.

2. Git **metin örnek proje Oxford konuşma** penceresi. Abonelik anahtarınızı Yapıştır **başlatmak için abonelik anahtarınızı buraya yapıştırın** gösterildiği gibi metin kutusu. Abonelik anahtarınızı PC veya dizüstü bilgisayarınız üzerinde kalıcı hale getirmek için seçin **anahtarı Kaydet**. Abonelik anahtarı sistemden silmek için seçin **Delete tuşuna** PC ya da dizüstü bilgisayara kaldırmak için.

   ![Konuşma tanıma anahtar Yapıştır açma](../Images/SpeechRecog_paste_key.PNG)

3. Altında **konuşma tanıma kaynak**, iki ana giriş kategoriye ayrılan altı konuşma kaynakları birini seçin:

   * Bilgisayarınızın mikrofon veya iliştirilmiş bir mikrofon konuşma yakalamak için kullanın.
   * Bir ses dosyasını yürütün.

   Her kategori üç tanıma modu vardır:

    * **ShortPhrase modu**: bir utterance en fazla 15 saniye uzun. Sunucuya gönderilen veri gibi istemci birden çok kısmi sonuçlar ve birden çok n en iyi seçenek bir nihai sonucu alır.
    * **LongDictation modu**: bir utterance kadar iki dakikadan uzun. Sunucuya gönderilen veri gibi istemci birden çok kısmi sonuçlar ve sunucuyu cümle duraklatır burada gösterir göre birden çok son sonuçlarını alır.
    * **Hedefi algılama**: sunucu giriş konuşma hakkında daha fazla yapılandırılmış bilgi döndürür. Hedefi algılamayı kullanmak için önce kullanarak bir modeli eğitmek gereken [HALUK](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/).

Bu örnek uygulama ile örnek ses dosyalarını kullanın. Bu örnek samples/SpeechRecognitionServiceExample klasörü altında ile indirilen deposundaki dosyaları bulur. Başka bir dosya seçtiğinizde seçilirse bu örnek ses dosyalarını otomatik olarak çalışacak **Shortphrase modu için wav dosyası kullan** veya **Longdictation modu için wav dosyası kullan** konuşmanızı giriş olarak. Şu anda yalnızca WAV ses biçimi destekleniyor.

![Konuşma metin arabirimi](../Images/HelloJones.PNG)

## <a name="samples-explained"></a>Açıklanan örnekleri

### <a name="recognition-events"></a>Tanıma olayları

* **Kısmi sonuçlar olayları**: bile konuşarak bitirmeden ne size, belirten konuşma hizmet tahmin her zaman bu olayı adlı (kullanırsanız `MicrophoneRecognitionClient`) veya veri gönderme işlemini tamamladıktan (kullanıyorsanız `DataRecognitionClient`).
* **Hata olayları**: hizmet bir hata algıladığında çağrılır.
* **Hedefi olayları**: adlı istemcilerde "WithIntent" (yalnızca modunda ShortPhrase) son tanıma sonra sonuç yapılandırılmış bir JSON hedefi ayrıştırılır.
* **Neden olayları**:
  * İçinde `ShortPhrase` modu, bu olay adı verilir ve Konuşmayı bitirdikten sonra n en iyi sonuçlar verir.
  * İçinde `LongDictation` modu, olay işleyicisi çağrılır birden çok kez hizmeti cümle duraklatır burada tanımlar tabanlı.
  * **Her n en iyi seçenek**, güvenirlik değeri ve birkaç farklı biçimlerde tanınan metni döndürülür. Daha fazla bilgi için bkz: [çıktı biçimi](../Concepts.md#output-format).

Olay işleyicileri zaten kod açıklamaları biçiminde kodda işaret.

## <a name="related-topics"></a>İlgili konular

* [Microsoft Speech Masaüstü Kitaplığı Başvurusu](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-Windows/master/docs/SpeechSDK/index.html)
* [Microsoft konuşma tanıma API'si Android üzerinde Java kullanmaya başlama](GetStartedJavaAndroid.md)
* [Microsoft konuşma tanıma API'si Objective C'de iOS kullanmaya başlama](Get-Started-ObjectiveC-iOS.md)
* [JavaScript Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedJSWebsockets.md)
* [REST aracılığıyla Microsoft konuşma tanıma API'si ile çalışmaya başlama](GetStartedREST.md)
