---
title: Kullanım metin konuşma hizmetlerini kullanarak okuma | Microsoft Docs
description: Konuşma kullanmak metne konuşma hizmetinde kullanmayı öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 1ed2ee73b32f71d2e1ca34c6de9d1cb2649d7f0c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355643"
---
# <a name="use-text-to-speech-in-speech-service"></a>Konuşma hizmetinde "Metin okuma" kullanın

Konuşma hizmeti basit bir HTTP isteği aracılığıyla metin okuma işlevselliği sağlar. Uygun uç noktasına söylenir metin göndermek ve hizmeti bir ses dosyası döndürür (`.wav`) içeren oluşturulan konuşma. Bunu yöntemlerine gibi uygulamanızı daha sonra bu ses kullanabilirsiniz.

Metin okuma düz metin (ASCII veya UTF8) olabilir veya bir POST gövdesi isteği [SSML](speech-synthesis-markup.md) belge. Düz metin istekleri varsayılan sesli olarak konuşulan. Çoğu durumda, bir SSML gövdesi kullanmak istediğiniz. HTTP isteği bir yetki belirteci eklemeniz gerekir. 

Bölgesel metin okuma uç noktalar burada gösterilir. Aboneliğiniz için uygun kullanın.

Bölge| Uç Nokta
-|-
Batı ABD| `https://westus.tts.speech.microsoft.com/cognitiveservices/v1`
Doğu Asya| `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1`
Kuzey Avrupa| `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1`

> [!NOTE]
> Bir özel sesli yazı tipi oluşturduysanız, kendisi için yukarıdaki olanları yerine oluşturulmuş uç noktası kullan.

## <a name="specify-a-voice"></a>Sesli belirtin

Sesli belirtmek için kullanın `<voice>` [SSML](speech-synthesis-markup.md) etiketi. Örneğin:

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Bkz: [metin okuma seslerini](supported-languages.md#text-to-speech) kullanılabilir seslerini ve adları listesi.

## <a name="make-a-request"></a>Bir istekte bulunun

Bir metin okuma HTTP isteği, istek gövdesinde söylenir metinle POST modunda yapılır. HTTP isteği gövdesinin uzunluğu en fazla 1024 karakter olabilir. İstek aşağıdaki üst bilgilerine sahip olmanız gerekir: 

Üst bilgi|Değerler|Yorumlar
-|-|-
|`Content-Type` | `application/ssml+xml` | Giriş metin biçimi.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`audio-16khz-16kbps-mono-siren`<br>`riff-16khz-16kbps-mono-siren`<br>`riff-16khz-16bit-mono-pcm`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | Çıktı ses biçimi.
|`User-Agent`   |Uygulama adı | Uygulama adı gereklidir ve 255'den az karakter olmalıdır.
| `Authorization`   | Belirteç Hizmeti için abonelik anahtarınızı sunarak elde yetkilendirme belirteci. Her belirteç on dakika için geçerlidir. Bkz: [REST API'leri: kimlik doğrulaması](rest-apis.md#authentication).

> [!NOTE]
> Seçilen ses ve çıkış biçimi farklı hızları varsa, ses gerektiğinde örneklenmiş. 24khz sesleri desteklemez `audio-16khz-16kbps-mono-siren` ve `riff-16khz-16kbps-mono-siren` çıktı biçimi. 

Örnek istek aşağıda gösterilmiştir.

```xml
POST /cognitiveservices/v1
HTTP/1.1
Host: westus.tts.speech.microsoft.com
X-Microsoft-OutputFormat: riff-24khz-16bit-mono-pcm
Content-Type: application/ssml+xml
User-Agent: Test TTS application
Authorization: (authorization token)

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
</voice> </speak>
```

Belirtilen çıktı biçimi ses 200 durumuna sahip bir yanıt gövdesi içerir.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Bir hata oluşursa, aşağıdaki durum kodları kullanılır. Hatanın yanıt gövdesini de sorun açıklamasını içerir.

|Kod|Açıklama|Sorun|
|-|-|-|
400 |Hatalı İstek |Gerekli bir parametre eksik, boş veya null. Veya ya da geçirilen değeri gerekli veya isteğe bağlı bir parametre geçersiz. Çok uzun bir üstbilgi buna yaygın bir sorundur.
401|Yetkilendirilmemiş |İstek yetkili değil. Abonelik anahtarınızı emin olun veya belirteci geçerli değil.
413|İstek varlığı çok büyük|SSML giriş en fazla 1024 karakter uzunluğunda olabilir.
|502|Hatalı Ağ Geçidi    | Ağ veya sunucu tarafı sorun. Geçersiz üstbilgi da gösterebilir.

Konuşma REST API metne hakkında daha fazla bilgi için bkz: [REST API'leri](rest-apis.md#text-to-speech).

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
- [C# Konuşma tanıması](quickstart-csharp-windows.md)
