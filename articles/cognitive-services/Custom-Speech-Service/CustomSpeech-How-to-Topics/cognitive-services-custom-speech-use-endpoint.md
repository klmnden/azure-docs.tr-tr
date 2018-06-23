---
title: Azure üzerinde özel konuşma hizmetiyle bir özel konuşma uç noktası kullan | Microsoft Docs
description: Bilişsel hizmetler özel konuşma hizmetinde ile özel bir konuşma metin uç noktası kullanmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: d28065d7962ee660cafd4b3321abdd6a8f94abcb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351814"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Özel bir konuşma metin uç noktası kullan
Varsayılan Bilişsel hizmetler konuşma uç noktası için olabildiğince benzer şekilde bir Azure özel konuşma hizmeti konuşma metin uç noktasına istekleri gönderebilirsiniz. Bu uç noktalar konuşma API varsayılan Uç noktalara işlevsel olarak aynıdır. Bu nedenle, istemci kitaplığının veya konuşma API'si için REST API aracılığıyla kullanılabilir olan aynı ayrıca özel uç noktası için kullanılabilir bir işlevdir.

Bu hizmet kullanarak oluşturduğunuz uç noktaları farklı sayıda eş zamanlı istekleri işleyebilir. Birim aboneliğinizle ilişkili fiyatlandırma katmanına bağlıdır. Çok fazla istek alırsa, bir hata oluşur. Ücretsiz katmanı istekleri aylık bir sınıra sahiptir.

Gerçek zamanlı olarak aktarılan veri hizmeti varsayar. Daha hızlı gönderilirse, istek gerçek zamanlı ses süresi geçene kadar çalıştıran olarak kabul edilir.

> [!NOTE]
> Destekliyoruz [yeni web yuvalarını](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) henüz. Özel konuşma uç noktasıyla web yuvalarını kullanmayı planlıyorsanız, buradaki yönergeleri izleyin.
>
> Yeni [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) desteği yakında kullanıma sunulacaktır. HTTP üzerinden özel konuşma uç noktanızı arama yapmayı planlıyorsanız, buradaki yönergeleri izleyin.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Konuşma istemci kitaplığı kullanılarak İsteği Gönder

