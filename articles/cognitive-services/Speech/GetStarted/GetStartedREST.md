---
title: REST kullanarak Microsoft konuşma tanıma API'si ile çalışmaya başlama | Microsoft Docs
description: Konuşma tanıma API'si konuşulan sesi metne dönüştürmek için Microsoft Bilişsel hizmetler erişmek için REST kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 53785cdfd75c23910802f2be20e6305817b3b097
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352270"
---
# <a name="get-started-with-speech-recognition-by-using-the-rest-api"></a>REST API kullanarak konuşma tanıma ile çalışmaya başlama

Bulut tabanlı konuşma hizmetiyle konuşulan sesi metne dönüştürmek için REST API kullanarak uygulamaları geliştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Konuşma API'sine abone olmak ve ücretsiz deneme aboneliği anahtarı alma

Konuşma API Bilişsel hizmetler (daha önce proje Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alma [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma API seçtikten sonra Seç **alma API anahtarı** anahtarı alınamadı. Birincil ve ikincil anahtar döndürür. Her iki anahtar kullanabilmek için her iki anahtarı aynı kotasını bağlıdır.

> [!IMPORTANT]
>* Abonelik anahtarı edinin. REST API erişebilmeniz için önce bilmeniz gereken bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
>* Abonelik anahtarınızı kullanın. Aşağıdaki REST örneklerinde YOUR_SUBSCRIPTION_KEY kendi abonelik anahtarı ile değiştirin. 
>
>* Başvurmak [kimlik doğrulaması](../how-to/how-to-authentication.md) sayfa için bir abonelik anahtarı alma.

### <a name="prerecorded-audio-file"></a>Önceden kaydedilmiş ses dosyası

Bu örnekte, REST API kullanma göstermek için kaydedilen ses dosyası kullanın. Kendiniz kısa tümcecik belirten bir ses dosyasını kaydedin. Örneğin, "Bugün gibi hava durumu nedir?" deyin veya "izlemek için komik filmler bulunamıyor." Konuşma tanıma API'si dış mikrofon girişi de destekler.

> [!NOTE]
> Örnek, ses bir WAV dosya olarak kaydedilir gerektirir **PCM tek kanal (tekli), 16 KHz**.

## <a name="build-a-recognition-request-and-send-it-to-the-speech-recognition-service"></a>Tanıma isteği oluşturun ve konuşma tanıma hizmete gönderin

Konuşma tanıma için sonraki adım uygun istek üstbilgisi ve gövde ile Konuşma HTTP uç noktaları için bir POST isteği göndermektir.

### <a name="service-uri"></a>Hizmet URI'si

URI tanımlanmış konuşma tanıma hizmeti temel [tanıma modları](../concepts.md#recognition-modes) ve [tanıma dillerini](../concepts.md#recognition-languages):

```HTTP
https://speech.platform.bing.com/speech/recognition/<RECOGNITION_MODE>/cognitiveservices/v1?language=<LANGUAGE_TAG>&format=<OUTPUT_FORMAT>
```

`<RECOGNITION_MODE>` tanıma modunu belirtir ve aşağıdaki değerlerden biri olmalıdır: `interactive`, `conversation`, veya `dictation`. Bu bir URI gereken kaynak yoludur. Daha fazla bilgi için bkz: [tanıma modları](../concepts.md#recognition-modes).

`<LANGUAGE_TAG>` Sorgu dizesinde gerekli bir parametredir. Ses dönüştürme için hedef dil tanımlar: Örneğin, `en-US` İngilizce (ABD) için. Daha fazla bilgi için bkz: [tanıma dillerini](../concepts.md#recognition-languages).

`<OUTPUT_FORMAT>` Sorgu dizesinde isteğe bağlı bir parametredir. İzin verilen değerler: `simple` ve `detailed`. Varsayılan olarak, hizmet sonuçlarında döndürür `simple` biçimi. Daha fazla bilgi için bkz: [çıktı biçimi](../concepts.md#output-format).

Bazı URI'ler hizmet örnekleri aşağıdaki tabloda listelenmiştir.

| Tanıma modu  | Dil | Çıktı biçimi | Hizmet URI'si |
|---|---|---|---|
| `interactive` | pt-BR | Varsayılan | https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| `conversation` | tr-TR | Ayrıntılı |https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed |
| `dictation` | fr-FR | Basit | https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR&format=simple |

> [!NOTE]
> Yalnızca uygulamanızı konuşma tanıma hizmetini çağırmak için REST API'leri kullanıyorsa, hizmet URI'si gereklidir. Birini kullanırsanız [istemci kitaplıkları](GetStartedClientLibraries.md), genellikle hangi URI kullanılır bilmeniz yok. İstemci kitaplıkları farklı hizmet URI, yalnızca belirli istemci kitaplığı için geçerli olan kullanabilir. Daha fazla bilgi için tercih ettiğiniz istemci kitaplığına bakın.

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki alanları istek üstbilgisinde ayarlamanız gerekir:

- `Ocp-Apim-Subscription-Key`: Hizmete çağrı her saat abonelik anahtarınızı geçmelidir `Ocp-Apim-Subscription-Key` üstbilgi. Konuşma hizmeti de yetkilendirmeyi destekler Abonelik anahtarları yerine belirteçleri. Daha fazla bilgi için bkz: [kimlik doğrulama](../How-to/how-to-authentication.md).
- `Content-type``Content-type` Alan biçimini ve ses akışı codec açıklar. Şu anda yalnızca WAV dosyası ve PCM Mono 16000 kodlama desteklenir. Bu biçim için Content-type değer `audio/wav; codec=audio/pcm; samplerate=16000`.

`Transfer-Encoding` Alan isteğe bağlı olur. Bu alan ayarlamak, `chunked`, küçük parçalara ses kesme. Daha fazla bilgi için bkz: [öbekli aktarım](../How-to/how-to-chunked-transfer.md).

Bir örnek istek üstbilgisi aşağıda verilmiştir:

```HTTP
POST https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: speech.platform.bing.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="send-a-request-to-the-service"></a>Hizmetine bir istek gönderin

Aşağıdaki örnek, konuşma REST Uç noktalara bir konuşma tanıma İsteği Gönder gösterilmektedir. Kullandığı `interactive` tanıma modu.

> [!NOTE]
> Değiştir `YOUR_AUDIO_FILE` ile önceden kaydedilmiş ses dosyanızın yolu. Değiştir `YOUR_SUBSCRIPTION_KEY` kendi abonelik anahtara sahip.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authorization token returned by the token service.
$RecoRequestHeader = @{
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY';
  'Transfer-Encoding' = 'chunked';
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[Curl](#tab/curl)

Örnek curl Linux bash ile kullanır. Platformunuz üzerinde kullanılabilir durumda değilse, curl yüklemeniz gerekebilir. Örneğin Windows, Git Bash, zsh ve diğer Kabukları Cygwin üzerinde de çalışır.

> [!NOTE]
> Tutmak `@` değiştirirken ses dosya adından önce `YOUR_AUDIO_FILE` önceden kaydedilmiş ses dosyanızın yolu ile belirtir, değeri `--data-binary` veri yerine bir dosya adı değil.

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";

// Send an audio file by 1024 byte chunks
using (FileStream fs = new FileStream(YOUR_AUDIO_FILE, FileMode.Open, FileAccess.Read))
{

    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```

---

## <a name="process-the-speech-recognition-response"></a>Konuşma tanıma yanıtı işlemi

İstek işlendikten sonra konuşma hizmeti sonuçları bir yanıt olarak JSON biçiminde döndürür.

> [!NOTE]
> Önceki kod hata verirse bakın [sorun giderme](../troubleshooting.md) olası nedenini bulmak için.

Aşağıdaki kod parçacığını nasıl yanıt akıştan okuyabilir örneği gösterir.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

```Powershell
# show the response in JSON format
ConvertTo-Json $RecoResponse
```

# <a name="curltabcurl"></a>[Curl](#tab/curl)

Bu örnekte, curl doğrudan yanıt iletisini dize döndürür. JSON biçiminde göstermek istiyorsanız, ek araçlar, örneğin, jq kullanabilirsiniz.

```
curl -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE | jq
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

```cs
/*
* Get the response from the service.
*/
Console.WriteLine("Response:");
using (WebResponse response = request.GetResponse())
{
    Console.WriteLine(((HttpWebResponse)response).StatusCode);

    using (StreamReader sr = new StreamReader(response.GetResponseStream()))
    {
        responseString = sr.ReadToEnd();
    }

    Console.WriteLine(responseString);
    Console.ReadLine();
}
```

---

Aşağıdaki örnek bir JSON yanıtı şöyledir:

```json
OK
{
  "RecognitionStatus": "Success",
  "Offset": 22500000,
  "Duration": 21000000,
  "NBest": [{
    "Confidence": 0.941552162,
    "Lexical": "find a funny movie to watch",
    "ITN": "find a funny movie to watch",
    "MaskedITN": "find a funny movie to watch",
    "Display": "Find a funny movie to watch."
  }]
}
```

## <a name="limitations"></a>Sınırlamalar

REST API bazı sınırlamalar vardır:

- Bu, yalnızca en çok 15 saniye ses akışını destekler.
- Bunu Ara sonuçların tanıma sırasında desteklemiyor. Kullanıcılar yalnızca son tanıma sonucu alır.

Bu sınırlamalara kaldırmak için konuşarak [istemci kitaplıkları](GetStartedClientLibraries.md). Veya doğrudan çalışabileceğiniz [konuşma WebSocket Protokolü](../API-Reference-REST/websocketprotocol.md).

## <a name="whats-next"></a>Sırada ne var?

- C#, Java, vb. REST API kullanılması hakkında bilgi için bkz: [örnek uygulamaları](../samples.md).
- Bulun ve hataları düzeltmek için bkz: [sorun giderme](../troubleshooting.md).
- Daha gelişmiş özellikleri kullanmak için konuşma kullanarak başlamak bkz. [istemci kitaplıkları](GetStartedClientLibraries.md).

### <a name="license"></a>Lisans

Tüm Bilişsel Services SDK'ları ve örnekleri MIT lisansı ile lisanslanır. Daha fazla bilgi için bkz: [lisans](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
