---
title: Proje yanıt Arama başvurusu - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Proje yanıt arama uç noktası için başvuru.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 8c95fac0c031ec62a9d98d6c3278bd3b3f345140
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354166"
---
# <a name="project-answer-search-v7-reference"></a>Proje yanıt arama v7 başvurusu

Bing yanıt SearchAPI sorgu parametresi alıp döndüren bir `searchResponse` ile `answerType`: `facts` veya `entities`. 

Yanıt arama API kullanan uygulamaları bir sorgu parametresi Önizleme için bir URL ile uç nokta isteği gönderin.  İstek içermelidir `q=searchTerm` parametre ve *Apim abonelik anahtar Ocp* üstbilgi.   

JSON yanıt bulguları ve arama nesnesi hakkında ayrıntılar içeren varlıklar için ayrıştırılabilir.

## <a name="endpoint"></a>Uç Nokta
Yanıt arama sonuçları istemek için aşağıdaki uç noktası için bir istek gönderin. Üstbilgiler ve URL parametreleri daha ayrıntılı belirtimleri tanımlamak için kullanın.

GET uç noktası: 
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

````

İstek, HTTPS protokolünü kullanmak ve sorgu parametresi aşağıdaki içerir:
-  q =<URL> -arama nesneyi tanımlayan sorgu

