---
title: Microsoft Çeviricisi metin API V2.0 başvurusu | Microsoft Docs
titleSuffix: Cognitive Services
description: V2.0 Microsoft Çeviricisi metin API başvuru belgeleri.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/15/2018
ms.author: v-jansko
ms.openlocfilehash: e32e28608d2fecf27b61acff74af7eb6849f0ba1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355150"
---
# <a name="translator-text-api-v20"></a>Çevirici metin API v2.0

> [!IMPORTANT]
> Bu sürümü Çeviricisi metin API, kullanım dışı bırakıldı. [Görüntüleme Çeviricisi metin API v3 belgelerine](v3-0-reference.md).

Microsoft Çeviricisi V2 metin API uygulamalarınızı, Web siteleri, araçları veya diğer çözümleri çok dilli kullanıcı deneyimi sağlamak için sorunsuz bir şekilde tümleştirilebilir. Endüstri standartları yararlanarak, bunu herhangi bir donanım platformda ve herhangi bir işletim sistemiyle dil çeviri ve metin dili algılama veya metin okuma gibi dil ilgili diğer işlemleri gerçekleştirmek için kullanılabilir. Microsoft Çeviricisi API'si hakkında daha fazla bilgi için burayı tıklatın.

## <a name="getting-started"></a>Başlarken
Microsoft Çeviricisi metin gerekir API erişmek için [için Microsoft Azure'a kaydolun](../translator-text-how-to-signup.md).

## <a name="authorization"></a>Yetkilendirme
Tüm Microsoft Çeviricisi metin API çağrıları kimliğini doğrulamak için bir abonelik anahtarı gerektirir. API kimlik doğrulamasının iki modlarını destekler:

* Bir erişim belirteci kullanarak. Başvurulan abonelik anahtarı kullanmak **adım** yetkilendirme hizmete bir POST isteği yaparak bir erişim belirteci üretmek için 9. Ayrıntılar için belirteç hizmeti belgelerine bakın. Erişim belirteci Authorization üstbilgisi veya access_token sorgu parametresini kullanarak Çeviricisi service geçirin. Erişim belirteci 10 dakika için geçerlidir. Her 10 dakikada yeni bir erişim belirteci edinmek ve aynı erişimi kullanarak bu 10 dakika içinde yinelenen istekleri için belirteç tutun.

* Abonelik anahtarı kullanarak doğrudan. Bir değer olarak, abonelik anahtarınızı geçirmek `Ocp-Apim-Subscription-Key` isteğiniz Çeviricisi API ile birlikte gelen üstbilgi. Bu modda, bir erişim belirteci üretmek için kimlik doğrulama belirteci hizmeti çağrısı gerekmez.

Abonelik anahtarınızı ve erişim belirteci görünümünden gizli gizli olarak göz önünde bulundurun.

## <a name="profanity-handling"></a>Uygunsuz metin işleme
Normalde Çeviricisi hizmet mevcut çeviri kaynağında uygunsuz metin korur. Uygunsuz metin derecesini ve sözcükleri saygısız içerikli yapar bağlam kültürler arasında farklı ve sonuç olarak hedef dilde uygunsuz metin derecesini yükseltilmiş azaltılmış veya olabilir.

Kaynak metin uygunsuz metin varlığını bakılmaksızın çevirisini uygunsuz metin alma önlemek istiyorsanız seçeneği destekleyen yöntemler için filtreleme uygunsuz metin kullanabilirsiniz. Seçeneği, silinmiş veya uygun etiketleriyle işaretlenmiş uygunsuz metin görmeyi veya eylem seçmenizi sağlar. Kabul edilen değerlerini `ProfanityAction` olan `NoAction` (varsayılan), işaretlenmiş ve `Deleted`.


|ProfanityAction    |Eylem |Örnek kaynak (Japonca)  |Örnek çeviri (İngilizce)  |
|:--|:--|:--|:--|
|NoAction   |Varsayılan. Aynı seçenek ayarı bulunamadı. Uygunsuz metin kaynaktan hedefe geçer.        |彼はジャッカスです。     |Kendisine bir jackass olur.   |
|İşaretli     |Saygısız içerikli sözcükler tarafından XML etiketleri arasına <profanity> ve </profanity>.     |彼はジャッカスです。 |Gerçekleştirilmesine bir <profanity>jackass</profanity>.    |
|Silinen    |Saygısız içerikli sözcükleri değiştirme yapmadan çıktısından kaldırılır.     |彼はジャッカスです。 |Gerçekleştirilmesine bir.   |

    
## <a name="excluding-content-from-translation"></a>İçerik çevrilmesi hariç
İçerik HTML gibi etiketlerle çevirirken (`contentType=text/html`), belirli içerik çevrilmesi hariç tutmak bazen kullanışlıdır. Öznitelik kullanabilir `class=notranslate` özgün dilinde kalacağı içeriği belirtmek için. Aşağıdaki örnekte, ilk içeriğini `div` öğesi değil çevrilir, ikinci içerik sırasında `div` öğesi çevrilir.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>GET / Çevir

### <a name="implementation-notes"></a>Uygulama Notları
Bir metin dizesi bir dilden diğerine çevirir.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Dönüş değeri:** çevrilmiş metni temsil eden dize.

Daha önce kullandıysanız `AddTranslation` veya `AddTranslationArray` 5 veya aynı kaynak cümleyi daha yüksek bir derecelendirme çevirisi girmek için `Translate` sisteminiz için kullanılabilir olan üst seçimi döndürür. "Aynı kaynak cümle" tam aynı (% 100 eşleşen), büyük/küçük harf, boşluk, etiket değerleri ve bir tümcenin sonunda noktalama dışında anlamına gelir. 5 veya üzeri bir derecelendirme derecelendirmesi depolanıyorsa döndürülen sonuç Microsoft Translator tarafından otomatik çeviri olacaktır.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

dize

Yanıt içerik türü: uygulama/xml 

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama    |Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID  |(boş)    |Gereklidir. Yetkilendirme veya Apim abonelik anahtar Ocp üstbilgisi kullanılırsa, AppID alanı boş bırakın başka dahil "Bearer" içeren bir dize + "" + "access_token".|sorgu|dize|
|metin|(boş)   |Gereklidir. Çevirmek için metin temsil eden dize. Metin boyutu 10000 karakteri aşmamalıdır.|sorgu|dize|
|başlangıç|(boş)   |İsteğe bağlı. Çeviri metnin dil kodunu temsil eden dize. Örneğin, İngilizce için en.|sorgu|dize|
|-|(boş) |Gereklidir. Metne çevirmek için dil kodu temsil eden dize.|sorgu|dize|
|contentType|(boş)    |İsteğe bağlı. Çevrildiğini metin biçimi. Desteklenen biçimler şunlardır: metin/düz (varsayılan) ve metin/html. Herhangi bir HTML doğru biçimlendirilmiş ve eksiksiz öğesi olması gerekir.|sorgu|dize|
|category|(boş)   |İsteğe bağlı. Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak "Genel".|sorgu|dize|
|Yetkilendirme|(boş)  |AppID alan veya Apim abonelik anahtar Ocp üstbilgisi belirtilmemişse gereklidir. Yetkilendirme belirtecini: "Bearer" + "" + "access_token".|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)  |AppID alan veya yetkilendirme üst bilgisi belirtilmemişse gereklidir.|üst bilgi|dize|


### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisi X-MS-transfer-Info dahil istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-translatearray"></a>/TranslateArray sonrası

### <a name="implementation-notes"></a>Uygulama Notları
Kullanım `TranslateArray` çevirileri birden fazla kaynak metni için alma yöntemi.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

İstek gövdesini biçimi şu şekilde olmalıdır:

```
<TranslateArrayRequest>
  <AppId />
  <From>language-code</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" />
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</TranslateArrayRequest>
```

İçinde öğelerin `TranslateArrayRequest` şunlardır:


* `appid`: Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.
* `from`: İsteğe bağlı. Metinden çevirmek için dil kodu temsil eden dize. Boş bırakılırsa yanıt dil otomatik algılamayı sonucunu içerir.
* `options`: İsteğe bağlı. Bir `Options` aşağıda listelenen değerleri içeren nesne. Bunlar tüm isteğe bağlıdır ve varsayılan en yaygın ayarlar. Belirtilen öğeleri alfabetik olarak listelenmiş olması gerekir.
    - `Category`: Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak `general`.
    - `ContentType`: Çevrildiğini metin biçimi. Desteklenen biçimler: `text/plain` (varsayılan), `text/xml` ve `text/html`. Herhangi bir HTML doğru biçimlendirilmiş ve eksiksiz öğesi olması gerekir.
    - `ProfanityAction`: Profanities yukarıda açıklandığı şekilde nasıl işlendiğini belirler. Kabul edilen değerler, `ProfanityAction` olan `NoAction` (varsayılan), `Marked` ve `Deleted`.
    - `State`: Correlate istek ve yanıt yardımcı olmak için kullanıcı durumu. Aynı içeriği yanıt olarak döndürülür.
    - `Uri`: Bu URI tarafından sonuçları filtrelenir. Varsayılan: `all`.
    - `User`: Bu kullanıcı tarafından sonuçları filtrelenir. Varsayılan: `all`.
* `texts`: Gerekli. Çeviri metinleri içeren bir dizi. Tüm dizeleri aynı dilde olmalıdır. Çevrilecek tüm metinleri toplamı 10000 karakteri aşmamalıdır. Dizi öğelerinin üst limiti 2000'dir.
* `to`: Gerekli. Metne çevirmek için dil kodu temsil eden dize.

