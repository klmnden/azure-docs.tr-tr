---
title: Bing konuşma WebSocket Protokolü | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Üzerinde WebSockets Protokolü belgeleri için Bing konuşma tabanlı
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d6601f57d87b518b2061df64174818432b822755
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60515330"
---
# <a name="bing-speech-websocket-protocol"></a>Bing konuşma WebSocket Protokolü

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bing konuşma metin dönüştürme konuşma sesine kullanılabilir en gelişmiş algoritmalar özellikleri bir bulut tabanlı bir platformdur. Bing konuşma protokolü tanımlar [bağlantı kurma](#connection-establishment) istemci uygulamaları, hizmet ve ortaklarınıza arasında alınıp verilen konuşma tanıma iletileri arasındaki ([istemci kaynaklı iletiler](#client-originated-messages) ve [hizmeti kaynaklı iletiler](#service-originated-messages)). Ayrıca, [telemetri iletilerini](#telemetry-schema) ve [hata işleme](#error-handling) açıklanmıştır.

## <a name="connection-establishment"></a>Bağlantı kurma

Konuşma hizmeti Protokolü WebSocket standart bir belirtim izleyen [IETF RFC 6455](https://tools.ietf.org/html/rfc6455). WebSocket bağlantısı HTTP semantiğine kullanmak yerine bir WebSocket bağlantısı yükseltmek için müşterinin isteğine belirten bir HTTP üst bilgilerini içeren bir HTTP isteği olarak başlar. Sunucu HTTP döndürerek WebSocket bağlantı katılmak için konusundaki isteği gösterir `101 Switching Protocols` yanıt. Bu el sıkışması alışverişi sonra hem istemci hem de hizmet yuva açık tutun ve bilgi göndermek ve almak için bir ileti tabanlı iletişim kuralı'ı kullanmaya başlamak.

WebSocket el sıkışması başlamak için istemci uygulama hizmetine bir HTTPS GET isteği gönderir. Bu, konuşma belirli olan diğer üst birlikte standart WebSocket yükseltme üstbilgileri içerir.

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

Tüm sesli istekleri gerektiren [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) şifreleme. Şifrelenmemiş sesli istekleri kullanımı desteklenmiyor. Aşağıdaki TLS sürümü desteklenir:

* TLS 1.2

### <a name="connection-identifier"></a>Bağlantı tanımlayıcı

Konuşma hizmeti, tüm istemcilerin bu bağlantıyı tanımlamak için benzersiz bir kimliği eklemenizi gerektirir. İstemciler *gerekir* dahil *X ConnectionID* bir WebSocket anlaşması'na başlattığınızda başlığı. *X ConnectionID* başlığı olmalıdır bir [evrensel benzersiz tanımlayıcı](https://en.wikipedia.org/wiki/Universally_unique_identifier) (UUID) değeri. İçermeyen WebSocket Yükseltme isteklerini *X ConnectionID*, için bir değer belirtmeyin *X ConnectionID* üst bilgi veya geçerli bir içerme hizmeti ile bir HTTP tarafındanreddedilenUUIDdeğer`400 Bad Request` yanıt.

### <a name="authorization"></a>Yetkilendirme

Ek olarak standart WebSocket el sıkışması üstbilgileri, sesli istekleri gerektiren bir *yetkilendirme* başlığı. Bağlantı isteklerini HTTP hizmetiyle tarafından bu başlığı reddedilir olmadan `403 Forbidden` yanıt.

*Yetkilendirme* üst bilgisi, JSON Web Token (JWT) erişim belirteci içermelidir.

Geçerli JWT erişim belirteçlerini almak için kullanılan API anahtarlarını edinin ve abone olma hakkında daha fazla bilgi için bkz: [Bilişsel hizmetler abonelik](https://azure.microsoft.com/try/cognitive-services/) sayfası.

API anahtarı belirteç hizmetine geçirilir. Örneğin:

``` HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken
Content-Length: 0
```

Aşağıdaki üst bilgi bilgileri için belirteç erişimi gereklidir.

| Ad | Biçimi | Açıklama |
|----|----|----|
| Ocp-Apim-Subscription-Key | ASCII | Abonelik anahtarınız |

JWT belirteci belirteç hizmetine döndürür `text/plain`. Daha sonra JWT olarak geçirilen bir `Base64 access_token` anlaşma için bir *yetkilendirme* dizesiyle ön ekli üst bilgi `Bearer`. Örneğin:

`Authorization: Bearer [Base64 access_token]`

### <a name="cookies"></a>Tanımlama bilgileri

İstemciler *gerekir* belirtildiği gibi HTTP tanımlama bilgilerini desteklediği [RFC 6265](https://tools.ietf.org/html/rfc6265).

### <a name="http-redirection"></a>HTTP yeniden yönlendirmesi

İstemciler *gerekir* tarafından belirtilen standart yönlendirme mekanizmaları [HTTP protokolü belirtimi](https://www.w3.org/Protocols/rfc2616/rfc2616.html).

### <a name="speech-endpoints"></a>Konuşma tanıma uç noktaları

İstemciler *gerekir* konuşma hizmeti uygun bir uç noktasını kullanın. Uç nokta tanıma modu ve dil dayanır. Tablo bazı örnekler göstermektedir.

| Mod | `Path` | Hizmet URI'si |
| -----|-----|-----|
| Etkileşimli | /Speech/Recognition/interactive/cognitiveservices/V1 | https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| Konuşma | /Speech/Recognition/Conversation/cognitiveservices/V1 | https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US |
| Yazdırma | /Speech/Recognition/Dictation/cognitiveservices/V1 | https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR |

Daha fazla bilgi için [hizmet URI'si](../GetStarted/GetStartedREST.md#service-uri) sayfası.

### <a name="report-connection-problems"></a>Rapor bağlantı sorunları

İstemciler bir bağlantı yaparken karşılaştığınız tüm sorunları hemen bildirmelisiniz. Raporlama başarısız bağlantılar için ileti Protokolü açıklanan [bağlantı başarısızlık telemetrilerini](#connection-failure-telemetry).

### <a name="connection-duration-limitations"></a>Bağlantı süresi sınırlamaları

Tipik bir web hizmetinin HTTP bağlantılarıyla karşılaştırıldığında WebSocket bağlantılarını son bir *uzun* zaman. Konuşma hizmeti hizmetine WebSocket bağlantılarını süresi sınırlamalar getirir:

 * Herhangi bir etkin WebSocket bağlantısı için en uzun süresi 10 dakikadır. Bir bağlantı, hizmet veya istemci WebSocket iletileri bu bağlantı üzerinden gönderirse etkindir. Sınıra ulaşıldığında, hizmet uyarmadan bağlantıyı sonlandırır. İstemciler, rezervasyon sırasında veya en yüksek bağlantı ömrü yakın etkin bağlantı gerektirmeyen kullanıcı senaryoları geliştirmeniz gerekir.

 * Etkin olmayan herhangi bir WebSocket bağlantısı için süre üst sınırını 180 saniyedir. Bir bağlantı, hizmet ya da istemci bağlantı üzerinden bir WebSocket ileti gönderdiğinde etkin değil. Etkin olmayan en fazla ömrü ulaşıldıktan sonra hizmeti devre dışı WebSocket bağlantıyı sonlandırır.

## <a name="message-types"></a>İleti türleri

İstemciyi ve hizmeti arasında bir WebSocket bağlantısı kurulduktan sonra hem istemci hem de hizmet iletileri gönderebilir. Bu bölümde, bu WebSocket iletilerin biçimini tanımlar.

[IETF RFC 6455](https://tools.ietf.org/html/rfc6455) bir metin veya ikili kodlama kullanarak WebSocket iletileri veri iletebilir belirtir. İki kodlama farklı-hat üzerinde biçimler kullanır. Her biçim verimli kodlama, iletim ve ileti yükü kod çözme için optimize edilmiştir.

### <a name="text-websocket-messages"></a>Metin WebSocket iletileri

WebSocket sms'ler bir üst bilgiler ve HTTP iletileri için kullanılan bilinen çift satır başı satır çifti tarafından ayrılmış bir gövde bölümünü içeren metin tabanlı bilgiler bir yükü getirir. Ve HTTP iletileri gibi metin WebSocket iletisi üst bilgilerinde belirtin *adı: değer* biçimi tek satır başı satır çifti tarafından ayrılmış. Bir metin WebSocket iletideki herhangi bir metin *gerekir* kullanın [UTF-8](https://tools.ietf.org/html/rfc3629) kodlama.

WebSocket sms'ler ileti yolu üst bilgisinde belirtmelidir *yolu*. Bu üstbilgisinin değerini daha sonra bu belgede tanımlanan konuşma protokolü ileti türlerinden biri olması gerekir.

### <a name="binary-websocket-messages"></a>İkili WebSocket iletileri

İkili yük ikili WebSocket iletileri taşır. Konuşma hizmeti protokolünde ses için gönderilen ve ikili WebSocket iletileri kullanarak hizmetinden alınan. Diğer tüm iletiler sms'ler WebSocket ' dir.

Metin WebSocket iletileri gibi ikili WebSocket iletileri üstbilgi ve gövde bölümü oluşur. İlk 2 baytı ikili WebSocket iletisinin belirtin, buna [büyük endian](https://en.wikipedia.org/wiki/Endianness) sipariş, 16 bit tam sayı boyutu üst bilgisi bölümü. En düşük bölüm boyut 0 bayt'tır. En büyük boyutu 8192 bayttır. İkili WebSocket ileti üst bilgisindeki metin *gerekir* kullanın [US-ASCII](https://tools.ietf.org/html/rfc20) kodlama.

İkili bir WebSocket iletisindeki üst WebSocket sms'ler olduğu gibi aynı biçimde kodlanmış. *Ad: değer* biçimi tek satır başı satır çifti tarafından ayrılmış. İkili WebSocket ileti üstbilgisinde bir ileti yol belirtmelisiniz *yolu*. Bu üstbilgisinin değerini daha sonra bu belgede tanımlanan konuşma protokolü ileti türlerinden biri olması gerekir.

Metin ve ikili WebSocket iletileri hem konuşma hizmeti protokolü kullanılır.

## <a name="client-originated-messages"></a>İstemci kaynaklı iletiler

Bağlantı kurulduktan sonra ileti göndermek hem istemci hem de hizmetini başlatabilirsiniz. Bu bölümde, biçimini ve konuşma hizmeti için istemci uygulamaları Gönder ileti yükü açıklanmaktadır. Bölüm [hizmeti kaynaklı iletiler](#service-originated-messages) konuşma hizmeti kaynaklanan ve istemci uygulamaları için gönderilen iletileri gösterir.

Hizmetlere istemci tarafından gönderilen ana iletiler `speech.config`, `audio`, ve `telemetry` iletileri. Biz, her ileti ayrıntılı düşünmeden önce ortak ileti üstbilgileri bu iletiler için açıklanan gereklidir.

### <a name="required-message-headers"></a>Gerekli ileti üstbilgileri

Aşağıdaki üst bilgiler, tüm istemci kaynaklı iletiler için gereklidir.

| Üstbilgi | Değer |
|----|----|
| `Path` | Bu belgede belirtilen ileti yolu |
| X-RequestId | "No-dash" biçiminde UUID |
| X-zaman damgası | İstemci UTC saati ISO 8601 biçimli zaman damgası |

#### <a name="x-requestid-header"></a>X-RequestId üstbilgisi

İstemci kaynaklı istekler tarafından benzersiz şekilde tanımlanır *X-RequestId* ileti üst bilgisi. Bu üst bilgisi, tüm istemci kaynaklı iletiler için gereklidir. *X-RequestId* üstbilgi değeri olmalıdır bir UUID'ye "no-dash" formunda, örneğin, *123e4567e89b12d3a456426655440000*. Bunu *olamaz* kurallı biçiminde olması *123e4567-e89b-12d3-a456-426655440000*. Olmadan istekleri bir *X-RequestId* üst bilgi veya biçimi yanlış UUID WebSocket bağlantısı sonlandırmak Service'in için kullandığı bir üstbilgi değeri.

#### <a name="x-timestamp-header"></a>X-zaman damgası üstbilgisi

Konuşma hizmeti için bir istemci uygulama tarafından gönderilen her ileti *gerekir* dahil bir *X zaman damgası* başlığı. Bu üst bilgi değeri istemci ileti gönderdiğinde zamandır. Olmadan istekleri bir *X zaman damgası* üst bilgi veya yanlış biçimini kullanan bir üstbilgi değerini WebSocket bağlantısı sona erdirmek hizmeti neden olabilir.

*X zaman damgası* üst bilgi değeri, 'yyyy'-'MM'-'dd'T' HH biçiminde olmalıdır ': 'mm':'ss '.' fffffffZ' 'fffffff', bir saniyenin olduğu. Örneğin, '12,5', '12 + 5/10 saniye ve '12.526' yol ' 12 yanı sıra 526/1000 saniye' anlamına gelir. Bu biçim ile uyumlu [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ve standart HTTP aksine *tarih* üst bilgi, milisaniye çözümleme sağlayabilir. İstemci uygulamaları en yakın milisaniyeye zaman damgaları yuvarlak. İstemci uygulamaları için gereken cihaz saati doğru zaman kullanarak ilerlediğinden emin olmak bir [Ağ Saati Protokolü (NTP) sunucusu](https://en.wikipedia.org/wiki/Network_Time_Protocol).

### <a name="message-speechconfig"></a>İleti `speech.config`

Konuşma hizmeti, en iyi olası konuşma tanıma sağlamak için uygulamanızın özelliklerini bilmek ister. Gerekli özellikleri veriler, uygulamanızı çalıştıran işletim sistemi ve cihaz hakkında bilgi içerir. Bu bilgileri sağlamanız `speech.config` ileti.

İstemciler *gerekir* Gönder bir `speech.config` hemen bunlar konuşma hizmeti ve tüm göndermeden önce bağlantıyı kurduktan sonra ileti `audio` iletileri. Göndermek gereken bir `speech.config` bağlantı yalnızca bir kez iletisi.

| Alan | Açıklama |
|----|----|
| WebSocket ileti kodlama | Text |
| Gövde | Yükü olarak JSON yapısı |

#### <a name="required-message-headers"></a>Gerekli ileti üstbilgileri

| Üst bilgi adı | Değer |
|----|----|
| `Path` | `speech.config` |
| X-zaman damgası | İstemci UTC saati ISO 8601 biçimli zaman damgası |
| İçerik türü | Uygulama/json; Charset = utf-8 |

Tüm istemci kaynaklı iletiler gibi konuşma tanıma hizmeti protokolü ile `speech.config` ileti *gerekir* dahil bir *X zaman damgası* kaydeder olduğunda iletinin gönderildiği istemci UTC saati zamanı üst bilgisi hizmet için. `speech.config` İleti *yok* gerektiren bir *X-RequestId* üst bilgisi bu ileti konuşma belirli bir istekle ilişkili olmadığından.

#### <a name="message-payload"></a>İleti yükü
Yükü `speech.config` iletisidir uygulamayla ilgili bilgileri içeren bir JSON yapısı. Aşağıdaki örnek, bu bilgileri gösterir. İstemci ve cihaz bağlamı bilgilerini dahil *bağlam* JSON yapısı öğesidir.

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

System.version öğesinin `speech.config` ileti, konuşma istemci uygulama veya cihaz tarafından kullanılan SDK yazılım sürümünü içerir. Değer şu şekildedir *major.minor.build.branch*. Atlayabilirsiniz *dal* geçerli değilse bileşeni.

##### <a name="os-element"></a>İşletim sistemi öğesi

| Alan | Açıklama | Kullanım |
|-|-|-|
| OS.Platform | İşletim sistemi örnek uygulamasını barındıran platformu, Windows, Android, iOS veya Linux |Gerekli |
| os.name | Örneğin, Debian veya Windows 10 işletim sistemi ürün adı | Gerekli |
| OS.Version | Biçimindeki işletim sistemi sürümünü *major.minor.build.branch* | Gerekli |

##### <a name="device-element"></a>Cihaz öğesi

| Alan | Açıklama | Kullanım |
|-|-|-|
| Device.Manufacturer | Cihaz donanım üreticisi | Gerekli |
| Device.model | Cihaz modeli | Gerekli |
| Device.Version | Cihaz üreticisi tarafından sağlanan cihaz yazılımı sürümü. Bu değer, cihaz üreticisi tarafından izlenen bir sürümünü belirtir. | Gerekli |

### <a name="message-audio"></a>İleti `audio`

Konuşma özellikli istemci uygulamalar bir dizi ses öbekler ses akışı dönüştürerek, konuşma tanıma Hizmeti'ne ses gönderin. Her bir öbeği ses bir hizmet tarafından transcribed için konuşulan ses parçasını yürütür. Tek bir ses öbek boyutu üst sınırı 8192 bayttır. Ses akışı iletiler *ikili WebSocket iletileri*.

İstemcilerin kullandığı `audio` hizmete bir ses öbek gönderilecek ileti. İstemciler, ses öbekler halinde mikrofondan okuyun ve bu öbekleri döküm için konuşma tanıma Hizmeti'ne gönderin. İlk `audio` ileti düzgün ses kodlama hizmeti tarafından desteklenen biçimlerden biri için uygun olduğunu belirtir. iyi biçimlendirilmiş bir üst bilgisi içermesi gerekir. Ek `audio` iletiler ikili ses akışı yalnızca mikrofondan okunan veriler içerir.

İstemciler isteğe bağlı olarak gönderebilir bir `audio` sıfır uzunluklu gövdesi olan ileti. Bu ileti konuşma kullanıcı durdurdu, utterance tamamlandıktan ve mikrofon kapalı istemci bildiği hizmet söyler.

Konuşma hizmeti kullanan ilk `audio` içeren yeni bir istek/yanıt döngüsü başlangıcı göstermek için bir benzersiz istek tanımlayıcısı iletisi veya *kapatma*. Hizmet aldıktan sonra bir `audio` ileti isteğe bağlı olarak yeni bir istek tanımlayıcısı ile herhangi bir önceki bırakma ile ilişkili olan kuyruğa alınmış veya Gönderilmemiş iletileri atar.

| Alan | Açıklama |
|-------------|----------------|
| WebSocket ileti kodlama | binary |
| Gövde | Ses öbek için ikili veriler. En büyük boyutu 8192 bayttır. |

#### <a name="required-message-headers"></a>Gerekli ileti üstbilgileri

Aşağıdaki üst bilgiler tüm gerekli `audio` iletileri.

| Üstbilgi         |  Değer     |
| ------------- | ---------------- |
| `Path` | `audio` |
| X-RequestId | "No-dash" biçiminde UUID |
| X-zaman damgası | İstemci UTC saati ISO 8601 biçimli zaman damgası |
| İçerik türü | Ses içerik türü. Türü olmalıdır *ses/x-wav* (PCM) veya *ses/silk* (SILK). |

#### <a name="supported-audio-encodings"></a>Ses kodlamaları

Bu bölümde, konuşma tanıma hizmeti tarafından desteklenen ses codec bileşenleri açıklanmaktadır.

##### <a name="pcm"></a>PCM

Konuşma hizmeti sıkıştırılmamış pulse kod modülasyon (PCM) ses kabul eder. Ses, hizmete gönderilir [WAV](https://en.wikipedia.org/wiki/WAV) ilk ses öbek için biçim *gerekir* içeren geçerli bir [Kaynak Değişim dosya biçimi](https://en.wikipedia.org/wiki/Resource_Interchange_File_Format) (RIFF) üst bilgisi. Bir istemci bir bırakma yapan bir ses Öbek ile başlatır, *değil* geçerli bir RIFF üst bilgisi ekleyin, hizmet isteği reddeder ve WebSocket bağlantıyı sonlandırır.

PCM ses *gerekir* örneklenen 16 kHz örneği ve bir kanal başına 16 bit (*RIFF-16khz-16 bit-mono-pcm*). Konuşma hizmeti stereo ses akışları desteklemiyor ve belirtilen bit hızı, örnek hızı veya kanal sayısına kullanmayan ses akışları reddeder.

##### <a name="opus"></a>Geçerli

Açık, gayri münhasır, çok yönlü bir ses codec geçerli olur. Konuşma hizmeti destekleyen geçerli bir sabit bit hızında `32000` veya `16000`. Yalnızca `OGG` kapsayıcı için geçerli şu anda desteklenen tarafından belirtilen `audio/ogg` MIME türü.

Geçerli kullanmak için değiştirmeniz [JavaScript örnek](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/master/samples/browser/Sample.html#L101) değiştirip `RecognizerSetup` döndürmek için yöntemi.

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

#### <a name="detect-end-of-speech"></a>Konuşma sonu algılayın

İnsanlar değil açıkça sinyal bunlar tamamladığınızda konuşma. Giriş bir ses akışı olarak konuşma sonu işlemek için iki seçenek sahip olan konuşma kabul eden herhangi bir uygulama: hizmet uç Konuşma Algılama ve istemcisi bitiş Konuşma Algılama. Bu iki seçenek hizmet son Konuşma Algılama genellikle daha iyi bir kullanıcı deneyimi sağlar.

##### <a name="service-end-of-speech-detection"></a>Hizmet uç Konuşma Algılama

İdeal eller serbest konuşma deneyimi oluşturmak için kullanıcının dikte bittiğinde algılamak hizmeti uygulamalar sağlar. İstemcilerin gönderdiği ses mikrofondan *ses* hizmet sessizlik algılar ve geri ile yanıt verdiği kadar chunks bir `speech.endDetected` ileti.

##### <a name="client-end-of-speech-detection"></a>İstemcisi bitiş Konuşma Algılama

Kullanıcının bir şekilde konuşma sonu sinyal olanak tanıyan istemci uygulamaları da verebilirsiniz hizmet, sinyal. Örneğin, bir istemci uygulaması "Durdur" ya da kullanıcı basabilirsiniz "Sesini kapat" düğmesini olabilir. İstemci uygulamaları konuşma son sinyal gönderme bir *ses* öbek iletisiyle bir sıfır uzunluklu gövdesi. Konuşma hizmeti, bu iletiyi sonu gelen bir ses akışı olarak yorumlar.

### <a name="message-telemetry"></a>İleti `telemetry`

İstemci uygulamaları *gerekir* sonuna kadar her Aç Aç hakkında telemetri konuşma tanıma Hizmeti'ne göndererek kabul etmiş olursunuz. Dönüş son bildirim isteği ve yanıtının tamamlanması için gerekli tüm iletileri düzgün istemci tarafından alınan emin olmak konuşma tanıma hizmeti sağlar. Dönüş son bildirimi de istemci uygulamalarını beklendiği gibi çalıştığını doğrulamak konuşma tanıma hizmeti sağlar. Konuşma özellikli uygulamanızın sorunlarını gidermek için yardıma ihtiyacınız varsa, bu bilgileri benzersizdir.

İstemciler gerekir bildirimi bir bırakma sonuna göndererek bir `telemetry` ileti alma hemen sonra bir `turn.end` ileti. İstemciler onaylamak çalışır `turn.end` olabildiğince çabuk. Dönüş son onaylamak bir istemci uygulama başarısız olursa, konuşma tanıma hizmeti bir hata bağlantısıyla sonlandırabilir. İstemcileri yalnızca bir gönderme gerekir `telemetry` ileti için her istek ve yanıt tarafından tanımlanan *X-RequestId* değeri.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Text |
| `Path` | `telemetry` |
| X-zaman damgası | İstemci UTC saati ISO 8601 biçimli zaman damgası |
| İçerik türü | `application/json` |
| Gövde | Aç istemci bilgilerini içeren bir JSON yapısı |

Gövdesi için şema `telemetry` ileti tanımlanmış [Telemetri şema](#telemetry-schema) bölümü.

#### <a name="telemetry-for-interrupted-connections"></a>Kesilen bağlantıları için telemetri

Ağ bağlantısı bir bırakma sırasında herhangi bir nedenle başarısız olur ve istemciyi oluşturmazsa *değil* almak bir `turn.end` ileti hizmetinden, istemcinin gönderdiği bir `telemetry` ileti. Bu ileti, sonraki açışınızda istemci hizmeti için bağlantı kurar başarısız istek açıklar. İstemciler hemen gönderilecek bir bağlantı denemesi gerekmez `telemetry` ileti. İleti istemcide arabelleğe alınır ve gelecekteki bir kullanıcı tarafından istenen bağlantı üzerinden gönderilir. `telemetry` Başarısız istek iletisi *gerekir* kullanın *X-RequestId* başarısız istek değeri. İçin diğer iletiler gönderip beklemenize gerek kalmadan bir bağlantı kurulur hemen sonra hizmete gönderilen.

## <a name="service-originated-messages"></a>Hizmeti kaynaklı iletiler

Bu bölümde, konuşma hizmeti kaynaklanan ve istemciye gönderilen iletileri açıklanmaktadır. Konuşma hizmeti, istemci yeteneklerini bir kayıt defteri tutar ve her bir istemci tarafından bu nedenle gerekli olmayan tüm istemciler, burada açıklanan tüm iletileri alır iletileri oluşturur. Konuyu uzatmamak amacıyla, iletileri değeri tarafından başvurulan *yolu* başlığı. Örneğin, bir WebSocket kısa mesaj ile diyoruz *yolu* değer `speech.hypothesis` speech.hypothesis iletisi.

### <a name="message-speechstartdetected"></a>İleti `speech.startDetected`

`speech.startDetected` İleti konuşma hizmeti ses akışı konuşma algılandığını gösterir.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Text |
| `Path` | `speech.startDetected` |
| İçerik türü | Uygulama/json; Charset = utf-8 |
| Gövde | Konuşma başlangıcını algılandığında koşullarla ilgili bilgiler içeren JSON yapısı. *Uzaklığı* bu yapı alanında belirtir (100 nanosaniyelik birimler) cinsinden uzaklık zaman konuşma algılandı akışın başlangıç göre bir ses akışı olarak. |

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

Sırasında Konuşma tanıma, konuşma tanıma hizmeti sözcükler hakkında varsayımlar düzenli aralıklarla tanınan hizmeti oluşturur. Konuşma hizmeti bu hipotezi yaklaşık her 300 milisaniye istemciye gönderir. `speech.hypothesis` Uygundur *yalnızca* kullanıcı konuşma deneyimi için. Bağımlılığın içeriği veya metin doğruluğunu bu iletileri atmanız değil.

 `speech.hypothesis` İletisidir bazı metin işleme yeteneği olan ve algılanabileceğini kişiye sürüyor tanıma neredeyse gerçek zamanlı geri bildirim sağlamak istiyorsanız bu istemciler için geçerlidir.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Text |
| `Path` | `speech.hypothesis` |
| X-RequestId | "No-dash" biçiminde UUID |
| İçerik türü | uygulama/json |
| Gövde | Konuşma varsayım JSON yapısı |

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

*Uzaklığı* öğesi belirtir (100 nanosaniyelik birimler) cinsinden uzaklık zaman tümceciği tanınan, ilgili ses akışı başlangıcını.

*Süresi* öğesi bu konuşma deyimi süresi (100 nanosaniyelik cinsinden) belirtir.

İstemciler sıklığı, zamanlama veya konuşma varsayım veya her iki konuşma hipotezi metinde tutarlılığını içindeki metin hakkında varsayımlar yapmamanız gerekir. Yol açan hipotezleri hizmetinde transkripsiyonu işlemine yalnızca anlık görüntüleridir. Kararlı bir döküm birikmesi temsil etmiyor. Örneğin, bir ilk konuşma varsayım "ince eğlenceli" sözcükler içerebilir ve İkinci varsayım "komik bulun." sözcükler içerebilir Konuşma hizmeti, konuşma varsayım metin herhangi bir sonradan işleme (örneğin, büyük/küçük harf, noktalama işaretleri) gerçekleştirmez.

### <a name="message-speechphrase"></a>İleti `speech.phrase`

Konuşma hizmeti ne zaman belirler hizmeti oluşturan değişmez bir tanıma sonucu oluşturmak için yeterli bilgiye sahip bir `speech.phrase` ileti. Konuşma hizmeti, kullanıcı bir cümle veya tümceciği tamamlandığını algıladıktan sonra bu sonuçlar verir.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Text |
| `Path` | `speech.phrase` |
| İçerik türü | uygulama/json |
| Gövde | Konuşma tümcecik JSON yapısı |

Konuşma tümcecik JSON Şeması aşağıdaki alanları içerir: `RecognitionStatus`, `DisplayText`, `Offset`, ve `Duration`. Bu alanlar hakkında daha fazla bilgi için bkz. [Transkripsiyonu yanıtları](../concepts.md#transcription-responses).

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

`speech.endDetected` İletiyi istemci uygulama hizmetine ses akışı durması gerektiğini belirtir.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Text |
| `Path` | `speech.endDetected` |
| Gövde | Konuşma sonu algılandığında uzaklık içeren JSON yapısı. Uzaklık birimleri 100 nanosaniyelik uzaklığı başından itibaren ses tanıma için kullanılan temsil edilir. |
| İçerik türü | Uygulama/json; Charset = utf-8 |

#### <a name="sample-message"></a>Örnek ileti

```HTML
Path: speech.endDetected
Content-Type: application/json; charset=utf-8
X-RequestId: 123e4567e89b12d3a456426655440000

{
  "Offset": 0
}
```

*Uzaklığı* öğesi belirtir (100 nanosaniyelik birimler) cinsinden uzaklık zaman tümceciği tanınan, ilgili ses akışı başlangıcını.

### <a name="message-turnstart"></a>İleti `turn.start`

`turn.start` Hizmet perspektifinden bir bırakma başlangıcını bildirir. `turn.start` İleti, her zaman, aldığınız her istek için ilk yanıt iletisi. Almazsanız, bir `turn.start` iletisi, hizmet bağlantı durumunun geçersiz olduğunu varsayar.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Text |
| `Path` | `turn.start` |
| İçerik türü | Uygulama/json; Charset = utf-8 |
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

Gövdesi `turn.start` iletisidir Aç başlangıcını bağlamının içeren JSON yapısı. *Bağlam* öğesi içeren bir *serviceTag* özelliği. Bu özellik, hizmet Aç ile ilişkili bir etiket değeri belirtir. Uygulamanızdaki hataları giderme hakkında Yardım gerekiyorsa, bu değer Microsoft tarafından kullanılabilir.

### <a name="message-turnend"></a>İleti `turn.end`

`turn.end` Hizmet perspektifinden bir bırakma sonuna bildirir. `turn.end` İleti, her zaman herhangi bir istek için aldığınız son yanıt iletisi. İstemciler bu iletinin alınması temizleme etkinlikleri ve bir boşta durumuna geçiş için bir sinyal olarak kullanabilir. Almazsanız, bir `turn.end` iletisi, hizmet bağlantı durumunun geçersiz olduğunu varsayar. Bu durumlarda, hizmet mevcut bağlantıyı kapatın ve yeniden.

| Alan | Açıklama |
| ------------- | ---------------- |
| WebSocket ileti kodlama | Text |
| `Path` | `turn.end` |
| Gövde | None |

#### <a name="sample-message"></a>Örnek ileti

```HTML
Path: turn.end
X-RequestId: 123e4567e89b12d3a456426655440000
```

## <a name="telemetry-schema"></a>Telemetri şeması

Gövdesi *telemetri* iletisidir bir etkinleştirin ya da bir bağlanma denemelerinin istemci bilgilerini içeren bir JSON yapısı. Yapısı, istemci olaylarını ortaya çıktığında kayıt istemci zaman damgaları yapılır. Her zaman damgası "X-zaman damgası header." başlıklı bölümde açıklandığı gibi ISO 8601 biçiminde olmalıdır *Telemetri* , belirtmeyin tüm gerekli alanları JSON yapısındaki ya da doğru zaman damgası biçimi kullanmayın iletileri istemci bağlantısını sona erdirmek hizmeti neden olabilir. İstemciler *gerekir* tüm gerekli alanlar için geçerli değerler sağlayın. İstemciler *gereken* uygun olduğunda isteğe bağlı alanlar için değerler sağlayın. Bu bölümdeki örnekte gösterilen değerleri yalnızca gösterim amaçlıdır.

Telemetri şema, aşağıdaki bölümlere ayrılmıştır: alınan ileti zaman damgaları ve ölçümleri. Aşağıdaki bölümlerde biçimini ve her parça kullanımını belirtilmişse.

### <a name="received-message-time-stamps"></a>Alınan ileti zaman damgaları

İstemciler hizmete bağlandıktan sonra aldıkları tüm iletiler için giriş saat değerlerini içermelidir. Bu değerleri zaman kaydetmelisiniz olduğunda istemci *alınan* ağdan gelen her ileti. Değeri başka bir zaman kaydetmemelisiniz değil. Örneğin, istemci zaman kayıt, bu *etkilediği* ileti üzerinde. Alınan ileti zaman damgaları bir dizi içinde belirtilen *ad: değer* çiftleri. Çift adını belirtir *yolu* iletinin değeri. Değer çiftinin iletisi alındığında istemci saati belirtir. Veya, belirtilen adda birden fazla iletisi alındı, değer çiftinin bu iletileri alındığında gösteren zaman damgaları dizisi.

```JSON
  "ReceivedMessages": [
    { "speech.hypothesis": [ "2016-08-16T15:03:48.172Z", "2016-08-16T15:03:48.331Z", "2016-08-16T15:03:48.881Z" ] },
    { "speech.endDetected": "2016-08-16T15:03:49.721Z" },
    { "speech.phrase": "2016-08-16T15:03:50.001Z" },
    { "turn.end": "2016-08-16T15:03:51.021Z" }
  ]
```

İstemciler *gerekir* JSON gövdesine bu iletileri zaman damgalarının dahil olmak üzere hizmet tarafından gönderilen tüm iletilerin alınmasını kabul etmiş olursunuz. Bir iletinin alınması onaylamak bir istemci başarısız olursa, hizmet bağlantı sonlandırabilir.

### <a name="metrics"></a>Ölçümler

İstemciler bir istek yaşam süresi boyunca oluşan olaylar hakkında bilgi içermelidir. Aşağıdaki ölçümler desteklenir: `Connection`, `Microphone`, ve `ListeningTrigger`.

### <a name="metric-connection"></a>Ölçüm `Connection`

`Connection` Ölçüm bağlantı denemeleri istemci tarafından ayrıntılarını belirtir. WebSocket bağlantısı başlatıldığında ve tamamlanmış ölçüm zamanı damgalarıyla içermelidir. `Connection` Ölçüm gereklidir *yalnızca ilk Aç bağlantısı için*. Bu bilgileri içerecek şekilde sonraki kapatır gerekli değildir. Bir bağlantı kurulmadan önce bir istemci birden çok bağlantı denemeleri yaparsa hakkında bilgi *tüm* bağlantı girişimleri dahil edilmelidir. Daha fazla bilgi için [bağlantı başarısızlık telemetrilerini](#connection-failure-telemetry).

| Alan | Açıklama | Kullanım |
| ----- | ----------- | ----- |
| Ad | `Connection` | Gerekli |
| Kimlik | Bağlantı tanımlayıcı değeri kullanıldı *X ConnectionID* Bu bağlantı isteği üst bilgisi | Gerekli |
| Başlatma | İstemci bağlantı isteği zaman gönderdiği saati | Gerekli |
| End | Zaman istemci bağlantı başarıyla kuruldu bildirim alındığında veya hata durumlarında reddedildi, reddedildi veya başarısız oldu | Gerekli |
| Hata | Varsa, oluşan hata açıklaması. Bağlantı başarılı olursa, istemcilerin bu alan atlamak. Bu alan uzunluğunun üst sınırı 50 karakterdir. | Aksi takdirde atlanmış hata durumları için gerekli |

Hata açıklaması en fazla 50 karakter arasında olmalıdır ve ideal olarak aşağıdaki tabloda listelenen değerlerden biri olmalıdır. Hata koşulu şu değerlerden biri olarak eşleşmiyorsa, istemciler birleştiren bir hata durumu açıklaması kullanarak kullanabilir [CamelCasing](https://en.wikipedia.org/wiki/Camel_case) boşluk olmadan. Gönderme olanağı bir *telemetri* ileti hizmet, bu nedenle yalnızca geçici bir bağlantı gerektirir ya da geçici hata koşulları rapor içinde *telemetri* ileti. Hata koşulları *kalıcı olarak* blok hizmetine bir bağlantı kurulurken bir istemcinin istemci hizmete herhangi bir ileti göndermesini engellemek dahil olmak üzere *telemetri* iletileri.

| Hata | Kullanım |
| ----- | ----- |
| DNSfailure | İstemci, ağ yığınındaki bir DNS hatası nedeniyle hizmetine bağlanamadı. |
| NoNetwork | İstemci bağlantı çalıştı, ancak ağ yığınını, fiziksel ağ kullanılabilir olduğunu bildirdi. |
| NoAuthorization | İstemci bağlantısı, bağlantı için bir yetkilendirme belirteci alma girişimi sırasında başarısız oldu. |
| NoResources | İstemci, bir bağlantısı kurmaya çalışırken bazı yerel kaynaklara (örneğin bellek) yetersiz kaldı. |
| Yasak | İstemci HTTP hizmetin döndürdüğü için hizmete bağlanamadı `403 Forbidden` WebSocket yükseltme isteği durum kodu. |
| Yetkilendirilmemiş | İstemci HTTP hizmetin döndürdüğü için hizmete bağlanamadı `401 Unauthorized` WebSocket yükseltme isteği durum kodu. |
| BadRequest | İstemci HTTP hizmetin döndürdüğü için hizmete bağlanamadı `400 Bad Request` WebSocket yükseltme isteği durum kodu. |
| ServerUnavailable | İstemci HTTP hizmetin döndürdüğü için hizmete bağlanamadı `503 Server Unavailable` WebSocket yükseltme isteği durum kodu. |
| ServerError | İstemcinin hizmetin döndürdüğü için hizmete bağlanamadı bir `HTTP 500` WebSocket yükseltme isteği durum kodu iç hata. |
| zaman aşımı | İstemcinin bağlantı isteği, hizmetten bir yanıt olmadan zaman aşımına uğradı. *Son* alan ne zaman istemci zaman aşımına uğradı ve bağlantı için bekleme durduruldu saati içerir. |
| Senderconnections'da | İstemci bağlantı bazı iç istemci hatası nedeniyle sonlandırıldı. |

### <a name="metric-microphone"></a>Ölçüm `Microphone`

`Microphone` Ölçüm için tüm konuşma kapatır gereklidir. Bu ölçüm sırasında ses girişi etkin bir konuşma isteği için kullanılan istemci üzerinde süreyi ölçer.

Aşağıdaki örnekler, kayıt için kılavuz olarak kullanın *Başlat* saat değerlerini `Microphone` istemci uygulamanıza ölçüm:

* Bir istemci uygulaması, bir kullanıcı mikrofon başlatmak için fiziksel bir düğme basmalısınız gerektirir. Düğmesine basın sonra istemci uygulaması giriş mikrofondan okur ve konuşma hizmetine gönderir. *Başlat* değerini `Microphone` ölçüm mikrofon başlatıldı ve giriş sağlamak hazır olduğunda Düğme basma saatinden kaydeder. *Son* değerini `Microphone` ölçüm kaydeder olduğunda istemci uygulaması durduruldu aldığı sonra hizmete ses akışı zaman `speech.endDetected` hizmetinden alınan ileti.

* Bir istemci uygulaması "her zaman" dinlediği bir anahtar sözcüğü spotter kullanır. Yalnızca anahtar sözcüğü spotter konuşulan tetikleyici tümcecik algıladıktan sonra istemci uygulaması mikrofondan girişini toplamak ve konuşma tanıma Hizmeti'ne gönderin. *Başlat* değerini `Microphone` ölçüm mikrofon girişten kullanmaya başlamak için istemci anahtar sözcüğü spotter bildirim zaman zaman kaydeder. *Son* değerini `Microphone` ölçüm kaydeder olduğunda istemci uygulaması durduruldu aldığı sonra hizmete ses akışı zaman `speech.endDetected` hizmetinden alınan ileti.

* Bir istemci uygulaması için sabit bir ses akışı erişebilir ve sessizlik/Konuşma Algılama ilgili ses akışı gerçekleştiren bir *Konuşma Algılama Modülü*. *Başlat* değerini `Microphone` ölçüm zamanı kaydeder olduğunda *Konuşma Algılama Modülü* giriş ses akıştan kullanmaya başlamak için İstemci bildirim. *Son* değerini `Microphone` ölçüm kaydeder olduğunda istemci uygulaması durduruldu aldığı sonra hizmete ses akışı zaman `speech.endDetected` hizmetinden alınan ileti.

* Bir istemci uygulamanın ikinci Aç çok bırakma isteği işleme ve giriş için ikinci bırakma toplamak üzere mikrofon etkinleştirmek için bir hizmet yanıt iletisi olarak bilgilendirilir. *Başlat* değerini `Microphone` ölçüm ne zaman istemci uygulaması mikrofon sağlar ve giriş ilgili ses kaynağından kullanmaya başladığında zaman kaydeder. *Son* değerini `Microphone` ölçüm kaydeder olduğunda istemci uygulaması durduruldu aldığı sonra hizmete ses akışı zaman `speech.endDetected` hizmetinden alınan ileti.

*Son* saat değeri için `Microphone` ölçüm ne zaman istemci uygulaması durduruldu ses giriş akış zaman kaydeder. Çoğu durumda, bu olay, kısa süre içinde alınan istemci sonra gerçekleşir `speech.endDetected` hizmetinden alınan ileti. İstemci uygulamaları doğrulayın, düzgün bir şekilde protokolü, sağlayarak uygun olduğunu *son* saat değeri için `Microphone` ölçüm için giriş saat değerinden daha sonra gerçekleşir `speech.endDetected` ileti. Ve genellikle bir sonuna başka bir bırakma başlangıcını arasında bir gecikme olduğundan, istemcilerin protokol uyumluluğu, sağlayarak doğrulayabilir *Başlat* süresi `Microphone` herhangi bir sonraki bırakma için ölçüm doğru süreyi kaydeder olduğunda istemci *çalışmaya* mikrofonun ses akış girişi hizmetine kullanarak.

| Alan | Açıklama | Kullanım |
| ----- | ----------- | ----- |
| Ad | Mikrofon | Gerekli |
| Başlatma | Ne zaman istemci mikrofon veya başka bir ses akışı ses girişi kullanmaya veya bir tetikleyici anahtar sözcüğü spotter alınan saati | Gerekli |
| End | Ne zaman istemci ses veya mikrofon akış kullanarak durduruldu. süre | Gerekli |
| Hata | Varsa, oluşan hata açıklaması. Mikrofon işlem başarılı olursa, istemcilerin bu alan atlamak. Bu alan uzunluğunun üst sınırı 50 karakterdir. | Aksi takdirde atlanmış hata durumları için gerekli |

### <a name="metric-listeningtrigger"></a>Ölçüm `ListeningTrigger`
`ListeningTrigger` Ölçüm ölçer zaman zaman kullanıcı Konuşma giriş başlatan eylemi yürütür. `ListeningTrigger` Ölçüm isteğe bağlıdır, ancak bu ölçüm sağlayan istemciler Bunu yapmak için önerilir.

Aşağıdaki örnekler, kayıt için kılavuz olarak kullanın *Başlat* ve *son* saat değerlerini `ListeningTrigger` istemci uygulamanıza ölçüm.

* Bir istemci uygulaması, bir kullanıcı mikrofon başlatmak için fiziksel bir düğme basmalısınız gerektirir. *Başlat* düğmesine basma süresini Bu ölçüm kayıtları için bir değer. *Son* değeri düğmesine basma tamamlandığı zaman kaydeder.

* Bir istemci uygulaması "her zaman" dinlediği bir anahtar sözcüğü spotter kullanır. Anahtar sözcüğü sonra konuşulan tetikleyici tümcecik spotter algılar istemci uygulaması giriş mikrofondan okur ve konuşma hizmetine gönderir. *Başlat* Bu ölçüm tetikleyicisi tümcecik olarak ardından algılandı ses alındığında anahtar sözcüğü spotter zaman kayıtları için bir değer. *Son* değeri zaman son tetikleyici tümceciği konuşma kullanıcı tarafından zaman kaydeder.

* Bir istemci uygulaması için sabit bir ses akışı erişebilir ve sessizlik/Konuşma Algılama ilgili ses akışı gerçekleştiren bir *Konuşma Algılama Modülü*. *Başlat* Bu ölçüm zamanı kaydeder değeri, *Konuşma Algılama Modülü* alındı konuşma ardından algılandı ses. *Son* değeri zaman kaydeder olduğunda *Konuşma Algılama Modülü* konuşma algılandı.

* Bir istemci uygulamanın ikinci Aç çok bırakma isteği işleme ve giriş için ikinci bırakma toplamak üzere mikrofon etkinleştirmek için bir hizmet yanıt iletisi olarak bilgilendirilir. İstemci uygulaması gereken *değil* dahil bir `ListeningTrigger` ölçüm için bu etkinleştirin.

| Alan | Açıklama | Kullanım |
| ----- | ----------- | ----- |
| Ad | ListeningTrigger | İsteğe bağlı |
| Başlatma | İstemci dinleme tetikleyici başlatıldığı saat | Gerekli |
| End | Ne zaman istemci dinleme tetikleyici bitiş zamanı | Gerekli |
| Hata | Varsa, oluşan hata açıklaması. Tetikleyici işlem başarılı olduysa, istemciler bu alan atlamak. Bu alan uzunluğunun üst sınırı 50 karakterdir. | Aksi takdirde atlanmış hata durumları için gerekli |

#### <a name="sample-message"></a>Örnek ileti

Aşağıdaki örnek, bir telemetri iletisi ReceivedMessages hem ölçümleri bölümlerle gösterir:

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

Bu bölümde, hata iletileri ve işlemek için uygulamanız gereken koşullar açıklanır.

### <a name="http-status-codes"></a>HTTP durum kodları

WebSocket yükseltme isteği sırasında Konuşma hizmeti herhangi bir standart HTTP durum kodları gibi döndürebilir `400 Bad Request`vb. Uygulamanız bu hata koşulları doğru şekilde işlemesi gerekir.

#### <a name="authorization-errors"></a>Yetkilendirme hataları

WebSocket yükseltme sırasında Konuşma tanıma hizmeti bir HTTP döndürür. yanlış yetkilendirme sağlanmazsa `403 Forbidden` durum kodu. Bu hata kodu tetikleyebileceğiniz koşullar arasında şunlardır:

* Eksik *yetkilendirme* üstbilgisi

* Geçersiz yetkilendirme belirteci

* Süresi dolan yetkilendirme belirteci

`403 Forbidden` Hata iletisi olmayan konuşma hizmeti ile bir sorun olduğunu gösteriyor. Bu hata iletisi, istemci uygulama bir sorun olduğunu gösterir.

### <a name="protocol-violation-errors"></a>Protokol ihlali hatalarını

Konuşma hizmeti, istemciden gelen herhangi bir protokol ihlali algılarsa, hizmet döndüren sonra WebSocket bağlantıyı sonlandırır. bir *durum kodu* ve *neden* sonlandırma için. İstemci uygulamaları, sorun giderme ve ihlali gidermek için bu bilgileri kullanabilir.

#### <a name="incorrect-message-format"></a>Yanlış ileti biçimi

Bir istemci bir metin veya ikili ileti bu belirtiminde belirtilen doğru biçimde kodlanmamış hizmetine gönderir, hizmeti ile bağlantıyı kapatır. bir *1007 geçersiz yük verisi* durum kodu.

Aşağıdaki örneklerde gösterildiği gibi hizmet çeşitli nedenlerle, bu durum kodunu döndürür:

* "Yanlış ileti biçimi. İkili ileti geçersiz üstbilgi boyutu ön eki vardır." İstemcisi, bir geçersiz üstbilgi boyutu ön ekine sahip bir ikili ileti gönderilir.

* "Yanlış ileti biçimi. İkili ileti geçersiz üstbilgi boyutuna sahiptir." İstemcisi belirtilen geçersiz üstbilgi boyutu ikili bir ileti gönderilir.

* "Yanlış ileti biçimi. İkili ileti üstbilgileri UTF-8 kod çözme başarısız oldu." İstemcisi, UTF-8'de düzgün şekilde kodlanmış olmayan üst bilgilerini içeren bir ikili ileti gönderilir.

* "Yanlış ileti biçimi. SMS mesajı hiçbir veri içermiyor." İstemci hiçbir gövde verilerini içeren bir SMS mesajı gönderdik.

* "Yanlış ileti biçimi. SMS mesajı UTF-8 kod çözme başarısız oldu." İstemci, UTF-8'de düzgün şekilde kodlanmamış bir SMS mesajı gönderdik.

* "Yanlış ileti biçimi. Metin iletisi üst bilgi ayırıcı içerir." İstemci, bir başlık ayırıcıyı içermiyordu veya yanlış bir başlık ayırıcıyı kullanılan bir SMS mesajı gönderdik.

#### <a name="missing-or-empty-headers"></a>Eksik veya boş üstbilgileri

Bir istemci gerekli üst bilgileri olmayan bir ileti gönderirse *X-RequestId* veya *yolu*, hizmeti ile bağlantıyı kapatır bir *1002 protokol hatası* durum kodu. Yapılacak olan "üst bilgisi eksik veya boş. {Üst bilgi adı}."

#### <a name="requestid-values"></a>RequestId değerleri

Belirten bir istemci bir ileti gönderir, bir *X-RequestId* üstbilgisi hatalı biçimde, hizmete bağlantıyı kapatır ve döndürür bir *1002 protokol hatası* durumu. "İsteği geçersiz. iletidir X-RequestId üstbilgi değeri yok-dash UUID biçiminde belirtilmedi."

#### <a name="audio-encoding-errors"></a>Ses kodlama hataları

Bir istemci bir bırakma ve ses biçimi başlatan bir ses öbek gönderir veya kodlama gerekli belirtime uygun değil, hizmet bağlantıyı kapatır ve döndürür bir *1007 geçersiz yük verisi* durum kodu. İleti hata kaynağı kodlama biçimi gösterir.

#### <a name="requestid-reuse"></a>RequestId yeniden kullanma

Bir istemci, bırakma alınan istek tanımlayıcısı yeniden kullanan bir ileti gönderir, bir bırakma tamamladıktan sonra hizmeti bağlantıyı kapatır ve döndürür bir *1002 protokol hatası* durum kodu. "İsteği geçersiz. iletidir İstek tanımlayıcıları kullanılmasını izin verilmez."

## <a name="connection-failure-telemetry"></a>Bağlantı hatası telemetri

İyi kullanıcı deneyimini sağlamak için istemciler bir bağlantı içinde önemli kontrol noktaları zaman damgalarında konuşma hizmeti kullanarak bilgilendirmeniz *telemetri* ileti. İstemcilerin hizmeti çalıştı ancak başarısız bağlantılar, bildirmek eşit derecede önemlidir.

Başarısız her bağlantı denemesi için istemcileri oluşturma bir *telemetri* benzersiz bir iletiyle *X-RequestId* üstbilgi değeri. İstemci bağlantı kuramadı, çünkü *ReceivedMessages* JSON gövdesi alanında atlanabilir. Yalnızca `Connection` girişi *ölçümleri* alanı dahildir. Bu girdi karşılaşıldı hata koşulu yanı sıra başlangıç ve bitiş zaman damgası içerir.

### <a name="connection-retries-in-telemetry"></a>Telemetri, bağlantı yeniden deneme

İstemciler ayırt *deneme* gelen *birden çok bağlantı denemeleri* bağlantı denemesi tetikleyen olayı tarafından. Herhangi bir kullanıcı girişi program aracılığıyla gerçekleştirilen bağlantı girişimleri, yeniden denemeler var. Kullanıcı girişine yanıt olarak gerçekleştirilen birden çok bağlantı denemeleri, birden çok bağlantı denemeleri ' dir. İstemcilerin her kullanıcı tetiklenen bağlantı denemesi benzersiz bir vermek *X-RequestId* ve *telemetri* ileti. İstemcileri yeniden *X-RequestId* için programlı bir yeniden deneme. Birden çok deneme için tek bir bağlantı denemesi yapıldı, her yeniden deneme girişimi olarak dahil edilen bir `Connection` girişi *telemetri* ileti.

Örneğin, bir kullanıcı bir bağlantı başlatmak için anahtar sözcüğü tetikleyici konuşacak ve ilk bağlantı denemesi DNS hataları nedeniyle başarısız olduğunu varsayalım. Ancak, program aracılığıyla istemci tarafından yapılan ikinci denemesi başarılı olur. Ek kullanıcı girişi gerektirmeden istemci bağlantıyı yeniden çünkü tek bir istemcinin kullandığı *telemetri* birden çok ileti `Connection` bağlantı açıklamak için girdileri.

Başka bir örnek olarak, bir kullanıcı bir bağlantı başlatmak için anahtar sözcüğü tetikleyici konuşacak ve üç denemeden sonra bu bağlantı girişimi başarısız olduğunu varsayalım. İstemci, hizmete bağlanmaya durakları sağlar ve bir sorun oluştu, kullanıcıya bildirir. Kullanıcı daha sonra yeniden anahtar sözcüğü tetikleyici konuşur. İstemci hizmete bağlandığında, bu kez, varsayalım. Bağlandıktan sonra istemci anında gönderir bir *telemetri* üçünü içeren hizmete ileti `Connection` bağlantı hataları tanımlayan girdileri. Alma sonra `turn.end` ileti, istemcinin gönderdiği başka *telemetri* başarılı bağlantı açıklayan ileti.

## <a name="error-message-reference"></a>Hata iletisi başvurusu

### <a name="http-status-codes"></a>HTTP durum kodları

| HTTP durum kodu | Açıklama | Sorun giderme |
| - | - | - |
| 400 Hatalı istek | İstemci, yanlış bir WebSocket bağlantısı isteği gönderdi. | Tüm HTTP üst bilgilerini ve gerekli parametreleri tarafından sağlanan ve değerlerin doğru olduğundan emin olun. |
| 401 Yetkisiz | İstemcinin gerekli yetkilendirme bilgilerini içermiyordu. | Göndereceğiniz onay *yetkilendirme* üst bilgisinde WebSocket bağlantısı kurar. |
| 403 Yasak | İstemci yetkilendirme bilgileri gönderilen, ancak geçersiz. | Süresi dolmuş veya geçersiz bir değer göndereceğiniz değil, kontrol *yetkilendirme* başlığı. |
| 404 Bulunamadı | İstemci, desteklenmeyen bir URL yoluna erişmeye çalıştı. | WebSocket bağlantısı için doğru URL kullanıyorsanız denetleyin. |
| 500 sunucu hatası | Hizmet bir iç hatayla karşılaştı ve istek karşılayamadı. | Çoğu durumda, bu geçici bir hatadır. İsteği yeniden deneyin. |
| 503 Hizmet Kullanılamıyor | Hizmet isteği işleyemedi. | Çoğu durumda, bu geçici bir hatadır. İsteği yeniden deneyin. |

### <a name="websocket-error-codes"></a>WebSocket hata kodları

| WebSocketsStatus kod | Açıklama | Sorun giderme |
| - | - | - |
| 1000 normal kapatma | Hizmet bir hata olmadan WebSocket bağlantısı kapatıldı. | Nasıl ve ne zaman hizmeti WebSocket bağlantısı sonlandırabilirsiniz anladığınızdan emin olmak için belgeler, beklenmeyen WebSocket kapatma, yeniden okuyun. |
| 1002 protokol hatası | İstemci, Protokolü gereksinimlerine uyacak şekilde başarısız oldu. | Protokolü belgeleri anlamak ve gereksinimleri hakkında açık olduğundan emin olun. Önceki belgelerini protokolü gereksinimleri ihlal olmadığını görmek için hata nedenleri hakkında okuyun. |
| 1007 geçersiz yük verisi | İstemcisi, bir protokol iletisinde geçersiz bir yükü gönderilir. | Hatalar için hizmete gönderilen son ileti denetleyin. Yük hataları hakkında önceki belgelerini okuyun. |
| 1011 değerinin kodunu sunucu hatası | Hizmet bir iç hatayla karşılaştı ve istek karşılayamadı. | Çoğu durumda, bu geçici bir hatadır. İsteği yeniden deneyin. |

## <a name="related-topics"></a>İlgili konular

Bkz: bir [JavaScript SDK'sı](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript) diğer bir deyişle WebSocket tabanlı konuşma hizmeti Protokolü uygulaması.
