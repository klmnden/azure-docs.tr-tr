---
title: Proje URL'si Önizleme başvurusu
titlesuffix: Azure Cognitive Services
description: Proje URL'si Önizleme uç noktası için başvuru.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh
ms.openlocfilehash: 69db722295c9c81d45913bd078fe9cc5ab74c512
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60462597"
---
# <a name="project-url-preview-v7-reference"></a>Proje URL'si Önizleme v7 başvurusu

URL önizlemesi Web kaynaklarını kısa açıklamaları blog gönderileri, forum tartışmalarına, Önizleme sayfalarını, vb. için destekler. URL, Internet kaynağının herhangi bir türde olabilir: Web sayfası, haber, resim veya video. Sorgu, bir http veya https şemasına sahip bir mutlak URL olmalıdır; Göreli URL'ler veya diğer düzenleri gibi ftp: / / desteklenmez.

URL önizlemesi kullanan uygulamalar, sorgu parametresi önizlemek için bir URL uç noktasına Web istekleri göndermek. İstek içermelidir *Ocp-Apim-Subscription-Key* başlığı.

JSON yanıtı Önizleme bilgilerini ayrıştırılabilir: ad, bir kaynağın açıklamasını *isFamilyFriendly*ve temsili bir görüntü ve tam kaynak çevrimiçi erişim sağlayan bağlantılar.

URL önizlemesi yalnızca verilerden Önizleme parçacıkları ve küçük resim görüntüleriyle köprülü sosyal medyada sohbet Robotu veya benzer teklifleri paylaşımı son kullanıcı tarafından başlatılan URL'deki kaynak sitelerini görüntülemek için kullanmanız gerekir. Değil kopyalayın, saklamak veya gerekir proje URL'si Önizlemesi'nden aldığınız herhangi bir veri önbelleği. Web sitesi ya da içerik sahipleri alabileceğiniz önizlemelerini devre dışı bırakmak için tüm istekleri sahip olmanız gerekir.

## <a name="endpoint"></a>Uç Nokta
URL önizlemesi sonuçlarının istemek için aşağıdaki uç noktaya bir istek gönderin. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

GET uç noktası:
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=queryURL

