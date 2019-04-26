---
title: Konuşmayı metne API Başvurusu (REST) - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Konuşma metin REST API'sini kullanmayı öğrenin. Bu makalede, sorgu seçenekleri, yetkilendirme seçenekleri hakkında bilgi edineceksiniz yapısı bir istek ve yanıt.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: baaa7b1068e13863293e0968cb0bf1ffb198882b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60487240"
---
# <a name="speech-to-text-rest-api"></a>Konuşmayı metne REST API

Alternatif olarak [Speech SDK'sı](speech-sdk.md), konuşma Hizmetleri konuşma metni bir REST API kullanarak dönüştürme izin verir. Her erişilebilen bir uç noktaya bir bölge ile ilişkilidir. Uygulamanızı kullanmayı planladığınız uç nokta için bir abonelik anahtarı gerektirir.

Konuşmayı metne REST API kullanarak önce anlayın:
* REST API istekleri yalnızca kaydedilen ses 10 saniyelik içerebilir.
* Konuşmayı metne REST API, yalnızca son sonuçları döndürür. Kısmi sonuçlar sağlanmaz.

Uzun ses uygulamanız için bir gereksinim gönderdiği kullanmayı [Speech SDK'sı](speech-sdk.md) veya [toplu transkripsiyonu](batch-transcription.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Bölgeler ve uç noktaları

Bu bölgeler, REST API kullanarak konuşma metin döküm için desteklenir. Eşleşen abonelik bölgenizi uç nokta seçtiğinizden emin olun.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

## <a name="query-parameters"></a>Sorgu parametreleri

Bu parametreleri REST isteğinin sorgu dizesinde eklenebilir.

| Parametre | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `language` | Tanınan konuşulan dil tanımlar. Bkz: [desteklenen diller](language-support.md#speech-to-text). | Gerekli |
| `format` | Sonuç biçimi belirtir. Kabul edilen değerler `simple` ve `detailed`. Basit sonuçlarında `RecognitionStatus`, `DisplayText`, `Offset`, ve `Duration`. Ayrıntılı yanıtlar, birden çok sonuçla güvenle değerleri ve dört farklı temsilleri içerir. Varsayılan ayar `simple`. | İsteğe bağlı |
| `profanity` | Tanıma sonuçları küfür nasıl ele alınacağını belirtir. Değerler kabul `masked`, yıldız işareti ile küfür değiştirir `removed`, sonuç, tüm küfür kaldırın veya `raw`, sonuçta küfür içerir. Varsayılan ayar `masked`. | İsteğe bağlı |

## <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, Konuşmayı metne istekler için gerekli ve isteğe bağlı üst bilgileri listeler.

|Üst bilgi| Açıklama | Gerekli / isteğe bağlı |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Konuşma Hizmetleri abonelik anahtarınız. | Ya da bu üst bilgi veya `Authorization` gereklidir. |
| `Authorization` | Bir yetkilendirme belirteci word tarafından öncesinde `Bearer`. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Ya da bu üst bilgi veya `Ocp-Apim-Subscription-Key` gereklidir. |
| `Content-type` | Sağlanan ses verisi codec ve biçim açıklar. Kabul edilen değerler `audio/wav; codecs=audio/pcm; samplerate=16000` ve `audio/ogg; codecs=opus`. | Gerekli |
| `Transfer-Encoding` | Öbekli ses, tek bir dosya yerine gönderilen veri olduğunu belirtir. Yalnızca ses verileri varsa bu üstbilgiyi kullanır. | İsteğe bağlı |
| `Expect` | Öbekli aktarım kullanıyorsanız, gönderme `Expect: 100-continue`. Konuşma Hizmetleri, ilk isteği onayla ve ek veri bekler.| Öbekli ses veri gönderen gereklidir. |
| `Accept` | Sağlanırsa, olmalıdır `application/json`. Konuşma Hizmetleri sonuçları JSON biçiminde sağlayın. Bir uyumsuz varsayılan değer her zaman için iyi bir uygulama, bu nedenle, bir belirtmezseniz dahil bazı Web isteği çerçeveleri sağlar `Accept`. | İsteğe bağlı, ancak önerilir. |

## <a name="audio-formats"></a>Ses biçimleri

Ses HTTP gövdesi gönderilen `POST` isteği. Bu tabloda biçimlerden birinde olmalıdır:

| Biçimlendir | Codec bileşeni | Bit hızı | Örnek hızı |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bit | 16 kHz, mono |
| OGG | GEÇERLİ | 16-bit | 16 kHz, mono |

>[!NOTE]
>Yukarıdaki biçimleri, REST API ve konuşma Hizmetleri WebSocket üzerinden desteklenir. [Speech SDK'sı](speech-sdk.md) WAV PCM codec ile biçim şu anda yalnızca destekler.

## <a name="sample-request"></a>Örnek istek

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

## <a name="http-status-codes"></a>HTTP durum kodları

Her yanıt için HTTP durum kodu, başarı veya sık karşılaşılan hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| 100 | Devam | İlk istek kabul edildi. Kalan verileri göndermek ile devam edin. (İle öbekli aktarım kullanılır.) |
| 200 | Tamam | İstek başarılı oldu; yanıt gövdesi bir JSON nesnesidir. |
| 400 | Hatalı istek | Dil kodu yok veya sağlanan desteklenen bir dil değil; ses dosyası geçersiz. |
| 401 | Yetkilendirilmemiş | Abonelik anahtarı veya yetkilendirme belirteci, belirtilen bölge veya geçersiz uç nokta geçersiz. |
| 403 | Yasak | Eksik abonelik anahtarı veya yetkilendirme belirteci. |

## <a name="chunked-transfer"></a>Öbekli aktarım

Öbekli aktarım (`Transfer-Encoding: chunked`) tanıma gecikme süresi, aktarım sırasında ses dosyası işlemesi konuşma Hizmetleri izin verdiğinden azaltmaya yardımcı olabilir. REST API, kısmi veya Ara sonuçlar sağlamaz. Bu seçenek, yalnızca yanıt verme hızını artırmak için tasarlanmıştır.

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

## <a name="response-parameters"></a>Yanıt parametreleri

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

`detailed` Biçimi aynı verileri içeren `simple` , bunların ile biçimde `NBest`, aynı tanıma sonucun alternatif yorum listesi. Bu sonuçları büyük olasılıkla'nden alınarak sıralanır az büyük olasılıkla ilk giriş ana tanıma işleminin sonucu aynıdır.  Kullanırken `detailed` biçimi `DisplayText` olarak sağlanan `Display` her sonucu için `NBest` listesi.

Her bir nesnenin `NBest` liste aşağıdakileri içerir:

| Parametre | Açıklama |
|-----------|-------------|
| `Confidence` | Güvenilirlik puanı 1.0 (tam güven) girişinin 0,0 (güven yok) |
| `Lexical` | Sözcük şeklinde tanınan metin: Gerçek sözcüklerin tanınır. |
| `ITN` | Ters metin normalleştirilmiş ("") kurallı tanınan metinle telefon numaraları, sayılar, kısaltmaları ("doktor smith" için "dr smith") ve uygulanan diğer dönüşümler. |
| `MaskedITN` | Edemezsiniz formun, istenmesi halinde uygulanan küfür maskeleme ile. |
| `Display` | Noktalama işaretleri ve eklenen büyük/küçük harf ile tanınan metin görüntüleme formu. Bu parametre aynı olan `DisplayText` biçimi ayarlandığında sağlanan `simple`. |

## <a name="sample-responses"></a>Örnek yanıt

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

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
