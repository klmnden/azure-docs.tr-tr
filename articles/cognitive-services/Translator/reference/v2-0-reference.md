---
title: Translator metin çevirisi API'si V2.0
titleSuffix: Azure Cognitive Services
description: V2.0 Translator metin çevirisi API'si için başvuru belgeleri.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: v-jansko
ms.openlocfilehash: b65182cac91f6ed3dc653d6d9e77f80e99346bb7
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918017"
---
# <a name="translator-text-api-v20"></a>Translator metin çevirisi API'si v2.0

> [!IMPORTANT]
> Translator metin çevirisi API'si bu sürümünde kullanım dışı bırakıldı. [Translator Text API v3 belgelerine görüntülemek](v3-0-reference.md).

Translator Text API V2 uygulamalarınızı, Web sitelerini, araçları veya diğer çözümleri çok dilli kullanıcı deneyimleri sağlamak için sorunsuz bir şekilde tümleştirilebilir. Endüstri standartlarından yararlanarak, bunu herhangi bir platformda donanım ve işletim sistemiyle dil çevirisi ve dil algılama metin veya metin okuma gibi dil ilgili diğer işlemleri gerçekleştirmek için kullanılabilir. Microsoft Translator API'si hakkında daha fazla bilgi için buraya tıklayın.

## <a name="getting-started"></a>Başlarken
Translator Text API gerekecek erişmeye [için Microsoft Azure'a kaydolun](../translator-text-how-to-signup.md).

## <a name="authorization"></a>Yetkilendirme
Translator metin çevirisi API'si tüm çağrıların kimliğini doğrulamak için bir abonelik anahtarı gerektirir. API kimlik doğrulamasının iki modu destekler:

* Erişim belirteci kullanarak. Başvurulan abonelik anahtarını kullanın **adım** yetkilendirme hizmete bir POST isteği yaparak bir erişim belirteci oluşturmak için 9. Ayrıntılar için belirteç hizmeti belgelerine bakın. Yetkilendirme üst bilgisi veya access_token sorgu parametresi kullanılarak Translator hizmetine erişim belirtecini geçirin. Erişim belirteci 10 dakika için geçerlidir. 10 dakikada bir yeni bir erişim belirteci edinmek ve bu 10 dakika içinde aynı erişimi kullanarak yönelik yinelenen isteklerden belirteci tutun.

* Bir abonelik anahtarı kullanarak doğrudan. Bir değer olarak abonelik anahtarınızı aktarmak `Ocp-Apim-Subscription-Key` isteğiniz, Translator API'sinde bulunan üst bilgi. Bu modda, bir erişim belirteci oluşturmak için kimlik doğrulama belirteci hizmeti çağırmak zorunda değildir.

Abonelik anahtarınız ve erişim belirteci görünümünden gizlenmelidir gizli diziler olarak göz önünde bulundurun.

## <a name="profanity-handling"></a>Küfür işleme
Normalde Translator hizmeti, mevcut çeviri kaynağında küfür korur. Küfür derecesini ve sözcükler Küfürlü yapan bağlam kültürler arasında farklılık gösterir ve hedef dilde küfür derecesini sonucunda yükseltilmiş veya azalır.

Küfür kaynak metin küfür varlığını bakılmaksızın çevirisini almamak istiyorsanız, bunu destekleyen yöntemler için filtreleme küfür kullanabilirsiniz. Seçenek silinmiş ya da uygun etiketlerle işaretlenmiş küfür görmek isteyip istemediğinizi veya hiçbir eyleme girişilmedi seçmenize olanak tanır. Kabul edilen değerler, `ProfanityAction` olan `NoAction` (varsayılan), Marked ve `Deleted`.


|ProfanityAction    |Eylem |Örnek kaynak (Japonca)  |Örnek çeviri (İngilizce)  |
|:--|:--|:--|:--|
|NoAction   |Varsayılan. Aynı seçenek ayarı bulunamadı. Küfür kaynaktan hedefe geçer.        |彼はジャッカスです。     |He bir jackass olur.   |
|İşaretli     |Küfürlü sözcükleri tarafından XML etiketleri arasına <profanity> ve </profanity>.     |彼はジャッカスです。 |He's bir <profanity>jackass</profanity>.    |
|Silinen    |Küfürlü sözcükleri değiştirme yapmadan çıktı CİHAZDAN kaldırılır.     |彼はジャッカスです。 |He's bir.   |

    
## <a name="excluding-content-from-translation"></a>' Den içeriği hariç
HTML gibi etiket ile içerik çevrilirken (`contentType=text/html`), belirli bir içeriğe çevrilmesi hariç tutmak bazen kullanışlıdır. Öznitelik kullanabilir `class=notranslate` özgün dilinde kalması gereken içeriği belirtmek için. Aşağıdaki örnekte, ilk içeriğini `div` öğesi değil çevrilir, ikinci içeriği çalışırken `div` öğesi çevrilir.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>GET / Çevir

### <a name="implementation-notes"></a>Uygulama Notları
Bir metin dizesi bir dilden diğerine çevirir.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Dönüş değeri:** Çevrilen metni temsil eden bir dize.

Daha önce kullandıysanız `AddTranslation` veya `AddTranslationArray` derecelendirme ile 5 veya daha yüksek aynı kaynak cümle için çeviri girmek için `Translate` sisteminize kullanılabilir ilk seçimi döndürür. "Aynı kaynak cümle" tam aynı (% 100 eşleşen), büyük/küçük harf, boşluk, etiket değerlerini ve bir tümcenin sonuna noktalama dışında anlamına gelir. Ardından 5 veya üzeri bir derecelendirme derecelendirmesi depolanıyorsa döndürülen sonuç olarak Microsoft Translator otomatik çeviri olacaktır.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

string

Yanıt içerik türü: application/xml 

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama    |Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği  |(boş)    |Gereklidir. Yetkilendirme veya Ocp-Apim-Subscription-Key üstbilgi kullandıysanız, AppID alanı boş bırakın başka dahil "Bearer" içeren bir dize + "" + "access_token".|sorgu|string|
|metin|(boş)   |Gereklidir. Çevrilecek metin temsil eden bir dize. Metin boyutu 10000 karakterden uzun olmamalıdır.|sorgu|string|
|başlangıç|(boş)   |İsteğe bağlı. Çeviri metnin dil kodu temsil eden bir dize. Örneğin, biri İngilizce için.|sorgu|string|
|-|(boş) |Gereklidir. Metni Çevir oluşturulacağı dilin kodu temsil eden bir dize.|sorgu|string|
|contentType|(boş)    |İsteğe bağlı. Çevrildikten metin biçimi. Desteklenen biçimler şunlardır: text/plain (varsayılan) ve metin/html. Herhangi bir HTML doğru biçimlendirilmeli, tam bir öğe olması gerekir.|sorgu|string|
|category|(boş)   |İsteğe bağlı. Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak "Genel".|sorgu|string|
|Yetkilendirme|(boş)  |AppID alanı veya Ocp-Apim-Subscription-Key üst bilgisi belirtilmemişse gereklidir. Yetkilendirme belirteci:  "Bearer" + "" + "access_token".|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)  |AppID alanı veya yetkilendirme üst bilgisi belirtilmemişse gereklidir.|üst bilgi|string|


### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgilerinde X-MS-işlem-Info yer istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-translatearray"></a>/TranslateArray gönderin

### <a name="implementation-notes"></a>Uygulama Notları
Kullanım `TranslateArray` çevirileri için birden fazla kaynak metni almak için yöntemi.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

İstek gövdesi biçimi şu şekilde olmalıdır:

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

Öğeleri içinde `TranslateArrayRequest` şunlardır:


* `appid`: Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.
* `from`: İsteğe bağlı. Metni Çevir oluşturulacağı dilin kodu temsil eden bir dize. Boş bırakılırsa yanıt otomatik dil algılama sonucunu içerir.
* `options`: İsteğe bağlı. Bir `Options` aşağıda listelenen değerler içeren bir nesne. Bunlar tümü isteğe bağlıdır ve varsayılan en sık kullanılan ayarları için. Belirtilen öğelerin alfabetik olarak listelenmiş olmalıdır.
    - `Category`: Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak `general`.
    - `ContentType`: Çevrildikten metin biçimi. Desteklenen biçimler `text/plain` (varsayılan), `text/xml` ve `text/html`. Herhangi bir HTML doğru biçimlendirilmeli, tam bir öğe olması gerekir.
    - `ProfanityAction`: Yukarıda açıklandığı gibi profanities nasıl işleneceğini belirtir. Kabul edilen değerler, `ProfanityAction` olan `NoAction` (varsayılan), `Marked` ve `Deleted`.
    - `State`: Performanstaki istek ve yanıt yardımcı olmak için kullanıcı durumu. Aynı içeriğini yanıta döndürülür.
    - `Uri`: Bu URI sonuçları filtreleyin. Varsayılan: `all`.
    - `User`: Bu kullanıcı tarafından sonuçları filtreleyin. Varsayılan: `all`.
