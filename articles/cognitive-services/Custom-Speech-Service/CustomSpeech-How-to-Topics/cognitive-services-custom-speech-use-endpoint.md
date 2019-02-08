---
title: Azure'da özel konuşma hizmeti ile özel konuşma tanıma uç noktası kullanma | Microsoft Docs
description: Bilişsel hizmetler'deki özel konuşma hizmeti ile özel bir konuşmayı metne uç noktasını kullanmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: nitinme
ms.openlocfilehash: 95d91b5bfea81b44aee2f6ae3be5ca2c8ad223ce
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55855956"
---
# <a name="use-a-custom-speech-to-text-endpoint"></a>Özel bir konuşmayı metne dönüştürme uç noktası kullanma

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Varsayılan Bilişsel hizmetler konuşma uç noktası için mümkün olduğunca benzer şekilde bir Azure özel konuşma hizmeti konuşma metin uç noktasına istekler gönderebilir. Bu uç noktaları konuşma tanıma API'si varsayılan uç noktalar için işlevsel olarak aynıdır. İstemci kitaplığı veya konuşma tanıma API'si için REST API aracılığıyla kullanılabilir olan aynı işlevleri bu nedenle, aynı zamanda özel uç noktanız için olarak kullanılabilir.

Bu hizmeti kullanarak oluşturduğunuz uç noktaları, farklı sayıda eş zamanlı istekleri işleyebilir. Birim aboneliğinizle ilişkili fiyatlandırma katmanına bağlıdır. Çok fazla istek alındı, bir hata meydana gelir. Ücretsiz katman, istekleri aylık sınırı vardır.

Gerçek zamanlı olarak aktarılan veri hizmeti varsayar. Daha hızlı gönderilir, gerçek zamanlı ses süresi bitene kadar çalıştırma isteği kabul edilir.

> [!NOTE]
> Destekliyoruz [yeni web yuvalarını](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) henüz. Özel konuşma tanıma uç noktanız ile web yuvalarını kullanmayı planlıyorsanız, buradaki yönergeleri izleyin.
>
> Yeni [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) desteği yakında. HTTP üzerinden özel konuşma tanıma uç noktanızı arayın planlıyorsanız, buradaki yönergeleri izleyin.
>

## <a name="send-requests-by-using-the-speech-client-library"></a>Konuşma istemcisi kitaplığını kullanarak istekleri gönderme

