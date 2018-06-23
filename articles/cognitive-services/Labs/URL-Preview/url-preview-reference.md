---
title: Proje URL'si Önizleme başvurusu - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Proje URL'si Önizleme uç noktasını referansı.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: adc2f83f703e740e40d9ba4fd3ed08ba429e5d97
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354179"
---
# <a name="project-url-preview-v7-reference"></a>Proje URL'si Önizleme v7 başvurusu

URL önizleme Web kaynakların kısa açıklamaları blog gönderileri, forum tartışmalara, Önizleme sayfaları, vb. için destekler.  URL, Internet kaynağının herhangi bir türde olabilir: Web sayfası, haber, görüntü veya video. Sorgu, bir http veya https şeması ile mutlak bir URL olmalıdır; göreli URL veya ftp gibi diğer düzenleri: / / desteklenmez.

URL önizleme kullanan uygulamalar bir sorgu parametresi Önizleme için bir URL ile uç nokta Web istekleri gönderir.  İstek içermelidir *Apim abonelik anahtar Ocp* üstbilgi.   

JSON yanıt Önizleme bilgilerini ayrıştırılabilir: ad, açıklama kaynak *isFamilyFriendly*ve temsili bir görüntü ve çevrimiçi tam kaynağa erişim sağlamak bağlantılar.

Önizleme parçacıkları ve resimlerin köprülü sosyal medya üzerinde sohbet bot veya benzer teklifleri paylaşımı son kullanıcı tarafından başlatılan URL'de kaynak sitelerini görüntülemek için yalnızca URL önizleme verileri kullanmanız gerekir. Değil kopyalamanız, saklamak veya gerekir proje URL'si Önizlemesi'nden aldığınız herhangi bir veriyi önbelleğe. Web sitesi ya da içerik sahipleri alabilirsiniz önizlemeleri devre dışı bırakmak için tüm istekleri kabul gerekir.

## <a name="endpoint"></a>Uç Nokta
URL önizleme sonuçlarını istemek için aşağıdaki uç noktası için bir istek gönderin. Üstbilgiler ve URL parametreleri daha ayrıntılı belirtimleri tanımlamak için kullanın.

GET uç noktası: 
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=queryURL

````

İstek, HTTPS protokolünü kullanmak ve sorgu parametresi aşağıdaki içerir:

 q - Önizleme için URL'yi tanımlayan sorgu 

Aşağıdaki bölümler yanıt nesneleri, sorgu parametrelerini ve arama sonuçlarını etkilemek üstbilgileri hakkında teknik ayrıntılar sağlar. 
  