İsteğe bağlı öğeleri atlanabilir. TranslateArrayRequest doğrudan alt öğelerinin alfabetik olarak listelenmiş olması gerekir.

TranslateArray yöntemi kabul `application/xml` veya `text/xml` için `Content-Type`.

**Dönüş değeri:** A `TranslateArrayResponse` dizi. Her `TranslateArrayResponse` aşağıdaki öğelere sahiptir:

* `Error`: Biri oluştuğunda bir hata gösterir. Aksi takdirde null olarak ayarlanır.
* `OriginalSentenceLengths`: Her cümlede özgün kaynak metnin uzunluğunu gösteren tamsayı dizisi. Dizi uzunluğu cümleleri sayısını gösterir.
* `TranslatedText`: Çevrilen metin.
* `TranslatedSentenceLengths`: Her cümlede çevrilmiş metnin uzunluğunu gösteren tamsayı dizisi. Dizi uzunluğu cümleleri sayısını gösterir.
* `State`: Correlate istek ve yanıt yardımcı olmak için kullanıcı durumu. İstek içeriği döndürür.

Yanıt gövdesi biçimi aşağıdaki gibidir.

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <TranslateArrayResponse>
    <From>language-code</From>
    <OriginalTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </OriginalTextSentenceLengths>
    <State/>
    <TranslatedText>string-value</TranslatedText>
    <TranslatedTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </TranslatedTextSentenceLengths>
  </TranslateArrayResponse>
</ArrayOfTranslateArrayResponse>
```

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Başarılı yanıt dizisi içerir `TranslateArrayResponse` yukarıda açıklanan biçimde.

dize

Yanıt içerik türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme|(boş)) |AppID alan veya Apim abonelik anahtar Ocp üstbilgisi belirtilmemişse gereklidir. Yetkilendirme belirtecini: "Bearer" + "" + "access_token".|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)|AppID alan veya yetkilendirme üst bilgisi belirtilmemişse gereklidir.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu   |Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin. Sık karşılaşılan hatalar şunlardır: <ul><li>Dizi öğesi boş olamaz</li><li>Geçersiz kategori</li><li>Dilden geçersiz</li><li>Dili için geçersiz</li><li>İstek, çok sayıda öğe içeriyor</li><li>Başlangıç dili desteklenmiyor</li><li>Kime dili desteklenmiyor</li><li>Çevir isteğine çok fazla veri yok</li><li>HTML doğru biçimde değil.</li><li>Çok fazla dizeleri Çevir isteği geçirildi</li></ul>|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisi X-MS-transfer-Info dahil istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Uygulama Notları
Diller için kolay adlar alır geçirilen parametre olarak `languageCodes`ve geçirilen yerel dilini kullanarak yerelleştirilmiş.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

İstek gövdesi için kolay adlar almak için ISO 639-1 dil kodlarını temsil eden bir dize dizisi içerir. Örneğin:

```
<ArrayOfstring xmlns:i="http://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Dönüş değeri:** istenen diline yerelleştirilmiş Çeviricisi hizmeti tarafından desteklenen diller adlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
İstenen diline yerelleştirilmiş Çeviricisi hizmeti tarafından desteklenen diller adlarını içeren bir dize dizisi.

dize

Yanıt içerik türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|dize|
|Yerel ayar|(boş) |Gereklidir. Tek başına bir dil ve dil adlarını yerelleştirme için bir ISO 3166 iki harfli büyük alt kod ilişkili bir ISO 639 iki harfli küçük kültür kodu veya bir ISO 639 küçük kültür kodu bileşimini temsil eden bir dize.|sorgu|dize|
|Yetkilendirme|(boş)  |Gerekli olursa AppID alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)  |Gerekli olursa AppID alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisi X-MS-transfer-Info dahil istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-getlanguagesfortranslate"></a>/GetLanguagesForTranslate Al

### <a name="implementation-notes"></a>Uygulama Notları
Çeviri hizmeti tarafından desteklenen dilleri gösteren dil kodlarının listesini alın.  `Translate` ve `TranslateArray` herhangi iki bu diller arasında çevirebilir.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Dönüş değeri:** Çeviricisi Hizmetleri tarafından desteklenen dil kodlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Çevirici Hizmetleri tarafından desteklenen dil kodlarını içeren bir dize dizisi.

dize

Yanıt içerik türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|dize|
|Yetkilendirme|(boş)  |Gerekli olursa `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)|Gerekli olursa `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisi X-MS-transfer-Info dahil istek kimliği ile birlikte belirtin.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-getlanguagesforspeak"></a>/GetLanguagesForSpeak Al

### <a name="implementation-notes"></a>Uygulama Notları
Konuşma sentezi için kullanılabilen dilleri alır.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Dönüş değeri:** konuşma Birleştirici Çeviricisi hizmeti tarafından desteklenen dil kodlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Konuşma Birleştirici Çeviricisi hizmeti tarafından desteklenen dil kodlarını içeren bir dize dizisi.

