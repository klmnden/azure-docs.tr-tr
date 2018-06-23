---
title: Kavramları | Microsoft Docs
description: Microsoft konuşma Service için kullanılan temel kavramları.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: bc23f4fb7dfc045a0f8cc87155c31875c4de8450
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352307"
---
# <a name="basic-concepts"></a>Temel kavramlar

Bu sayfa Microsoft konuşma tanıma hizmetinde bazı temel kavramları açıklar. Bu sayfa, uygulamanızda Microsoft konuşma tanıma API'si kullanmadan önce okumanızı öneririz.

## <a name="understanding-speech-recognition"></a>Konuşma tanıma anlama

Bu, konuşma etkin bir uygulama oluşturmakta olduğunuz ilk kez kullanıyorsanız veya varolan bir uygulamaya konuşma yetenekleri ekleme ilk kez çalıştırılıyorsa, bu bölümde başlamanıza yardımcı olur. Konuşma etkinleştirilmiş uygulamalarla biraz deneyim zaten varsa bu bölüm yalnızca atlamanız seçebilir veya tamamen konuşma en eski el iseniz ve protokol ayrıntılarını sağ almak istediğiniz atlayabilirsiniz.

### <a name="audio-streams"></a>Ses akışları

Konuşma temel kavramlarını arasında yapısıdır *ses akışı*. Tek bir noktada oluştuğunda ve tek bir bilgi içeren bir tuş vuruşu aksine bir konuşma isteği yüzlerce milisaniye yayılan ve birçok kilobayt bilgileri içerir. Konuşma utterances süresi, kendi uygulama için bir kolaylaştırılmış ve zarif konuşma deneyimi sağlamak isteyen geliştiriciler için bazı zorluk gösterir. Bugünün bilgisayarlar ve algoritmaları konuşma transcription utterance süresi yaklaşık yarısı 2 saniyelik utterance yaklaşık 1 saniye içinde transcribed ancak bir 1 saniye arasında bir gecikme işleme kullanıcı deneyimleri herhangi bir uygulama olduğundan gerçekleştirin Kolaylaştırılmış ne Zarif.

Neyse ki, yolları vardır "transcription zaman kullanıcı başka bir bölümü konuşarak sırada utterance bir parçası transcription gerçekleştirerek gizleme". Kullanıcı farkında olmasını sağlayın Örneğin, 100 milisaniye 10 parçalara 1 saniye utterance bölme ve gerçekleştirerek transcription her bir öbeğin sırayla, üzerinde transcription için gereken toplam 500 milisaniye "gizlenebilir" 450 transcription. klasöründe konuşarak sırada gerçekleştirilen. Bu örnekte, düşünmek unutmayın sonraki 100 kullanıcı konuşarak sırada hizmet transcription ses önceki 100 milisaniyede üzerinde çalıştığını, kullanıcı Konuşmayı durduğunda bu nedenle hizmet yalnızca kabaca 100 transcribe gerekir bir sonucu oluşturmak için ses milisaniye.

Bu kullanıcı deneyimi elde etmek için konuşma ses bilgi yığınlar halinde toplanır ve kullanıcı konuşur gibi transcribed. Bu ses öbekleri topluca gelen *ses akışı*, ve bu ses öbekleri hizmete gönderme işlemini adlı *ses akış.* Ses akışı, herhangi bir konuşma etkin uygulama önemli bir parçasıdır; Öbek boyutu ayarlama ve akış uygulaması en iyi duruma getirme, uygulamanızın kullanıcı deneyimini artırmak en etkili yollarından biri bazılarıdır.

### <a name="microphones"></a>Mikrofonlar

Kendi kulağına kullanarak konuşulan ses kişiler işlem ancak inanimate donanım mikrofonlar kullanır. Herhangi bir uygulamada konuşma etkinleştirmek için uygulamanız için ses akışı sağlama mikrofon ile tümleştirmek gerekir.

