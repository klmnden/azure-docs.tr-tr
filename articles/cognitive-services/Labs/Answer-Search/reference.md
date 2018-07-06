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
ms.openlocfilehash: a12761c2d913cd7ffaa2cbc2cd42576c6bc96434
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866993"
---
# <a name="project-answer-search-v7-reference"></a>Proje yanıt arama v7 başvurusu

Bing yanıt SearchAPI bir sorgu parametresi alan ve döndüren bir `searchResponse` ile `answerType`: `facts` veya `entities`. 

Yanıt arama API kullanan uygulamalardan sorgu parametresi önizlemek için bir URL ile uç nokta isteği gönderin.  İstek içermelidir `q=searchTerm` parametresi ve *Ocp-Apim-Subscription-Key* başlığı.   

Olgu ve arama'nın hakkındaki ayrıntıları içeren varlıkları için JSON yanıtı ayrıştırılamaz.

## <a name="endpoint"></a>Uç Nokta
Yanıt arama sonuçları istemek için aşağıdaki uç noktaya bir istek gönderin. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

GET uç noktası: 
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

````

İstek, HTTPS protokolünü kullanmak ve ardından sorgu parametresi içerir:
-  q =<URL> -arama nesneyi tanımlayan sorgu

İsteğinde bulunmak nasıl gösteren örnekler için bkz: [C# hızlı](c-sharp-quickstart.md) veya [Java Hızlı Başlangıç](java-quickstart.md). 

Aşağıdaki bölümler yanıt nesneleri, sorgu parametreleri ve arama sonuçlarını etkileyecek üstbilgileri hakkında teknik ayrıntılar sağlar. 
  
İstekleri içermelidir üstbilgileri hakkında daha fazla bilgi için bkz: [üstbilgileri](#headers).  
  
İstekleri içermelidir sorgu parametreleri hakkında daha fazla bilgi için bkz: [sorgu parametreleri](#query-parameters).  
  
JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).

Maksimum sorgu URL'SİNİN uzunluğu 2.048 karakterdir. URL uzunluğu sınırı aşmadığından emin olmak için sorgu parametrelerinizin uzunluğu en fazla 1500'den az karakter olmalıdır. URL 2.048 karakterden uzunsa sunucu 404 bulunamadı hatası döndürür.  

İzin verilen kullanım ve sonuçları görüntüleme hakkında daha fazla bilgi için bkz. [kullanın ve gereksinimlerini görüntülemek](use-display-requirements.md). 

> [!NOTE]
> URL önizlemesi, diğer arama API'lerini için anlamlı olan bazı istek üst etkilemez
> - Pragma – URL önizlemesi önbellek kullanıp kullanmadığını denetim çağırana sahip değil
> - Önbellek denetimi – çağıran URL önizlemesi önbellek kullanıp kullanmadığını denetim yok
> - Kullanıcı Aracısı

> Ayrıca, bazı parametreler için URL önizleme API'sı şu anda anlamlı değildir, ancak gelecekte geliştirilmiş Genelleştirme için kullanılabilir. 
 
## <a name="headers"></a>Üst bilgiler  
İstek ve yanıt içerebilecek üst bilgiler verilmiştir.  
  
|Üst bilgi|Açıklama|  
|------------|-----------------|  
|Kabul|İsteğe bağlı isteği üstbilgisi.<br /><br /> Varsayılan medya türü application/json şeklindedir. Yanıt kullandığını belirtmek için [JSON-LD](http://json-ld.org/), uygulama/ld + json Accept üst bilgisi ayarlayın.|  
|<a name="acceptlanguage" />Kabul dil|İsteğe bağlı isteği üstbilgisi.<br /><br /> Kullanıcı arabirimi dizeleri için kullanılacak dil virgülle ayrılmış listesi. Tercih sırasına göre azalan düzende listesidir. Beklenen biçim'dahil olmak üzere daha fazla bilgi için bkz. [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Bu üst bilgi ve [setLang](#setlang) sorgu parametresi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.<br /><br /> De belirtmeniz gerekir bu başlığı ayarlarsanız [cc](#cc) sorgu parametresi. Pazar için sonuçlar döndürecek şekilde belirlemek için Bing listeden bulur ve bununla birleştirir ilk desteklenen dil kullanan `cc` parametre değeri. Liste, desteklenen bir dil içermiyorsa, Bing en yakın dil ve istek destekleyen Pazar bulur veya pazar sonuçları için varsayılan veya bir toplanmış kullanır. Bing kullanılan Pazar belirlemek için BingAPIs pazara açılma üstbilgi bakın.<br /><br /> Bu üstbilgiyi kullanır ve `cc` birden çok dil belirtirseniz sorgu parametresi. Aksi takdirde kullanın [mkt](#mkt) ve [setLang](#setlang) sorgu parametreleri.<br /><br /> Bir kullanıcı arabirimi dizesi kullanıcı arabiriminde bir etiket olarak kullanılan bir dizedir. JSON yanıtı nesneleri birkaç kullanıcı arabirimi dizeleri vardır. Yanıt nesnelerinin Bing.com özelliklerinde herhangi bir bağlantı belirtilen dili uygulayın.|  
|<a name="market" />BingAPIs-Pazar|Yanıtı üstbilgisi.<br /><br /> İstek tarafından kullanılan Pazar. Form \<languageCode\>-\<countryCode\>. Örneğin, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Yanıtı üstbilgisi.<br /><br /> İsteğinin ayrıntılarını içeren günlük girdisi kimliği. Bir hata oluştuğunda, bu kimliği yakalama Belirlemek ve sorunu çözmek mümkün değilse, bu kimliği yanı sıra destek ekibinin sağladığı diğer bilgiler içerir.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Gerekli isteği üstbilgisi.<br /><br /> Bu hizmet için RMS'ye kaydolurken aldığınız abonelik anahtarını [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragması|İsteğe bağlı bir istek üst bilgisi<br /><br /> Varsayılan olarak, önbelleğe alınmış içeriği varsa Bing döndürür. Önbelleğe alınan içerik döndürmesini Bing önlemek için Pragma üstbilgisi no-cache olarak ayarlayın (örneğin, Pragması: no-cache).
|<a name="useragent" />Kullanıcı Aracısı|İsteğe bağlı isteği üstbilgisi.<br /><br /> İsteğin kaynaklandığı kullanıcı aracısı. Bing, mobil kullanıcıların iyileştirilmiş bir deneyim sağlamak için Kullanıcı aracısını kullanır. İsteğe bağlı olsa da, her zaman bu üstbilgisi belirtmeniz önerilir.<br /><br /> Kullanıcı Aracısı, yaygın olarak kullanılan tüm tarayıcılar gönderen aynı dize olmalıdır. Kullanıcı aracıları hakkında daha fazla bilgi için bkz. [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Kullanıcı Aracısı dizeleri örnekleri aşağıda verilmiştir.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (uyumlu; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Dokunma; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 derleme/GINGERBREAD) AppleWebKit/533.1 (KHTML; ister Gecko) sürüm/4.0 mobil Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; Mac OS X gibi CPU iPhone OS 6_1) AppleWebKit/536.26 (KHTML; ister Gecko) mobil/10B142 iPhone4; BingWeb/3.03.1428.20120423 1<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Dokunma; RV:11.0) Gecko ister<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; Mac OS X gibi CPU işletim sistemi 7_0) AppleWebKit/537.51.1 (Gecko gibi KHTML) sürüm/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X MSEdge ClientID|İsteğe bağlı istek ve yanıt üst bilgisi.<br /><br /> Bing Bing API çağrıları arasında tutarlı bir davranış ile kullanıcılara sağlamak için bu üstbilgiyi kullanır. Bing sıklıkla yeni özellikler ve geliştirmeler uçuşlar ve istemci kimliği farklı uçuşlar trafiğinde atamak için bir anahtar olarak kullanır. Birden çok istekler genelinde bir kullanıcı için aynı istemci Kimliğini kullanmazsanız Bing için birden çok çakışan uçuşlar kullanıcı atayabilir. Birden çok çakışan uçuşlar için atanan bir tutarsız kullanıcı deneyimine neden olabilir. Örneğin, bir farklı uçuş ataması ilk değerinden ikinci isteği varsa deneyimi beklenmeyen olabilir. Bing web sonuçları, istemci için uygun hale getirmek için Ayrıca, istemci Kimliğini kullanabilirsiniz kimliğin arama geçmişi, kullanıcı için daha zengin bir deneyim sağlar.<br /><br /> Bing sonucu sonuçlarımızda bir istemci kimliği tarafından oluşturulan bir etkinlik analiz ederek geliştirmeye yardımcı olmak için bu üst bilgi de kullanır. İlgi geliştirmeleri, Bing API'leri ve sırayla tıklama etkinleştirir daha yüksek ücretler için API tüketici teslim sonuçlarının daha iyi bir nitelikle yardımcı olur.<br /><br /> **Önemli:** isteğe bağlı olsa da, bu üst bilgi gerekli göz önünde bulundurmanız gerekir. İstemci kimliği aynı son kullanıcı ve cihaz birleşimi için birden çok istekler genelinde kalıcı tutarlı bir kullanıcı deneyimi ve daha iyi sonuçlar kalitesini aracılığıyla 2) daha yüksek tıklama oranları Bing API'lerinden almak 1) API tüketici sağlar.<br /><br /> Bu üst bilgiye Uygula temel kullanım kuralları aşağıda verilmiştir.<br /><ul><li>Cihazda uygulamanızın kullandığı her bir kullanıcı bir benzersiz olmalıdır, Bing oluşturulan istemci kimliği.<br /><br/>İstekte bu başlığı eklemezseniz, Bing bir kimlik üretir ve X MSEdge ClientID yanıt üst bilgisinde döndürür. Bir istekte bu üst bilgi içermemelidir yalnızca bir kez ilk kez kullanıcı o cihazda uygulamanızın kullandığı ' dir.<br /><br/></li><li>İstemci Kimliğini, cihazda bu kullanıcı için uygulamanıza yaptığı her Bing API isteği için kullanın.<br /><br/></li><li>**DİKKAT:** bu istemci kimliği için herhangi bir authenticatable kullanıcı hesabı bilgisi değişkenlerinden değil emin olmanız gerekir.</li><br/><li>Kalıcı istemci kimliği. Bir tarayıcı uygulamasında kimliği kalıcı hale getirmek için kalıcı bir HTTP tanımlama bilgisi kimliği tüm oturumlarda kullanıldığından emin olmak için kullanın. Oturum tanımlama bilgisinin kullanmayın. Mobil uygulamalar gibi diğer uygulamalar için cihazın kalıcı depolama kimliği kalıcı hale getirmek için kullanın.<br /><br/>Kullanıcı, cihazda uygulamanızın kullandığı bir sonraki açışınızda, kalıcı bir istemci kimliği alın.</li></ul><br /> **Not:** Bing yanıtlarını olabilir veya bu başlığı içermeyebilir. Bu üst bilgi yanıtı içeriyorsa, istemci kimliği yakalamak ve o cihazdaki kullanıcı için tüm sonraki Bing istekler için kullanın.<br /><br /> **Not:** X MSEdge ClientID eklerseniz, tanımlama bilgilerinin istekte içermemesi gerekir.|  
|<a name="clientip" />X MSEdge Clientıp|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemci cihazı IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgileri güvenli arama davranışını belirlemek için kullanır.<br /><br /> **Not:** isteğe bağlı olsa da, her zaman bu üst bilgi ve X-Search-Location üst bilgisini belirtmeniz önerilir.<br /><br /> Adres (örneğin, son sekizli 0 olarak değiştirerek) karartmak değil. Her yerden gerçek cihazın konumuna olmaması konumu adresi sonuçlarında obfuscating, hangi hatalı sonuçlar sunan Bing içinde neden olabilir.|  
|<a name="location" />X arama konumu|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemcinin coğrafi konumunu tanımlayan anahtar/değer çiftleri noktalı virgülle ayrılmış listesi. Güvenli arama davranışlarını belirlemek ve ilgili yerel içeriğini döndürmek için Bing konum bilgileri kullanır. Anahtar/değer çifti olarak belirtmek \<anahtarı\>:\<değer\>. Kullanıcının konumunu belirtmek üzere kullanacak anahtarlar şunlardır:<br /><br /><ul><li>LAT&mdash;derece cinsinden istemcinin konumun enlem. Büyüktür veya eşittir-90.0 enlem olmalıdır ve +90.0 küçüktür veya eşittir. Güney latitudes negatif değerleri göstermek ve Kuzey latitudes pozitif değerleri gösterir.<br /><br /></li><li>uzun&mdash;derece cinsinden istemcinin konumun boylam. Büyüktür veya eşittir-180.0 boylam olmalıdır ve +180.0 küçüktür veya eşittir. Batı longitudes negatif değerleri göstermek ve Doğu longitudes pozitif değerleri gösterir.<br /><br /></li><li>RE&mdash; koordinatları yatay doğruluğunu belirten RADIUS, ölçümleri içinde. Cihazın konum hizmeti tarafından döndürülen değeri geçirin. GPS/Wi-Fi için 22 m, hücre tower Üçlü 380 m ve 18, 000 m ters IP araması için normal değerler olabilir.<br /><br /></li><li>TS&mdash; istemci konumunda ne zaman, UTC UNIX zaman damgası. (UNIX zaman damgası 1 Ocak 1970'ten beri geçen saniye sayısı sayısıdır.)<br /><br /></li><li>HEAD&mdash;isteğe bağlı. İstemcinin göreli başlığını veya seyahat yönü. 0 ile göreli true Kuzey saat yönünde sayım 360 derece olarak seyahat yönünü belirtin. Bu anahtar yalnızca, belirtin `sp` anahtardır sıfır.<br /><br /></li><li>SP&mdash; istemci cihazı dolaşan saniye başına metre olarak yatay hız (hızlı).<br /><br /></li><li>alt&mdash; metre olarak bir istemci cihazının yüksekliği.<br /><br /></li><li>olan&mdash;isteğe bağlı. Koordinatları dikey doğruluğunu belirten RADIUS, ölçümleri içinde. Varsayılan olarak 50 kilometre RADIUS. Yalnızca belirtirseniz, bu anahtarı belirtirsiniz `alt` anahtarı.<br /><br /></li></ul> **Not:** Bu anahtarları isteğe bağlıdır, ancak daha doğru konuma sonucu olan sağlayan daha fazla bilgi.<br /><br /> **Not:** her zaman kullanıcının coğrafi konumu belirtmek için önerilir. Konum (örneğin, istemci VPN kullanıyorsa) istemcinin IP adresini kullanıcının fiziksel konum doğru şekilde yansıtmaz durumunda özellikle önemlidir. En iyi sonuçlar için bu başlığı ve X MSEdge Clientıp başlığı içermelidir, ancak en az bu başlığı içermelidir.|

> [!NOTE] 
> Kullanım Koşulları ile ilgili bu üstbilgileri kullanımı dahil olmak üzere ilgili tüm yasalara uyumluluk gerekli olduğunu unutmayın. Örneğin, Avrupa gibi belirli daireleri de kullanıcı aygıtları üzerinde belirli izleme cihazları yerleştirme önce kullanıcı onayı almak için gereksinimi yoktur.
  

## <a name="query-parameters"></a>Sorgu parametreleri  
Aşağıdaki sorgu parametreleri istek içerebilir. Gerekli Parametreler için gerekli sütununa bakın. URL gereken sorgu parametrelerine kodlayın.  
  
  
|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />Mkt|Sonuçları nereden geldiğini Pazar. <br /><br />Olası Pazar değerler listesi için bkz. [Pazar kodları](#market-codes).<br /><br /> **Not:** URL önizleme API'sı şu anda yalnızca tr destekler-bize pazara çıkma sürelerini ve dili.<br /><br />|Dize|Evet|  
|<a name="query" />q|Önizleme URL'si|Dize|Evet|  
|<a name="responseformat" />responseFormat|Yanıt için kullanılacak medya türü. Büyük küçük harf duyarsız olası değerler şunlardır:<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> JSON varsayılandır. JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).<br /><br />  JsonLd belirtirseniz, yanıt gövdesi, arama sonuçlarını içeren JSON-LD nesneler içerir. JSON-LD hakkında daha fazla bilgi için bkz. [JSON-LD](http://json-ld.org/).|Dize|Hayır|  
|<a name="safesearch" />safeSearch|Yetişkinlere yönelik içeriğe filtrelemek için kullanılan bir filtre. Olası büyük küçük harf duyarsız filtre değerleri şunlardır.<br /><ul><li>Kapalı&mdash;yetişkinlere yönelik metin, görüntü veya video Web sayfalarının döndürür.<br /><br/></li><li>Orta&mdash;yetişkinlere yönelik metin ancak yetişkin değil görüntü veya video Web sayfalarının döndürür.<br /><br/></li><li>Katı&mdash;yetişkinlere yönelik metin, görüntü veya video Web sayfalarının döndürmüyor.</li></ul><br /> Orta varsayılandır.<br /><br /> **Not:** isteği bir pazar geliyorsa, gerektiren bu Bing'in yetişkinlere yönelik ilke `safeSearch` ayarlanır Strıct için Bing yoksayar `safeSearch` değeri ve Strıct kullanır.<br/><br/>**Not:** kullanırsanız `site:` sorgu işleci yanıt ne bakılmaksızın yetişkinlere yönelik içerik içerebilir fırsat yok `safeSearch` sorgu parametresi ayarlanır. Kullanım `site:` yalnızca sitedeki içerikleri haberdar ve senaryonuz yetişkinlere yönelik içeriğe olasılığını destekler. |Dize|Hayır|  
|<a name="setlang" />setLang|Kullanıcı arabirimi dizeleri için kullanılacak dili. ISO 639-1 2 harfli dil kodunu kullanarak dili belirtin. Örneğin, İngilizce dil kodu tr ' dir. TR (Türkçe) varsayılandır.<br /><br /> İsteğe bağlı olsa da, her zaman dil belirtmeniz gerekir. Genellikle, ayarladığınız `setLang` tarafından belirtilen ile aynı dile `mkt` kullanıcının farklı bir dilde görüntülenen kullanıcı arabirimi dizeleri istediği sürece.<br /><br /> Bu parametre ve [Accept-Language](#acceptlanguage) üstbilgi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.<br /><br /> Bir kullanıcı arabirimi dizesi kullanıcı arabiriminde bir etiket olarak kullanılan bir dizedir. JSON yanıtı nesneleri birkaç kullanıcı arabirimi dizeleri vardır. Ayrıca, herhangi bir bağlantı Bing.com yanıt nesnelerinin özelliklerinde belirtilen dil uygulayın.|Dize|Hayır| 


## <a name="response-objects"></a>Yanıt nesneleri  
Yanıt şeması ya da bir [Web] sayfasıdır veya ErrorResponse, Web araması API'si olduğu gibi. İstek başarısız olursa, en üst düzey nesnedir [ErrorResponse](#errorresponse) nesne.


|Nesne|Açıklama|  
|------------|-----------------|  
|[Web]|Önizleme özniteliklerini içeren üst düzey JSON nesnesi.|  
|[Olay]|Bilgiler içeren üst düzey JSON nesnesi.| 
|[Varlıklar|Varlık ayrıntıları içeren üst düzey JSON nesnesi.| 

  
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

  
  
### <a name="license"></a>Lisans  
Altında bir metin veya resim kullanılabilir lisans tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|ad|Lisans adı.|Dize|  
|url|Kullanıcı Lisansı hakkında daha fazla bilgi edinebileceğiniz bir Web sitesi URL'si.<br /><br /> Köprü oluşturmak için adını ve URL'sini kullanın.|Dize|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Lisans attribution için sözleşmeye dayalı bir kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|LicenseAttribution için ayarlanmış bir tür ipucu.|Dize|  
|lisans|İçeriği altında kullanılabilir lisans.|[Lisans](#license)|  
|licenseNotice|Hedeflenen alanının yanındaki lisans. Örneğin, "Metin SA tarafından CC lisansı altında".<br /><br /> Lisans'ın adını ve URL'sini kullan `license` lisans ayrıntılarını açıklayan Web sitesi için köprü oluşturma için alan. Ardından, lisans adı değiştirin `licenseNotice` oluşturduğunuz köprü dizesiyle (örneğin, CC-tarafından-SA).|Dize|  
|mustBeCloseToContent|Kuralın uygulanacağı alan yakınlık kapatın kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boole değeri. Varsa **true**, içeriği yakınında içinde yerleştirilmelidir. Varsa **false**, veya bu alan mevcut değil, arayanın kararımıza içeriği yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.|Dize|  
  

### <a name="link"></a>Bağlantı  
Köprü bileşenlerinin tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|Tür ipucu.|Dize|  
|metin|Görünen metin.|Dize|  
|url|BİR URL. URL'yi kullanın ve metin, köprü oluşturmak için görüntüler.|Dize|  
  

### <a name="linkattribution"></a>LinkAttribution  
İçin bağlantı attribution sözleşmeye dayalı bir kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|LinkAttribution için ayarlanmış bir tür ipucu.|Dize|  
|mustBeCloseToContent|Kuralın uygulanacağı alan yakınlık kapatın kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boole değeri. Varsa **true**, içeriği yakınında içinde yerleştirilmelidir. Varsa **false**, veya bu alan mevcut değil, arayanın kararımıza içeriği yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.<br /><br /> Bir hedef belirtilmemişse atıf varlığa bir bütün olarak uygular ve varlık sunu takip görüntülenmesi gerekir. Bir hedef belirtmeyen birden çok metin ve bağlantı attribution kuralı varsa, bunları birleştirmek ve onları görüntülemek kullanarak bir "veri:" etiketi. Örneğin, "verilerden < sağlayıcısı name1\> &#124; < sağlayıcısı name2\>".|Dize|  
|metin|Attribution metin.|Dize|  
|url|Sağlayıcının Web sitesi URL'si. Kullanım `text` ve'nın köprü oluşturmak için URL.|Dize|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Medya attribution için sözleşmeye dayalı bir kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|MediaAttribution için ayarlanmış bir tür ipucu.|Dize|  
|mustBeCloseToContent|Kuralın uygulanacağı alan yakınlık kapatın kural içeriğini yerleştirilmelidir olup olmadığını belirleyen bir Boole değeri. Varsa **true**, içeriği yakınında içinde yerleştirilmelidir. Varsa **false**, veya bu alan mevcut değil, arayanın kararımıza içeriği yerleştirilebilir.|Boole|  
|targetPropertyName|Kuralın uygulanacağı alanın adı.|Dize|  
|url|Medya içeriklerinin köprüsünü oluşturmak için kullandığınız URL. Örneğin, hedef görüntü varsa görüntü tıklanabilir yapmak için URL kullanın.|Dize|  
  
  
  
### <a name="organization"></a>Kuruluş  
Bir yayımcı olarak tanımlar.  
  
Bir yayımcı adının veya Web sitesi veya her ikisini sağlayabilir unutmayın.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|ad|Yayımcının adı.|Dize|  
|url|Yayımcının Web sitesi URL'si.<br /><br /> Yayımcının Web sitesi sağlamayabilir unutmayın.|Dize|  
  
  

### <a name="webpage"></a>Web sayfası  
Hakkında bilgilerini tanımlayan bir önizleme Web sayfası.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|
|ad|Sayfa başlığı, mutlaka HTML Başlığı|Dize|
|url|Aslında gezinilen URL'si (istek ve ardından yeniden yönlendirmeleri)|Dize|  
|açıklama|Sayfa içeriği ve kısa açıklama|Dize|  
|isFamilyFriendly|Web dizindeki öğeler için en doğru; Bu algılama yöntemi yalnızca bir URL ve sayfa içeriği göre gerçek zamanlı öğesinden yapın|boole|
|primaryImageOfPage/contentUrl|Önizlemede dahil etmek için temsili bir görüntü URL'si|Dize| 
  
  
### <a name="querycontext"></a>QueryContext  
Bing istek için kullanılan sorgu bağlamı tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|adultIntent|Belirtilen sorgu yetişkinlere yönelik sonuçlar olup olmadığını belirten bir Boole değeri. Değer **true** yetişkinlere yönelik sonuçlar; sorgu varsa, aksi takdirde, **false**.|Boole|  
|alterationOverrideQuery|Orijinal dizeyi kullanmak için Bing zorlamak için kullanılacak sorgu dizesi. Örneğin, sorgu dizesi ise *downwind saling*, geçersiz kılma sorgu dizesi olacaktır *+ downwind saling*. Sonuçlanan sorgu dizesini kodlayın unutmayın *% 2Bsaling + downwind*.<br /><br /> Bu alan, yalnızca özgün sorgu dizesi bir yazım hatası içeriyorsa dahildir.|Dize|  
|alteredQuery|Bing tarafından sorguyu gerçekleştirmek için kullanılan sorgu dizesi. Bing yazım hatalarını özgün sorgu dizesini içerdiği değiştirilen sorgu dizesini kullanır. Örneğin, sorgu dizesi ise `saling downwind`, değiştirilen sorgu dizesi olacaktır `sailing downwind`.<br /><br /> Bu alan, yalnızca özgün sorgu dizesi bir yazım hatası içeriyorsa dahildir.|Dize|  
|askUserForLocation|Bing doğru sonuçlar sağlamak için kullanıcının konumuna isteyip istemediğini gösteren bir Boole değeri. Kullanarak kullanıcının bulunduğu konum belirttiyseniz [X MSEdge Clientıp](#clientip) ve [X arama konumu](#location) üst bilgiler, bu alan yoksayabilirsiniz.<br /><br /> "Günün hava durumu" veya "kullanıcının konumuna doğru sonuçlar sağlamak için gereken Yakınımdaki restoranlar" gibi konumu kullanan sorgular için bu alan ayarlanır **true**.<br /><br /> ' % S'konum (örneğin, "Seattle hava") içeren konumu kullanan sorgular için bu alan kümesine **false**. Bu alan ayrıca kümesine **false** konumu "gibi en iyi satıcılar" uyumlu olmayan sorgular.|Boole|  
|originalQuery|İstekte belirtilen sorgu dizesi.|Dize|  

### <a name="identifiable"></a>Tanımlama
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|id|Bir kaynak tanımlayıcısı|Dize|
 
### <a name="rankinggroup"></a>RankingGroup
Tanımlar grubu bir arama sonuçları, aşağıdaki gibi mainline.
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|öğeler|Grup içinde görüntülemek için arama sonuçları listesi.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Görüntülenecek bir arama sonucu öğesi tanımlar.
|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|resultIndex|Görüntülenecek yanıtında öğenin sıfır tabanlı dizini. Bu alan öğe içermiyorsa, yanıt tüm öğeleri görüntüler. Örneğin, haber yanıt tüm haber makalelerini görüntüler.|Tamsayı|
|answerType|Görüntülenecek öğe içeren yanıtı. Örneğin, haber.<br /><br />Yanıt SearchResponse nesnesinde bulunacak türünü kullanın. Türü bir SearchResponse alan adıdır.<br /><br /> Ancak, yalnızca bu nesne değeri alanı varsa yanıt türünü kullanın. Aksi takdirde, yoksayın.|Dize|
|textualIndex|Görüntülenecek textualAnswers yanıt dizini.| İşaretsiz tamsayı|
|değer|Görüntülenecek yanıt veya öğeyi görüntülemek için bir yanıt tanımlayan kimliği. Kimliği bir yanıt tanımlıyorsa, yanıtın tüm öğeleri görüntüler.|Tanımlama|

### <a name="rankingresponse"></a>RankingResponse  
Arama sonuçları sayfası içeriği yerleştirilmesi gerektiğini ve hangi sırayla tanımlar.  
  
|Ad|Değer|  
|----------|-----------|  
|<a name="ranking-mainline" />mainline|Ana hatta da görüntülemek için arama sonuçları.|  
|<a name="ranking-pole" />kutup|Arama sonuçlarını, en çok görünen alınmasına üyelerine gösterilen (örneğin, ana hat görüntülenen ve kenar çubuğunuzu).|  
|<a name="ranking-sidebar" />Kenar Çubuğu|Kenar çubuğunda görüntülemek için arama sonuçları.| 


### <a name="searchresponse"></a>SearchResponse  
İstek başarılı olduğunda, yanıtı içeren üst düzey nesnesi tanımlar.  
  
Hizmet bir saldırı hizmet reddi şüphelenen, istek başarılı olduğunu unutmayın (HTTP durum kodudur 200 Tamam); Ancak, yanıt gövdesi boş olur.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|Tür ipucu SearchResponse için ayarlanır.|Dize|  
|Web sayfası|Önizleme tanımlayan bir JSON nesnesi|dize|  
  
  
### <a name="textattribution"></a>TextAttribution  
Düz metin attribution için sözleşmeye dayalı bir kural tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_türü|TextAttribution için ayarlanmış bir tür ipucu.|Dize|  
|metin|Attribution metin.<br /><br /> Metin atıf varlığa bir bütün olarak uygular ve varlık sunu takip görüntülenmesi gerekir. Bir hedef belirtmeyen birden çok metin veya bağlantı attribution kuralı varsa, bunları birleştirmek ve onları görüntülemek kullanarak bir "veri:" etiketi.|Dize| 


## <a name="error-codes"></a>Hata kodları

Bir isteği döndüren olası HTTP durum kodları şunlardır:  
  
|Durum Kodu|Açıklama|  
|-----------------|-----------------|  
|200|Başarılı.|  
|400|Sorgu parametrelerden biri eksik veya geçerli değil.|  
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
- [C# hızlı başlangıç](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)

