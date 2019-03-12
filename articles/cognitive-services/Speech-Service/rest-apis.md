---
title: REST API'ler - konuşma Hizmetleri konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Konuşmayı metne ve metin okuma REST API'lerini kullanmayı öğrenin. Bu makalede, sorgu seçenekleri, yetkilendirme seçenekleri hakkında bilgi edineceksiniz yapısı bir istek ve yanıt.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 83e613616c37272ed2fdd42288678a57c520b8fa
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57532465"
---
# <a name="speech-service-rest-apis"></a>Konuşma hizmeti REST API'leri

Alternatif olarak [Speech SDK'sı](speech-sdk.md), konuşma hizmeti, bir dizi REST API'si ile metin okuma Konuşmayı metne dönüştürme sağlar. Her erişilebilen bir uç noktaya bir bölge ile ilişkilidir. Uygulamanızı kullanmayı planladığınız uç nokta için bir abonelik anahtarı gerektirir.

REST API'lerini kullanarak önce anlayın:
* REST API kullanarak konuşma metin istekleri yalnızca kaydedilen ses 10 saniyelik içerebilir.
* Konuşmayı metne REST API, yalnızca son sonuçları döndürür. Kısmi sonuçlar sağlanmaz.
* Metin okuma REST API, bir yetkilendirme üst bilgisi gerektirir. Bu, hizmete erişmek için bir belirteç değişimi tamamlanması gerektiği anlamına gelir. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication).

## <a name="authentication"></a>Kimlik Doğrulaması

Ya da Konuşmayı metne veya metinden konuşmaya REST API'sine yapılan her isteğin yetkilendirme üst bilgisi gerektirir. Bu tablo, hangi üstbilgileri her hizmet için desteklenen gösterilmektedir:

| Desteklenen yetkilendirme üstbilgileri | Konuşmayı Metne Dönüştürme | Metin okuma |
|------------------------|----------------|----------------|
| Ocp-Apim-Subscription-Key | Evet | Hayır |
| Yetkilendirme: Taşıyıcı | Evet | Evet |

Kullanırken `Ocp-Apim-Subscription-Key` üst bilgi, yalnızca abonelik anahtarınızı girin isteniyor. Örneğin:

```
'Ocp-Apim-Subscription-Key': 'YOUR_SUBSCRIPTION_KEY'
```

Kullanırken `Authorization: Bearer` üst bilgi, işiniz için istekte bulunmak için gereken `issueToken` uç noktası. Bu istekte 10 dakika için geçerli olan bir erişim belirteci için abonelik anahtarınızı exchange. Sonraki birkaç bölümde bir belirteç almak, bir belirteç kullanın ve bir belirteç yenileme öğreneceksiniz.

### <a name="how-to-get-an-access-token"></a>Bir erişim belirteci alma

Erişim belirteci almak için bir isteğin yapılacağı gerekecektir `issueToken` uç noktayı kullanarak `Ocp-Apim-Subscription-Key` ve abonelik anahtarınız.

Bu bölgeler ve uç noktaları desteklenir:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

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

### <a name="how-to-use-an-access-token"></a>Bir erişim belirteci kullanma

Hizmet olarak erişim belirtecinin gönderilmesi gereken `Authorization: Bearer <TOKEN>` başlığı. Her bir erişim belirteci 10 dakika için geçerlidir. Dilediğiniz zaman yeni bir belirteç elde edebilirsiniz, ancak ağ trafiğini ve gecikme süresini en aza indirmek için aynı belirteci dokuz dakikalığına kullanmanızı öneririz.

Metin okuma REST API için örnek bir HTTP isteği şu şekildedir:

```http
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.tts.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

<speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Hello, world!
</voice></speak>
```

### <a name="how-to-renew-an-access-token-using-c"></a>Kullanarak bir erişim belirteci yenilemeC#

