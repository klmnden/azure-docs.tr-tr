---
title: Microsoft Çeviricisi konuşma API Başvurusu | Microsoft Docs
titleSuffix: Cognitive Services
description: Microsoft Çeviricisi konuşma API başvuru belgeleri.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jansko
ms.openlocfilehash: be8faddf56158de3399713c41638c0b913b4627e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355642"
---
# <a name="microsoft-translator-speech-api"></a>Microsoft Translator Konuşma Tanıma API’si

Bu hizmet, bir dilden diğerine konuşma konuşma başka bir dilde metne transcribe için akış bir API sunar. API geri çevrilen metin konuşma metin okuma özelliklerini de tümleşir. Microsoft Çeviricisi konuşma API Skype Çeviricisi görülen görüşmeleri gerçek zamanlı çevrilmesi gibi senaryolara olanak sağlar.

Microsoft Çeviricisi konuşma API ile istemci uygulamaları hizmetine konuşma ses akışı ve bir akış kaynağı dil ve hedef dilde kendi çevirisi tanınan metni içeren metin tabanlı sonuçları geri alabilirsiniz. Metin sonuçlarını, otomatik konuşma tanıma (ASR) gelen ses akışına derin sinir ağları tarafından uygulayarak oluşturulur. Ham ASR çıktı daha fazla kullanıcının istediğinden daha yakından yansıtmak için DoğruMetin adında yeni bir teknik tarafından geliştirilmiştir. Örneğin, DoğruMetin disfluencies (hmms ve coughs) ve geri yükleme uygun noktalama işaretleri ve büyük/küçük harf kaldırır. Maskeleme veya profanities hariç özelliği de dahil edilmiştir. Tanıma ve çeviri motorları özellikle konuşma konuşma işlemek için eğitilmiş olup. Konuşma çeviri hizmeti sessizlik algılama bir utterance sonuna belirlemek için kullanır. Sesli etkinliğinde bir duraklamadan sonra hizmeti yeniden tamamlanmış utterance nihai sonucu akışı sağlanacak. Hizmet ayrıca Ara teşekkürler ve bir utterance için çeviriler ediyor vermek geri kısmi sonuçlar gönderebilirsiniz. Son sonuçlar için hizmet konuşma (okuma) hedef dillerde konuşulan metinden sentezlemek olanağı sağlar. Metin okuma ses, istemci tarafından belirtilen biçimde oluşturulur. WAV ve MP3 biçimleri kullanılabilir.

Microsoft Çeviricisi konuşma API İstemci ve sunucu arasında bir tam çift yönlü iletişim kanalı sağlamak için WebSocket Protokolü yararlanır. Bir uygulama hizmeti kullanmak için aşağıdaki adımları gerektirir:

