---
title: C# hizmet kitaplığı kullanarak Microsoft konuşma tanıma API'si ile çalışmaya başlama | Microsoft Docs
description: Konuşma dilini metne dönüştürmek için Microsoft konuşma tanıma hizmet kitaplığını kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/17/2017
ms.author: zhouwang
ms.openlocfilehash: 0320f41658a7ac4d6bf9e88ed998c853b665d485
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352277"
---
# <a name="get-started-with-the-speech-recognition-service-library-in-c35-for-net-windows"></a>Konuşma tanıma hizmet Kitaplığı'nda C kullanmaya başlama&#35; .NET Windows için

Kendi bulut hizmetine sahip ve kendi hizmetinden konuşma hizmetini çağırmak isteyen geliştiriciler için hizmet kitaplığıdır. Cihaz bağlı uygulamalardan konuşma tanıma hizmetini çağırmak istiyorsanız, bu SDK kullanmayın. (Diğer istemci kitaplıkları veya REST API'leri için kullanın.)

C# hizmet kitaplığı kullanmak için yükleme [NuGet paketi Microsoft.Bing.Speech](https://www.nuget.org/packages/Microsoft.Bing.Speech/). Bir kitaplık API Başvurusu'ı için bkz: [Microsoft konuşma C# hizmet Kitaplığı](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

Aşağıdaki bölümlerde, yükleme, yapı ve C# hizmet kitaplığı kullanarak C# örnek uygulamayı çalıştırın açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Aşağıdaki örnek Windows 8 + ve .NET 4.5 + için geliştirilen kullanarak Framework [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Örnek uygulamayı Al

Örnekten kopyalama [konuşma C# hizmet kitaplığı örneği](https://github.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary) deposu.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olmak ve ücretsiz deneme aboneliği anahtarı alma

Konuşma API Bilişsel hizmetler (daha önce proje Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alma [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma API seçtikten sonra Seç **alma API anahtarı** anahtarı alınamadı. Birincil ve ikincil anahtar döndürür. Her iki anahtar kullanabilmek için her iki anahtarı aynı kotasını bağlıdır.

> [!IMPORTANT]
> * Abonelik anahtarı edinin. Konuşma istemci kitaplıkları kullanmadan önce bilmeniz gereken bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
> * Abonelik anahtarınızı kullanın. Sağlanan C# hizmet kitaplığı örnek uygulaması ile bir komut satırı parametreleri abonelik anahtarınızı sağlamanız gerekir. Daha fazla bilgi için bkz: [örnek uygulamayı çalıştırın](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>1. adım: örnek uygulama yükleme

1. Visual Studio 2015'ı başlatın ve seçin **dosya** > **açık** > **proje/çözüm**.

2. SpeechClient.sln adlı Visual Studio 2015 çözümü (.sln) dosyasını açmak için çift tıklayın. Visual Studio'da Çözüm açılır.

## <a name="step-2-build-the-sample-application"></a>2. adım: örnek uygulama oluşturma

Ctrl + Shift + B tuşuna basın veya seçin **yapı** Şerit menüsünde. Ardından **yapı çözümü**.

## <a name="step-3-run-the-sample-application"></a>3. adım: örnek uygulamayı çalıştırma

1. Yapı tamamlandıktan sonra F5 tuşuna basın veya seçin **Başlat** örneği çalıştırmak için Şerit menüsünde.

2. Örneğin, SpeechClientSample\bin\Debug örnek çıktı dizinini açın. Shift + sağ tıklatın ve seçin **komut penceresini Burada Aç**.

3. Çalıştırma `SpeechClientSample.exe` aşağıdaki bağımsız değişkenlerle:

   * Arg [0]: bir giriş ses WAV dosyası belirtin.
   * Arg [1]: ses yerel ayarları belirtin.
   * Arg [2]: tanıma modları belirtin: *kısa* için `ShortPhrase` modu ve *uzun* için `LongDictation` modu.
   * Arg [3]: konuşma tanıma hizmete erişmek için abonelik anahtarı belirtin.

## <a name="samples-explained"></a>Açıklanan örnekleri

### <a name="recognition-modes"></a>Tanıma modları

* `ShortPhrase` mod: bir utterance en fazla 15 saniye uzun. Sunucuya gönderilen veri gibi istemcinin birden çok kısmi sonuçlar ve son bir en iyi sonucu alır.
* `LongDictation` mod: bir utterance 10 dakikadan uzun. Sunucuya gönderilen veri gibi istemci birden çok kısmi sonuçlar ve sunucuyu cümle duraklatır burada gösterir göre birden çok son sonuçlarını alır.

### <a name="supported-audio-formats"></a>Desteklenen ses biçimleri

Konuşma API aşağıdaki codec bileşenlerini kullanarak ses/WAV destekler:

* PCM tek kanal
* Siren
* SirenSR

### <a name="preferences"></a>Tercihler

Bir SpeechClient oluşturmak için öncelikle bir Tercihler nesnesi oluşturmanız gerekir. Tercihler nesne konuşma hizmetinin davranışını yapılandırır parametre kümesidir. Aşağıdaki alanları oluşur:

* `SpeechLanguage`: Konuşma hizmete gönderilen ses yerel ayarı.
* `ServiceUri`: Konuşma hizmetini çağırmak için kullanılan uç noktası.
* `AuthorizationProvider`: Bir IAuthorizationProvider konuşma hizmete erişebilmek için belirteçleri getirmek için kullanılan uygulama. Örnek bir Bilişsel hizmetler yetkilendirme sağlayıcısı sağlasa da, belirteç önbelleğe alma işlemek için kendi uygulamanızı oluşturmak önerilir.
* `EnableAudioBuffering`: Gelişmiş bir seçenek. Bkz: [bağlantı yönetimi](#connection-management).

### <a name="speech-input"></a>Konuşma giriş

SpeechInput nesnesi iki alandan oluşur:

* **Ses**: kendisinden SDK çeker ses tercih ettiğiniz bir akış uygulaması. Herhangi olabilir [akış](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx) okumayı destekler.
   > [!NOTE]
   > Akış döndürdüğünde SDK akışın sonuna algılar **0** ' nı okuma.

* **RequestMetadata**: konuşma isteğiyle ilgili meta verileri. Daha fazla bilgi için bkz: [başvuru](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

### <a name="speech-request"></a>Konuşma isteği

SpeechClient ve SpeechInput nesneleri örneği sonra RecognizeAsync konuşma hizmetinde bir isteği yapmak için kullanın.

```cs
    var task = speechClient.RecognizeAsync(speechInput);
```

İstek tamamlandıktan sonra RecognizeAsync tarafından döndürülen görev tamamlandığında. Son RecognitionResult tanıma sonudur. Hizmet veya SDK beklenmedik şekilde başarısız olursa görev başarısız olabilir.

### <a name="speech-recognition-events"></a>Konuşma tanıma olayları

#### <a name="partial-results-event"></a>Kısmi sonuçlar olay

Konuşma hizmet bile konuşarak bitirmeden ne size, belirten tahmin her zaman bu olayı adlı (kullanırsanız `MicrophoneRecognitionClient`) veya veri gönderme işlemini tamamladıktan (kullanıyorsanız `DataRecognitionClient`). Kullanarak olaya abone olabilirsiniz `SpeechClient.SubscribeToPartialResult()`. Veya genel olaylar Abonelik yöntemini kullanabilirsiniz `SpeechClient.SubscribeTo<RecognitionPartialResult>()`.

**Dönüş biçimi** | Açıklama |
------|------
**LexicalForm** | Bu form ham, işlenmemiş konuşma tanıma sonuçları ihtiyaç duyan uygulamalar tarafından kullanım için uygundur.
**Görüntü metni** | Ters metin normalleştirme, büyük/küçük harf, noktalama ve uygulanan uygunsuz metin maskeleme tanınan deyimle. Uygunsuz metin yıldız işaretiyle ilk karakter sonra Örneğin, "d ***." maskelenir Bu form bir kullanıcıya konuşma tanıma sonuçlarını görüntülemek uygulamaları tarafından kullanım için uygundur.
**Güven** | Güvenirlik düzeyini tanınan tümceciği konuşma tanıma sunucu tarafından tanımlanan için ilişkili ses temsil eder.
**MediaTime** | Geçerli saati (zaman 100 nanosaniyelik birimlerindeki) ses akışı başlangıcı göre.
**MediaDuration** | Geçerli tümcecik süresi'ses segmente (100 nanosaniyelik birim süre) göre/uzunluğu.

#### <a name="result-event"></a>Sonuç olay
Konuşmayı tamamladığınızda (içinde `ShortPhrase` modu), bu olay çağrılır. N en iyi seçenek için bir sonuç verdi. İçinde `LongDictation` modu, olay çağrılabilir birden çok kez tabanlı sunucuyu cümle duraklatır burada gösterir. Kullanarak olaya abone olabilirsiniz `SpeechClient.SubscribeToRecognitionResult()`. Veya genel olaylar Abonelik yöntemini kullanabilirsiniz `SpeechClient.SubscribeTo<RecognitionResult>()`.

**Dönüş biçimi** | Açıklama |
------|------|
**RecognitionStatus** | Tanıma nasıl üretilmiştir üzerinde durumu. Örneğin, başarılı tanıma sonucunda veya iptal bağlantı, vb. sonucu üretilmiştir.
**Deyimleri** | N en iyi tanınan tümcecikleri tanıma güvenle kümesi.

Tanıma sonuçları hakkında daha fazla bilgi için bkz: [çıktı biçimi](../Concepts.md#output-format).

### <a name="speech-recognition-response"></a>Konuşma tanıma yanıt

Konuşma yanıt örnek:
```
--- Partial result received by OnPartialResult  
---what  
--- Partial result received by OnPartialResult  
--what's  
--- Partial result received by OnPartialResult  
---what's the web  
--- Partial result received by OnPartialResult   
---what's the weather like  
---***** Phrase Recognition Status = [Success]   
***What's the weather like? (Confidence:High)  
What's the weather like? (Confidence:High) 
```

## <a name="connection-management"></a>Bağlantı Yönetimi

API istek başına tek bir WebSocket bağlantısı kullanır. En iyi kullanıcı deneyimi için SDK konuşma hizmetine yeniden bağlanın ve onu alınan son RecognitionResult tanıma başlatmak çalışır. Örneğin, iki dakikadan uzun ses isteği ise, bir dakikalık işaretten RecognitionEvent SDK alınan ve sonra beş saniyede bir ağ hatası oluştu, SDK'sı bir dakikalık işaretinden başlayan yeni bir bağlantı başlatır.

>[!NOTE]
>Akış aramayı desteklemiyor olabilir çünkü SDK'sı bir dakikalık işaretine arama değil. Bunun yerine, SDK ses arabelleğe almak için kullanır ve RecognitionResult olaylarını alır gibi arabellek temizler bir iç arabellek tutar.

## <a name="buffering-behavior"></a>Arabelleğe alma davranışı

Varsayılan olarak, bir ağ kesme oluştuğunda kurtarabilirsiniz böylece SDK ses arabelleğe alır. Ağ bağlantısının sırasında kaybedilen ses atmak ve bağlantıyı yeniden başlatmak için tercih olduğu bir senaryoda ayarlayarak ses arabelleğe almayı devre dışı bırakmak en iyisidir `EnableAudioBuffering` Tercihler nesnesindeki `false`.

## <a name="related-topics"></a>İlgili konular

[Microsoft konuşma C# hizmet Kitaplığı Başvurusu](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html)
