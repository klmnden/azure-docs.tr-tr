---
title: REST kullanarak Bing konuşma tanıma API'sini kullanmaya başlama | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Konuşmayı metne dönüştürmek için Microsoft Bilişsel hizmetler konuşma tanıma API'si erişmek için REST kullanma.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ead4026ecec4878c69bc21a9ebc989eaf3d69a13
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515146"
---
# <a name="quickstart-use-the-bing-speech-recognition-rest-api"></a>Hızlı Başlangıç: Bing konuşma tanıma REST API'si kullanma

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bulut tabanlı Bing konuşma hizmeti sayesinde konuşma kayıtlarını metne dönüştürmesine için REST API kullanarak uygulamalar geliştirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Konuşma tanıma API'si için abone olur ve ücretsiz deneme aboneliği anahtarını alma

Konuşma tanıma API'si, Bilişsel hizmetler (daha önce Project Oxford) bir parçasıdır. Ücretsiz deneme aboneliği anahtarları alabilirsiniz [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma tanıma API'si belirledikten sonra seçin **API anahtarı alma** anahtarını almak için. Birincil ve ikincil anahtar döndürür. İki anahtarı kullanabilmeniz için her iki anahtarı aynı kotası bağlıdır.

> [!IMPORTANT]
>* Bir abonelik anahtarı edinirler. REST API erişebilmeniz için önce olmalıdır bir [abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/).
>
>* Abonelik anahtarınızı kullanın. Aşağıdaki REST örneklerinde YOUR_SUBSCRIPTION_KEY kendi abonelik anahtarı ile değiştirin.
>
>* Başvurmak [kimlik doğrulaması](../how-to/how-to-authentication.md) sayfasını nasıl bir abonelik anahtarı edinirler.

### <a name="prerecorded-audio-file"></a>Önceden kaydedilmiş bir ses dosyası

Bu örnekte, REST API'SİNİN nasıl kullanılacağı göstermek için kayıtlı bir ses dosyası kullanın. Kısa ifade belirten kendiniz bir ses dosyasını kaydedin. Örneğin, "Bugün gibi hava durumu nedir?" düşünelim. veya "izlemek için komik filmler bulun." Konuşma tanıma API'si, dış mikrofon giriş da destekler.

> [!NOTE]
> Örnek ilgili ses bir WAV dosyası olarak kaydedilir gerektirir **PCM tek kanal (tekli), 16 KHz**.

## <a name="build-a-recognition-request-and-send-it-to-the-speech-recognition-service"></a>Derleme tanıma isteği ve konuşma tanıma Hizmeti'ne gönderin

Bir sonraki adımda konuşma tanıma, konuşma HTTP uç noktalarına uygun istek üstbilgi ve gövde ile bir POST isteği göndermektir.

### <a name="service-uri"></a>Hizmet URI'si