* `texts`: Gereklidir. Metin çevirisi içeren bir dizi. Tüm dizeleri aynı dilde olması gerekir. Çevrilecek tüm metinler toplamı 10000 karakterden uzun olmamalıdır. Dizi öğelerinin sayısı 2000'dir.
* `to`: Gereklidir. Metni Çevir oluşturulacağı dilin kodu temsil eden bir dize.

İsteğe bağlı öğeler atlanabilir. TranslateArrayRequest doğrudan alt öğeleri alfabetik olarak listelenmiş olmalıdır.

TranslateArray yöntemi kabul `application/xml` veya `text/xml` için `Content-Type`.

**Dönüş değeri:** A `TranslateArrayResponse` dizisi. Her `TranslateArrayResponse` aşağıdaki öğelere sahiptir:

* `Error`: Biri oluştuğunda bir hata olduğunu gösterir. Aksi takdirde null olarak ayarlanır.
* `OriginalSentenceLengths`: Özgün kaynak metindeki her cümle uzunluğunu belirten bir tamsayı dizisi. Dizinin uzunluğunu cümleler sayısını gösterir.
* `TranslatedText`: Çevrilmiş metin.
* `TranslatedSentenceLengths`: Her bir cümlede çevrilmiş metnin uzunluğunu belirten bir tamsayı dizisi. Dizinin uzunluğunu cümleler sayısını gösterir.
* `State`: Performanstaki istek ve yanıt yardımcı olmak için kullanıcı durumu. İstek olduğu gibi aynı içeriği döndürür.

Yanıt gövdesi biçiminin aşağıdaki gibidir.

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
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
Başarılı bir yanıt bir dizi içeren `TranslateArrayResponse` yukarıda açıklandığı biçimde.

string

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme|(boş)) |AppID alanı veya Ocp-Apim-Subscription-Key üst bilgisi belirtilmemişse gereklidir. Yetkilendirme belirteci:  "Bearer" + "" + "access_token".|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|AppID alanı veya yetkilendirme üst bilgisi belirtilmemişse gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu   |Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin. Sık karşılaşılan hatalar şunlardır: <ul><li>Dizi öğesi boş olamaz</li><li>Geçersiz kategori</li><li>Dili geçersiz</li><li>Dil için geçersiz</li><li>İstek çok fazla öğe içeriyor</li><li>Kaynak dili desteklenmiyor</li><li>Kime dili desteklenmiyor</li><li>Çevirme istek çok fazla veri içeriyor</li><li>HTML doğru biçimde değil.</li><li>Çok fazla dizeleri Çevir istek gönderildi.</li></ul>|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgilerinde X-MS-işlem-Info yer istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Uygulama Notları
Alır kolay adları diller için geçirilen parametre olarak `languageCodes`ve geçirilen yerel ayar dili kullanılarak yerelleştirilmiş.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

İstek gövdesi için kolay adlar almak için ISO 639-1 dil kodlarını temsil eden bir dize dizisi içerir. Örneğin:

```
<ArrayOfstring xmlns:i="https://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Dönüş değeri:** İstenen diline yerelleştirilmiş Translator hizmeti tarafından desteklenen diller adlarını içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
İstenen diline yerelleştirilmiş Translator hizmeti tarafından desteklenen diller adlarını içeren bir dize dizisi.

string

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|string|
|yerel ayar|(boş) |Gereklidir. Bir dil ve dil adlarının yerelleştirmek için ISO 3166 iki harfli büyük alt kodu ile ilişkili bir ISO 639 iki harfli küçük kültür kodu veya bir ISO 639 küçük kültür kodu birleşimi kendisi tarafından temsil eden bir dize.|sorgu|string|
|Yetkilendirme|(boş)  |Gerekli if AppID alanın veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)  |Gerekli if AppID alanın veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgilerinde X-MS-işlem-Info yer istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-getlanguagesfortranslate"></a>/GetLanguagesForTranslate Al

### <a name="implementation-notes"></a>Uygulama Notları
Çeviri hizmeti tarafından desteklenen dilleri gösteren dil kodlarının listesini edinin.  `Translate` ve `TranslateArray` herhangi ikisi bu diller arasında çevirebilir.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Dönüş değeri:** Translator Services tarafından desteklenen dil kodu içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Translator Services tarafından desteklenen dil kodu içeren bir dize dizisi.

string

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|string|
|Yetkilendirme|(boş)  |Gerekli if `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Gerekli if `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgilerinde X-MS-işlem-Info yer istek kimliği ile birlikte belirtin.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-getlanguagesforspeak"></a>/GetLanguagesForSpeak Al

### <a name="implementation-notes"></a>Uygulama Notları
Konuşma sentezi için kullanılabilen dilleri alır.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Dönüş değeri:** Konuşma sentezi için Translator hizmeti tarafından desteklenen dil kodu içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Konuşma sentezi için Translator hizmeti tarafından desteklenen dil kodu içeren bir dize dizisi.

string

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|string|
|Yetkilendirme|(boş)|Gerekli if `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Gerekli if `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|
 
### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401|Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-speak"></a>GET / konuşun

### <a name="implementation-notes"></a>Uygulama Notları
İstenen dilde konuşulan geçilen metni wave veya mp3 akışı döndürür.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Dönüş değeri:** İstenen dilde konuşulan geçilen metni wave veya mp3 akışı.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

İkili

Yanıt içerik türü: application/xml 

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|string|
|metin|(boş)   |Gereklidir. Bir cümle veya belirtilen dil için wave akış söylenir cümleler içeren bir dize. Metni konuşmaya boyutunu 2000 karakterden uzun olmamalıdır.|sorgu|string|
|language|(boş)   |Gereklidir. Metni konuşmaya desteklenen dil kodunu temsil eden bir dize. Kod yönteminden döndürülen kodlarının listesi mevcut olmalıdır `GetLanguagesForSpeak`.|sorgu|string|
|biçim|(boş)|İsteğe bağlı. İçerik türü kimliği belirten bir dize Şu anda `audio/wav` ve `audio/mp3` kullanılabilir. Varsayılan değer `audio/wav` şeklindedir.|sorgu|string|
|seçenekler|(boş)    |<ul><li>İsteğe bağlı. Sentezlenen Konuşma özelliklerini belirten bir dize:<li>`MaxQuality` ve `MinSize` ses sinyaller kalitesini belirtmek kullanılabilir. İle `MaxQuality`, sesler en yüksek kalitede ve ile alabilirsiniz `MinSize`, en küçük boyutu olan kişilerden daha fazlasını elde edebilirsiniz. Varsayılan değer `MinSize`.</li><li>`female` ve `male` ses istenen dinleyicilerinin belirtmek kullanılabilir. `female` varsayılan değerdir. Dikey çubuk kullanın <code>\|</code> birden fazla seçeneği eklenecek. Örneğin `MaxQuality|Male`.</li></li></ul> |sorgu|string|
|Yetkilendirme|(boş)|Gerekli if `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)  |Gerekli if `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-detect"></a>GET / algılayın

### <a name="implementation-notes"></a>Uygulama Notları
Kullanım `Detect` seçilen bir metin parçası dili tanımlamak için yöntemi.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Dönüş değeri:** Verilen metni için iki karakterli dil kodu içeren bir dize. .

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

string

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)  |Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|string|
|metin|(boş)|Gereklidir. Dilinden tanımlanması için olan metin içeren bir dize. Metin boyutu 10000 karakterden uzun olmamalıdır.|sorgu| string|
|Yetkilendirme|(boş)|Gerekli if `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key  |(boş)    |Gerekli if `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|


## <a name="post-detectarray"></a>/DetectArray gönderin

### <a name="implementation-notes"></a>Uygulama Notları
Kullanım `DetectArray` tek seferde bir dize dizisi dili tanımlamak için yöntemi. Her bir dizi öğesine bağımsız olarak algılanmasını gerçekleştirir ve dizinin her satır için bir sonuç döndürür.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

İstek gövdesi biçimi şu şekilde olmalıdır.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

Metin boyutu 10000 karakterden uzun olmamalıdır.

**Dönüş değeri:** Bir giriş dizisinin her satır için iki karakterli dil kodu içeren bir dize dizisi.

Yanıt gövdesi biçiminin aşağıdaki gibidir.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
DetectArray başarılı oldu. Bir giriş dizisinin her satır için iki karakterli dil kodu içeren bir dize dizisi döndürür.

string

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|string|
|Yetkilendirme|(boş)|Gerekli if `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Gerekli if `appid` alan veya yetkilendirme üst bilgisi belirtilmemiş.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgilerinde X-MS-işlem-Info yer istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-addtranslation"></a>/AddTranslation Al

### <a name="implementation-notes"></a>Uygulama Notları

> [!IMPORTANT]
> **KULLANIMDAN KALDIRMA NOT:** 31 Ocak 2018'den sonra bu yöntem yeni bir cümle gönderimler kabul etmez ve bir hata iletisi alırsınız. Lütfen bu duyuru işbirliğine dayalı bir çeviri işlevlere değişiklikler hakkında bakın.

Bir çeviri, çeviri bellek ekler.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

string

