---
title: Microsoft konuşma tanıma WebSocket Protokolü | Microsoft Docs
description: Üzerinde WebSockets Protokolü belgeleri konuşma hizmeti için temel
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: 17954536e8bdb49c09204c2e522586b79cb1bef5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352318"
---
# <a name="speech-service-websocket-protocol"></a>Konuşma hizmet WebSocket Protokolü

  Konuşma, metne dönüştürme konuşulan sesi için kullanılabilen en gelişmiş algoritmalar özellikleri bulut tabanlı bir platform hizmetidir. Konuşma hizmet protokolü tanımlar [bağlantı kurma](#connection-establishment) istemci uygulamaları ve hizmet ve ortaklarınıza arasında alınıp konuşma tanıma iletileri arasında ([istemci kaynaklı iletileri](#client-originated-messages)ve [hizmeti kaynaklı iletileri](#service-originated-messages)). Ayrıca, [telemetri iletilerini](#telemetry-schema) ve [hata işleme](#error-handling) açıklanmaktadır.

## <a name="connection-establishment"></a>Bağlantı kurma

Konuşma hizmet protokolü WebSocket standart izler [IETF RFC 6455](https://tools.ietf.org/html/rfc6455). WebSocket bağlantısı HTTP semantiği kullanmak yerine bir WebSocket bağlantı yükseltmek için istemcinin desire belirtmek HTTP üst bilgilerini içeren bir HTTP isteği olarak başlar. Bir HTTP döndürerek WebSocket bağlantı katılmak için konusundaki istek sunucuyu gösterir `101 Switching Protocols` yanıt. Bu el sıkışma exchange sonra hem istemci hem de hizmet yuva açık tutmak ve bilgi göndermek ve almak için bir ileti tabanlı protokolü kullanarak başlayın.

WebSocket el sıkışma başlamak için istemci uygulaması hizmete bir HTTPS GET isteği gönderir. Konuşma özgü diğer üstbilgileri birlikte standart WebSocket yükseltme üstbilgilerini içerir.

```HTTP
GET /speech/recognition/interactive/cognitiveservices/v1 HTTP/1.1
Host: speech.platform.bing.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
Authorization: t=EwCIAgALBAAUWkziSCJKS1VkhugDegv7L0eAAJqBYKKTzpPZOeGk7RfZmdBhYY28jl&p=
X-ConnectionId: A140CAF92F71469FA41C72C7B5849253
Origin: https://speech.platform.bing.com
```

Hizmet yanıt verir:

```HTTP
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: upgrade
Sec-WebSocket-Key: 2PTTXbeeBXlrrUNsY15n01d/Pcc=
Set-Cookie: SpeechServiceToken=AAAAABAAWTC8ncb8COL; expires=Wed, 17 Aug 2016 15:39:06 GMT; domain=bing.com; path="/"
Date: Wed, 17 Aug 2016 15:03:52 GMT
```

Tüm konuşma istekleri gerektiren [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) şifreleme. Şifrelenmemiş konuşma istekleri kullanımı desteklenmiyor. Aşağıdaki TLS sürümü desteklenir:

* TLS 1.2

### <a name="connection-identifier"></a>Bağlantı tanımlayıcısı

Konuşma hizmeti, tüm istemcilerin bağlantıyı tanımlamak için benzersiz bir kimliği eklemenizi gerektirir. İstemcileri *gerekir* dahil *X ConnectionID* WebSocket el sıkışma başlattığınızda üstbilgi. *X ConnectionID* üstbilgi olmalıdır bir [evrensel benzersiz tanımlayıcı](https://en.wikipedia.org/wiki/Universally_unique_identifier) (UUID) değeri. İçermeyen WebSocket yükseltme istekleri *X ConnectionID*, için bir değer belirtmezseniz *X ConnectionID* , üst veya geçerli bir içerme bir HTTP hizmetiyletarafındanreddedilenUUIDdeğer`400 Bad Request` yanıt.

### <a name="authorization"></a>Yetkilendirme

Standart WebSocket el sıkışma üstbilgilerini yanı sıra konuşma istekleri gerektiren bir *yetkilendirme* üstbilgi. Bağlantı isteklerini bu başlığı reddedilir olmadan bir HTTP ile hizmeti tarafından `403 Forbidden` yanıt.

*Yetkilendirme* üstbilgi bir JSON Web Token (JWT) erişim belirteci içermesi gerekir.

Abone olmak ve geçerli JWT erişim belirteçleri almak için kullanılan API anahtarlarını almak hakkında daha fazla bilgi için bkz: [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası.

API anahtarını belirteç hizmetine geçirilir. Örneğin:

``` HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

Aşağıdaki üst bilgileri için belirteç erişimi gereklidir.

| Ad | Biçimlendir | Açıklama |
|----|----|----|
| Ocp Apim abonelik anahtarı | ASCII | Abonelik anahtarınız |

JWT erişim belirteci olarak belirteç hizmetine döndürür `text/plain`. JWT olarak geçirilen sonra bir `Base64 access_token` el sıkışması olarak için bir *yetkilendirme* dizesiyle önekli üstbilgi `Bearer`. Örneğin:

`Authorization: Bearer [Base64 access_token]`

### <a name="cookies"></a>Tanımlama bilgileri

İstemcileri *gerekir* belirtilen HTTP tanımlama bilgilerini destekleyen [RFC 6265](https://tools.ietf.org/html/rfc6265).

### <a name="http-redirection"></a>HTTP yeniden yönlendirmesi

İstemcileri *gerekir* tarafından belirtilen standart yeniden yönlendirme mekanizmaları [HTTP protokolü belirtimi](http://www.w3.org/Protocols/rfc2616/rfc2616.html).

### <a name="speech-endpoints"></a>Konuşma uç noktaları

İstemcileri *gerekir* konuşma hizmeti uygun bir uç noktası kullan. Uç nokta tanıma modu ve dil dayanır. Tablo bazı örnekler göstermektedir.

| Mod | Yol | Hizmet URI'si |
| -----|-----|-----|
| Etkileşimli | /Speech/Recognition/interactive/cognitiveservices/V1 |https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| Konuşma | /Speech/Recognition/Conversation/cognitiveservices/V1 |https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US |
| Yazdırma | /Speech/Recognition/Dictation/cognitiveservices/V1 |https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR |

Daha fazla bilgi için bkz: [hizmeti URI'si](../GetStarted/GetStartedREST.md#service-uri) sayfası.

### <a name="report-connection-problems"></a>Rapor bağlantı sorunları

İstemciler bir bağlantı yaparken karşılaşılan tüm sorunları hemen bildirmeniz gerekir. Raporlama başarısız bağlantıları için ileti Protokolü açıklanan [bağlantı hatası telemetri](#connection-failure-telemetry).

### <a name="connection-duration-limitations"></a>Bağlantı süresi sınırlamaları

Tipik web hizmeti HTTP bağlantıları ile karşılaştırıldığında, WebSocket bağlantıları son bir *uzun* zaman. Konuşma Service sınırlamalar hizmetine WebSocket bağlantı süresi yerleştirir:

 * Etkin bir WebSocket bağlantı için en uzun süresi 10 dakikadır. Hizmet veya istemci o bağlantı üzerinden WebSocket iletileri gönderir, bağlantı durumu etkindir. Sınıra ulaşıldığında hizmet uyarmadan bağlantıyı sonlandırır. İstemcileri veya en yüksek bağlantı yaşam süresi'e yakın etkin kalmasını bağlantı gerektirmeyen bazı kullanıcı senaryoları geliştirmelisiniz.

 * Herhangi bir etkin olmayan WebSocket bağlantısı için en uzun süre 180 saniyedir. Bir bağlantı hizmet ne istemci WebSocket ileti bağlantı üzerinden gönderilen durumunda etkin değil. En fazla etkin olmayan yaşam süresi ulaşıldıktan sonra hizmeti devre dışı WebSocket bağlantıyı sonlandırır.

## <a name="message-types"></a>İleti türleri

İstemci ile hizmet arasında WebSocket bağlantısı kurulduktan sonra hem istemci hem de hizmet iletileri gönderebilir. Bu bölümde bu WebSocket iletiler biçimi açıklanmaktadır.

[IETF RFC 6455](https://tools.ietf.org/html/rfc6455) WebSocket iletileri bir metin veya ikili kodlama kullanılarak veri iletebilir belirtir. İki Kodlamalar farklı üzerinde hat biçimleri kullanın. Her biçim verimli kodlama, iletim ve ileti yükünü çözülmesi için optimize edilmiştir.

### <a name="text-websocket-messages"></a>Metin WebSocket iletileri

Metin WebSocket iletileri üstbilgiler ve HTTP iletileri için kullanılan tanıdık başı çift yeni satır çifti ile ayrılmış bir gövde bölümünü oluşan metin bilgileri yükü getirir. Ve HTTP iletileri gibi metin WebSocket iletileri üstbilgilerinde belirtin *ad: değer* biçimi tek satır başı yeni satır çifti tarafından ayrılmış. Bir metin WebSocket iletideki herhangi bir metin *gerekir* kullanmak [UTF-8](https://tools.ietf.org/html/rfc3629) kodlama.

Metin WebSocket iletileri yol belirtmelisiniz. bir ileti üstbilgisinde *yolu*. Bu üstbilgi değerini daha sonra bu belgede tanımlanan konuşma protokolü ileti türlerinden biri olması gerekir.

### <a name="binary-websocket-messages"></a>İkili WebSocket iletileri

İkili WebSocket iletileri bir ikili yükü getirir. Konuşma hizmeti protokolünde ses için aktarılan ve ikili WebSocket iletileri kullanarak hizmetinden alındı. Diğer tüm iletileri metin WebSocket iletileri edilir. 

Metin WebSocket iletileri gibi bir başlığı ve gövde bölümü ikili WebSocket iletileri oluşur. İlk 2 bayt ikili WebSocket iletisinin belirtin, buna [big endian](https://en.wikipedia.org/wiki/Endianness) sipariş, üstbilgi bölümünün 16 bit tam sayı boyutu. En düşük üstbilgi bölüm boyutu 0 bayt'tır. En büyük boyutu 8.192 bayttır. İkili WebSocket iletilerinin üstbilgisi metinde *gerekir* kullanmak [US-ASCII](https://tools.ietf.org/html/rfc20) kodlama.

Bir ikili WebSocket ileti üstbilgilerinde metin WebSocket iletileri olduğu gibi aynı biçimde kodlanır. *Ad: değer* tek satır başı yeni satır çifti tarafından ayrılmış biçimi. İkili WebSocket iletileri yol belirtmelisiniz. bir ileti üstbilgisinde *yolu*. Bu üstbilgi değerini daha sonra bu belgede tanımlanan konuşma protokolü ileti türlerinden biri olması gerekir.

Metin ve ikili WebSocket iletileri konuşma hizmeti protokolünde kullanılır. 

## <a name="client-originated-messages"></a>İstemci kaynaklanan iletileri

Bağlantı kurulduktan sonra hem istemci hem de hizmet iletileri göndermek başlatabilirsiniz. Bu bölümde biçimini ve istemci uygulamaları konuşma hizmetine gönderdiğiniz iletileri yükünü açıklanmaktadır. Bölüm [hizmeti kaynaklı iletileri](#service-originated-messages) konuşma hizmetinde kaynaklanan ve istemci uygulamaları için gönderilen iletileri gösterir.

Hizmetlere istemci tarafından gönderilen ana iletiler `speech.config`, `audio`, ve `telemetry` iletileri. Biz her ileti ayrıntılı düşünmeden önce bu iletiler ileti üstbilgilerini açıklanan ortak gereklidir.

### <a name="required-message-headers"></a>Gerekli ileti üstbilgileri

Aşağıdaki üst bilgiler, tüm istemci kaynaklanan iletiler için gereklidir.

| Üst bilgi | Değer |
|----|----|
| Yol | Bu belgede belirtilen ileti yolu |
| X-RequestId | "No-tire" biçiminde UUID |
| X-zaman damgası | ISO 8601 biçiminde istemci UTC saat zaman damgası |

#### <a name="x-requestid-header"></a>X-RequestId üstbilgisi

İstemci kaynaklanan istekleri benzersiz şekilde tanımlanır *X RequestId* ileti üstbilgisi. Bu üst istemci kaynaklanan tüm iletiler için gereklidir. *X RequestId* üstbilgi değeri olmalıdır bir UUID "no-tire" formunda, örneğin, *123e4567e89b12d3a456426655440000*. Bu *olamaz* kurallı biçiminde olması *123e4567-e89b-12d3-a456-426655440000*. Olmadan ister bir *X RequestId* üstbilgisi veya biçimi yanlış UUID WebSocket bağlantıyı sonlandırmasına hizmetinin neden için kullandığı bir üstbilgi değeri.

#### <a name="x-timestamp-header"></a>X-zaman damgası üstbilgisi

Konuşma hizmeti için bir istemci uygulaması tarafından gönderilen her ileti *gerekir* içeren bir *X zaman damgası* üstbilgi. Bu üstbilgisi değeri istemci ileti gönderdiğinde saattir. Olmadan ister bir *X zaman damgası* üstbilgisi veya yanlış biçimi kullanan bir üstbilgi değeri ile WebSocket bağlantıyı sonlandırmasına hizmeti neden olabilir.

*X zaman damgası* üstbilgi değeri, 'yyyy'-'MM'-'dd'T' ss biçiminde olmalıdır ': 'mm':'ss '.' fffffffZ' 'fffffff' bir saniyenin olduğu. Örneğin, '12,5', '12 + 5/10 saniye ve '12.526' anlamına gelir ' 12 artı 526/1000 saniye' anlamına gelir. Bu biçim ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve standart HTTP aksine *tarih* üstbilgisi, onu milisaniye çözünürlüğü sağlayabilir. İstemci uygulamaları yakın milisaniye zaman damgaları yuvarlak. İstemci uygulamaları için gereken aygıt saat doğru şekilde zaman kullanarak ilerlediğinden emin olmak bir [ağ zaman Protokolü (NTP) sunucusu](https://en.wikipedia.org/wiki/Network_Time_Protocol).

### <a name="message-speechconfig"></a>İleti `speech.config`

Konuşma hizmet en iyi olası konuşma tanıma sağlamak için uygulamanızın özelliklerini bilmek ister. Gerekli özellikleri veri cihaz ve uygulamanızı'nın temelini oluşturan işletim sistemi hakkında bilgiler içerir. Bu bilgiyi sağlamanız `speech.config` ileti.

İstemcileri *gerekir* Gönder bir `speech.config` hemen bunlar konuşma hizmetine ve herhangi bir göndermeden önce bağlantı kurduktan sonra ileti `audio` iletileri. Göndermesi gerekir. bir `speech.config` bağlantı başına yalnızca bir kez iletisi.

| Alan | Açıklama |
|----|----|
| WebSocket ileti kodlama | Metin |
| Gövde | Yükü JSON yapısındaki olarak |

#### <a name="required-message-headers"></a>Gerekli ileti üstbilgileri

| Üstbilgi adı | Değer |
|----|----|
| Yol | `speech.config` |
| X-zaman damgası | ISO 8601 biçiminde istemci UTC saat zaman damgası |
| Content-Type | Uygulama/json; Charset = utf-8 |

Tüm istemci kaynaklanan iletileri gibi konuşma hizmet protokolü ile `speech.config` ileti *gerekir* içeren bir *X zaman damgası* ileti gönderilme zaman istemci UTC saati süresi kayıtları üstbilgisi için hizmet. `speech.config` İleti *yok* gerektiren bir *X RequestId* üstbilgi bu iletiyi belirli konuşma istekle ilişkili olmadığından.

#### <a name="message-payload"></a>İleti yükü
Yükü `speech.config` iletisidir uygulamayla ilgili bilgileri içeren bir JSON yapısı. Aşağıdaki örnek, bu bilgileri gösterir. İstemci ve cihaz bağlamı bilgileri yer almaktadır *bağlamı* JSON yapısındaki öğesidir. 

```JSON
{
  "context": {
    "system": {
      "version": "2.0.12341",
    },
    "os": {
      "platform": "Linux",
      "name": "Debian",
      "version": "2.14324324"
    },
    "device": {
      "manufacturer": "Contoso",
      "model": "Fabrikan",
      "version": "7.341"
      }
    },
  }
}
```

##### <a name="system-element"></a>Sistem öğesi

System.version öğesinin `speech.config` ileti konuşma istemci uygulaması ya da aygıt tarafından kullanılan SDK yazılım sürümünü içerir. Değer biçimindedir *major.minor.build.branch*. Atlayabilirsiniz *şube* geçerli değilse bileşeni.

##### <a name="os-element"></a>İşletim sistemi öğesi

| Alan | Açıklama | Kullanım |
|-|-|-|
| OS.Platform | İşletim sistemi örneğin uygulamasını barındıran platform, Windows, Android, iOS veya Linux |Gerekli |
| OS.Name | Örneğin, Debian veya Windows 10 işletim sistemi ürün adı | Gerekli |
| OS.Version | Formunda işletim sistemi sürümüne *major.minor.build.branch* | Gerekli |

##### <a name="device-element"></a>Aygıt öğesi

| Alan | Açıklama | Kullanım |
|-|-|-|
| Device.Manufacturer | Aygıt donanım üreticisi | Gerekli |
| Device.model | Cihaz modeli | Gerekli |
| Device.Version | Aygıt üreticisi tarafından sağlanan cihaz yazılım sürümü. Bu değer üretici tarafından izlenen cihaz sürümünü belirtir. | Gerekli |

### <a name="message-audio"></a>İleti `audio`

Konuşma etkinleştirilmiş istemci uygulamaları ses öbekleri bir dizi ses akışı dönüştürerek, konuşma hizmetine ses gönderin. Ses, her bir öbeğin bir parçasını hizmeti tarafından transcribed için konuşulan ses taşır. Tek bir ses öbek en büyük boyutunu 8.192 bayttır. Ses akışı iletiler *ikili WebSocket iletileri*.

İstemcilerin kullandığı `audio` hizmete bir ses öbek gönderilecek ileti. İstemcileri parçalar mikrofon gelen ses okuyun ve bu öbekleri için transcription konuşma hizmetine göndermek. İlk `audio` ileti düzgün ses hizmeti tarafından desteklenen kodlama biçimlerden birine uyan belirtir doğru biçimlendirilmiş bir üstbilgi içermesi gerekir. Ek `audio` iletiler ikili ses akışı yalnızca mikrofon okunan veriler içerir.

İstemciler isteğe bağlı olarak gönderme bir `audio` sıfır uzunluklu gövdesi olan ileti. Bu ileti konuşma kullanıcı durduruldu, utterance tamamlandı ve mikrofon kapalı istemci bilir hizmet söyler.

Konuşma hizmetini kullanan ilk `audio` yeni bir istek/yanıt döngüsü başlangıcı göstermek için bir benzersiz istek tanımlayıcısını içeren ileti veya *kapatma*. Hizmet aldıktan sonra bir `audio` ileti isteğe bağlı olarak yeni bir istek tanımlayıcısı ile tüm önceki Aç ile ilişkili herhangi bir Sıraya alınmış veya gönderilmeyen iletisi atar.

| Alan | Açıklama |
|-------------|----------------|
| WebSocket ileti kodlama | İkili |
| Gövde | Ses öbek ikili verileri. En büyük boyutu 8.192 bayttır. |

#### <a name="required-message-headers"></a>Gerekli ileti üstbilgileri

Aşağıdaki üst bilgiler tüm gerekli `audio` iletileri.

| Üst bilgi         |  Değer     |
| ------------- | ---------------- |
| Yol | `audio` |
| X-RequestId | "No-tire" biçiminde UUID |
| X-zaman damgası | ISO 8601 biçiminde istemci UTC saat zaman damgası |
| Content-Type | Ses içerik türü. Tür ya da olmalıdır *ses/x-wav* (PCM) veya *ses/silk* (SILK). |

#### <a name="supported-audio-encodings"></a>Desteklenen ses kodlamaları

Bu bölümde konuşma hizmeti tarafından desteklenen ses codec bileşenleri açıklanmaktadır.

##### <a name="pcm"></a>PCM

Konuşma hizmet sıkıştırılmamış darbeli kod modülasyon (PCM) ses kabul eder. Ses hizmetine gönderilir [WAV](https://en.wikipedia.org/wiki/WAV) ilk ses öbek biçimde biçimlendirmek *gerekir* geçerli bir içeren [Kaynak Değişim dosya biçimi](https://en.wikipedia.org/wiki/Resource_Interchange_File_Format) (RIFF) üstbilgisi. Bir istemci mu ses bir Öbek ile Aç başlatır, *değil* geçerli bir RIFF üstbilgisini, hizmet isteği reddeder ve WebSocket bağlantıyı sonlandırır.

PCM ses *gerekir* örneklenen 16 kHz örnek ve bir kanal başına 16 bit ile (*RIFF-16khz-16 bit-mono-pcm*). Konuşma hizmet stereo ses akışları desteklemez ve belirtilen bit hızı, örnek hızı veya kanal sayısı kullanmayın ses akışları reddeder.

##### <a name="opus"></a>Geçerli

Geçerli bir açık, ücretsiz, çok yönlü ses codec bileşenidir. Konuşma hizmeti geçerli bir sabit bit hızında destekler `32000` veya `16000`. Yalnızca `OGG` geçerli için kapsayıcıyı şu anda desteklenen tarafından belirtilen `audio/ogg` MIME türü.

Geçerli kullanmak için değiştirmeniz [JavaScript örnek](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/master/samples/browser/Sample.html#L101) değiştirip `RecognizerSetup` döndürülecek yöntemi.

```javascript
return SDK.CreateRecognizerWithCustomAudioSource(
            recognizerConfig,
            authentication,
            new SDK.MicAudioSource(
                     new SDK.OpusRecorder(
                     {
                         mimeType: "audio/ogg",
                         bitsPerSecond: 32000
                     }
              )
          ));
```

#### <a name="detect-end-of-speech"></a>Konuşma sonu Algıla

İnsanlar değil açıkça sinyal bunlar tamamladığınızda konuşarak. Girişte bir ses akışını konuşma sonu işleme için iki seçenek var olan konuşma kabul eden herhangi bir uygulama: hizmet konuşma sonu algılama ve istemci konuşma sonu algılama. Bu iki seçenek hizmet konuşma sonu algılama genellikle daha iyi bir kullanıcı deneyimi sağlar.

##### <a name="service-end-of-speech-detection"></a>Hizmet konuşma sonu algılama

İdeal tutmadan konuşma deneyim oluşturmak için kullanıcı konuşarak bittiğinde algılayan hizmet uygulamaları izin verin. İstemcilerin gönderdiği ses mikrofon gelen *ses* hizmeti sessizlik algılar ve geri ile yanıt kadar öbekleri bir `speech.endDetected` ileti.

##### <a name="client-end-of-speech-detection"></a>İstemci konuşma sonu algılama

Bazı şekilde konuşma sonu sinyal kullanıcıya izin istemci uygulamaları da verebilirsiniz hizmet, sinyal. Örneğin, bir istemci uygulaması "Durdur" ya da kullanıcı basabilirsiniz "Sessiz" düğmesi olabilir. İstemci uygulamaları konuşma bitiş sinyali göndermek bir *ses* öbek iletisiyle sıfır uzunluklu gövdesi. Konuşma hizmeti bu ileti gelen ses akışın sonuna olarak yorumlar.

### <a name="message-telemetry"></a>İleti `telemetry`

İstemci uygulamaları *gerekir* konuşma hizmetine Aç hakkında telemetriyi göndererek her Aç sonuna bildiremedi. Bırakma uç bildirim konuşma istek ve yanıt tamamlanması için gerekli tüm iletileri düzgün istemci tarafından alınan emin olmak hizmet sağlar. Dönüş son bildirimi de Konuşma istemci uygulamaları beklendiği şekilde çalıştığını doğrulamak hizmet sağlar. Bu bilgiler, konuşma etkinleştirilmiş uygulamanız sorun gidermek için yardıma ihtiyacınız varsa çok yararlı olur.

İstemcileri gerekir kabul bir dönüş sonuna göndererek bir `telemetry` iletisi alındıktan hemen sonra bir `turn.end` ileti. İstemcileri kabul dener `turn.end` mümkün olan en kısa sürede. Dönüş son onaylamak bir istemci uygulaması başarısız olursa, konuşma hizmeti bir hata ile bağlantı sonlandırabilir. İstemcileri yalnızca bir gönderme gerekir `telemetry` her istek ve yanıt tarafından tanımlanan ileti *X RequestId* değeri.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Metin |
| Yol | `telemetry` |
| X-zaman damgası | ISO 8601 biçiminde istemci UTC saat zaman damgası |
| Content-Type | `application/json` |
| Gövde | Aç istemci bilgilerini içeren bir JSON yapısı |

Gövdesi için şema `telemetry` ileti tanımlanmış [Telemetri şema](#telemetry-schema) bölümü.

#### <a name="telemetry-for-interrupted-connections"></a>Telemetri kesintiye uğramış bağlantıları için

Ağ bağlantısı, bırakma sırasında herhangi bir nedenle başarısız olursa ve istemci mu *değil* alma bir `turn.end` ileti hizmetinden istemci gönderir bir `telemetry` ileti. Bu ileti başarısız istek istemci hizmetine bir bağlantı yapar sonraki açışınızda açıklar. İstemcileri zorunda değilsiniz hemen göndermek için bir bağlantı girişimi `telemetry` ileti. İleti istemcide arabelleğe ve gelecekteki bir kullanıcı tarafından istenen bağlantı üzerinden gönderilir. `telemetry` İçin başarısız istek iletisi *gerekir* kullanmak *X RequestId* başarısız istek değeri. Göndermek veya diğer iletileri almak için beklemeden bir bağlantı oluşturulur hemen sonra hizmete gönderilen.

## <a name="service-originated-messages"></a>Hizmet kaynaklanan iletileri

Bu bölümde konuşma hizmetinde kaynaklanan ve istemciye gönderilen iletileri açıklanmaktadır. Konuşma hizmeti bir kayıt defteri istemci özelliklerini tutar ve her istemci tarafından şekilde gerekli olmayan tüm istemciler burada açıklanan tüm iletileri almak mesajları oluşturur. Konuyu uzatmamak amacıyla, iletileri değeri tarafından başvurulan *yolu* üstbilgi. Örneğin, biz WebSocket bir mesaj başvurmak *yolu* değeri `speech.hypothesis` olarak speech.hypothesis iletisi.

### <a name="message-speechstartdetected"></a>İleti `speech.startDetected`

`speech.startDetected` İleti, konuşma hizmet Ses akışında konuşma algıladı gösterir.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Metin |
| Yol | `speech.startDetected` |
| Content-Type | Uygulama/json; Charset = utf-8 |
| Gövde | Konuşma başlangıcı algılandığında koşullarla ilgili bilgiler içeren JSON yapısı. *Uzaklık* bu yapı alanında belirtir (100 nanosaniyelik birimlerindeki) uzaklığı zaman konuşma algılandı Ses akışında, akış başlangıç göre. |

#### <a name="sample-message"></a>Örnek ileti

```HTML
Path: speech.startDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 100000
}
```

### <a name="message-speechhypothesis"></a>İleti `speech.hypothesis`

Konuşma tanıma sırasında Konuşma hizmeti varsayımlar sözcükler hakkında düzenli olarak kabul edilen hizmet oluşturur. Konuşma hizmeti bu varsayımlar yaklaşık her 300 milisaniye istemciye gönderir. `speech.hypothesis` Uygun *yalnızca* kullanıcı konuşma deneyimini geliştirmek için. İçerik veya metin doğruluğunu bağımlılıkları bu iletileri gerçekleştirmeniz gereken değil.

 `speech.hypothesis` İletisidir bazı metin işleme yeteneği olan ve konuşarak kişiye sürüyor tanıma yakın gerçek zamanlı geribildirim sağlamak istiyorsanız bu istemciler için geçerlidir.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Metin |
| Yol | `speech.hypothesis` |
| X-RequestId | "No-tire" biçiminde UUID |
| Content-Type | uygulama/json |
| Gövde | Konuşma varsayımınızın JSON yapısı |

#### <a name="sample-message"></a>Örnek ileti

```HTML
Path: speech.hypothesis
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Text": "this is a speech hypothesis",
  "Offset": 0,
  "Duration": 23600000
}
```

*Uzaklık* öğesi belirtir (100 nanosaniyelik birimlerindeki) uzaklığı zaman tümcecik tanınan, ses akışı başlangıç göre.

*Süresi* öğesi bu konuşma deyimi (100 nanosaniyelik birimleri) süresini belirtir.

İstemcileri sıklığı, zamanlama veya bir konuşma varsayımınızın veya her iki konuşma varsayımlar metinde tutarlılığını bulunan metin dair varsayımlar yapmamalısınız. Varsayımlar hizmet transcription işlemine yalnızca anlık görüntüler değildir. Transcription kararlı toplamı temsil etmiyor. Örneğin, ilk konuşma varsayımınızın "ince eğlenceli" sözcükler içerebilir ve İkinci varsayım "komik bulunamıyor." sözcüklerini içerebilir Konuşma hizmet konuşma varsayımınızın metinde üzerinde (örneğin, büyük/küçük harf, noktalama) sonrası işlemi gerçekleştirmez.

### <a name="message-speechphrase"></a>İleti `speech.phrase`

Konuşma hizmeti zaman belirler hizmet üretir değişmeyeceği tanıma sonucu oluşturmak için yeterli bilgiye sahip bir `speech.phrase` ileti. Kullanıcı bir cümle veya tümcecik tamamlandığını algılandıktan sonra konuşma hizmeti bu sonuçlar üretir.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Metin |
| Yol | `speech.phrase` |
| Content-Type | uygulama/json |
| Gövde | Konuşma tümcecik JSON yapısı |

Konuşma tümcecik JSON Şeması aşağıdaki alanları içerir: `RecognitionStatus`, `DisplayText`, `Offset`, ve `Duration`. Bu alanlar hakkında daha fazla bilgi için bkz: [Transcription yanıtları](../concepts.md#transcription-responses).

#### <a name="sample-message"></a>Örnek ileti

```HTML
Path: speech.phrase
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": 0,
  "Duration": 12300000
}
```

### <a name="message-speechenddetected"></a>İleti `speech.endDetected`

`speech.endDetected` İleti istemci uygulaması hizmetine ses akışı durması gerektiğini belirtir.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Metin |
| Yol | `speech.endDetected` |
| Gövde | Konuşma sonu algılandığında uzaklık içeren JSON yapısı. Tanıma için kullanılan ses başından uzaklık 100 nanosaniyelik birimleri uzaklığı temsil edilir. |
| Content-Type | Uygulama/json; Charset = utf-8 |

#### <a name="sample-message"></a>Örnek ileti

```HTML
Path: speech.endDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 0
}
```

*Uzaklık* öğesi belirtir (100 nanosaniyelik birimlerindeki) uzaklığı zaman tümcecik tanınan, ses akışı başlangıç göre.

### <a name="message-turnstart"></a>İleti `turn.start`

`turn.start` Bir dönüş başlangıcı hizmet perspektifinden işaret eder. `turn.start` İletisidir her zaman için herhangi bir istek aldığınız ilk yanıt iletisi. Almazsanız, bir `turn.start` iletisi, hizmet bağlantı durumunun geçersiz olduğunu varsayın.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Metin |
| Yol | `turn.start` |
| Content-Type | Uygulama/json; Charset = utf-8 |
| Gövde | JSON yapısı |

#### <a name="sample-message"></a>Örnek ileti

```HTML
Path: turn.start
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "context": {
    "serviceTag": "7B33613B91714B32817815DC89633AFA"
  }
}
```

Gövdesini `turn.start` iletisidir Aç başlangıcı bağlamının içeren bir JSON yapısı. *Bağlamı* öğesi içeren bir *serviceTag* özelliği. Bu özellik service Aç ile ilişkili bir etiket değeri belirtir. Hataları, uygulamanızda sorun gidermek için yardıma ihtiyacınız varsa bu değer Microsoft tarafından kullanılabilir.

### <a name="message-turnend"></a>İleti `turn.end`

`turn.end` Hizmet perspektifinden bir dönüş sonuna işaret eder. `turn.end` İletisidir her zaman için herhangi bir istek aldığınız son yanıt iletisi. İstemciler bu iletinin alınması, bir sinyal temizleme etkinlikleri ve boşta durumuna geçiş için olarak kullanabilirsiniz. Almazsanız, bir `turn.end` iletisi, hizmet bağlantı durumunun geçersiz olduğunu varsayın. Bu durumlarda, var olan bağlantıyı hizmetine kapatın ve yeniden bağlayın.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Metin |
| Yol | `turn.end` |
| Gövde | None |

#### <a name="sample-message"></a>Örnek ileti

```HTML
Path: turn.end
X-RequestId: 123e4567e89b12d3a456426655440000
```

## <a name="telemetry-schema"></a>Telemetri şeması

Gövdesini *telemetri* iletisidir bir dönüş veya denenen bağlantısı ile ilgili istemci bilgilerini içeren bir JSON yapısı. Yapı istemci olaylarını ortaya çıktığında kayıt istemci zaman damgaları yapılır. Her zaman damgası "X-zaman damgası üstbilgisi." başlıklı bölümde açıklandığı gibi ISO 8601 biçiminde olması gerekir *Telemetri* , belirlemezseniz tüm gerekli alanların JSON yapısındaki veya doğru zaman damgası biçimi kullanmayın iletileri istemciye bağlantıyı sonlandırmasına hizmeti neden olabilir. İstemcileri *gerekir* tüm gerekli alanlar için geçerli değerler sağlayın. İstemcileri *gereken* uygun olan her durumda isteğe bağlı alanları için değerler. Bu bölümdeki örnekte gösterilen yalnızca gösterim amacıyla değerlerdir.

Telemetri şema, aşağıdaki bölümlere ayrılmıştır: alınan ileti zaman damgaları ve ölçümleri. Biçim ve her bölümünün kullanımı aşağıdaki bölümlerde belirtilmiş.

### <a name="received-message-time-stamps"></a>Alınan ileti zaman damgaları

İstemcilerin başarıyla hizmete bağlandıktan sonra aldıkları tüm iletiler için giriş saat değerlerini içermelidir. Bu değerler zaman kaydetmelisiniz olduğunda istemci *alınan* ağdan her ileti. Değeri başka bir zaman kaydetmelisiniz değil. Örneğin, istemci zaman kayıt zaman, *işlem* ileti üzerinde. Alınan ileti zaman damgaları bir dizi içinde belirtilen *ad: değer* çiftleri. Çift adını belirtir *yolu* iletinin değeri. Değer çiftinin iletisi alındığında istemci saati belirtir. Veya, belirtilen adda birden fazla iletisi alındı, değer çiftinin bu iletileri alındığında gösteren zaman damgaları dizisi.

```JSON
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.172Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ]
```

İstemcileri *gerekir* JSON gövdesine bu iletilerin zaman damgaları dahil olmak üzere hizmet tarafından gönderilen tüm iletiler alınmasını bildiremedi. Bir iletinin alınması onaylamak bir istemci başarısız olursa, hizmet bağlantı sonlandırabilir.

### <a name="metrics"></a>Ölçümler

İstemciler bir istek süresi boyunca oluşan olaylarla ilgili bilgileri içermelidir. Aşağıdaki ölçümleri desteklenir: `Connection`, `Microphone`, ve `ListeningTrigger`.

### <a name="metric-connection"></a>Ölçüm `Connection`

`Connection` Ölçüm istemci tarafından bağlantı girişimleri ayrıntılarını belirtir. WebSocket bağlantısı başlatıldığında ve tamamlandı ölçüm zaman damgaları, eklemeniz gerekir. `Connection` Metrik gereklidir *yalnızca ilk Aç bağlantısının için*. Bu bilgileri içerecek şekilde sonraki kapatır gerekli değildir. Bir bağlantı kurulmadan önce bir istemci birden çok bağlantı denemeleri yaparsa hakkında bilgi *tüm* bağlantı denemelerinin eklenmelidir. Daha fazla bilgi için bkz: [bağlantı hatası telemetri](#connection-failure-telemetry).

| Alan | Açıklama | Kullanım |
| ----- | ----------- | ----- |
| Ad | `Connection` | Gerekli |
| Kimlik | İçinde kullanılan bağlantı tanımlayıcı değeri *X ConnectionID* Bu bağlantı isteği üstbilgisi | Gerekli |
| Başlatma | İstemci bağlantı isteği zaman gönderdiği saati | Gerekli |
| Son | Zaman istemci bağlantısı başarıyla kuruldu bildirim alındığında veya hata durumlarında reddedildi, reddedildi veya başarısız | Gerekli |
| Hata | Varsa, oluşan hata açıklaması. Bağlantı başarılı olursa, istemcilerin bu alan atlayın. Bu alan uzunluğu en fazla 50 karakter uzunluğunda olabilir. | Aksi takdirde atlanmış hata durumları için gerekli |

Hata açıklaması en fazla 50 karakter uzunluğunda olmalıdır ve ideal olarak aşağıdaki tabloda listelenen değerlerden biri olmalıdır. Hata koşulu şu değerlerden biri eşleşmiyorsa, istemciler hata koşulu kısa bir açıklamasını kullanarak kullanabilir [CamelCasing](https://en.wikipedia.org/wiki/Camel_case) boşluk olmadan. Gönderme olanağı bir *telemetri* ileti hizmetine, böylece yalnızca geçici bir bağlantı gerektiriyor veya geçici hata koşulları bildirilen içinde *telemetri* ileti. Hata koşulları *kalıcı olarak* hizmetine bir bağlantı kurarak bir istemcinin engelleme istemci hizmeti için herhangi bir iletisi göndermesinin dahil olmak üzere *telemetri* iletileri.

| Hata | Kullanım |
| ----- | ----- |
| DNSfailure | İstemci, ağ yığınındaki DNS hatası nedeniyle hizmetine bağlanamadı. |
| NoNetwork | İstemci bağlantı çalıştı ancak ağ yığınını fiziksel ağ kullanılabilir olduğunu bildirdi. |
| NoAuthorization | İstemci bağlantısı, bağlantı için bir yetki belirteci almaya çalışırken başarısız oldu. |
| NoResources | İstemci, bir bağlantı kurmayı çalışırken yerel bazı kaynaklar (örneğin bellek) yetersiz kaldı. |
| Yasak | İstemci hizmeti bir HTTP döndürdüğünden hizmete bağlanamadı `403 Forbidden` WebSocket yükseltme isteği durum kodu. |
| Yetkilendirilmemiş | İstemci hizmeti bir HTTP döndürdüğünden hizmete bağlanamadı `401 Unauthorized` WebSocket yükseltme isteği durum kodu. |
| BadRequest | İstemci hizmeti bir HTTP döndürdüğünden hizmete bağlanamadı `400 Bad Request` WebSocket yükseltme isteği durum kodu. |
| ServerUnavailable | İstemci hizmeti bir HTTP döndürdüğünden hizmete bağlanamadı `503 Server Unavailable` WebSocket yükseltme isteği durum kodu. |
| ServerError | İstemci hizmet döndürdüğünden hizmete bağlanamadı bir `HTTP 500` WebSocket yükseltme isteği iç hata durum kodu. |
| Zaman Aşımı | İstemcinin bağlantı isteği hizmetinden bir yanıt olmadan zaman aşımına uğradı. *Son* alan ne zaman istemci zaman aşımına uğradı ve bağlantı için bekleme durduruldu saati içerir. |
| ClientError | İstemci bağlantısı bazı iç istemci hata nedeniyle sonlandırıldı. | 

### <a name="metric-microphone"></a>Ölçüm `Microphone`

`Microphone` Ölçüm için tüm konuşma kapatır gereklidir. Bu ölçüm sırasında ses giriş etkin bir konuşma istek için kullanılan istemci süreyi ölçer.

Aşağıdaki örnekler, kayıt için kılavuz olarak kullanın *Başlat* saat değerlerini `Microphone` istemci uygulamanızda ölçüm:

* Bir istemci uygulaması, bir kullanıcı mikrofon başlamak için fiziksel bir düğmeyi basmalısınız gerektirir. Düğmesine basın sonra istemci uygulaması mikrofon girdiyi okur ve konuşma hizmetine gönderir. *Başlat* değerini `Microphone` ölçüm mikrofon başlatıldı ve giriş sağlamak hazır olduğunda Düğme basma sonraki süreyi kaydeder. *Son* değerini `Microphone` ölçüm kayıtları ne zaman istemci uygulaması durduruldu aldığı sonra hizmetine ses akışı zaman `speech.endDetected` hizmetinden alınan ileti.

* Bir istemci uygulaması "her zaman" dinlediği bir anahtar sözcüğü spotter kullanır. Yalnızca bir konuşma tetikleyici deyimi anahtar sözcüğü spotter algılandıktan sonra istemci uygulaması giriş mikrofon toplamak ve konuşma hizmetine göndermek. *Başlat* değerini `Microphone` ölçüm zaman anahtar sözcüğü spotter bildirim mikrofon girişten kullanmaya başlamak için istemci zaman kaydeder. *Son* değerini `Microphone` ölçüm kayıtları ne zaman istemci uygulaması durduruldu aldığı sonra hizmetine ses akışı zaman `speech.endDetected` hizmetinden alınan ileti.

* Bir istemci uygulaması için sabit bir ses akışını erişimi vardır ve bu ses akışını sessizlik/Konuşma Algılama gerçekleştiren bir *Konuşma Algılama Modülü*. *Başlat* değerini `Microphone` ölçüm kayıtları zaman zaman *Konuşma Algılama Modülü* giriş ses akıştan kullanmaya başlamak için İstemci bildirim. *Son* değerini `Microphone` ölçüm kayıtları ne zaman istemci uygulaması durduruldu aldığı sonra hizmetine ses akışı zaman `speech.endDetected` hizmetinden alınan ileti.

* Bir istemci uygulaması ikinci Aç çok bırakma isteği işliyor ve giriş için ikinci bırakma toplamak için mikrofon etkinleştirmek için bir hizmet yanıt iletisi tarafından bilgilendirilir. *Başlat* değerini `Microphone` ölçüm kayıtları zaman zaman istemci uygulaması mikrofon sağlar ve bu ses kaynaktan giriş kullanarak başlatır. *Son* değerini `Microphone` ölçüm kayıtları ne zaman istemci uygulaması durduruldu aldığı sonra hizmetine ses akışı zaman `speech.endDetected` hizmetinden alınan ileti.

*Son* saat değeri için `Microphone` ölçüm ne zaman istemci uygulaması durdurulma ses giriş akış zamanı kaydeder. Çoğu durumda, bu olay, alınan kısa bir süre sonra istemcinin gerçekleşir `speech.endDetected` hizmetinden alınan ileti. İstemci uygulamaları doğrulayın bunlar düzgün protokol, sağlayarak uyumsuz emin *son* saat değeri için `Microphone` ölçüm oluşur için giriş saati değerden daha sonra `speech.endDetected` ileti. Ve genellikle bir dönüş sonuna başka bir dönüş başlangıcı arasında bir gecikme olduğundan, istemciler Protokolü uygunluk, sağlayarak doğrulayabilir *Başlat* süresi `Microphone` ölçüm tüm sonraki bırakma için doğru zamanı kaydeder, istemci *başlatılan* hizmetine ses giriş akışı mikrofonun kullanarak.

| Alan | Açıklama | Kullanım |
| ----- | ----------- | ----- |
| Ad | Mikrofon | Gerekli |
| Başlatma | Ses kullanarak istemciyi yeniden başlatıldığı zaman mikrofon veya diğer ses akışı giriş veya bir tetikleyici anahtar sözcüğü spotter alınan | Gerekli |
| Son | Ne zaman istemci mikrofon veya ses akışı kullanarak durdurulma zamanı | Gerekli |
| Hata | Varsa, oluşan hata açıklaması. Mikrofon işlemleri başarılı olursa, istemcilerin bu alan atlayın. Bu alan uzunluğu en fazla 50 karakter uzunluğunda olabilir. | Aksi takdirde atlanmış hata durumları için gerekli |

### <a name="metric-listeningtrigger"></a>Ölçüm `ListeningTrigger`
`ListeningTrigger` Ölçüm zaman kullanıcı yürütür Konuşma giriş başlatır eylem zaman ölçer. `ListeningTrigger` Ölçüm isteğe bağlı, ancak bu ölçüm sağlayan istemciler Bunu yapmak için önerilir.

Aşağıdaki örnekler, kayıt için kılavuz olarak kullanın *Başlat* ve *son* saat değerlerini `ListeningTrigger` istemci uygulamanızda ölçüm.

* Bir istemci uygulaması, bir kullanıcı mikrofon başlamak için fiziksel bir düğmeyi basmalısınız gerektirir. *Başlat* Düğme basma zaman bu ölçüm kayıtları için bir değer. *Son* değeri Düğme basma bittiğinde saati kaydeder.

* Bir istemci uygulaması "her zaman" dinlediği bir anahtar sözcüğü spotter kullanır. Sonra anahtar sözcüğü bir konuşma tetikleyici deyimi spotter algılar istemci uygulaması mikrofon girdiyi okur ve konuşma hizmetine gönderir. *Başlat* anahtar sözcüğü spotter tetikleyici tümcecik sonra algılandı ses alındığında zaman bu ölçüm kayıtları için bir değer. *Son* değeri zaman son tetikleyici tümceciği konuşma kullanıcı tarafından zaman kaydeder.

* Bir istemci uygulaması için sabit bir ses akışını erişimi vardır ve bu ses akışını sessizlik/Konuşma Algılama gerçekleştiren bir *Konuşma Algılama Modülü*. *Başlat* zaman bu ölçüm kayıtları için bir değer, *Konuşma Algılama Modülü* alınan konuşma sonra algılandı ses. *Son* değeri kaydeder zaman zaman *Konuşma Algılama Modülü* konuşma algılandı.

* Bir istemci uygulaması ikinci Aç çok bırakma isteği işliyor ve giriş için ikinci bırakma toplamak için mikrofon etkinleştirmek için bir hizmet yanıt iletisi tarafından bilgilendirilir. İstemci uygulaması gereken *değil* içeren bir `ListeningTrigger` bu bırakma için ölçüm.

| Alan | Açıklama | Kullanım |
| ----- | ----------- | ----- |
| Ad | ListeningTrigger | İsteğe bağlı |
| Başlatma | İstemci dinleme tetikleyici başlatıldığı zaman | Gerekli |
| Son | İstemci dinleme tetikleyici bittiği zaman | Gerekli |
| Hata | Varsa, oluşan hata açıklaması. Tetikleyici işlemi başarılı olursa, istemcilerin bu alan atlayın. Bu alan uzunluğu en fazla 50 karakter uzunluğunda olabilir. | Aksi takdirde atlanmış hata durumları için gerekli |

#### <a name="sample-message"></a>Örnek ileti

Aşağıdaki örnek ReceivedMessages ve ölçümleri bölümleri içeren bir telemetri iletisi gösterir:

```HTML
Path: telemetry
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000
X-Timestamp: 2016-08-16T15:03:54.183Z

