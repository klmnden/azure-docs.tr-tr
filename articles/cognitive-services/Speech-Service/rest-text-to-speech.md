---
title: Metin okuma API Başvurusu (REST) - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Metin okuma REST API'sini kullanmayı öğrenin. Bu makalede, sorgu seçenekleri, yetkilendirme seçenekleri hakkında bilgi edineceksiniz yapısı bir istek ve yanıt.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 9cbd924f87ff2f5b38f67a1bf7db34c36e9c264b
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65020451"
---
# <a name="text-to-speech-rest-api"></a>Metin okuma REST API

Konuşma Hizmetleri izin [Sentezlenen Konuşmayı metne dönüştürme](#convert-text-to-speech) ve [desteklenen sesleri listesini alma](#get-a-list-of-voices) bir dizi REST API'si kullanarak bir bölge için. Her kullanılabilir uç nokta bir bölge ile ilişkilidir. Kullanmayı planlıyorsanız uç nokta/bölge için bir abonelik anahtarı gereklidir.

Her biri belirli bir dil ve yerel ayar tarafından tanımlanan diyalekti, destekleyen sinir ve standart metinden konuşmaya seslerle metin okuma REST API'sini destekler.

* Sesler tam bir listesi için bkz. [dil desteği](language-support.md#text-to-speech).
* Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz. [bölgeleri](regions.md#text-to-speech).

> [!IMPORTANT]
> Maliyetleri için standart, özel ve sinir sesleri farklılık gösterir. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

Bu API kullanmadan önce anlayın:

* Metin okuma REST API, bir yetkilendirme üst bilgisi gerektirir. Bu, hizmete erişmek için bir belirteç değişimi tamamlanması gerektiği anlamına gelir. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="get-a-list-of-voices"></a>Sesler listesini alma

`voices/list` Uç noktası için belirli bir bölge/uç sesleri tam listesini almak sağlar.

### <a name="regions-and-endpoints"></a>Bölgeler ve uç noktaları

| Bölge | Uç Nokta |
|--------|----------|
| Avustralya Doğu | https://australiaeast.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Güney Brezilya | https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Orta Kanada | https://canadacentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Orta ABD | https://centralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Doğu Asya | https://eastasia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Doğu ABD | https://eastus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Doğu ABD 2 | https://eastus2.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Fransa Orta | https://francecentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Hindistan Orta | https://centralindia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Japonya Doğu | https://japaneast.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Kore Orta | https://koreacentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Orta Kuzey ABD | https://northcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Kuzey Avrupa | https://northeurope.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Orta Güney ABD | https://southcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Güneydoğu Asya | https://southeastasia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Birleşik Krallık Güney | https://uksouth.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Batı Avrupa | https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Batı ABD | https://westus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Batı ABD 2 | https://westus2.tts.speech.microsoft.com/cognitiveservices/voices/list |

### <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, metin okuma istekleri için gerekli ve isteğe bağlı üst bilgileri listeler.

| Üst bilgi | Açıklama | Gerekli / isteğe bağlı |
|--------|-------------|---------------------|
| `Authorization` | Bir yetkilendirme belirteci word tarafından öncesinde `Bearer`. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Gerekli |

### <a name="request-body"></a>İstek gövdesi

Bir gövdesi için gerekli değildir `GET` Bu uç noktaya yönelik istekler.

### <a name="sample-request"></a>Örnek istek

Bu isteği yalnızca bir yetkilendirme üst bilgisi gerektirir.

```http
GET /cognitiveservices/voices/list HTTP/1.1

Host: westus.tts.speech.microsoft.com
Authorization: Bearer [Base64 access_token]
```

### <a name="sample-response"></a>Örnek yanıt

Bu yanıt, yanıt yapısını göstermek için kısaltıldı.

> [!NOTE]
> Ses kullanılabilirlik bölgesi/uç noktası tarafından değişir.

```json
[
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)",
        "ShortName": "ar-EG-Hoda",
        "Gender": "Female",
        "Locale": "ar-EG"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)",
        "ShortName": "ar-SA-Naayf",
        "Gender": "Male",
        "Locale": "ar-SA"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)",
        "ShortName": "bg-BG-Ivan",
        "Gender": "Male",
        "Locale": "bg-BG"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)",
        "ShortName": "ca-ES-HerenaRUS",
        "Gender": "Female",
        "Locale": "ca-ES"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)",
        "ShortName": "cs-CZ-Jakub",
        "Gender": "Male",
        "Locale": "cs-CZ"
    },

    ...

]
```

### <a name="http-status-codes"></a>HTTP durum kodları

Her yanıt için HTTP durum kodu, başarı veya sık karşılaşılan hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| 200 | Tamam | İstek başarılı oldu. |
| 400 | Bozuk İstek | Gerekli parametre eksik, boş veya null. Veya, gerekli veya isteğe bağlı parametresi için geçirilen değer geçersiz. Çok uzun üstbilgi buna yaygın bir sorundur. |
| 401 | Yetkilendirilmemiş | İstek yetkili değil. Abonelik anahtarı veya belirteç geçerli ve doğru bölgesinde olduğundan emin olmak için kontrol edin. |
| 429 | Çok Fazla İstek | Kota veya aboneliğiniz için izin isteği sayısını aştınız. |
| 502 | Hatalı Ağ Geçidi | Ağ veya sunucu tarafı sorun. Geçersiz üst bilgileri de gösterebilir. |


## <a name="convert-text-to-speech"></a>Metin okumayı dönüştürme

`v1` Uç nokta kullanarak metin okuma dönüştürmenize olanak [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md).

### <a name="regions-and-endpoints"></a>Bölgeler ve uç noktaları

Bu bölgeler, REST API kullanarak metin okuma için desteklenir. Eşleşen abonelik bölgenizi uç nokta seçtiğinizden emin olun.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

### <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, metin okuma istekleri için gerekli ve isteğe bağlı üst bilgileri listeler.

| Üst bilgi | Açıklama | Gerekli / isteğe bağlı |
|--------|-------------|---------------------|
| `Authorization` | Bir yetkilendirme belirteci word tarafından öncesinde `Bearer`. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Gerekli |
| `Content-Type` | Sağlanan metin için içerik türünü belirtir. Kabul değeri: `application/ssml+xml`. | Gerekli |
| `X-Microsoft-OutputFormat` | Ses çıkış biçimini belirtir. Kabul edilen değerlerin tam listesi için bkz. [ses çıkış](#audio-outputs). | Gerekli |
| `User-Agent` | Uygulama adı. Sağlanan değer 255 karakterden kısa olmalıdır. | Gerekli |

### <a name="audio-outputs"></a>Ses çıkarır

Bu, her isteği olarak gönderilir ve ses desteklenen biçimler listesini `X-Microsoft-OutputFormat` başlığı. Her bir bit hızı ve kodlama türünü içerir. 24 KHz, 16 KHz konuşma Hizmetleri destekler ve 8 KHz ses çıkarır.

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
    name='en-US-JessaRUS'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

Hızlı başlangıçtan dile özel örnekler için bkz:

* [.NET Core, C#](quickstart-dotnet-text-to-speech.md)
* [Python](quickstart-python-text-to-speech.md)
* [Node.js](quickstart-nodejs-text-to-speech.md)

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