```

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
> 
> Ayrıca, bazı parametreler için URL önizleme API'sı şu anda anlamlı değildir, ancak gelecekte geliştirilmiş Genelleştirme için kullanılabilir.

## <a name="headers"></a>Üst bilgiler
İstek ve yanıt içerebilecek üst bilgiler verilmiştir.

|Üst bilgi|Açıklama|
|------------|-----------------|
|<a name="market" />BingAPIs-Market|Yanıt üst bilgisi.<br /><br /> İstek tarafından kullanılan pazar. Biçimi şöyledir: \<languageCode\>-\<countryCode\>. Örneğin, tr-TR.|
|<a name="traceid" />BingAPIs-TraceId|Yanıt üst bilgisi.<br /><br /> İsteğin ayrıntılarını içeren günlük girdisinin kimliği. Hata oluştuğunda, bu kimliği yakalayın. Sorunu belirleyemez ve çözemezseniz, Destek ekibine diğer bilgilerle birlikte bu kimliği de sağlayın.|
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Gerekli istek üst bilgisi.<br /><br /> [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/)'de bu hizmete kaydolduğunuzda aldığınız abonelik anahtarı.|
|<a name="clientid" />X-MSEdge-ClientID|İsteğe bağlı istek ve yanıt üst bilgisi.<br /><br /> Bing, kullanıcılara tüm Bing API çağrılarında tutarlı bir davranış sağlamak için bu üst bilgiyi kullanır. Bing sık sık yeni özellikler ve geliştirmeler dağıtır ve farklı dağıtımlarda trafik ataması yapmak için anahtar olarak istemci kimliğini kullanır. Bir kullanıcı için birden çok istekte aynı istemci kimliğini kullanmazsanız, Bing kullanıcıyı birden çok çakışan dağıtıma atayabilir. Birden çok çakışan dağıtıma eklenmek, tutarsız bir kullanıcı deneyimine yol açabilir. Örneğin, ikinci isteğin dağıtım ataması ilkinden farklıysa, beklenmeyen bir deneyim yaşanabilir. Ayrıca, Bing istemci kimliğini kullanarak web sonuçlarını istemci kimliğinin arama geçmişine uyarlayabilir ve bu sayede kullanıcıya daha zengin bir deneyim sağlayabilir.<br /><br /> Bing, istemci kimliği tarafından oluşturulan etkinliği analiz ederek sonuç derecelendirmelerini geliştirmeye yardımcı olması için de bu üst bilgiyi kullanabilir. İlgi geliştirmeleri Bing API'lerinin daha kaliteli sonuçlar vermesine yardımcı olur ve böylelikle API tüketicisi için daha yüksek tıklama oranları getirir.<br /><br />Bu üst bilgi için geçerli olan temel kullanım kuralları şunlardır:<br /><ul><li>Cihazda uygulamanızı kullanan her kullanıcının Bing tarafından oluşturulan benzersiz bir istemci kimliği olmalıdır.<br /><br/>İsteğe bu üst bilgiyi eklemezseniz, Bing bir kimlik oluşturur ve bu kimliği X-MSEdge-ClientID yanıt üst bilgisinde döndürür. İsteğe bu üst bilgiyi EKLEMEMENİZ gereken tek durum, söz konusu cihazda kullanıcının uygulamanızı ilk kez kullanmasıdır.<br /><br/></li><li>Cihazda uygulamanızın bu kullanıcı için yaptığı her Bing API'si isteğinde istemci kimliğini kullanın.<br /><br/></li><li>**DİKKAT:** Bu istemci kimliği için herhangi bir authenticatable kullanıcı hesabı bilgisi değişkenlerinden değil emin olmanız gerekir.</li><br/><li>İstemci kimliğinin kalıcı olmasını sağlayın. Tarayıcı uygulamasında kimliği kalıcı hale getirmek için, tüm oturumlarda kimliğin kullanmasını sağlayacak bir kalıcı HTTP tanımlama bilgisi kullanın. Oturum tanımlama bilgisi kullanmayın. Mobil uygulamalar gibi diğer uygulamalarda, kimliği kalıcı hale getirmek için cihazın kalıcı depolamasını kullanın.<br /><br/>Kullanıcı o cihazda uygulamanızı yeniden kullandığında, kalıcı hale getirdiğiniz istemci kimliğini alın.</li></ul><br /> **NOT:** Bing yanıtlarını olabilir veya bu başlığı içermeyebilir. Yanıt bu üst bilgiyi içeriyorsa, istemci kimliğini yakalayın ve o cihazda kullanıcı için bunu izleyen tüm Bing isteklerinde onu kullanın.<br /><br /> **NOT:** X MSEdge ClientID eklerseniz, istekte tanımlama bilgisi içermemelidir.|
|<a name="clientip" />X-MSEdge-ClientIP|İsteğe bağlı istek üst bilgisi.<br /><br /> İstemci cihazının IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgisini kullanarak güvenli arama davranışını saptar.<br /><br /> Adresi karartmayın (örneğin, son sekiz karakteri 0'la değiştirerek). Adresin karartılması, cihazın gerçek konumuna yakın olmayan bir konum sonucu verir ve bu da Bing'in hatalı sonuçlar sağlamasına yol açabilir.|

## <a name="query-parameters"></a>Sorgu parametreleri
Aşağıdaki sorgu parametreleri istek içerebilir. Gerekli Parametreler için gerekli sütununa bakın. URL gereken sorgu parametrelerine kodlayın. Sorgu, bir http veya https şemasına sahip bir mutlak URL olmalıdır; Göreli URL'ler veya ftp gibi diğer düzenleri desteklemiyoruz: / /

|Ad|Değer|Tür|Gerekli|
|----------|-----------|----------|--------------|
|<a name="mkt" />mkt|Sonuçların geldiği pazar. <br /><br />Olası Pazar değerler listesi için Pazar kodları bölümüne bakın.<br /><br /> **NOT:** URL önizleme API'sı şu anda yalnızca ABD coğrafya ve İngilizce dilini desteklemektedir.<br /><br />|String|Evet|
|<a name="query" />q|Önizleme URL'si|String|Evet|
|<a name="responseformat" />responseFormat|Yanıt için kullanılacak medya türü. Büyük küçük harf duyarsız olası değerler şunlardır:<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> JSON varsayılandır. JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).<br /><br />JsonLd belirtirseniz, yanıt gövdesi, arama sonuçlarını içeren JSON-LD nesneler içerir. JSON-LD hakkında daha fazla bilgi için bkz. [JSON-LD](https://json-ld.org/).|String|Hayır|
|<a name="safesearch"/>safeSearch|Geçersiz yetişkinlere yönelik içeriği veya korsan içeriği, 400 ' hata koduyla engellendi ve *isFamilyFriendly* bayrağı alınmadı. <p>Yetişkinlere yönelik içeriği için yasal, aşağıda davranıştır. Durum kodu 200 döndürür ve *isFamilyFriendly* bayrağı false olarak ayarlanır.<ul><li>safeSearch strict =: Başlık, açıklama, URL ve görüntü döndürülmez.</li><li>safeSearch Orta; = Başlık, URL ve açıklama ancak açıklayıcı görüntü alın.</li><li>safeSearch; = Tüm yanıt nesneleri/öğeleri – başlık, URL, açıklama ve resim alın.</li></ul> |String|Gerekli değildir. </br> Varsayılan olarak safeSearch için strict =.|

## <a name="response-objects"></a>Yanıt nesneleri
Yanıt şeması ya da bir [Web] sayfasıdır veya ErrorResponse, Web araması API'si olduğu gibi. İstek başarısız olursa, en üst düzey nesnedir [ErrorResponse](#errorresponse) nesne.

|Object|Açıklama|
|------------|-----------------|
|[Web sayfası](#webpage)|Önizleme özniteliklerini içeren üst düzey JSON nesnesi.|

### <a name="error"></a>Hata
Gerçekleşen hata tanımlar.

|Öğe|Açıklama|Tür|
|-------------|-----------------|----------|
|<a name="error-code" />Kod|Hata kategorisi tanımlar hata kodu. Olası kodlarının listesi için bkz. [hata kodları](#error-codes).|String|
|<a name="error-message" />İleti|Hatanın açıklaması.|String|
|<a name="error-moredetails" />moreDetails|Hata hakkında ek bilgi sağlayan bir açıklama.|String|
|<a name="error-parameter" />Parametre|Sorgu parametresi hataya neden olan istek.|String|
|<a name="error-subcode" />Alt|Hatayı tanımlar hata kodu. Örneğin, varsa `code` InvalidRequest, olan `subCode` ParameterInvalid veya ParameterInvalidValue olabilir. |String|
|<a name="error-value" />Değer|Geçerli değildi sorgu parametrenin değeri.|String|

### <a name="errorresponse"></a>ErrorResponse
Başarısız istek olduğunda yanıt içeren üst düzey nesnesi.

|Ad|Değer|Tür|
|----------|-----------|----------|
|_type|Tür ipucu.|String|
|<a name="errors" />Hataları|İsteğin neden başarısız olma nedenlerini tanımlayan hataların listesi.|[Hata](#error)]|

### <a name="webpage"></a>Web sayfası
Hakkında bilgilerini tanımlayan bir önizleme Web sayfası.

|Ad|Değer|Tür|
|----------|-----------|----------|
|ad|Sayfa başlığı, mutlaka HTML Başlığı|String|
|url|Aslında gezinilen URL'si (istek ve ardından yeniden yönlendirmeleri)|String|
|açıklama|Sayfa içeriği ve kısa açıklama|String|
|isFamilyFriendly|Web dizindeki öğeler için en doğru; Bu algılama yöntemi yalnızca bir URL ve sayfa içeriği göre gerçek zamanlı öğesinden yapın|boole|
|primaryImageOfPage/contentUrl|Önizlemede dahil etmek için temsili bir görüntü URL'si|String|

### <a name="identifiable"></a>Tanımlama
|Ad|Değer|Tür|
|-------------|-----------------|----------|
|id|Bir kaynak tanımlayıcısı|String|

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
- [C# hızlı başlangıcı](csharp.md)
- [Java hızlı başlangıcı](java-quickstart.md)
- [JavaScript hızlı başlangıcı](javascript.md)
- [Node hızlı başlangıcı](node-quickstart.md)
- [Python hızlı başlangıcı](python-quickstart.md)