dize

Yanıt içerik türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|dize|
|Yetkilendirme|(boş)|Gerekli olursa `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)|Gerekli olursa `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|
 
### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401|Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgi dahil istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-speak"></a>GET / seslendir

### <a name="implementation-notes"></a>Uygulama Notları
İstenen dilde konuşulan geçilen metin wave veya mp3 akışı döndürür.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Dönüş değeri:** wave veya mp3 akışı istenen dilde konuşulan geçen metin.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

İkili

Yanıt içerik türü: uygulama/xml 

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|dize|
|metin|(boş)   |Gereklidir. Bir cümle veya cümleleri için wave akış söylenir belirtilen dil içeren bir dize. Konuşma metin boyutunu 2000 karakteri aşmamalıdır.|sorgu|dize|
|Dil|(boş)   |Gereklidir. Metin ile iletişim kurması için desteklenen dil kodunu temsil eden bir dize. Kod yönteminden döndürülen kodlarının listesi bulunmalıdır `GetLanguagesForSpeak`.|sorgu|dize|
|Biçimi|(boş)|İsteğe bağlı. İçerik türü kimliğini belirten bir dize Şu anda `audio/wav` ve `audio/mp3` kullanılabilir. Varsayılan değer `audio/wav`.|sorgu|dize|
|seçenekler|(boş)    |<ul><li>İsteğe bağlı. Birleştirilen Konuşma özelliklerini belirten bir dize:<li>`MaxQuality` ve `MinSize` ses sinyalleri kalitesini belirtmek kullanılabilir. İle `MaxQuality`, sesler yüksek kaliteli ve ile alabilirsiniz `MinSize`, en küçük boyuta sahip sesi elde edebilirsiniz. Varsayılan değer `MinSize`.</li><li>`female` ve `male` sesinin istenen cinsiyetiniz belirtmek kullanılabilir. `female` varsayılan değerdir. Dikey çubuk kullanmak '|` to include multiple options. For example  `MaxQuality|Erkek '.</li></li></ul> |sorgu|dize|
|Yetkilendirme|(boş)|Gerekli olursa `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)  |Gerekli olursa `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgi dahil istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-detect"></a>GET / Algıla

### <a name="implementation-notes"></a>Uygulama Notları
Kullanım `Detect` seçilen bir metin dili belirlemek üzere yöntem.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Dönüş değeri:** verilen metni için bir iki karakter dil kodu içeren bir dize. .

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

dize

Yanıt içerik türü: uygulama/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID|(boş)  |Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|dize|
|metin|(boş)|Gereklidir. Tanıtılması dilinden olduğu bazı metin içeren bir dize. Metin boyutu 10000 karakteri aşmamalıdır.|sorgu| dize|
|Yetkilendirme|(boş)|Gerekli olursa `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp Apim abonelik anahtarı  |(boş)    |Gerekli olursa `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgi dahil istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|


## <a name="post-detectarray"></a>/DetectArray sonrası

### <a name="implementation-notes"></a>Uygulama Notları
Kullanım `DetectArray` dize dizisi dilinin aynı anda belirlemek üzere yöntem. Her bireysel dizi öğesi bağımsız algılama gerçekleştirir ve dizinin her satır için bir sonuç döndürür.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

İstek gövdesini biçimi şu şekilde olmalıdır.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

Metin boyutu 10000 karakteri aşmamalıdır.

**Dönüş değeri:** iki karakter uzunluğunda bir dil içeren bir dize dizisi kodları Giriş dizisinin her satır için.

Yanıt gövdesi biçimi aşağıdaki gibidir.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
DetectArray başarılı oldu. Giriş dizisinin her satır için iki karakter dil kodlarını içeren bir dize dizisi döndürür.

dize

