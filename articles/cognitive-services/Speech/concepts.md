---
title: Bing konuşma kavramları | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Microsoft konuşma hizmeti için kullanılan temel kavramları.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c114c726bea34465972a282acac6b8acbbf9a80f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60514970"
---
# <a name="basic-concepts"></a>Temel kavramlar

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bu sayfa, Microsoft konuşma tanıma hizmeti bazı temel kavramları açıklar. Bu sayfa, Microsoft konuşma tanıma API'si, uygulamanızda kullanmadan önce okumanızı öneririz.

## <a name="understanding-speech-recognition"></a>Konuşma tanıma anlama

Bu konuşma özellikli bir uygulama oluşturuyorsanız ilk kez kullanıyorsanız veya varolan bir uygulamaya Konuşma özelliklerini ekliyoruz ilk kez ise, bu bölümde, başlamanıza yardımcı olur. Konuşma özellikli uygulamaların biraz deneyim zaten varsa, bu bölüm yalnızca atlamanız seçebilir ya da tamamen bir konuşma eski elinizin işiniz ve protokolü ayrıntılara doğru hale getirmek istiyorsanız bunu atlayabilirsiniz.

### <a name="audio-streams"></a>Ses akışları

Konuşma temel kavramlarını arasında yapısıdır *ses akışı*. Tek bir noktada saatinden ve tek bir bilgiyi içeren bir tuş vuruşu aksine konuşmada geçen bir istek yüzlerce milisaniye yayılan ve birçok kilobayt bilgileri içerir. Konuşulan konuşma süresi, kendi uygulama kolaylaştırılmış ve şık konuşma deneyimi sağlamak isteyen geliştiriciler için bazı güçlük sunar. Günümüzün bilgisayarlar ve algoritmaları konuşma transkripsiyonu utterance süresi yaklaşık yarısını 2 saniyelik utterance yaklaşık 1 saniye içinde transcribed, ancak 1 saniye gecikmeden işleme kullanıcı deneyimleri herhangi bir uygulama için gerçekleştirin Kolaylaştırılmış ne Zarif.

Neyse ki, vardır, "kullanıcı başka bir bölümü konuştuğunu sırada utterance bir parçası üzerinde döküm gerçekleştirerek transkripsiyonu zaman gizleme". Kullanıcı farkında değildir Örneğin, bir 1 saniye utterance 100 milisaniye cinsinden süre 10 parçalara bölmek ve döküm her bir öbeği sırasıyla yaparak, üzerinde döküm için gereken toplam 500 milisaniye "gizlenebilir" 450 transkripsiyonu, böylelikle derse Konuşmayı sırada gerçekleştirilmekte olan. Bu örnek hakkında düşünmeye unutmayın hizmeti kullanıcı, sonraki 100 Konuşmayı sırada ses önceki 100 milisaniye transkripsiyonu gerçekleştiriyor, kullanıcı Konuşmayı durduğunda bu nedenle hizmet yalnızca kabaca 100 konuşmaların gerekir bir sonuç üretmek için ses milisaniye.

Bu kullanıcı deneyimi elde etmek için konuşulan ses bilgi yığınlar toplanır ve kullanıcı konuştukça transcribed. Bu ses öbekleri topluca gelen *ses akışı*, ve bu ses öbekleri hizmete gönderme işlemini çağrılır *ses akışı.* Ses akış, herhangi bir konuşma özellikli uygulamanın önemli bir parçasıdır; Öbek boyutu ayarlama ve akış uygulaması en iyi duruma getirme, uygulamanızın kullanıcı deneyimini geliştiriyor en etkili yolları bazılarıdır.

### <a name="microphones"></a>Mikrofon

Kişi kendi kulağına kullanarak konuşulan ses işlem ancak inanimate donanım mikrofonlar kullanır. Konuşma herhangi bir uygulamada etkinleştirmek için uygulamanız için ses akışı sağlama mikrofon tümleştirmek gerekir.

