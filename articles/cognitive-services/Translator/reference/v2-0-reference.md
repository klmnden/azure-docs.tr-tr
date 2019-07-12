---
title: Translator metin çevirisi API'si v2.0
titleSuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si v2.0 için başvuru belgeleri.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: swmachan
ms.openlocfilehash: c18c062d5537603284acb37081ac0a4eb8d2fd20
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797810"
---
# <a name="translator-text-api-v20"></a>Translator metin çevirisi API'si v2.0

> [!IMPORTANT]
> Translator metin çevirisi API'si bu sürümünde kullanım dışı bırakıldı. [Translator metin çevirisi API'si, sürüm 3 için belgeleri görüntülemek](v3-0-reference.md).

Translator metin çevirisi API'si, sürüm 2, uygulamalarınızı, Web siteleri, Araçlar veya diğer çözümleri, çok dilli kullanıcı deneyimleri sağlamak için sorunsuz bir şekilde tümleştirilebilir. Bunu herhangi bir platformda donanım ve işletim sistemiyle dil çevirisi ve metin dil algılama ve metin okuma, gibi diğer dil ile ilgili görevleri gerçekleştirmek için endüstri standartlarına göre kullanabilirsiniz. Daha fazla bilgi için [Translator Text API](../translator-info-overview.md).

## <a name="getting-started"></a>Başlarken
Translator metin çevirisi API'si erişmek için yapmanız [için Microsoft Azure'a kaydolun](../translator-text-how-to-signup.md).

## <a name="authentication"></a>Authentication 
Translator metin çevirisi API'si tüm çağrıları, kimlik doğrulaması için bir abonelik anahtarı gerektirir. API, üç kimlik doğrulama yöntemlerini destekler:

- Bir erişim belirteci. 9\. adımda başvurulan abonelik anahtarı kimlik doğrulaması hizmete bir POST isteği yaparak bir erişim belirteci oluşturmak için kullanın. Ayrıntılar için belirteç hizmeti belgelerine bakın. Erişim belirteci kullanarak Translator hizmete geçirin `Authorization` üst bilgi veya `access_token` sorgu parametresi. Erişim belirteci 10 dakika için geçerlidir. 10 dakikada bir yeni bir erişim belirteci edinmek ve 10 dakika içinde aynı erişimi kullanarak yönelik yinelenen isteklerden belirteci tutun.
- Doğrudan kullanılan bir abonelik anahtarı. Bir değer olarak abonelik anahtarınızı aktarmak `Ocp-Apim-Subscription-Key` isteğiniz, Translator metin çevirisi API'si-bulunan üst bilgi. Abonelik anahtarı doğrudan kullandığınızda, bir erişim belirteci oluşturmak için belirteci kimlik doğrulaması hizmeti çağırmak zorunda değilsiniz.
- Bir [Azure Bilişsel hizmetler hizmetli abonelik](https://azure.microsoft.com/pricing/details/cognitive-services/). Bu yöntem, birden çok hizmeti isteklerinde kimlik doğrulaması için tek bir gizli anahtar kullanmanıza olanak sağlar.
Birden çok hizmet bir gizli anahtar kullandığınızda, isteğinizle birlikte iki kimlik doğrulama üst bilgileri eklemeniz gerekir. İlk üstbilgi gizli anahtar geçirir. İkinci başlık aboneliğinizle ilişkilendirilmiş bir bölgeye belirtir:
   - `Ocp-Apim-Subscription-Key`
   - `Ocp-Apim-Subscription-Region`

Bölge için birden çok hizmet Text API aboneliği gereklidir. Seçtiğiniz bölge hizmetli abonelik anahtarını kullanırken metin çevirisi için kullanabileceğiniz tek bölgedir. Bu, çoklu hizmet aboneliğinizi Azure Portal'da için RMS'ye kaydolurken seçtiğiniz aynı bölgede olması gerekir.

Kullanılabilen bölgeler `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `japaneast`, `northeurope`, `southcentralus`, `southeastasia`, `uksouth`, `westcentralus`, `westeurope`, `westus`, ve `westus2`.

Abonelik anahtarınızı ve erişim görünümünden gizlenmelidir gizli belirteç.

## <a name="profanity-handling"></a>Küfür işleme
Normalde, Translator hizmeti kaynakta mevcut olan küfür korur. Küfür ve sözcükler Küfürlü yapan bağlam derecesini kültüre göre farklılık gösterir. Bu nedenle hedef dilde küfür derecesini artırılacağı veya.

Kaynak metni olsa bile çevirisini küfür engellemek istiyorsanız, bunu destekleyen yöntemler için filtreleme küfür kullanabilirsiniz. Seçeneği veya silinmiş ya da uygun etiketlerle işaretlenmiş küfür görmek istediğiniz hedef küfür izin vermek isteyip istemediğinizi seçmenizi sağlar. Kabul edilen değerler, `ProfanityAction` olan `NoAction` (varsayılan), `Marked`, ve `Deleted`.


|ProfanityAction    |Action |Örnek kaynak (Japonca)  |Örnek çeviri (İngilizce)  |
|:--|:--|:--|:--|
|NoAction   |Varsayılan. Aynı seçenek ayarı bulunamadı. Küfür kaynaktan hedefe geçer.        |彼はジャッカスです。     |He bir jackass olur.   |
|İşaretli     |Küfürlü sözcükleri tarafından XML etiketleri arasına \<küfür > ve \</profanity >.       |彼はジャッカスです。 |He's bir \<küfür > jackass\</profanity >.  |
|Silme    |Küfürlü sözcükleri değiştirme yapmadan çıktı CİHAZDAN kaldırılır.     |彼はジャッカスです。 |He's bir.   |

    
## <a name="excluding-content-from-translation"></a>' Den içeriği hariç
HTML etiketlerini içerikle Çevir ne zaman (`contentType=text/html`), belirli bir içeriğe çevrilmesi hariç tutmak bazen kullanışlıdır. Öznitelik kullanabileceğiniz `class=notranslate` özgün dilinde kalması gereken içeriği belirtmek için. Aşağıdaki örnekte, ilk içerik `div` öğesi olmaz dönüştürülür, ancak ikinci içeriği `div` öğesi çevrilir.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>GET / Çevir

### <a name="implementation-notes"></a>Uygulama Notları
Bir metin dizesi bir dilden diğerine çevirir.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Dönüş değeri:** Çevrilen metni temsil eden bir dize.

Daha önce kullandıysanız `AddTranslation` veya `AddTranslationArray` derecelendirme ile 5 veya daha yüksek aynı kaynak cümle için çeviri girmek için `Translate` sisteminize kullanılabilir ilk seçimi döndürür. "Aynı kaynak cümle" büyük/küçük harf, boşluk, etiket değerlerini ve bir tümcenin sonuna noktalama dışında tam olarak aynı (% 100 eşleme için),'anlamına gelir. 5 veya üzeri bir derecelendirme derecelendirmesi depolanırsa, sonuç olarak Microsoft Translator otomatik çeviri olacaktır.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

dize

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama    |Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid  |(boş)    |Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|text|(boş)   |Gerekli. Çevrilecek metin temsil eden bir dize. Metin, 10. 000'den fazla karakter içeremez.|query|dize|
|from|(boş)   |İsteğe bağlı. Çevrildikten metnin dil kodu temsil eden bir dize. Örneğin, biri İngilizce için.|query|dize|
|-|(boş) |Gerekli. Metne çevirmek için dil kodunu temsil eden bir dize.|query|dize|
|contentType|(boş)    |İsteğe bağlı. Çevrildikten metin biçimi. Desteklenen biçimler `text/plain` (varsayılan) ve `text/html`. HTML öğelerinin öğeleri doğru biçimlendirilmiş ve eksiksiz olması gerekir.|query|dize|
|category|(boş)   |İsteğe bağlı. Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan, `general` değeridir.|query|dize|
|Authorization|(boş)  |Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)  |Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|


### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-translatearray"></a>/TranslateArray gönderin

### <a name="implementation-notes"></a>Uygulama Notları
Çevirileri için birden fazla kaynak metni alır.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

İstek gövdesi biçimi şu şekildedir:

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

Bu öğeleri bulunan `TranslateArrayRequest`:


* `AppId`: Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `AppId` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.
* `From`: İsteğe bağlı. Çevrildikten metnin dil kodu temsil eden bir dize. Bu alan boş bırakılırsa, yanıt otomatik dil algılama sonucunu içerir.
* `Options`: İsteğe bağlı. Bir `Options` aşağıdaki değerleri içeren nesne. Bunlar tüm isteğe bağlı ve varsayılan için en yaygın ayarlar. Belirtilen öğelerin alfabetik olarak listelenmiş olmalıdır.
    - `Category`: Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan, `general` değeridir.
    - `ContentType`: Çevrildikten metin biçimi. Desteklenen biçimler `text/plain` (varsayılan), `text/xml`, ve `text/html`. HTML öğelerinin öğeleri doğru biçimlendirilmiş ve eksiksiz olması gerekir.
    - `ProfanityAction`: Daha önce açıklandığı gibi profanities işlenme belirtir. Kabul edilen değerler `NoAction` (varsayılan), `Marked`, ve `Deleted`.
    - `State`: İstek ve yanıt ilişkilendirmenize yardımcı olması için kullanıcı durumu. Yanıtta aynı içerik döndürülür.
    - `Uri`: Bu URI sonuçları filtreleyin. Varsayılan: `all`.
    - `User`: Bu kullanıcı tarafından sonuçları filtreleyin. Varsayılan: `all`.
* `Texts`: Gerekli. Metin çevirisi içeren bir dizi. Tüm dizeleri aynı dilde olması gerekir. Çevrilecek tüm metin toplam 10.000 karakterden uzun olamaz. Dizi öğelerinin sayısı 2. 000 ' dir.
* `To`: Gerekli. Metne çevirmek için dil kodunu temsil eden bir dize.

İsteğe bağlı öğeleri atlayabilirsiniz. Doğrudan alt öğeleri `TranslateArrayRequest` alfabetik olarak listelenmiş olmalıdır.

`TranslateArray` Yöntemi kabul `application/xml` veya `text/xml` için `Content-Type`.

**Dönüş değeri:** A `TranslateArrayResponse` dizisi. Her `TranslateArrayResponse` bu öğelere sahiptir:

* `Error`: Biri oluşursa bir hata olduğunu gösterir. Aksi takdirde null olarak ayarlanır.
* `OriginalSentenceLengths`: Kaynak metindeki her cümle uzunluğunu belirten bir tamsayı dizisi. Dizinin uzunluğunu cümleler sayısını gösterir.
* `TranslatedText`: Çevrilmiş metin.
* `TranslatedSentenceLengths`: Her bir cümlede çevrilmiş metnin uzunluğunu belirten bir tamsayı dizisi. Dizinin uzunluğunu cümleler sayısını gösterir.
* `State`: İstek ve yanıt ilişkilendirmenize yardımcı olması için kullanıcı durumu. İstek aynı içeriğe döndürür.

Yanıt gövdesi biçiminin şu şekildedir:

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
Başarılı bir yanıt bir dizi içeren `TranslateArrayResponse` daha önce açıklanan biçimde bir dizi.

dize

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Authorization|(boş)  |Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)|Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu   |Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin. Sık karşılaşılan hatalar şunlardır: <ul><li>Dizi öğesi boş olamaz.</li><li>Geçersiz kategori.</li><li>Dil geçersiz.</li><li>Dil için geçersiz.</li><li>İstek çok fazla öğe içeriyor.</li><li>Kaynak dili desteklenmiyor.</li><li>Kime dili desteklenmiyor.</li><li>Çevirme istek çok fazla veri içeriyor.</li><li>HTML doğru biçimde değil.</li><li>Çok fazla dizeleri, istek Çevir geçirilmiş.</li></ul>|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Uygulama Notları
Alır kolay adları diller için geçirilen parametre olarak `languageCodes`, içine geçirilen yerelleştirilmiş `locale` dili.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

İstek gövdesi kolay adları almak istediğiniz ISO 639-1 dil kodlarını temsil eden bir dize dizisi içerir. Bir örneği aşağıda verilmiştir:

```
<ArrayOfstring xmlns:i="https://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Dönüş değeri:** İstenen diline yerelleştirilmiş Translator hizmeti tarafından desteklenen dil adlarını içeren dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
İstenen diline yerelleştirilmiş Translator hizmeti tarafından desteklenen diller adlarını içeren dize dizisi.

dize

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid|(boş)|Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|Yerel ayar|(boş) |Gerekli. Dil adlarını yerelleştirmek için aşağıdakilerden birini temsil eden bir dize kullanılır: <ul><li>Bir dil ile ilişkili bir ISO 639 iki harfli küçük kültür kodu ve ISO 3166 iki harfli büyük alt kodu birleşimi. <li>Tek başına bir ISO 639 küçük kültür kodu.|query|dize|
|Authorization|(boş)  |Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)  |Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-getlanguagesfortranslate"></a>/GetLanguagesForTranslate Al