Bu C# kodudur, mongodb'nin daha önce gösterilen sınıfı. `Authentication` Sınıfı otomatik olarak alır yeni bir erişim belirteci dokuz dakikada bir zamanlayıcı kullanarak. Bu yaklaşım, program çalışırken geçerli bir belirteç her zaman kullanılabilir olmasını sağlar.

> [!NOTE]
> Bir zamanlayıcı kullanmak yerine, bir zaman damgası, son belirteç zaman edinilen depolayabilirsiniz. Yalnızca, süresi dolmak üzere olduğunda yeni bir talep edebilir. Bu yaklaşım yeni belirteçleri gereksiz yere isteyen önler ve seyrek konuşma isteklerde programları için daha uygun olabilir.

Önceki örneklerde olduğu gibi emin `FetchTokenUri` değerle eşleşen abonelik bölgenizi. Abonelik anahtarınızı sınıfı başlattığınızda geçirin.

```cs
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

## <a name="speech-to-text-api"></a>Konuşma metin çevirisi API'si

Konuşmayı metne REST API, yalnızca kısa konuşma destekler. İstekleri en fazla 10 saniye ses toplam süre 14 saniye içerebilir. REST API, son sonuçları, kısmi ya da Ara sonuçlar yalnızca döndürür.

Uzun ses uygulamanız için bir gereksinim gönderdiği kullanmayı [Speech SDK'sı](speech-sdk.md) veya [toplu transkripsiyonu](batch-transcription.md).

### <a name="regions-and-endpoints"></a>Bölgeler ve uç noktaları

Bu bölgeler, REST API kullanarak konuşma metin döküm için desteklenir. Eşleşen abonelik bölgenizi uç nokta seçtiğinizden emin olun.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="query-parameters"></a>Sorgu parametreleri

Bu parametreleri REST isteğinin sorgu dizesinde eklenebilir.

| Parametre | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `language` | Tanınan konuşulan dil tanımlar. Bkz: [desteklenen diller](language-support.md#speech-to-text). | Gerekli |
| `format` | Sonuç biçimi belirtir. Kabul edilen değerler `simple` ve `detailed`. Basit sonuçlarında `RecognitionStatus`, `DisplayText`, `Offset`, ve `Duration`. Ayrıntılı yanıtlar, birden çok sonuçla güvenle değerleri ve dört farklı temsilleri içerir. Varsayılan ayar `simple`. | İsteğe bağlı |
| `profanity` | Tanıma sonuçları küfür nasıl ele alınacağını belirtir. Değerler kabul `masked`, yıldız işareti ile küfür değiştirir `removed`, sonuç, tüm küfür kaldırın veya `raw`, sonuçta küfür içerir. Varsayılan ayar `masked`. | İsteğe bağlı |

### <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, Konuşmayı metne istekler için gerekli ve isteğe bağlı üst bilgileri listeler.

|Üst bilgi| Açıklama | Gerekli / isteğe bağlı |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Konuşma hizmeti abonelik anahtarınız. | Ya da bu üst bilgi veya `Authorization` gereklidir. |
| `Authorization` | Bir yetkilendirme belirteci word tarafından öncesinde `Bearer`. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Ya da bu üst bilgi veya `Ocp-Apim-Subscription-Key` gereklidir. |
| `Content-type` | Sağlanan ses verisi codec ve biçim açıklar. Kabul edilen değerler `audio/wav; codecs=audio/pcm; samplerate=16000` ve `audio/ogg; codecs=opus`. | Gerekli |
| `Transfer-Encoding` | Öbekli ses, tek bir dosya yerine gönderilen veri olduğunu belirtir. Yalnızca ses verileri varsa bu üstbilgiyi kullanır. | İsteğe bağlı |
| `Expect` | Öbekli aktarım kullanıyorsanız, gönderme `Expect: 100-continue`. Konuşma hizmeti, ilk istek bildirir ve ek veri bekler.| Öbekli ses veri gönderen gereklidir. |
| `Accept` | Sağlanırsa, olmalıdır `application/json`. Konuşma hizmeti, json'da sonuçlar sağlar. Bir uyumsuz varsayılan değer her zaman için iyi bir uygulama, bu nedenle, bir belirtmezseniz dahil bazı Web isteği çerçeveleri sağlar `Accept`. | İsteğe bağlı, ancak önerilir. |

### <a name="audio-formats"></a>Ses biçimleri

Ses HTTP gövdesi gönderilen `POST` isteği. Bu tabloda biçimlerden birinde olmalıdır:

| Biçimlendir | Codec bileşeni | Bit hızı | Örnek hızı |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bit | 16 kHz, mono |
| OGG | GEÇERLİ | 16-bit | 16 kHz, mono |

>[!NOTE]
>Yukarıdaki biçimleri, REST API ve konuşma hizmeti, WebSocket üzerinden desteklenir. [Speech SDK'sı](speech-sdk.md) WAV PCM codec ile biçim şu anda yalnızca destekler.

### <a name="sample-request"></a>Örnek istek

Bu tipik bir HTTP isteğidir. Aşağıdaki örnek, ana bilgisayar adı ve gerekli üst bilgileri içerir. Hizmet bu örnekte yer almayan ses veri girmeniz gerektiğini unutmayın. Belirtildiği gibi daha önce parçalama, ancak gerekli değildir önerilir.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status-codes"></a>HTTP durum kodları

Her yanıt için HTTP durum kodu, başarı veya sık karşılaşılan hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| 100 | Devam | İlk istek kabul edildi. Kalan verileri göndermek ile devam edin. (İle öbekli aktarım kullanılır.) |
| 200 | Tamam | İstek başarılı oldu; yanıt gövdesi bir JSON nesnesidir. |
| 400 | Hatalı istek | Dil kodu yok veya sağlanan desteklenen bir dil değil; ses dosyası geçersiz. |
| 401 | Yetkilendirilmemiş | Abonelik anahtarı veya yetkilendirme belirteci, belirtilen bölge veya geçersiz uç nokta geçersiz. |
| 403 | Yasak | Eksik abonelik anahtarı veya yetkilendirme belirteci. |

### <a name="chunked-transfer"></a>Öbekli aktarım

Öbekli aktarım (`Transfer-Encoding: chunked`) tanıma gecikme süresi, aktarım sırasında ses dosyası işlemesi konuşma hizmeti izin verdiğinden azaltmaya yardımcı olabilir. REST API, kısmi veya Ara sonuçlar sağlamaz. Bu seçenek, yalnızca yanıt verme hızını artırmak için tasarlanmıştır.

Bu kod örneği, nasıl öbekler halinde ses gönderileceğini gösterir. Yalnızca ilk öbekte ses dosyanın üst bilgisi içermelidir. `request` HTTPWebRequest nesneyi uygun REST uç noktasına bağlanır. `audioFile` ses dosyası diskte yoludur.

```csharp

    HttpWebRequest request = null;
    request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
    request.SendChunked = true;
    request.Accept = @"application/json;text/xml";
    request.Method = "POST";
    request.ProtocolVersion = HttpVersion.Version11;
    request.Host = host;
    request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
    request.Headers["Ocp-Apim-Subscription-Key"] = args[1];
    request.AllowWriteStreamBuffering = false;

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