{
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.171Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ],
  "Metrics": [
    {
      "Name": "Connection",
      "Id": "A140CAF92F71469FA41C72C7B5849253",
      "Start": "2016-08-16T15:03:47.921Z",
      "End": "2016-08-16T15:03:48.000Z",
    },
    {
      "Name": "ListeningTrigger",
      "Start": "2016-08-16T15:03:48.776Z",
      "End": "2016-08-16T15:03:48.777Z",
    },
    {
      "Name": "Microphone",
      "Start": "2016-08-16T15:03:47.921Z",
      "End": "2016-08-16T15:03:51.921Z",
    },
  ],
}
```

## <a name="error-handling"></a>Hata işleme

Bu bölümde, hata iletileri ve işlemek için uygulamanız gereken koşulları türleri açıklanmaktadır.

### <a name="http-status-codes"></a>HTTP durum kodları

WebSocket yükseltme isteği sırasında Konuşma hizmeti herhangi bir standart HTTP durum kodları gibi döndürebilir `400 Bad Request`, vb. Uygulamanız bu hata koşulları düzgün şekilde işlemelidir.

#### <a name="authorization-errors"></a>Yetkilendirme hataları

WebSocket yükseltme sırasında Konuşma hizmeti bir HTTP döndürür sağlanan yanlış yetkilendirme ise `403 Forbidden` durum kodu. Bu hata kodu tetikleyebilir koşullar arasında şunlardır:

* Eksik *yetkilendirme* üstbilgisi

* Geçersiz kimlik doğrulama belirteci

* Süresi dolan yetkilendirme belirteci

`403 Forbidden` Hata iletisi konuşma hizmeti ile bir sorun olduğunun göstergesi değil. Bu hata iletisini istemci uygulaması bir sorun olduğunu gösterir.

### <a name="protocol-violation-errors"></a>Protokolü ihlali hataları

Konuşma hizmeti bir istemciden herhangi bir protokolü ihlali algılarsa, hizmet döndürmeden sonra WebSocket bağlantıyı sonlandırır bir *durum kodu* ve *neden* sonlandırma için. İstemci uygulamaları, sorun giderme ve ihlallerini düzeltmek için bu bilgileri kullanabilir.

#### <a name="incorrect-message-format"></a>Hatalı ileti biçimi

Bir istemci bir metin veya ikili ileti bu belirtiminde verilen doğru biçimde kodlanmamış hizmetine gönderirse, hizmeti ile bağlantıyı kapatır bir *1007 geçersiz yükü veri* durum kodu. 

Hizmet, aşağıdaki örneklerde gösterildiği gibi çeşitli nedenlerle, bu durum kodunu döndürür:

* "Hatalı ileti biçimi. İkili ileti geçersiz üstbilgi boyutu öneke sahip." İstemci bir geçersiz üstbilgi boyutu öneke sahip ikili bir ileti gönderdi.

* "Hatalı ileti biçimi. İkili ileti geçersiz üstbilgi boyutuna sahiptir." İstemcisi belirtilen geçersiz üstbilgi boyutu ikili bir ileti gönderilir.

* "Hatalı ileti biçimi. İkili ileti üstbilgilerini UTF-8 kod çözme başarısız oldu." İstemci doğru UTF-8'de kodlanmış değil üstbilgiler içeren bir ikili ileti gönderdi.

* "Hatalı ileti biçimi. SMS mesajı veri içermiyor." İstemcisi, gövde verileri içeren bir SMS mesajı gönderilir.

* "Hatalı ileti biçimi. SMS mesajı UTF-8 kod çözme başarısız oldu." İstemcisi UTF-8'de doğru bir şekilde kodlanmamış bir SMS mesajı gönderilir.

* "Hatalı ileti biçimi. Metin iletisi başlığı ayırıcı içerir." İstemcisi bir üstbilgi ayırıcı içermiyordu veya yanlış üstbilgi ayırıcı kullanılan bir SMS mesajı gönderilir.

#### <a name="missing-or-empty-headers"></a>Eksik veya boş üstbilgileri

Bir istemci gerekli üstbilgileri sahip olmayan bir ileti gönderirse *X RequestId* veya *yolu*, hizmeti ile bağlantıyı kapatır bir *1002 protokol hatası* durum kodu. İleti "eksik veya boş. başlığıdır {Üstbilgi adı}."

#### <a name="requestid-values"></a>RequestId değerleri

Bir istemci bir ileti gönderirse belirtir bir *X RequestId* üstbilgisi hatalı bir biçimde, hizmet bağlantıyı kapatır ve döndürür bir *1002 protokol hatası* durumu. İletisidir "geçersiz istek. X-RequestId üstbilgi değeri Hayır tire UUID biçiminde belirtilmedi."

#### <a name="audio-encoding-errors"></a>Ses kodlama hataları

Bir istemci bir etkinleştirin ve ses biçimi başlatan bir ses öbek gönderir veya kodlama gerekli belirtime uygun değil, hizmet bağlantıyı kapatır ve döndürür bir *1007 geçersiz yükü veri* durum kodu. İleti kaynak hata kodlama biçimi belirtir.

#### <a name="requestid-reuse"></a>RequestId yeniden kullanma

Bir istemci bu bırakma isteği tanımlayıcıdan yeniden kullanır iletisi gönderirse bir dönüş bittikten sonra hizmet bağlantıyı kapatır ve döndüren bir *1002 protokol hatası* durum kodu. İletisidir "geçersiz istek. İstek tanımlayıcılarını kullanılmasını izin verilmez."

## <a name="connection-failure-telemetry"></a>Bağlantı hatası telemetri

En iyi kullanıcı deneyimini sağlamak için istemciler bir bağlantı içindeki önemli kontrol noktaları zaman damgalarını konuşma hizmeti kullanarak bilgilendirmeniz *telemetri* ileti. İstemcileri çalıştı ancak başarısız bağlantıları hizmet bildirmek derecede önemlidir.

Başarısız olan her bağlantı denemesi için istemcileri oluşturma bir *telemetri* benzersiz bir iletiyle *X RequestId* üstbilgi değeri. İstemci bir bağlantı kuramadı çünkü *ReceivedMessages* JSON gövdesi alanında etmeyebilirsiniz. Yalnızca `Connection` girişi *ölçümleri* alan eklenmiştir. Bu girdi karşılaşıldı hata koşulu yanı sıra başlangıç ve bitiş zaman damgaları içerir.

### <a name="connection-retries-in-telemetry"></a>Telemetri, bağlantı yeniden deneme

İstemcileri ayırt *yeniden deneme* gelen *birden çok bağlantı denemeleri* bağlantı denemesi tetikleyen olayı tarafından. Herhangi bir kullanıcı girişi programlı olarak gerçekleştirilen bağlantı denemeleri yeniden denemeler var. Yanıt kullanıcı girişi olarak yürütülen birden çok bağlantı denemeleri birden çok bağlantı denemeleri ' dir. İstemciler her bir kullanıcı tarafından tetiklenen bağlantı denemesi benzersiz bir vermek *X RequestId* ve *telemetri* ileti. İstemcileri yeniden *X RequestId* programlı yeniden deneme için. Tek bir bağlantı girişimi için birden çok deneme yapılmışsa, yeniden deneme girişimlerinden olarak eklenmiştir bir `Connection` girişi *telemetri* ileti.

Örneğin, bir kullanıcı bir bağlantı başlatmak için anahtar sözcüğü tetikleyici konuşur ve DNS hatalar nedeniyle ilk bağlantı denemesi başarısız varsayalım. Ancak, program aracılığıyla istemci tarafından yapılan ikinci bir deneme başarılı. İstemci, tek bir kullanır, kullanıcıdan ek girişi gerektirmeden istemci bağlantısı denenen çünkü *telemetri* birden çok ileti `Connection` bağlantı açıklamak için girdileri.

Başka bir örnek olarak, bir kullanıcı bir bağlantı başlatmak için anahtar sözcüğü tetikleyici konuşur ve üç denemeden sonra bu bağlantı girişimleri başarısız olur varsayalım. İstemci, hizmete bağlanmaya durakları sağlar ve bir sorun oluştu kullanıcıya bildirir. Kullanıcı daha sonra yeniden anahtar sözcüğü tetikleyici konuşur. Bu süre, istemci hizmete bağlandığında varsayalım. Bağlandıktan sonra istemci hemen gönderir bir *telemetri* üç içeren hizmete iletiye `Connection` bağlantı hataları tanımlar girişleri. Alındıktan sonra `turn.end` iletisi, istemci gönderir başka *telemetri* başarılı bağlantı açıklayan ileti.

## <a name="error-message-reference"></a>Hata iletisi başvurusu

### <a name="http-status-codes"></a>HTTP durum kodları

| HTTP durum kodu | Açıklama | Sorun giderme |
| - | - | - |
| 400 Hatalı istek | İstemcisi yanlış WebSocket bağlantı isteği gönderilir. | Tüm gerekli parametreleri ve HTTP üstbilgileri sağlanan ve değerleri doğru olduğunu denetleyin. |
| 401 Yetkisiz | İstemci gerekli yetkilendirme bilgilerini içermiyordu. | Göndereceğiniz onay *yetkilendirme* WebSocket Bağlantısı'nda başlığı. |
| 403 Yasak | İstemci yetkilendirme bilgilerini gönderilen ancak geçersizdi. | Süresi dolmuş veya geçersiz bir değer göndereceğiniz değil, denetleme *yetkilendirme* üstbilgi. |
| 404 Bulunamadı | İstemci, desteklenmeyen bir URL yolu erişme girişiminde bulunuldu. | WebSocket bağlantısı için doğru URL'yi kullandığınızdan denetleyin. |
| 500 sunucu hatası | Hizmet dahili bir hatayla karşılaştı ve istek karşılayamadı. | Çoğu durumda, bu geçici bir hatadır. İsteği yeniden deneyin. |
| 503 Hizmet Kullanılamıyor | Hizmet isteği işleyemedi. | Çoğu durumda, bu geçici bir hatadır. İsteği yeniden deneyin. |

### <a name="websocket-error-codes"></a>WebSocket hata kodları

| WebSocketsStatus kodu | Açıklama | Sorun giderme |
| - | - | - |
| 1000 normal kapatma | Hizmet bir hata olmadan WebSocket bağlantı kapatıldı. | WebSocket kapatma beklenmeyen olduysa, nasıl ve ne zaman hizmeti WebSocket bağlantısı sonlandırabilir anladığınızdan emin olmak için belgeleri yeniden okuyun. |
| 1002 protokol hatası | İstemci protokolü gereksinimlerine uyacak şekilde başarısız oldu. | Protokolü belgeleri anlamak ve gereksinimleri hakkında açık olduğundan emin olun. Protokolü gereksinimleri ihlal olmadığını görmek için hata nedenleri hakkında önceki belgelerini okuyun. |
| 1007 geçersiz yük verileri | İstemci geçersiz bir yükü bir protokol iletisi gönderdi. | Hatalar için hizmetine gönderilen son ileti denetleyin. Yük hataları hakkında önceki belgelerini okuyun. |
| 1011 sunucu hatası | Hizmet dahili bir hatayla karşılaştı ve istek karşılayamadı. | Çoğu durumda, bu geçici bir hatadır. İsteği yeniden deneyin. |

## <a name="related-topics"></a>İlgili konular

Bkz: bir [JavaScript SDK'sı](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript) olan konuşma hizmet WebSocket tabanlı Protokolü uygulaması.