### <a name="implementation-notes"></a>Uygulama Notları
Çeviri hizmeti tarafından desteklenen dilleri gösteren dil kodlarının listesini alır.  `Translate` ve `TranslateArray` herhangi ikisi bu diller arasında çevirebilir.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Dönüş değeri:** Translator hizmeti tarafından desteklenen dil kodu içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Translator hizmeti tarafından desteklenen dil kodu içeren bir dize dizisi.

dize

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid|(boş)|Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|Authorization|(boş)  |Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)|Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-getlanguagesforspeak"></a>/GetLanguagesForSpeak Al

### <a name="implementation-notes"></a>Uygulama Notları
Konuşma sentezi için kullanılabilen dilleri alır.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Dönüş değeri:** Konuşma sentezi için Translator hizmeti tarafından desteklenen dil kodu içeren bir dize dizisi.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Konuşma sentezi için Translator hizmeti tarafından desteklenen dil kodu içeren bir dize dizisi.

dize

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid|(boş)|Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|Authorization|(boş)|Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)|Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|
 
### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401|Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-speak"></a>GET / konuşun

### <a name="implementation-notes"></a>Uygulama Notları
İstenen dilde konuşma geçilen metni MP3 ya da WAV akışı döndürür.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Dönüş değeri:** İstenen dilde konuşma geçilen metin ya da WAV MP3 bir akış.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