### <a name="response-parameters"></a>Yanıt parametreleri

Sonuçları JSON olarak sağlanır. `simple` Biçimi bu üst düzey alanlar içerir.

| Parametre | Açıklama  |
|-----------|--------------|
|`RecognitionStatus`|Durumu gibi `Success` başarılı tanıma. Sonraki tabloya bakın.|
|`DisplayText`|Harf, noktalama işaretleri, ters metin normalleştirme (dönüştürme konuşulan metnin gibi 200 "iki yüz" veya "Dr kısa form sonra tanınan metin. Smith"için"doktor smith") ve küfür maskeleme. Yalnızca başarı sunar.|
|`Offset`|Tanınan konuşma tanıma ses akışı başlar süre (100 nanosaniyelik birimleri).|
|`Duration`|Ses akışı olarak tanınan konuşma süresi (100 nanosaniyelik birimlerindeki).|

`RecognitionStatus` Alan, bu değerleri içerebilir:

| Durum | Açıklama |
|--------|-------------|
| `Success` | Tanıma başarılı oldu ve `DisplayText` alanının olduğunu farkedin. |
| `NoMatch` | Konuşma Tanıma Ses akışında algılandı, ancak hiçbir hedef dil sözcükleri eşleştirilmiş olan. Genellikle kullanıcı Konuşmayı olandan farklı bir dil tanıma dilidir anlamına gelir. |
| `InitialSilenceTimeout` | Ses akışı başlangıcını yalnızca sessizlik ve konuşma için beklerken zaman aşımına hizmetini içeriyordu. |
| `BabbleTimeout` | Ses akışı başlangıcını yalnızca gürültü ve konuşma için beklerken zaman aşımına hizmetini içeriyordu. |
| `Error` | Tanıma hizmeti bir iç hatayla karşılaştı ve çalışmaya devam edemedi. Mümkün olduğunda yeniden deneyin. |