URI tanımlanan konuşma tanıma hizmeti temel [tanıma modları](../concepts.md#recognition-modes) ve [tanıma diller](../concepts.md#recognition-languages):

```HTTP
https://speech.platform.bing.com/speech/recognition/<RECOGNITION_MODE>/cognitiveservices/v1?language=<LANGUAGE_TAG>&format=<OUTPUT_FORMAT>
```

`<RECOGNITION_MODE>` tanıma modunu belirtir ve aşağıdaki değerlerden biri olmalıdır: `interactive`, `conversation`, veya `dictation`. Bu bir URI gerekli kaynak yoludur. Daha fazla bilgi için [tanıma modları](../concepts.md#recognition-modes).

`<LANGUAGE_TAG>` Sorgu dizesinde gerekli bir parametredir. Ses dönüştürme için hedef dil tanımlar: Örneğin, `en-US` İngilizce (Amerika Birleşik Devletleri). Daha fazla bilgi için [tanıma diller](../concepts.md#recognition-languages).

`<OUTPUT_FORMAT>` Sorgu dizesinde isteğe bağlı bir parametredir. Kendi izin verilen değerler `simple` ve `detailed`. Varsayılan olarak, hizmet sonuçları döndürür. `simple` biçimi. Daha fazla bilgi için [çıkış biçimi](../concepts.md#output-format).

Hizmetin URI bazı örnekler aşağıdaki tabloda listelenmiştir.

| Tanıma modu  | Dil | Çıkış biçimi | Hizmet URI'si |
|---|---|---|---|
| `interactive` | pt-BR | Varsayılan | https:\//speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| `conversation` | en-US | Ayrıntılı | https:\//speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US & biçimi = ayrıntılı |
| `dictation` | fr-FR | Basit | https:\//speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR & biçimi basit = |

> [!NOTE]
> Yalnızca uygulamanızın konuşma tanıma hizmeti çağırmak için REST API'leri kullanıyorsa, hizmet URI'si gereklidir. Aşağıdakilerden birini kullanırsanız [istemci kitaplıkları](GetStartedClientLibraries.md), genellikle bilmek hangi URI kullanılır gerekmez. İstemci kitaplıkları, yalnızca belirli bir istemci kitaplığı için geçerli olan farklı bir URI'leri, hizmet kullanıyor olabilir. Daha fazla bilgi için seçtiğiniz istemci kitaplığı bakın.

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki alanlar, istek üstbilgisinde ayarlamanız gerekir:

- `Ocp-Apim-Subscription-Key`: Hizmete çağrı her zaman abonelik anahtarınızı geçmelidir `Ocp-Apim-Subscription-Key` başlığı. Konuşma hizmeti de yetkilendirmeyi destekler Abonelik anahtarları yerine belirteçler. Daha fazla bilgi için bkz. [Kimlik doğrulaması](../How-to/how-to-authentication.md).
- `Content-type`: `Content-type` Alan biçimini ve ses akışı codec bileşenini açıklar. Şu anda yalnızca WAV dosyası ve PCM Mono 16000 kodlama desteklenir. Bu biçim içerik türü değeri `audio/wav; codec=audio/pcm; samplerate=16000`.

`Transfer-Encoding` alanı isteğe bağlıdır. Bu alan ayarlamanız `chunked`, ses küçük öbeklere kesme. Daha fazla bilgi için [öbekli aktarım](../How-to/how-to-chunked-transfer.md).

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

Aşağıdaki örnek, konuşma REST Uç noktalara bir konuşma tanıma isteği göndermek nasıl gösterir. Kullandığı `interactive` tanıma modu.

> [!NOTE]
> Değiştirin `YOUR_AUDIO_FILE` ile önceden kaydedilmiş ses dosyanızın yolu. Değiştirin `YOUR_SUBSCRIPTION_KEY` kendi abonelik anahtarınızla.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

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

Bu örnek, Linux üzerinde bash ile curl kullanır. Platformunuzda bulunan kullanılabilir durumda değilse, curl yüklemeniz gerekebilir. Örnek, Windows, Git Bash, zsh ve diğer Kabukları Cygwin üzerinde de çalışır.

> [!NOTE]
> Tutun `@` değiştirilirken ses dosya adından önce `YOUR_AUDIO_FILE` , önceden kaydedilmiş bir ses dosyası için yol olarak belirtir değerini `--data-binary` veri yerine bir dosya adı.

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

İstek işlendikten sonra konuşma tanıma hizmeti sonuçları bir yanıt olarak JSON biçiminde döndürür.

> [!NOTE]
> Önceki kod bir hata verirse bakın [sorun giderme](../troubleshooting.md) olası nedenini bulmak için.

Aşağıdaki kod parçacığı yanıt akıştan nasıl edinebilirsiniz örneği gösterilmektedir.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell
# show the response in JSON format
ConvertTo-Json $RecoResponse
```

# <a name="curltabcurl"></a>[Curl](#tab/curl)

Bu örnekte, curl doğrudan yanıt iletisini dize döndürür. JSON biçiminde göstermek istiyorsanız, ek araçlar, örneğin, jq kullanabilirsiniz.

```
curl -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE | jq
```

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

Aşağıdaki örnek bir JSON yanıtı verilmiştir:

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

REST API, bazı sınırlamalara sahiptir:

- Bu, yalnızca 15 saniye kadar ses akışı destekler.
- Bunu Ara sonuçlar sırasında tanıma desteklemiyor. Kullanıcılar yalnızca son tanıma işleminin sonucu alır.

Bu kısıtlamaları kaldırmak için konuşma özelliklerinden yararlanın [istemci kitaplıkları](GetStartedClientLibraries.md). Veya doğrudan çalışabileceğiniz [konuşma WebSocket Protokolü](../API-Reference-REST/websocketprotocol.md).

## <a name="whats-next"></a>Sırada ne var?

- C#, Java, vb. REST API kullanma hakkında bilgi için bkz: [örnek uygulamalar](../samples.md).
- Bulun ve hataları düzeltmek için bkz: [sorun giderme](../troubleshooting.md).
- Daha gelişmiş özellikleri kullanmak için nasıl konuşma kullanarak başlamak bkz. [istemci kitaplıkları](GetStartedClientLibraries.md).

### <a name="license"></a>Lisans

Tüm Bilişsel Hizmetleri SDK'lar ve örnekler MIT lisansı ile birlikte lisanslanır. Daha fazla bilgi için [lisans](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