## <a name="1-getting-started"></a>1. Başlarken
Microsoft Çeviricisi metin gerekir API erişmek için [için Microsoft Azure'a kaydolun](translator-speech-how-to-signup.md).

## <a name="2-authentication"></a>2. Kimlik Doğrulaması

Kimlik doğrulaması için abonelik anahtarı kullanın. Microsoft Çeviricisi konuşma API kimlik doğrulamasının iki modlarını destekler:

* **Bir erişim belirteci kullanarak:** , uygulamanızda belirteci Hizmeti'nden bir erişim belirteci alın. Bilişsel hizmetler kimlik doğrulama hizmeti tarafından bir erişim belirteci almak için Microsoft Çeviricisi konuşma API abonelik anahtarınızı kullanın. Erişim belirteci 10 dakika için geçerlidir. Her 10 dakikada yeni bir erişim belirteci edinmek ve aynı erişimi kullanarak bu 10 dakika içinde yinelenen istekleri için belirteç tutun.

* **Abonelik anahtarı kullanarak doğrudan:** bir değer olarak, uygulamanızda abonelik anahtarınızı geçirmek `Ocp-Apim-Subscription-Key` üstbilgi.

Abonelik anahtarınızı ve erişim belirteci görünümünden gizli gizli olarak kabul eder.

## <a name="3-query-languages"></a>3. Sorgu dilleri
**Desteklenen diller geçerli kümesini dilleri kaynak sorgu.** [Diller kaynak](languages-reference.md) dil kümesini gösterir ve konuşma tanıma, metin çeviri ve okuma için kullanılabilir seslerini. Her bir dil veya sesli aynı dil veya sesli tanımlamak için Microsoft Çeviricisi konuşma API'si kullanan bir tanımlayıcı verilir.

## <a name="4-stream-audio"></a>4. Akış ses
**Bir bağlantı açmak ve hizmet ses akışı başlayın.** Hizmet URL'si `wss://dev.microsofttranslator.com/speech/translate`. Parametreler ve hizmet tarafından beklenen ses biçimlerini aşağıda açıklanan `/speech/translate` işlemi. Parametrelerden biri adım 2'deki erişim belirteci geçirmek için kullanılır.

## <a name="5-process-the-results"></a>5. İşlem sonuçları
**Geri hizmetinden akışı sonuçları işleyin.** Kısmi sonuçlar, son sonuçları ve metin okuma ses kesimleri biçimi belgelerinde açıklanan `/speech/translate` aşağıdaki işlemi.

Microsoft Çeviricisi konuşma API kullanımını gösteren kod örnekleri kullanılabilir [Microsoft Çeviricisi Github site](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Uygulama Notları

Bir konuşma çeviri oturumu /Speech/translate Establishes Al

### <a name="connecting"></a>Bağlanıyor
Hizmete bağlanmadan önce daha sonra bu bölümde verilen parametre listesini gözden geçirin. Bir örnek isteğidir:

`GET wss://dev.microsofttranslator.com/speech/translate?from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa&api-version=1.0`
`Ocp-Apim-Subscription-Key: {subscription key}`
`X-ClientTraceId: {GUID}`

İstek konuşulan İngilizce hizmete akışı ve olması İtalyanca çevrilen olduğunu belirtir. Her bir son tanıma sonucu bir metin okuma ses yanıt Elsa adlı kadın sesi ile oluşturur. İstek kimlik bilgilerini içerdiğine dikkat edin `Ocp-Apim-Subscription-Key header`. Ayrıca istek üstbilgisinde bir genel benzersiz tanımlayıcı ayarlayarak en iyi uygulama izleyen `X-ClientTraceId`. Böylece ortaya çıkan sorunları gidermek için kullanılabilir bir istemci uygulaması izleme kimliği oturum açmanız gerekir.

### <a name="sending-audio"></a>Ses gönderme
Bağlantı kurulduktan sonra istemci hizmete ses akışı başlar. İstemci yığınlar halinde ses gönderir. Her bir öbeğin ikili türde Websocket ileti kullanılarak aktarılır.

Ses giriş dalga biçiminin ses dosyası biçiminde olan (WAVE ya da daha yaygın olarak nedeniyle dosya adı uzantısını WAV olarak bilinir). İstemci uygulaması tek kanal, 16 kHz örneklenen imzalı 16 bit PCM ses akışını. İstemci tarafından akışı bayt ilk kümesi WAV üstbilgi içerir. Tek bir kanal 16 kHz örneklenen 16 bit PCM akış imzalı 44 baytlık üstbilgi gösterilmiştir:

|Uzaklık|Değer|
|:---|:---|
|0 - 3|"RIFF"|
|4 - 7|0|
|8 - 11|"DALGA"|
|12 - 15|"fmt"|
|16 - 19|16|
|20 - 21|1|
|22 - 23|1|
|24 - 27|16000|
|28 - 31|32000|
|32 - 33|2|
|34 - 35|16|
|36 - 39|"data"|
|40 - 43|0|

Toplam dosya boyutu (bayt 4-7) ve "verileri" (bayt 40-43) boyutunu sıfıra ayarlanır dikkat edin. Burada toplam boyutu mutlaka önceden bilinmiyor akış senaryosu için Tamam budur.

WAV (RIFF) üstbilgisi gönderdikten sonra istemci ses veri öbekleri gönderir. İstemci, genellikle sabit bir süre (örneğin akışı 100ms aynı anda ses) temsil eden sabit bir boyuta öbekleri akışı sağlanacak.

### <a name="final-result"></a>Nihai sonucu
Bir son konuşma tanıma sonuç bir utterance sonunda oluşturulur. Bir sonuç hizmetinden metin türünde WebSocket ileti kullanarak bunu istemciye iletilir. İleti içeriği aşağıdaki özelliklere sahip bir nesnenin JSON serileştirmesi şöyledir:

* `type`: Sonuç türü tanımlamak için dize sabiti. Son elde etmek için son değerdir.
* `id`: Tanıma sonucu atanan tanımlayıcı dize.
* `recognition`: Kaynak dil tanınan metin. Metin yanlış tanıma durumunda boş bir dize olabilir.
* `translation`: Hedef dilde çevrilmiş tanınan metni.
* `audioTimeOffset`: Ticks tanıma başlangıcı saat konumu (1 değer = 100 nanosaniye). Akış göre başlangıç uzaklığı.
* `audioTimeSize`: Tanıma çizgilerine (100 nanosaniye) cinsinden süre.
* `audioStreamPosition`: Tanıma başlangıcı bayt uzaklığı. Akış başlangıcından göre uzaklığı.
* `audioSizeBytes`: Tanıma bayt cinsinden boyutu.

Ses akışı tanıma konumlandırma sonuçlarında varsayılan olarak dahil değil unutmayın. `TimingInfo` Özelliği, istemci tarafından seçilmiş olması gerekir (bkz: `features` parametresi).

Bir örnek nihai sonucu şu şekildedir:

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
Kısmi veya Ara konuşma tanıma sonuçları istemciye varsayılan akışı gerçekleştirilmez. İstemci, bunları istemek için özellikler sorgu parametresini kullanabilirsiniz.

Kısmi bir sonuç hizmetinden metin türünde WebSocket ileti kullanarak bunu istemciye iletilir. İleti içeriği aşağıdaki özelliklere sahip bir nesnenin JSON serileştirmesi şöyledir:

* `type`: Sonuç türü tanımlamak için dize sabiti. Kısmi sonuçlar için kısmi değerdir.
* `id`: Tanıma sonucu atanan tanımlayıcı dize.
* `recognition`: Kaynak dil tanınan metin.
* `translation`: Hedef dilde çevrilmiş tanınan metni.
* `audioTimeOffset`: Ticks tanıma başlangıcı saat konumu (1 değer = 100 nanosaniye). Akış göre başlangıç uzaklığı.
* `audioTimeSize`: Tanıma çizgilerine (100 nanosaniye) cinsinden süre.
* `audioStreamPosition`: Tanıma başlangıcı bayt uzaklığı. Akış başlangıcından göre uzaklığı.
* `audioSizeBytes`: Tanıma bayt cinsinden boyutu.

Ses akışı tanıma konumlandırma sonuçlarında varsayılan olarak dahil değil unutmayın. TimingInfo özelliği istemci tarafından seçilmiş olması gerekir (özellikleri parametre bakın).

Bir örnek nihai sonucu şu şekildedir:

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
Metin okuma özelliği etkin olduğunda (bkz: `features` parametresi aşağıdaki), nihai sonucu konuşulan çevrilmiş metni ses tarafından izlenir. Ses veri öbekli ve hizmetten Websocket iletileri ve Binary türünde bir dizi istemciye gönderilebilir. Bir istemci, her iletinin FIN bit denetleyerek akışın sonuna algılayabilir. Son ikili ileti bit FIN kendine akışın sonuna belirtmek için bir tane gerekir. Akış biçimlerinin değerine bağlıdır `format` parametresi.

### <a name="closing-the-connection"></a>Bağlantı kesiliyor
Bir istemci uygulaması ses akışı sona erdi ve son nihai sonucu aldı, WebSocket kapanış el sıkışması başlatılıyor tarafından bağlantı kapatmalısınız. Sunucunun bağlantıyı sonlandırmasına neden olacak koşullar vardır. Aşağıdaki WebSocket kapalı kodları, istemci tarafından alınan:

* `1003 - Invalid Message Type`: Aldığı veri türü kabul edemez nedeni sunucu bağlantısı sonlandırıyor. Bu, genellikle gelen ses uygun üstbilgiyi ile başlamıyor ortaya çıkar.
* `1000 - Normal closure`: İstek yerine sonra bağlantıyı kapattı. Sunucu bağlantısı kapanacak: hiçbir ses saati; uzun bir süre için istemci tarafından alındığında ne zaman sessizlik uzun bir süre için sağlanacağına; bir oturum izin verilen en uzun süreyi (yaklaşık 90 dakika) ulaştığında.
* `1001 - Endpoint Unavailable`: Sunucunun kullanılamaz hale gelecek gösterir. İstemci uygulama, yeniden deneme sayısına bir sınır ile bağlanmayı deneyebilir.
* `1011 - Internal Server Error`: Sunucuda bir hata nedeniyle sunucu tarafından bağlantı kapatılacak.

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:---|:---|:---|:---|:---|
|API sürümü|1.0|İstemci tarafından istenilen API sürümü. İzin verilen değerler: `1.0`.|sorgu   |dize|
|başlangıç|(boş)   |Gelen konuşma dilini belirtir. Değer dil tanımlayıcılardan biri `speech` dilleri API yanıtından kapsamında.|sorgu|dize|
|-|(boş)|Transcribed metne çevirmek için dilini belirtir. Değer dil tanımlayıcılardan biri `text` dilleri API yanıtından kapsamında.|sorgu|dize|
|SaaS Uygulamaları Geliştirme|(boş)   |Virgülle ayrılmış istemci tarafından seçilen özellikler kümesidir. Kullanılabilir özellikler şunlardır:<ul><li>`TextToSpeech`: hizmet son çevrilmiş tümcenin çevrilmiş ses döndürmelidir belirtir.</li><li>`Partial`: hizmete ses akışı sırasında hizmet Ara tanıma sonuçlarını döndürme olduğunu belirtir.</li><li>`TimingInfo`: Hizmet her tanıma ile ilişkili zamanlama bilgileri döndürmelidir belirtir.</li></ul>Örnek olarak, bir istemci belirtirsiniz `features=partial,texttospeech` kısmi sonuçlar ve metin okuma, ancak hiçbir zamanlama bilgisini almak için. Son sonuçları istemciye her zaman akışı unutmayın.|sorgu|dize|
|Ses|(boş)|Çevrilmiş metni metin okuma işleme için kullanmak üzere hangi sesli tanımlar. Değer dilleri API yanıtından tts kapsamda sesli tanımlayıcılardan biridir. Sesli sistem otomatik olarak ayarlanır belirtilmezse metin okuma özelliği etkinleştirilmişse, birini seçin.|sorgu|dize|
|Biçimi|(boş)|Hizmet tarafından döndürülen metin okuma ses akışı biçimini belirtir. Kullanılabilir seçenekler şunlardır:<ul><li>`audio/wav`: Dalga biçiminin ses akışı. İstemci ses biçimi düzgün şekilde yorumlamaya WAV başlığı kullanmanız gerekir. Metin okuma için WAV ses 16, 24 kHz veya 16 kHz örnekleme oranını tek kanalıyla PCM bitidir.</li><li>`audio/mp3`: MP3 ses akışı.</li></ul>`audio/wav` varsayılan değerdir.|sorgu|dize|
|ProfanityAction    |(boş)    |Hizmet konuşma tanınan profanities nasıl yöneteceğini belirtir. Geçerli eylemler şunlardır:<ul><li>`NoAction`: Profanities olduğu gibi bırakılır.</li><li>`Marked`: Profanities işaretçisi ile değiştirilir. Bkz: `ProfanityMarker` parametresi.</li><li>`Deleted`: Profanities silinir. Örneğin, varsa word `"jackass"` bir uygunsuz metin, tümcecik kabul `"He is a jackass."` hale gelecek `"He is a .".`</li></ul>Varsayılan olarak işaretlenir.|sorgu|dize|
|ProfanityMarker|(boş)    |Nasıl algılanan profanities belirtir ne zaman işleneceğini `ProfanityAction` ayarlanır `Marked`. Geçerli seçenekler şunlardır:<ul><li>`Asterisk`: Profanities dize ile değiştirilir `***`. Örneğin, varsa word `"jackass"` bir uygunsuz metin, tümcecik kabul `"He is a jackass."` hale gelecek `"He is a ***.".`</li><li>`Tag`: Uygunsuz metin çevrelenen uygunsuz metin XML etiketi. Örneğin, varsa word `"jackass"` bir uygunsuz metin, tümcecik kabul `"He is a jackass."` olacak `"He is a <profanity>jackass</profanity>."`.</li></ul>Varsayılan değer `Asterisk`.|sorgu|dize|
|Yetkilendirme|(boş)  |İstemcinin taşıyıcı belirtecini değerini belirtir. Önek kullanması `Bearer` değeri tarafından izlenen `access_token` kimlik doğrulama belirteci hizmeti tarafından döndürülen değer.|üst bilgi   |dize|
|Ocp Apim abonelik anahtarı|(boş)|Gerekli olursa `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|
|access_token|(boş)   |Geçerli bir OAuth erişim belirteci geçirmek için alternatif bir yolu. Taşıyıcı belirteci genellikle üstbilgiyle sağlanan `Authorization`. Bazı websocket kitaplıkları üstbilgileri ayarlamak istemci kodu izin vermez. Böyle bir durumda istemcinin kullanabileceği `access_token` sorgu parametresi geçerli bir belirteci geçirin. Bir erişim belirteci kimliğini doğrulamak için kullanırken `Authorization` üstbilgi ayarlı değil, ardından `access_token` ayarlamanız gerekir. Üstbilgi ve sorgu parametresi ayarlarsanız, sorgu parametresi yoksayılır. İstemcileri yalnızca belirtecin geçip için bir yöntem kullanmanız gerekir.|sorgu|dize|
|Abonelik anahtarı|(boş)   |Abonelik anahtarı geçirmek için alternatif bir yolu. Bazı websocket kitaplıkları üstbilgileri ayarlamak istemci kodu izin vermez. Böyle bir durumda istemcinin kullanabileceği `subscription-key` sorgu parametresi geçerli bir abonelik anahtarı geçirirsiniz. Bir abonelik anahtar kimlik doğrulaması için kullanırken `Ocp-Apim-Subscription-Key` üstbilgisi ayarlanmamış sonra abonelik anahtarı ayarlanmalıdır. Üstbilgi ve sorgu parametresi ayarlarsanız, sorgu parametresi yoksayılır. İstemcileri yalnızca kullanması gereken bir yöntem geçirmek için `subscription key`.|sorgu|dize|
|X-ClientTraceId    |(boş)    |Bir istek izleme için kullanılan istemci tarafından oluşturulan bir GUID. Uygun sorunlarını gidermek için istemciler her istek ile yeni bir değer girin ve oturum.<br/>Üstbilgi kullanmak yerine, bu değer sorgu parametresi geçirilebilir `X-ClientTraceId`. Üstbilgi ve sorgu parametresi ayarlarsanız, sorgu parametresi yoksayılır.|üst bilgi|dize|
|X-correlationıd değeri|(boş)    |Birden çok kanaldan konuşmada ilişkilendirmek için kullanılan istemci tarafından oluşturulan bir tanımlayıcı. Birden çok konuşma çeviri oturumlar, kullanıcılar arasında görüşmeleri etkinleştirmek için oluşturulabilir. Bu senaryoda, tüm konuşma çeviri oturumlar aynı bağıntı kimliği kanallar birbirine bağlamak için kullanın. Bu, izleme ve tanılama kolaylaştırır. Tanımlayıcı için uygun olmalıdır: `^[a-zA-Z0-9-_.]{1,64}$`<br/>Üstbilgi kullanmak yerine, bu değer sorgu parametresi geçirilebilir `X-CorrelationId`. Üstbilgi ve sorgu parametresi ayarlarsanız, sorgu parametresi yoksayılır.|üst bilgi|dize|
|X-ClientVersion|(boş)    |İstemci uygulaması sürümü tanımlar. Örnek: "2.1.0.123".<br/>Üstbilgi kullanmak yerine, bu değer sorgu parametresi geçirilebilir `X-ClientVersion`. Üstbilgi ve sorgu parametresi ayarlarsanız, sorgu parametresi yoksayılır.|üst bilgi|dize|
|X-OsPlatform|(boş)   |İstemci uygulaması üzerinde çalıştığı işletim sistemi sürümü ve adını tanımlar. Örnekler: "Android 5.0", "iOs 8.1.3", "Windows 8.1".<br/>Üstbilgi kullanmak yerine, bu değer sorgu parametresi geçirilebilir `X-OsPlatform`. Üstbilgi ve sorgu parametresi ayarlarsanız, sorgu parametresi yoksayılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|Yanıt modeli|Üst bilgiler|
|:--|:--|:--|:--|
|101    |WebSocket yükseltin.|Model örnek değer <br/> Nesne {}|X-RequestId<br/>Sorun giderme amacıyla istek tanımlayan bir değer.<br/>dize|
|400    |Hatalı istek. Geçerli olduğundan emin olmak için giriş parametrelerini denetleyin. Yanıt nesnesini hatanın ayrıntılı açıklamasını içerir.|||
|401    |Yetkilendirilmedi. Kimlik bilgileri olarak ayarlanmadığından, geçerli olduğunu ve Azure veri Pazar aboneliğinizi kullanılabilir bakiye ile iyi yeri olduğundan emin olun.|||
|500    |Bir hata oluştu. Sorun devam ederse, istemci izleme tanımlayıcısı (X-ClientTraceId) rapor ya da istek tanımlayıcısıyla (X-RequestId).|||
|503    |Sunucu geçici olarak kullanılamıyor. Lütfen isteği yeniden deneyin. Sorun devam ederse, istemci izleme tanımlayıcısı (X-ClientTraceId) rapor ya da istek tanımlayıcısıyla (X-RequestId).|||

    


    





    
    




    




    




    

            




        