Mikrofonunuz API'leri mikrofonun ses bayt alma durdurmak ve başlatmak izin vermelidir. Hangi kullanıcı eylemlerini konuşma dinleme başlatmak için mikrofon tetikleyecek karar vermeniz gerekir. Dinleniyor başlangıcı tetikleyen bir düğme basma tercih veya tercih bir *anahtar sözcük* veya *word Uyandırma* mikrofon ve bu modül çıktısını kullanmak için her zaman dinleme spotter Microsoft konuşma hizmete gönderme ses tetikler.

### <a name="end-of-speech"></a>Konuşma sonu

Algılama *zaman* Konuşmacı sahip *durduruldu* konuşarak insanlar için yeterince basit görünüyor ancak laboratuar koşullar dışında yerine zor bir sorundur. Genellikle çok fazla şey karmaşık hale ortam gürültü olduğundan için saf sessizlik bir utterance sonra yalnızca aramak yeterli değil. Microsoft konuşma hizmeti hızlı bir şekilde bir kullanıcı Konuşmayı durduruldu ve hizmeti uygulamanız bu olayının bilgilendirebilir, ancak bu düzenlemenin, uygulamanızın en son ne zaman kullanıcı Durdur konuşarak bilmeniz olduğu anlamına algılama mükemmel bir işi yapar. Bu, uygulamanızın olduğu tüm diğer giriş biçimlerini gibi değil *ilk* kullanıcı başlatır zaman giriş bilmeniz *ve* sona erer.

### <a name="asynchronous-service-responses"></a>Zaman uyumsuz hizmet yanıtları

Kullanıcı girişi tamamlandığında, bilgi sahibi olmak için uygulamanız gereken olgu, herhangi bir performans cezaları veya programlama sorunlar, uygulamanızın üzerinde zorunlu tuttukları değil, ancak konuşma isteklerinden farklı giriş isteğinde yapılandırmanızda gerektirir / yanıt desenleri, size tanıdık olduğu. Kullanıcı konuşarak durduğunda, uygulamanızın bilemezsiniz olduğundan, uygulamanız aynı anda ve zaman uyumsuz olarak hizmetinden bir yanıt beklenirken hizmetine ses akışını sağlamak devam etmeniz gerekir. Bu desen HTTP gibi diğer istek/yanıt web protokolleri benzemez. Bu protokoller, herhangi bir yanıt almadan önce bir isteği tamamlamak gerekir; Microsoft konuşma hizmeti protokolünde yanıtları almak *istek için ses akışa hala durumdayken*.

> [!NOTE]
> Bu özellik, konuşma HTTP REST API'si kullanılırken desteklenmiyor.

### <a name="turns"></a>Kapatır

Konuşma bilgilerinin bir taşıyıcı ' dir. Konuşurken, bu bilgileri dinleme birisine elinizde olan bilgiler iletmek çalışıyorsunuz. Bilgileri açıklamak olduğunda, genellikle konuşarak ve dinleme kapatır alır. Benzer şekilde, uygulamanızın genellikle dinleme çoğu yapmasa da alternatif olarak dinleme ve yanıt, konuşma etkinleştirilmiş uygulamanızı kullanıcılarla etkileşim. Kullanıcının Konuşma giriş ve hizmet yanıt bu girişi olarak adlandırılan bir *kapatma*. A *kapatma* kullanıcı konuşur ve uygulamanızı konuşma hizmet yanıtı işlenmesi tamamlandığında sona erdiğinde başlatır.

### <a name="telemetry"></a>Telemetri

Bir konuşma etkin cihaz veya uygulama oluşturma, hatta deneyimli geliştiriciler için zor olabilir. Akış tabanlı protokolleri genellikle ilk bakışta göz korkutucu ve sessizlik algılama tamamen yeni gibi önemli ayrıntılar gibi görünüyor. Bir tek istek/yanıt çifti tamamlamak için başarıyla gönderilmesine ve alınmasına gerek çok fazla sayıda iletilerle olduğu *çok* bu iletileri hakkında doğru ve eksiksiz veri toplamak önemli. Microsoft konuşma hizmeti protokolü, bu verilerin toplanması için sağlar. Gerekli verileri mümkün olduğunca doğru bir şekilde sağlamak için her çabayı; doğru ve eksiksiz veri sağlayarak, kendiniz--yardımcı olacağını gerektiğinde Microsoft ekibinden konuşma hizmeti istemci uygulamanız ilgili gidermede yardım, toplanan telemetri verileri kalitesini sorun için kritik olacaktır Çözümleme.