Yanıt içerik türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|dize|
|Yetkilendirme|(boş)|Gerekli olursa `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)|Gerekli olursa `appid` alan veya yetkilendirme üst bilgisi belirtilmemiş.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisi X-MS-transfer-Info dahil istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-addtranslation"></a>/AddTranslation Al

### <a name="implementation-notes"></a>Uygulama Notları

> [!IMPORTANT]
> **Kullanımdan kaldırma Not:** 31 Ocak 2018 sonra bu yöntem yeni cümleyi göndermeleri kabul etmez ve bir hata iletisi alırsınız. Lütfen bu duyuru işbirliği çeviri işlevleri değişiklikler hakkında bakın.

Çeviri çeviri belleği ekler.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

dize

Yanıt içerik türü: uygulama: xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü   |
|:--|:--|:--|:--|:--|
|AppID|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|dize|
|OriginalText|(boş)|Gereklidir. Gelen çevirmek için metin içeren bir dize. En çok 1000 karakter dizesi içeriyor.|sorgu|dize|
|TranslatedText|(boş) |Gereklidir. Bir dizeyi içeren hedef dilde çevrilir. Dize en fazla 2000 karakter var.|sorgu|dize|
|başlangıç|(boş)   |Gereklidir. Çeviri metnin dil kodunu temsil eden dize. tr = İngilizce, de Almanca vb. =...|sorgu|dize|
|-|(boş)|Gereklidir. Metne çevirmek için dil kodu temsil eden dize.|sorgu|dize|
|rating|(boş) |İsteğe bağlı. Bu dize Kalite Derecelendirme temsil eden bir tamsayı. -10 ile 10 arasında değer. Varsayılan olarak 1.|sorgu|integer|
|contentType|(boş)    |İsteğe bağlı. Çevrildiğini metin biçimi. Desteklenen biçimler şunlardır: "metin/düz" ve "text/html". Herhangi bir HTML doğru biçimlendirilmiş ve eksiksiz öğesi olması gerekir.   |sorgu|dize|
|category|(boş)|İsteğe bağlı. Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak "Genel".|sorgu|dize|
|kullanıcı|(boş)|Gereklidir. Gönderim başlatanın izlemek için kullanılan bir dize.|sorgu|dize|
|uri|(boş)|İsteğe bağlı. Bu çeviri içerik konumunu içeren bir dize.|sorgu|dize|
|Yetkilendirme|(boş)|Gerekli olursa AppID alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.    |üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)|Gerekli olursa `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|410|AddTranslation artık desteklenmiyor.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisi X-MS-transfer-Info dahil istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-addtranslationarray"></a>/AddTranslationArray sonrası

### <a name="implementation-notes"></a>Uygulama Notları

> [!IMPORTANT]
> **Kullanımdan kaldırma Not:** 31 Ocak 2018 sonra bu yöntem yeni cümleyi göndermeleri kabul etmez ve bir hata iletisi alırsınız. Lütfen bu duyuru işbirliği çeviri işlevleri değişiklikler hakkında bakın.

Çeviri bellek eklemek için çevirileri bir dizi ekler. Bu bir dizi sürümüdür `AddTranslation`.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

İstek gövdesini biçimi aşağıdaki gibidir.

```
<AddtranslationsRequest>
  <AppId></AppId>
  <From>A string containing the language code of the source language</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
  </Options>
  <To>A string containing the language code of the target language</To>
  <Translations>
    <Translation xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
      <OriginalText>string-value</OriginalText>
      <Rating>int-value</Rating>
      <Sequence>int-value</Sequence>
      <TranslatedText>string-value</TranslatedText>
    </Translation>
  </Translations>
</AddtranslationsRequest>
```

AddtranslationsRequest öğesi içindeki öğeler şunlardır:

* `AppId`: Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.
* `From`: Gerekli. Kaynak dili dil kodu içeren bir dize. Tarafından döndürülen dillerden olmalıdır `GetLanguagesForTranslate` yöntemi.
* `To`: Gerekli. Hedef Dil dil kodu içeren bir dize. Tarafından döndürülen dillerden olmalıdır `GetLanguagesForTranslate` yöntemi.
* `Translations`: Gerekli. Çeviri bellek eklemek için çevirileri dizisi. Her çeviri içermelidir: originalText, translatedText ve derecelendirme. Her bir originalText ve translatedText boyutu 1000 karakter sınırlıdır. Toplam originalText(s) ve translatedText(s) 10000 karakteri aşmamalıdır. Dizi öğeleri sayısının 100'dür.
* `Options`: Gerekli. Kategori, ContentType, URI ve kullanıcı çeşitli seçenekler kümesi. Kullanıcı gereklidir. Kategori, ContentType ve URI isteğe bağlıdır. Belirtilen öğeleri alfabetik olarak listelenmiş olması gerekir.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
AddTranslationArray yöntemi başarılı oldu. 31 Ocak 2018 sonra cümleyi göndermeleri kabul edilmedi. Hizmet 410 hata koduyla yanıt verir.

dize

Yanıt içerik türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme|(boş)|AppID alan veya Apim abonelik anahtar Ocp üstbilgisi belirtilmemişse gereklidir. Yetkilendirme belirtecini: "Bearer" + "" + "access_token".|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)|AppID alan veya yetkilendirme üst bilgisi belirtilmemişse gereklidir.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|410    |AddTranslation artık desteklenmiyor.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgi dahil istek kimliği sağlamak `X-MS-Trans-Info`.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-breaksentences"></a>/BreakSentences Al

### <a name="implementation-notes"></a>Uygulama Notları
Bir metin parçası cümleleri keser ve her tümcedeki uzunlukları içeren bir dizi döndürür.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Dönüş değeri:** tümcelerin uzunlukları temsil eden dizisi. Dizi uzunluğu cümleleri sayısı ve her tümcenin uzunluğu değerlerdir.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Cümleleri uzunlukları temsil eden dizisi. Dizi uzunluğu cümleleri sayısı ve her tümcenin uzunluğu değerlerdir.

