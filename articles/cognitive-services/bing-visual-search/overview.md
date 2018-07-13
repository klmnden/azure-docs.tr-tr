---
title: Bing görsel arama API'sine genel bakış | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Ayrıntıları veya benzer resimler veya alışveriş kaynakları gibi bir görüntü ile ilgili Öngörüler alma işlemi gösterilmektedir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: aa563d89b1834f5be952f13c31a2451d809709b1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006538"
---
# <a name="what-is-bing-visual-search-api"></a>Bing Görsel Arama API’si nedir?

Bing görsel arama API'sine Bing.com/images üzerinde gösterilen görüntü ayrıntılarını için benzer bir deneyim sağlar. Görsel arama ile resim karşıya yükleyin ve geri görsel açıdan benzer resimler, alışveriş kaynakları, görüntü ve daha fazlasını içeren Web sayfalarının görüntü ile ilgili Öngörüler edinin. Bir görüntü yüklemek yerine, görüntüleri arama sonuçlarındaki bir görüntüden aldığınız bir ınsights belirteci de sağlayabilirsiniz (bkz [Bing resimler API](../bing-image-search/overview.md)).

Görsel arama ünlüleri, anıtları ve yer işareti, resim, ev mobilyaları, biçimde, ürünler, karakter tanıma (OCR) ve daha fazla tanımlayabilirsiniz.

Görsel arama bulmanıza olanak tanır ınsights aşağıda verilmiştir.

- Görsel olarak benzer resimler&mdash;giriş görüntünün görsel olarak benzer resimler listesi
- Görsel olarak benzer ürünleri&mdash;giriş görüntüde verilen ürün görsel olarak benzer ürünleri içeren görüntülerin listesini
- Kaynakları alışveriş&mdash;yerleri burada satın alabilirsiniz giriş görüntüde verilen öğe listesi
- İlgili aramalar&mdash;diğerlerinden veya, tarafından yapılan ilgili aramalar bir listesini resmin içeriğini temel alan
- Görüntü içeren Web sayfaları&mdash;girdi görüntüsünün içeren Web sayfalarının listesi
- Tarif&mdash;giriş görüntüde verilen tabağın için tarif içeren Web sayfalarının listesi

Bu Öngörüler yanı sıra, görsel arama de giriş görüntüden türetilmiş koşulları (etiketler) farklı bir dizi döndürür. Bu etiketler, kullanıcıların görüntüde bulunan kavramlarını keşfedin izin verin. Örneğin, girdi görüntüsünün ünlü bir athlete ise, etiketler biri athlete adını olabilir, başka bir etiket Spor olabilir. Veya bir apple pasta girdi görüntüsünün ise kullanıcılar ilgili kavramları gözatabilirsiniz etiketleri Apple pasta, pasta, Desserts olabilir.

Görsel arama sonuçları, sınırlayıcı kutular bölgeler için görüntüde faiz de içerir. Örneğin, görüntüyü birkaç ünlüleri içeriyorsa, sonuçları her biri, görüntüde tanınan ünlüleri için sınırlayıcı kutular içerebilir. Veya bir ürün veya görüntüde giysi Bing tanır, tanınan bir ürün veya giysi öğe için bir sınırlayıcı kutu sonucu içerebilir.

> [!IMPORTANT]
> / Resimler/Ayrıntılar uç noktasına kullanıyorsanız [resim öngörüleri alın](../bing-image-search/image-insights.md), çünkü daha kapsamlı içgörüler sağlar görsel arama kullanmanız için kodunuzu güncelleştirmeniz gerekir.


## <a name="the-request"></a>İstek

Bir görüntü ile ilgili öngörüleri almak için Seçenekler şunlardır: 

- Bir görüntüden birine önceki bir çağrı alma bir ınsights belirteci Gönder [Bing resimler API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) uç noktaları
- Görüntü URL'sini gönderin
- Bir görüntü (ikili) yükleyin