Mikrofon yönelik API'ler, ses bayt mikrofondan durdurmak ve başlatmak izin vermeniz gerekir. Hangi kullanıcı eylemlerini konuşma dinlemeye başla mikrofon tetikleyecek karar vermeniz gerekir. Bir düğme basma dinleme başlangıç tetiklemek seçebilir ya da tercih bir *anahtar sözcük* veya *word Uyandırma* spotter mikrofon ve çıkış için bu modülü kullanmak için her zaman dinleme Microsoft konuşma hizmeti için gönderme ses tetikleyin.

### <a name="end-of-speech"></a>Konuşma sonu

Algılama *olduğunda* Konuşmacı sahip *durduruldu* Konuşmayı insanlar için yeterince basit gibi görünüyor ancak laboratuar koşullar dışında çok zor bir sorundur. Bu genellikle çok fazla şey genişlemesiyle için ortam gürültü olduğundan için saf sessizlik bir utterance sonra yalnızca aramak yeterli değildir. Microsoft konuşma hizmeti hızlı bir şekilde açıklamak gerekirse, bir kullanıcı durdurdu ve bunu, uygulamanızın hizmeti bilgilendirebilir, ancak bu düzenleme uygulamanızın son kullanıcı ne zaman durdurma Konuşmayı bilmek olduğu anlamına gelir algılama mükemmel bir işi yapar. Bu, uygulamanızın olduğu tüm diğer giriş biçimlerini gibi değildir *ilk* ne zaman başlar kullanıcının girişinin bilmek *ve* sona erer.

### <a name="asynchronous-service-responses"></a>Zaman uyumsuz hizmet yanıtları

Kullanıcı girişi tamamlandığında, bilgilendirilmesi uygulamanızın ihtiyaç duyduğu gerçek herhangi bir performans cezaları ya da uygulamanızın programlama bizden koymaz, ancak farklı giriş istekten konuşma istekleriyle ilgili düşündüğünüz gerektirmez / yanıt desenleri ile ilgili bilgi sahibi olduğunuz. Kullanıcının dikte durduğunda, uygulamanızın bilemezsiniz olduğundan, uygulamanızı hizmetinden gelen yanıt üzerinde aynı anda ve zaman uyumsuz olarak beklenirken hizmetine ses akışı devam etmeniz gerekir. Bu düzen, diğer gibi HTTP istek/yanıt web protokolleri benzemez. Bu protokolleri, herhangi bir yanıt almadan önce bir isteği tamamlaması gerekir; Microsoft konuşma hizmeti protokolünde yanıtlar almasına *ses isteği için akışa devam ederken*.

> [!NOTE]
> Bu özellik, konuşma HTTP REST API kullanırken desteklenmiyor.

### <a name="turns"></a>Kapatır

Konuşma bilgilerinin bir taşıyıcı ' dir. Konuşurken, elinizde bu bilgileri dinlediği bir kişiye bilgileri aktarmaya çalışıyorsunuz. Bilgi taşımak, genellikle konuşma ve bekleyen kapatır alır. Benzer şekilde, uygulamanızın genellikle çoğu dinleme yapmasına rağmen konuşma özellikli uygulamanızın kullanıcılarla dönüşümlü olarak dinleme ve yanıt, etkileşim. Kullanıcının okunan giriş ve bu giriş hizmeti yanıtı çağırıldı bir *kapatma*. A *kapatma* kullanıcı konuşacak ve uygulamanızı konuşma hizmeti yanıtı işlenmesi tamamlandığında sona başlatılır.

### <a name="telemetry"></a>Telemetri

