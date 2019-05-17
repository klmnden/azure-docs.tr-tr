---
title: Bing yerel iş arama API'si v7 başvurusu | Microsoft Docs
description: Bing yerel iş arama API'si, programlama öğeleri açıklar.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 82b2f5ca70927856aeac889675b5ec4a54ae034f
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796758"
---
# <a name="bing-local-business-search-api-v7-reference"></a>Bing yerel iş arama API'si v7 başvurusu

Yerel iş arama API'si, Restoran, otel veya diğer yerel işletmeler içeren sonuçları almak için Bing arama sorgusu gönderir. Basamak için sorgu yerel iş veya bir kategoriyi (örneğin, Yakınımdaki restoranlar) adını belirtebilirsiniz. Varlık sonuçları kişileri, yerleri veya nesneleri kapsar. Bu bağlamda yerdir iş varlıkları, durumları, ülkeler/bölgeler vb.  

Bu bölümde, yanıt nesneleri ve sorgu parametreleri ve arama sonuçları etkiler üstbilgileri hakkında teknik ayrıntılar sağlar. İsteğinde bulunmak nasıl gösteren örnekler için bkz: [yerel iş arama C# hızlı](quickstarts/local-quickstart.md) veya [yerel iş arama Java Hızlı Başlangıç](quickstarts/local-search-java-quickstart.md). 
  
İstekleri içermelidir üstbilgileri hakkında daha fazla bilgi için bkz: [üstbilgileri](#headers).  
  
İstekleri içermelidir sorgu parametreleri hakkında daha fazla bilgi için bkz: [sorgu parametreleri](#query-parameters).  
  
JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).

İzin verilen kullanım ve sonuçları görüntüleme hakkında daha fazla bilgi için bkz. [kullanın ve gereksinimlerini görüntülemek](use-display-requirements.md).


  
## <a name="endpoint"></a>Uç Nokta  
Yerel iş sonuçları istemek için bir GET isteği gönder: 

``` 
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search

```
  
İstek, HTTPS protokolünü kullanmalıdır.  
  
> [!NOTE]
> Maksimum URL uzunluğu 2.048 karakterdir. URL uzunluğu sınırı aşmadığından emin olmak için sorgu parametrelerinizin uzunluğu en fazla 1500'den az karakter olmalıdır. URL 2.048 karakterden uzunsa sunucu 404 bulunamadı hatası döndürür.  
  
  
## <a name="headers"></a>Üst bilgiler  
İstek ve yanıt içerebilecek üst bilgiler verilmiştir.  
  
|Üst bilgi|Açıklama|  
|------------|-----------------|  
|Kabul|İsteğe bağlı istek üst bilgisi.<br /><br /> Varsayılan medya türü application/json şeklindedir. Yanıt kullandığını belirtmek için [JSON-LD](https://json-ld.org/), uygulama/ld + json Accept üst bilgisi ayarlayın.|  
|<a name="acceptlanguage" />Accept-Language|İsteğe bağlı istek üst bilgisi.<br /><br /> Kullanıcı arabirimi dizelerinde kullanılacak virgülle sınırlanmış bir dil listesi. Liste, tercih edilme durumuna göre azalan düzende sıralanır. Beklenen biçim de içinde olmak üzere daha fazla bilgi için bkz. [RFC2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Bu üst bilgi ve [setLang](#setlang) sorgu parametresi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin.<br /><br /> Bu üst bilginin ayarlarsanız, cc sorgu parametresini de belirtmeniz gerekir. Hangi pazardan sonuç döndürüleceğini belirlemek için, Bing listeden bulduğu ilk desteklenen dili kullanır ve bunu `cc` parametresinin değeriyle birleştirir. Liste desteklenen bir dil içermiyorsa, Bing isteği destekleyen en yakın dili ve pazarı bulur ya da sonuçlar için toplu veya varsayılan bir pazar kullanır. Bing'in kullandığı pazarı saptamak için BingAPIs-Market üst bilgisine bakın.<br /><br /> Ancak birden çok dil belirtirseniz bu üst bilgiyi ve `cc` sorgu parametresini kullanın. Aksi takdirde, [mkt](#mkt) ile [setLang](#setlang) sorgu parametrelerini kullanın.<br /><br /> Kullanıcı arabirimi dizesi, kullanıcı arabiriminde etiket olarak kullanılan dizedir. JSON yanıt nesnelerinde çok az kullanıcı arabirimi dizesi vardır. Yanıt nesnelerinde Bing.com özelliklerine yönelik bağlantılar da belirtilen dildedir.|  
|<a name="market" />BingAPIs-Market|Yanıt üst bilgisi.<br /><br /> İstek tarafından kullanılan pazar. Biçimi şöyledir: \<languageCode\>-\<countryCode\>. Örneğin, tr-TR.|  
|<a name="traceid" />BingAPIs-TraceId|Yanıt üst bilgisi.<br /><br /> İsteğin ayrıntılarını içeren günlük girdisinin kimliği. Hata oluştuğunda, bu kimliği yakalayın. Sorunu belirleyemez ve çözemezseniz, Destek ekibine diğer bilgilerle birlikte bu kimliği de sağlayın.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Gerekli istek üst bilgisi.<br /><br /> [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/)'de bu hizmete kaydolduğunuzda aldığınız abonelik anahtarı.|  
|<a name="pragma" />Pragma|İsteğe bağlı istek üst bilgisi<br /><br /> Varsayılan olarak, Bing önbelleğe alınmış içeriği (varsa) döndürür. Bing'in önbelleğe alınmış içeriği döndürmesini önlemek için, Pragma üst bilgisini no-cache olarak ayarlayın (örneğin, Pragma: no-cache).
|<a name="useragent" />User-Agent|İsteğe bağlı istek üst bilgisi.<br /><br /> İsteği başlatan kullanıcı aracısı. Bing, mobil kullanıcılara iyileştirilmiş bir deneyim sağlamak için kullanıcı aracısını kullanır. İsteğe bağlı olsa da, bu üst bilgiyi her zaman belirtmeniz önerilir.<br /><br /> User-agent, yaygın olarak kullanılan tarayıcılardan gönderilen dizeyle aynı olmalıdır. Kullanıcı aracıları hakkında bilgi için bkz. [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Aşağıda örnek user-agent dizelerini bulabilirsiniz.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; like Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|İsteğe bağlı istek ve yanıt üst bilgisi.<br /><br /> Bing, kullanıcılara tüm Bing API çağrılarında tutarlı bir davranış sağlamak için bu üst bilgiyi kullanır. Bing sık sık yeni özellikler ve geliştirmeler dağıtır ve farklı dağıtımlarda trafik ataması yapmak için anahtar olarak istemci kimliğini kullanır. Bir kullanıcı için birden çok istekte aynı istemci kimliğini kullanmazsanız, Bing kullanıcıyı birden çok çakışan dağıtıma atayabilir. Birden çok çakışan dağıtıma eklenmek, tutarsız bir kullanıcı deneyimine yol açabilir. Örneğin, ikinci isteğin dağıtım ataması ilkinden farklıysa, beklenmeyen bir deneyim yaşanabilir. Ayrıca, Bing istemci kimliğini kullanarak web sonuçlarını istemci kimliğinin arama geçmişine uyarlayabilir ve bu sayede kullanıcıya daha zengin bir deneyim sağlayabilir.<br /><br /> Bing, istemci kimliği tarafından oluşturulan etkinliği analiz ederek sonuç derecelendirmelerini geliştirmeye yardımcı olması için de bu üst bilgiyi kullanabilir. İlgi geliştirmeleri Bing API'lerinin daha kaliteli sonuçlar vermesine yardımcı olur ve böylelikle API tüketicisi için daha yüksek tıklama oranları getirir.<br /><br /> **ÖNEMLİ:** İsteğe bağlı olsa da, bu üst bilgi gerekli dikkate almanız gerekir. Aynı son kullanıcı ile cihaz bileşimi için birden çok istekte aynı istemci kimliğini kullanıldığında, 1) API tüketicisi tutarlı bir kullanıcı deneyimi elde eder ve 2) Bing API'lerinden daha kaliteli sonuçlar alındığından tıklama oranları daha yüksek olur.<br /><br /> Bu üst bilgi için geçerli olan temel kullanım kuralları şunlardır:<br /><ul><li>Cihazda uygulamanızı kullanan her kullanıcının Bing tarafından oluşturulan benzersiz bir istemci kimliği olmalıdır.<br /><br/>İsteğe bu üst bilgiyi eklemezseniz, Bing bir kimlik oluşturur ve bu kimliği X-MSEdge-ClientID yanıt üst bilgisinde döndürür. İsteğe bu üst bilgiyi EKLEMEMENİZ gereken tek durum, söz konusu cihazda kullanıcının uygulamanızı ilk kez kullanmasıdır.<br /><br/></li><li>Cihazda uygulamanızın bu kullanıcı için yaptığı her Bing API'si isteğinde istemci kimliğini kullanın.<br /><br/></li><li>**DİKKAT:** Bu istemci kimliği için herhangi bir authenticatable kullanıcı hesabı bilgisi değişkenlerinden değil emin olmanız gerekir.</li><br/><li>İstemci kimliğinin kalıcı olmasını sağlayın. Tarayıcı uygulamasında kimliği kalıcı hale getirmek için, tüm oturumlarda kimliğin kullanmasını sağlayacak bir kalıcı HTTP tanımlama bilgisi kullanın. Oturum tanımlama bilgisi kullanmayın. Mobil uygulamalar gibi diğer uygulamalarda, kimliği kalıcı hale getirmek için cihazın kalıcı depolamasını kullanın.<br /><br/>Kullanıcı o cihazda uygulamanızı yeniden kullandığında, kalıcı hale getirdiğiniz istemci kimliğini alın.</li></ul><br /> **NOT:** Bing yanıtlarını olabilir veya bu başlığı içermeyebilir. Yanıt bu üst bilgiyi içeriyorsa, istemci kimliğini yakalayın ve o cihazda kullanıcı için bunu izleyen tüm Bing isteklerinde onu kullanın.<br /><br /> **NOT:** X MSEdge ClientID eklerseniz, istekte tanımlama bilgisi içermemelidir.|  
|<a name="clientip" />X-MSEdge-ClientIP|İsteğe bağlı istek üst bilgisi.<br /><br /> İstemci cihazının IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgisini kullanarak güvenli arama davranışını saptar.<br /><br /> **NOT:** İsteğe bağlı olsa da, her zaman bu üst bilgi ve X-Search-Location üst bilgisini belirtmeniz önerilir.<br /><br /> Adresi karartmayın (örneğin, son sekiz karakteri 0'la değiştirerek). Adresin karartılması, cihazın gerçek konumuna yakın olmayan bir konum sonucu verir ve bu da Bing'in hatalı sonuçlar sağlamasına yol açabilir.|  
|<a name="location" />X-Search-Location|İsteğe bağlı istek üst bilgisi.<br /><br /> İstemcinin coğrafi konumunu açıklayan noktalı virgülle sınırlanmış anahtar/değer çifti listesi. Bing konum bilgisini kullanarak güvenli arama davranışını saptar ve ilgili yerel içeriği döndürür. Anahtar/değer çiftini \<anahtar\>:\<değer\> olarak belirtin. Aşağıda, kullanıcının konumunu belirtmek için kullandığınız anahtarlar gösterilir.<br /><br /><ul><li>LAT&mdash;derece cinsinden istemcinin konumun enlem. Enlem -90,0 değerinden büyük veya bu değere eşit ve +90,0 değerinden küçük veya bu değere eşit olmalıdır. Negatif değerler güney enlemlerini ve pozitif değerler de kuzey enlemlerini gösterir.<br /><br /></li><li>uzun&mdash;derece cinsinden istemcinin konumun boylam. Boylam -180,0 değerinden büyük veya bu değere eşit ve +180,0 değerinden küçük veya bu değere eşit olmalıdır. Negatif değerler batı boylamlarını ve pozitif değerler de doğu boylamlarını gösterir.<br /><br /></li><li>RE&mdash; koordinatları yatay doğruluğunu belirten RADIUS, ölçümleri içinde. Cihazın konum hizmeti tarafından döndürülen değeri geçirin. Normalde değerler GPS/Wi-Fi için 22 m, baz istasyonu triangülasyonu için 380 m ve ters IP araması için 18.000 m'dir.<br /><br /></li><li>TS&mdash; istemci konumunda ne zaman, UTC UNIX zaman damgası. (UNIX zaman damgası 1 Ocak 1970'den başlayarak saniye sayısıdır.)<br /><br /></li><li>head&mdash;İsteğe bağlı. İstemcinin göreli seyahat yönü. Gerçek kuzeye göre saat yönünün tersine 0 ile 360 derece arasında bir seyahat yönü belirtin. Bu anahtarı ancak `sp` anahtarı sıfırdan farklı bir değer olduğunda belirtin.<br /><br /></li><li>SP&mdash; istemci cihazı dolaşan saniye başına metre olarak yatay hız (hızlı).<br /><br /></li><li>alt&mdash; metre olarak bir istemci cihazının yüksekliği.<br /><br /></li><li>are&mdash;İsteğe bağlı. Koordinatların dikey doğruluğunu belirten metre cinsinden yarıçap. Varsayılan olarak 50 kilometre RADIUS. Bu anahtarı ancak `alt` anahtarı belirttiğiniz durumda belirtin.<br /><br /></li></ul> **NOT:** Bu anahtar isteğe bağlıdır, ancak daha doğru konuma sonucu olan sağlayan daha fazla bilgi.<br /><br /> **NOT:** Her zaman kullanıcının coğrafi konumu belirtmek için önerilir. İstemcinin IP adresi kullanıcının fiziksel konumunu doğru yansıtmıyorsa (örneğin istemci VPN kullanıyorsa), konumun belirtilmesi özellikle önemlidir. En iyi sonuçları elde etmek için, bu üst bilgiyi ve X-MSEdge-ClientIP üst bilgisini eklemelisiniz; ama en azından bu üst bilgiyi eklemeniz gerekir.|

> [!NOTE] 
> Kullanım Koşulları'nın, bu üst bilgilerin kullanımıyla ilgili olanlar da dahil olmak üzere tüm ilgili yasalara uymayı gerektirdiğini unutmayın. Örneğin, Avrupa gibi bazı yasama bölgelerinde kullanıcı cihazlarına izleme cihazları takmadan önce kullanıcının iznini almak gerekir.
  

## <a name="query-parameters"></a>Sorgu parametreleri  
Aşağıdaki sorgu parametreleri istek içerebilir. Gerekli Parametreler için gerekli sütununa bakın. URL gereken sorgu parametrelerine kodlayın.  
  
  
|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|
|<a name="count" />Sayısı|Sonuçları döndürmek için tarafından belirtilen dizin ile başlayan sayısını `offset` parametresi.|String|Hayır|   
|<a name="localCategories" />localCategories|Arama iş kategoriye göre tanımlayan seçenekleri listesi.  Bkz: [yerel iş kategorilerde arama](local-categories.md)|String|Hayır|  
|<a name="mkt" />mkt|Sonuçların geldiği pazar. <br /><br />Olası Pazar değerler listesi için Pazar kodları bölümüne bakın.<br /><br /> **NOT:** Yerel iş arama API'si şu anda yalnızca tr destekler-bize pazara çıkma sürelerini ve dili.<br /><br />|String|Evet|
|<a name="offset"/>uzaklık|Tarafından belirtilen sonuçları başlamak için dizini `count` parametresi.|Integer|Hayır|  
|<a name="query" />q|Kullanıcı arama terimi.|String|Hayır|  
|<a name="responseformat" />responseFormat|Yanıt için kullanılacak medya türü. Büyük küçük harf duyarsız olası değerler şunlardır:<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> JSON varsayılandır. JSON hakkında bilgi yanıt içerdiğini nesneleri için bkz. [yanıt nesneleri](#response-objects).<br /><br />  JsonLd belirtirseniz, yanıt gövdesi, arama sonuçlarını içeren JSON-LD nesneler içerir. JSON-LD hakkında daha fazla bilgi için bkz. [JSON-LD](https://json-ld.org/).|String|Hayır|  
|<a name="safesearch" />safeSearch|Yetişkinlere yönelik içeriği filtrelemek için kullanılan bir filtre. Aşağıdakiler, büyük/küçük harfe duyarlı olmayan olası filtre değerleridir.<br /><ul><li>Kapalı&mdash;yetişkinlere yönelik metin, görüntü veya video Web sayfalarının döndürür.<br /><br/></li><li>Orta&mdash;yetişkinlere yönelik metin ancak yetişkin değil görüntü veya video Web sayfalarının döndürür.<br /><br/></li><li>Katı&mdash;yetişkinlere yönelik metin, görüntü veya video Web sayfalarının döndürmüyor.</li></ul><br /> Varsayılan ayar Moderate değeridir.<br /><br /> **NOT:** İstek bir pazar geliyorsa gerektiren bu Bing'in yetişkinlere yönelik ilke `safeSearch` ayarlanır Strıct için Bing yoksayar `safeSearch` değeri ve Strıct kullanır.<br/><br/>**NOT:** Kullanırsanız `site:` sorgu işleci yanıt ne bakılmaksızın yetişkinlere yönelik içerik içerebilir fırsat yok `safeSearch` sorgu parametresi ayarlanır. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın. |String|Hayır|  
|<a name="setlang" />setLang|Kullanıcı arabirimi dizelerinde kullanılacak dil. Dili belirtirken ISO 639-1 2 harfi dil kodunu kullanın. Örneğin, Türkçe için dil kodu TR'dir. Varsayılan değer EN (İngilizce) ayarıdır.<br /><br /> İsteğe bağlı olsa da, her zaman dil belirtmelisiniz. Kullanıcı tarafından kullanıcı arabirimi dizelerinin farklı dilde görüntülenmesi istenmediği sürece, normalde `setLang` parametresini `mkt` parametresiyle aynı dile ayarlarsınız.<br /><br /> Bu parametre ve [Accept-Language](#acceptlanguage) üst bilgisi karşılıklı olarak birbirini dışlar. İkisini birlikte belirtmeyin.<br /><br /> Kullanıcı arabirimi dizesi, kullanıcı arabiriminde etiket olarak kullanılan dizedir. JSON yanıt nesnelerinde çok az kullanıcı arabirimi dizesi vardır. Ayrıca, yanıt nesnelerinde Bing.com özelliklerine yönelik bağlantılar da belirtilen dildedir.|String|Hayır| 


## <a name="response-objects"></a>Yanıt nesneleri  
Yanıt içerebilecek JSON yanıt nesneleri şunlardır: İstek başarılı olursa, en üst düzey nesnedir yanıt [SearchResponse](#searchresponse) nesne. İstek başarısız olursa, en üst düzey nesnedir [ErrorResponse](#errorresponse) nesne.


|Object|Açıklama|  
|------------|-----------------|  
|[Yerleştir](#place)|Bir restoran veya otel gibi yerel bir iş hakkında bilgileri tanımlar.|  

  
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

  
  
### <a name="license"></a>Lisans  
Altında bir metin veya resim kullanılabilir lisans tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|name|Lisans adı.|String|  
|url|Kullanıcı Lisansı hakkında daha fazla bilgi edinebileceğiniz bir Web sitesi URL'si.<br /><br /> Köprü oluşturmak için adını ve URL'sini kullanın.|String|  


### <a name="link"></a>Bağla  
Köprü bileşenlerinin tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_type|Tür ipucu.|String|  
|metin|Görünen metin.|String|  
|url|BİR URL. URL'yi kullanın ve metin, köprü oluşturmak için görüntüler.|String|  
  


  
### <a name="organization"></a>Kuruluş  
Bir yayımcı olarak tanımlar.  
  
Bir yayımcı adının veya Web sitesi veya her ikisini sağlayabilir unutmayın.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|name|Yayımcının adı.|String|  
|url|Yayımcının Web sitesi URL'si.<br /><br /> Yayımcının Web sitesi sağlamayabilir unutmayın.|String|  
  
  

### <a name="place"></a>Yerleştir  
Bir restoran veya otel gibi yerel bir iş hakkında bilgileri tanımlar.  
  
|Ad|Değer|Tür|  
|----------|-----------|----------|  
|_type|Tür ipucu, aşağıdakilerden birini ayarlanabilir:<br /><br /><ul><li>Otel</li><li>LocalBusiness<br /></li><li>Restoran</ul><li>|String|  
|adres|Varlığın bulunduğu, posta adresi.|PostalAddress|  
|entityPresentationInfo|Varlığın türü belirlemek için kullanabileceğiniz ipuçları gibi varlık hakkında ek bilgi. Örneğin, bir restoran veya otel olup. `entityScenario` Alan ListItem için ayarlanır.|entityPresentationInfo|  
|name|Varlığın adı.|String|  
|Telefon|Varlığın telefon numarası.|String|  
|url|Varlığın Web sitesi URL'si.<br /><br /> Varlığın adı ile birlikte bu URL'yi kullanın köprü tıklandığında oluşturan kullanıcıyı varlığın Web sitesine götürür.|String|  
|webSearchUrl|Burası Bing'in arama sonucunu URL'si.|String| 
  
  
### <a name="querycontext"></a>QueryContext  
Bing istek için kullanılan sorgu bağlamı tanımlar.  
  
|Öğe|Açıklama|Tür|  
|-------------|-----------------|----------|  
|adultIntent|Belirtilen sorgu yetişkinlere yönelik sonuçlar olup olmadığını belirten bir Boole değeri. Değer **true** yetişkinlere yönelik sonuçlar; sorgu varsa, aksi takdirde, **false**.|Boolean|  
|alterationOverrideQuery|Orijinal dizeyi kullanmak için Bing zorlamak için kullanılacak sorgu dizesi. Örneğin, sorgu dizesi ise *downwind saling*, geçersiz kılma sorgu dizesi olacaktır *+ downwind saling*. Sonuçlanan sorgu dizesini kodlayın unutmayın *% 2Bsaling + downwind*.<br /><br /> Bu alan, yalnızca özgün sorgu dizesi bir yazım hatası içeriyorsa dahildir.|String|  
|alteredQuery|Bing tarafından sorguyu gerçekleştirmek için kullanılan sorgu dizesi. Bing yazım hatalarını özgün sorgu dizesini içerdiği değiştirilen sorgu dizesini kullanır. Örneğin, sorgu dizesi ise `saling downwind`, değiştirilen sorgu dizesi olacaktır `sailing downwind`.<br /><br /> Bu alan, yalnızca özgün sorgu dizesi bir yazım hatası içeriyorsa dahildir.|String|  
|askUserForLocation|Bing doğru sonuçlar sağlamak için kullanıcının konumuna isteyip istemediğini gösteren bir Boole değeri. Kullanarak kullanıcının bulunduğu konum belirttiyseniz [X MSEdge Clientıp](#clientip) ve [X arama konumu](#location) üst bilgiler, bu alan yoksayabilirsiniz.<br /><br /> "Günün hava durumu" veya "kullanıcının konumuna doğru sonuçlar sağlamak için gereken Yakınımdaki restoranlar" gibi konumu kullanan sorgular için bu alan ayarlanır **true**.<br /><br /> ' % S'konum (örneğin, "Seattle hava") içeren konumu kullanan sorgular için bu alan kümesine **false**. Bu alan ayrıca kümesine **false** konumu "gibi en iyi satıcılar" uyumlu olmayan sorgular.|Boolean|  
|originalQuery|İstekte belirtilen sorgu dizesi.|String|  

### <a name="identifiable"></a>Tanımlama

|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|kimlik|Bir kaynak tanımlayıcısı|String|
 
### <a name="rankinggroup"></a>RankingGroup
Tanımlar grubu bir arama sonuçları, aşağıdaki gibi mainline.

|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|items|Grup içinde görüntülemek için arama sonuçları listesi.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Görüntülenecek bir arama sonucu öğesi tanımlar.

|Ad|Değer|Tür|  
|-------------|-----------------|----------|
|resultIndex|Görüntülenecek yanıtında öğenin sıfır tabanlı dizini. Bu alan öğe içermiyorsa, yanıt tüm öğeleri görüntüler. Örneğin, haber yanıt tüm haber makalelerini görüntüler.|Integer|
|answerType|Görüntülenecek öğe içeren yanıtı. Örneğin, haber.<br /><br />Yanıt SearchResponse nesnesinde bulunacak türünü kullanın. Türü bir SearchResponse alan adıdır.<br /><br /> Ancak, yalnızca bu nesne değeri alanı varsa yanıt türünü kullanın. Aksi takdirde, yoksayın.|String|
|textualIndex|Görüntülenecek textualAnswers yanıt dizini.| İşaretsiz tamsayı|
|value|Görüntülenecek yanıt veya öğeyi görüntülemek için bir yanıt tanımlayan kimliği. Kimliği bir yanıt tanımlıyorsa, yanıtın tüm öğeleri görüntüler.|Tanımlama|

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
|_type|Tür ipucu SearchResponse için ayarlanır.|String|  
|Basamak|Arama sorgusu için uygun olan varlıklar listesi.|JSON nesnesi|  
|QueryContext|Bing istek için kullanılan sorgu dizesi içeren bir nesne.<br /><br /> Bu nesne, kullanıcı tarafından girildiği gibi sorgu dizesi içerir. Ayrıca, sorgu dizesi bir yazım hatası içeriyorsa, Bing sorgu için kullanılan bir değiştirilen sorgu dizesi de içerebilir.|[QueryContext](#querycontext)|  


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
- [Yerel iş arama hızlı başlangıç](quickstarts/local-quickstart.md)
- [Yerel iş arama Java hızlı başlangıç](quickstarts/local-search-java-quickstart.md)
- [Yerel iş arama düğümü hızlı başlangıç](quickstarts/local-search-node-quickstart.md)
- [Yerel iş arama Python hızlı başlangıç](quickstarts/local-search-python-quickstart.md)
