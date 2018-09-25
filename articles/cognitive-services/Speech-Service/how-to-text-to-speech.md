---
title: Konuşma Hizmetleri kullanarak metinden konuşmaya kullanın
description: Metin okuma konuşma hizmeti kullanmayı öğrenin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/08/2018
ms.author: v-jerkin
ms.openlocfilehash: 776b8496ea3f46287e2eeec7c150b8d60ca3e553
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964113"
---
# <a name="use-text-to-speech-in-speech-service"></a>"Metin okuma" konuşma hizmeti kullanın

Konuşma hizmeti, basit bir HTTP isteği aracılığıyla metin okuma işlevselliği sağlar. `POST` Uygun uç noktaya ve hizmet konuşulan metnin ses dosyası döndürür (`.wav`) içeren oluşturulan konuşma. Bunu beğeni gibi uygulamanızın daha sonra bu ses kullanabilirsiniz.

Metin okuma (ASCII veya UTF8) düz metin olabilir veya bir istek gövdesi gönderinin [SSML'yi](speech-synthesis-markup.md) belge. Düz metin istekleri ile bir varsayılan ses konuşulan. Çoğu durumda, bir SSML'yi gövdesi kullanmak istiyorsunuz. HTTP isteği içermelidir bir [yetkilendirme](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#authentication) belirteci. 

Bölgesel metin okuma uç noktaları burada gösterilir. Bir aboneliğiniz için uygun kullanın.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

## <a name="specify-a-voice"></a>Bir ses belirtin

Bir ses belirtmek için kullanın `<voice>` [SSML'yi](speech-synthesis-markup.md) etiketi. Örneğin:

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Bkz: [seslerle metin okuma](supported-languages.md#text-to-speech) kullanılabilir seslerini ve adlarının listesi.

## <a name="make-a-request"></a>İstekte bulunma

Bir metin okuma HTTP isteği, istek gövdesinde söylenir POST modunda metin ile yapılır. HTTP isteği gövdesinin uzunluğunu en fazla 1024 karakter olabilir. İstek şu olmalıdır: 

Üst bilgi|Değerler|Yorumlar
-|-|-
|`Content-Type` | `application/ssml+xml` | Giriş metin biçimi.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`riff-16khz-16bit-mono-pcm`<br>`raw-8khz-8bit-mono-mulaw`<br>`riff-8khz-8bit-mono-mulaw`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | Çıkış ses biçimi.
|`User-Agent`   |Uygulama adı | Uygulama adı gereklidir ve 255'den az karakter olmalıdır.
| `Authorization`   | Belirteç Hizmeti için abonelik anahtarınızı sunarak alınan yetkilendirme belirteci. Her belirteç on dakika için geçerlidir. Bkz: [REST API'leri: kimlik doğrulaması](rest-apis.md#authentication).

> [!NOTE]
> Seçilen ses ve çıkış biçimi farklı bit hızlarında varsa, ses, gerektiği şekilde örneklenmiş.

Bir örnek istek aşağıda gösterilmiştir.

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

200 yanıt gövdesi durumundaki belirtilen çıkış biçiminde bir ses içerir.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Bir hata oluşursa, aşağıdaki durum kodları kullanılır. Hatanın yanıt gövdesi de sorun açıklamasını içerir.

|Kod|Açıklama|Sorun|
|-|-|-|
400 |Bozuk İstek |Gerekli parametre eksik, boş veya null. Veya, gerekli veya isteğe bağlı parametresi için geçirilen değer geçersiz. Çok uzun üstbilgi buna yaygın bir sorundur.
401|Yetkilendirilmemiş |İstek yetkili değil. Abonelik anahtarınızı emin olun veya belirteç geçerli değil.
413|İstek varlığı çok büyük|Giriş SSML'yi çok büyük veya 3'den fazla içerdiği `<voice>` öğeleri.
429|Çok Fazla İstek|Kota veya aboneliğiniz için izin isteği sayısını aştınız.
|502|Hatalı Ağ Geçidi    | Ağ veya sunucu tarafı sorun. Geçersiz üst bilgileri de gösterebilir.

Metni konuşma REST API'si hakkında daha fazla bilgi için bkz. [REST API'leri](rest-apis.md#text-to-speech).

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C++'ta konuşma tanıma](quickstart-cpp-windows.md)
- [C# ' de Konuşma tanıma](quickstart-csharp-dotnet-windows.md)
- [Java'da konuşma tanıma](quickstart-java-android.md)