> [!NOTE]
> Ses yalnızca küfür oluşuyorsa ve `profanity` sorgu parametresi ayarlandığında `remove`, hizmeti bir konuşma sonuç döndürmez.

`detailed` Biçimi aynı verileri içeren `simple` , bunların ile biçimde `NBest`, aynı konuşma tanıma sonucun alternatif yorum listesi. Bu sonuçları büyük olasılıkla'nden alınarak sıralanır az büyük olasılıkla ilk giriş ana tanıma işleminin sonucu aynıdır.  Kullanırken `detailed` biçimi `DisplayText` olarak sağlanan `Display` her sonucu için `NBest` listesi.

Her bir nesnenin `NBest` liste aşağıdakileri içerir:

| Parametre | Açıklama |
|-----------|-------------|
| `Confidence` | Güvenilirlik puanı 1.0 (tam güven) girişinin 0,0 (güven yok) |
| `Lexical` | Sözcük şeklinde tanınan metin: Gerçek sözcüklerin tanınır. |
| `ITN` | Ters metin normalleştirilmiş ("") kurallı tanınan metinle telefon numaraları, sayılar, kısaltmaları ("doktor smith" için "dr smith") ve uygulanan diğer dönüşümler. |
| `MaskedITN` | Edemezsiniz formun, istenmesi halinde uygulanan küfür maskeleme ile. |
| `Display` | Noktalama işaretleri ve eklenen büyük/küçük harf ile tanınan metin görüntüleme formu. Bu parametre aynı olan `DisplayText` biçimi ayarlandığında sağlanan `simple`. |

### <a name="sample-responses"></a>Örnek yanıt

Bu tipik bir yanıt için olan `simple` tanıma.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Bu tipik bir yanıt için olan `detailed` tanıma.

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="text-to-speech-api"></a>Metin okuma API'si

Her biri belirli bir dil ve yerel ayar tarafından tanımlanan diyalekti, destekleyen sinir ve standart metinden konuşmaya seslerle metin okuma REST API'sini destekler.

