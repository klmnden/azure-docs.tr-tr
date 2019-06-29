---
title: Translator konuşma çevirisi API'si başvurusu
titleSuffix: Azure Cognitive Services
description: Translator konuşma tanıma API'si için başvuru belgeleri.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: reference
ms.date: 05/18/2018
ms.author: swmachan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 0f083a6ca3079128aad4aba3a53013df378a6106
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446895"
---
# <a name="translator-speech-api"></a>Translator Konuşma Çevirisi API’si

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Bu hizmet, bir akış API'sini bir dilden damıtarak konuşma bağlamında kullanılabilen konuşma başka bir dil metne özelliği sunar. API, geri çevrilen metni seslendirme için metin okuma özellikleri de tümleştirir. Translator konuşma tanıma API'si, Skype Translator görüldüğü gibi gerçek zamanlı konuşma çevirisi gibi senaryolara olanak sağlar.

Translator konuşma tanıma API'si ile istemci uygulamaları, hizmet konuşma sesine akışla aktarma ve geri tanınan metin kaynak dili ve çevirisini hedef dil dahil metin tabanlı sonuçları bir akışını alın. Metin sonuçları, gelen ses akışına derin sinir ağlarıyla desteklenen Otomatik Konuşma Tanıma (ASR) uygulanarak oluşturulur. Ham ASR çıktı daha yakından kullanıcının amacını yansıtmak için TrueText adında yeni bir teknik tarafından daha fazla geliştirildi. Örneğin, TrueText disfluencies (hmms ve coughs) ve geri yükleme uygun noktalama işaretleri ve büyük/küçük harf kaldırır. Küfürleri maskeleme veya çıkarma olanağı da sağlanır. Tanıma ve çeviri altyapıları, konuşmaları işlemek için özel olarak eğitilmiştir. Konuşma çevirisi hizmeti sessizlik algılama bir utterance sonuna belirlemek için kullanır. Ses etkinliğinde bir duraklama olduktan sonra, hizmet tamamlanan konuşma için nihai sonucun geri akışını yapar. Ayrıca hizmet kısmi sonuçları da geri gönderebilir; bu yolla devam eden konuşma için ara tanıma ve çeviriler sağlanır. Son sonuçlar için hizmet konuşulan metnin hedef dilde konuşma (okuma) sentezlemek olanağı sağlar. Metni konuşmaya dönüştürme sesi, istemci tarafından belirtilen biçimde oluşturulur. WAV ve MP3 biçimleri kullanılabilir.

Translator konuşma tanıma API'si, istemci ve sunucu arasında bir tam çift yönlü iletişim kanalı sağlayan WebSocket Protokolü yararlanır. Bir uygulama hizmeti kullanmak için aşağıdaki adımları gerektirir:

## <a name="1-getting-started"></a>1. Başlarken
Translator Text API gerekecek erişmeye [için Microsoft Azure'a kaydolun](translator-speech-how-to-signup.md).

## <a name="2-authentication"></a>2. Kimlik Doğrulaması

Kimlik doğrulaması için abonelik anahtarını kullanın. Translator konuşma tanıma API'si, kimlik doğrulamasının iki modu destekler:

* **Erişim belirteci kullanarak:** Uygulamanızda, belirteci Hizmeti'nden bir erişim belirteci alın. Translator konuşma tanıma API'si abonelik anahtarınızı Azure Bilişsel hizmetler kimlik doğrulama hizmetinden bir erişim belirteci almak için kullanın. Erişim belirteci 10 dakika için geçerlidir. 10 dakikada bir yeni bir erişim belirteci edinmek ve bu 10 dakika içinde aynı erişimi kullanarak yönelik yinelenen isteklerden belirteci tutun.

* **Bir abonelik anahtarı kullanarak doğrudan:** Uygulamanızda bir değer olarak abonelik anahtarınızı aktarmak `Ocp-Apim-Subscription-Key` başlığı.

Abonelik anahtarınız ve erişim belirteci görünümünden gizlenmelidir gizli olarak kabul eder.

## <a name="3-query-languages"></a>3. Sorgu dil
**Geçerli kümesini desteklenen diller için dil kaynak sorgulayın.** [Dilleri kaynak](languages-reference.md) dil kümesini kullanıma sunar ve sesler konuşma tanıma, metin çevirisi ve metin okuma için kullanılabilir. Her bir dil veya sesli aynı dil veya sesli tanımlamak için Translator konuşma tanıma API'si kullanan bir tanımlayıcı verilir.

## <a name="4-stream-audio"></a>4. Stream ses
**Bir bağlantıyı açın ve hizmet için ses akışı başlayın.** Hizmet URL'si `wss://dev.microsofttranslator.com/speech/translate`. Parametreler ve hizmet tarafından beklenen ses biçimleri, aşağıda açıklanan `/speech/translate` işlemi. Parametrelerden biri, adım 2'deki erişim belirteci geçirmek için kullanılır.

## <a name="5-process-the-results"></a>5. Sonuçları işlemek
**Akışa geri hizmetinden sonuçları işleyebilirsiniz.** Kısmi sonuçlar, son sonuçları ve metin okuma ses parçaları biçimi belgelerinde açıklanan `/speech/translate` aşağıdaki işlemi.

Translator konuşma çevirisi API'sine kullanımını gösteren kod örnekleri web'da [Microsoft Translator GitHub site](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Uygulama Notları

Konuşma çevirisi için oturumu /Speech/translate Establishes Al

### <a name="connecting"></a>Bağlanma
Hizmete bağlanmadan önce daha sonra bu bölümde verilen parametrelere listesini gözden geçirin. Bir örnek istek şöyledir:

`GET wss://dev.microsofttranslator.com/speech/translate?from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa&api-version=1.0`
`Ocp-Apim-Subscription-Key: {subscription key}`
`X-ClientTraceId: {GUID}`

İstek İngilizce konuşulan hizmete akışa ve İtalyanca çevrilmiş belirtir. Her son tanıma işleminin sonucu Elsa adlı kadın sesi ile metin okuma ses bir yanıt oluşturur. İstek kimlik bilgilerini içerdiğine dikkat edin `Ocp-Apim-Subscription-Key header`. Ayrıca istek üst bilgisinde bir genel benzersiz tanıtıcısı ayarlayarak en iyi uygulama aşağıdaki `X-ClientTraceId`. Böylece ortaya çıkan sorunları gidermek için kullanılabilir bir istemci uygulaması izleme kimliği oturum açmanız gerekir.

### <a name="sending-audio"></a>Ses gönderme
Bağlantı kurulduktan sonra istemci hizmete ses akışı başlar. İstemci öbekler halinde ses gönderir. Her bir öbeği Binary türünde Websocket bir iletiyi kullanarak aktarılır.

Ses giriştir dalga ses dosyası biçimi (WAVE ya da daha fazla yaygın olarak da WAV nedeniyle dosya adı uzantısı bilinen). İstemci uygulaması, tek kanal, 16 kHz örneklenen işaretli 16 bit PCM ses akışı. İstemci tarafından akışa bayt ilk kümesi WAV üst bilgi içerir. Tek bir kanal 16 kHz örneklenen 16 bit PCM stream imzalı 44 baytlık üst bilgi gösterilmiştir:

|Offset|Değer|
|:---|:---|
|0 - 3|"RIFF"|
|4 - 7|0|
|8 - 11|"WAVE"|
|12 - 15|"fmt"|
|16 - 19|16|
|20 - 21|1|
|22 - 23|1|
|24 - 27|16000|
|28 - 31|32000|
|32 - 33|2|
|34 - 35|16|
|36 - 39|"veri"|
|40 - 43|0|

Toplam dosya boyutu (bayt 4-7) ve "veri" (bayt 40-43) boyutu sıfır olarak ayarlanır. Burada toplam boyutu mutlaka önceden bilinmiyor akış senaryosu için Tamam budur.

WAV (RIFF) üst bilgisi gönderdikten sonra istemci ses veri öbekleri gönderir. İstemci, sabit bir süre (örneğin akışı 100ms birer birer ses) temsil eden sabit boyutlu öbeklere genellikle akışı sağlanacak.

### <a name="signal-the-end-of-the-utterance"></a>Sinyal utterance sonu
Translator konuşma tanıma API'si, döküm ve ses akışı çevirisi ses gönderiyorsanız olarak döndürür. Son transkripti, son çeviri ve çevrilmiş ses için yalnızca utterance sonunda döndürülür. Bazı durumlarda utterance sonuna zorlamak isteyebilirsiniz. Lütfen sessizlik 2.5 utterance sonuna zorlamak için saniye gönderin.

### <a name="final-result"></a>Son Sonuç
Bir son konuşma tanıma işleminin sonucu bir utterance sonunda oluşturulur. Bir sonuç hizmetinden istemciye metin türünde bir WebSocket ileti kullanarak aktarılır. İleti içeriğini aşağıdaki özelliklere sahip bir nesnenin JSON seri hale getirme:

* `type`: Sonuç türü tanımlamak için dize sabiti. Son sonuçları için son değerdir.
* `id`: Tanıma işleminin sonucu için atanan tanımlayıcı dize.
* `recognition`: Tanınan metin kaynak dili. Metin boş bir dize false tanıma durumunda olabilir.
* `translation`: Tanınan metin hedef çevrilmiş joomla.
* `audioTimeOffset`: Saat döngüsü içindeki tanıma başlangıç saati uzaklığı (1 değer çizgisi = 100 nanosaniye). Uzaklık akış göre başlangıcıdır.
* `audioTimeSize`: Tanıma Tick (100 nanosaniye) cinsinden süre.
* `audioStreamPosition`: Tanıma başlangıcı bayt uzaklığı. Akış başlangıcına göre uzaklığı var.
* `audioSizeBytes`: Tanıma bayt cinsinden boyutu.

Ses akışı tanıma konumlandırma sonuçları varsayılan olarak dahil değil unutmayın. `TimingInfo` Özelliği istemci tarafından seçilmesi gerekir (bkz `features` parametresi).

Bir örnek nihai sonucu aşağıdaki gibidir:

```
{
  type: "final"
  id: "23",
  recognition: "what was said",
  translation: "translation of what was said",
  audioStreamPosition: 319680,
  audioSizeBytes: 35840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 21900000
}
```

### <a name="partial-result"></a>Kısmi sonuç
Kısmi veya Ara konuşma tanıma sonuçları istemciye varsayılan akışı gerçekleştirilmez. İstemci istekleri için özellikleri sorgu parametresini kullanabilirsiniz.

Kısmi bir sonuç hizmetinden istemciye metin türünde bir WebSocket ileti kullanarak aktarılır. İleti içeriğini aşağıdaki özelliklere sahip bir nesnenin JSON seri hale getirme:

* `type`: Sonuç türü tanımlamak için dize sabiti. Kısmi sonuçlar için kısmi değerdir.
* `id`: Tanıma işleminin sonucu için atanan tanımlayıcı dize.
* `recognition`: Tanınan metin kaynak dili.
* `translation`: Tanınan metin hedef çevrilmiş joomla.
* `audioTimeOffset`: Saat döngüsü içindeki tanıma başlangıç saati uzaklığı (1 değer çizgisi = 100 nanosaniye). Uzaklık akış göre başlangıcıdır.
* `audioTimeSize`: Tanıma Tick (100 nanosaniye) cinsinden süre.
* `audioStreamPosition`: Tanıma başlangıcı bayt uzaklığı. Akış başlangıcına göre uzaklığı var.
* `audioSizeBytes`: Tanıma bayt cinsinden boyutu.

Ses akışı tanıma konumlandırma sonuçları varsayılan olarak dahil değil unutmayın. İstemci tarafından TimingInfo özellik seçilmelidir (özellikleri parametre bakın).

Bir örnek nihai sonucu aşağıdaki gibidir:

```
{
  type: "partial"
  id: "23.2",
  recognition: "what was",
  translation: "translation of what was",
  audioStreamPosition: 319680,
  audioSizeBytes: 25840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 11900000
}
```

### <a name="text-to-speech"></a>Metin okuma
Metin okuma özelliği etkinleştirildiğinde (bkz `features` parametre aşağıdaki), nihai sonucu konuşulan çevrilen metnin ses tarafından izlenir. Ses verisi öbekli ve hizmetten Binary türünde Websocket iletiler dizisi olarak istemciye gönderilen. Bir istemci, her iletinin FIN bit denetleyerek akışın sonuna algılayabilir. Son ikili ileti bir akışın sonuna belirten, bit FIN kümesine sahip olursunuz. Akış biçimi değerine bağlıdır `format` parametresi.

### <a name="closing-the-connection"></a>Bağlantı kesiliyor
Bir istemci uygulaması ses akışı tamamlandı ve son nihai sonucu aldı, WebSocket kapatma el sıkışması başlatarak bağlantı kapatmalısınız. Bağlantıyı sonlandırmak sunucu neden olan koşul vardır. Aşağıdaki WebSocket kapalı kodları, istemci tarafından alınan:

* `1003 - Invalid Message Type`: Sunucunun aldığı veri türü kabul edemez çünkü bağlantı sonlandırılıyor. Bu genellikle, gelen sesi uygun bir üst bilgisi ile başlamıyor gerçekleşir.
* `1000 - Normal closure`: Sonra isteği yerine getiren bağlantıyı kapattı. Sunucu bağlantı kapatılacak: ses yok zaman; uzun bir süre için istemci tarafından alındığında sessizlik uzun bir süre için akışa; oturum, izin verilen en uzun süreyi (yaklaşık 90 dakika) ulaştığında.
* `1001 - Endpoint Unavailable`: Sunucu kullanılamayacak gösterir. İstemci uygulaması, yeniden deneme sayısına bir sınır ile bağlanmayı deneyebilir.
* `1011 - Internal Server Error`: Sunucuda bir hata nedeniyle sunucu tarafından bağlantı kapatılacak.

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:---|:---|:---|:---|:---|
|API sürümü|1.0|İstemci tarafından istenen API sürümü. İzin verilen değerler: `1.0`.|query   |string|
|from|(boş)   |Gelen konuşma dilini belirtir. Değer dil tanımlayıcılardan biridir `speech` dilleri API yanıtından kapsam.|query|string|
|-|(boş)|Transcribed metne çevrilecek dilini belirtir. Değer dil tanımlayıcılardan biridir `text` dilleri API yanıtından kapsam.|query|string|
|SaaS Uygulamaları Geliştirme|(boş)   |Virgülle ayrılmış istemci tarafından seçilen özellikler kümesidir. Kullanılabilir özellikler şunlardır:<ul><li>`TextToSpeech`: hizmet son çevrilen cümlenin çevrilmiş ses döndürmesi gerektiğini belirtir.</li><li>`Partial`: hizmet ses hizmete akışa sırasında Ara tanıma sonuçları döndürmesi gerektiğini belirtir.</li><li>`TimingInfo`: Hizmet her tanıma ile ilişkili zamanlama bilgilerini döndürmesi gerektiğini belirtir.</li></ul>Örneğin, bir istemci belirtirsiniz `features=partial,texttospeech` kısmi sonuçlar ve metin okuma, ancak hiçbir zamanlama bilgilerini almak için. Son sonuçları istemciye her zaman akışa unutmayın.|query|string|
|Ses|(boş)|Hangi sesli metin okuma çevrilmiş metin işleme için kullanılacağını tanımlar. Değer dilleri API yanıtından tts kapsamda ses tanımlayıcılardan biridir. Bir ses, sistem otomatik olarak ayarlanır belirtilmezse metin okuma özelliği etkinleştirilmişse seçin.|query|string|
|format|(boş)|Hizmet tarafından döndürülen metin okuma ses akışı biçimini belirtir. Kullanılabilen seçenekler:<ul><li>`audio/wav`: Oluşturulan dalga biçiminin ses akışı. İstemci, ses biçimi doğru şekilde yorumlamasına WAV başlığı kullanmanız gerekir. Metin okuma için WAV ses tek kanal PCM 24 kHz veya 16 kHz örnekleme oranını 16 bit ' dir.</li><li>`audio/mp3`: MP3 ses akışı.</li></ul>`audio/wav` varsayılan değerdir.|query|string|
|ProfanityAction    |(boş)    |Hizmet konuşma dilinde tanınan profanities nasıl işleyeceğini belirtir. Geçerli eylemler şunlardır:<ul><li>`NoAction`: Profanities olduğu gibi bırakılır.</li><li>`Marked`: Profanities işaretçisi ile değiştirilir. Bkz: `ProfanityMarker` parametresi.</li><li>`Deleted`: Profanities silinir. Örneğin, word `"jackass"` ifadesinin bir küfür kabul edilir `"He is a jackass."` olur `"He is a .".`</li></ul>Varsayılan olarak işaretlenmiş.|query|string|
|ProfanityMarker|(boş)    |Nasıl algılanan profanities belirtir ne zaman işleneceğini `ProfanityAction` ayarlanır `Marked`. Geçerli seçenekler şunlardır:<ul><li>`Asterisk`: Profanities dize ile değiştirilir `***`. Örneğin, word `"jackass"` ifadesinin bir küfür kabul edilir `"He is a jackass."` olur `"He is a ***.".`</li><li>`Tag`: Küfür küfür XML etiketi tarafından çevrilmiş. Örneğin, word `"jackass"` ifadesinin bir küfür kabul edilir `"He is a jackass."` olacak `"He is a <profanity>jackass</profanity>."`.</li></ul>Varsayılan değer: `Asterisk`.|query|string|
|Yetkilendirme|(boş)  |İstemcinin taşıyıcı belirteç değerini belirtir. Önek kullanması `Bearer` değeri tarafından izlenen `access_token` kimlik doğrulama belirteci hizmet tarafından döndürülen değer.|üst bilgi   |string|
|Ocp-Apim-Subscription-Key|(boş)|Gerekli if `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|
|access_token|(boş)   |Geçerli bir OAuth erişim belirteci geçirmek için alternatif bir yolu. Taşıyıcı belirteç genellikle üstbilgiyle sağlanan `Authorization`. Bazı websocket kitaplıklar, üst bilgilerini ayarlayacak şekilde istemci kodu izin vermeyin. Böyle bir durumda istemcinin kullanabileceği `access_token` sorgu parametresi geçerli bir belirteç geçirilecek. Varsa, kimlik doğrulamak için bir erişim belirteci kullanarak `Authorization` üst bilgisi ayarlanmadı, ardından `access_token` ayarlamanız gerekir. Hem üst hem de sorgu parametresi ayarlarsanız, sorgu parametresi göz ardı edilir. İstemcileri yalnızca belirtecin geçip için bir yöntem kullanmanız gerekir.|query|string|
|Abonelik anahtarı|(boş)   |Abonelik anahtarı geçirmek için alternatif bir yolu. Bazı websocket kitaplıklar, üst bilgilerini ayarlayacak şekilde istemci kodu izin vermeyin. Böyle bir durumda istemcinin kullanabileceği `subscription-key` sorgu parametresi geçerli bir abonelik anahtarı geçirilecek. Varsa, kimlik doğrulamak için bir abonelik anahtarı kullanarak `Ocp-Apim-Subscription-Key` üst bilgisi ayarlanmadı sonra abonelik anahtarı ayarlanmalıdır. Hem üst hem de sorgu parametresi ayarlarsanız, sorgu parametresi göz ardı edilir. İstemcileri yalnızca kullanması gereken yöntemini geçirilecek `subscription key`.|query|string|
|X-ClientTraceId    |(boş)    |Bir istek izleme için kullanılan istemci tarafından oluşturulan GUID. Doğru sorunlarını gidermek için istemciler her bir istekle yeni bir değer sağlayın ve oturumu.<br/>Üst bilgi kullanmak yerine, bu değer ile sorgu parametresi geçirilebilir `X-ClientTraceId`. Hem üst hem de sorgu parametresi ayarlarsanız, sorgu parametresi göz ardı edilir.|üst bilgi|string|
|X-Correlationıd|(boş)    |Bir konuşma birden çok kanalda ilişkilendirmek için kullanılan istemci tarafından oluşturulan bir tanımlayıcı. Birden çok konuşma çevirisi oturumlar, kullanıcılar arasında yapılan görüşmeler etkinleştirmek için oluşturulabilir. Böyle bir senaryoda tüm konuşma çevirisi oturumları kanalları birbirine bağlamak için aynı bağıntı Kimliğini kullanın. Bu, izleme ve tanılamayı kolaylaştırır. Tanımlayıcı için uygun olmalıdır: `^[a-zA-Z0-9-_.]{1,64}$`<br/>Üst bilgi kullanmak yerine, bu değer ile sorgu parametresi geçirilebilir `X-CorrelationId`. Hem üst hem de sorgu parametresi ayarlarsanız, sorgu parametresi göz ardı edilir.|üst bilgi|string|
|X-ClientVersion|(boş)    |İstemci uygulama sürümünü tanımlar. Örnek: "2.1.0.123".<br/>Üst bilgi kullanmak yerine, bu değer ile sorgu parametresi geçirilebilir `X-ClientVersion`. Hem üst hem de sorgu parametresi ayarlarsanız, sorgu parametresi göz ardı edilir.|üst bilgi|string|
|X-OsPlatform|(boş)   |İstemci uygulamasının üzerinde çalıştığı işletim sistemi sürümü ve adını tanımlar. Örnekler: "Android 5.0", "iOs 8.1.3", "Windows 8.1".<br/>Üst bilgi kullanmak yerine, bu değer ile sorgu parametresi geçirilebilir `X-OsPlatform`. Hem üst hem de sorgu parametresi ayarlarsanız, sorgu parametresi göz ardı edilir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|Yanıt modeli|Üst bilgiler|
|:--|:--|:--|:--|
|101    |WebSocket yükseltin.|Model örnek değer <br/> Nesne {}|X-RequestId<br/>Sorun giderme amacıyla isteği tanımlayan bir değer.<br/>string|
|400    |Hatalı istek. Geçerli olduklarından emin olmak için giriş parametrelerini denetleyin. Yanıt nesnesini hatanın daha ayrıntılı bir açıklama içerir.|||
|401    |Yetkisiz. Kimlik bilgileri belirlenen, geçerli olduğunu ve iyi durumda olduğunuzu kullanılabilir bir bakiye ile Azure veri marketi aboneliğinizin olduğundan emin olun.|||
|500    |Bir hata oluştu. Sorun devam ederse, istemci izleme tanımlayıcısı (X-ClientTraceId) rapor ya da istek tanımlayıcısı (X-RequestId).|||
|503    |Sunucu geçici olarak kullanılamıyor. Lütfen isteği yeniden deneyin. Sorun devam ederse, istemci izleme tanımlayıcısı (X-ClientTraceId) rapor ya da istek tanımlayıcısı (X-RequestId).|||