Yanıt içerik türü: uygulama: xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü   |
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|string|
|originalText|(boş)|Gereklidir. Gelen Çevrilecek metin içeren bir dize. Dize en fazla 1000 karakter oluşabilir.|sorgu|string|
|translatedText|(boş) |Gereklidir. Çevrilmiş metni hedef dil içeren bir dize. Dize en fazla 2000 karakterden oluşabilir.|sorgu|string|
|başlangıç|(boş)   |Gereklidir. Çeviri metnin dil kodu temsil eden bir dize. tr İngilizce = de Almanca vb. =...|sorgu|string|
|-|(boş)|Gereklidir. Metni Çevir oluşturulacağı dilin kodu temsil eden bir dize.|sorgu|string|
|rating|(boş) |İsteğe bağlı. Bu dize için kalite sıralaması temsil eden bir tamsayı. -10 ile 10 arasında değeri. Varsayılan olarak 1.|sorgu|integer|
|contentType|(boş)    |İsteğe bağlı. Çevrildikten metin biçimi. Desteklenen biçimler şunlardır: "text/plain" ve "text/html". Herhangi bir HTML doğru biçimlendirilmeli, tam bir öğe olması gerekir.   |sorgu|string|
|category|(boş)|İsteğe bağlı. Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak "Genel".|sorgu|string|
|kullanıcı|(boş)|Gereklidir. Gönderim gönderene izlemek için kullanılan bir dize.|sorgu|string|
|uri|(boş)|İsteğe bağlı. Bu çeviri içerik konumunu içeren bir dize.|sorgu|string|
|Yetkilendirme|(boş)|Gerekli if AppID alanın veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.    |üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|Gerekli if `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|410|AddTranslation artık desteklenmiyor.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgilerinde X-MS-işlem-Info yer istek kimliği ile birlikte belirtin.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-addtranslationarray"></a>POST /AddTranslationArray

### <a name="implementation-notes"></a>Uygulama Notları

> [!IMPORTANT]
> **KULLANIMDAN KALDIRMA NOT:** 31 Ocak 2018'den sonra bu yöntem yeni bir cümle gönderimler kabul etmez ve bir hata iletisi alırsınız. Lütfen bu duyuru işbirliğine dayalı bir çeviri işlevlere değişiklikler hakkında bakın.

Çevirileri çeviri bellek eklemek için bir dizi ekler. Bu bir dizi sürümüdür `AddTranslation`.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

İstek gövdesi biçimi aşağıdaki gibidir.

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

AddtranslationsRequest öğe içindeki öğeler şunlardır:

* `AppId`: Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.
* `From`: Gereklidir. Kaynak dili dil kodu içeren bir dize. Tarafından döndürülen dillerden biri olmalıdır `GetLanguagesForTranslate` yöntemi.
* `To`: Gereklidir. Hedef Dil dil kodu içeren bir dize. Tarafından döndürülen dillerden biri olmalıdır `GetLanguagesForTranslate` yöntemi.
* `Translations`: Gereklidir. Çevirileri için çeviri bellek eklemek için bir dizi. Her çeviri içermelidir: originalText, translatedText ve derecelendirmesi. Her bir originalText ve translatedText boyutu için 1000 karakter sınırlıdır. Toplam originalText(s) ve translatedText(s) 10000 karakterden uzun olmamalıdır. Dizi öğelerinin sayısı 100'dür.
* `Options`: Gereklidir. Kategori, ContentType, URI ve kullanıcı dahil seçenekleri kümesi. Kullanıcı gereklidir. Kategori, ContentType ve URI isteğe bağlıdır. Belirtilen öğelerin alfabetik olarak listelenmiş olmalıdır.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
AddTranslationArray yöntemi başarılı oldu. 31 Ocak 2018'den sonra cümle gönderimler kabul edilmez. Hizmet 410 hata kodu ile yanıt verecektir.

string

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme|(boş)|AppID alanı veya Ocp-Apim-Subscription-Key üst bilgisi belirtilmemişse gereklidir. Yetkilendirme belirteci:  "Bearer" + "" + "access_token".|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|AppID alanı veya yetkilendirme üst bilgisi belirtilmemişse gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|410    |AddTranslation artık desteklenmiyor.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-breaksentences"></a>/BreakSentences Al

### <a name="implementation-notes"></a>Uygulama Notları
Bir metin parçası cümleler böler ve her cümle içinde uzunluklarını içeren bir dizi döndürür.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Dönüş değeri:** Cümleleri uzunluklarının temsil eden tamsayı dizisi. Dizinin uzunluğunu cümleler sayısıdır ve her cümle uzunluğunu değerlerdir.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Cümleleri uzunluklarının temsil eden tamsayı dizisi. Dizinin uzunluğunu cümleler sayısıdır ve her cümle uzunluğunu değerlerdir.

integer

Yanıt içerik türü: application/xml 

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)  |Gereklidir. Yetkilendirme veya Ocp-Apim-Subscription-Key üstbilgi kullandıysanız, AppID alanı boş bırakın başka dahil "Bearer" içeren bir dize + "" + "access_token".|sorgu| string|
|metin|(boş)   |Gereklidir. Cümleleri bölmek için metin temsil eden bir dize. Metin boyutu 10000 karakterden uzun olmamalıdır.|sorgu|string|
|language   |(boş)    |Gereklidir. Giriş metni dil kodunu temsil eden bir dize.|sorgu|string|
|Yetkilendirme|(boş)|AppID alanı veya Ocp-Apim-Subscription-Key üst bilgisi belirtilmemişse gereklidir. Yetkilendirme belirteci:  "Bearer" + "" + "access_token".    |üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)|AppID alanı veya yetkilendirme üst bilgisi belirtilmemişse gereklidir.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401|Geçersiz kimlik bilgileri|
|500|Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgilerinde X-MS-işlem-Info yer istek kimliği ile birlikte belirtin.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Uygulama Notları
Çevirileri belirtilen dil çifti için bir dizi, deponun ve MT altyapısı alır. Tüm kullanılabilir çevirileri döndürür gibi GetTranslations çevirme farklıdır.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

İstek gövdesi aşağıdaki biçime sahiptir isteğe bağlı TranslationOptions nesnesini içerir.

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

`TranslateOptions` Nesne aşağıda listelenen değerler içerir. Bunlar tümü isteğe bağlıdır ve varsayılan en sık kullanılan ayarları için. Belirtilen öğelerin alfabetik olarak listelenmiş olmalıdır.

* `Category`: Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak "Genel".
* `ContentType`: Desteklenen tek ve "text/plain" varsayılan seçenektir.
* `IncludeMultipleMTAlternatives`: Boole bayrağı birden fazla alternatifleri MT altyapısından döndürülüp döndürülmeyeceğini belirler. Geçerli değerler true ve false (büyük-küçük harfe duyarlı). Varsayılan değer false'tur ve yalnızca 1 seçenek içerir. Bayrağı true olarak ayarlamak, çeviri, işbirliğine dayalı çevirileri framework (CTF) ile tamamen tümleşik yapay alternatifleri oluşturmak için sağlar. Kod Çözücü en iyi n listesinden yapay alternatifleri ekleyerek CTF içinde hiçbir alternatifleri sahip cümleleri alternatifleri döndürmek için bu özellik sağlar.
    - Derecelendirmeleri derecelendirmeleri şu şekilde uygulanır: 1) 5 derecesi en iyi bir otomatik çeviri var. 2) CTF'den diğer yollar için -10 + 10'dan Gözden Geçiren yetkilisi yansıtır. 3) otomatik olarak oluşturulan (en iyi n) çeviri alternatifleri de derecesi 0 ve 100 eşleşme derecesi sahiptir.
    - Döndürülen alternatifleri sayısı kadar maxTranslations olmakla birlikte daha az olabilir alternatifleri sayısı.
    - Dil çiftleri bu işlevselliği çevirileri Basitleştirilmiş ve Geleneksel Çince, her iki yönde de arasında kullanılamaz. Diğer tüm Microsoft Translator desteklenen dil çiftleri için kullanılabilir.
* `State`: Performanstaki istek ve yanıt yardımcı olmak için kullanıcı durumu. Aynı içeriğini yanıta döndürülür.
* `Uri`: Bu URI sonuçları filtreleyin. Değer ayarlanmışsa varsayılan değer tümüdür.
* `User`: Bu kullanıcı tarafından sonuçları filtreleyin. Değer ayarlanmışsa varsayılan değer tümüdür.

İstek `Content-Type` olmalıdır `text/xml`.

**Dönüş değeri:** Yanıt biçimi aşağıdaki gibidir.

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
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

* `Translations`: Bir dizi bir eşleşme bulunursa, TranslationMatch (aşağıya bakın) nesneleri depolanır. Çevirileri hafif varyantlarının (benzer öğe eşleştirmesi) özgün metin içerebilir. Çevirileri sıralanır: % 100 aşağıdaki belirsiz eşleşen ilk olarak eşleşir.
* `From`: Yöntemin ilk dil belirtmediyseniz bu otomatik dil algılama sonucu olacaktır. Aksi durumda olur dili verilir.
* `State`: Performanstaki istek ve yanıt yardımcı olmak için kullanıcı durumu. TranslateOptions parametresinde belirtilen değeri içerir.

TranslationMatch nesne aşağıdakilerden oluşur:

* `Error`: Belirli bir giriş dizesini bir hata oluştu hata kodu saklanır. Aksi takdirde alanı boş.
* `MatchDegree`: Sistem filtresinin eşleşmeleri de dahil olmak üzere deposu ile karşılaştırarak giriş cümleler eşleşir.  Ne kadar yakın giriş metni depoda orijinal metinle eşleşen MatchDegree gösterir. Aralık 0-100'e 0 olduğu hiçbir benzerlik döndürülen değer ve 100 tam büyük/küçük harf duyarlı eşleşmedir.
MatchedOriginalText: Bu sonuç için eşleştirildi orijinal metni. Yalnızca eşleşen orijinal metni giriş metin farklı döndürdü. Benzer eşleştirme kaynak metnin döndürmek için kullanılır. Microsoft Translator sonuçları döndürülmez.
* `Rating`: Kalite kararı kişinin yetkilisi gösterir. Makine çevirisi 5 derecesi sonuçlara yol açabilir. Yetkili olarak sağlanan çevirileri 6-10 derecesi genellikle sahip artırabileceksiniz anonim olarak sağlanan çevirileri genellikle 1-4 derecesi sahiptir.
* `Count`: Bu çeviri Bu derecelendirme seçili sayısı. Değer otomatik olarak çevrilmiş yanıtı için 0 olacaktır.
* `TranslatedText`: Çevrilmiş metin.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
A `GetTranslationsResponse` yukarıda açıklanan biçimde bir nesne.

string

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Uygulama Kimliği|(boş)|Gereklidir. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, AppID alanı boş bırakın, aksi takdirde dahil içeren bir dize `"Bearer" + " " + "access_token"`.|sorgu|string|
|metin|(boş)|Gereklidir. Çevrilecek metin temsil eden bir dize. Metin boyutu 10000 karakterden uzun olmamalıdır.|sorgu|string|
|başlangıç|(boş)|Gereklidir. Çeviri metnin dil kodu temsil eden bir dize.|sorgu|string|
|- |(boş)    |Gereklidir. Metni Çevir oluşturulacağı dilin kodu temsil eden bir dize.|sorgu|string|
|maxTranslations|(boş)|Gereklidir. Döndürülecek çevirileri maksimum sayısını temsil eden bir tamsayı.|sorgu|integer|
|Yetkilendirme| (boş)|Gerekli if `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|string| üst bilgi|
|Ocp-Apim-Subscription-Key|(boş)  |Gerekli if `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-gettranslationsarray"></a>/GetTranslationsArray gönderin

### <a name="implementation-notes"></a>Uygulama Notları
Kullanım `GetTranslationsArray` birden fazla kaynak metni için birden fazla çeviri aday almak için yöntemi.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

İstek gövdesi biçimi aşağıdaki gibidir.

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

* `AppId`: Gereklidir. Yetkilendirme üst bilgisi kullandıysanız, AppID alanı boş bırakın başka dahil içeren bir dize `"Bearer" + " " + "access_token"`.
* `From`: Gereklidir. Çeviri metnin dil kodu temsil eden bir dize.
* `MaxTranslations`: Gereklidir. Döndürülecek çevirileri maksimum sayısını temsil eden bir tamsayı.
* `Options`: İsteğe bağlı. Aşağıda listelenen değerleri içeren bir seçenekler nesne. Bunlar tümü isteğe bağlıdır ve varsayılan en sık kullanılan ayarları için. Belirtilen öğelerin alfabetik olarak listelenmiş olmalıdır.
    - Kategori ': Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan olarak genel.
    - `ContentType`: Desteklenen tek ve varsayılan olarak, metin/düz bir seçenektir.
    - `IncludeMultipleMTAlternatives`: Boole bayrağı birden fazla alternatifleri MT altyapısından döndürülüp döndürülmeyeceğini belirler. Geçerli değerler true ve false (büyük-küçük harfe duyarlı). Varsayılan değer false'tur ve yalnızca 1 seçenek içerir. Bayrağı true olarak ayarlamak, çeviri, işbirliğine dayalı çevirileri framework (CTF) ile tamamen tümleşik yapay alternatifleri oluşturmak için sağlar. Kod Çözücü en iyi n listesinden yapay alternatifleri ekleyerek CTF içinde hiçbir alternatifleri sahip cümleleri alternatifleri döndürmek için bu özellik sağlar.
        - Derecelendirmeleri derecelendirmeleri şu şekilde uygulanır: 1) 5 derecesi en iyi bir otomatik çeviri var. 2) CTF'den diğer yollar için -10 + 10'dan Gözden Geçiren yetkilisi yansıtır. 3) otomatik olarak oluşturulan (en iyi n) çeviri alternatifleri de derecesi 0 ve 100 eşleşme derecesi sahiptir.
        - Döndürülen alternatifleri sayısı kadar maxTranslations olmakla birlikte daha az olabilir alternatifleri sayısı.
        - Dil çiftleri bu işlevselliği çevirileri Basitleştirilmiş ve Geleneksel Çince, her iki yönde de arasında kullanılamaz. Diğer tüm Microsoft Translator desteklenen dil çiftleri için kullanılabilir.
* `State`: Performanstaki istek ve yanıt yardımcı olmak için kullanıcı durumu. Aynı içeriğini yanıta döndürülür.
* `Uri`: Bu URI sonuçları filtreleyin. Değer ayarlanmışsa varsayılan değer tümüdür.
* `User`: Bu kullanıcı tarafından sonuçları filtreleyin. Değer ayarlanmışsa varsayılan değer tümüdür.
* `Texts`: Gereklidir. Metin çevirisi içeren bir dizi. Tüm dizeleri aynı dilde olması gerekir. Çevrilecek tüm metinler toplamı 10000 karakterden uzun olmamalıdır. Dizi öğelerinin sayısı 10'dur.
* `To`: Gereklidir. Metni Çevir oluşturulacağı dilin kodu temsil eden bir dize.

İsteğe bağlı öğeler atlanabilir. Doğrudan alt öğesi olan öğeleri `GetTranslationsArrayRequest` alfabetik olarak listelenmiş olmalıdır.

İstek `Content-Type` olmalıdır `text/xml`.

**Dönüş değeri:** Yanıt biçimi aşağıdaki gibidir.

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
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

* `Translations`: Bir dizi eşleşme bulundu, depolanan `TranslationMatch` nesneleri (aşağıya bakın). Çevirileri hafif varyantlarının (benzer öğe eşleştirmesi) özgün metin içerebilir. Çevirileri sıralanır: % 100 aşağıdaki belirsiz eşleşen ilk olarak eşleşir.
* `From`: Yöntem belirtmediyseniz bir `From` dil, bu otomatik dil algılama sonucu olacaktır. Aksi durumda olur dili verilir.
* `State`: Performanstaki istek ve yanıt yardımcı olmak için kullanıcı durumu. İçinde haliyle aynı değeri içeren `TranslateOptions` parametresi.

`TranslationMatch` Nesne aşağıdakilerden oluşur:
* `Error`: Belirli bir giriş dizesini bir hata oluştu hata kodu saklanır. Aksi takdirde alanı boş.
* `MatchDegree`: Sistem filtresinin eşleşmeleri de dahil olmak üzere deposu ile karşılaştırarak giriş cümleler eşleşir.  `MatchDegree` ne kadar yakın giriş metni depoda orijinal metinle eşleşen gösterir. Aralık 0-100'e 0 olduğu hiçbir benzerlik döndürülen değer ve 100 tam büyük/küçük harf duyarlı eşleşmedir.
* `MatchedOriginalText`: Bu sonuç için eşleştirildi orijinal metni. Yalnızca eşleşen orijinal metni giriş metin farklı döndürdü. Benzer eşleştirme kaynak metnin döndürmek için kullanılır. Microsoft Translator sonuçları döndürülmez.
* `Rating`: Kalite kararı kişinin yetkilisi gösterir. Makine çevirisi 5 derecesi sonuçlara yol açabilir. Yetkili olarak sağlanan çevirileri 6-10 derecesi genellikle sahip artırabileceksiniz anonim olarak sağlanan çevirileri genellikle 1-4 derecesi sahiptir.
* `Count`: Bu çeviri Bu derecelendirme seçili sayısı. Değer otomatik olarak çevrilmiş yanıtı için 0 olacaktır.
* `TranslatedText`: Çevrilmiş metin.


### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

string

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre Türü|Veri Türü|
|:--|:--|:--|:--|:--|
|Yetkilendirme  |(boş)    |Gerekli if `appid` alan veya `Ocp-Apim-Subscription-Key` üstbilgisi belirtilmedi. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|string|
|Ocp-Apim-Subscription-Key|(boş)  |Gerekli if `appid` alan veya `Authorization` üstbilgisi belirtilmedi.|üst bilgi|string|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Neden|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [V3 Translator metin API'si geçirme](../migrate-to-v3.md)