* Sesler tam bir listesi için bkz. [dil desteği](language-support.md#text-to-speech).
* Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeleri](regions.md#text-to-speech).

> [!IMPORTANT]
> Maliyetleri için standart, özel ve sinir sesleri farklılık gösterir. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, Konuşmayı metne istekler için gerekli ve isteğe bağlı üst bilgileri listeler.

| Üst bilgi | Açıklama | Gerekli / isteğe bağlı |
|--------|-------------|---------------------|
| `Authorization` | Bir yetkilendirme belirteci word tarafından öncesinde `Bearer`. Ek bilgi için bkz: [kimlik doğrulaması](#authentication). | Gerekli |
| `Content-Type` | Sağlanan metin için içerik türünü belirtir. Kabul değeri: `application/ssml+xml`. | Gerekli |
| `X-Microsoft-OutputFormat` | Ses çıkış biçimini belirtir. Kabul edilen değerlerin tam listesi için bkz. [ses çıkış](#audio-outputs). | Gerekli |
| `User-Agent` | Uygulama adı. 255 karakterden kısa olmalıdır. | Gerekli |

### <a name="audio-outputs"></a>Ses çıkarır

Bu, her isteği olarak gönderilir ve ses desteklenen biçimler listesini `X-Microsoft-OutputFormat` başlığı. Her bir bit hızı ve kodlama türünü içerir. Konuşma hizmeti, 24 KHz, 16 KHz destekler ve 8 KHz ses çıkarır.

|||
|-|-|
| `raw-16khz-16bit-mono-pcm` | `raw-8khz-8bit-mono-mulaw` |
| `riff-8khz-8bit-mono-alaw` | `riff-8khz-8bit-mono-mulaw` |
| `riff-16khz-16bit-mono-pcm` | `audio-16khz-128kbitrate-mono-mp3` |
| `audio-16khz-64kbitrate-mono-mp3` | `audio-16khz-32kbitrate-mono-mp3` |
| `raw-24khz-16bit-mono-pcm` | `riff-24khz-16bit-mono-pcm` |
| `audio-24khz-160kbitrate-mono-mp3` | `audio-24khz-96kbitrate-mono-mp3` |
| `audio-24khz-48kbitrate-mono-mp3` | |

> [!NOTE]
> Seçilen ses ve çıkış biçimi farklı bit hızlarında varsa, ses, gerektiği şekilde örneklenmiş. Ancak, 24khz sesleri desteklemeyen `audio-16khz-16kbps-mono-siren` ve `riff-16khz-16kbps-mono-siren` Çıkış biçimleri.

### <a name="request-body"></a>İstek gövdesi

Her gövdesi `POST` isteği olarak gönderilecek [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md). SSML'yi sesi ve Sentezlenen konuşma metin okuma hizmet tarafından döndürülen dili seçmenize olanak sağlar. Desteklenen sesleri tam bir listesi için bkz. [dil desteği](language-support.md#text-to-speech).

> [!NOTE]
> Özel ses kullanıyorsanız, bir istek gövdesi (ASCII veya UTF-8) düz metin olarak gönderilebilir.

### <a name="sample-request"></a>Örnek istek

Bu HTTP isteğinin SSML ses ve dil belirtmek için kullanır. Gövde 1.000 karakterden uzun olamaz.

```http
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female'
    name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

### <a name="http-status-codes"></a>HTTP durum kodları

Her yanıt için HTTP durum kodu, başarı veya sık karşılaşılan hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| 200 | Tamam | İstek başarılı oldu; ses dosyası yanıt gövdesidir. |
| 400 | Bozuk İstek | Gerekli parametre eksik, boş veya null. Veya, gerekli veya isteğe bağlı parametresi için geçirilen değer geçersiz. Çok uzun üstbilgi buna yaygın bir sorundur. |
| 401 | Yetkilendirilmemiş | İstek yetkili değil. Abonelik anahtarı veya belirteç geçerli ve doğru bölgesinde olduğundan emin olmak için kontrol edin. |
| 413 | İstek varlığı çok büyük | SSML'yi giriş metni, 1024 karakterden uzun. |
| 429 | Çok Fazla İstek | Kota veya aboneliğiniz için izin isteği sayısını aştınız. |
| 502 | Hatalı Ağ Geçidi | Ağ veya sunucu tarafı sorun. Geçersiz üst bilgileri de gösterebilir. |

HTTP durum ise `200 OK`, yanıt gövdesi istenen biçiminde bir ses dosyası içerir. Bu dosya, aktarılan, arabellek için kaydedildi veya bir dosyaya kaydedilebilir olarak yürütülebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