Konuşma istemci kitaplığını kullanarak özel uç noktanızı istekleri göndermek için tanıma istemcisini başlatın. İstemci SDK konuşarak gelen [NuGet](http://nuget.org/). Arama *konuşma tanıma*ve konuşma tanıma paketi Microsoft'tan platformunuz için seçin. Bazı örnek kod bulunabilir [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). İstemci konuşma SDK'sını Fabrika SAX **SpeechRecognitionServiceFactory**, aşağıdaki yöntemleri sunar:

  *   ```CreateDataClient(...)```: Bir veri tanıma istemcisi.
  *   ```CreateDataClientWithIntent(...)```: Veri tanıma istemci amacıyla.
  *   ```CreateMicrophoneClient(...)```: Bir mikrofon tanıma istemci.
  *   ```CreateMicrophoneClientWithIntent(...)```: Bir mikrofon tanıma istemci amacıyla.

Ayrıntılı belgeler için bkz: [Bing konuşma API](https://docs.microsoft.com/azure/cognitive-services/speech/home). Özel konuşma hizmet uç noktalarına aynı SDK'yı destekler.

Veri tanıma konuşma tanıma için uygun bir dosya veya diğer ses kaynağı gibi verilerden istemcidir. Mikrofon tanıma istemci Mikrofon konuşma tanıma için uygundur. Ya da istemci hedefi kullanımını yapılandırılmış hedefi sonuçlarından döndürebilir [dil anlama akıllı hizmeti](https://www.luis.ai/) (senaryonuz için HALUK uygulama oluşturduysanız HALUK).

Tüm dört istemci türleri iki şekilde oluşturulabilir. İlk yol standart Bilişsel hizmetler konuşma API'sini kullanır. İkinci yol özel konuşma hizmeti ile oluşturulan özel uç noktanız için karşılık gelen bir URL belirtmenize olanak tanır.

Örneğin, oluşturabileceğiniz bir **DataRecognitionClient** , gönderdiği istekleri için özel bir uç noktası aşağıdaki yöntemi kullanarak:

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

**Your_subscriptionId** ve **endpointURL** abonelik anahtarı ve web yuvaları URL'si başvurmak sırasıyla üzerinde **dağıtım bilgileri** sayfası.

**AuthenticationUri** kimlik doğrulama Hizmeti'nden belirteç almak için kullanılır. Bu URI aşağıdaki örnek kodda gösterildiği gibi ayrı olarak ayarlamanız gerekir.

Bu örnek kod SDK istemcisi kullanmayı gösterir:

```csharp
var dataClient = SpeechRecognitionServiceFactory.CreateDataClient(
  SpeechRecognitionMode.LongDictation,
  "en-us",
  "your_subscriptionId",
  "your_subscriptionId",
  "endpointURL");
// set the authorization Uri
dataClient.AuthenticationUri = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
```

> [!NOTE]
> Kullandığınızda **oluşturma** yöntemleri SDK nedeniyle, iki kez aşırı abonelik kimliği sağlanmalıdır **oluşturma** yöntemleri.
>

Özel konuşma hizmeti iki farklı URL'ler kısa ve uzun formlu tanıma için kullanır. Her ikisi de listelendiğini **dağıtımları** sayfası. Doğru uç nokta URL'sini kullanmak istediğiniz belirli form için kullanın.

Özel uç noktanızı çeşitli tanıma istemcileriyle çağırma hakkında daha fazla bilgi için bkz: [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop) sınıfı. Bu sayfada belgelerine akustik modeli uyarlama başvuruyor ancak özel konuşma hizmeti kullanılarak oluşturulan tüm uç noktaları için geçerlidir.

## <a name="send-requests-by-using-the-speech-protocol"></a>Konuşma protokolü kullanarak istekleri gönderme

Gösterilen uç noktaları [konuşma Protokolü](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) açık kaynak Web yuva konuşma protokolü için noktalarıdır.

Şu an için yalnızca resmi istemci uygulamasıdır [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). İsterseniz başlaması, sağlanan örnek kodu aşağıdaki değişiklikleri yapın ve örneği yeniden oluşturmak:

1. İçinde _src\sdk\speech.browser\SpeechConnectionFactory.ts_, ana bilgisayar adı "wss://speech.platform.bing.com" dağıtımınızı Ayrıntıları sayfasında gösterilen ana bilgisayar adıyla değiştirin. Tam URI burada ancak yalnızca yerleştirmeyin *wss* protokol düzeni ve ana bilgisayar adı. Örneğin:

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Ayarlama _recognitionMode_ parametresinde _samples\browser\Samples.html_ , gereksinimlerinize göre:
    * _RecognitionMode.Interactive_ desteklediği en fazla 15 saniye ister.
    * _RecognitionMode.Conversation_ ve _RecognitionMode.Dictation_ (her ikisi de özel konuşma hizmetinde eşdeğer) destek istekleri en fazla 10 dakika.

3. Örnek, kullanmadan önce "yapı gulp" kullanarak oluşturun.

> [!NOTE]
> Bu protokol için doğru URI kullandığınızdan emin olun. Gerekli düzenidir *wss* (değil *http* istemci protokolü olduğu gibi). 

Daha fazla bilgi için bkz: [Bing konuşma API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries) belgeleri.

## <a name="send-requests-by-using-http"></a>HTTP kullanarak İsteği Gönder

Bir HTTP post kullanarak özel uç noktasına istek gönderilirken Bilişsel hizmetler Bing konuşma API'sine HTTP isteğiyle göndermeye benzer. URL adresi özel dağıtımınızın yansıtacak şekilde değiştirin.

Bilişsel hizmetler konuşma uç noktası ve hizmeti ile oluşturulan özel uç noktaları için HTTP üzerinden gönderilen istekleri bazı kısıtlamalar vardır. HTTP isteği kısmi sonuçlar tanıma işlemi sırasında döndüremez. Ayrıca, istekleri süresini ses içerik için 10 saniye ve genel 14 saniye ile sınırlıdır.

Bir post isteği oluşturmak için Bilişsel hizmetler konuşma API için kullandığınız aynı süreci izleyin.

1. Abonelik kimliğinizi kullanarak bir erişim belirteci alın Bu belirteç tanıma uç noktasına erişmek için gereklidir. 10 dakika için yeniden.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      **Subscriptionıd** bu dağıtım için kullandığınız abonelik kimliği için ayarlamanız gerekir. Yanıt, bir sonraki istek için gereksinim duyduğunuz düz belirtecidir.

2. Ses uç POST kullanarak yeniden gönderin.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    **Belirteci** önceki çağrısı ile alınan erişim belirteci. **Https_endpoint** gösterilen özel konuşma metin uç noktanızı, tam olarak adresidir **dağıtım bilgileri** sayfası.

HTTP post parametreleri ve yanıt biçimi hakkında daha fazla bilgi için bkz: [Microsoft Bilişsel Hizmetleri Bing konuşma HTTP API](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Hizmet Kitaplığı kullanarak istekleri gönderme
Hizmet Kitaplığı olmak hizmetinizin etkinleştirir konuşulan dilinde metin gerçek zamanlı olarak dönüştürmek için Microsoft Speech transcription Bulutu kullanın, böylece istemci uygulamanızı ses gönderme ve alma kısmi tanıma sonuçlarını aynı anda ve zaman uyumsuz olarak yedekleyin. Hizmeti SDK'sının ayrıntı bulunabilir [burada](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary)

> [!NOTE]
> Uygulamasında yetkilendirme sağlayıcısı URI'sini değiştirmek zorunda hizmet kitaplığı kullanırken **IAuthorizationProvider** için https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken.

## <a name="next-steps"></a>Sonraki adımlar
* İle doğruluğunu artırmak için [özel akustik modeli](cognitive-services-custom-speech-create-acoustic-model.md).
* İle doğruluğunu artırmak bir [özel dil modeli](cognitive-services-custom-speech-create-language-model.md).
