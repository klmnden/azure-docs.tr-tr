---
title: Metin okuma API Microsoft konuşma hizmetinin | Microsoft Docs
description: Gerçek zamanlı metin okuma dönüştürme sesleri ve dilleri çeşitli sağlamak için metin okuma API kullanın
services: cognitive-services
author: priyaravi20
manager: yanbo
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 03/16/2017
ms.author: priyar
ms.openlocfilehash: 4b633cefa37c11511a8171d5a7f61b03dfaa4466
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352517"
---
# <a name="bing-text-to-speech-api"></a>Bing metin okuma API

## <a name="Introduction"></a>Giriş

Bing metin okuma ile API, uygulamanızın burada metin anında İnsan sesli konuşma içinde oluşturulan ve bir ses dosyası olarak döndürülen bulut sunucusu, HTTP istekleri gönderebilirsiniz. Bu API, çeşitli farklı sesler ve dilleri gerçek zamanlı metin okuma dönüştürmede sağlamak için birçok farklı bağlamlarında kullanılabilir.

## <a name="VoiceSynReq"></a>Sesli Birleştirici isteği

### <a name="Subscription"></a>Yetkilendirme belirteci

Her ses Birleştirici isteği bir JSON Web Token (JWT) erişim belirteci gerektirir. JWT erişim belirteci konuşma istek üstbilgisinde geçirilir. Belirtecin 10 dakikalık bir sona erme saati vardır. Abone olma ve geçerli JWT erişim belirteçleri almak için kullanılan API anahtarlarını alma hakkında daha fazla bilgi için bkz: [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/).

API anahtarını belirteç hizmetine geçirilir. Örneğin:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

Belirteç erişimi için gerekli üst bilgileri aşağıdaki gibidir.

Ad| Biçimlendir | Açıklama
----|----|----
Ocp Apim abonelik anahtarı | ASCII | Abonelik anahtarınız

JWT erişim belirteci olarak belirteç hizmetine döndürür `text/plain`. JWT olarak geçirilen sonra bir `Base64 access_token` konuşma uç noktası dizesiyle önekli bir authorization üstbilgisi olarak `Bearer`. Örneğin:

`Authorization: Bearer [Base64 access_token]`

İstemciler, metin okuma hizmete erişmek için aşağıdaki bitiş noktasını kullanmanız gerekir:

`https://speech.platform.bing.com/synthesize`

>[!NOTE]
>Bir erişim belirteci daha önce açıklandığı gibi abonelik anahtarınızla edindiğiniz kadar bu bağlantıyı oluşturan bir `403 Forbidden` yanıt hata.

### <a name="Http"></a>HTTP üstbilgileri

Aşağıdaki tabloda sesli Birleştirici istekleri için kullanılan HTTP üst bilgilerini gösterir.

Üst bilgi |Değer |Yorumlar
----|----|----
Content-Type | Uygulama/ssml + xml | Giriş içerik türü.
X Microsoft OutputFormat | **1.** ssml-16 khz-16 bit-mono-tts <br> **2.** ham-16 khz-16 bit-mono-pcm <br>**3.** ses-16 khz-16 KB/sn-mono-siren <br> **4.** RIFF-16 khz-16 KB/sn-mono-siren <br> **5.** RIFF-16 khz-16 bit-mono-pcm <br> **6.** ses-16 khz-128kbitrate-mono-mp3 <br> **7.** ses-16 khz-64kbitrate-mono-mp3 <br> **8.** ses-16 khz-32kbitrate-mono-mp3 | Çıktı ses biçimi.
X arama AppID | Bir GUID (onaltılık yalnızca, tirelere) | İstemci uygulaması benzersiz olarak tanımlayan bir kimliği. Bu uygulamalar için depolama kimliği olabilir. Bir kullanılabilir durumda değilse, bir uygulama için oluşturulan kullanıcı kimliği olabilir.
X arama ClientID | Bir GUID (onaltılık yalnızca, tirelere) | Her yükleme için uygulama örneğini benzersiz olarak tanımlayan bir kimliği.
Kullanıcı Aracısı | Uygulama adı | Uygulama adı gereklidir ve 255'den az karakter olmalıdır.
Yetkilendirme | Yetkilendirme belirteci |  Bkz: <a href="#Subscription">yetkilendirme belirtecini</a> bölümü.

### <a name="InputParam"></a>Giriş parametreleri

