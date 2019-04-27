---
title: C# hizmet kitaplığı kullanarak Microsoft konuşma tanıma API'si ile çalışmaya başlama | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Konuşma dilini metne dönüştürmek için Bing konuşma tanıma hizmeti Kitaplığı'nı kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 0f445d1fff48ee7a04c0b1c1d64c808f87d824b7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515220"
---
# <a name="quickstart-use-the-bing-speech-recognition-service-library-in-c35-for-net-windows"></a>Hızlı Başlangıç: Bing konuşma tanıma hizmeti kitaplık C'de kullanın&#35; .NET Windows için

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Hizmet Kitaplığı kendi bulut hizmetine sahip ve kendi hizmetinden konuşma hizmeti çağırmak amacıyla isteyen geliştiricilere yöneliktir. Cihaz bağlı uygulamalardan konuşma tanıma hizmeti çağırmak istiyorsanız, bu SDK'sı kullanmayın. (Diğer istemci kitaplıkları veya REST API'leri için kullanın.)

C# hizmet kitaplığı kullanmak için yükleyin [NuGet paketini Microsoft.Bing.Speech](https://www.nuget.org/packages/Microsoft.Bing.Speech/). Kitaplığı API Başvurusu için bkz. [Microsoft konuşma C# hizmet Kitaplığı](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

Aşağıdaki bölümlerde, yükleme, oluşturma ve C# hizmet kitaplığı kullanarak C# örnek uygulamayı çalıştırma açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Aşağıdaki örnek Windows 8 + ve .NET 4.5 + için geliştirilen kullanarak Framework [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).

### <a name="get-the-sample-application"></a>Örnek uygulaması edinir