binary

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid|(boş)|Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|text|(boş)   |Gerekli. Belirtilen dilde stream için konuşulan için bir veya daha fazla cümleleri içeren bir dize. Metin 2000 karakterden uzun olmamalıdır.|query|dize|
|dil|(boş)   |Gerekli. Metni konuşmaya dilin desteklenen dil kodu temsil eden bir dize. Yöntem tarafından döndürülen kodları kod olmalıdır `GetLanguagesForSpeak`.|query|dize|
|format|(boş)|İsteğe bağlı. İçerik türü kimliği belirten bir dize Şu anda `audio/wav` ve `audio/mp3` kullanılabilir. Varsayılan değer `audio/wav` şeklindedir.|query|dize|
|options|(boş)    |İsteğe bağlı. Sentezlenen Konuşma özelliklerini belirten bir dize:<ul><li>`MaxQuality` ve `MinSize` ses sinyalini kalitesini belirtin. `MaxQuality` en yüksek kalitede sağlar. `MinSize` en küçük dosya boyutu sağlar. Varsayılan `MinSize`.</li><li>`female` ve `male` ses istenen dinleyicilerinin belirtin. Varsayılan, `female` değeridir. Dikey çubuk kullanın (<code>\|</code>) birden fazla seçeneği eklenecek. Örneğin, `MaxQuality|Male`.</li></li></ul>  |query|dize|
|Authorization|(boş)|Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)  |Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-detect"></a>GET / algılayın

### <a name="implementation-notes"></a>Uygulama Notları
Metnin bir bölümünü dilini tanımlar.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Dönüş değeri:** Bir metin için iki karakterli dil kodu içeren bir dize.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

dize

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid|(boş)  |Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|text|(boş)|Gerekli. Tanıtılması dilinden olduğu metin içeren bir dize. Metin 10.000 karakterden uzun olmamalıdır.|query|  dize|
|Authorization|(boş)|Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key  |(boş)    |Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|


## <a name="post-detectarray"></a>/DetectArray gönderin

### <a name="implementation-notes"></a>Uygulama Notları

Dize dizisi dillerde tanımlar. Bağımsız olarak her bir dizi öğesine dilini algılar ve dizinin her satır için bir sonuç döndürür.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

İstek gövdesi biçimi şu şekildedir:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

Metin 10.000 karakterden uzun olamaz.

**Dönüş değeri:** Giriş dizisinin her satır için iki karakterli dil kodu içeren bir dize dizisi.

Yanıt gövdesi biçiminin şu şekildedir:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
`DetectArray` başarılı oldu. Giriş dizisinin her satır için iki karakterli dil kodu içeren bir dize dizisi döndürür.

dize

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid|(boş)|Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|Authorization|(boş)|Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)|Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-addtranslation"></a>/AddTranslation Al

### <a name="implementation-notes"></a>Uygulama Notları

> [!IMPORTANT]
> **Kullanımdan kaldırma Not:** 31 Ocak 2018'den sonra bu yöntem, yeni cümle gönderimler kabul edilmeyecektir. Bir hata iletisi alırsınız. Lütfen değişiklikleri işbirliğine dayalı Translation Framework (CTF) için duyurusuna bakın.

Bir çeviri, çeviri bellek ekler.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

dize

