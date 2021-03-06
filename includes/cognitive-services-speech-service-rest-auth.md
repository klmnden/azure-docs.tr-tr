---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 03/29/2019
ms.author: erhopf
ms.openlocfilehash: 5f06ca04b0b6ea48ebb49952df71cb02946777fa
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333395"
---
## <a name="authentication"></a>Kimlik Doğrulaması

Her isteğin yetkilendirme üst bilgisi gerektirir. Bu tablo, hangi üstbilgileri her hizmet için desteklenen gösterilmektedir:

| Desteklenen yetkilendirme üstbilgileri | Konuşmayı Metne Dönüştürme | Metin okuma |
|------------------------|----------------|----------------|
| Ocp-Apim-Subscription-Key | Evet | Hayır |
| Yetkilendirme: Taşıyıcı | Evet | Evet |

Kullanırken `Ocp-Apim-Subscription-Key` üst bilgi, yalnızca abonelik anahtarınızı girin isteniyor. Örneğin:

```
'Ocp-Apim-Subscription-Key': 'YOUR_SUBSCRIPTION_KEY'
```

Kullanırken `Authorization: Bearer` üst bilgi, işiniz için istekte bulunmak için gereken `issueToken` uç noktası. Bu istekte 10 dakika için geçerli olan bir erişim belirteci için abonelik anahtarınızı exchange. Sonraki birkaç bölümde bir belirteç almak ve bir belirteç öğreneceksiniz.

### <a name="how-to-get-an-access-token"></a>Bir erişim belirteci alma

Erişim belirteci almak için bir isteğin yapılacağı gerekecektir `issueToken` uç noktayı kullanarak `Ocp-Apim-Subscription-Key` ve abonelik anahtarınız.

Bu bölgeler ve uç noktaları desteklenir:

[!INCLUDE [](./cognitive-services-speech-service-endpoints-token-service.md)]

Erişim belirteci isteği oluşturmak için aşağıdaki örnekleri kullanın.

#### <a name="http-sample"></a>HTTP örnek

Bu örnek, bir belirteç almak için basit bir HTTP isteği içindir. Değiştirin `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınız ile. Aboneliğiniz Batı ABD bölgesinde değil, yerini `Host` üst bilgi, bölgenin ana bilgisayar adına sahip.

```http
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

Yanıt gövdesi JSON Web Token (JWT) biçimlerindeki erişim belirteci içerir.

#### <a name="powershell-sample"></a>PowerShell örneği

Bu örnek, bir erişim belirteci almak için basit bir PowerShell Betiği verilmiştir. Değiştirin `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınız ile. Doğru uç noktaya aboneliğinizi eşleşen bölgeyi kullandığınızdan emin olun. Bu örnek, şu anda Batı ABD için ayarlanmış.

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

#### <a name="curl-sample"></a>cURL örnek

cURL Linux (ve Linux için Windows alt sistemi) kullanılabilir komut satırı aracıdır. Bu cURL komutu bir erişim belirteci almak nasıl gösterir. Değiştirin `YOUR_SUBSCRIPTION_KEY` konuşma hizmeti abonelik anahtarınız ile. Doğru uç noktaya aboneliğinizi eşleşen bölgeyi kullandığınızdan emin olun. Bu örnek, şu anda Batı ABD için ayarlanmış.

```cli
curl -v -X POST
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
 -H "Content-type: application/x-www-form-urlencoded" \
 -H "Content-Length: 0" \
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

#### <a name="c-sample"></a>C# örneği

Bu C# sınıfı bir erişim belirteci almak nasıl gösterir. Konuşma hizmeti abonelik anahtarınızı sınıfı başlattığınızda geçirin. Aboneliğiniz Batı ABD bölgesinde değilse değiştirin `FetchTokenUri` aboneliğiniz için bölge eşleştirilecek.

```cs
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

#### <a name="python-sample"></a>Python örneği

```python
# Request module must be installed.
# Run pip install requests if necessary.
import requests

subscription_key = 'REPLACE_WITH_YOUR_KEY'

def get_token(subscription_key):
    fetch_token_url = 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken'
    headers = {
        'Ocp-Apim-Subscription-Key': subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    access_token = str(response.text)
    print(access_token)
```

### <a name="how-to-use-an-access-token"></a>Bir erişim belirteci kullanma

Hizmet olarak erişim belirtecinin gönderilmesi gereken `Authorization: Bearer <TOKEN>` başlığı. Her bir erişim belirteci 10 dakika için geçerlidir. Dilediğiniz zaman yeni bir belirteç elde edebilirsiniz, ancak ağ trafiğini ve gecikme süresini en aza indirmek için aynı belirteci dokuz dakikalığına kullanmanızı öneririz.

Metin okuma REST API için örnek bir HTTP isteği şu şekildedir:

```http
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.stt.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

// Message body here...
```
