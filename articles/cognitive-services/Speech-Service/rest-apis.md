---
title: Konuşma hizmeti REST API'leri
description: Konuşma hizmeti için REST API referansı.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: 7d5656d6599e1d8d2a3e85b9d41bcce6490e1511
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124176"
---
# <a name="speech-service-rest-apis"></a>Konuşma hizmeti REST API'leri

Birleşik konuşma hizmeti REST API'ları tarafından sağlanan API'leri benzerdir [Bing konuşma API'si](https://docs.microsoft.com/azure/cognitive-services/Speech). Bing konuşma hizmeti tarafından kullanılan uç noktalarını uç noktalarına farklıdır. Bölgesel uç noktaları kullanılabilir ve kullanmakta olduğunuz uç noktaya karşılık gelen bir abonelik anahtarı kullanması gerekir.

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Konuşmayı metne dönüştürme REST API'si uç noktaları aşağıdaki tabloda gösterilmektedir. Eşleşen abonelik bölgenizi kullanın.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

> [!NOTE]
> Akustik model veya dil modeli ya da telaffuz özelleştirdiyseniz, özel uç noktanıza kullanın.

Bu API yalnızca kısa konuşma destekler. İstekleri 10 saniyeye kadar ses içeren ve en son 14 saniyede toplam en fazla. REST API, yalnızca kısmi veya Ara sonuçlar Nihai sonuç döndürür. Konuşma hizmeti de sahip bir [toplu transkripsiyonu](batch-transcription.md) uzun ses özelliği API.

### <a name="query-parameters"></a>Sorgu parametreleri

REST isteğinin sorgu dizesinde aşağıdaki parametreleri eklenebilir.

|Parametre adı|Gerekli/isteğe bağlı|Anlamı|
|-|-|-|
|`language`|Gerekli|Tanınması için bir dil tanımlayıcısı. Bkz: [desteklenen diller](supported-languages.md#speech-to-text).|
|`format`|İsteğe bağlı<br>Varsayılan: `simple`|Sonuç biçimi `simple` veya `detailed`. Basit sonuçlarında `RecognitionStatus`, `DisplayText`, `Offset`ve süresi. Ayrıntılı sonuçları güvenle değerleri ve dört farklı temsilleri ile birden çok aday içerir.|
|`profanity`|İsteğe bağlı<br>Varsayılan: `masked`|Küfür tanıma sonuçları işlemek nasıl. Olabilir `masked` (küfür yıldız işareti ile değiştirir), `removed` (kaldırır. tüm küfür), veya `raw` (küfür içerir).

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki alanlar, HTTP istek bağlığında gönderilir.

|Üst bilgi|Anlamı|
|------|-------|
|`Ocp-Apim-Subscription-Key`|Konuşma hizmeti abonelik anahtarınızı. Ya da bu üst bilgi veya `Authorization` sağlanmalıdır.|
|`Authorization`|Bir yetkilendirme belirteci word tarafından öncesinde `Bearer`. Ya da bu üst bilgi veya `Ocp-Apim-Subscription-Key` sağlanmalıdır. Bkz: [kimlik doğrulaması](#authentication).|
|`Content-type`|Biçimi ve verilerin ses codec açıklar. Şu anda, bu değer olmalıdır `audio/wav; codec=audio/pcm; samplerate=16000`.|
|`Transfer-Encoding`|İsteğe bağlı. Verilen olmalıdır `chunked` yerine tek bir dosyayı birden çok küçük öbekler halinde gönderilmesi ses verilerin sağlamak için.|
|`Expect`|Öbekli aktarım kullanıyorsanız, gönderme `Expect: 100-continue`. Konuşma hizmeti, ilk istek bildirir ve ek veri bekler.|
|`Accept`|İsteğe bağlı. Sağlanırsa, içermelidir `application/json`gibi konuşma tanıma hizmeti sonuçları JSON biçiminde sağlar. (Bir uyumsuz varsayılan değer her zaman için iyi bir uygulama, bu nedenle, bir belirtmezseniz dahil bazı Web isteği çerçeveleri sağlar `Accept`)|

### <a name="audio-format"></a>Ses biçimi

Ses HTTP gövdesi gönderilen `PUT` istemek ve 16-bit WAV PCM tek kanalda (tekli) 16 KHz biçiminde olmalıdır.

### <a name="chunked-transfer"></a>Öbekli aktarım

Öbekli aktarım (`Transfer-Encoding: chunked`), while ses dosyası iletilen işlemeye başlamak konuşma tanıma hizmeti izin verdiğinden tanıma gecikme süresini azaltmaya yardımcı olabilir. REST API, kısmi veya Ara sonuçlar sağlamaz; Bu seçenek, yalnızca yanıt verme hızını artırmak için tasarlanmıştır.

Aşağıdaki kod öbekler halinde ses gönderme işlemini gösterir. `request` HTTPWebRequest nesneyi uygun REST uç noktasına bağlanır. `audioFile` ses dosyası diskte yoludur.

```csharp
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

### <a name="example-request"></a>Örnek istek

Tipik bir istek verilmiştir.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status"></a>HTTP durumu

Yanıtın HTTP durum, başarı veya sık karşılaşılan hata koşulları belirtir.

HTTP kodu|Anlamı|Olası neden
-|-|-|
100|Devam|İlk istek kabul edildi. Kalan verileri göndermek ile devam edin. (İle öbekli aktarım kullanılır.)
200|Tamam|İstek başarılı oldu; yanıt gövdesi bir JSON nesnesidir.
400|Hatalı istek|Dil kodu yok veya sağlanan desteklenen bir dil değil; ses dosyası geçersiz.
401|Yetkilendirilmemiş|Abonelik anahtarı veya yetkilendirme belirteci, belirtilen bölge veya geçersiz uç nokta geçersiz.
403|Yasak|Eksik abonelik anahtarı veya yetkilendirme belirteci.

### <a name="json-response"></a>JSON yanıtı

Sonuçları JSON biçiminde döndürülür. `simple` Biçimi yalnızca aşağıdaki üst düzey alanları içerir.

|Alan adı|İçerik|
|-|-|
|`RecognitionStatus`|Durumu gibi `Success` başarılı tanıma. Sonraki tabloya bakın.|
|`DisplayText`|Harf, noktalama işaretleri, ters metin normalleştirme (dönüştürme konuşulan metnin gibi 200 "iki yüz" veya "Dr kısa form sonra tanınan metin. Smith"için"doktor smith") ve küfür maskeleme. Yalnızca başarı sunar.|
|`Offset`|Tanınan konuşma tanıma ses akışı başlar süre (100 nanosaniyelik birimleri).|
|`Duration`|Ses akışı olarak tanınan konuşma süresi (100 nanosaniyelik birimlerindeki).|

`RecognitionStatus` Alan, aşağıdaki değerleri içerebilir.

|Durum değeri|Açıklama
|-|-|
| `Success` | Tanıma başarılı oldu ve görüntü metni alanı yok. |
| `NoMatch` | Konuşma Tanıma Ses akışında algılandı, ancak hiçbir hedef dil sözcükleri eşleştirilmiş olan. Genellikle kullanıcı Konuşmayı olandan farklı bir dil tanıma dilidir anlamına gelir. |
| `InitialSilenceTimeout` | Ses akışı başlangıcını yalnızca sessizlik ve konuşma için beklerken zaman aşımına hizmetini içeriyordu. |
| `BabbleTimeout` | Ses akışı başlangıcını yalnızca gürültü ve konuşma için beklerken zaman aşımına hizmetini içeriyordu. |
| `Error` | Tanıma hizmeti bir iç hatayla karşılaştı ve çalışmaya devam edemedi. Mümkün olduğunda yeniden deneyin. |

> [!NOTE]
> Kullanıcı yalnızca küfür verirse ve `profanity` sorgu parametresi ayarlandığında `remove`, hizmet tanıma modunda olmadığı sürece bir konuşma sonuç döndürmez `interactive`. Bu durumda, hizmeti ile bir konuşma sonuç döndürür bir `RecognitionStatus` , `NoMatch`. 

`detailed` Biçimi içeren aynı alanları `simple` , bunların ile biçimde bir `NBest` alan. `NBest` Alan büyük olasılıkla'den az büyük olasılıkla sıralanmış aynı konuşma alternatif ınterpretations listesi verilmiştir. İlk giriş ana tanıma işleminin sonucu aynıdır. Her girişin aşağıdaki alanları içerir:

|Alan adı|İçerik|
|-|-|
|`Confidence`|Güvenilirlik puanı 1.0 (tam güven) girişinin 0,0 (güven yok)
|`Lexical`|Sözcük şeklinde tanınan metin: Gerçek sözcüklerin tanınır.
|`ITN`|Ters metin normalleştirilmiş ("") kurallı tanınan metinle telefon numaraları, sayılar, kısaltmaları ("doktor smith" için "dr smith") ve uygulanan diğer dönüşümler.
|`MaskedITN`| Edemezsiniz formun, istenmesi halinde uygulanan küfür maskeleme ile.
|`Display`| Noktalama işaretleri ve eklenen büyük/küçük harf ile tanınan metin görüntüleme formu. Aynı `DisplayText` en üst düzey sonuç.

### <a name="sample-responses"></a>Örnek yanıt

Tipik bir yanıt için verilmiştir `simple` tanıma.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Tipik bir yanıt aşağıdadır `detailed` tanıma.

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

## <a name="text-to-speech"></a>Metin Okuma

Konuşma hizmetin metin okuma API REST uç noktaları verilmiştir. Eşleşen abonelik bölgenizi uç noktası kullanma.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

> [!NOTE]
> Özel ses tipi oluşturduysanız, ilişkili özel bir uç noktasını kullanın.

Konuşma hizmeti, Bing konuşma tarafından desteklenen 16Khz çıkış ek olarak 24-KHz ses çıkış destekler. Kullanmak için dört 24-KHz Çıkış biçimleri kullanılabilir `X-Microsoft-OutputFormat` HTTP üst bilgisi, iki 24-KHz sesleri olarak `Jessa24kRUS` ve `Guy24kRUS`.

Yerel ayar | Dil   | Cinsiyet | Hizmet adı eşleme
-------|------------|--------|------------
tr-TR  | İngilizce (ABD) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, Jessa24kRUS)" 
tr-TR  | İngilizce (ABD) | Erkek   | "Microsoft Server Konuşma metin konuşma ses (en-US, Guy24kRUS)"

Kullanılabilir seslerini tam listesi kullanılabilir [desteklenen diller](supported-languages.md#text-to-speech).

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki alanlar, HTTP istek bağlığında gönderilir.

|Üst bilgi|Anlamı|
|------|-------|
|`Authorization`|Bir yetkilendirme belirteci word tarafından öncesinde `Bearer`. Gereklidir. Bkz: [kimlik doğrulaması](#authentication).|
|`Content-Type`|Giriş içerik türü: `application/ssml+xml`.|
|`X-Microsoft-OutputFormat`|Çıkış ses biçimi. Sonraki tabloya bakın.|
|`X-Search-AppId`|İstemci uygulaması benzersiz olarak tanımlayan yalnızca onaltılık GUID (çizgi içermeyen). Bu depo kimliği olabilir Mağaza uygulaması değil ff, herhangi bir GUID kullanabilirsiniz.|
|`X-Search-ClientId`|Her yükleme için bir uygulama örneği benzersiz olarak tanımlayan yalnızca onaltılık GUID (çizgi içermeyen).|
|`User-Agent`|Uygulama adı. Gerekli; 255'den az karakter içermelidir.|

Kullanılabilir ses çıkış biçimleri (`X-Microsoft-OutputFormat`) bir bit hızı ve bir kodlama dahil edilip derecelendirilir.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

### <a name="request-body"></a>İstek gövdesi

Konuşma olarak sentezlenecek metni, HTTP gövdesi olarak gönderilen `POST` isteği ya da düz metin veya [konuşma sentezi biçimlendirme dili](speech-synthesis-markup.md) metni UTF-8 kodlamalı (SSML'yi) biçimi. Hizmetin varsayılan ses dışındaki bir ses kullanmak istiyorsanız SSML'yi kullanmanız gerekir.

### <a name="sample-request"></a>Örnek istek

Aşağıdaki HTTP isteği bir SSML'yi gövdesi ses seçmek için kullanır. Gövde 1000 karakter uzunluğunda olmalıdır.

```xml
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

### <a name="http-response"></a>HTTP yanıtı

Yanıtın HTTP durum, başarı veya sık karşılaşılan hata koşulları belirtir.

HTTP kodu|Anlamı|Olası neden
-|-|-|
200|Tamam|İstek başarılı oldu; ses dosyası yanıt gövdesidir.
400|Hatalı istek|Gerekli üstbilgi alanı değeri çok uzun veya geçersiz SSML'yi belge eksik.
401|Yetkilendirilmemiş|Abonelik anahtarı veya yetkilendirme belirteci, belirtilen bölge veya geçersiz uç nokta geçersiz.
403|Yasak|Eksik abonelik anahtarı veya yetkilendirme belirteci.
413|İstek varlığı çok büyük|Giriş metni 1.000 karakterden daha uzun.

HTTP durum ise `200 OK`, yanıt gövdesi istenen biçiminde bir ses dosyası içerir. Bu dosya, aktarılan ya bir arabellek veya daha sonra kayıttan yürütmek ya da diğer kullanım için Dosya kaydedildi olarak yürütülen.

## <a name="authentication"></a>Kimlik Doğrulaması

Konuşma hizmetin REST API'sine bir istek göndermek için bir abonelik anahtarı ya da bir erişim belirteci gerektirir. Genel olarak, abonelik anahtarını doğrudan göndermek kolay; Konuşma hizmeti erişim belirteci, ardından alır. Ancak, yanıt süresi en aza indirmek için bunun yerine bir erişim belirteci kullanmak isteyebilirsiniz.

Bölgesel bir konuşma hizmeti için abonelik anahtarınızı sunarak belirteç edinme `issueToken` uç noktası, aşağıdaki tabloda gösterilen. Eşleşen abonelik bölgenizi uç noktası kullanma.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

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
> Bir zamanlayıcı kullanmak yerine, bir zaman damgası, son belirteç alınamadı, daha sonra yalnızca geçerlilik süresi bitmeye yakın olması durumunda, yeni bir istek saklayabilirsiniz. Bu yaklaşım, yeni belirteçleri gereksiz yere isteyen önler ve seyrek konuşma isteklerde programları için daha uygun olabilir.

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

