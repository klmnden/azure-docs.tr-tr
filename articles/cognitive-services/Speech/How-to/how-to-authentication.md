---
title: Bing konuşma kimlik doğrulaması | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Bing konuşma API'si kullanılacak kimlik doğrulaması isteme
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 11d6256fb63452b849a80abab181876d14b3b6a6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515051"
---
# <a name="authenticate-to-the-speech-api"></a>Konuşma tanıma API'si için kimlik doğrulaması

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bing konuşma kullanarak kimlik doğrulamasını destekler:

- Abonelik anahtarı.
- Bir yetkilendirme belirteci.

## <a name="use-a-subscription-key"></a>Bir abonelik anahtarı kullanma

Konuşma hizmeti kullanmak için Bilişsel hizmetler (daha önce Project Oxford) bir parçasıdır konuşma tanıma API'sine abone olması gerekir. Ücretsiz deneme aboneliği anahtarları alabilirsiniz [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma tanıma API'si belirledikten sonra seçin **API anahtarı alma** anahtarını almak için. Birincil ve ikincil anahtar döndürür. İki anahtarı kullanabilmeniz için her iki anahtarı aynı kotası bağlıdır.

Uzun süreli kullanım veya daha fazla kotaya için kaydolmaya bir [Azure hesabı](https://azure.microsoft.com/free/).

Abonelik anahtarı geçmesi gerek konuşma REST API'sini kullanmayı `Ocp-Apim-Subscription-Key` istek üstbilgisinde alan.

Ad| Biçimlendir| Açıklama
----|-------|------------
Ocp-Apim-Subscription-Key | ASCII | YOUR_SUBSCRIPTION_KEY

İstek üstbilgisi örneği verilmiştir:

```HTTP
POST https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: speech.platform.bing.com
Transfer-Encoding: chunked
Expect: 100-continue
```

> [!IMPORTANT]
> Kullanırsanız [istemci kitaplıkları](../GetStarted/GetStartedClientLibraries.md) uygulamanızda abonelik anahtarınız ile yetkilendirme belirteci alabilirsiniz aşağıdaki bölümde açıklanan şekilde doğrulayın. İstemci kitaplıkları, bir yetkilendirme belirteci almanız ve ardından belirteci kimlik doğrulaması için kullanmak için abonelik anahtarını kullanın.

## <a name="use-an-authorization-token"></a>Bir yetkilendirme belirteci kullanın

Alternatif olarak, bir yetkilendirme belirteci kimlik doğrulaması için kimlik doğrulama/yetkilendirme kanıtı olarak kullanabilirsiniz. Bu belirteci almak için önce bir abonelik anahtarı konuşma tanıma API'SİNDEN açıklandığı almanız gerekir [önceki bölümde yer](#use-a-subscription-key).

### <a name="get-an-authorization-token"></a>Bir yetkilendirme belirteci alma

Geçerli abonelik anahtarı oluşturduktan sonra Bilişsel Hizmetler'in belirteç hizmetine bir POST isteği gönderin. Yanıtta bir JSON Web Token (JWT olarak) yetkilendirme belirtecini alır.

> [!NOTE]
> Belirteç, 10 dakikalık bir zaman aşımı vardır. Belirteci yenilemek için aşağıdaki bölüme bakın.

Belirteç hizmet URI'si şuradan ulaşabilirsiniz:

```
https://api.cognitive.microsoft.com/sts/v1.0/issueToken
```

Aşağıdaki kod örneği, bir erişim belirteci almak gösterilmektedir. Değiştirin `YOUR_SUBSCRIPTION_KEY` kendi abonelik anahtarınızla:

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

# <a name="curltabcurl"></a>[Curl](#tab/curl)

Bu örnek, Linux üzerinde bash ile curl kullanır. Platformunuzda bulunan kullanılabilir durumda değilse, curl yüklemeniz gerekebilir. Örnek, Windows, Git Bash, zsh ve diğer Kabukları Cygwin üzerinde de çalışır.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
    /*
     * This class demonstrates how to get a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = "https://api.cognitive.microsoft.com/sts/v1.0";
        private string subscriptionKey;
        private string token;

        public Authentication(string subscriptionKey)
        {
            this.subscriptionKey = subscriptionKey;
            this.token = FetchTokenAsync(FetchTokenUri, subscriptionKey).Result;
        }

        public string GetAccessToken()
        {
            return this.token;
        }

        private async Task<string> FetchTokenAsync(string fetchUri, string subscriptionKey)
        {
            using (var client = new HttpClient())
            {
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                UriBuilder uriBuilder = new UriBuilder(fetchUri);
                uriBuilder.Path += "/issueToken";

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```

---

POST isteğinin örneği aşağıda verilmiştir:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
Connection: Keep-Alive
```

Bir yetkilendirme belirteci Hizmeti'nden belirteç alınamıyor, abonelik anahtarınızı hala geçerli olup olmadığını denetleyin. Ücretsiz bir deneme sürümü anahtarı kullanıyorsanız, Git [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfasında, ücretsiz deneme sürümü anahtarı uygulamak için kullandığınız hesabı kullanarak oturum açma ve abonelik anahtarının süresi doldu veya aşıyor denetlemek için "Oturum açma" tıklayın Kota.

### <a name="use-an-authorization-token-in-a-request"></a>Bir istekte bir yetkilendirme belirteci kullanın

Konuşma tanıma API'si çağrısı, her Yetkilendirme belirteci geçirmek gereken `Authorization` başlığı. `Authorization` Üst bilgi, bir JWT belirteç içermelidir.

Aşağıdaki örnek konuşma REST API çağırdığınızda bir yetkilendirme belirteci kullanmayı gösterir.

> [!NOTE]
> Değiştirin `YOUR_AUDIO_FILE` ile önceden kaydedilmiş ses dosyanızın yolu. Değiştirin `YOUR_ACCESS_TOKEN` önceki adımda aldığınız yetkilendirme belirteciyle [yetkilendirme belirtecini alma](#get-an-authorization-token).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authrization token returned by the token service.
$RecoRequestHeader = @{
  'Authorization' = 'Bearer '+ $OAuthToken;
  'Transfer-Encoding' = 'chunked'
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[Curl](#tab/curl)

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = @"speech.platform.bing.com";
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Authorization"] = "Bearer " + token;

// Send an audio file by 1024 byte chunks
using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
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

### <a name="renew-an-authorization-token"></a>Yetkilendirme belirteci yenileme

Yetkilendirme belirtecini, belirli bir zaman aralığına sonra (şu anda 10 dakika) süresi dolar. Süresi dolmadan önce yetkilendirme belirtecini yenilemek gerekir.

Aşağıdaki kod, C# yetkilendirme belirteci yenilemeye ilişkin örnek bir uygulamadır:

```cs
    /*
     * This class demonstrates how to get a valid O-auth token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = "https://api.cognitive.microsoft.com/sts/v1.0";
        private string subscriptionKey;
        private string token;
        private Timer accessTokenRenewer;

        //Access token expires every 10 minutes. Renew it every 9 minutes.
        private const int RefreshTokenDuration = 9;

        public Authentication(string subscriptionKey)
        {
            this.subscriptionKey = subscriptionKey;
            this.token = FetchToken(FetchTokenUri, subscriptionKey).Result;

            // renew the token on set duration.
            accessTokenRenewer = new Timer(new TimerCallback(OnTokenExpiredCallback),
                                           this,
                                           TimeSpan.FromMinutes(RefreshTokenDuration),
                                           TimeSpan.FromMilliseconds(-1));
        }

        public string GetAccessToken()
        {
            return this.token;
        }

        private void RenewAccessToken()
        {
            this.token = FetchToken(FetchTokenUri, this.subscriptionKey).Result;
            Console.WriteLine("Renewed token.");
        }

        private void OnTokenExpiredCallback(object stateInfo)
        {
            try
            {
                RenewAccessToken();
            }
            catch (Exception ex)
            {
                Console.WriteLine(string.Format("Failed renewing access token. Details: {0}", ex.Message));
            }
            finally
            {
                try
                {
                    accessTokenRenewer.Change(TimeSpan.FromMinutes(RefreshTokenDuration), TimeSpan.FromMilliseconds(-1));
                }
                catch (Exception ex)
                {
                    Console.WriteLine(string.Format("Failed to reschedule the timer to renew access token. Details: {0}", ex.Message));
                }
            }
        }

        private async Task<string> FetchToken(string fetchUri, string subscriptionKey)
        {
            using (var client = new HttpClient())
            {
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                UriBuilder uriBuilder = new UriBuilder(fetchUri);
                uriBuilder.Path += "/issueToken";

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```