integer

Yanıt içerik türü: uygulama/xml 

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID|(boş)  |Gereklidir. Yetkilendirme veya Apim abonelik anahtar Ocp üstbilgisi kullanılırsa, AppID alanı boş bırakın başka dahil "Bearer" içeren bir dize + "" + "access_token".|sorgu| dize|
|metin|(boş)   |Gereklidir. Cümleleri bölmek için metni temsil eden dize. Metin boyutu 10000 karakteri aşmamalıdır.|sorgu|dize|
|Dil   |(boş)    |Gereklidir. Giriş metni dil kodu temsil eden dize.|sorgu|dize|
|Yetkilendirme|(boş)|AppID alan veya Apim abonelik anahtar Ocp üstbilgisi belirtilmemişse gereklidir. Yetkilendirme belirtecini: "Bearer" + "" + "access_token".    |üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)|AppID alan veya yetkilendirme üst bilgisi belirtilmemişse gereklidir.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401|Geçersiz kimlik bilgileri|
|500|Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisi X-MS-transfer-Info dahil istek kimliği ile birlikte belirtin.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Uygulama Notları
Belirtilen dil çifti için çevirileri bir dizi depolama alanını ve MT altyapısı alır. Tüm kullanılabilir çevirileri döndürür gibi GetTranslations Çevir farklıdır.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

İstek gövdesini aşağıdaki biçime sahiptir isteğe bağlı TranslationOptions nesnesini içerir.

```
<TranslateOptions xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
  <Category>string-value</Category>
  <ContentType>text/plain</ContentType>
  <ReservedFlags></ReservedFlags>
  <State>int-value</State>
  <Uri>string-value</Uri>
  <User>string-value</User>
</TranslateOptions>
```

`TranslateOptions` Nesne aşağıda listelenen değerleri içerir. Bunlar tüm isteğe bağlıdır ve varsayılan en yaygın ayarlar. Belirtilen öğeleri alfabetik olarak listelenmiş olması gerekir.

* `Category`: Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak "Genel".
* `ContentType`: Yalnızca desteklenen ve "metin/düz" varsayılan seçenektir.
* `IncludeMultipleMTAlternatives`: birden fazla alternatifleri MT altyapısı döndürülen olup olmadığını belirlemek için mantıksal bayrak. Geçerli değerler true ve false (büyük küçük harfe duyarlı). Varsayılan, false ve yalnızca 1 alternatif içerir. Bayrağı true olarak ayarlamak işbirliği çevirileri framework (CTF) ile tamamen tümleşik çevirisini yapay alternatifleri oluşturmak için izin verir. Kod çözücü n en iyi listesinden yapay alternatifleri ekleyerek CTF içinde hiçbir alternatifleri sahip cümleleri alternatifleri döndürmek için bu özellik sağlar.
    - Derecelendirmeleri derecelendirmelerini aşağıdaki gibi uygulanır: 1) en iyi otomatik çeviri 5 derecesi içeriyor. 2) CTF'den diğer yollar için -10 + 10'dan İnceleme yetkilisi yansıtır. 3) otomatik olarak oluşturulan (n-en iyi) çeviri alternatifleri 0 derecesi yoksa ve 100 eşleşme derecesi.
    - Döndürülen alternatifleri sayısı kadar maxTranslations olmakla birlikte, daha az olabilir alternatifleri sayısı.
    - Dil çiftleri bu işlevselliği çevirileri Basitleştirilmiş ve Geleneksel Çince, her iki yönde arasında kullanılamaz. Diğer tüm Microsoft Translator desteklenen dil çiftleri için kullanılabilir.
* `State`: Correlate istek ve yanıt yardımcı olmak için kullanıcı durumu. Aynı içeriği yanıt olarak döndürülür.
* `Uri`: Bu URI tarafından sonuçları filtrelenir. Herhangi bir değer ayarlarsanız, tüm varsayılandır.
* `User`: Bu kullanıcı tarafından sonuçları filtrelenir. Herhangi bir değer ayarlarsanız, tüm varsayılandır.

İstek `Content-Type` olmalıdır `text/xml`.

**Dönüş değeri:** yanıt biçimi aşağıdaki gibidir.

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <From>Two character language code</From>
  <State/>
  <Translations>
    <TranslationMatch>
      <Count>int-value</Count>
      <MatchDegree>int-value</MatchDegree>
      <MatchedOriginalText/>
      <Rating>int value</Rating>
      <TranslatedText>string-value</TranslatedText>
    </TranslationMatch>
  </Translations>