Aşağıda, bir görüntü belirteç veya URL görsel arama gönderirseniz, POST gövdesinde içermelidir JSON nesnesi gösterilmektedir. 

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

`imageInfo` Nesne ya da içermelidir `url` veya `imageInsightsToken` alan ancak ikisine birden değil. Ayarlama `url` Internet erişilebilir görüntünün URL'sini alanı. Desteklenen en yüksek görüntü boyutu 1 MB'dir.

`imageInsightsToken` Bir ınsights belirtecine ayarlamanız gerekir. Insights belirteç almak için Bing resim API'sini çağırın. Yanıta bir listesini içeren `Image` nesneleri. Her `Image` nesne içeren bir `imageInsightsToken` belirteç içeren alan.

`cropArea` Alandır isteğe bağlı. Kırpma alanının üst, sol üst köşedeki ve alt, bir ilgi bölgesi sağ köşesindeki belirtir. 0.0 ile 1.0 aralığında değerleri belirtin. Toplam genişlik ve yükseklik yüzdesi değerlerdir. Örneğin, yukarıdaki örnek sağ yarısında görüntünün ilgi bölgesi işaretler. İlgi bölgesi ınsights isteği sınırlamak istiyorsanız bunu içerir.

`filters` Nesne içeren bir site filtre (bkz `site` alanı) için belirli bir etki alanı benzer resimler ve benzer ürünleri sonuçları kısıtlamak için kullanabilirsiniz. Örneğin bir Surface Book görüntüyse ayarlayabilirsiniz `site` www.microsoft.com için. 

Görüntüyü yerel bir kopyasını ilgili Öngörüler almak istiyorsanız, görüntüyü ikili veri olarak yükleyin.

