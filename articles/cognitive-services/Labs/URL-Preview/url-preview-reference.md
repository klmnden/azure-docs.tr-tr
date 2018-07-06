---
title: Proje URL'si Önizleme başvurusu - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Proje URL'si Önizleme uç noktası için başvuru.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 46c011d62b6ae51f5f7d292345e6ece0e27a8541
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865884"
---
# <a name="project-url-preview-v7-reference"></a>Proje URL'si Önizleme v7 başvurusu

URL önizlemesi Web kaynaklarını kısa açıklamaları blog gönderileri, forum tartışmalarına, Önizleme sayfalarını, vb. için destekler.  URL, Internet kaynağının herhangi bir türde olabilir: Web sayfası, haber, resim veya video. Sorgu, bir http veya https şemasına sahip bir mutlak URL olmalıdır; Göreli URL'ler veya diğer düzenleri gibi ftp: / / desteklenmez.

URL önizlemesi kullanan uygulamalar, sorgu parametresi önizlemek için bir URL uç noktasına Web istekleri göndermek.  İstek içermelidir *Ocp-Apim-Subscription-Key* başlığı.   

JSON yanıtı Önizleme bilgilerini ayrıştırılabilir: ad, bir kaynağın açıklamasını *isFamilyFriendly*ve temsili bir görüntü ve tam kaynak çevrimiçi erişim sağlayan bağlantılar.

URL önizlemesi yalnızca verilerden Önizleme parçacıkları ve küçük resim görüntüleriyle köprülü sosyal medyada sohbet Robotu veya benzer teklifleri paylaşımı son kullanıcı tarafından başlatılan URL'deki kaynak sitelerini görüntülemek için kullanmanız gerekir. Değil kopyalayın, saklamak veya gerekir proje URL'si Önizlemesi'nden aldığınız herhangi bir veri önbelleği. Web sitesi ya da içerik sahipleri alabileceğiniz önizlemelerini devre dışı bırakmak için tüm istekleri sahip olmanız gerekir.

## <a name="endpoint"></a>Uç Nokta
URL önizlemesi sonuçlarının istemek için aşağıdaki uç noktaya bir istek gönderin. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

GET uç noktası: 
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=queryURL

````

İstek, HTTPS protokolünü kullanmak ve ardından sorgu parametresi içerir:

 q - Önizleme URL'si tanımlayan sorgu 

Aşağıdaki bölümler yanıt nesneleri, sorgu parametreleri ve arama sonuçlarını etkileyecek üstbilgileri hakkında teknik ayrıntılar sağlar. 
  
