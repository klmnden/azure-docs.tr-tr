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
ms.openlocfilehash: 311d0cb7f208c0f720b8611510fb65efc65c12bc
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39112882"
---
# <a name="speech-service-rest-apis"></a>Konuşma hizmeti REST API'leri

Birleşik konuşma hizmeti REST API'ları tarafından sağlanan API'leri benzerdir [konuşma tanıma API'si](https://docs.microsoft.com/azure/cognitive-services/Speech) (eski adıyla Bing konuşma hizmeti da bilinir). Uç noktaları önceki konuşma hizmeti tarafından kullanılan uç noktalarını farklıdır.

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

İçinde Konuşmayı metne dönüştürme API'si, yalnızca kullanılan uç noktalarını önceki konuşma hizmeti konuşma tanıma API'si farklılık gösterir. Yeni uç nokta, aşağıdaki tabloda gösterilmiştir. Eşleşen abonelik bölgenizi kullanın.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

Konuşmayı metne dönüştürme API'si aksi benzer [REST API](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) önceki konuşma tanıma API'si için.

Konuşmayı metne dönüştürme REST API'si, yalnızca kısa konuşma destekler. İstekleri 10 saniyeye kadar ses içeren ve en son 14 saniyede toplam en fazla. REST API, yalnızca kısmi veya Ara sonuçlar Nihai sonuç döndürür.

> [!NOTE]
> Akustik model veya dil modeli ya da telaffuz özelleştirdiyseniz, özel uç noktanıza kullanın.

## <a name="text-to-speech"></a>Metin Okuma

Yeni metin okuma API'si, 24 KHz ses çıkış destekler. `X-Microsoft-OutputFormat` Üstbilgi artık aşağıdaki değerleri içerebilir.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

Konuşma hizmeti, artık iki 24-KHz kişilerden daha fazlasını sağlar:

Yerel ayar | Dil   | Cinsiyet | Hizmet adı eşleme
-------|------------|--------|------------
tr-TR  | İngilizce (ABD) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, Jessa24kRUS)" 
tr-TR  | İngilizce (ABD) | Erkek   | "Microsoft Server Konuşma metin konuşma ses (en-US, Guy24kRUS)"

Birleşik konuşma hizmeti metin okuma API için REST uç noktalar aşağıda verilmiştir. Eşleşen abonelik bölgenizi uç noktası kullanma.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

Başvurmanız bu farklılıkları göz önünde bulundurun [REST API belgelerini](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/bingvoiceoutput) önceki konuşma tanıma API'si için.

## <a name="authentication"></a>Kimlik Doğrulaması

Konuşma hizmetin REST API'sine bir istek göndermek için bir erişim belirteci gerektirir. Bölgesel bir konuşma hizmeti için abonelik anahtarınızı sağlayarak bir belirteç elde `issueToken` uç noktası, aşağıdaki tabloda gösterilen. Eşleşen abonelik bölgenizi uç noktası kullanma.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Her bir erişim belirteci 10 dakika için geçerlidir. Dilediğiniz zaman yeni bir belirteci edinebilirsiniz — dahil olmak üzere, isterseniz, her konuşma REST API istekten önce yeni. Ağ trafiğini ve gecikme süresini en aza indirmek için ancak aynı belirteci dokuz dakikalığına kullanmanızı öneririz.

Aşağıdaki bölümlerde, bir belirteç almak üzere nasıl ve bir istekte kullanma işlemini gösterir.

### <a name="getting-a-token-http"></a>Bir belirteç alınırken: HTTP

Aşağıda bir belirteç almak için örnek HTTP isteğidir. Değiştirin `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınız ile. Aboneliğiniz, Batı ABD bölgesinde değil ise, yerine `Host` üst bilgi, bölgenin ana bilgisayar adına sahip.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

Bu istek için yanıt gövdesinin Java Web Token (JWT) biçiminde bir erişim belirtecidir.

### <a name="getting-a-token-powershell"></a>Bir belirteç alınırken: PowerShell

Aşağıdaki Windows PowerShell Betiği, bir erişim belirteci almak nasıl gösterir. Değiştirin `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınız ile. Aboneliğiniz, Batı ABD bölgesinde değil ise, ana bilgisayar adı verilen URI'ın buna göre değişir.

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

### <a name="getting-a-token-curl"></a>Bir belirteç alınırken: cURL

cURL Linux (ve Linux için Windows alt sistemi) kullanılabilir komut satırı aracıdır. Aşağıdaki cURL komutu bir erişim belirteci almak nasıl gösterir. Değiştirin `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınız ile. Aboneliğiniz, Batı ABD bölgesinde değil ise, ana bilgisayar adı verilen URI'ın buna göre değişir.

> [!NOTE]
> Komutu okunabilirlik için birden çok satırda gösterilir, ancak tek bir satırda bir kabuk isteminde girdiğiniz.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="getting-a-token-c"></a>Bir belirteç alınırken: C#

C# sınıfı aşağıdaki bir erişim belirteci almak nasıl gösterir. Konuşma hizmeti abonelik anahtarınızı sınıfı örneği oluşturulurken geçirin. Aboneliğiniz, Batı ABD bölgesinde değil ise, ana bilgisayar adını değiştirmek `FetchTokenUri` uygun şekilde.

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

Bir REST API isteğinde bir belirteç kullanmak için bunu sağlamak `Authorization` sözcüğü aşağıdaki üst bilgi, `Bearer`. Örneğin, işte bir örnek metin içeren bir belirteç konuşma REST isteği. Gerçek belirtecinizin yerine `YOUR_ACCESS_TOKEN` ve doğru ana bilgisayar adı `Host` başlığı.

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

Yetkilendirme belirtecini, 10 dakika sonra süresi dolar. Süresi dolmadan önce yeni bir belirteç elde yetki yenileme — Örneğin, dokuz dakika sonra. 

Aşağıdaki C# kod daha önce gösterilen sınıfı mongodb'nin ' dir. `Authentication` Sınıfı kullanarak bir zamanlayıcıyı dokuz dakikada otomatik olarak yeni bir erişim belirteci alır. Bu yaklaşım, program çalışırken geçerli bir belirteç her zaman kullanılabilir olmasını sağlar.

> [!NOTE]
> Bir zamanlayıcı kullanmak yerine, ne zaman geçerli belirteç alınamadı, daha sonra yalnızca geçerli belirteç süresi dolmak üzere olduğunda yeni bir istek zaman damgası saklayabilirsiniz. Bu yaklaşım, yeni belirteçleri gereksiz yere isteyen önler ve seyrek konuşma isteklerde programları için daha uygun olabilir.

Önceki örneklerde olduğu gibi emin `FetchTokenUri` değerle eşleşen abonelik bölgenizi. Abonelik anahtarınızı sınıfı örneği oluşturulurken geçirin.

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

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [Akustik model özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modeli özelleştirme](how-to-customize-language-model.md)