Yanıt içerik türü: uygulama: xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü   |
|:--|:--|:--|:--|:--|
|appid|(boş)|Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|originalText|(boş)|Gerekli. Çevrilecek metin içeren bir dize. Maksimum dize uzunluğunu 1.000 karakterdir.|query|dize|
|translatedText|(boş) |Gerekli. Metin içeren bir dize hedef dile çevrilir. Maksimum dize uzunluğunu 2.000 karakter olabilir.|query|dize|
|from|(boş)   |Gerekli. Özgün dilindeki metin dil kodunu temsil eden bir dize. Örneğin, biri İngilizce ve Almanca için de için.|query|dize|
|-|(boş)|Gerekli. Metne çevrilecek dilinin dil kodunu temsil eden bir dize.|query|dize|
|rating|(boş) |İsteğe bağlı. Dize için kalite sıralaması temsil eden bir tamsayı. , -10 ile 10 arasında değerdir. Varsayılan değer 1'dir.|query|integer|
|contentType|(boş)    |İsteğe bağlı. Çevrildikten metin biçimi. Desteklenen biçimler `text/plain` ve `text/html`. HTML öğelerinin öğeleri doğru biçimlendirilmiş ve eksiksiz olması gerekir.    |query|dize|
|category|(boş)|İsteğe bağlı. Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan, `general` değeridir.|query|dize|
|kullanıcı|(boş)|Gerekli. Gönderim gönderene izlemek için kullanılan bir dize.|query|dize|
|URI|(boş)|İsteğe bağlı. Çeviri içerik konumunu içeren bir dize.|query|dize|
|Authorization|(boş)|Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.  |üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)|Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|410|`AddTranslation` artık desteklenmiyor.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-addtranslationarray"></a>POST /AddTranslationArray

### <a name="implementation-notes"></a>Uygulama Notları

> [!IMPORTANT]
> **Kullanımdan kaldırma Not:** 31 Ocak 2018'den sonra bu yöntem, yeni cümle gönderimler kabul edilmeyecektir. Bir hata iletisi alırsınız. Lütfen değişiklikleri işbirliğine dayalı Translation Framework (CTF) için duyurusuna bakın.

Bir dizi çevirileri için çeviri bellek ekler. Bu yöntem, bir dizi sürümüdür `AddTranslation`.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

İstek gövdesi biçimi şu şekildedir:

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

Bu öğeleri bulunan `AddtranslationsRequest`:

* `AppId`: Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `AppId` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.
* `From`: Gerekli. Kaynak dili dil kodu içeren bir dize. Tarafından döndürülen dillerden biri olmalıdır `GetLanguagesForTranslate` yöntemi.
* `To`: Gerekli. Hedef Dil dil kodu içeren bir dize. Tarafından döndürülen dillerden biri olmalıdır `GetLanguagesForTranslate` yöntemi.
* `Translations`: Gerekli. Çevirileri için çeviri bellek eklemek için bir dizi. Her çeviri içermelidir `OriginalText`, `TranslatedText`, ve `Rating`. Her en büyük boyutunu `OriginalText` ve `TranslatedText` 1.000 karakterdir. Tüm toplam `OriginalText` ve `TranslatedText` öğeleri 10.000 karakterden uzun olamaz. Dizi öğelerinin sayısı 100'dür.
* `Options`: Gerekli. Bir dizi seçenek dahil olmak üzere, `Category`, `ContentType`, `Uri`, ve `User`. `User` gerekli değildir. `Category`, `ContentType`, ve `Uri` isteğe bağlıdır. Belirtilen öğelerin alfabetik olarak listelenmiş olmalıdır.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
`AddTranslationArray` Yöntem başarılı oldu. 

31 Ocak 2018'den sonra cümle gönderimler kabul edilmez. Hizmet 410 hata kodu ile yanıt verecektir.

dize

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Authorization|(boş)|Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)|Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|410    |`AddTranslation` artık desteklenmiyor.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="get-breaksentences"></a>/BreakSentences Al

### <a name="implementation-notes"></a>Uygulama Notları
Metnin bir bölümünü cümleler böler ve her cümle uzunlukları içeren bir dize döndürür.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Dönüş değeri:** Cümleleri uzunluklarının temsil eden bir tamsayı dizisi. Dizinin uzunluğunu cümleler sayısını temsil eder. Değerleri her cümle uzunluğunu temsil eder.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
Cümleleri uzunluklarının temsil eden bir tamsayı dizisi. Dizinin uzunluğunu cümleler sayısını temsil eder. Değerleri her cümle uzunluğunu temsil eder.

integer

Yanıt içerik türü: application/xml

### <a name="parameters"></a>Parametreler

|Parametre|Değer|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid|(boş)  |Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query| dize|
|text|(boş)   |Gerekli. Cümleleri bölmek için metin temsil eden bir dize. Metin en büyük boyutunu 10.000 karakterdir.|query|dize|
|dil   |(boş)    |Gerekli. Giriş metni dil kodunu temsil eden bir dize.|query|dize|
|Authorization|(boş)|Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.   |üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)|Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400|Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401|Geçersiz kimlik bilgileri.|
|500|Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Uygulama Notları
Çevirileri belirtilen dil çifti için bir dizi, deponun ve MT altyapısı alır. `GetTranslations` farklıdır `Translate` içeren tüm kullanılabilir çevirileri döndürür.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

