---
title: Microsoft konuşma hizmetin kimliğini | Microsoft Docs
description: Microsoft konuşma API kullanılacak kimlik doğrulama isteği
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: e36168cf3ff938af44f1028c2d26fd475d60b148
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352210"
---
# <a name="authenticate-to-the-speech-api"></a>Konuşma API'SİNDE kimlik doğrulaması

Konuşma hizmeti kullanarak kimlik doğrulamasını destekler:

- Bir abonelik anahtarı.
- Bir yetkilendirme belirteci.

## <a name="use-a-subscription-key"></a>Bir abonelik tuşunu kullanın

Konuşma hizmetini kullanmak için önce Bilişsel hizmetler (daha önce proje Oxford) parçası olan konuşma API abone olmalısınız. Ücretsiz deneme aboneliği anahtarları alma [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası. Konuşma API seçtikten sonra Seç **alma API anahtarı** anahtarı alınamadı. Birincil ve ikincil anahtar döndürür. Her iki anahtar kullanabilmek için her iki anahtarı aynı kotasını bağlıdır.

Uzun vadeli kullanın veya daha yüksek bir kota için kaydolun bir [Azure hesabı](https://azure.microsoft.com/free/).

Konuşma REST API kullanmak için abonelik anahtarında geçmesi gerektiğini `Ocp-Apim-Subscription-Key` istek üstbilgisi alanındaki.

Ad| Biçimlendir| Açıklama
----|-------|------------
Ocp Apim abonelik anahtarı | ASCII | YOUR_SUBSCRIPTION_KEY

İstek üstbilgilerinden biri bir örnek verilmiştir:

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
> Kullanırsanız [istemci kitaplıkları](../GetStarted/GetStartedClientLibraries.md) , uygulamanızda abonelik anahtarınızla yetkilendirme belirtecini alabilir aşağıdaki bölümde açıklandığı gibi doğrulayın. İstemci kitaplıkları abonelik anahtarının bir yetkilendirme belirtecini almak ve belirteç kimlik doğrulaması için kullanmak üzere kullanın.

## <a name="use-an-authorization-token"></a>Bir yetkilendirme belirteci kullanın

Alternatif olarak, bir yetki belirteci kimlik doğrulaması için kimlik doğrulama/yetkilendirme kanıtı olarak kullanabilirsiniz. Bu belirteç almak için önce bir abonelik anahtarı konuşma API'SİNDEN açıklandığı şekilde edinmeniz gerekir [bölüm önceki](#use-a-subscription-key).

### <a name="get-an-authorization-token"></a>Bir yetkilendirme belirteci alma

Geçerli bir abonelik anahtarı aldıktan sonra Bilişsel hizmetler belirteç hizmetine bir POST isteği gönderin. Yanıtta bir JSON Web Token (JWT olarak) yetkilendirme belirtecini alırsınız.

> [!NOTE]
> Belirtecin 10 dakikada bir sona erme tarihi vardır. Belirteç yenilemek için aşağıdaki bölümüne bakın.

Belirteç hizmet URI'si burada bulunur:

```
https://api.cognitive.microsoft.com/sts/v1.0/issueToken
```

Aşağıdaki kod örneği, bir erişim belirteci almak gösterilmiştir. Değiştir `YOUR_SUBSCRIPTION_KEY` kendi abonelik anahtarı ile:

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

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

Örnek curl Linux bash ile kullanır. Platformunuz üzerinde kullanılabilir durumda değilse, curl yüklemeniz gerekebilir. Örneğin Windows, Git Bash, zsh ve diğer Kabukları Cygwin üzerinde de çalışır.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

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

Bir örnek POST isteği verilmiştir:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
Connection: Keep-Alive
```

Bir yetkilendirme belirteci Hizmeti'nden belirteç alınamıyor, abonelik anahtarınızı hala geçerli olup olmadığını denetleyin. Ücretsiz bir deneme anahtarı kullanıyorsanız, Git [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfasında, "Oturum açma" ücretsiz deneme anahtarı uygulamak için kullanılan hesabı kullanarak oturum açma ve abonelik anahtarının süresi doldu veya aşıyor olup olmadığını denetlemek için Kota.

### <a name="use-an-authorization-token-in-a-request"></a>Bir istekte bir yetki belirteci kullanın

Konuşma API çağrısı, her Yetkilendirme belirteçte geçirmenize gerek `Authorization` üstbilgi. `Authorization` Üstbilgisi JWT erişim belirteci içermesi gerekir.

Aşağıdaki örnek, konuşma REST API çağrısı zaman bir yetki belirteci kullanmayı gösterir.

> [!NOTE]
> Değiştir `YOUR_AUDIO_FILE` ile önceden kaydedilmiş ses dosyanızın yolu. Değiştir `YOUR_ACCESS_TOKEN` önceki adımda aldığınız yetki belirteciyle [bir yetki belirteci alma](#get-an-authorization-token).

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/Powershell)

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

# <a name="ctabcsharp"></a>[C#](#tab/CSharp)

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

### <a name="renew-an-authorization-token"></a>Bir yetkilendirme belirtecini yenileyin

Yetkilendirme belirtecini belirli bir dönemdeki sonra (şu anda 10 dakika) süresi dolar. Süresi dolmadan önce yetkilendirme belirtecini yenilemeniz gerekir.

Aşağıdaki kod, C# nasıl yetkilendirme belirtecini yenileyin dair örnek bir uygulamadır:

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
