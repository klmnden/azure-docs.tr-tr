---
title: Metin okuma API'si, Microsoft konuşma hizmeti | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Seslerle ve dilleri çeşitli gerçek zamanlı metinden konuşmaya dönüştürme sağlamak için metin okuma API'si kullanma
services: cognitive-services
author: priyaravi20
manager: yanbo
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: priyar
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: a046bec5d81d828d88716d31c84e9cbcdcea1a08
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515434"
---
# <a name="bing-text-to-speech-api"></a>Bing metin okuma API'si

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

## <a name="Introduction"></a>Giriş

Bing ile metin okuma API'si, uygulamanızın nerede metin anında İnsan görünen konuşmaya oluşturulan ve bir ses dosyası olarak döndürülen bulut sunucusu, HTTP istekleri gönderebilirsiniz. Bu API, gerçek zamanlı metinden konuşmaya dönüştürme farklı sesler ve dilleri çeşitli sağlamak için birçok farklı bağlamda kullanılabilir.

## <a name="VoiceSynReq"></a>Sesli sentezi isteği

### <a name="Subscription"></a>Yetkilendirme belirteci

Her ses sentezi isteği bir JSON Web Token (JWT) erişim belirteci gerektirir. JWT erişim belirteci aracılığıyla konuşma istek üst bilgisinde geçirilir. Belirteç, 10 dakikalık bir sona erme süresi vardır. Abone olma ve geçerli JWT erişim belirteçlerini almak için kullanılan API anahtarlarını alma hakkında daha fazla bilgi için bkz. [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/).

API anahtarı belirteç hizmetine geçirilir. Örneğin:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

Belirteç erişimi için gereken üst bilgi bilgileri aşağıdaki gibidir.

Ad| Biçimlendir | Açıklama
----|----|----
Ocp-Apim-Subscription-Key | ASCII | Abonelik anahtarınız

JWT belirteci belirteç hizmetine döndürür `text/plain`. Daha sonra JWT olarak geçirilen bir `Base64 access_token` dizesiyle önekli bir yetkilendirme üst bilgisi olarak konuşma tanıma uç noktasına `Bearer`. Örneğin:

`Authorization: Bearer [Base64 access_token]`

İstemciler aşağıdaki uç noktayı metin okuma hizmetine erişmek için kullanmanız gerekir:

`https://speech.platform.bing.com/synthesize`

>[!NOTE]
>Bir erişim belirteci daha önce açıklandığı gibi abonelik anahtarınızla edindiğiniz kadar bu bağlantı oluşturur. bir `403 Forbidden` yanıtı hatası.

### <a name="Http"></a>HTTP üstbilgileri

Aşağıdaki tablo, sesli sentezi istekleri için kullanılan HTTP üst bilgilerini gösterir.

Üst bilgi |Değer |Yorumlar
----|----|----
Content-Type | Uygulama/ssml'yi + xml şeklindedir | Giriş içerik türü.
X Microsoft OutputFormat | **1.** ssml'yi-16 khz-16 bit-mono-tts <br> **2.** ham-16 khz-16 bit-mono-pcm <br>**3.** ses-16 khz-16 KB/sn-mono-siren <br> **4.** RIFF-16 khz-16 KB/sn-mono-siren <br> **5.** RIFF-16 khz-16 bit-mono-pcm <br> **6.** ses-16 khz-128kbitrate-mono-mp3 <br> **7.** ses-16 khz-64kbitrate-mono-mp3 <br> **8.** ses-16 khz-32kbitrate-mono-mp3 | Çıkış ses biçimi.
X-Search-AppId | Bir GUID (onaltılık yalnızca, çizgi içermeyen) | İstemci uygulaması benzersiz olarak tanımlayan bir kimliği. Bu uygulamalar için depolama kimliği olabilir. Bir kullanılabilir durumda değilse, bir uygulama için oluşturulan kullanıcı kimliği olabilir.
X arama ClientID | Bir GUID (onaltılık yalnızca, çizgi içermeyen) | Her yükleme için uygulama örneğini benzersiz şekilde tanımlayan bir kimliği.
Kullanıcı Aracısı | Uygulama adı | Uygulama adı gereklidir ve 255'den az karakter olmalıdır.
Yetkilendirme | Yetkilendirme belirteci |  Bkz: <a href="#Subscription">yetkilendirme belirteci</a> bölümü.