</GetTranslationsResponse>
```

Bu içeren bir `GetTranslationsResponse` aşağıdaki değerleri içeren öğe:

* `Translations`: Bir dizi bulunan, depolanan TranslationMatch (aşağıya bakın) nesnelerini eşleştirir. Çeviriler (belirsiz eşleme) özgün metni küçük çeşitlemelerini içerebilir. Çeviriler sıralanır: % 100 eşleşen ilk olarak, aşağıdaki benzer eşleşir.
* `From`: Yöntemi başlangıç dili belirtmediyseniz, bu otomatik dil algılama sonucu olacaktır. Aksi durumda olur dilden verilir.
* `State`: Correlate istek ve yanıt yardımcı olmak için kullanıcı durumu. TranslateOptions parametresinde belirtilen değeri içerir.

TranslationMatch nesne aşağıdakilerden oluşur:

* `Error`: Belirli bir giriş dizesi için bir hata oluştu, hata kodu depolanır. Aksi takdirde alan boştur.
* `MatchDegree`: Sistem filtresinin eşleşmeleri dahil olmak üzere depolamaya karşılık giriş cümleleri eşleşir.  Giriş metni deposunda bulunan özgün metin ne kadar yakından eşleşen MatchDegree gösterir. Aralık 0 ile 100'e 0 olduğu hiçbir benzerlik döndürülen değer ve 100 tam büyük küçük harf duyarlı eşleşmedir.
MatchedOriginalText: Bu sonucu için eşleşen özgün metin. Yalnızca eşleşen özgün metin giriş metin farklı olup olmadığını döndürdü. Benzer eşleştirme kaynak metnin döndürmek için kullanılır. Microsoft Translator sonuç döndürülmedi.
* `Rating`: Kalite kararı kişinin yetkilisi gösterir. Makine çevirisi 5 derecesi sonuçlara yol açabilir. Yetkili olarak sağlanan çevirileri 6-10 derecesi genellikle sahip adımında anonim olarak sağlanan çevirileri genellikle 1-4 derecesi sahip olur.
* `Count`: Bu derecelendirme bu çeviri seçilme sayısı. Değer 0 otomatik olarak çevrilmiş yanıt için olacaktır.
* `TranslatedText`: Çevrilen metin.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
A `GetTranslationsResponse` yukarıda açıklanan biçimde nesnesi.

dize

Yanıt içerik türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|AppID|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgisi kullanıldığında, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|dize|
|metin|(boş)|Gereklidir. Çevirmek için metin temsil eden dize. Metin boyutu 10000 karakteri aşmamalıdır.|sorgu|dize|
|başlangıç|(boş)|Gereklidir. Çeviri metnin dil kodunu temsil eden dize.|sorgu|dize|
|- |(boş)    |Gereklidir. Metne çevirmek için dil kodu temsil eden dize.|sorgu|dize|
|maxTranslations|(boş)|Gereklidir. Döndürülecek çevirileri maksimum sayısını temsil eden bir tamsayı.|sorgu|integer|
|Yetkilendirme| (boş)|Gerekli olursa `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.|dize| üst bilgi|
|Ocp Apim abonelik anahtarı|(boş)  |Gerekli olursa `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgi dahil istek kimliği sağlamak `X-MS-Trans-Info`.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-gettranslationsarray"></a>/GetTranslationsArray sonrası

### <a name="implementation-notes"></a>Uygulama Notları
Kullanım `GetTranslationsArray` birden fazla kaynak metni için birden fazla çeviri aday alma yöntemi.

URI isteği `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

İstek gövdesini biçimi aşağıdaki gibidir.

```
<GetTranslationsArrayRequest>
  <AppId></AppId>
  <From>language-code</From>
  <MaxTranslations>int-value</MaxTranslations>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"/>
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</GetTranslationsArrayRequest>
```

`GetTranslationsArrayRequest` Aşağıdaki öğeleri içerir:

* `AppId`: Gerekli. AppID alanı boş bırakın, başka Authorization Üstbilgisi kullandıysanız dahil içeren bir dize `"Bearer" + " " + "access_token"`.
* `From`: Gerekli. Çeviri metnin dil kodunu temsil eden dize.
* `MaxTranslations`: Gerekli. Döndürülecek çevirileri maksimum sayısını temsil eden bir tamsayı.
* `Options`: İsteğe bağlı. Aşağıda listelenen değerleri içeren bir seçenekleri nesne. Bunlar tüm isteğe bağlıdır ve varsayılan en yaygın ayarlar. Belirtilen öğeleri alfabetik olarak listelenmiş olması gerekir.
    - Kategori ': çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak genel.
    - `ContentType`: Yalnızca desteklenen ve varsayılan metin/düz bir seçenektir.
    - `IncludeMultipleMTAlternatives`: birden fazla alternatifleri MT altyapısı döndürülen olup olmadığını belirlemek için mantıksal bayrak. Geçerli değerler true ve false (büyük küçük harfe duyarlı). Varsayılan, false ve yalnızca 1 alternatif içerir. Bayrağı true olarak ayarlamak işbirliği çevirileri framework (CTF) ile tamamen tümleşik çevirisini yapay alternatifleri oluşturmak için izin verir. Kod çözücü n en iyi listesinden yapay alternatifleri ekleyerek CTF içinde hiçbir alternatifleri sahip cümleleri alternatifleri döndürmek için bu özellik sağlar.
        - Derecelendirmeleri derecelendirmelerini aşağıdaki gibi uygulanır: 1) en iyi otomatik çeviri 5 derecesi içeriyor. 2) CTF'den diğer yollar için -10 + 10'dan İnceleme yetkilisi yansıtır. 3) otomatik olarak oluşturulan (n-en iyi) çeviri alternatifleri 0 derecesi yoksa ve 100 eşleşme derecesi.
        - Döndürülen alternatifleri sayısı kadar maxTranslations olmakla birlikte, daha az olabilir alternatifleri sayısı.
        - Dil çiftleri bu işlevselliği çevirileri Basitleştirilmiş ve Geleneksel Çince, her iki yönde arasında kullanılamaz. Diğer tüm Microsoft Translator desteklenen dil çiftleri için kullanılabilir.