Konuşma istemcisi kitaplığını kullanarak özel uç noktanıza istek göndermek için tanıma İstemcisi'ni başlatın. İstemci SDK'sı konuşarak gelen [NuGet](http://nuget.org/). Arama *konuşma tanıma*, konuşma tanıma paket platformunuz için Microsoft'tan seçin. Bazı örnek kodlar bulunabilir [GitHub](https://github.com/Microsoft/Cognitive-Speech-STT-Windows). Konuşma istemci SDK'sı bir Üreteç sınıfını sağlar **SpeechRecognitionServiceFactory**, aşağıdaki yöntemleri sunar:

  *   ```CreateDataClient(...)```: Bir veri tanıma istemcisi.
  *   ```CreateDataClientWithIntent(...)```: Amacıyla veri tanıma istemcisi.
  *   ```CreateMicrophoneClient(...)```: Mikrofon tanıma istemcisi.
  *   ```CreateMicrophoneClientWithIntent(...)```: Mikrofon tanıma istemcisi amacıyla.

Ayrıntılı belgeler için bkz. [Bing konuşma API'si](https://docs.microsoft.com/azure/cognitive-services/speech/home). Özel konuşma Hizmeti uç noktaları aynı SDK'yı destekler.

Veri tanıma istemcisi için konuşma tanıma, verilerden bir dosya veya başka bir ses kaynağından uygundur. Mikrofon tanıma istemci mikrofondan konuşma tanıma için uygundur. Hedefi ya da istemcisinde kullanımını yapılandırılmış hedefi sonuçlar döndürebilir [Language Understanding Intelligent Service](https://www.luis.ai/) (LUIS), senaryonuz için bir LUIS uygulaması derlediyseniz durumunda.

Dört türde istemci iki şekilde oluşturulabilir. İlk yol, standart bir Bilişsel hizmetler konuşma API'sini kullanır. İkinci yol özel konuşma hizmeti ile oluşturulan özel uç noktanıza karşılık gelen URL'yi belirtmenize olanak verir.

Örneğin, oluşturabileceğiniz bir **DataRecognitionClient** gönderen istekler için özel bir uç noktası aşağıdaki yöntemi kullanarak:

```csharp
public static DataRecognitionClient CreateDataClient(SpeeechRecognitionMode speechRecognitionMode, string language, string primaryOrSecondaryKey, **string url**);
```

**Your_subscriptionId** ve **endpointURL** abonelik anahtarını ve web yuvaları URL'si bakın sırasıyla üzerinde **dağıtım bilgilerini** sayfası.

**AuthenticationUri** kimlik doğrulama hizmeti tarafından bir belirteç almak için kullanılır. Bu URI aşağıdaki örnek kodda gösterildiği gibi ayrı olarak ayarlamanız gerekir.

Bu örnek kod, istemci SDK'sı kullanma işlemini gösterir:

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
> Kullanırken **Oluştur** yöntemleri SDK nedeniyle, iki kez aşırı abonelik Kimliğini sağlamalıdır **Oluştur** yöntemleri.
>

Özel konuşma hizmeti, iki farklı URL'ler için form kısa ve uzun biçimli tanıma kullanır. Her ikisini de listelenen **dağıtımları** sayfası. Kullanmak istediğiniz belirli bir form için doğru uç nokta URL'sini kullanın.

Özel uç noktanıza çeşitli tanıma istemcilerle çağırma hakkında daha fazla bilgi için bkz. [SpeechRecognitionServiceFactory](https://www.microsoft.com/cognitive-services/Speech-api/documentation/GetStarted/GetStartedCSharpDesktop) sınıfı. Akustik model uyarlama için bu sayfadaki belgeleri başvuruyor, ancak özel konuşma hizmeti kullanılarak oluşturulan tüm uç noktalar için geçerlidir.

## <a name="send-requests-by-using-the-speech-protocol"></a>Konuşma protokolü kullanarak istekleri gönderme

Uç noktalar için gösterilen [konuşma Protokolü](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/websocketprotocol) açık kaynak Web yuvası konuşma protokolü için uç noktalardır.

Şu an için yalnızca resmi istemci uygulamasıdır [JavaScript](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). İsterseniz başlamak, sağlanan örnek kodu aşağıdaki değişiklikleri yapın ve örneği tekrar oluşturmak:

1. İçinde _src\sdk\speech.browser\SpeechConnectionFactory.ts_, ana bilgisayar adı "wss://speech.platform.bing.com" dağıtımınızın Ayrıntıları sayfasında gösterilen konak adıyla değiştirin. Tam URI burada ancak yalnızca eklemeyin *wss* protokol şeması ve ana bilgisayar adı. Örneğin:

    ```JavaScript
    private get Host(): string {
        return Storage.Local.GetOrAdd("Host", "wss://<your_key_goes_here>.api.cris.ai");
    }
    ```

2. Ayarlama _recognitionMode_ parametresinde _samples\browser\Samples.html_ gereksinimlerinize göre:
    * _RecognitionMode.Interactive_ istekleri 15 saniyeye kadar destekler.
    * _RecognitionMode.Conversation_ ve _RecognitionMode.Dictation_ (hem de özel konuşma hizmeti eşdeğerdir) destek istekleri en fazla 10 dakika.

3. Kullanmadan önce "gulp derleme" kullanarak örneği derleyin.

> [!NOTE]
> Bu protokol için doğru URI kullandığınızdan emin olun. Gerekli şema *wss* (değil *http* istemci protokolü olduğu gibi). 

Daha fazla bilgi için [Bing konuşma API'si](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedclientlibraries) belgeleri.

## <a name="send-requests-by-using-http"></a>HTTP istekleri göndermek

Bir HTTP post kullanılarak özel uç noktanıza istek gönderme, Bilişsel hizmetler Bing konuşma API'si için bir HTTP isteğiyle göndermeye benzer. URL'yi özel dağıtımınızın adresini yansıtacak şekilde değiştirin.

Bilişsel hizmetler konuşma uç noktası hem de bu hizmet ile oluşturulan özel uç noktaları için HTTP üzerinden gönderilen istekleri bazı kısıtlamalar vardır. HTTP isteği kısmi sonuçlar tanıma işlemi sırasında döndüremez. Ayrıca, istek süresini ses içeriği için 10 saniye ve genel 14 saniye ile sınırlıdır.

Bir post isteği oluşturmak için Bilişsel hizmetler konuşma tanıma API'si için kullandığınız aynı süreci izleyin.

1. Abonelik kimliğinizi kullanarak bir erişim belirteci alın Bu belirteci tanıma uç noktasına erişmek için gereklidir. Bu, 10 dakika için yeniden kullanılabilir.

    ```
    curl -X POST --header "Ocp-Apim-Subscription-Key:<subscriptionId>" --data "" "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    ```
      **Subscriptionıd** bu dağıtım için kullandığınız abonelik kimliği için ayarlanması gerekir. Yanıt, bir sonraki istek için gereksinim duyduğunuz düz belirtecidir.

2. Ses uç noktasına POST kullanarak yeniden gönderin.

    ```
    curl -X POST --data-binary @example.wav -H "Authorization: Bearer <token>" -H "Content-Type: application/octet-stream" "<https_endpoint>"
    ```

    **Belirteci** önceki çağrı ile alınan erişim belirteci. **Https_endpoint** gösterilen özel konuşma metin uç noktanızı, tam adresidir **dağıtım bilgilerini** sayfası.

HTTP post parametreleri ve yanıt biçimi hakkında daha fazla bilgi için bkz. [Microsoft Bilişsel hizmetler Bing konuşma HTTP API'si](https://www.microsoft.com/cognitive-services/speech-api/documentation/API-Reference-REST/BingVoiceRecognition#SampleImplementation).

## <a name="send-requests-by-using-the-service-library"></a>Hizmet Kitaplığı kullanarak istekleri gönderme
Olmak hizmetinizin hizmet kitaplığı sağlayan gerçek zamanlı metin konuşulan dili dönüştürmek için Microsoft Speech transkripsiyonu Bulutu kullanın, böylece istemci uygulamanızı ses gönderebilir ve alabilir, kısmi tanıma sonuçları aynı anda ve zaman uyumsuz olarak geri. Ayrıntılı hizmet SDK'sı bulunabilir [burada](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedcsharpservicelibrary)

> [!NOTE]
> Uygulamasında yetkilendirme sağlayıcısı URI'sini değiştirmek zorunda hizmet Kitaplığı kullanıldığında **IAuthorizationProvider** için https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken.

## <a name="next-steps"></a>Sonraki adımlar
* İle doğruluğunu artırmak, [özel akustik model](cognitive-services-custom-speech-create-acoustic-model.md).
* İle doğruluğunu artırmak bir [özel dil modeli](cognitive-services-custom-speech-create-language-model.md).