Bing metin okuma API isteklerini HTTP POST çağrıları kullanılarak yapılır. Üstbilgiler önceki bölümde belirtilir. Gövde oluşturulan için metni temsil eden konuşma Birleştirici işaretleme dili (SSML) giriş içeriyor. Konuşma dili gibi yönlerini ve Konuşmacı cinsiyetiniz denetlemek için kullanılan biçimlendirme bir açıklaması için bkz: [SSML W3C belirtimi](http://www.w3.org/TR/speech-synthesis/).

>[!NOTE]
>Desteklenen SSML giriş en büyük boyutu, tüm etiketleri de dahil olmak üzere, 1.024 karakterdir.

###  <a name="SampleVoiceOR"></a>Örnek: ses çıkış isteği

Ses çıkış isteği örneği aşağıdaki gibidir:

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

Bing metin okuma API istemciye ses göndermek için HTTP POST kullanır. Ses akışı ve codec API yanıtını içerir ve istenen çıkış biçimi eşleşir. Belirtilen istek için döndürülen ses 15 saniye aşmamalıdır.

### <a name="SuccessfulRecResponse"></a>Örnek: başarılı Birleştirici yanıt

Aşağıdaki kod, bir JSON isteğine yanıt olarak bir başarılı sesli Birleştirici örneğidir. Açıklamaları ve kodunu biçimlendirme yalnızca bu örnek amaçlıdır ve gerçek yanıttan göz ardı edilir.

```HTTP
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

### <a name="RecFailure"></a>Örnek: Birleştirici hatası

Aşağıdaki örnek kod bir ses birleştirme sorgu hatası için bir JSON yanıtı gösterir:

```HTTP
HTTP/1.1 400 XML parser error
Content-Type: text/xml
Content-Length: 0
```

### <a name="ErrorResponse"></a>Hata yanıtları

Hata | Açıklama
----|----
HTTP 400 Hatalı istek | Gerekli bir parametre eksik, boş veya null veya ya da geçirilen değeri gerekli veya isteğe bağlı bir parametre geçersiz. "Geçersiz" yanıt almak için bir neden, izin verilen uzunluktan daha uzun bir dize değeri geçiyor. Sorunlu parametre kısa bir açıklamasını dahil edilir.
HTTP/401 Yetkisiz | İstek yetkili değil.
HTTP/413 RequestEntityTooLarge  | SSML giriş, nelerin desteklendiği daha büyüktür.
HTTP/502 BadGateway | Ağ ile ilgili bir sorun veya sunucu tarafı sorun yoktur.

Bir hata yanıtı örneği aşağıdaki gibidir:

```HTTP
HTTP/1.0 400 Bad Request
Content-Length: XXX
Content-Type: text/plain; charset=UTF-8

Voice name not supported
```

## <a name="ChangeSSML"></a>Ses çıkış SSML aracılığıyla değiştirme

Microsoft metin okuma API destekleyen SSML 1.0 W3C içinde tanımlanan [konuşma Birleştirici işaretleme dili (SSML) sürüm 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/). Bu bölüm değiştirme örnekleri konuşarak gibi oluşturulan ses çıkış özelliklerini oranı, telaffuz SSML etiketleri kullanarak vb. belirli gösterir.

1. BREAK ekleme

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, BenjaminRUS)'> Welcome to use Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.</voice> </speak>
  ```

2. Konuşma hızını değiştirmek

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody rate="+30.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

3. Söyleniş

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'> <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme></voice> </speak>
  ```

4. Toplu değiştirme

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody volume="+20.00%">Welcome to use Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

5. Aralık değiştirme

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>Welcome to use <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody></voice> </speak>
  ```

6. Değişiklik prosody dağılımı

  ```
  <speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'><voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'><prosody contour="(80%,+20%) (90%,+30%)" >Good morning.</prosody></voice> </speak>
  ```

> [!NOTE]
> Ses verileri içeren 8 k veya 16 k wav olmasını Not Dosyalanan şu biçimde: **CRC kodu** (CRC-32): 4 bayt (DWORD), geçerli aralık 0x00000000 ~ 0xFFFFFFFF; **Ses biçimi bayrağı**: 4 bayt (DWORD), geçerli aralık 0x00000000 ~ 0xFFFFFFFF; **Örnek sayısı**: 4 bayt (DWORD), geçerli aralık 0x00000000 ~ 0x7FFFFFFF; **İkili Gövde boyutu**: 4 bayt (DWORD), geçerli aralık 0x00000000 ~ 0x7FFFFFFF; **İkili gövde**: n bayt sayısı.

## <a name="SampleApp"></a>Örnek uygulama

Uygulama ayrıntıları için bkz: [Visual C# .NET metin okuma örnek uygulama](https://github.com/Microsoft/Cognitive-Speech-TTS/blob/master/Samples-Http/CSharp/TTSProgram.cs).

## <a name="SupLocales"></a>Desteklenen yerel ayarlar ve ses yazı tipleri

Aşağıdaki tabloda bazı ilgili sesli yazı tipleri ve desteklenen yerel tanımlar.

Yerel ayar | Cinsiyet | Hizmet adı eşleme
---------|--------|------------
ar-ÖRN * | Kadın | "Microsoft Server Konuşma metin okuma okuma (ar-Örneğin, Hoda)"
ar-SA'sı | Erkek | "Microsoft Server Konuşma metin okuma ses (ar-SA, Naayf)"
BG-BG | Erkek | "Microsoft Server Konuşma metin okuma ses (bg-BG, çalışan Ivan)"
CA-ES | Kadın | "Microsoft Server Konuşma metin okuma ses (ca-ES, HerenaRUS)"
cs-CZ | Erkek | "Microsoft Server Konuşma metin okuma okuma (cs-CZ, Jakub)"
da DK | Kadın | "Microsoft Server Konuşma metin okuma ses (da-DK, HelleRUS)"
de AT | Erkek | "Microsoft Server Konuşma metin okuma ses (de-AT, Michael)"
de CH | Erkek | "Microsoft Server Konuşma metin okuma ses (de-CH, Karsten)"
de-DE | Kadın | "Microsoft Server Konuşma metin okuma ses (de-DE, Hedda)"
de-DE | Kadın | "Microsoft Server Konuşma metin okuma ses (de-DE, HeddaRUS)"
de-DE | Erkek | "Microsoft Server Konuşma metin okuma ses (de-DE, Stefan, Apollo)"
el-GR | Erkek | "Microsoft Server Konuşma metin okuma ses (el-GR, Stefanos)"
tr AU | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-AU, Catherine)"
tr AU | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-AU, HayleyRUS)"
tr-CA | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-CA, Gamze)"
tr-CA | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-CA, HeatherRUS)"
tr GB | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-GB, Çiğdem, Apollo)"
tr GB | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-GB, HazelRUS)"
tr GB | Erkek | "Microsoft Server Konuşma metin okuma ses (tr-GB, George, Apollo)"
tr-IE | Erkek | "Microsoft Server Konuşma metin okuma ses (tr-IE, Gamze)"
tr-IN | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-ın, Heera, Apollo)"
tr-IN | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-IN, PriyaRUS)"
tr-IN | Erkek | "Microsoft Server Konuşma metin okuma ses (tr-ın, Ravi, Apollo)"
tr-TR | Kadın | "Microsoft Server Konuşma metin okuma ses (en-US, ZiraRUS)"
tr-TR | Kadın | "Microsoft Server Konuşma metin okuma ses (en-US, JessaRUS)"
tr-TR | Erkek | "Microsoft Server Konuşma metin okuma ses (en-US, BenjaminRUS)"
es-ES | Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, Gamze, Apollo)"
es-ES | Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, HelenaRUS)"
es-ES | Erkek | "Microsoft Server Konuşma metin okuma ses (es-ES, Pablo, Apollo)"
es-MX | Kadın | "Microsoft Server Konuşma metin okuma ses (es-MX, HildaRUS)"
es-MX | Erkek | "Microsoft Server Konuşma metin okuma ses (es-MX, Raul, Apollo)"
Fi-FI | Kadın | "Microsoft Server Konuşma metin okuma ses (fi-FI, HeidiRUS)"
fr-CA | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, Caroline)"
fr-CA | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, HarmonieRUS)"
FR CH | Erkek | "Microsoft Server Konuşma metin okuma ses (fr-CH, Guillaume)"
fr-FR | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, Julie, Apollo)"
fr-FR | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, HortenseRUS)"
fr-FR | Erkek | "Microsoft Server Konuşma metin okuma ses (fr-FR, Paul, Apollo)"
He-IL| Erkek| "Microsoft Server Konuşma metin okuma ses (he-IL, Asaf)"
yüksek giriş | Kadın | "Microsoft Server Konuşma metin okuma ses (hi-ın, Kalpana, Apollo)"
yüksek giriş | Kadın | "Microsoft Server Konuşma metin okuma ses (hi-ın, Kalpana)"
yüksek giriş | Erkek | "Microsoft Server Konuşma metin okuma ses (hi-ın, Hemant)"
hr-HR | Erkek | "Microsoft Server Konuşma metin okuma ses (hr-HR, Matej)"
hu-HU | Erkek | "Microsoft Server Konuşma metin okuma ses (hu-HU, Szabolcs)"
Kimliği kimliği | Erkek | "Microsoft Server Konuşma metin okuma ses (-ID, Andika)"
it-IT | Erkek | "Microsoft Server Konuşma metin okuma ses (it-IT, Cosimo, Apollo)"
ja-JP | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ayumi, Apollo)"
ja-JP | Erkek | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ichiro, Apollo)"
ja-JP | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, HarukaRUS)"
ja-JP | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, LuciaRUS)"
ja-JP | Erkek | "Microsoft Server Konuşma metin okuma ses (ja-JP, EkaterinaRUS)"
ko-KR | Kadın | "Microsoft Server Konuşma metin okuma ses (ko-KR, HeamiRUS)"
ms-MY | Erkek | "Microsoft Server Konuşma metin okuma ses (ms MY, Rizwan)"
nb-NO | Kadın | "Microsoft Server Konuşma metin okuma ses (nb-Hayır, HuldaRUS)"
NL-NL | Kadın | "Microsoft Server Konuşma metin okuma ses (nl-NL, HannaRUS)"
pl-PL | Kadın | "Microsoft Server Konuşma metin okuma ses (pl-PL, PaulinaRUS)"
pt-BR | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-BR, HeloisaRUS)"
pt-BR | Erkek | "Microsoft Server Konuşma metin okuma ses (pt-BR, Daniel, Apollo)"
pt-PT | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-PT, HeliaRUS)"
Ro-RO | Erkek | "Microsoft Server Konuşma metin okuma ses (ro-RO, Andrei)"
ru-RU | Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, Irina, Apollo)"
ru-RU | Erkek | "Microsoft Server Konuşma metin okuma ses (ru-RU, Pavel, Apollo)"
SK SK | Erkek | "Microsoft Server Konuşma metin okuma ses (sk-SK, Filip)"
SL SI | Erkek | "Microsoft Server Konuşma metin okuma ses (sl-sı, Lado)"
sv-SE | Kadın | "Microsoft Server Konuşma metin okuma ses (sv-SE, HedvigRUS)"
eri IN | Erkek | "Microsoft Server Konuşma metin okuma ses (eri-IN, Valluvar)"
TH-TH | Erkek | "Microsoft Server Konuşma metin okuma ses (th-TH, Pattara)"
tr-TR | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-TR, SedaRUS)"
VI VN | Erkek | "Microsoft Server Konuşma metin okuma ses (VI-VN, bir)"
zh-CN | Kadın | "Microsoft Server Konuşma metin okuma ses (zh-CN, HuihuiRUS)"
zh-CN | Kadın | "Microsoft Server Konuşma metin okuma ses (zh-CN, Yaoyao, Apollo)"
zh-CN | Erkek | "Microsoft Server Konuşma metin okuma ses (zh-CN, Kangkang, Apollo)"
zh-HK | Kadın | "Microsoft Server Konuşma metin okuma ses (zh-HK Tracy, Apollo)"
zh-HK | Kadın | "Microsoft Server Konuşma metin okuma ses (zh-HK TracyRUS)"
zh-HK | Erkek | "Microsoft Server Konuşma metin okuma ses (zh-HK Danny, Apollo)"
zh-TW | Kadın | "Microsoft Server Konuşma metin okuma ses (zh-TW, Yating, Apollo)"
zh-TW | Kadın | "Microsoft Server Konuşma metin okuma ses (zh-TW, HanHanRUS)"
zh-TW | Erkek | "Microsoft Server Konuşma metin okuma ses (zh-TW, Zhiwei, Apollo)"
 * ar-ÖRN Modern standart Arapça (MSA) destekler.

> [!NOTE]
> Unutmayın önceki hizmet adlarını **Microsoft Server Konuşma metin okuma okuma (cs-CZ, Vit)** ve **konuşma sesli (tr-IE Shaun) için Microsoft Server Konuşma metin** 3/31/2018 sonra içinde kullanım Bing konuşma API'nin özellikleri en iyi duruma getirme sırası. Lütfen güncelleştirilmiş adlarıyla kodunuzu güncelleştirin.

## <a name="TrouNSupport"></a>Sorun giderme ve desteği

Tüm sorular ve sorunlar için post [Bing konuşma hizmet](https://social.msdn.microsoft.com/Forums/en-US/home?forum=SpeechService) MSDN Forumu. Tüm ayrıntılar gibi şunları içerir:

* Bir dize örneği şöyledir tam istek.
* Uygunsa, içeren bir başarısız istek tam çıktısı kimlikleri oturum açın.
* Başarısız olan istek yüzdesi.