İstekleri nasıl Göster örnekler için bkz [C# Hızlı Başlangıç](c-sharp-quickstart.md) veya [Java quickstart](java-quickstart.md). 

Aşağıdaki bölümler yanıt nesneleri, sorgu parametrelerini ve arama sonuçlarını etkilemek üstbilgileri hakkında teknik ayrıntılar sağlar. 
  
İstekleri içermelidir üstbilgileri hakkında daha fazla bilgi için bkz: [üstbilgileri](#headers).  
  
İstekleri içermelidir sorgu parametreleri hakkında daha fazla bilgi için bkz: [sorgu parametreleri](#query-parameters).  
  
JSON hakkında bilgi yanıtı içerdiğini nesneleri için bkz: [yanıt nesneleri](#response-objects).

Maksimum sorgu URL uzunluğu 2.048 karakterdir. URL uzunluğu sınırı aşmadığından emin olmak için sorgu parametrelerinizin uzunluğu en fazla 1500'den az karakter olmalıdır. URL 2.048 karakteri aşarsa sunucu 404 bulunamadı döndürür.  

İzin verilen kullanın ve sonuçları görüntüleme hakkında daha fazla bilgi için bkz: [kullanın ve gereksinimlerini görüntülemek](use-display-requirements.md). 

> [!NOTE]
> Diğer arama API'leri anlamlı bazı istek üstbilgileri URL önizleme etkilemez
> - Pragma – çağıran URL önizleme önbellek kullanıp kullanmadığını üzerinde denetime sahip değil
> - Cache-Control-çağıran URL önizleme önbellek kullanıp kullanmadığını üzerinde denetime sahip değil
> - Kullanıcı Aracısı

> Ayrıca, bazı parametreler için URL önizleme API şu anda anlamlı değildir, ancak gelecekte geliştirilmiş Genelleştirme için kullanılabilir. 
 
## <a name="headers"></a>Üst bilgiler  
İstek ve yanıt içerebilir üstbilgileri verilmiştir.  
  
|Üst bilgi|Açıklama|  
|------------|-----------------|  
|Kabul|İsteğe bağlı isteği üstbilgisi.<br /><br /> Varsayılan medya türü application/json şeklindedir. Yanıt kullanmasını belirtmek için [JSON-LD](http://json-ld.org/), Accept üstbilgisi ayarlamak için uygulama/ld + json.|  
|<a name="acceptlanguage" />Kabul dili|İsteğe bağlı isteği üstbilgisi.<br /><br /> Kullanıcı arabirimi dizeleri için kullanılacak dil virgülle ayrılmış listesi. Tercih sırasına göre azalan düzende listesidir. Beklenen biçimle dahil olmak üzere daha fazla bilgi için bkz: [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Bu başlığı ve [setLang](#setlang) sorgu parametresi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.<br /><br /> De belirtmeniz gerekir, bu başlığı ayarlarsanız, [cc](#cc) sorgu parametresi. Sonuçları döndürmek için pazara belirlemek için listeden bulur ve bununla birleştirir ilk desteklenen dil Bing kullanır `cc` parametre değeri. Liste desteklenen bir dil içermiyorsa, Bing en yakın dil ve istek destekleyen Pazar bulur veya pazar sonuçları için varsayılan veya bir toplanmış kullanır. Bing kullanılan Pazar belirlemek için BingAPIs Pazar üstbilgi bakın.<br /><br /> Bu üstbilgiyi kullanır ve `cc` yalnızca birden çok dil belirtirseniz sorgu parametresi. Aksi takdirde kullanın [mkt](#mkt) ve [setLang](#setlang) sorgu parametreleri.<br /><br /> Bir kullanıcı arabirimi dizesi kullanıcı arabiriminde bir etiketi olarak kullanılan bir dizedir. JSON yanıt nesneleri birkaç kullanıcı arabirimi dizeleri vardır. Herhangi bir bağlantı yanıt nesneleri aratıp özelliklerinde belirtilen dili uygulayın.|  
|<a name="market" />BingAPIs-Pazar|Yanıtı üstbilgisi.<br /><br /> İstek tarafından kullanılan Pazar. Form \<languageCode\>-\<countryCode\>. Örneğin, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Yanıtı üstbilgisi.<br /><br /> İstek ayrıntılarını içeren bir günlük girişi kimliği. Bir hata oluştuğunda, bu kimliği yakalama Belirlemek ve sorunu çözmek mümkün değilse, Destek ekibine sağladığınız bilgilerin yanı sıra bu Kimliğini içerir.|  
|<a name="subscriptionkey" />Ocp Apim abonelik anahtarı|Gerekli isteği üstbilgisi.<br /><br /> Bu hizmet için kaydolan getirdiğinizde aldığınız abonelik anahtarı [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|İsteğe bağlı istek üstbilgisi<br /><br /> Varsayılan olarak, Bing varsa önbelleğe alınmış içeriği döndürür. Önbelleğe alınmış içeriği döndürmesini Bing engellemek için Pragma üstbilgi no cache olarak ayarlayın (örneğin, Pragma: no-cache).
|<a name="useragent" />Kullanıcı Aracısı|İsteğe bağlı isteği üstbilgisi.<br /><br /> İsteğin kaynaklandığı kullanıcı aracısı. Bing kullanıcı aracısı mobil kullanıcılar en iyi duruma getirilmiş bir deneyim sağlamak için kullanır. İsteğe bağlı olsa da her zaman bu üstbilgisi belirtmeniz önerilir.<br /><br /> Kullanıcı Aracısı, yaygın olarak kullanılan tarayıcılar gönderir aynı dize olmalıdır. Kullanıcı aracıları hakkında daha fazla bilgi için bkz: [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Kullanıcı Aracısı dizeleri örnekleri verilmiştir.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (uyumlu; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 derleme/Zencefilli KURABİYE) AppleWebKit/533.1 (KHTML; ister Gecko) sürüm/4.0 mobil Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone işletim sistemi 6_1 Mac OS X gibi) AppleWebKit/536.26 (KHTML; ister Gecko) mobil/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; RV:11.0) ister Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 Mac OS X gibi) AppleWebKit/537.51.1 (Gecko gibi KHTML) sürüm/7.0 mobil/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X MSEdge ClientID|İsteğe bağlı istek ve yanıt üstbilgi.<br /><br /> Bing kullanıcılara Bing API çağrıları arasında ile tutarlı davranışı sağlamak için bu üstbilgiyi kullanır. Bing genellikle yeni özellikler ve geliştirmeler uçuşlar ve istemci kimliği farklı uçuşlar trafiğinde atamak için bir anahtar olarak kullanır. Birden çok istekte bir kullanıcı için aynı istemci kimliği kullanmıyorsanız Bing kullanıcı için birden çok çakışan uçuşlar atayabilir. İçin birden çok çakışan uçuşlar atanmasını tutarsız kullanıcı deneyimi için yol açabilir. Örneğin, ikinci istek ilk daha farklı uçuş atama sahipse deneyimi beklenmeyen olabilir. Bing web sonuçlarını o istemciye uyarlamak için Ayrıca, istemci Kimliğini kullanabilirsiniz kimliğin arama geçmişi, kullanıcı için daha zengin bir deneyim sağlar.<br /><br /> Bing bir istemci kimliği tarafından oluşturulan etkinliğini çözümleyerek sonuç derecelendirmeleri geliştirmeye yardımcı olmak için bu üst de kullanır. Bing API'leri ve sırayla tıklatın aracılığıyla etkinleştirir yüksek hızları için API tüketici teslim sonuçlarının daha iyi kalitesiyle Yardım ilgi geliştirmeleri.<br /><br /> **Önemli:** isteğe bağlı olsa da bu başlığı gerekli göz önünde bulundurmanız gerekir. İstemci kimliği aynı son kullanıcı ve aygıt birleşimi için birden çok istekte kalıcı tutarlı bir kullanıcı deneyimi ve daha iyi sonuçlar kalitesini aracılığıyla 2) daha yüksek tıklatın aracılığıyla ücretlerin Bing API'lerden almak 1) API tüketici sağlar.<br /><br /> Bu başlığına uygulanan temel kullanım kuralları aşağıda verilmiştir.<br /><ul><li>Cihazda uygulamanızın kullandığı her bir kullanıcı bir benzersiz olması gerekir, istemci kimliği Bing oluşturulan<br /><br/>Bu üst istekte eklemezseniz Bing kimliği oluşturur ve X MSEdge ClientID yanıt üstbilgisinde döndürür. Bu üst bir istekte içermemelidir yalnızca kullanıcı uygulamanızı bu cihaz üzerinde ilk kullandığında saattir.<br /><br/></li><li>Cihazda bu kullanıcı için uygulamanızı yapar her Bing API'si isteği için istemci Kimliğini kullanın.<br /><br/></li><li>**DİKKAT:** bu istemci kimliği herhangi bir authenticatable kullanıcı hesabı bilgisi linkable değil emin olmalısınız.</li><br/><li>İstemci kimliği Sürdür Bir tarayıcı uygulaması kimliği sürdürmek için tüm oturumlarında kullanılan kimliği emin olmak için kalıcı bir HTTP tanımlama bilgisi kullanın. Bir oturum tanımlama bilgisi kullanmayın. Mobil uygulamalar gibi diğer uygulamalar için cihazın kalıcı depolama kimliği sürdürmek için kullanın.<br /><br/>Kullanıcı bu aygıtta uygulamanızı kullanan sonraki açışınızda, kalıcı istemci kimliği alın.</li></ul><br /> **Not:** Bing yanıtlar olabilir ya da bu başlığı içermeyebilir. Yanıt bu üst bilgisi içeriyorsa, istemci kimliği yakalayın ve bu cihazdaki kullanıcı için tüm sonraki Bing istekler için kullanın.<br /><br /> **Not:** X MSEdge ClientID eklerseniz, tanımlama bilgileri istek içermemesi gerekir.|  
|<a name="clientip" />X MSEdge ClientIP|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemci aygıtı IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgileri güvenli arama davranışını belirlemek için kullanır.<br /><br /> **Not:** isteğe bağlı olsa da, her zaman bu başlığı ve X arama konum üstbilgisi belirtmeniz önerilir.<br /><br /> Adres (örneğin, son sekizli 0 olarak değiştirerek) belirsizleştirirseniz değil. Herhangi bir yere gerçek konumuna cihazın olmaması konumu adresi sonuçlarında obfuscating, hangi hatalı sonuçları hizmet veren Bing neden olabilir.|  
|<a name="location" />X arama konumu|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemcinin coğrafi konumunu tanımlayan anahtar/değer çiftleri noktalı virgülle ayrılmış listesi. Bing güvenli arama davranışını belirlemek için ve ilgili yerel içerik döndürmek için konum bilgileri kullanır. Anahtar/değer çifti olarak belirtmek \<anahtar\>:\<değeri\>. Kullanıcının konumunu belirtmek üzere kullanacak anahtarları şunlardır:<br /><br /><ul><li>LAT&mdash;derece cinsinden istemcinin konumun enlem. Enlem-90.0 eşit veya daha büyük olmalıdır ve +90.0 küçük veya buna eşit. Negatif değerler Güney latitudes belirtmek ve Kuzey latitudes pozitif değer belirtin.<br /><br /></li><li>uzun&mdash;derece cinsinden istemcinin konumu boylamını. Boylam-180.0 eşit veya daha büyük olmalıdır ve +180.0 küçük veya buna eşit. Negatif değerler Batı longitudes belirtmek ve Doğu longitudes pozitif değer belirtin.<br /><br /></li><li>RE&mdash; koordinatları yatay doğruluğunu belirten RADIUS, ölçümler içinde. Cihazın konum hizmeti tarafından döndürülen değer geçirin. Tipik değerleri GPS/Wi-Fi için 22 m, hücre kule Üçlü 380 m ve geriye doğru IP arama için 18, 000 m olabilir.<br /><br /></li><li>TS&mdash; ne zaman istemci konumunda oluştu, UTC UNIX zaman damgası. (UNIX zaman damgası 1 Ocak 1970'ten beri geçen saniye sayısıdır.)<br /><br /></li><li>HEAD&mdash;isteğe bağlı. İstemcinin göreli başlığını veya seyahat yönü. 0'dan göre doğru Kuzey yönünde sayım 360 derece olarak seyahat yönünü belirtin. Bu anahtar yalnızca, belirtin `sp` anahtarıdır sıfır olmayan bir değer.<br /><br /></li><li>SP&mdash; istemci aygıt dolaşan saniye başına metre içinde Yatay hız (hızlı).<br /><br /></li><li>alt&mdash; istemci aygıtı ölçümler yüksekliği.<br /><br /></li><li>olan&mdash;isteğe bağlı. Koordinatları dikey doğruluğunu belirten RADIUS, ölçümler içinde. Varsayılan değer 50 kilometre RADIUS. Yalnızca belirtirseniz, bu anahtarı belirtmek `alt` anahtarı.<br /><br /></li></ul> **Not:** Bu anahtarları isteğe bağlı, ancak daha doğru konuma sonucu olan sağlayan daha fazla bilgi.<br /><br /> **Not:** her zaman kullanıcının coğrafi konumu belirtmek için önerilir. Konum (örneğin, istemci VPN kullanıyorsa) istemcinin IP adresini kullanıcının fiziksel konumunu doğru şekilde yansıtmaz durumunda özellikle önemlidir. En iyi sonuçlar için bu başlığı ve X MSEdge ClientIP başlığı içermelidir, ancak en azından, bu başlığı içermelidir.|

> [!NOTE] 
> Kullanım Koşulları'nı bu üstbilgileri kullanımıyla dahil olmak üzere tüm geçerli yasaları ile uyumluluğu gerekli olduğunu unutmayın. Örneğin, Avrupa gibi belirli daireleri de kullanıcı aygıtları üzerinde belirli izleme cihazlarını yerleştirme önce kullanıcı onayı almak için gereksinimi yoktur.
  

## <a name="query-parameters"></a>Sorgu parametreleri  
İstek aşağıdaki sorgu parametreleri içerebilir. Gerekli parametre için gerekli sütununa bakın. URL gerekir sorgu parametrelerini kodlayın.  
  
  
|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />Mkt|Sonuçları alınacağı yeri Pazar. <br /><br />Olası Pazar değerler listesi için bkz: [Pazar kodları](#market-codes).<br /><br /> **Not:** URL önizleme API'sı şu anda yalnızca tr destekler-Pazar ve dili.<br /><br />|Dize|Evet|  
|<a name="query" />q|Önizleme için URL|Dize|Evet|  
|<a name="responseformat" />responseFormat|Yanıt için kullanılacak medya türü. Büyük küçük harf duyarsız olası değerler şunlardır:<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> JSON varsayılandır. JSON hakkında bilgi yanıtı içerdiğini nesneleri için bkz: [yanıt nesneleri](#response-objects).<br /><br />  JsonLd belirtirseniz, yanıt gövdesi, arama sonuçlarını içeren JSON-LD nesneleri içerir. JSON-LD hakkında daha fazla bilgi için bkz: [JSON-LD](http://json-ld.org/).|Dize|Hayır|  
|<a name="safesearch" />güvenli arama|Yetişkinlere yönelik içeriğe filtre uygulamak için kullanılan bir filtre. Olası büyük küçük harf duyarsız filtre değerleri şunlardır:<br /><ul><li>Devre dışı&mdash;dönüş yetişkin metin, görüntüler veya videolar Web sayfaları.<br /><br/></li><li>Orta&mdash;dönüş yetişkin metin, ancak değil yetişkin görüntüler veya videolar Web sayfaları.<br /><br/></li><li>Katı&mdash;yetişkin metin, görüntüler veya videolar Web sayfalarının döndürmüyor.</li></ul><br /> Orta varsayılandır.<br /><br /> **Not:** isteği bir pazar geliyorsa gerektiren bu Bing'ın yetişkinlere yönelik ilke `safeSearch` ayarlanır katı için Bing yoksayar `safeSearch` değer ve katı kullanır.<br/><br/>**Not:** kullanırsanız `site:` sorgu işleci yanıt yetişkinlere yönelik içeriğe ne bağımsız olarak içerebilir fırsat yok `safeSearch` sorgu parametresi olarak ayarlanmış. Kullanım `site:` yalnızca sitedeki içeriğin farkında ve senaryonuz yetişkinlere yönelik içeriğe olasılığını destekler. |Dize|Hayır|  
|<a name="setlang" />setLang|Kullanıcı arabirimi dizeleri için kullanılacak dili. ISO 639-1 2 harfli dil kodunu kullanarak dili belirtin. Örneğin, İngilizce dil kodu tr. TR (İngilizce) varsayılandır.<br /><br /> İsteğe bağlı olsa da her zaman dil belirtmelisiniz. Genellikle, ayarladığınız `setLang` tarafından belirtilen aynı dil için `mkt` farklı bir dilde görüntülenen kullanıcı arabirimi dizeleri kullanıcının istediği sürece.<br /><br /> Bu parametre ve [Accept-Language](#acceptlanguage) üstbilgi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.<br /><br /> Bir kullanıcı arabirimi dizesi kullanıcı arabiriminde bir etiketi olarak kullanılan bir dizedir. JSON yanıt nesneleri birkaç kullanıcı arabirimi dizeleri vardır. Ayrıca, herhangi bir bağlantı yanıt nesneleri aratıp özelliklerinde belirtilen dil uygulayın.|Dize|Hayır| 


## <a name="response-objects"></a>Yanıt nesneleri  
Yanıt şeması ya da bir [Web] olduğu veya Web arama API olduğu gibi ErrorResponse. İstek başarısız olursa, en üst düzey nesnesidir [ErrorResponse](#errorresponse) nesnesi.


|Nesne|Açıklama|  
|------------|-----------------|  
|[Web]|Önizleme özniteliklerini içeren üst düzey JSON nesnesi.|  
|[Olay]|Bulguları içeren üst düzey JSON nesnesi.| 
|[Varlıklar|Varlık ayrıntıları içeren üst düzey JSON nesnesi.| 

  
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

  
  
### <a name="license"></a>Lisans  
Altında metin veya resim kullanılabilir lisans tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|ad|Lisans adı.|Dize|  
|url|Kullanıcı Lisansı hakkında daha fazla bilgi nereden bir Web sitesi URL'si.<br /><br /> Köprü oluşturma için adı ve URL kullanın.|Dize|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Lisans attribution sözleşme kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_ad|LicenseAttribution için ayarlanan tür ipucu.|Dize|  
|lisans|İçeriği altında kullanılabilir lisans.|[Lisans](#license)|  
|licenseNotice|Hedeflenen alanın yanındaki görüntülemek için lisans. Örneğin, "Metin CC tarafından SA lisansı altında".<br /><br /> Lisans kişinin adı ve URL kullan `license` lisans ayrıntılarını açıklayan bir Web sitesi için köprü oluşturma için alan. Lisans adı yerine `licenseNotice` oluşturduğunuz köprü (örneğin, CC-tarafından-SA) dizesi.|Dize|  
|mustBeCloseToContent|Kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boolean değeri kuralın uygulanacağı alanına yakınlık kapatın. Varsa **doğru**, içeriği yakınında içinde yer almalıdır. Varsa **yanlış**, ya da bu alanı yok, içeriği arayanın istediğiniz kadar yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.|Dize|  
  

### <a name="link"></a>Bağlantı  
Köprü bileşenlerinin tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_ad|Tür ipucu.|Dize|  
|metin|Görüntü metni.|Dize|  
|url|BİR URL. Köprü oluşturma için metni görüntüle ve URL kullanın.|Dize|  
  

### <a name="linkattribution"></a>LinkAttribution  
Bağlantı attribution sözleşme kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_ad|LinkAttribution için ayarlanan tür ipucu.|Dize|  
|mustBeCloseToContent|Kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boolean değeri kuralın uygulanacağı alanına yakınlık kapatın. Varsa **doğru**, içeriği yakınında içinde yer almalıdır. Varsa **yanlış**, ya da bu alanı yok, içeriği arayanın istediğiniz kadar yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.<br /><br /> Bir hedef belirtilmezse, attribution varlık bir bütün olarak uygulanır ve varlık sunu hemen ardından görüntülenmesi gerekir. Bir hedef belirtmeyen birden çok metin ve bağlantı attribution kurallar varsa, bunları birleştirmek ve onları görüntülemek gerekir kullanarak bir "verileri:" etiketi. Örneğin, "verilerden < Sağlayıcı Ad1\> &#124; < Sağlayıcı ad2\>".|Dize|  
|metin|Attribution metin.|Dize|  
|url|Sağlayıcının Web sitesi URL'si. Kullanım `text` ve köprüsü oluşturmak için URL.|Dize|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Medya attribution sözleşme kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_ad|MediaAttribution için ayarlanan tür ipucu.|Dize|  
|mustBeCloseToContent|Kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boolean değeri kuralın uygulanacağı alanına yakınlık kapatın. Varsa **doğru**, içeriği yakınında içinde yer almalıdır. Varsa **yanlış**, ya da bu alanı yok, içeriği arayanın istediğiniz kadar yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.|Dize|  
|url|Ortam içeriğinin köprüsü oluşturmak için kullandığınız URL'si. Örneğin, hedef görüntü ise, yansıma tıklanabilir yapmak için URL'yi kullanın.|Dize|  
  
  
  
### <a name="organization"></a>Kuruluş  
Bir yayımcı tanımlar.  
  
Bir yayımcı adlarına veya kendi Web sitesi ya da her ikisini de sağlayabilir unutmayın.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|ad|Publisher'ın adı.|Dize|  
|url|Publisher'ın Web sitesi URL'si.<br /><br /> Yayımcı bir Web sitesi sağlamayabilir unutmayın.|Dize|  
  
  

### <a name="webpage"></a>Web sayfası  
Hakkında bilgi tanımlayan bir Web sayfası önizlemede.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|
|ad|Sayfa başlığı, mutlaka HTML Başlığı|Dize|
|url|Gerçekte gezinme URL'si (istek ve ardından yeniden yönlendirmeleri)|Dize|  
|açıklama|Kısa açıklaması sayfa ve içerik|Dize|  
|isFamilyFriendly|Web dizindeki öğeler için en doğru; gerçek zamanlı öğesinden yalnızca URL ve sayfa içeriğini değil göre bu algılama yapın|boole|
|primaryImageOfPage/contentUrl|Önizlemede dahil etmek için temsili bir resim URL'si|Dize| 
  
  
### <a name="querycontext"></a>QueryContext  
Bing istek için kullanılan sorgu bağlamı tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|adultIntent|Belirtilen sorgu yetişkin hedefi olup olmadığını gösteren bir Boole değeri. Değer **true** yetişkin hedefi; sorgu varsa, aksi takdirde, **false**.|Boole|  
|alterationOverrideQuery|Özgün dizeyi kullanmak için Bing zorlamak için kullanılacak sorgu dizesi. Örneğin, sorgu dizesi ise *downwind saling*, geçersiz kılma sorgu dizesi olacaktır *+ downwind saling*. Sonuçlanan sorgu dizesini kodlayın unutmayın *% 2Bsaling + downwind*.<br /><br /> Bu alan, yalnızca özgün sorgu dizesi bir yazım hata içeriyorsa dahil edilir.|Dize|  
|alteredQuery|Sorguyu gerçekleştirmek için Bing tarafından kullanılan sorgu dizesi. Özgün sorgu dizesi yazım içeriyorsa Bing değiştirilmiş sorgu dizesini kullanır. Örneğin, sorgu dizesi ise `saling downwind`, değiştirilen sorgu dizesi olacaktır `sailing downwind`.<br /><br /> Bu alan, yalnızca özgün sorgu dizesi bir yazım hata içeriyorsa dahil edilir.|Dize|  
|askUserForLocation|Bing doğru sonuçlar sağlamak için kullanıcının konumunu gerekli olup olmadığını gösteren bir Boole değeri. Kullanıcının konumuna kullanarak belirtilmişse [X MSEdge ClientIP](#clientip) ve [X arama konumunu](#location) üst bilgiler, bu alan yoksayabilirsiniz.<br /><br /> "Bugünün hava durumu" veya "doğru sonuçlar sağlamak için kullanıcının konumunu gereken Restoran bana yakın" gibi konumu kullanan sorgular için bu alan ayarlanır **doğru**.<br /><br /> Konum (örneğin, "Seattle hava") içeren konumu kullanan sorgular için bu alan ayarlamak **false**. Bu alan ayrıca ayarlamak **false** konumu "gibi en iyi satıcılar" farkında olmayan sorgular için.|Boole|  
|originalQuery|İstekte belirtilen sorgu dizesi.|Dize|  

### <a name="identifiable"></a>Tanımlama
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|id|Bir kaynak tanımlayıcısı|Dize|
 
### <a name="rankinggroup"></a>RankingGroup
Tanımlayan grubu bir arama sonuçları, gibi mainline.
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|öğeler|Grubundaki görüntülemek için arama sonuçlarının listesi.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Görüntülemek için bir arama sonucu öğesi tanımlar.
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|resultIndex|Görüntülenecek yanıt öğenin sıfır tabanlı dizini. Öğe bu alan içermiyorsa, tüm öğeleri yanıtında görüntüler. Örneğin, tüm haber makaleleri haber yanıtında görüntüler.|Tamsayı|
|answerType|Görüntülemek için öğeyi içeren yanıt. Örneğin, haber.<br /><br />Yanıt SearchResponse nesnesinde bulunacak türünü kullanın. SearchResponse alanın adını türüdür.<br /><br /> Ancak, yalnızca bu nesne değeri alanı varsa yanıt türünü kullanın; Aksi takdirde yok sayın.|Dize|
|textualIndex|Görüntülenecek textualAnswers yanıtında dizini.| İşaretsiz tamsayı|
|değer|Görüntülemek için bir yanıt veya öğeyi görüntülemek için bir yanıt tanımlayan kimliği. Bir yanıt kodu tanımlar, yanıt tüm öğeleri görüntüleyin.|Tanımlama|

### <a name="rankingresponse"></a>RankingResponse  
Arama sonuçları sayfası içeriği yerleştirilmesi gerektiğini ve hangi sırayla tanımlar.  
  
|Ad|Değer|  
|----------|-----------|  
|<a name="ranking-mainline" />mainline|Mainline içinde görüntülemek için arama sonuçları.|  
|<a name="ranking-pole" />kutbu'na|En görünür işleme karşılanan arama sonuçları (örneğin, mainline gösterilen ve kenar çubuğu).|  
|<a name="ranking-sidebar" />Kenar Çubuğu|Kenar çubuğunda görüntülemek için arama sonuçları.| 


### <a name="searchresponse"></a>SearchResponse  
İstek başarılı olduğunda, yanıtı içeren üst düzey nesnenin tanımlar.  
  
Bir hizmet reddi saldırısına hizmet suspects, istek başarılı olduğunu unutmayın (HTTP durum kodu 200 Tamam); Ancak, yanıt gövdesi boş olur.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_ad|SearchResponse için ayarlanmış ipucu yazın.|Dize|  
|Web sayfası|Önizleme tanımlayan bir JSON nesnesi|dize|  
  
  
### <a name="textattribution"></a>TextAttribution  
Düz metin attribution sözleşme kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_ad|TextAttribution için ayarlanan tür ipucu.|Dize|  
|metin|Attribution metin.<br /><br /> Metin attribution varlık bir bütün olarak uygulanır ve varlık sunu hemen ardından görüntülenmesi gerekir. Bir hedef belirtmeyen birden çok metin veya bağlantı attribution kurallar varsa, bunları birleştirmek ve onları görüntülemek gerekir kullanarak bir "verileri:" etiketi.|Dize| 


## <a name="error-codes"></a>Hata kodları

Bir istek döndürür olası HTTP durum kodları şunlardır:  
  
|Durum Kodu|Açıklama|  
|-----------------|-----------------|  
|200|Başarılı.|  
|400|Sorgu parametrelerden biri eksik veya geçerli değil.|  
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
- [C# hızlı başlangıç](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)