Konuşma özellikli cihaz veya uygulama oluşturma, hatta deneyimli geliştiriciler için zor olabilir. Stream dayanan protokoller genellikle ilk bakışta göz korkutucu ve sessizlik algılama tamamen yeni gibi önemli ayrıntıları gibi görünüyor. Bir tek istek/yanıt çifti tamamlanması başarıyla gönderilmesine ve alınmasına gerek kalmadan çok sayıda iletilerle olduğu *çok* bu iletileri hakkında doğru ve eksiksiz veri toplamak önemli. Microsoft konuşma hizmeti protokolü, bu verilerin toplanması için sağlar. Gerekli verileri mümkün olduğunca doğru bir şekilde sağlamak için her çabayı; tam ve doğru verileri sağlayarak, kendiniz--yardımcı olacağını gerektiğinde Yardım istemci uygulamanız ilgili sorunu giderme Microsoft konuşma hizmeti ekibi, toplanan telemetri verilerini kalitesini sorun için kritik olacaktır analizi.

> [!NOTE]
> Bu özellik, konuşma tanıma REST API kullanırken desteklenmiyor.

### <a name="speech-application-states"></a>Konuşma uygulama durumları

Uygulamanızdaki Konuşma giriş etkinleştirmek için uygulayacağınız adımlar, fare tıkladığında veya dokunduğunda parmak gibi diğer giriş biçimlerini adımları biraz farklı. Uygulamanızı mikrofonun dinleme ve hizmetten yanıt beklerken ve boş bir durumda olduğunda konuşma hizmeti için veri gönderen takip gerekir. Aşağıdaki diyagramda bu durumlar arasındaki ilişki gösterilmektedir.

![Konuşma uygulama durumu diyagramı](Images/speech-application-state-diagram.png)

Microsoft konuşma hizmeti bazı durumların katılan olduğundan, hizmeti Protokolü uygulama geçişinizi durumlar arasında Yardım iletileri tanımlar. Yorumlar ve konuşma uygulama durumlarını yönetmek ve izlemek için bu iletişim kuralı iletileri hareket uygulamanız gerekir.

## <a name="using-the-speech-recognition-service-from-your-apps"></a>Konuşma tanıma hizmeti uygulamalarınızdan gelen kullanma

Microsoft konuşma tanıma hizmeti, geliştiricilerin, uygulamalarına konuşma eklemek iki yol sağlar.