> [!NOTE]
> Bu özellik, konuşma tanıma REST API kullanırken desteklenmiyor.

### <a name="speech-application-states"></a>Konuşma uygulama durumları

Konuşma giriş uygulamanızda etkinleştirmek için uygulayacağınız adımlar fare tıklamaları veya parmak dokunur gibi diğer giriş biçimlerini adımları biraz farklı değildir. Uygulamanızın mikrofonun dinleme ve veri hizmetinden yanıt beklenirken ve boş bir durumda olduğunda konuşma hizmetine gönderme izlemek gerekir. Aşağıdaki diyagramda bu durumlar arasındaki ilişki gösterilmektedir.

![Konuşma uygulama durumu diyagramı](Images/speech-application-state-diagram.png)

Microsoft konuşma hizmeti bazı durumlar katılan olduğundan, hizmet protokolü, uygulama geçiş durumlarındaki Yardım iletileri tanımlar. Uygulamanız, yorumlar ve izlemek ve konuşma uygulama durumları yönetmek için bu protokol iletilerini hareket gerekiyor.

## <a name="using-the-speech-recognition-service-from-your-apps"></a>Uygulamalarınızı konuşma tanıma hizmetinden kullanma

Microsoft konuşma tanıma hizmeti için uygulamalarını konuşma eklemek geliştiriciler için iki yöntem sağlar.