İsteğe bağlı isteğin gövdesini içeren `TranslationOptions` bu biçime sahip nesnesi:

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

`TranslateOptions` Nesne, aşağıdaki listedeki değerleri içerir. Bunlar tüm isteğe bağlı ve varsayılan için en yaygın ayarlar. Belirtilen öğelerin alfabetik olarak listelenmiş olmalıdır.

* `Category`: Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan, `general` değeridir.
* `ContentType`: Desteklenen tek seçenek ve varsayılan `text/plain`.
* `IncludeMultipleMTAlternatives`: Birden fazla alternatif MT altyapısından döndürülüp döndürülmeyeceğini belirtmek için bir Boole bayrağı. Geçerli değerler `true` ve `false` (büyük-küçük harfe duyarlı). Varsayılan `false`, yalnızca bir alternatif döndürür. Bayrağını ayarlamak `true` işbirliğine dayalı Translation Framework (CTF) ile tamamen tümleşik yapay alternatifleri oluşturulmasını sağlar. Özelliği, gelen yapay alternatifleri ekleyerek CTF içinde hiçbir çevirileri sahip cümleleri döndüren alternatifler sağlar *n*-en iyi kod çözücü listesi.
    - Derecelendirmeleri. Derecelendirmeleri şu şekilde uygulanır: 
         - En iyi bir otomatik çeviri, 5 derecesi vardır.
       - Gözden Geçiren yetkilisi CTF gelen alternatifleri yansıtır. Bunlar için -10 + 10 ' aralığı.
       - Otomatik olarak oluşturulan (*n*-en iyi) 0 ile 100 eşleşme derecesi bir derecelendirme çeviri seçeneğiniz vardır.
    - Alternatifleri sayısı. Döndürülen alternatifleri sayısı belirtilen değeri olarak yüksek `maxTranslations`, ancak daha düşük olabilir.
    - Dil çiftleri. Bu işlevsellik, herhangi bir yönde Basitleştirilmiş Çince ve Geleneksel Çince arasında çevirileri için kullanılamaz. Microsoft Translator tarafından desteklenen diğer tüm dil çiftleri için kullanılabilir.
* `State`: İstek ve yanıt ilişkilendirmenize yardımcı olması için kullanıcı durumu. Yanıtta aynı içerik döndürülür.
* `Uri`: Bu URI sonuçları filtreleyin. Değer ayarlanmışsa varsayılan değer `all`.
* `User`: Bu kullanıcı tarafından sonuçları filtreleyin. Değer ayarlanmışsa varsayılan değer `all`.

İstek `Content-Type` olmalıdır `text/xml`.

**Dönüş değeri:** Yanıt biçimi şu şekildedir:

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

Bu yanıt içeren bir `GetTranslationsResponse` aşağıdaki değerleri içeren öğe:

* `Translations`: Bir dizi eşleşme bulundu, depolanan `TranslationMatch` nesneleri (aşağıdaki bölümde açıklanmıştır). Çevirileri hafif varyantlarının (benzer öğe eşleştirmesi) özgün metin içerebilir. Çevirileri sıralanır: % 100 belirsiz eşleşme sonraki ilk olarak eşleşir.
* `From`: Yöntem belirtmiyorsa bir `From` dil, bu değeri gelen otomatik dil algılamanın dışında. Aksi takdirde, belirtilen olur `From` dili.
* `State`: İstek ve yanıt ilişkilendirmenize yardımcı olması için kullanıcı durumu. Sağlanan değer içeren `TranslateOptions` parametresi.

`TranslationMatch` Nesnesi, bu değerlerden oluşur:

* `Error`: Belirli bir giriş dizesini bir hata oluşursa hata kodu. Aksi takdirde, bu alan boştur.
* `MatchDegree`: ne kadar yakın giriş metni depoda orijinal metinle eşleşen gösterir. Sistem filtresinin eşleşmeleri de dahil olmak üzere deposu ile karşılaştırarak giriş cümleler eşleşir. Değer aralıkları burada hiçbir benzerlik 0 ise ve 100 tam, büyük küçük harfe duyarlı bir eşleşme 100, 0'dan döndürdü.
* `MatchedOriginalText`: Bu sonuç için eşleştirildi orijinal metni. Yalnızca eşleşen orijinal metni giriş metninden farklı olarak bu değer döndürülür. Benzer eşleştirme kaynak metnin döndürmek için kullanılır. Microsoft Translator sonuçları için bu değer döndürülmez.
* `Rating`: Kalite kararı kişinin yetkilisi gösterir. Makine çevirisi, 5 derecesi sonuçlara. Anonim olarak sağlanan Çeviriler, genellikle 1 ile 4 arasında bir derecelendirme sahiptir. Yetkili olarak sağlanan Çeviriler, genellikle 6 ile 10 arasında bir derecelendirme sahip olur.
* `Count`: Bu çeviri Bu derecelendirme seçili sayısı. Otomatik olarak çevrilmiş yanıtı için 0 değerdir.
* `TranslatedText`: Çevrilmiş metin.

### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)
A `GetTranslationsResponse` daha önce açıklanan biçimde bir nesne.

dize

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|appid|(boş)|Gerekli. Varsa `Authorization` veya `Ocp-Apim-Subscription-Key` üstbilgi kullanılır, değiştirmeyin `appid` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.|query|dize|
|text|(boş)|Gerekli. Çevrilecek metin temsil eden bir dize. Metin en büyük boyutunu 10.000 karakterdir.|query|dize|
|from|(boş)|Gerekli. Çevrildikten metnin dil kodu temsil eden bir dize.|query|dize|
|- |(boş)    |Gerekli. Metne çevrilecek dilinin dil kodunu temsil eden bir dize.|query|dize|
|maxTranslations|(boş)|Gerekli. Çevirileri döndürülecek en fazla sayısını temsil eden bir tamsayı.|query|integer|
|Authorization| (boş)|Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır. Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|dize|  üst bilgi|
|Ocp-Apim-Subscription-Key|(boş)  |Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503|Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="post-gettranslationsarray"></a>/GetTranslationsArray gönderin

### <a name="implementation-notes"></a>Uygulama Notları
Birden çok kaynak metinler için birden fazla çeviri adayları alır.

İstek URİ'si `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

İstek gövdesi biçimi şu şekildedir:

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

`GetTranslationsArrayRequest` Bu öğeleri içerir:

* `AppId`: Gerekli. Varsa `Authorization` üstbilgi kullanılır, değiştirmeyin `AppId` boş. Aksi takdirde, içeren bir dize içeren `"Bearer" + " " + "access_token"`.
* `From`: Gerekli. Çevrildikten metnin dil kodu temsil eden bir dize.
* `MaxTranslations`: Gerekli. Çevirileri döndürülecek en fazla sayısını temsil eden bir tamsayı.
* `Options`: İsteğe bağlı. Bir `Options` aşağıdaki değerleri içeren nesne. Bunlar tüm isteğe bağlı ve varsayılan için en yaygın ayarlar. Belirtilen öğelerin alfabetik olarak listelenmiş olmalıdır.
    - `Category`: Çeviri kategorisi (etki alanı) içeren bir dize. Varsayılan, `general` değeridir.
    - `ContentType`: Desteklenen tek seçenek ve varsayılan `text/plain`.
    - `IncludeMultipleMTAlternatives`: Birden fazla alternatif MT altyapısından döndürülüp döndürülmeyeceğini belirtmek için bir Boole bayrağı. Geçerli değerler `true` ve `false` (büyük-küçük harfe duyarlı). Varsayılan `false`, yalnızca bir alternatif döndürür. Bayrağını ayarlamak `true` çeviri, işbirliğine dayalı çevirileri Framework (CTF) ile tamamen tümleşik yapay alternatiflerini oluşturulmasını sağlar. Özelliği, gelen yapay alternatifleri ekleyerek CTF içinde hiçbir alternatifleri sahip cümleleri döndüren alternatifler sağlar *n*-en iyi kod çözücü listesi.
        - Derecelendirmeleri derecelendirmeleri şu şekilde uygulanır:
          - En iyi bir otomatik çeviri, 5 derecesi vardır.
          - Gözden Geçiren yetkilisi CTF gelen alternatifleri yansıtır. Bunlar için -10 + 10 ' aralığı.
          - Otomatik olarak oluşturulan (*n*-en iyi) 0 ile 100 eşleşme derecesi bir derecelendirme çeviri seçeneğiniz vardır.
        - Alternatifleri sayısı. Döndürülen alternatifleri sayısı belirtilen değeri olarak yüksek `maxTranslations`, ancak daha düşük olabilir.
        - Dil çiftleri. Bu işlevsellik, herhangi bir yönde Basitleştirilmiş Çince ve Geleneksel Çince arasında çevirileri için kullanılamaz. Microsoft Translator tarafından desteklenen diğer tüm dil çiftleri için kullanılabilir.