* `State`: Correlate istek ve yanıt yardımcı olmak için kullanıcı durumu. Aynı içeriği yanıt olarak döndürülür.
* `Uri`: Bu URI tarafından sonuçları filtrelenir. Herhangi bir değer ayarlarsanız, tüm varsayılandır.
* `User`: Bu kullanıcı tarafından sonuçları filtrelenir. Herhangi bir değer ayarlarsanız, tüm varsayılandır.
* `Texts`: Gerekli. Çeviri metinleri içeren bir dizi. Tüm dizeleri aynı dilde olmalıdır. Çevrilecek tüm metinleri toplamı 10000 karakteri aşmamalıdır. Dizi öğeleri sayısı 10'dur.
* `To`: Gerekli. Metne çevirmek için dil kodu temsil eden dize.

İsteğe bağlı öğeleri atlanabilir. Olan öğeleri doğrudan alt `GetTranslationsArrayRequest` alfabetik olarak listelenmiş olması gerekir.

İstek `Content-Type` olmalıdır `text/xml`.

**Dönüş değeri:** yanıt biçimi aşağıdaki gibidir.

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <GetTranslationsResponse>
    <From>language-code</From>
    <State/>
    <Translations>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText>string-value</MatchedOriginalText>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText/>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
    </Translations>
  </GetTranslationsResponse>
</ArrayOfGetTranslationsResponse>
```

Her `GetTranslationsResponse` öğesi aşağıdaki değerleri içerir:

* `Translations`: Bir dizi eşleşme bulundu, depolanmış `TranslationMatch` (aşağıya bakın) nesneleri. Çeviriler (belirsiz eşleme) özgün metni küçük çeşitlemelerini içerebilir. Çeviriler sıralanır: % 100 eşleşen ilk olarak, aşağıdaki benzer eşleşir.
* `From`: Yöntem belirtmediyseniz bir `From` dili, bu otomatik dil algılama sonucu olacaktır. Aksi durumda olur dilden verilir.
* `State`: Correlate istek ve yanıt yardımcı olmak için kullanıcı durumu. İçinde belirtilen değeri içerir `TranslateOptions` parametresi.

`TranslationMatch` Nesne aşağıdakilerden oluşur:
* `Error`: Belirli bir giriş dizesi için bir hata oluştu, hata kodu depolanır. Aksi takdirde alan boştur.
* `MatchDegree`: Sistem filtresinin eşleşmeleri dahil olmak üzere depolamaya karşılık giriş cümleleri eşleşir.  `MatchDegree` Giriş metni deposunda bulunan özgün metin nasıl yakından eşleştiğini gösterir. Aralık 0 ile 100'e 0 olduğu hiçbir benzerlik döndürülen değer ve 100 tam büyük küçük harf duyarlı eşleşmedir.
* `MatchedOriginalText`: Bu sonuç için eşleşen özgün metin. Yalnızca eşleşen özgün metin giriş metin farklı olup olmadığını döndürdü. Benzer eşleştirme kaynak metnin döndürmek için kullanılır. Microsoft Translator sonuç döndürülmedi.
* `Rating`: Kalite kararı kişinin yetkilisi gösterir. Makine çevirisi 5 derecesi sonuçlara yol açabilir. Yetkili olarak sağlanan çevirileri 6-10 derecesi genellikle sahip adımında anonim olarak sağlanan çevirileri genellikle 1-4 derecesi sahip olur.
* `Count`: Bu derecelendirme bu çeviri seçilme sayısı. Değer 0 otomatik olarak çevrilmiş yanıt için olacaktır.
* `TranslatedText`: Çevrilen metin.


### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

dize

Yanıt içerik türü: uygulama/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme  |(boş)    |Gerekli olursa `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirtecini: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp Apim abonelik anahtarı|(boş)  |Gerekli olursa `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı denetleyin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgi dahil istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API v3 Çeviricisi metne geçirme](../migrate-to-v3.md)