- [REST API](GetStarted/GetStartedREST.md): geliştiriciler uygulamalarını gelen HTTP çağrıları için konuşma tanıma hizmetini kullanabilirsiniz.
- [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md): Gelişmiş özellikler için geliştiriciler Microsoft Speech istemci kitaplıkları indirebilir ve bunların uygulamalarla bağlantı.  İstemci kitaplıkları, farklı diller (C#, Java, JavaScript, ObjectiveC) kullanarak çeşitli platformlarda (Windows, Android, iOS) kullanılabilir.

| Uygulama alanları | [REST API'ler](GetStarted/GetStartedREST.md) | [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Dönüştürme kısa konuşma ses (ses uzunluğu < 15 s) Ara sonuç olmadan örneğin, komutları. | Evet | Evet |
| Uzun bir ses (> 15 s) Dönüştür | Hayır | Evet |
| İstenen Ara sonuçlarla Akış ses | Hayır | Evet |
| HALUK kullanarak ses Dönüştürülen metin anlama | Hayır | Evet |

 Diliniz veya platformunuz henüz bir SDK yoksa, göre kendi uygulama oluşturabileceğiniz [Protokolü belgeleri](API-Reference-REST/websocketprotocol.md).

## <a name="recognition-modes"></a>Tanıma modları

Tanıma üç modu vardır: `interactive`, `conversation`, ve `dictation`. Tanıma modu konuşma tanıma nasıl kullanıcıları seslendir olasılığı olan bağlı olarak ayarlar. Uygulamanız için uygun tanıma modu seçin.

> [!NOTE]
> Tanıma modları sahip olabileceği farklı davranışlar [REST Protokolü](#rest-speech-recognition-api) yaparlar daha [WebSocket Protokolü](#webSocket-speech-recognition-api). Örneğin, REST API bile konuşma veya Dikte modunda sürekli tanıma desteklemez.
> [!NOTE]
> REST veya WebSocket Protokolü doğrudan kullandığınızda bu modların geçerlidir. [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md) farklı parametreler tanıma modu belirtmek için kullanın. Daha fazla bilgi için tercih ettiğiniz istemci kitaplığına bakın.

Microsoft konuşma hizmeti tüm tanıma modları için yalnızca bir tanıma tümcecik sonucunu döndürür. Tüm tek utterance 15 saniye sınırı yoktur.

### <a name="interactive-mode"></a>Etkileşimli mod

İçinde `interactive` modu, bir kullanıcının kısa istek yaptığında ve yanıt olarak bir eylemi gerçekleştirmek için uygulama bekler.

Aşağıdaki özelliklere etkileşimli mod uygulamaları tipik şunlardır:

- Başka bir insan değil de, bir makineye Konuşma dili konusunda kullanıcıları bilgilendirin.
- Uygulama kullanıcıları deyin, temel yapmak için uygulama istediklerini üzerinde istedikleri zaman öncesinde bilirsiniz.
- Utterances genellikle en son hakkında 2-3 saniye.

### <a name="conversation-mode"></a>Konuşma modu

İçinde `conversation` modu, kullanıcıların İnsan İnsan konuşmada gerçekleştiriliyor.

Aşağıdaki özelliklere konuşma modu uygulamaları tipik şunlardır:

- Başka bir kişiye varsayılır konusunda kullanıcıları bilgilendirin.
- Konuşma tanıma konuşma metin görmek bir veya iki katılımcı vererek İnsan görüşmeleri geliştirir.
- Kullanıcılar, söylemek istedikleri her zaman planlamadığınız.
- Kullanıcılar sık argo ve diğer resmi olmayan konuşma kullanın.

### <a name="dictation-mode"></a>Yazdırma modu

İçinde `dictation` modu, kullanıcıların başka bir işleme için uygulamaya uzun utterances belirtmesini.

Aşağıdaki özelliklere dikte modu uygulamaları tipik şunlardır:

- Bir makineye varsayılır konusunda kullanıcıları bilgilendirin.
- Konuşma tanıma metin sonuçlarını kullanıcılara gösterilir.
- Kullanıcılar, çoğunlukla söyleyin ve daha resmi bir dil kullanmak istedikleri planlayın.
- Kullanıcıların employ tam, son 5-8 saniye cümleler.

> [!NOTE]
> Yazdırma ve konuşma modlarında Microsoft konuşma hizmeti kısmi sonuçlar döndürmez. Bunun yerine, hizmetin ses akışı sessizlik sınırlarında sonra kararlı tümcecik sonuçlar döndürür. Microsoft bu sürekli tanıma modda kullanıcı deneyimini geliştirmek için konuşma Protokolü geliştirmek.

## <a name="recognition-languages"></a>Tanıma dilleri

*Tanıma dili* , uygulama kullanıcısı konuşur dilini belirtir. Belirtin *tanıma dili* ile *dil* bağlantıda URL sorgu parametresi. Değeri *dil* sorgu parametresi IETF dil etiket kullanır [BCP 47](https://en.wikipedia.org/wiki/IETF_language_tag), ve **gerekir** konuşma tanıma API'si tarafından desteklenen dilleri biri. Konuşma hizmeti tarafından desteklenen diller tam listesi sayfasında bulunabilir [desteklenen dilleri](API-Reference-REST/supportedlanguages.md).

Microsoft konuşma hizmeti görüntüleyerek geçersiz bağlantı istekleri reddeder bir `HTTP 400 Bad Request` yanıt. Geçersiz bir istek biridir:

- İçermemesi bir *dil* sorgusu parametre değeri.
- İçeren bir *dil* sorgu parametresi yanlış biçimlendirilmiş.
- İçeren bir *dil* sorgu parametresi destek dilleri biri değil.

Bir veya tüm hizmet tarafından desteklenen diller destekleyen bir uygulama oluşturmak tercih edebilirsiniz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte, bir uygulamanın kullandığı *konuşma* ABD İngilizce Konuşmacı için konuşma tanıma modu.

```HTTP
https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

## <a name="transcription-responses"></a>Transcription yanıtları

Transcription yanıtları Dönüştürülen metin ses istemcilere döndürür. Transcription yanıt aşağıdaki alanları içerir:

- `RecognitionStatus` tanıma durumunu belirtir. Olası değerler aşağıdaki tabloda verilmiştir.

| Durum | Açıklama |
| ------------- | ---------------- |
| Başarılı | Tanıma başarılı oldu ve görüntü metni alan yok |
| NoMatch | Konuşma ses akışını algılandı, ancak hedef dilden sözcük eşleştirildiklerinden. [NoMatch tanıma Status(#nomatch-recognition-status) daha fazla ayrıntı için bkz:  |
| InitialSilenceTimeout | Ses akışı başlangıcı yalnızca sessizlik ve konuşma için beklerken zaman aşımına hizmet bulunan |
| BabbleTimeout | Ses akışı başlangıcı yalnızca gürültü ve konuşma için beklerken zaman aşımına hizmet bulunan |
| Hata | Tanıma hizmeti bir iç hatayla karşılaştı ve çalışmaya devam edemedi |

- `DisplayText` tanınan tümceciği büyük/küçük harf ve noktalama ters metin normalleştirme uygulanan sonra uygunsuz metin yıldız işaretiyle maskelenmiş temsil eder. Görüntü metni alanı yoksa *yalnızca* varsa `RecognitionStatus` alan değerine sahip `Success`.

- `Offset` hangi tümcecik, ses akışı başlangıç göreli tanınan uzaklığı (100 nanosaniyelik birimleri) belirtir.

- `Duration`Bu konuşma deyimi (100 nanosaniyelik birimleri) süresini belirtir.

Transcription yanıt daha fazla bilgi isterseniz döndürür. Bkz: [çıktı biçimi](#output-format) nasıl daha ayrıntılı çıktı döndürmek.

Microsoft konuşma hizmeti büyük/küçük harf ve noktalama işaretleri, eklemeyi içerir ek transcription işlem uygunsuz metin maskeleme ve ortak formlar metne normalleştirme destekler. Örneğin, bir kullanıcı tarafından temsil edilen bir deyim verirse sözcükler "altı iPhone satın hatırlat", "6 iPhone satın hatırlat." Microsoft'un konuşma Hizmetleri transcribed metni döndürür Word'ün "altı" "6" sayıya işlem adlı *ters metin normalleştirme* (*ITN* kısaca).

### <a name="nomatch-recognition-status"></a>NoMatch tanıma durumu

Transcription yanıtı döndürür `NoMatch` içinde `RecognitionStatus` zaman Microsoft konuşma hizmeti konuşma ses akışta algılar, ancak bu istek için kullanılan dil dilbilgisi konuşma eşleşecek şekilde alamıyor. Örneğin, bir *NoMatch* koşul tanıyıcı ABD İngilizcesi Konuşma dili olarak beklerken kullanıcı şeyin Almanca diyorsa oluşabilir. Utterance dalga biçiminin desenini İnsan konuşma varlığını gösterir, ancak konuşulan sözcüklerin hiçbiri tanıyıcı tarafından kullanılan ABD İngilizce sözlüğü eşleşir.

Başka bir *NoMatch* durum tanıma algoritması Ses akışında bulunan ses için doğru bir eşleşme bulamadı olduğunda meydana gelir. Bu durum gerçekleştiğinde, Microsoft konuşma hizmeti üretebilir *speech.hypothesis* içeren iletileri *metin onaylanmadığına* ancak oluşturacak bir *speech.phrase*, ileti *RecognitionStatus* olan *NoMatch*. Bu durum normaldir; varsayımlar doğruluğu veya metnin doğruluk hakkında yapmamalısınız *speech.hypothesis* ileti. Microsoft konuşma hizmeti oluşturduğundan Ayrıca, çalıştığını kabul gerekir değil *speech.hypothesis* hizmet oluşturabildiği iletileri bir *speech.phrase* ile ileti  *RecognitionStatus* *başarı*.

## <a name="output-format"></a>Çıktı biçimi

Microsoft konuşma hizmeti, çeşitli yükü biçimlerde transcription yanıtları döndürebilir. Tüm yüklerini JSON yapıları ' dir.

Belirterek tümceciği sonuç biçimi denetleyebilirsiniz `format` URL sorgu parametresi. Varsayılan olarak hizmet verir `simple` sonuçları.

| Biçimlendir | Açıklama |
|-----|-----|
| `simple` | Tanıma durumunu ve görüntü formdaki tanınan metni içeren Basitleştirilmiş tümcecik sonucu. |
| `detailed` | Tanıma durumu ve her tümcecik sonucu tüm dört tanıma formları ve güven puanı, içerdiği tümcecik sonuçları N en iyi listesi. |

`detailed` Biçimi içeren [N en iyi değerleri](#n-best-values), ek olarak `RecognitionStatus`, `Offset`, ve `duration`, yanıt.

### <a name="n-best-values"></a>N en iyi değerleri

Dinleyicileri, İnsan veya makine, hiçbir zaman olabilir bunlar heard belirli *tam olarak* ne söylenir. Dinleyici atayabileceğiniz bir *olasılık* yalnızca bir utterance belirli yorumu için. 

Kim ile sık etkileşimde başkalarına konuşurken normal koşullarda, kişiler konuşulan sözcükler algılamayı bir yüksek olasılığı vardır. Makine tabanlı konuşma dinleyicileri çaba benzer doğruluğu düzeyleri elde etmek ve sağ koşullarda [insanlar eşlikli elde](https://blogs.microsoft.com/next/2016/10/18/historic-achievement-microsoft-researchers-reach-human-parity-conversational-speech-recognition/#sm.001ykosqs14zte8qyxj2k9o28oz5v).

Konuşma tanıma kullanılan algoritmaları işleme normal bir parçası olarak bir utterance, alternatif yorumlar keşfedin. Genellikle, bu alternatifleri tek yorumlama lehinde kanıt bunaltıcı hale geldiğinde atılır. En iyi durumda olmayan koşullarda, ancak diğer olası Yorumlar listesini Konuşma tanıyıcı tamamlanır. Üst *N* bu listede alternatifleri çağrılır *N en iyi listesi*. Her bir alternatif atanmış bir [güvenirlik puan](#confidence). GÜVENİRLİK 0 ile 1 arasında puanlar. Bir puan 1 en yüksek güvenirlik düzeyini temsil eder. 0 puan güvenirlik en düşük düzeyde temsil eder.

> [!NOTE]
> N en iyi listesinde girdi sayısı birden çok utterances arasında farklılık gösterir. Girdi sayısı birden çok teşekkürler arasında değişebilir *aynı* utterance. Bu değişim konuşma tanıma algoritması probabilistic yapısını doğal ve beklenen sonucunu ' dir.

N en iyi listesinde döndürülen her bir giriş içerir

- `Confidence`, temsil eden [güvenirlik puanları](#confidence) bu girişinin.
- `Lexical`, olduğu [sözcük form](#lexical-form) tanınan metin.
- `ITN`, olduğu [ITN form](#itn-form) tanınan metin.
- `MaskedITN`, olduğu [maskelenmiş ITN form](#masked-itn-form) tanınan metin.
- `Display`, olduğu [görüntüleme formu](#display-form) tanınan metin.

### GÜVENİRLİK puanları <a id="confidence"></a>

GÜVENİRLİK puanları konuşma tanıma sistemlere tam sayı. Microsoft konuşma hizmeti gelen güven puanları edinir bir *güvenirlik sınıflandırıcı*. Microsoft güvenilirlik sınıflandırıcı maximally arasında doğru ve yanlış tanıma ayırt etmek için tasarlanmış özelliklerle kümesi üzerinden eğitir. GÜVENİRLİK puanları ayrı sözcükleri ve tüm utterances için değerlendirilir.

Hizmet tarafından döndürülen güvenirlik puanları kullanmayı seçerseniz, aşağıdaki davranışını dikkat edin:

- GÜVENİRLİK puanlarını ve aynı tanıma modu ve dil içinde yalnızca karşılaştırılabilir. Puanları farklı dil ya da farklı tanıma modlar arasında karşılaştırın değil. Örneğin, bir güven puan etkileşimli tanıma modunda olan *hiçbir* güvenirlik puan Dikte modunda bağıntı.
- GÜVENİRLİK puanları utterances kısıtlı kümesine göre en iyi şekilde kullanılır. Doğal olarak çok sayıda utterances puanlarını sonuçlarındaki önemli ölçüde yoktur.

GÜVENİRLİK puan değer olarak kullanmayı seçerseniz bir *eşik* uygulamanızı görev yapan üzerinde konuşma tanıma eşik değerleri oluşturmak için kullanın.

- Konuşma tanıma utterances uygulamanız için temsili bir örnek üzerinde yürütün.
- Örnek kümesindeki her tanıma güvenirlik puanlarını toplayın.
- Bu örnek için güvenle bazı Yüzdeliğini eşik değeri temel.

Tek eşik değer tüm uygulamalar için uygundur. Bir uygulama için bir kabul edilebilir güvenirlik puan başka bir uygulama için kabul edilebilir olabilir.

### <a name="lexical-form"></a>sözcük formu

Sözcük tanınan metin tam olarak nasıl onu utterance ve noktalama işareti ya da büyük/küçük harf olmadan oluştu biçimidir. Örneğin, "1020 Kurumsal yolu" adresi sözcük biçiminde olur *on yirmi Kurumsal şekilde*, böylece konuşulan varsayılarak. Sözcük formun cümlede "anımsatmak bana 5 Kurşun Kalem satın almak için" olan *beş Kurşun Kalem satın hatırlat*.

Sözcük formun standart metin normalleştirmesi için gereken uygulamalar için uygundur. Sözcük formun de işlenmemiş tanıma sözcükler gerektiren uygulamalar için uygundur.

Uygunsuz metin sözcük biçiminde hiçbir zaman maskelenir.

### <a name="itn-form"></a>ITN formu

Metin normalleştirme metin "kurallı" başka bir forma bir biçimden diğerine dönüştürme işlemidir. Örneğin, telefon numarası "555-1212" kurallı biçimi dönüştürülebilir *beş beş beş bir iki bir iki*. *Ters* metin normalleştirme (ITN) tersine çevirir sözcükleri dönüştürme bu işlem, "beş beş beş bir iki bir iki" ters kurallı biçimi için *555-1212*. Bir tanıma sonucunun ITN form büyük/küçük harf veya noktalama işareti içermez.

ITN form tanınan metin üzerinde hareket uygulamalar için uygundur. Örneğin, bir kullanıcının arama terimleri seslendir izin verir ve ardından bu koşulları web sorguda kullanan bir uygulama ITN form kullanırsınız. Uygunsuz metin hiçbir zaman ITN biçiminde maskelenir. Uygunsuz metin maske, kullanın *maskelenmiş ITN form*.

### <a name="masked-itn-form"></a>Maskeli ITN formu

Uygunsuz metin doğal konuşulan dilinde bir parçası olduğundan, bunlar konuşulan Microsoft konuşma hizmeti gibi sözcükler ve tümcecikleri tanır. Uygunsuz metin ancak, tüm uygulamalar, özellikle bu uygulamalarla sınırlı, yetişkin olmayan kullanıcı İzleyici için uygun olmayabilir.

Maskeli ITN formun ters metin normalleştirme formun maskeleme uygunsuz metin geçerlidir. Uygunsuz metin maske, uygunsuz metin parametre değerini ayarlamak için `masked`. Uygunsuz metin maskelenir dilin uygunsuz metin sözlüğü bir parçası olarak tanınan sözcükleri yıldız işaretiyle değiştirilir. Örneğin: *5 satın hatırlat *** Kurşun Kalem*. Maskeli ITN formun tanıma sonucunun büyük/küçük harf veya noktalama işareti içermez.

> [!NOTE]
> Uygunsuz metin sorgusu parametre değeri ayarlanmışsa `raw`, maskelenmiş ITN formun ITN form ile aynıdır. Uygunsuz metin olduğu *değil* maskelenir.

### <a name="display-form"></a>Form Görüntüle

Noktalama işaretleri ve büyük/küçük harf Vurgu, duraklatma ve metin anlamak daha kolay hale getirir benzeri nerede bulunacağı yer işaret eder. Görüntü formun noktalama işaretleri ve büyük/küçük harf tanıma sonuçları, konuşma metin görüntüleme uygulamalar için en uygun form kolaylaştırarak ekler.

Görüntü formun maskelenmiş ITN formun genişlettiğinden uygunsuz metin parametre değeri ayarlayabileceğiniz `masked` veya `raw`. Değer ayarlanmışsa `raw`, kullanıcı tarafından konuşulan herhangi uygunsuz metin tanıma sonuçları dahil edin. Değer ayarlanmışsa `masked`, dilin uygunsuz metin sözlüğü bir parçası olarak kabul edilen sözcükler, yıldız işareti ile değiştirilir.

### <a name="sample-responses"></a>Örnek yanıt

Tüm yüklerini JSON yapıları ' dir.

Bir yük biçiminde `simple` tümceyi sonucu:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Bir yük biçiminde `detailed` tümceyi sonucu:

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

## <a name="profanity-handling-in-speech-recognition"></a>Konuşma tanıma uygunsuz metin işleme

Microsoft konuşma hizmeti İnsan okuma, sözcük ve birçok kişi "uygunsuz metin." sınıflandırabilir deyimleri dahil olmak üzere tüm biçimlerinin tanır Kullanarak hizmet uygunsuz metin nasıl işlediğini kontrol edebilirsiniz *uygunsuz metin* sorgu parametresi. Varsayılan olarak, hizmet maskeleri uygunsuz metin içinde *speech.phrase* sonuçlanır ve döndürmez *speech.hypothesis* uygunsuz metin içeren iletileri.

| *Uygunsuz metin* değeri | Açıklama |
| - | - |
| `masked` | Bir yıldız işareti ile maskeleri uygunsuz metin. Bu, varsayılan davranıştır. | 
| `removed` | Uygunsuz metin tüm sonuçlarından kaldırır. |
| `raw` | Tanır ve tüm sonuçlarında uygunsuz metin döndürür. |

### <a name="profanity-value-masked"></a>Uygunsuz metin değeri `Masked`

Uygunsuz metin maske, ayarlamak için *uygunsuz metin* sorgu parametresi değeri *maskelenmiş*. Zaman *uygunsuz metin* sorgu parametresi bu değere sahip veya bir istek için hizmet belirtilmemiş *maskeleri* uygunsuz metin. Hizmet tanıma sonuçlarında uygunsuz metin yıldız işaretiyle değiştirerek maskeleme gerçekleştirir. Uygunsuz metin maskeleme işleme belirttiğinizde, hizmet döndürmez *speech.hypothesis* uygunsuz metin içeren iletileri.

### <a name="profanity-value-removed"></a>Uygunsuz metin değeri `Removed`

Zaman *uygunsuz metin* sorgusu parametre değeri içeriyor *kaldırılan*, hizmet uygunsuz metin hem de kaldırır *speech.phrase* ve *speech.hypothesis* iletileri. Sonuçları aynıdır *uygunsuz metin sözcük değil konuşulan gibi*.

#### <a name="profanity-only-utterances"></a>Yalnızca uygunsuz metin utterances

Bir kullanıcı Seslendir *yalnızca* uygulama uygunsuz metin kaldırılacak hizmetin yapılandırıldığında uygunsuz metin. Tanıma modu ise, bu senaryo için *dikte* veya *konuşma*, hizmet döndürmez bir *speech.result*. Tanıma modu ise *etkileşimli*, hizmet iadeleri bir *speech.result* durum koduyla *NoMatch*. 

### <a name="profanity-value-raw"></a>Uygunsuz metin değeri `Raw`

Zaman *uygunsuz metin* sorgusu parametre değeri içeriyor *ham*, hizmeti kaldırma veya maske her ikisinde uygunsuz metin *speech.phrase* veya  *Speech.hypothesis* iletileri.