### <a name="InputParam"></a>Giriş parametreleri

Bing metin okuma API'si isteklerini HTTP POST çağrıları kullanılarak yapılır. Üstbilgileri, önceki bölümde belirtilir. Gövde sentezlenecek metni temsil eden konuşma sentezi işaretleme dili (SSML'yi) giriş içerir. Konuşma gibi dil özelliklerini ve cinsiyet konuşmacının denetlemek için kullanılan biçimlendirme açıklaması için bkz: [SSML'yi W3C belirtimi](https://www.w3.org/TR/speech-synthesis/).

>[!NOTE]
>SSML'yi giriş, desteklenen en büyük boyutunu, tüm etiketleri dahil olmak üzere, 1024 karakterdir.

###  <a name="SampleVoiceOR"></a>Örnek: ses çıkışı isteği

Ses çıkış isteğinin bir örneği aşağıdaki gibidir:

```HTTP
POST /synthesize
HTTP/1.1
Host: speech.platform.bing.com

X-Microsoft-OutputFormat: riff-8khz-8bit-mono-mulaw
Content-Type: application/ssml+xml
Host: speech.platform.bing.com
Content-Length: 197
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female' name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>Microsoft Bing Voice Output API</voice></speak>
```

## <a name="VoiceOutResponse"></a>Ses çıkış yanıt

Bing metin okuma API'si, HTTP POST ses istemciye geri göndermek için kullanır. API yanıtı ses akışı ve codec bileşeni içerir ve istenen çıktı biçimi eşleşir. Belirli bir istek için döndürülen ses 15 saniyeyi aşmamalıdır.

### <a name="SuccessfulRecResponse"></a>Örnek: başarılı sentezi yanıt

Aşağıdaki kod, bir JSON yanıtı başarılı ses sentezi isteğine örneğidir. Açıklamalar ve kodun biçimi yalnızca bu örnek amaçlıdır ve gerçek yanıttan göz ardı edilir.

```HTTP
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

### <a name="RecFailure"></a>Örnek: Birleştirici hatası

Aşağıdaki kod örneği bir ses birleştirme sorgu hatası için bir JSON yanıtı gösterilir:

```HTTP
HTTP/1.1 400 XML parser error
Content-Type: text/xml
Content-Length: 0
```

### <a name="ErrorResponse"></a>Hata yanıtları

Hata | Açıklama
----|----
HTTP/400 Hatalı istek | Gerekli parametre eksik, boş veya null olduğu veya gerekli veya isteğe bağlı parametresi için geçirilen değer geçersiz. "Geçersiz" yanıt almak için bir neden, izin verilen uzunluktan daha uzun bir dize değeri geçiyor. Sorunlu parametresi kısa bir açıklamasını dahil edilir.
HTTP/401 Yetkisiz | İstek yetkili değil.
HTTP/413 RequestEntityTooLarge  | SSML'yi giriş, nelerin desteklendiği daha büyüktür.
HTTP/502 BadGateway | Ağ ile ilgili bir sorun veya bir sunucu tarafı sorun yoktur.

Bir hata yanıtı örneği aşağıdaki gibidir:

```HTTP
HTTP/1.0 400 Bad Request
Content-Length: XXX
Content-Type: text/plain; charset=UTF-8

Voice name not supported
```

## <a name="ChangeSSML"></a>Ses çıkış SSML'yi aracılığıyla değiştirme

Microsoft metin okuma API'si destekler SSML'yi 1.0 W3C tanımlandığı şekilde [konuşma sentezi işaretleme dili (SSML'yi) sürüm 1.0](https://www.w3.org/TR/2009/REC-speech-synthesis-20090303/). Bu bölüm değiştirme örnekleri oluşturulan ses çıkış konuşma gibi özelliklerini oranı, Söyleniş SSML'yi etiketleri kullanarak vb. belirli gösterir.

1. Kesme ekleme

   ```
   <speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)'> Welcome to use Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.</voice> </speak>
   ```

2. Konuşma hızını değiştirme

   ```
   <speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody rate="+30.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
   ```

3. Söylenişi

   ```
   <speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'> <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme></voice> </speak>
   ```

4. Birimi Değiştir

   ```
   <speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody volume="+20.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
   ```

5. Aralık değiştirme

   ```
   <speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>Welcome to use <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
   ```

6. Değişiklik prosody dağılımı

   ```
   <speak version='1.0' xmlns="https://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody contour="(80%,+20%) (90%,+30%)" >Good morning.</prosody></voice> </speak>
   ```

> [!NOTE]
> Not ses verilerini şu biçimde Dosyalanan 8 k ya da 16 k wav gerekir: **CRC kod** (CRC-32): 4 bayt (DWORD), geçerli aralık 0x00000000 ~ 0xFFFFFFFF; **Ses biçimi bayrağı**: 4 bayt (DWORD), geçerli aralık 0x00000000 ~ 0xFFFFFFFF; **Örnek sayısı**: 4 bayt (DWORD), geçerli aralık 0x00000000 ~ 0x7FFFFFFF; **İkili Gövde boyutu**: 4 bayt (DWORD), geçerli aralık 0x00000000 ~ 0x7FFFFFFF; **İkili gövde**: n baytı.

## <a name="SampleApp"></a>Örnek uygulama

Uygulama ayrıntıları için bkz. [Visual C# .NET metin okuma örnek uygulaması](https://github.com/Microsoft/Cognitive-Speech-TTS/blob/master/Samples-Http/CSharp/TTSProgram.cs).

## <a name="SupLocales"></a>Desteklenen yerel ayarlar ve ses tipleri

Aşağıdaki tabloda bazı desteklenen yerel ayarlar ve ilgili ses tipi olarak tanımlar.

Yerel Ayar | Cinsiyet | Hizmet adı eşleme
---------|--------|------------
ar-Örneğin * | Kadın | "Microsoft sunucu konuşma Sesli konuşmayı metne (ar-Örneğin, Hoda)"
ar-SA | Erkek | "Microsoft Server Konuşma metin konuşma ses (ar-SA, Naayf)"
BG-BG | Erkek | "Microsoft Server Konuşma metin okuma ses (bg-BG, çalışan Ivan)"
CA-ES | Kadın | "Microsoft Server Konuşma metin okuma ses (ca-ES, HerenaRUS)"
cs-CZ | Erkek | "Microsoft sunucu konuşma Sesli konuşmayı metne (cs-CZ, Jakub)"
v-DK | Kadın | "Microsoft Server Konuşma metin konuşma ses (v-DK, HelleRUS)"
de-AT | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-AT, Michael)"
de-CH | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-CH, Karsten)"
de-DE | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, Hedda)"
de-DE | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, HeddaRUS)"
de-DE | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-DE, Stefan, Apollo)"
el-GR | Erkek | "Microsoft Server Konuşma metin konuşma ses (el-GR, Stefanos)"
tr-AU | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, Catherine)"
tr-AU | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, HayleyRUS)"
CA tr | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, Gamze)"
CA tr | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, HeatherRUS)"
en-GB | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, Susan, Apollo)"
en-GB | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, HazelRUS)"
en-GB | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-GB, George, Apollo)"
IE tr | Erkek | "Microsoft Server Konuşma metin konuşma ses (tr-IE, Sean)"
tr-giriş | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN Heera, Apollo)"
tr-giriş | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN, PriyaRUS)"
tr-giriş | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-IN Ravi, Apollo)"
en-US | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, ZiraRUS)"
en-US | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, JessaRUS)"
en-US | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, BenjaminRUS)"
es-ES | Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, Gamze, Apollo)"
es-ES | Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, HelenaRUS)"
es-ES | Erkek | "Microsoft Server Konuşma metin okuma ses (es-ES, Pablo, Apollo)"
es-MX | Kadın | "Microsoft Server Konuşma metin okuma ses (es-MX, HildaRUS)"
es-MX | Erkek | "Microsoft Server Konuşma metin okuma ses (es-MX, Raul Apollo)"
FI-FI | Kadın | "Microsoft Server Konuşma metin konuşma ses (fi-FI, HeidiRUS)"
fr-CA | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, Caroline)"
fr-CA | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, HarmonieRUS)"
FR-CH | Erkek | "Microsoft Server Konuşma metin okuma ses (fr-CH, Guillaume)"
fr-FR | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, Julie, Apollo)"
fr-FR | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, HortenseRUS)"
fr-FR | Erkek | "Microsoft Server Konuşma metin okuma ses (fr-FR, Paul, Apollo)"
He IL| Erkek| "Microsoft Server Konuşma metin konuşma ses (he-IL, Asaf)"
yüksek giriş | Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba açma, Kalpana, Apollo)"
yüksek giriş | Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Kalpana)"
yüksek giriş | Erkek | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Hemant)"
hr-HR | Erkek | "Microsoft Server Konuşma metin okuma ses (hr-HR, Matej)"
hu-HU | Erkek | "Microsoft Server Konuşma metin konuşma ses (hu-HU, Szabolcs)"
ID | Erkek | "Microsoft Server Konuşma metin konuşma ses (-ID, Andika)"
İt-IT | Erkek | "Microsoft Server Konuşma metin konuşma ses (Cosimo, it-IT, Apollo)"
İt-IT | Kadın | "Microsoft Server Konuşma metin konuşma ses (it-IT, LuciaRUS)"
ja-JP | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ayumi, Apollo)"
ja-JP | Erkek | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ichiro, Apollo)"
ja-JP | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, HarukaRUS)"
ko-KR | Kadın | "Microsoft Server Konuşma metin konuşma ses (ko-KR, HeamiRUS)"
ms-MY | Erkek | "Microsoft Server Konuşma metin okuma ses (ms MY, Rizwan)"
NB-yok | Kadın | "Microsoft Server Konuşma metin okuma ses (nb-yok, HuldaRUS)"
NL-NL | Kadın | "Microsoft Server Konuşma metin okuma ses (nl-NL, HannaRUS)"
pl-PL | Kadın | "Microsoft Server Konuşma metin okuma ses (pl-PL, PaulinaRUS)"
pt-BR | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-BR, HeloisaRUS)"
pt-BR | Erkek | "Microsoft Server Konuşma metin okuma ses (pt-BR, Daniel, Apollo)"
pt-PT | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-PT, HeliaRUS)"
Ro-RO | Erkek | "Microsoft Server Konuşma metin konuşma ses (ro-RO, Andrei)"
ru-RU | Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, Irina, Apollo)"
ru-RU | Erkek | "Microsoft Server Konuşma metin okuma ses (ru-RU, Pavel, Apollo)"
ru-RU | Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, EkaterinaRUS)"
SK-SK | Erkek | "Microsoft Server Konuşma metin okuma ses (sk-SK, Filip)"
SL SI | Erkek | "Microsoft Server Konuşma metin okuma ses (sl-sı, Lado)"
sv-SE | Kadın | "Microsoft Server Konuşma metin konuşma ses (sv-SE, HedvigRUS)"
Veri-ın | Erkek | "Microsoft Server Konuşma metin konuşma ses (veri-ın, Valluvar)"
TH TH | Erkek | "Microsoft Server Konuşma metin okuma ses (th-TH, Pattara)"
tr-TR | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-TR, SedaRUS)"
VI VN | Erkek | "Microsoft Server Konuşma metin okuma ses (vi-VN bir)"
zh-CN | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, HuihuiRUS)"
zh-CN | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Yaoyao, Apollo)"
zh-CN | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Kangkang, Apollo)"
zh-HK | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK Tracy, Apollo)"
zh-HK | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK TracyRUS)"
zh-HK | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-HK Danny, Apollo)"
zh-TW | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW Yating, Apollo)"
zh-TW | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW, HanHanRUS)"
zh-TW | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-TW Zhiwei, Apollo)"

 * ar-ÖRN Modern standart Arapça (MSA) destekler.

> [!NOTE]
> Unutmayın önceki hizmet adları **Microsoft sunucu konuşma Sesli konuşmayı metne (cs-CZ, Vit)** ve **konuşma ses (tr-IE, Shaun) için Microsoft sunucu konuşma metin** 3/31/2018 de kullanımdan Bing konuşma API'SİNİN özellikleri en iyi duruma getirme sırası. Lütfen kodunuzu güncelleştirilmiş adları ile güncelleştirin.

## <a name="TrouNSupport"></a>Sorun giderme ve Destek

Tüm soruları ve sorunları gidermek üzere [Bing konuşma hizmeti](https://social.msdn.microsoft.com/Forums/en-US/home?forum=SpeechService) MSDN Forumu. Tüm ayrıntılar gibi şunlardır:

* Tam istek dize örneği.
* Uygunsa, tam çıktısını içeren bir başarısız istek kimliklerini oturum açın.
* Başarısız olan istek yüzdesi.
