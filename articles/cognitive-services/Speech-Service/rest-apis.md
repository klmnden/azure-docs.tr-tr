---
title: Konuşma hizmeti REST API'leri | Microsoft Docs
description: Konuşma hizmeti için REST API referansı.
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: e80c69657dfb7cbab7d29c94d3dd3c56574de7b7
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321998"
---
# <a name="speech-service-rest-apis"></a>Konuşma hizmeti REST API'leri

Birleştirilmiş konuşma hizmeti REST API'leri tarafından sağlanan API'lerine benzer [konuşma API](https://docs.microsoft.com/azure/cognitive-services/Speech) (daha önce Bing konuşma hizmet olarak bilinen). Uç noktaları önceki konuşma hizmeti tarafından kullanılan uç noktalarını farklıdır.

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Konuşmaya metin API, yalnızca kullanılan uç noktalarını önceki konuşma hizmetinden konuşma tanıma API'si farklılık gösterir. Yeni uç nokta aşağıdaki tabloda gösterilmektedir. Abonelik bölgenizi eşleşen birini kullanın.

Bölge| Konuşma metin uç noktası
-|-
Batı ABD| `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1`
Doğu Asya| `https://eastasia.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1`
Kuzey Avrupa| `https://northeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1`

> [!NOTE]
> Bir http 401 hatası önlemek için URI gerekli dilde eklemeniz gerekir. Bu nedenle en-US için doğru URI olacaktır: https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US

Konuşma metin API aksi benzer [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) önceki konuşma API'si.

Konuşma metin REST API için yalnızca kısa utterances destekler. İstekleri en fazla 10 saniye ses içeren ve genel 14 saniyelik en son olabilir. REST API yalnızca son sonuçları, kısmi veya Ara sonuçlar döndürür.

> [!NOTE]
> Özel uç noktanızı akustik model veya dil modeli veya telaffuz özelleştirdiyseniz, bunun yerine kullanın.

## <a name="text-to-speech"></a>Metin Okuma

Yeni metin okuma API 24-KHz ses çıkış destekler. `X-Microsoft-OutputFormat` Üstbilgisi şimdi aşağıdaki değerleri içerebilir.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

Konuşma hizmeti şimdi iki 24-KHz sesleri sağlar:

Yerel ayar | Dil   | Cinsiyet | Hizmet adı eşleme
-------|------------|--------|------------
tr-TR  | İngilizce (ABD) | Kadın | "Microsoft Server Konuşma metin okuma ses (en-US, Jessa24kRUS)" 
tr-TR  | İngilizce (ABD) | Erkek   | "Microsoft Server Konuşma metin okuma ses (en-US, Guy24kRUS)"

Birleştirilmiş konuşma hizmet metin okuma API'si için REST uç noktalar şunlardır: Abonelik bölgenizi eşleşen uç noktası kullan.

Bölge| Metin okuma uç noktası
-|-
Batı ABD|    `https://westus.tts.speech.microsoft.com/cognitiveservices/v1`
Doğu Asya|  `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1`
Kuzey Avrupa|   `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1`

> [!NOTE]
> Bir özel sesli yazı tipi oluşturduysanız, bunun yerine özel uç noktanızı kullanın.

Başvuruda gibi bu farklılıklar aklınızda tutun [REST API belgeleri](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/bingvoiceoutput) önceki konuşma API'si.

## <a name="authentication"></a>Kimlik Doğrulaması

Konuşma hizmetin REST API için istek gönderilirken bir erişim belirteci gerektirir. Bölgesel bir konuşma hizmeti için abonelik anahtarınızı sağlayarak bir belirteç elde `issueToken` uç noktası, aşağıdaki tabloda gösterilen. Abonelik bölgenizi eşleşen uç noktası kullan.

Bölge| Belirteci Hizmeti uç noktası
-|-
Batı ABD|    `https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken`
Doğu Asya|  `https://eastasia.api.cognitive.microsoft.com/sts/v1.0/issueToken`
Kuzey Avrupa|   `https://northeurope.api.cognitive.microsoft.com/sts/v1.0/issueToken`

Her erişim belirteci 10 dakika için geçerlidir. Herhangi bir zamanda yeni bir belirteci edinebilirsiniz — dahil olmak üzere, isterseniz, önce her konuşma REST API isteği yalnızca. Ağ trafiği ve gecikme süresi en aza indirmek için ancak, aynı belirteci dokuz dakika kullanmanızı öneririz.

Aşağıdaki bölümlerde bir belirteç almak üzere nasıl ve nasıl bir istekte kullanılacağını gösterir.

### <a name="getting-a-token-http"></a>Bir belirteci alma: HTTP

Aşağıda bir belirteç almak için bir örnek HTTP isteğidir. Değiştir `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınızı ile. Aboneliğiniz Batı ABD bölgesinde değilse Değiştir `Host` , bölgenin ana bilgisayar adı ile üstbilgisi.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

Bu istek için yanıt gövdesini erişim belirteci Java Web Token (JWT) biçiminde olur.

### <a name="getting-a-token-powershell"></a>Bir belirteci alma: PowerShell

Aşağıdaki Windows PowerShell komut dosyası bir erişim belirteci almak nasıl gösterilmektedir. Değiştir `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınızı ile. Aboneliğiniz Batı ABD bölgesinde değilse, konak adı verilen URI uygun şekilde değiştirin.

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken
 -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

### <a name="getting-a-token-curl"></a>Bir belirteci alma: cURL

cURL Linux (ve Linux için Windows alt) kullanılabilir komut satırı aracıdır. Aşağıdaki cURL komutunu bir erişim belirteci almak nasıl gösterilmektedir. Değiştir `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınızı ile. Aboneliğiniz Batı ABD bölgesinde değilse, konak adı verilen URI uygun şekilde değiştirin.

> [!NOTE]
> Komutu okunabilirlik için birden çok satıra gösterilir, ancak bir kabuk isteminde tek bir satırda girilen.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="getting-a-token-c"></a>Bir belirteci alma: C#

C# sınıfı aşağıdaki bir erişim belirteci almak nasıl gösterilmektedir. Konuşma hizmeti abonelik anahtarınızı sınıfı başlatılırken geçirin. Aboneliğiniz Batı ABD bölgesinde değilse, ana bilgisayar adını değiştirmek `FetchTokenUri` uygun şekilde.

```cs
    /*
     * This class demonstrates how to get a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri =
            "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
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

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```

### <a name="using-a-token"></a>Bir belirteç kullanma

Bir REST API isteğinde bir belirteç kullanmak için içinde sağlamak `Authorization` word aşağıdaki üstbilgi `Bearer`. Örneğin, işte konuşma REST isteğine bir belirteci içeren bir örnek metin. Gerçek belirtecinizin yerine `YOUR_ACCESS_TOKEN` ve doğru konak `Host` üstbilgi.

```xml
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.tts.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Hello, world!
</voice></speak>
```

### <a name="renewing-authorization"></a>Yetkilendirme yenileniyor

Yetkilendirme belirtecin 10 dakika sonra süresi dolar. Süresi dolmadan önce yeni bir belirteç elde ederek, yetkilendirme yenileme — Örneğin, dokuz dakika sonra. 

Aşağıdaki C# kodu daha önce sunulan sınıfı bir içeri kayma yerini alır. `Authentication` Sınıfı bir süreölçer dokuz dakikada otomatik olarak yeni bir erişim belirteci alır. Bu yaklaşım, program çalışırken geçerli bir belirteci her zaman kullanılabilir olmasını sağlar.

> [!NOTE]
> Bir zamanlayıcı kullanmak yerine, ne zaman geçerli belirteç alındı, daha sonra yalnızca geçerli belirtecin süresi dolmak üzere olduğunda yeni bir istek zaman damgası saklayabilirsiniz. Bu yaklaşım, yeni belirteçleri gereksiz yere isteyen önler ve sık konuşma isteklerde programlar için daha uygun olabilir.

Önceki gibi emin olun `FetchTokenUri` değerle eşleşen abonelik bölgenizi. Abonelik anahtarınızı sınıfı başlatılırken geçirin.

```cs
    /*
     * This class demonstrates how to maintain a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = 
            "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
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

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
* [Bkz. Konuşma modelini özelleştirmek için](how-to-customize-speech-models.md)