İstekleri içermelidir üstbilgileri hakkında daha fazla bilgi için bkz: [üstbilgileri](#headers).  
  
İstekleri içermelidir sorgu parametreleri hakkında daha fazla bilgi için bkz: [sorgu parametreleri](#query-parameters).  
  
JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).

Maksimum sorgu URL'SİNİN uzunluğu 2.048 karakterdir. URL uzunluğu sınırı aşmadığından emin olmak için sorgu parametrelerinizin uzunluğu en fazla 1500'den az karakter olmalıdır. URL 2.048 karakterden uzunsa sunucu 404 bulunamadı hatası döndürür.  

İzin verilen kullanım ve sonuçları görüntüleme hakkında daha fazla bilgi için bkz. [kullanın ve gereksinimlerini görüntülemek](use-display-requirements.md). 

> [!NOTE]
> URL önizlemesi, diğer arama API'lerini için anlamlı olan bazı istek üst etkilemez
> - Pragma – URL önizlemesi önbellek kullanıp kullanmadığını denetim çağırana sahip değil
> - Kullanıcı Aracısı – şu an için Önizleme API URL'si farklı yanıtlar PC, dizüstü bilgisayar veya mobil yayılan çağrıları için sağlamaz.

> Ayrıca, bazı parametreler için URL önizleme API'sı şu anda anlamlı değildir, ancak gelecekte geliştirilmiş Genelleştirme için kullanılabilir. 
 
## <a name="headers"></a>Üst bilgiler  
İstek ve yanıt içerebilecek üst bilgiler verilmiştir.  
  
|Üst bilgi|Açıklama|  
|------------|-----------------|   
|<a name="market" />BingAPIs-Pazar|Yanıtı üstbilgisi.<br /><br /> İstek tarafından kullanılan Pazar. Form \<languageCode\>-\<countryCode\>. Örneğin, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Yanıtı üstbilgisi.<br /><br /> İsteğinin ayrıntılarını içeren günlük girdisi kimliği. Bir hata oluştuğunda, bu kimliği yakalama Belirlemek ve sorunu çözmek mümkün değilse, bu kimliği yanı sıra destek ekibinin sağladığı diğer bilgiler içerir.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Gerekli isteği üstbilgisi.<br /><br /> Bu hizmet için RMS'ye kaydolurken aldığınız abonelik anahtarını [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/).|  
|<a name="clientid" />X MSEdge ClientID|İsteğe bağlı istek ve yanıt üst bilgisi.<br /><br /> Bing Bing API çağrıları arasında tutarlı bir davranış ile kullanıcılara sağlamak için bu üstbilgiyi kullanır. Bing sıklıkla yeni özellikler ve geliştirmeler uçuşlar ve istemci kimliği farklı uçuşlar trafiğinde atamak için bir anahtar olarak kullanır. Birden çok istekler genelinde bir kullanıcı için aynı istemci Kimliğini kullanmazsanız Bing için birden çok çakışan uçuşlar kullanıcı atayabilir. Birden çok çakışan uçuşlar için atanan bir tutarsız kullanıcı deneyimine neden olabilir. Örneğin, bir farklı uçuş ataması ilk değerinden ikinci isteği varsa deneyimi beklenmeyen olabilir. Bing web sonuçları, istemci için uygun hale getirmek için Ayrıca, istemci Kimliğini kullanabilirsiniz kimliğin arama geçmişi, kullanıcı için daha zengin bir deneyim sağlar.<br /><br /> Bing sonucu sonuçlarımızda bir istemci kimliği tarafından oluşturulan bir etkinlik analiz ederek geliştirmeye yardımcı olmak için bu üst bilgi de kullanır. İlgi geliştirmeleri, Bing API'leri ve sırayla tıklama etkinleştirir daha yüksek ücretler için API tüketici teslim sonuçlarının daha iyi bir nitelikle yardımcı olur.<br /><br />Bu üst bilgiye Uygula temel kullanım kuralları aşağıda verilmiştir.<br /><ul><li>Cihazda uygulamanızın kullandığı her bir kullanıcı bir benzersiz olmalıdır, Bing oluşturulan istemci kimliği.<br /><br/>İstekte bu başlığı eklemezseniz, Bing bir kimlik üretir ve X MSEdge ClientID yanıt üst bilgisinde döndürür. Bir istekte bu üst bilgi içermemelidir yalnızca bir kez ilk kez kullanıcı o cihazda uygulamanızın kullandığı ' dir.<br /><br/></li><li>İstemci Kimliğini, cihazda bu kullanıcı için uygulamanıza yaptığı her Bing API isteği için kullanın.<br /><br/></li><li>**DİKKAT:** bu istemci kimliği için herhangi bir authenticatable kullanıcı hesabı bilgisi değişkenlerinden değil emin olmanız gerekir.</li><br/><li>Kalıcı istemci kimliği. Bir tarayıcı uygulamasında kimliği kalıcı hale getirmek için kalıcı bir HTTP tanımlama bilgisi kimliği tüm oturumlarda kullanıldığından emin olmak için kullanın. Oturum tanımlama bilgisinin kullanmayın. Mobil uygulamalar gibi diğer uygulamalar için cihazın kalıcı depolama kimliği kalıcı hale getirmek için kullanın.<br /><br/>Kullanıcı, cihazda uygulamanızın kullandığı bir sonraki açışınızda, kalıcı bir istemci kimliği alın.</li></ul><br /> **Not:** Bing yanıtlarını olabilir veya bu başlığı içermeyebilir. Bu üst bilgi yanıtı içeriyorsa, istemci kimliği yakalamak ve o cihazdaki kullanıcı için tüm sonraki Bing istekler için kullanın.<br /><br /> **Not:** X MSEdge ClientID eklerseniz, tanımlama bilgilerinin istekte içermemesi gerekir.|  
|<a name="clientip" />X MSEdge Clientıp|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemci cihazı IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgileri güvenli arama davranışını belirlemek için kullanır.<br /><br />  Adres (örneğin, son sekizli 0 olarak değiştirerek) karartmak değil. Her yerden gerçek cihazın konumuna olmaması konumu adresi sonuçlarında obfuscating, hangi hatalı sonuçlar sunan Bing içinde neden olabilir.|  
<br /><br /></li></ul>   

## <a name="query-parameters"></a>Sorgu parametreleri  
Aşağıdaki sorgu parametreleri istek içerebilir. Gerekli Parametreler için gerekli sütununa bakın. URL gereken sorgu parametrelerine kodlayın. Sorgu, bir http veya https şemasına sahip bir mutlak URL olmalıdır; Göreli URL'ler veya ftp gibi diğer düzenleri desteklemiyoruz: / /
  
  
|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />Mkt|Sonuçları nereden geldiğini Pazar. <br /><br />Olası Pazar değerler listesi için bkz. [Pazar kodları](#market-codes).<br /><br /> **Not:** URL önizleme API'sı şu anda yalnızca ABD coğrafya ve İngilizce dilini desteklemektedir.<br /><br />|Dize|Evet|  
|<a name="query" />q|Önizleme URL'si|Dize|Evet|  
|<a name="responseformat" />responseFormat|Yanıt için kullanılacak medya türü. Büyük küçük harf duyarsız olası değerler şunlardır:<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> JSON varsayılandır. JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).<br /><br />  JsonLd belirtirseniz, yanıt gövdesi, arama sonuçlarını içeren JSON-LD nesneler içerir. JSON-LD hakkında daha fazla bilgi için bkz. [JSON-LD](http://json-ld.org/).|Dize|Hayır|
|<a name="safesearch"/>safeSearch|Geçersiz yetişkinlere yönelik içeriği veya korsan içeriği, 400 ' hata koduyla engellendi ve *isFamilyFriendly* bayrağı alınmadı. <p>Yetişkinlere yönelik içeriği için yasal, aşağıda davranıştır. Durum kodu 200 döndürür ve *isFamilyFriendly* bayrağı false olarak ayarlanır.<ul><li>safeSearch strict =: Başlık, açıklama, URL ve resmi olmayan döndürülür.</li><li>safeSearch Orta; = Başlık, URL ve açıklama ancak açıklayıcı görüntü alın.</li><li>safeSearch; = Tüm yanıt nesneleri/öğeleri – başlık, URL, açıklama ve resim alın.</li></ul> |Dize|Gerekli değildir. </br> Varsayılan olarak safeSearch için strict =.| 

## <a name="response-objects"></a>Yanıt nesneleri  
Yanıt şeması ya da bir [Web] sayfasıdır veya ErrorResponse, Web araması API'si olduğu gibi. İstek başarısız olursa, en üst düzey nesnedir [ErrorResponse](#errorresponse) nesne.


|Nesne|Açıklama|  
|------------|-----------------|  
|[Web sayfası](#webpage)|Önizleme özniteliklerini içeren üst düzey JSON nesnesi.|  

  
### <a name="error"></a>Hata  
Gerçekleşen hata tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|<a name="error-code" />Kod|Hata kategorisi tanımlar hata kodu. Olası kodlarının listesi için bkz. [hata kodları](#error-codes).|Dize|  
|<a name="error-message" />İleti|Hatanın açıklaması.|Dize|  
|<a name="error-moredetails" />moreDetails|Hata hakkında ek bilgi sağlayan bir açıklama.|Dize|  
|<a name="error-parameter" />Parametre|Sorgu parametresi hataya neden olan istek.|Dize|  
|<a name="error-subcode" />Alt|Hatayı tanımlar hata kodu. Örneğin, varsa `code` InvalidRequest, olan `subCode` ParameterInvalid veya ParameterInvalidValue olabilir. |Dize|  
|<a name="error-value" />Değer|Geçerli değildi sorgu parametrenin değeri.|Dize|  
  

### <a name="errorresponse"></a>ErrorResponse  
Başarısız istek olduğunda yanıt içeren üst düzey nesnesi.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|Tür ipucu.|Dize|  
|<a name="errors" />hataları|İsteğin neden başarısız olma nedenlerini tanımlayan hataların listesi.|[Hata](#error)]|   
  

### <a name="webpage"></a>Web sayfası  
Hakkında bilgilerini tanımlayan bir önizleme Web sayfası.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|
|ad|Sayfa başlığı, mutlaka HTML Başlığı|Dize|
|url|Aslında gezinilen URL'si (istek ve ardından yeniden yönlendirmeleri)|Dize|  
|açıklama|Sayfa içeriği ve kısa açıklama|Dize|  
|isFamilyFriendly|Web dizindeki öğeler için en doğru; Bu algılama yöntemi yalnızca bir URL ve sayfa içeriği göre gerçek zamanlı öğesinden yapın|boole|
|primaryImageOfPage/contentUrl|Önizlemede dahil etmek için temsili bir görüntü URL'si|Dize| 


### <a name="identifiable"></a>Tanımlama
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|id|Bir kaynak tanımlayıcısı|Dize|
 

## <a name="error-codes"></a>Hata kodları

Bir isteği döndüren olası HTTP durum kodları şunlardır:  
  
|Durum Kodu|Açıklama|  
|-----------------|-----------------|  
|200|Başarılı.|  
|400|Sorgu parametrelerden biri eksik veya geçerli değil.| 
|400|ServerError, ResourceError alt kod: İstenen URL erişilemedi|
|400|ServerError, ResourceError alt kod: İstenen URL (HTTP 404 döndürdüyse dahil) bir başarı kodu döndürmedi|
|400|InvalidRequest, engellenen alt kod: İstenen URL yetişkinlere yönelik içerik içerebilir ve engellendi| 
|401|Abonelik anahtarı eksik veya geçerli değil.|  
|403|Kullanıcının kimliği doğrulanır (örneğin, bunlar bir geçerli abonelik anahtarı kullanılır), ancak istenen kaynak için izniniz yok.<br /><br /> Çağıranın sorguları başına aylık kota aşılırsa Bing bu durumu döndürebilir.|  
|410|İstek HTTP yerine HTTPS protokolü kullanılır. Yalnızca desteklenen protokol https'dir.|  
|429|Çağıran ikinci kota başına sorguları aştı.|  
|500|Beklenmeyen sunucu hatası.|

İstek başarısız olursa, yanıtını içeren bir [ErrorResponse](#errorresponse) listesini içeren bir nesne [hata](#error) hata nedenini açıklayan bir nesne. Hata parametresi, ilgili olup olmadığını `parameter` alan sorun parametresi tanımlar. Ve hata için bir parametre değeri, ilişkili olup olmadığını `value` alan, geçerli olmayan bir değer tanımlar.

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

Olası hata kodu ve alt hata kodu değerleri şunlardır.

|Kod|Alt|Açıklama
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Uygulanmadı|HTTP durum kodunu 500'dür.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Engellendi|Her isteğin herhangi bir bölümü geçerli değil Bing InvalidRequest döndürür. Örneğin, bir gerekli parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterInvalidValue ise, HTTP durum kodu 400 ' dir.<br/><br/>HTTPS yerine HTTP protokolünü kullanıyorsanız, Bing HttpNotAllowed döndürür ve 410 HTTP durum kodudur.
|RateLimitExceeded|Hiçbir alt kodları|/ Saniye (QPS) sorguları veya sorgu başına aylık (QPM) kota aştığında Bing RateLimitExceeded döndürür.<br/><br/>QPS aşarsanız, HTTP durum kodu 429 Bing döndürür ve QPM aşarsanız, Bing 403 döndürür.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing çağıran doğrulandığında Bing InvalidAuthorization döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgisi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtmek, yedeklilik meydana gelir.<br/><br/>Hata InvalidAuthorization ise, HTTP durum kodunu 401 ' dir.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Çağıran kaynağa erişmek için izinlere sahip olmadığı durumlarda Bing InsufficientAuthorization döndürür. Bu abonelik anahtarını devre dışı bırakıldı veya süresi ortaya çıkabilir. <br/><br/>Hata InsufficientAuthorization, HTTP durum kodu 403 ise.

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](csharp.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [JavaScript hızlı başlangıç](javascript.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)