* `State`: İstek ve yanıt ilişkilendirmenize yardımcı olması için kullanıcı durumu. Yanıtta aynı içerik döndürülür.
* `Uri`: Bu URI sonuçları filtreleyin. Değer ayarlanmışsa varsayılan değer `all`.
* `User`: Bu kullanıcı tarafından sonuçları filtreleyin. Değer ayarlanmışsa varsayılan değer `all`.
* `Texts`: Gerekli. Metin çevirisi içeren bir dizi. Tüm dizeleri aynı dilde olması gerekir. Çevrilecek tüm metin toplam 10.000 karakterden uzun olamaz. Dizi öğelerinin sayısı 10'dur.
* `To`: Gerekli. Metne çevrilecek dilinin dil kodunu temsil eden bir dize.

İsteğe bağlı öğeleri atlayabilirsiniz. Doğrudan alt öğeleri `GetTranslationsArrayRequest` alfabetik olarak listelenmiş olmalıdır.

İstek `Content-Type` olmalıdır `text/xml`.

**Dönüş değeri:** Yanıt biçimi şu şekildedir:

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

Her `GetTranslationsResponse` öğesi bu değerleri içerir:

* `Translations`: Bir dizi eşleşme bulundu, depolanan `TranslationMatch` nesneleri (aşağıdaki bölümde açıklanmıştır). Çevirileri hafif varyantlarının (benzer öğe eşleştirmesi) özgün metin içerebilir. Çevirileri sıralanır: % 100 belirsiz eşleşme sonraki ilk olarak eşleşir.
* `From`: Yöntem belirtmiyorsa bir `From` dil, bu değeri gelen otomatik dil algılamanın dışında. Aksi takdirde, belirtilen olur `From` dili.
* `State`: İstek ve yanıt ilişkilendirmenize yardımcı olması için kullanıcı durumu. Sağlanan değer içeren `TranslateOptions` parametresi.

`TranslationMatch` Nesne aşağıdaki değerleri içerir:
* `Error`: Belirli bir giriş dizesini bir hata oluşursa hata kodu. Aksi takdirde, bu alan boştur.
* `MatchDegree`: ne kadar yakın giriş metni depoda orijinal metinle eşleşen gösterir. Sistem filtresinin eşleşmeleri de dahil olmak üzere deposu ile karşılaştırarak giriş cümleler eşleşir. Değer aralıkları burada hiçbir benzerlik 0 ise ve 100 tam, büyük küçük harfe duyarlı bir eşleşme 100, 0'dan döndürdü.
* `MatchedOriginalText`: Bu sonuç için eşleştirildi orijinal metni. Yalnızca eşleşen orijinal metni giriş metninden farklı olarak bu değer döndürülür. Benzer eşleştirme kaynak metnin döndürmek için kullanılır. Microsoft Translator sonuçları için bu değer döndürülmez.
* `Rating`: Kalite kararı kişinin yetkilisi gösterir. Makine çevirisi, 5 derecesi sonuçlara. Anonim olarak sağlanan Çeviriler, genellikle 1 ile 4 arasında bir derecelendirme sahiptir. Yetkili olarak sağlanan Çeviriler, genellikle bir derecelendirme 6-10 arasındaki sahiptir.
* `Count`: Bu çeviri Bu derecelendirme seçili sayısı. Otomatik olarak çevrilmiş yanıtı için 0 değerdir.
* `TranslatedText`: Çevrilmiş metin.


### <a name="response-class-status-200"></a>Yanıt sınıfı (durum 200)

dize

Yanıt içerik türü: application/xml
 
### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri türü|
|:--|:--|:--|:--|:--|
|Authorization  |(boş)    |Her iki gerekli `appid` alan ve `Ocp-Apim-Subscription-Key` üstbilgisi boş bırakılır.  Yetkilendirme belirteci: `"Bearer" + " " + "access_token"`.|üst bilgi|dize|
|Ocp-Apim-Subscription-Key|(boş)  |Her iki gerekli `appid` alan ve `Authorization` üstbilgisi boş bırakılır.|üst bilgi|dize|

### <a name="response-messages"></a>Yanıt iletilerini

|HTTP durum kodu|Reason|
|:--|:--|
|400    |Hatalı istek. Giriş parametreleri ve ayrıntılı hata yanıtı kontrol edin.|
|401    |Geçersiz kimlik bilgileri.|
|500    |Sunucu hatası. Sorun devam ederse, bize bildirin. Lütfen bize yaklaşık tarih ve saat istek ve yanıt üst bilgisinde yer istek kimliği sağlamak `X-MS-Trans-Info`.|
|503    |Hizmet geçici olarak kullanılamıyor. Lütfen yeniden deneyin ve hata devam ederse bize bildirin.|

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Translator metin API'si geçirme v3](../migrate-to-v3.md)