Örnekten kopyalama [konuşma C# hizmet kitaplığı örneği](https://github.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary) depo.

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olur ve ücretsiz deneme aboneliği anahtarını alma

Konuşma tanıma API'si, Bilişsel hizmetler (daha önce Project Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alabilirsiniz [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma tanıma API'si belirledikten sonra seçin **API anahtarı alma** anahtarını almak için. Birincil ve ikincil anahtar döndürür. İki anahtarı kullanabilmeniz için her iki anahtarı aynı kotası bağlıdır.

> [!IMPORTANT]
> * Bir abonelik anahtarı edinirler. Konuşma istemci kitaplıkları kullanabilmeniz için önce olmalıdır bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
> * Abonelik anahtarınızı kullanın. Sağlanan C# hizmet kitaplığı örnek uygulaması, bir komut satırı parametreleri abonelik anahtarınızı sağlamanız gerekir. Daha fazla bilgi için [örnek uygulamayı çalıştırma](#step-3-run-the-sample-application).

## <a name="step-1-install-the-sample-application"></a>1. Adım: Örnek uygulamayı yüklemek

1. Visual Studio 2015'i başlatın ve **dosya** > **açık** > **proje/çözüm**.

2. SpeechClient.sln adlı Visual Studio 2015 çözümü (.sln) dosyasını açmak için çift tıklayın. Çözüm, Visual Studio'da açılır.

## <a name="step-2-build-the-sample-application"></a>2. Adım: Örnek uygulaması oluşturma

Ctrl + Shift + B tuşuna basın veya **derleme** Şerit menüsünde. Ardından **Çözümü Derle**.

## <a name="step-3-run-the-sample-application"></a>3. Adım: Örnek uygulamayı çalıştırın

1. Derleme tamamlandıktan sonra F5 tuşuna basın veya seçin **Başlat** örneği çalıştırmak için Şerit menüsünde.

2. Örnek, örneğin, SpeechClientSample\bin\Debug için çıkış dizinini açın. SHIFT + sağ tıklatın ve seçin **burada açık bir komut penceresi**.

3. Çalıştırma `SpeechClientSample.exe` aşağıdaki bağımsız değişkenleriyle:

   * Arg [0]: Giriş Ses WAV dosyası belirtin.
   * [1]. değişken: Ses yerel ayarları belirtin.
   * [2] bağımsız değişken: Tanıma modları belirtin: *Kısa* için `ShortPhrase` modu ve *uzun* için `LongDictation` modu.
   * [3] bağımsız değişken: Konuşma tanıma hizmeti erişmek için abonelik anahtarını belirtin.

## <a name="samples-explained"></a>Açıklanan örnekleri

### <a name="recognition-modes"></a>Tanıma modları

* `ShortPhrase` modu: Uzun bir utterance fazla 15 saniye. İstemci, veriler sunucuya gönderildiğinde gibi birden çok kısmı sonuç ve bir son en iyi sonucu alır.
* `LongDictation` modu: Uzun bir utterance en fazla 10 dakika. Veriler sunucuya gönderildiğinde gibi istemci, birden çok kısmı sonuç ve birden çok Nihai sonuç, sunucunun cümle duraklamaları burada gösterir temel alır.

### <a name="supported-audio-formats"></a>Ses biçimleri desteklenir

Konuşma tanıma API'si, aşağıdakileri kullanarak ses/WAV destekler:

* PCM tek kanal
* Siren
* SirenSR

### <a name="preferences"></a>Tercihler

Bir SpeechClient oluşturmak için ilk tercihleri nesnesi oluşturmanız gerekir. Konuşma hizmeti davranışını yapılandıran bir dizi parametreleri tercihleri nesnedir. Bunu, aşağıdaki alanlardan oluşur:

* `SpeechLanguage`: Konuşma tanıma Hizmeti'ne gönderilen ses yerel ayar.
* `ServiceUri`: Konuşma hizmeti çağırmak için kullanılan uç nokta.
* `AuthorizationProvider`: Konuşma hizmeti erişmek için kullanılan belirteçleri getirilecek IAuthorizationProvider uygulaması. Örnek bir Bilişsel hizmetler yetkilendirme sağlayıcısı sağlasa da, belirteç önbelleğe işlemek için kendi uygulama oluşturmanızı öneririz.
* `EnableAudioBuffering`: Gelişmiş bir seçenek. Bkz: [bağlantı yönetimi](#connection-management).

### <a name="speech-input"></a>Konuşma giriş

SpeechInput nesne iki alandan oluşur:

* **Ses**: SDK'sı ses çeker, tercih ettiğiniz bir akış uygulaması. Herhangi olabilir [stream](https://msdn.microsoft.com/library/system.io.stream(v=vs.110).aspx) okuma destekleyen.
   > [!NOTE]
   > Akış döndürdüğünde SDK akışın sonuna algılar **0** olarak okundu.

* **RequestMetadata**: Konuşma isteğiyle ilgili meta veriler. Daha fazla bilgi için [başvuru](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html).

### <a name="speech-request"></a>Konuşma isteği

SpeechClient ve SpeechInput nesneleri örneği sonra RecognizeAsync konuşma hizmeti için bir istekte bulunmak için kullanın.

```cs
    var task = speechClient.RecognizeAsync(speechInput);
```

İstek tamamlandıktan sonra RecognizeAsync tarafından döndürülen görev tamamlanır. Son RecognitionResult tanıma sonudur. Hizmet veya SDK'sını beklenmedik şekilde başarısız olursa, görev başarısız olabilir.

### <a name="speech-recognition-events"></a>Konuşma tanıma olayları

#### <a name="partial-results-event"></a>Kısmi sonuçlar olay

Konuşma hizmeti bile Konuşmayı bitirmeden kişilerin, yorumlarını tahmin her başlatıldığında bu olay adlı (kullanırsanız `MicrophoneRecognitionClient`) veya veri gönderen son (kullanırsanız `DataRecognitionClient`). Kullanarak olaya abone olabilirsiniz `SpeechClient.SubscribeToPartialResult()`. Veya genel olay aboneliği yöntemi kullanabileceğiniz `SpeechClient.SubscribeTo<RecognitionPartialResult>()`.

**Dönüş biçimi** | Açıklama |
------|------
**LexicalForm** | Bu form, ham, işlenmemiş konuşma tanıma sonuçları gerek duyan uygulamalar tarafından kullanım için idealdir.
**Görüntü metni** | Ters metin normalleştirme, büyük/küçük harf, noktalama işaretleri ve uygulanan küfür maskeleme ile tanınan tümceciği. Küfür yıldız işareti ile ilk karakterden sonra Örneğin, "d ***." maskelenir. Bu formu, konuşma tanıma sonuçları kullanıcıya görüntüleyen uygulamalar tarafından kullanım için idealdir.
**güven** | Güvenirlik düzeyini tanınan tümceciği ilgili ses için konuşma tanıma sunucu tarafından tanımlanan temsil eder.
**MediaTime** | Geçerli saati (zaman 100 nanosaniyelik birimlerindeki) ses akışı başlangıcını göre.
**MediaDuration** | Geçerli deyim süresi/uzunluğu ilgili ses kesimdeki (100 nanosaniyelik birimler zaman).

#### <a name="result-event"></a>Olay sonucu
Konuşma tamamladığınızda (içinde `ShortPhrase` modu), bu olay çağrılır. Sonuç için en iyi n seçenekleri sunulur. İçinde `LongDictation` modu, olay çağrılabilir birden çok kez tabanlı sunucunun cümle duraklamaları burada gösterir. Kullanarak olaya abone olabilirsiniz `SpeechClient.SubscribeToRecognitionResult()`. Veya genel olay aboneliği yöntemi kullanabileceğiniz `SpeechClient.SubscribeTo<RecognitionResult>()`.

**Dönüş biçimi** | Açıklama |
------|------|
**RecognitionStatus** | Tanıma üretme üzerinde durumu. Örneğin, başarılı tanıma sonucunda veya iptal etme bağlantısı, vb. sonucunda üretilmiştir.
**İfadeleri** | En iyi n tanıma güvenle tanınan ifadeler kümesi.

Tanıma sonuçları hakkında daha fazla bilgi için bkz. [çıkış biçimi](../Concepts.md#output-format).

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

İstek başına tek bir WebSocket Bağlantısı API kullanır. En iyi kullanıcı deneyimi için SDK'sı konuşma hizmetine yeniden bağlanın ve tanıma aldığı son RecognitionResult başlatmak çalışır. Örneğin, ses isteği iki dakikadan uzun olduğundan, bir dakikalık işaretinde bir RecognitionEvent SDK alınan ve beş saniye sonra bir ağ hatası oluştu, SDK'sı bir dakikalık işaretinden başlayan yeni bir bağlantı başlatır.

>[!NOTE]
>SDK'sı, akış aramayı desteklemiyor olabilir çünkü bir dakikalık işaretine arama değil. Bunun yerine, SDK, bir iç arabellek ses arabelleğe almak için kullanır ve arabellek RecognitionResult olayları alırken temizler tutar.

## <a name="buffering-behavior"></a>Arabelleğe alma davranışı

Varsayılan olarak, bir ağ kesme oluştuğunda, kurtarabilmeniz SDK ses arabelleğe alır. Ağ bağlantı kesme sırasında kaybedilen ses atmak ve bağlantıyı yeniden başlatmak için tercih olduğu bir senaryoda, ayarlayarak ses arabelleğe almayı devre dışı bırakmak en iyi `EnableAudioBuffering` tercihleri nesnesindeki `false`.

## <a name="related-topics"></a>İlgili konular

[Microsoft konuşma C# hizmet Kitaplığı Başvurusu](https://cdn.rawgit.com/Microsoft/Cognitive-Speech-STT-ServiceLibrary/master/docs/index.html)