Gönderinin gövdesi içinde bu seçenekleri dahil olmak üzere hakkında daha fazla ayrıntı için bkz [içerik form türleri](#content-form-types).


### <a name="endpoint"></a>Uç Nokta

Görsel arama uç noktadır: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

İstekleri yalnızca HTTP POST istekleri gönderilmesi gerekir. 


### <a name="query-parameters"></a>Sorgu parametreleri

İsteğiniz belirtmelidir sorgu parametreleri şunlardır: En azından, içermelidir `mkt` sorgu parametresi.

|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|  
|<a name="cc" />Cc|Sonuçları nereden geldiğini ülkenin 2 karakterli ülke kodu.<br /><br /> Bu parametre ayarlarsanız, ayrıca belirtmelisiniz [Accept-Language](#acceptlanguage) başlığı. Bing diller listesinden bulur ve sonuçları döndürmek için Pazar belirlemek için belirtin ülke kodu ile dil birleştirir ilk desteklenen dilini kullanır. Dillerin listesi desteklenen bir dil içermiyorsa, Bing en yakın dil ve istek destekleyen Pazar bulur. Ya da bir toplu kullanın veya pazar sonuçları yerine belirtilen varsayılan olabilir.<br /><br /> Bu sorgu parametresini kullanmanız gerekir ve `Accept-Language` sorgu parametresi birden çok dil belirtirseniz; Aksi takdirde, kullanmanız gereken `mkt` ve `setLang` sorgu parametreleri.<br /><br /> Bu parametre ve [mkt](#mkt) sorgu parametresi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.|Dize|Hayır|  
|<a name="mkt" />Mkt|Sonuçları nereden geldiğini Pazar. <br /><br /> **Not:** biliniyorsa marketiyle ilgili her zaman belirtmek için önerilir. Pazar belirtme isteği yönlendirmek ve uygun ve en iyi bir yanıt Bing yardımcı olur.<br /><br /> Bu parametre ve [cc](#cc) sorgu parametresi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.|Dize|Evet|  
|<a name="safesearch" />safeSearch|Yetişkinlere yönelik içeriğe filtrelemek için kullanılan bir filtre. Olası büyük küçük harf duyarsız filtre değerleri şunlardır.<br /><ul><li>Kapalı&mdash;yetişkinlere yönelik metin veya görüntü ile Web sayfalarını döndürür.<br /><br/></li><li>Orta&mdash;yetişkinlere yönelik metin ancak yetişkin değil görüntüleri Web sayfalarının döndürür.<br /><br/></li><li>Katı&mdash;yetişkinlere yönelik metin veya görüntü ile Web sayfalarını döndürmüyor.</li></ul><br /> Orta varsayılandır.<br /><br /> **Not:** isteği bir pazar geliyorsa, gerektiren bu Bing'in yetişkinlere yönelik ilke `safeSearch` sıkı ayarlanması, Bing yoksayar `safeSearch` değeri ve Strıct kullanır.<br/><br/>**Not:** kullanırsanız `site:` sorgu işleci yanıt ne bakılmaksızın yetişkinlere yönelik içerik içerebilir fırsat yok `safeSearch` sorgu parametresi ayarlanır. Kullanım `site:` yalnızca sitedeki içerikleri haberdar ve senaryonuz yetişkinlere yönelik içeriğe olasılığını destekler. |Dize|Hayır|  
|<a name="setlang" />setLang|Kullanıcı arabirimi dizeleri için kullanılacak dili. ISO 639-1 2 harfli dil kodunu kullanarak dili belirtin. Örneğin, İngilizce dil kodu tr ' dir. TR (Türkçe) varsayılandır.<br /><br /> İsteğe bağlı olsa da, her zaman dil belirtmeniz gerekir. Genellikle, ayarladığınız `setLang` tarafından belirtilen ile aynı dile `mkt` kullanıcının farklı bir dilde görüntülenen kullanıcı arabirimi dizeleri istediği sürece.<br /><br /> Bu parametre ve [Accept-Language](#acceptlanguage) üstbilgi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.<br /><br /> Bir kullanıcı arabirimi dizesi kullanıcı arabiriminde bir etiket olarak kullanılan bir dizedir. JSON yanıtı nesneleri birkaç kullanıcı arabirimi dizeleri vardır. Ayrıca, herhangi bir bağlantı Bing.com yanıt nesnelerinin özelliklerinde belirtilen dil uygulayın.|Dize|Hayır| 

### <a name="headers"></a>Üst bilgiler

İsteğiniz belirtmelidir üst bilgiler verilmiştir. Content-Type ve Ocp-Apim-Subscription-Key üstbilgileri yalnızca gerekli üst bilgileri olan ancak kullanıcı aracısı, X MSEdge ClientID, X MSEdge Clientıp ve X arama konumu da içermelidir.


|Üst bilgi|Açıklama|  
|------------|-----------------|  
|<a name="acceptlanguage" />Kabul dil|İsteğe bağlı isteği üstbilgisi.<br /><br /> Kullanıcı arabirimi dizeleri için kullanılacak dil virgülle ayrılmış listesi. Tercih sırasına göre azalan düzende listesidir. Beklenen biçim'dahil olmak üzere daha fazla bilgi için bkz. [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Bu üst bilgi ve [setLang](#setlang) sorgu parametresi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.<br /><br /> De belirtmeniz gerekir bu başlığı ayarlarsanız [cc](#cc) sorgu parametresi. Pazar için sonuçlar döndürecek şekilde belirlemek için Bing listeden bulur ve bununla birleştirir ilk desteklenen dil kullanan `cc` parametre değeri. Liste, desteklenen bir dil içermiyorsa, Bing en yakın dil ve istek destekleyen Pazar bulur veya pazar sonuçları için varsayılan veya bir toplanmış kullanır. Bing kullanılan Pazar belirlemek için BingAPIs pazara açılma üstbilgi bakın.<br /><br /> Bu üstbilgiyi kullanır ve `cc` birden çok dil belirtirseniz sorgu parametresi. Aksi takdirde kullanın [mkt](#mkt) ve [setLang](#setlang) sorgu parametreleri.<br /><br /> Bir kullanıcı arabirimi dizesi kullanıcı arabiriminde bir etiket olarak kullanılan bir dizedir. JSON yanıtı nesneleri birkaç kullanıcı arabirimi dizeleri vardır. Yanıt nesnelerinin Bing.com özelliklerinde herhangi bir bağlantı belirtilen dili uygulayın.|  
|<a name="contenttype" />İçerik türü|Gerekli isteği üstbilgisi.<br /><br />Multipart/form-data'için ayarlanmış olması gerekir ve bir sınır parametresi içerir (örneğin, multipart/form-data; sınır =\<sınır dize\>). Daha fazla bilgi için [içerik form türleri](#content-form-types).
|<a name="market" />BingAPIs-Pazar|Yanıtı üstbilgisi.<br /><br /> İstek tarafından kullanılan Pazar. Form \<languageCode\>-\<countryCode\>. Örneğin, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Yanıtı üstbilgisi.<br /><br /> İsteğinin ayrıntılarını içeren günlük girdisi kimliği. Bir hata oluştuğunda, bu kimliği yakalama Belirlemek ve sorunu çözmek mümkün değilse, bu kimliği yanı sıra destek ekibinin sağladığı diğer bilgiler içerir.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Gerekli isteği üstbilgisi.<br /><br /> Bu hizmet için RMS'ye kaydolurken aldığınız abonelik anahtarını [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragması|İsteğe bağlı bir istek üst bilgisi<br /><br /> Varsayılan olarak, önbelleğe alınmış içeriği varsa Bing döndürür. Önbelleğe alınan içerik döndürmesini Bing önlemek için Pragma üstbilgisi no-cache olarak ayarlayın (örneğin, Pragması: no-cache).
|<a name="useragent" />Kullanıcı Aracısı|İsteğe bağlı isteği üstbilgisi.<br /><br /> İsteğin kaynaklandığı kullanıcı aracısı. Bing, mobil kullanıcıların iyileştirilmiş bir deneyim sağlamak için Kullanıcı aracısını kullanır. İsteğe bağlı olsa da, her zaman bu üstbilgisi belirtmeniz önerilir.<br /><br /> Kullanıcı Aracısı, yaygın olarak kullanılan tüm tarayıcılar gönderen aynı dize olmalıdır. Kullanıcı aracıları hakkında daha fazla bilgi için bkz. [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Kullanıcı Aracısı dizeleri örnekleri aşağıda verilmiştir.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (uyumlu; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Dokunma; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 derleme/GINGERBREAD) AppleWebKit/533.1 (KHTML; ister Gecko) sürüm/4.0 mobil Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; Mac OS X gibi CPU iPhone OS 6_1) AppleWebKit/536.26 (KHTML; ister Gecko) mobil/10B142 iPhone4; BingWeb/3.03.1428.20120423 1<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Dokunma; RV:11.0) Gecko ister<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; Mac OS X gibi CPU işletim sistemi 7_0) AppleWebKit/537.51.1 (Gecko gibi KHTML) sürüm/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X MSEdge ClientID|İsteğe bağlı istek ve yanıt üst bilgisi.<br /><br /> Bing Bing API çağrıları arasında tutarlı bir davranış ile kullanıcılara sağlamak için bu üstbilgiyi kullanır. Bing sıklıkla yeni özellikler ve geliştirmeler uçuşlar ve istemci kimliği farklı uçuşlar trafiğinde atamak için bir anahtar olarak kullanır. Birden çok istekler genelinde bir kullanıcı için aynı istemci Kimliğini kullanmazsanız Bing için birden çok çakışan uçuşlar kullanıcı atayabilir. Birden çok çakışan uçuşlar için atanan bir tutarsız kullanıcı deneyimine neden olabilir. Örneğin, bir farklı uçuş ataması ilk değerinden ikinci isteği varsa deneyimi beklenmeyen olabilir. Bing web sonuçları, istemci için uygun hale getirmek için Ayrıca, istemci Kimliğini kullanabilirsiniz kimliğin arama geçmişi, kullanıcı için daha zengin bir deneyim sağlar.<br /><br /> Bing sonucu sonuçlarımızda bir istemci kimliği tarafından oluşturulan bir etkinlik analiz ederek geliştirmeye yardımcı olmak için bu üst bilgi de kullanır. İlgi geliştirmeleri, Bing API'leri ve sırayla tıklama etkinleştirir daha yüksek ücretler için API tüketici teslim sonuçlarının daha iyi bir nitelikle yardımcı olur.<br /><br /> **Önemli:** isteğe bağlı olsa da, bu üst bilgi gerekli göz önünde bulundurmanız gerekir. İstemci kimliği aynı son kullanıcı ve cihaz birleşimi için birden çok istekler genelinde kalıcı tutarlı bir kullanıcı deneyimi ve daha iyi sonuçlar kalitesini aracılığıyla 2) daha yüksek tıklama oranları Bing API'lerinden almak 1) API tüketici sağlar.<br /><br /> Bu üst bilgiye Uygula temel kullanım kuralları aşağıda verilmiştir.<br /><ul><li>Cihazda uygulamanızın kullandığı her bir kullanıcı bir benzersiz olmalıdır, Bing oluşturulan istemci kimliği.<br /><br/>İstekte bu başlığı eklemezseniz, Bing bir kimlik üretir ve X MSEdge ClientID yanıt üst bilgisinde döndürür. Bir istekte bu üst bilgi içermemelidir yalnızca bir kez ilk kez kullanıcı o cihazda uygulamanızın kullandığı ' dir.<br /><br/></li><li>**DİKKAT:** bu istemci kimliği için herhangi bir kimliği doğrulanmış kullanıcı hesabı bilgisi değişkenlerinden değil emin olmanız gerekir.</li><li>İstemci Kimliğini, cihazda bu kullanıcı için uygulamanıza yaptığı her Bing API isteği için kullanın.<br /><br/></li><li>Kalıcı istemci kimliği. Bir tarayıcı uygulamasında kimliği kalıcı hale getirmek için kalıcı bir HTTP tanımlama bilgisi kimliği tüm oturumlarda kullanıldığından emin olmak için kullanın. Oturum tanımlama bilgisinin kullanmayın. Mobil uygulamalar gibi diğer uygulamalar için cihazın kalıcı depolama kimliği kalıcı hale getirmek için kullanın.<br /><br/>Kullanıcı, cihazda uygulamanızın kullandığı bir sonraki açışınızda, kalıcı bir istemci kimliği alın.</li></ul><br /> **Not:** Bing yanıtlarını olabilir veya bu başlığı içermeyebilir. Bu üst bilgi yanıtı içeriyorsa, istemci kimliği yakalamak ve o cihazdaki kullanıcı için tüm sonraki Bing istekler için kullanın.<br /><br /> **Not:** X MSEdge ClientID eklerseniz, tanımlama bilgilerinin istekte içermemesi gerekir.|  
|<a name="clientip" />X MSEdge Clientıp|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemci cihazı IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgileri güvenli arama davranışını belirlemek için kullanır.<br /><br /> **Not:** isteğe bağlı olsa da, her zaman bu üst bilgi ve X-Search-Location üst bilgisini belirtmeniz önerilir.<br /><br /> Adres (örneğin, son sekizli 0 olarak değiştirerek) karartmak değil. Her yerden gerçek cihazın konumuna olmaması konumu adresi sonuçlarında obfuscating, hangi hatalı sonuçlar sunan Bing içinde neden olabilir.|  
|<a name="location" />X arama konumu|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemcinin coğrafi konumunu tanımlayan anahtar/değer çiftleri noktalı virgülle ayrılmış listesi. Güvenli arama davranışlarını belirlemek ve ilgili yerel içeriğini döndürmek için Bing konum bilgileri kullanır. Anahtar/değer çifti olarak belirtmek \<anahtarı\>:\<değer\>. Kullanıcının konumunu belirtmek üzere kullanacak anahtarlar şunlardır:<br /><br /><ul><li>LAT&mdash;gerekli. Derece cinsinden istemcinin konumun enlem. Büyüktür veya eşittir-90.0 enlem olmalıdır ve +90.0 küçüktür veya eşittir. Güney latitudes negatif değerleri göstermek ve Kuzey latitudes pozitif değerleri gösterir.<br /><br /></li><li>uzun&mdash;gerekli. Derece cinsinden istemcinin konumun boylam. Büyüktür veya eşittir-180.0 boylam olmalıdır ve +180.0 küçüktür veya eşittir. Batı longitudes negatif değerleri göstermek ve Doğu longitudes pozitif değerleri gösterir.<br /><br /></li><li>RE&mdash;gerekli. Koordinatları yatay doğruluğunu belirten RADIUS, ölçümleri içinde. Cihazın konum hizmeti tarafından döndürülen değeri geçirin. GPS/Wi-Fi için 22 m, hücre tower Üçlü 380 m ve 18, 000 m ters IP araması için normal değerler olabilir.<br /><br /></li><li>TS&mdash;isteğe bağlı. İstemci konumu ne zaman, UTC UNIX zaman damgası. (UNIX zaman damgası 1 Ocak 1970'ten beri geçen saniye sayısı sayısıdır.)<br /><br /></li><li>HEAD&mdash;isteğe bağlı. İstemcinin göreli başlığını veya seyahat yönü. 0 ile göreli true Kuzey saat yönünde sayım 360 derece olarak seyahat yönünü belirtin. Bu anahtar yalnızca, belirtin `sp` anahtardır sıfır.<br /><br /></li><li>SP&mdash;isteğe bağlı. İstemci cihazı dolaşan saniye başına metre olarak yatay hız (hızlı).<br /><br /></li><li>alt&mdash;isteğe bağlı. İstemci aygıtı ölçümleri yüksekliği.<br /><br /></li><li>olan&mdash;isteğe bağlı. Koordinatları dikey doğruluğunu belirten RADIUS, ölçümleri içinde. Yalnızca belirtirseniz, bu anahtarı belirtirsiniz `alt` anahtarı.<br /><br /></li></ul> **Not:** anahtarların birçok isteğe bağlı, ancak daha doğru konuma sonucu olan sağlayan daha fazla bilgi.<br /><br /> **Not:** isteğe bağlı olsa da, her zaman kullanıcının coğrafi konumu belirtmek için önerilir. Konum (örneğin, istemci VPN kullanıyorsa) istemcinin IP adresini kullanıcının fiziksel konum doğru şekilde yansıtmaz durumunda özellikle önemlidir. En iyi sonuçlar için bu başlığı ve X MSEdge Clientıp başlığı içermelidir, ancak en az bu başlığı içermelidir.|

> [!NOTE] 
> Kullanım Koşulları ile ilgili bu üstbilgileri kullanımı dahil olmak üzere ilgili tüm yasalara uyumluluk gerekli olduğunu unutmayın. Örneğin, Avrupa gibi belirli daireleri de kullanıcı aygıtları üzerinde belirli izleme cihazları yerleştirme önce kullanıcı onayı almak için gereksinimi yoktur.


<a name="content-form-types" />

### <a name="content-form-types"></a>İçerik form türleri

Her istek, Content-Type üst bilgisi içermesi gerekir. Üst bilgi ayarlanmalıdır: multipart/form-data; sınır =\<sınır dize\>burada \<sınır dize\> sınır form verileri tanımlayan benzersiz ve donuk bir dizedir. Örneğin, sınır boundary_1234 abcd =.


Bir görüntü belirteç veya URL görsel arama gönderirseniz, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini içermelidir ve kendi `name` "knowledgeRequest" parametresi ayarlanmalıdır Hakkındaki ayrıntılar için `imageInfo` nesne, bkz: [istek](#the-request).


```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "url" : "https://contoso.com/2018/05/fashion/red.jpg"
    }
}

--boundary_1234-abcd--
```

Yerel bir görüntüyü karşıya yükleme, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini içermelidir. Kendi `name` parametresi ayarlanması gerekir "Görüntü" ve `filename` parametreyi bir dizeye ayarlayın. Content-Type üstbilgisi tüm yaygın olarak kullanılan resim mime türü için ayarlanabilir. Form içeriğini ikili görüntünün olur. Karşıya yükleyebilirsiniz en yüksek görüntü boyutu 1 MB'dir. En büyük genişliği veya yüksekliği 1.500 piksel olmalıdır veya daha az.


```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Karşıya yüklenen görüntünün bir bölgeyi belirtmek nasıl gösterir.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "cropArea" : {
            "top" : 0.2,
            "left" : 0.3,
            "bottom" : 0.7,
            "right" : 0.6
        }
    }
}

--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="image"
Content-Type: image/jpeg


ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```



### <a name="example-request"></a>Örnek istek

Bir görüntü belirteci ve ilgi bölgesi geçen tam görüntü ınsights istek gösterir. Bir önceki çağrıya /images/search ınsights belirteci alın.


```  
POST https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=en-us HTTP/1.1  
Content-Type: multipart/form-data; boundary=boundary_1234-abcd
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com 

--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.2,
            "bottom" : 0.7,
            "right" : 0.5
        }
    }
}

--boundary_1234-abcd--
```


## <a name="the-response"></a>Yanıt

Görüntü için kullanılabilir ınsights varsa, bir veya daha fazla yanıt içeren `tags` öngörüleri içerir. `image` Alan girdi görüntüsünün yönelik Öngörüler belirtecini içerir.

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {...},
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

`tags` Alan bir görünen ad ve eylemleri (Öngörüler) bir listesini içerir. Etiketlerinden birini içeren bir `displayName` boş dize olarak ayarlamak alanı. Bu etiket, görüntü, görsel açıdan benzer resimler ve görüntüde bulunan öğelerin alışveriş kaynaklar içeren Web sayfaları gibi varsayılan ınsights içerir. Görüntünün ilgi olduğundan, varsayılan ınsights etiket sınırlayıcı kutular bölgeleri için ilgilendiğiniz içermez.


```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {...},
        {...},
        {...},
        {...}
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

Varsayılan ınsights listesi için bkz. [varsayılan ınsights](./default-insights-tag.md).



Kalan etiketleri, kullanıcı ilginizi çekebilecek diğer bilgileri içerir. Örneğin, görüntünün metin içeriyorsa, etiketlerinden birini tanınan bir metni içeren bir TextResults Insight içerebilir. Veya, Bing görüntüde bir varlık (kişi, yer veya bir şey) tanıyorsa etiketlerinden birini varlığı belirlemek. Görsel arama, ayrıca giriş görüntüden türetilmiş koşulları (etiketler) farklı bir dizi döndürür. Bu etiketler, kullanıcıların görüntüde bulunan kavramlarını keşfedin izin verin. Girdi görüntüsünün ünlü bir athlete ise, örneğin, etiketlerinden birini spor, görüntüleri, spor bağlantılar içeren olabilir.

Her etiket, Insight uygulandığı ilgi, Öngörüler ve bir küçük resim görüntüsünün bölgelerini tanımlayan sınırlayıcı öngörülere kategorilere ayırmak için kullanabileceğiniz bir görünen ad içerir. Örneğin, bir spor jersey takmış bir kişi görüntünün ise etiketlerinden birini jersey sınırların ve VisualSearch ve ProductVisualSearch Öngörüler içeren bir sınırlayıcı kutu içerebilir. Ve başka bir etiket topically ilgili görüntüleri almak bir /images/search API isteği için bir URL veya Bing.com resim araması sonuçları kullanıcıya alan Bing.com arama URL'si içeren bir ImageResults öngörü içerebilir.

Sınırlayıcı görüntüde faiz bölgeleri kutular varsayılan ınsights etiket dışındaki tüm etiketleri içerir. Örneğin, görüntüyü birden çok tanınan kişi içeriyorsa, etiketlerin her kişi için sınırlayıcı kutular içerebilir veya görüntü tanınan giysi öğeler içeriyorsa, etiketleri sınırlayıcı kutular her tanınan giysi öğesi içerebilir. Ayırma-birleştirme görüntünün üzerine tıklandığında oluşturun, görüntünün bölgede içeriği ile ilgili ayrıntıları sağlayın sınırlayıcı kutular kullanabilirsiniz. Ayırma-birleştirme görüntünün tanımlayan sınırlayıcı kutular için bir görüntü içermemesi gerekir.

### <a name="text-recognition"></a>Metin tanıma

Görüntü hizmeti tanıdığı metni içeriyorsa, etiketlerinden birini TextResults Insight (işlem) içerir. InSight'ın `displayName` tanınan metin içeriyor. 

```json
    {
        "image" : {
            "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23Text..."
        },
        "displayName" : "##TextRecognition",
        "boundingBox" : {
            "queryRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            },
            "displayRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            }
        },
        "actions" : [{
            "displayName" : "WALK BIKE ACROSS BRIDGE",
            "actionType" : "TextResults"
        }],
        "sources" : ["OCR"]
    }
```

Çünkü etiketin `displayName` alan ##TextRecognition içeriyorsa, kategoriyi UX'i içinde olarak kullanmayın Gelecek herhangi bir görüntü adı ile başlayan ##. Bunun yerine eylem görüntüleme adı kullanın.


Metin tanıma, telefon numarası ve e-posta adresleri gibi belirli bir kartvizitler şirket iletişim bilgilerini de tanıyabilirsiniz. Sınırlayıcı kutu bilgilerini kartındaki konumunu belirtir. 

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        }
      },
      "actions" : [
        {
          "url" : "tel:888%20555%201212",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        }
      },
      "actions" : [
        {
          "url" : "mailto:someone@outlook.com",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        }
      },
      "actions" : [
        {
          "displayName" : "CHARLENE WHITNEY Graphic Designer 888 555 1212 someone@outlook.com www.contoso.com",
          "actionType" : "TextResults"
        }
      ],
      "sources" : ["OCR"]
    }
```

Görüntünün bir kişi, yer veya bir şey gibi tanınmış bir varlık içeriyorsa, etiketlerinden birini bir varlık öngörü içerebilir. 

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Statue+of+Liberty..."
      },
      "displayName" : "Statue of Liberty",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
          "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
          "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
          "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
          "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
          "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
          "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
        }
      },
      "actions" : [
        {
          "_type" : "ImageEntityAction",
          "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Statue+of+Liberty",
          "displayName" : "Statue of Liberty",
          "actionType" : "Entity",
        }
      ]
    }
```



## <a name="next-steps"></a>Sonraki adımlar

Hızlı başlangıçlar ilk isteğinizi hızlıca başlamak için bkz: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md).

API'yi deneyin. Git [görsel arama API'si sınama Konsolu'nu](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac). 


İle kendinizi alıştırın [görsel arama API'si başvurusu](https://aka.ms/bingvisualsearchreferencedoc). Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaların, üst bilgilerin ve sorgu parametrelerinin listesi yer alır. Ayrıca yanıt nesnelerinin tanımları da bulunur. 

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./use-and-display-requirements.md)'ni okumayı unutmayın.