İstekleri içermelidir üstbilgileri hakkında daha fazla bilgi için bkz: [üstbilgileri](#headers).  
  
İstekleri içermelidir sorgu parametreleri hakkında daha fazla bilgi için bkz: [sorgu parametreleri](#query-parameters).  
  
JSON hakkında bilgi yanıtı içerdiğini nesneleri için bkz: [yanıt nesneleri](#response-objects).

Maksimum sorgu URL uzunluğu 2.048 karakterdir. URL uzunluğu sınırı aşmadığından emin olmak için sorgu parametrelerinizin uzunluğu en fazla 1500'den az karakter olmalıdır. URL 2.048 karakteri aşarsa sunucu 404 bulunamadı döndürür.  

İzin verilen kullanın ve sonuçları görüntüleme hakkında daha fazla bilgi için bkz: [kullanın ve gereksinimlerini görüntülemek](use-display-requirements.md). 

> [!NOTE]
> Diğer arama API'leri anlamlı bazı istek üstbilgileri URL önizleme etkilemez
> - Pragma – çağıran URL önizleme önbellek kullanıp kullanmadığını üzerinde denetime sahip değil
> - User-Agent – şu an için Url Önizleme API farklı yanıtlar PC, dizüstü bilgisayar veya mobil yayılan çağrıları için sağlamaz.

> Ayrıca, bazı parametreler için URL önizleme API şu anda anlamlı değildir, ancak gelecekte geliştirilmiş Genelleştirme için kullanılabilir. 
 
## <a name="headers"></a>Üst bilgiler  
İstek ve yanıt içerebilir üstbilgileri verilmiştir.  
  
|Üst bilgi|Açıklama|  
|------------|-----------------|   
|<a name="market" />BingAPIs-Pazar|Yanıtı üstbilgisi.<br /><br /> İstek tarafından kullanılan Pazar. Form \<languageCode\>-\<countryCode\>. Örneğin, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Yanıtı üstbilgisi.<br /><br /> İstek ayrıntılarını içeren bir günlük girişi kimliği. Bir hata oluştuğunda, bu kimliği yakalama Belirlemek ve sorunu çözmek mümkün değilse, Destek ekibine sağladığınız bilgilerin yanı sıra bu Kimliğini içerir.|  
|<a name="subscriptionkey" />Ocp Apim abonelik anahtarı|Gerekli isteği üstbilgisi.<br /><br /> Bu hizmet için kaydolan getirdiğinizde aldığınız abonelik anahtarı [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/).|  
|<a name="clientid" />X MSEdge ClientID|İsteğe bağlı istek ve yanıt üstbilgi.<br /><br /> Bing kullanıcılara Bing API çağrıları arasında ile tutarlı davranışı sağlamak için bu üstbilgiyi kullanır. Bing genellikle yeni özellikler ve geliştirmeler uçuşlar ve istemci kimliği farklı uçuşlar trafiğinde atamak için bir anahtar olarak kullanır. Birden çok istekte bir kullanıcı için aynı istemci kimliği kullanmıyorsanız Bing kullanıcı için birden çok çakışan uçuşlar atayabilir. İçin birden çok çakışan uçuşlar atanmasını tutarsız kullanıcı deneyimi için yol açabilir. Örneğin, ikinci istek ilk daha farklı uçuş atama sahipse deneyimi beklenmeyen olabilir. Bing web sonuçlarını o istemciye uyarlamak için Ayrıca, istemci Kimliğini kullanabilirsiniz kimliğin arama geçmişi, kullanıcı için daha zengin bir deneyim sağlar.<br /><br /> Bing bir istemci kimliği tarafından oluşturulan etkinliğini çözümleyerek sonuç derecelendirmeleri geliştirmeye yardımcı olmak için bu üst de kullanır. Bing API'leri ve sırayla tıklatın aracılığıyla etkinleştirir yüksek hızları için API tüketici teslim sonuçlarının daha iyi kalitesiyle Yardım ilgi geliştirmeleri.<br /><br />Bu başlığına uygulanan temel kullanım kuralları aşağıda verilmiştir.<br /><ul><li>Cihazda uygulamanızın kullandığı her bir kullanıcı bir benzersiz olması gerekir, istemci kimliği Bing oluşturulan<br /><br/>Bu üst istekte eklemezseniz Bing kimliği oluşturur ve X MSEdge ClientID yanıt üstbilgisinde döndürür. Bu üst bir istekte içermemelidir yalnızca kullanıcı uygulamanızı bu cihaz üzerinde ilk kullandığında saattir.<br /><br/></li><li>Cihazda bu kullanıcı için uygulamanızı yapar her Bing API'si isteği için istemci Kimliğini kullanın.<br /><br/></li><li>**DİKKAT:** bu istemci kimliği herhangi bir authenticatable kullanıcı hesabı bilgisi linkable değil emin olmalısınız.</li><br/><li>İstemci kimliği Sürdür Bir tarayıcı uygulaması kimliği sürdürmek için tüm oturumlarında kullanılan kimliği emin olmak için kalıcı bir HTTP tanımlama bilgisi kullanın. Bir oturum tanımlama bilgisi kullanmayın. Mobil uygulamalar gibi diğer uygulamalar için cihazın kalıcı depolama kimliği sürdürmek için kullanın.<br /><br/>Kullanıcı bu aygıtta uygulamanızı kullanan sonraki açışınızda, kalıcı istemci kimliği alın.</li></ul><br /> **Not:** Bing yanıtlar olabilir ya da bu başlığı içermeyebilir. Yanıt bu üst bilgisi içeriyorsa, istemci kimliği yakalayın ve bu cihazdaki kullanıcı için tüm sonraki Bing istekler için kullanın.<br /><br /> **Not:** X MSEdge ClientID eklerseniz, tanımlama bilgileri istek içermemesi gerekir.|  
|<a name="clientip" />X MSEdge ClientIP|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemci aygıtı IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgileri güvenli arama davranışını belirlemek için kullanır.<br /><br />  Adres (örneğin, son sekizli 0 olarak değiştirerek) belirsizleştirirseniz değil. Herhangi bir yere gerçek konumuna cihazın olmaması konumu adresi sonuçlarında obfuscating, hangi hatalı sonuçları hizmet veren Bing neden olabilir.|  
<br /><br /></li></ul>   

## <a name="query-parameters"></a>Sorgu parametreleri  
İstek aşağıdaki sorgu parametreleri içerebilir. Gerekli parametre için gerekli sütununa bakın. URL gerekir sorgu parametrelerini kodlayın. Sorgu, bir http veya https şeması ile mutlak bir URL olmalıdır; göreli URL veya ftp gibi diğer düzenleri desteklemiyoruz: / /
  
  
|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />Mkt|Sonuçları alınacağı yeri Pazar. <br /><br />Olası Pazar değerler listesi için bkz: [Pazar kodları](#market-codes).<br /><br /> **Not:** URL önizleme API'sı şu anda yalnızca ABD coğrafya ve İngilizce dili destekler.<br /><br />|Dize|Evet|  
|<a name="query" />q|Önizleme için URL|Dize|Evet|  
|<a name="responseformat" />responseFormat|Yanıt için kullanılacak medya türü. Büyük küçük harf duyarsız olası değerler şunlardır:<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> JSON varsayılandır. JSON hakkında bilgi yanıtı içerdiğini nesneleri için bkz: [yanıt nesneleri](#response-objects).<br /><br />  JsonLd belirtirseniz, yanıt gövdesi, arama sonuçlarını içeren JSON-LD nesneleri içerir. JSON-LD hakkında daha fazla bilgi için bkz: [JSON-LD](http://json-ld.org/).|Dize|Hayır|
|<a name="safesearch"/>güvenli arama|Geçersiz yetişkinlere yönelik içeriğe veya korsan içeriği, 400, hata kodu ile engellenir ve *isFamilyFriendly* bayrağı alınmadı. <p>Davranış yasal yetişkinlere yönelik içeriğe için aşağıdadır. Durum kodu 200, döndürür ve *isFamilyFriendly* bayrağı false olarak ayarlanır.<ul><li>güvenli arama strict =: Başlık, açıklama, URL ve görüntü değil döndürülecek.</li><li>güvenli arama Orta; = Başlık, URL ve açıklama ancak açıklayıcı görüntüsü alın.</li><li>güvenli arama = off; Tüm yanıt nesneleri/öğelerini – başlık, URL, açıklama ve görüntü alma.</li></ul> |Dize|Gerekli değildir. </br> Varsayılan olarak güvenli arama için katı =.| 

## <a name="response-objects"></a>Yanıt nesneleri  
Yanıt şeması ya da bir [Web] olduğu veya Web arama API olduğu gibi ErrorResponse. İstek başarısız olursa, en üst düzey nesnesidir [ErrorResponse](#errorresponse) nesnesi.


|Nesne|Açıklama|  
|------------|-----------------|  
|[Web sayfası](#webpage)|Önizleme özniteliklerini içeren üst düzey JSON nesnesi.|  

  
### <a name="error"></a>Hata  
Gerçekleşen hata tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|<a name="error-code" />Kod|Hata kategorisi tanımlar hata kodu. Olası kodlarının listesi için bkz: [hata kodları](#error-codes).|Dize|  
|<a name="error-message" />İleti|Hatanın açıklaması.|Dize|  
|<a name="error-moredetails" />moreDetails|Hatayla ilgili ek bilgileri sağlayan bir açıklama.|Dize|  
|<a name="error-parameter" />Parametre|Hatanın nedeni isteğinde sorgu parametresi.|Dize|  
|<a name="error-subcode" />Alt kod|Hata tanımlayan hata kodu. Örneğin, varsa `code` InvalidRequest, olan `subCode` ParameterInvalid veya ParameterInvalidValue olabilir. |Dize|  
|<a name="error-value" />Değer|Geçerli değil. sorgu parametresinin değeri.|Dize|  
  

### <a name="errorresponse"></a>ErrorResponse  
İstek başarısız olduğunda yanıtı içeren üst düzey nesne.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_ad|Tür ipucu.|Dize|  
|<a name="errors" />hataları|İsteğin neden başarısız nedenleri açıklanmaktadır hataların listesi.|[Hata](#error)]|   
  

### <a name="webpage"></a>Web sayfası  
Hakkında bilgi tanımlayan bir Web sayfası önizlemede.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|
|ad|Sayfa başlığı, mutlaka HTML Başlığı|Dize|
|url|Gerçekte gezinme URL'si (istek ve ardından yeniden yönlendirmeleri)|Dize|  
|açıklama|Kısa açıklaması sayfa ve içerik|Dize|  
|isFamilyFriendly|Web dizindeki öğeler için en doğru; gerçek zamanlı öğesinden yalnızca URL ve sayfa içeriğini değil göre bu algılama yapın|boole|
|primaryImageOfPage/contentUrl|Önizlemede dahil etmek için temsili bir resim URL'si|Dize| 


### <a name="identifiable"></a>Tanımlama
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|id|Bir kaynak tanımlayıcısı|Dize|
 

## <a name="error-codes"></a>Hata kodları

Bir istek döndürür olası HTTP durum kodları şunlardır:  
  
|Durum Kodu|Açıklama|  
|-----------------|-----------------|  
|200|Başarılı.|  
|400|Sorgu parametrelerden biri eksik veya geçerli değil.| 
|400|ServerError, ResourceError alt kod: İstenen URL ulaşılamıyor|
|400|ServerError, ResourceError alt kod: İstenen URL (HTTP 404 döndürülürse dahil) bir başarı kodu döndürmedi|
|400|InvalidRequest, engellenen alt kod: İstenen URL yetişkinlere yönelik içeriğe içerebilir ve engellendi| 
|401|Abonelik anahtarı eksik veya geçerli değil.|  
|403|Kullanıcının kimliği doğrulanır (örneğin, bunlar bir geçerli abonelik anahtarı kullanılır), ancak istenen kaynak için izniniz yok.<br /><br /> Arayan sorgularını her ay kota aşılırsa Bing bu durum ayrıca döndürebilir.|  
|410|İstek HTTP yerine HTTPS protokolü kullanılır. HTTPS yalnızca desteklenen protokolüdür.|  
|429|Arayan sorgularını başına ikinci kota aşıldı.|  
|500|Beklenmeyen sunucu hatası.|

İstek başarısız olursa, yanıtı içeren bir [ErrorResponse](#errorresponse) listesini içeren bir nesne, [hata](#error) hata nedenini açıklayan nesnelerinin. Hata bir parametre için ilgili olup olmadığını `parameter` alan sorun parametre tanımlar. Ve hata için parametre değeri, ilgili olup olmadığını `value` alanı geçerli olmayan bir değer tanımlar.

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidRequest", 
      "subCode": "ParameterMissing", 
      "message": "Required parameter is missing.", 
      "parameter": "q" 
    }
  ]
}

{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

Olası hata kodu ve alt hata kodu değerleri şunlardır:

|Kod|Alt kod|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Uygulanmadı|HTTP durum kodunu 500'dür.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Engellendi|İstek, herhangi bir bölümünü geçersiz olduğunda Bing InvalidRequest döndürür. Örneğin, gerekli bir parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterInvalidValue ise, HTTP durum kodu 400 ' dir.<br/><br/>HTTPS yerine HTTP protokolünü kullanırsanız, Bing HttpNotAllowed döndürür ve HTTP durum 410 kodudur.
|RateLimitExceeded|Hiçbir alt kodları|Sorgular (QPS) saniyede veya sorguları her ay (QPM) kota aştıklarında Bing RateLimitExceeded döndürür.<br/><br/>QPS aşarsa, HTTP durum kodu 429 Bing döndürür ve QPM aşarsa, Bing 403 döndürür.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing çağıran doğrulayamadığında Bing InvalidAuthorization döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgisi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemini belirtirseniz, artıklık oluşur.<br/><br/>Hata InvalidAuthorization, HTTP durum kodunu 401 ise.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Arayan kaynağa erişim izni olmadığında Bing InsufficientAuthorization döndürür. Abonelik anahtarı devre dışı bırakılmış veya süresi dolmuş bu durum ortaya çıkabilir. <br/><br/>Hata InsufficientAuthorization, HTTP durum kodu 403 ise.

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](csharp.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [JavaScript hızlı başlangıç](javascript.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)