- [REST API'leri](GetStarted/GetStartedREST.md): Geliştiriciler, konuşma tanıma hizmeti uygulamalarını HTTP çağrıları kullanabilir.
- [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md): Gelişmiş özellikler için geliştiriciler Microsoft Speech istemci kitaplıklarını indirin ve kendi uygulamalarınızda bağlantı.  İstemci kitaplıkları (C#, Java, JavaScript, ObjectiveC) farklı dilleri kullanan çeşitli platformlarda (Windows, Android, iOS) kullanılabilir.

| Uygulama alanları | [REST API'ler](GetStarted/GetStartedREST.md) | [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Dönüştürme kısa konuşmada geçen bir ses (ses uzunluğu < 15 s) geçiş sonuçları gibi komutları. | Evet | Evet |
| Uzun bir ses (> 15 s) dönüştürme | Hayır | Evet |
| İstenen Ara sonuçlarla Stream ses | Hayır | Evet |
| LUIS kullanarak seslerden Dönüştürülen metin anlama | Hayır | Evet |

 Diliniz veya platformunuz henüz bir SDK yoksa, kendi uygulama temel oluşturabilirsiniz [Protokolü belgeleri](API-Reference-REST/websocketprotocol.md).

## <a name="recognition-modes"></a>Tanıma modları

Tanıma üç mod vardır: `interactive`, `conversation`, ve `dictation`. Konuşma tanıma nasıl kullanıcıları duyma olasılığınız olan bağlı tanıma modu ayarlar. Uygulamanız için uygun tanıma modunu seçin.

> [!NOTE]
> WebSocket Protokolü arkadaşlarınıza kıyasla tanıma modları REST Protokolü farklı davranışları olabilir. Örneğin, REST API sürekli tanıma, hatta konuşma veya Dikte modunda desteklemez.
> [!NOTE]
> Bu mod, doğrudan REST veya WebSocket protokolünü kullanırken geçerlidir. [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md) tanıma modunu belirtmek üzere farklı parametreler kullanın. Daha fazla bilgi için seçtiğiniz istemci kitaplığı bakın.

Microsoft konuşma hizmeti tüm tanıma modları için yalnızca bir tanıma ifade sonucunu döndürür. Herhangi bir tek utterance için 15 saniyelik bir sınır yoktur.

### <a name="interactive-mode"></a>Etkileşimli mod

İçinde `interactive` modu, kullanıcının kısa isteği yapan ve yanıt olarak bir eylemi gerçekleştirmek için uygulama bekler.

Aşağıdaki özelliklere etkileşimli mod uygulamaları tipik şunlardır:

- Kullanıcılar, başka bir insan değil de, bir makineye iletişim kurmanızı bildirin.
- Uygulama kullanıcıları, önceden varsayalım, alan uygulama yapmak istediğini üzerinde istedikleri bildirin.
- Konuşma son hakkında genellikle 2-3 saniye.

### <a name="conversation-mode"></a>Konuşma modu

İçinde `conversation` modu, kullanıcıların insanlar konuşmada gerçekleştiriliyor.

Aşağıdaki özelliklere konuşma modu uygulamalarının tipik şunlardır:

- Kullanıcılar başka bir kişiye konuşma olduğunu bilirsiniz.
- Konuşma tanıma, konuşulan metnin görmek bir veya iki katılımcıları vererek İnsan konuşmalar geliştirir.
- Kullanıcılar, söylemek istediği her zaman planlamıyorsanız.
- Kullanıcıların sık argo ve resmi olmayan diğer konuşma kullanın.

### <a name="dictation-mode"></a>Dikte modu

İçinde `dictation` modu, kullanıcılar uygulamaya daha fazla işleme için uzun konuşma belirtmesini.

Aşağıdaki özelliklere dikte modu uygulamalarının tipik şunlardır:

- Kullanıcılar için bir makine konuşma olduğunu bilirsiniz.
- Kullanıcılara, konuşma tanıma metin sonuçları gösterilir.
- Söyleyin ve daha resmi bir dil kullanmak istedikleri kullanıcıları genellikle planlayın.
- Kullanıcılar kullanan tam, son 5-8 saniye cümleler.

> [!NOTE]
> Yazdırma ve konuşma modlarında Microsoft konuşma hizmeti kısmi sonuçlar döndürmez. Bunun yerine, hizmet sessizlik sınırları ses akışına sonra kararlı tümcecik sonuçlarını döndürür. Microsoft, bu sürekli tanıma modda kullanıcı deneyimini iyileştirmek üzere konuşma Protokolü geliştirmek.

## <a name="recognition-languages"></a>Tanıma dilleri

*Tanıma dil* uygulama kullanıcınızın konuşur dili belirtir. Belirtin *tanıma dil* ile *dil* bağlantı URL'si sorgu parametresi. Değerini *dil* sorgu parametresini kullanır IETF dil etiketi [BCP 47](https://en.wikipedia.org/wiki/IETF_language_tag), ve **gerekir** konuşma tanıma API'si tarafından desteklenen dilleri, biri. Konuşma hizmeti tarafından desteklenen dillerin tam listesi sayfasında bulunabilir [desteklenen dilleri](API-Reference-REST/supportedlanguages.md).

Microsoft konuşma hizmeti görüntüleyerek geçersiz bağlantı istekleri reddeden bir `HTTP 400 Bad Request` yanıt. Geçersiz bir istek biridir:

- İçermemesi bir *dil* sorgu parametresi değeri.
- İçeren bir *dil* sorgu parametresi yanlış biçimlendirilmiş.
- İçeren bir *dil* sorgu desteği dillerden biri değil parametresi.

Birini veya tümünü hizmeti tarafından desteklenen dilleri destekleyen bir uygulama oluşturmak tercih edebilirsiniz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte, bir uygulamanın kullandığı *konuşma* ABD İngilizce Konuşmacı için konuşma tanıma modu.

```HTTP
https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

## <a name="transcription-responses"></a>Döküm yanıtları

Döküm yanıtları Dönüştürülen metin gelen istemcilere döndürür. Bir döküm yanıt aşağıdaki alanları içerir:

- `RecognitionStatus` tanıma durumunu belirtir. Olası değerler aşağıdaki tabloda verilmiştir.

| Durum | Açıklama |
| ------------- | ---------------- |
| Başarılı | Tanıma başarılı oldu ve görüntü metni alanı var. |
| NoMatch | Konuşma Tanıma Ses akışında algılandı, ancak hiçbir hedef dil sözcükleri eşleştirilmiş olan. [NoMatch tanıma Status(#nomatch-recognition-status) daha fazla ayrıntı için bkz.  |
| InitialSilenceTimeout | Ses akışı başlangıcını yalnızca sessizlik ve konuşma için beklerken zaman aşımına hizmeti yer alan |
| BabbleTimeout | Ses akışı başlangıcını yalnızca gürültü ve konuşma için beklerken zaman aşımına hizmeti yer alan |
| Hata | Tanıma hizmeti bir iç hatayla karşılaştı ve çalışmaya devam edemedi |

- `DisplayText` tanınan tümceciği, büyük/küçük harf, noktalama işaretleri ve ters metin normalleştirme uygulanmış sonra yıldız işareti ile küfür maskelenmiş temsil eder. Görüntü metni alanı yoksa *yalnızca* varsa `RecognitionStatus` alan değerine sahip `Success`.

- `Offset` başlangıçtan ifadesinin, ilgili ses akışı başlangıcını tanınan uzaklığını (100 nanosaniyelik cinsinden) belirtir.

- `Duration`Bu konuşma deyimi süresi (100 nanosaniyelik cinsinden) belirtir.

Bir döküm yanıt isterseniz daha fazla bilgi döndürür. Bkz: [çıkış biçimi](#output-format) nasıl daha ayrıntılı çıktı döndürülecek.

Microsoft konuşma hizmeti küfür maskeleme ve sık karşılaşılan biçimlerinden metne normalleştirme büyük/küçük harf ve noktalama işaretleri, ekleme içeren ek döküm işlemini destekler. Örneğin, bir kullanıcı bir ifade tarafından temsil edilen verirse sözcükler "altı iPhone satın hatırlat", Microsoft konuşma Hizmetleri transcribed metni döndürür "iPhone 6 satın hatırlat." Word'ün "altı" "6" sayıya işlem çağrılırken *ters metin normalleştirme* (*edemezsiniz* kısaca).

### <a name="nomatch-recognition-status"></a>NoMatch tanıma durumu

Döküm yanıtı döndürür `NoMatch` içinde `RecognitionStatus` kullandığınızda, Microsoft konuşma hizmeti konuşma ses akışı algılar ancak bu istek için kullanılan dil dilbilgisi konuşma eşleşecek şekilde izleyemiyor. Örneğin, bir *NoMatch* koşul tanıyıcı ABD İngilizce konuşulan dili olarak beklerken bir kullanıcının bir şey Almanca diyorsa oluşabilir. Utterance dalga desenini İnsan konuşma varlığını gösterir, ancak konuşulan sözcükleri hiçbiri tanıyıcı tarafından kullanılan ABD İngilizce sözlüğü eşleşir.

Başka bir *NoMatch* durum tanıma algoritması Ses akışında bulunan ses için doğru bir eşleşme bulamadı olduğunda meydana gelir. Bu durum gerçekleştiğinde, Microsoft konuşma hizmeti üretebilir *speech.hypothesis* içeren iletileri *metin onaylanmadığına* ancak oluşturacak bir *speech.phrase*hangi iletisinde *RecognitionStatus* olduğu *NoMatch*. Bu durum normaldir; metnin kalitesini ve doğruluğunu hakkında varsayımlar yapmamalısınız *speech.hypothesis* ileti. Microsoft konuşma hizmeti ürettiği için Ayrıca, kabul gerekir değil *speech.hypothesis* hizmet oluşturabildiği iletileri bir *speech.phrase* ile ileti  *RecognitionStatus* *başarı*.

## <a name="output-format"></a>Çıkış biçimi

Microsoft konuşma hizmeti yük biçimleri çeşitli transkripsiyonu yanıtlarını döndürebilir. Tüm yükleri JSON yapılardır.

İfade sonucu biçimi belirterek denetleyebilirsiniz `format` URL'si sorgu parametresi. Varsayılan olarak, hizmetin döndürdüğü `simple` sonuçları.

| Biçimlendir | Açıklama |
|-----|-----|
| `simple` | Tanıma durumu ve görüntüleme formu içinde tanınan metin içeren bir Basitleştirilmiş ifade sonucu. |
| `detailed` | Tanıma durumu ve en iyi N listesini ifade sonuçlar burada her ifade sonucu tüm dört tanıma formları ve bir güven puanı içerir. |

`detailed` Biçimi içeren [en iyi N değerleri](#n-best-values), ek olarak `RecognitionStatus`, `Offset`, ve `duration`, yanıt.

### <a name="n-best-values"></a>En iyi N değerleri

Dinleyiciler, İnsan veya makine, hiçbir zaman olabilir, heard belirli *tam olarak* ne konuşulan. Dinleyici atayabileceğiniz bir *olasılık* yalnızca bir utterance belirli bir yorumu için.

Başkalarının düğmelerinizi sık etkileşimde konuşurken normal koşullarında, algılamayı konuşulan sözcükleri, yüksek bir olasılık sahiptir. Makine tabanlı konuşma dinleyicileri çaba benzer doğruluğu seviyelerine ulaşmasını sağlamak ve doğru koşullarda [insanlar ile bunların denkliğini](https://blogs.microsoft.com/next/2016/10/18/historic-achievement-microsoft-researchers-reach-human-parity-conversational-speech-recognition/#sm.001ykosqs14zte8qyxj2k9o28oz5v).

Konuşma tanıma kullanılan algoritmalar bir utterance normal işlenmesi bir parçası olarak, alternatif ınterpretations keşfedin. Tek bir yorumu ile değiştiriliyor kanıt zorlamayı olduğunda genellikle, bu alternatifleri atılır. En iyi durumda olmayan koşullarında, ancak diğer olası ınterpretations listesini konuşma tanıyıcının tamamlanır. Üst *N* alternatifleri bu listedeki çağrılır *en iyi N listesi*. Her alternatif atanmış bir [güvenilirlik puanı](#confidence). 0 ile 1 arasında güvenle puanlar. Bir puan, 1, en yüksek güven düzeyini temsil eder. 0 puanı düşük güven düzeyini temsil eder.

> [!NOTE]
> Girdi sayısı bu değeri en iyi N listesinde birden çok konuşma arasında farklılık gösterir. Girdi sayısı birden çok tanıma arasında değişebilir *aynı* utterance. Bu farklılığa olasılıklara niteliği konuşma tanıma algoritması, bir doğal ve beklenen sonuç elde edilir.

En iyi N listesinde döndürülen her bir giriş içerir

- `Confidence`, temsil eden [güven puanları](#confidence) bu giriş.
- `Lexical`, olduğu [sözcük formu](#lexical-form) tanınan metin.
- `ITN`, olduğu [edemezsiniz form](#itn-form) tanınan metin.
- `MaskedITN`, olduğu [maskelenmiş edemezsiniz form](#masked-itn-form) tanınan metin.
- `Display`, olduğu [görüntüleme formu](#display-form) tanınan metin.

### Güven puanları <a id="confidence"></a>

Güven puanları, konuşma tanıma sistemlerine ayrılmaz. Microsoft konuşma hizmeti, gelen güven puanları alır bir *güvenle sınıflandırıcı*. Microsoft Güven sınıflandırıcı maximally doğru ve hatalı tanıma arasında ayırt etmek için tasarlanmış bir özellikler kümesini üzerinden eğitir. Güven puanları kelimeler ve tüm konuşma için değerlendirilir.

Hizmet tarafından döndürülen güven puanları kullanmayı seçerseniz, aşağıdaki davranışını dikkat edin:

- Güven puanları, aynı tanıma modu ve dil içinde yalnızca karşılaştırılabilir. Puanları farklı dil ya da farklı tanıma modları arasında karşılaştırma değil. Örneğin, bir güven puanı etkileşimli tanıma modunda olan *hiçbir* bağıntı Dikte modunda bir güven puanı için.
- Güven puanları, konuşma kısıtlı kümesine göre en iyi şekilde kullanılır. Doğal olarak çok sayıda konuşma puanları sonuçlarındaki önemli ölçüde yoktur.

Bir güven puanı değeri olarak kullanmayı seçerseniz bir *eşiği* uygulamanızı davranan üzerinde konuşma tanıma eşik değerleri oluşturmak için kullanın.

- Konuşma tanıma, konuşma, uygulamanız için temsili bir örnek üzerinde yürütün.
- Örnek kümesindeki her tanıma için güven puanlarını toplayın.
- Bu örnek için güvenle bazı Yüzdeliğini, eşik değerini temel.

Tek eşik değer, tüm uygulamalar için uygundur. Bir uygulama için bir kabul edilebilir bir güven puanı başka bir uygulama için kabul edilemez olabilir.

### <a name="lexical-form"></a>sözcük temelli form

Sözcük tanınan metin tam olarak nasıl bu utterance ve noktalama işaretleri veya büyük/küçük harf olmadan aktarılırken biçimidir. Örneğin, "1020 Kurumsal yolu" adresi sözcük biçiminde olacaktır *on yirmi Kurumsal şekilde*, böylece konuşulan varsayılarak. Sözcük temelli form cümlenin "anımsat 5 Kurşun Kalem satın almak için" olan *beş Kurşun Kalem satın hatırlat*.

Sözcük temelli form standart metin normalleştirme gerçekleştirmeniz gereken uygulamalar için en uygun seçenektir. Sözcük temelli form de işlenmemiş tanıma sözcükleri gerek duyan uygulamalar için uygundur.

Küfür hiçbir zaman sözcük biçiminde maskelenir.

### <a name="itn-form"></a>Edemezsiniz formu

Metin normalleştirme metin "canonical" başka bir forma bir biçimden diğerine dönüştürme işlemidir. Örneğin, telefon numarası "555-1212" için kurallı biçimi dönüştürülebilir *beş beş beş bir iki bir iki*. *Ters* metin normalleştirme (edemezsiniz) sözcükleri dönüştürülürken, bu işlemi tersine çevirir "beş beş beş bir iki bir iki" ters kurallı biçimi için *555-1212*. Bir tanıma işleminin sonucu edemezsiniz biçiminin büyük/küçük harf veya noktalama işareti içermez.

Edemezsiniz formun tanınan metin davranan uygulamalar için en uygun seçenektir. Örneğin, arama terimleri konuşmak açmasına olanak sağlar ve ardından bu terimleri web sorguda kullanan bir uygulama edemezsiniz formun kullanmanız gerekir. Küfür hiçbir zaman edemezsiniz biçiminde maskelenir. Küfür maske kullanılacağı *maskelenmiş edemezsiniz form*.

### <a name="masked-itn-form"></a>Maskeli edemezsiniz formu

Küfür doğal konuşma dilini bir parçası olduğundan, bunlar konuşulan Microsoft konuşma hizmeti gibi bir sözcük ve tümcecikleri tanır. Küfür ancak tüm uygulamalar, özellikle sınırlı, yetişkin olmayan kullanıcı hedef kitle ile uygulamalarınız için uygun olmaması.

Maskeli edemezsiniz formun ters metin normalleştirme formu maskeleme küfür geçerlidir. Küfür maske, küfür parametre değerine ayarlayın `masked`. Küfür maskelenir dilin küfür sözlüğe bir parçası olarak tanınan sözcükleri yıldız işareti ile değiştirilir. Örneğin: *5 satın hatırlat *** Kurşun Kalem*. Bir tanıma işleminin sonucu maskelenmiş edemezsiniz biçiminin büyük/küçük harf veya noktalama işareti içermez.

> [!NOTE]
> Küfür sorgu parametresi değeri ayarlanırsa `raw`, maskelenmiş edemezsiniz formun edemezsiniz formu ile aynıdır. Küfür olan *değil* maskelenir.

### <a name="display-form"></a>Görüntüleme formu

Noktalama işaretleri ve büyük/küçük harf Vurgu, duraklatma ve metin anlamak daha kolay hale getiren benzeri nerede bulunacağı yer sinyal. Görüntüleme formu tanıma sonuçları konuşulan metnin uygulamaları için en uygun form yapma, noktalama işaretleri ve büyük/küçük harf ekler.

Görüntüleme formu maskelenmiş edemezsiniz formun genişlettiğinden, küfür parametre değeri ayarlayabilirsiniz `masked` veya `raw`. Değer ayarlanmışsa `raw`, kullanıcı tarafından konuşulan herhangi küfür tanıma sonuçları içerir. Değer ayarlanmışsa `masked`, dilin küfür sözlüğe bir parçası olarak tanınan sözcükleri, yıldız işareti ile değiştirilir.

### <a name="sample-responses"></a>Örnek yanıt

Tüm yükleri JSON yapılardır.

Yük biçimi `simple` ifade sonucu:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Yük biçimi `detailed` ifade sonucu:

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

## <a name="profanity-handling-in-speech-recognition"></a>Konuşma tanıma küfür işleme

Microsoft konuşma hizmeti İnsan okuma, sözcük ve çoğu kişi "küfür" sınıflandırabilir deyimleri dahil olmak üzere tüm biçimlerinin tanır. Kullanarak hizmet küfür nasıl işlediğini denetleyebilirsiniz *küfür* sorgu parametresi. Varsayılan olarak, hizmet içinde küfür maskeleri *speech.phrase* sonuçlanır ve sonuç döndürmez *speech.hypothesis* küfür içeren iletileri.

| *Küfür* değeri | Açıklama |
| - | - |
| `masked` | Küfür yıldız işareti ile maskeler. Varsayılan davranıştır. |
| `removed` | Tüm sonuçları küfür kaldırır. |
| `raw` | Tanır ve küfür tüm sonuçları döndürür. |

### <a name="profanity-value-masked"></a>Küfür değeri `Masked`

Küfür maskelemek için ayarlayın *küfür* sorgu parametresi değeri *maskelenmiş*. Zaman *küfür* sorgu parametresi bu değere sahip olması veya bir istek için hizmet belirtilmemiş *maskeleri* küfür. Hizmet, yıldız işareti ile tanıma sonuçları küfür değiştirerek maskeleme gerçekleştirir. Küfür maskeleme işleme belirttiğinizde, hizmet olmayan döndürür *speech.hypothesis* küfür içeren iletileri.

### <a name="profanity-value-removed"></a>Küfür değeri `Removed`

Zaman *küfür* sorgu parametresinin değeri *kaldırıldı*, küfür hem de hizmet kaldırır *speech.phrase* ve *speech.hypothesis* iletileri. Aynı sonucu olan *küfür sözcükleri telaffuz konuşulmaz alacağı*.

#### <a name="profanity-only-utterances"></a>Yalnızca Küfürlü konuşma

Bir kullanıcı konuşmak *yalnızca* uygulama küfür kaldırılacak hizmetin yapılandırıldığında küfür. Tanıma modu ise bu senaryo için *dikte* veya *konuşma*, hizmet döndürmez bir *speech.result*. Tanıma modu ise *etkileşimli*, hizmet döndürür bir *speech.result* durum koduyla *NoMatch*.

### <a name="profanity-value-raw"></a>Küfür değeri `Raw`

Zaman *küfür* sorgu parametresinin değeri *ham*, hizmeti kaldırma veya her ikisinde küfür maske *speech.phrase* veya  *Speech.hypothesis* iletileri.
