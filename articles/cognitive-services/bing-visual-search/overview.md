---
title: Bing Visual arama API genel bakış | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Ayrıntılar veya bir görüntü benzer görüntüleri ya da alışveriş kaynakları hakkında Öngörüler almak nasıl gösterir.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 95f10d8ea7ebe1d40d45231a8ea40df81543fe8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354760"
---
# <a name="what-is-bing-visual-search-api"></a>Bing Visual arama API nedir?

Bing Visual arama API Bing.com/images üzerinde gösterilen görüntü ayrıntıları benzer bir deneyim sağlar. Görsel arama ile resim karşıya yükleyin ve görsel olarak benzer görüntüleri, alışveriş kaynakları, görüntü ve daha fazlasını içeren Web sayfalarının görüntü ile ilgili Öngörüler geri alın. Görüntüyü karşıya yerine görüntüleri arama sonuçlarında bir görüntüden alma bir Öngörüler belirteci de sağlayabilirsiniz (bkz [Bing görüntüleri API](../bing-image-search/overview.md)).

Görsel arama çok ünlüler, monuments ve bilinen yerler, resim, ev mobilyaları, şekilde, ürünler, karakter tanıma ve daha fazla tanımlayabilirsiniz.

Görsel arama keşfetmenize olanak tanır Öngörüler verilmiştir.

- Görsel olarak benzer görüntüleri&mdash;Giriş resminin görsel olarak benzer görüntüleri listesi
- Görsel olarak benzer ürünleri&mdash;giriş görüntüde gösterilen ürün görsel olarak benzer ürünleri içeren resimler listesi
- Kaynakları alışveriş&mdash;burada satın alabilirsiniz giriş görüntüde gösterilen öğenin yerlerin listesi
- İlgili aramalar&mdash;diğerlerinin veya, tarafından yapılan ilişkili aramaları listesini görüntüsü içeriğine bağlı
- Görüntü içeren Web sayfaları&mdash;giriş görüntüyü içeren Web sayfalarının listesi
- Tarif&mdash;giriş görüntüde gösterilen tabağın için tarif içeren Web sayfalarının listesi

Bu Öngörüler yanı sıra Visual arama da giriş görüntüden türetilmiş koşullarını (etiketleri) farklı bir dizi döndürür. Bu etiketler görüntüde bulunan kavramları keşfetmek kullanıcıların izin verin. Örneğin, giriş görüntü ünlü athlete olduğunda etiketlerinden birini athlete adını olabilir, spor başka bir etiket olabilir. Veya giriş görüntü, apple pasta olduğunda, kullanıcılar ilgili kavramları gözatabilirsiniz etiketler Apple pasta, Pastalar, Desserts olabilir.

Sınırlayıcı görüntüde ilgilendiğiniz bölgeler için kutular Visual arama sonuçlarını de içerir. Örneğin, birkaç çok ünlüler görüntüsü içeriyorsa, sonuçları her görüntüde tanınan çok ünlüler sınırlayıcı kutular içerebilir. Veya, Bing bir ürün veya görüntüdeki giysisinin tanısa sonucu tanınan bir ürün veya giysisinin öğe için bir sınırlayıcı kutu içerebilir.

> [!IMPORTANT]
> / Görüntüleri/ayrıntılar uç noktasına kullanıyorsanız [görüntü bilgileri elde](../bing-image-search/image-insights.md), çünkü daha kapsamlı Öngörüler sağlar Visual arama yerine kullanmak için kodunuzu güncelleştirmeniz gerekir.


## <a name="the-request"></a>İstek

Bir görüntü ile ilgili Öngörüler almak için Seçenekler şunlardır: 

- Önceki çağrıda birine bir görüntüden alma bir Öngörüler belirteci Gönder [Bing görüntüleri API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) uç noktaları
- Bir görüntü URL Gönder
- Bir görüntüyü (ikili) karşıya yükleyin


Bir görüntü belirteç veya URL Visual arama gönderirseniz, aşağıdaki POST gövdesinde içermelidir JSON nesnesi gösterir. 

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

`imageInfo` Nesne ya da içermelidir `url` ve `imageInsightsToken` alan ikisini birden belirtmeyin. Ayarlama `url` Internet erişilebilir görüntünün URL'si alanı. Desteklenen en büyük görüntü boyutu 1 MB'tır.

`imageInsightsToken` Bir Öngörüler belirteci ayarlamanız gerekir. Bir Öngörüler belirteci almak için Bing görüntü API çağrısı. Yanıtı bir listesini içeriyor. `Image` nesneleri. Her `Image` nesnesini içeren bir `imageInsightsToken` simgeyi içeren alan.

`cropArea` Alan isteğe bağlı olur. Kırpma alanının üstünde, sol köşe ve altında bir bölge ilgi sağ köşesindeki belirtir. 0.0 ile 1.0 aralığında değerlerini belirtin. Genel genişlik veya yüksekliğinin yüzdesi değerlerdir. Örneğin, yukarıdaki örnek sağ yarısında görüntünün ilgi bölge işaretler. Bölge ilgi Öngörüler isteğine sınırlamak istiyorsanız içerir.

`filters` Nesnesini içeren bir site filtre (bkz `site` alan) benzer görüntüleri ve benzer ürünleri sonuçları belirli bir etki alanına kısıtlamak için kullanabilirsiniz. Örneğin, görüntü Surface Book ise ayarlayabileceğiniz `site` www.microsoft.com için. 

Bir görüntü yerel bir kopyasını hakkında Öngörüler almak istiyorsanız, ikili veri olarak görüntüyü karşıya yükleyin.

Bu seçenekler POST gövdesinde dahil olmak üzere hakkında daha fazla bilgi için bkz [içerik form türleri](#content-form-types).


### <a name="endpoint"></a>Uç Nokta

Görsel arama uç noktası: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

Yalnızca HTTP POST istekleri olarak gönderdiği istekleri gerekir. 


### <a name="query-parameters"></a>Sorgu parametreleri

İsteğiniz belirtmelisiniz sorgu parametreleri verilmiştir. En azından, içermelidir `mkt` sorgu parametresi.

|Ad|Değer|Tür|Gerekli|  
|----------|-----------|----------|--------------|  
|<a name="cc" />cc|Sonuçları alınacağı yeri ülkenin 2 karakterlik ülke kodu.<br /><br /> Bu parametre ayarlarsanız, ayrıca belirtmelisiniz [Accept-Language](#acceptlanguage) üstbilgi. Bing diller listesinden bulur ve sonuçları döndürmek için pazara belirlemek için belirttiğiniz ülke kodu dili birleştirir ilk desteklenen dil kullanır. Diller listesinde desteklenen bir dil içermiyorsa, Bing en yakın dil ve istek destekleyen Pazar bulur. Veya bir toplanmış kullanın veya pazar yerine belirtilen bir sonuç için varsayılan olabilir.<br /><br /> Bu sorgu parametresini kullanmanız gerekir ve `Accept-Language` sorgu parametresi yalnızca birden çok dil belirtirseniz; Aksi halde, kullanmanız gereken `mkt` ve `setLang` sorgu parametreleri.<br /><br /> Bu parametre ve [mkt](#mkt) sorgu parametresi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.|Dize|Hayır|  
|<a name="mkt" />Mkt|Sonuçları alınacağı yeri Pazar. <br /><br /> **Not:** , biliniyorsa her zaman Pazar belirtmeniz önemle önerilir. Pazar belirtme isteği yönlendirmek ve uygun ve en iyi bir yanıt döndürür Bing yardımcı olur.<br /><br /> Bu parametre ve [cc](#cc) sorgu parametresi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.|Dize|Evet|  
|<a name="safesearch" />güvenli arama|Yetişkinlere yönelik içeriğe filtre uygulamak için kullanılan bir filtre. Olası büyük küçük harf duyarsız filtre değerleri şunlardır:<br /><ul><li>Devre dışı&mdash;dönüş yetişkin metin veya görüntüler ile Web sayfaları.<br /><br/></li><li>Orta&mdash;dönüş yetişkin metin ancak değil yetişkin görüntüleri ile Web sayfaları.<br /><br/></li><li>Katı&mdash;yetişkin metin veya görüntüler ile Web sayfaları döndürmüyor.</li></ul><br /> Orta varsayılandır.<br /><br /> **Not:** isteği bir pazar geliyorsa gerektiren bu Bing'ın yetişkinlere yönelik ilke `safeSearch` sıkı ayarlanması, Bing yoksayar `safeSearch` değer ve katı kullanır.<br/><br/>**Not:** kullanırsanız `site:` sorgu işleci yanıt yetişkinlere yönelik içeriğe ne bağımsız olarak içerebilir fırsat yok `safeSearch` sorgu parametresi olarak ayarlanmış. Kullanım `site:` yalnızca sitedeki içeriğin farkında ve senaryonuz yetişkinlere yönelik içeriğe olasılığını destekler. |Dize|Hayır|  
|<a name="setlang" />setLang|Kullanıcı arabirimi dizeleri için kullanılacak dili. ISO 639-1 2 harfli dil kodunu kullanarak dili belirtin. Örneğin, İngilizce dil kodu tr. TR (İngilizce) varsayılandır.<br /><br /> İsteğe bağlı olsa da her zaman dil belirtmelisiniz. Genellikle, ayarladığınız `setLang` tarafından belirtilen aynı dil için `mkt` farklı bir dilde görüntülenen kullanıcı arabirimi dizeleri kullanıcının istediği sürece.<br /><br /> Bu parametre ve [Accept-Language](#acceptlanguage) üstbilgi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.<br /><br /> Bir kullanıcı arabirimi dizesi kullanıcı arabiriminde bir etiketi olarak kullanılan bir dizedir. JSON yanıt nesneleri birkaç kullanıcı arabirimi dizeleri vardır. Ayrıca, herhangi bir bağlantı yanıt nesneleri aratıp özelliklerinde belirtilen dil uygulayın.|Dize|Hayır| 

### <a name="headers"></a>Üst bilgiler

İsteğiniz belirtmelisiniz üstbilgileri verilmiştir. Content-Type ve Apim abonelik anahtar Ocp üstbilgileri yalnızca gerekli üstbilgileri ancak bu User-Agent, X MSEdge ClientID, X MSEdge ClientIP ve X arama konumu eklemeniz gerekir.


|Üst bilgi|Açıklama|  
|------------|-----------------|  
|<a name="acceptlanguage" />Kabul dili|İsteğe bağlı isteği üstbilgisi.<br /><br /> Kullanıcı arabirimi dizeleri için kullanılacak dil virgülle ayrılmış listesi. Tercih sırasına göre azalan düzende listesidir. Beklenen biçimle dahil olmak üzere daha fazla bilgi için bkz: [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Bu başlığı ve [setLang](#setlang) sorgu parametresi karşılıklı olarak birbirini dışlar&mdash;her ikisini birden belirtmeyin.<br /><br /> De belirtmeniz gerekir, bu başlığı ayarlarsanız, [cc](#cc) sorgu parametresi. Sonuçları döndürmek için pazara belirlemek için listeden bulur ve bununla birleştirir ilk desteklenen dil Bing kullanır `cc` parametre değeri. Liste desteklenen bir dil içermiyorsa, Bing en yakın dil ve istek destekleyen Pazar bulur veya pazar sonuçları için varsayılan veya bir toplanmış kullanır. Bing kullanılan Pazar belirlemek için BingAPIs Pazar üstbilgi bakın.<br /><br /> Bu üstbilgiyi kullanır ve `cc` yalnızca birden çok dil belirtirseniz sorgu parametresi. Aksi takdirde kullanın [mkt](#mkt) ve [setLang](#setlang) sorgu parametreleri.<br /><br /> Bir kullanıcı arabirimi dizesi kullanıcı arabiriminde bir etiketi olarak kullanılan bir dizedir. JSON yanıt nesneleri birkaç kullanıcı arabirimi dizeleri vardır. Herhangi bir bağlantı yanıt nesneleri aratıp özelliklerinde belirtilen dili uygulayın.|  
|<a name="contenttype" />İçerik türü|Gerekli isteği üstbilgisi.<br /><br />Multipart/form-data için ayarlanmış olması gerekir ve bir sınır parametresini ekleyin (örneğin, multipart/form-data; sınır =\<sınır dize\>). Daha fazla ayrıntı için bkz: [içerik form türleri](#content-form-types).
|<a name="market" />BingAPIs-Pazar|Yanıtı üstbilgisi.<br /><br /> İstek tarafından kullanılan Pazar. Form \<languageCode\>-\<countryCode\>. Örneğin, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Yanıtı üstbilgisi.<br /><br /> İstek ayrıntılarını içeren bir günlük girişi kimliği. Bir hata oluştuğunda, bu kimliği yakalama Belirlemek ve sorunu çözmek mümkün değilse, Destek ekibine sağladığınız bilgilerin yanı sıra bu Kimliğini içerir.|  
|<a name="subscriptionkey" />Ocp Apim abonelik anahtarı|Gerekli isteği üstbilgisi.<br /><br /> Bu hizmet için kaydolan getirdiğinizde aldığınız abonelik anahtarı [Bilişsel Hizmetler](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|İsteğe bağlı istek üstbilgisi<br /><br /> Varsayılan olarak, Bing varsa önbelleğe alınmış içeriği döndürür. Önbelleğe alınmış içeriği döndürmesini Bing engellemek için Pragma üstbilgi no cache olarak ayarlayın (örneğin, Pragma: no-cache).
|<a name="useragent" />Kullanıcı Aracısı|İsteğe bağlı isteği üstbilgisi.<br /><br /> İsteğin kaynaklandığı kullanıcı aracısı. Bing kullanıcı aracısı mobil kullanıcılar en iyi duruma getirilmiş bir deneyim sağlamak için kullanır. İsteğe bağlı olsa da her zaman bu üstbilgisi belirtmeniz önerilir.<br /><br /> Kullanıcı Aracısı, yaygın olarak kullanılan tarayıcılar gönderir aynı dize olmalıdır. Kullanıcı aracıları hakkında daha fazla bilgi için bkz: [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Kullanıcı Aracısı dizeleri örnekleri verilmiştir.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (uyumlu; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 derleme/Zencefilli KURABİYE) AppleWebKit/533.1 (KHTML; ister Gecko) sürüm/4.0 mobil Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone işletim sistemi 6_1 Mac OS X gibi) AppleWebKit/536.26 (KHTML; ister Gecko) mobil/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; RV:11.0) ister Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 Mac OS X gibi) AppleWebKit/537.51.1 (Gecko gibi KHTML) sürüm/7.0 mobil/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X MSEdge ClientID|İsteğe bağlı istek ve yanıt üstbilgi.<br /><br /> Bing kullanıcılara Bing API çağrıları arasında ile tutarlı davranışı sağlamak için bu üstbilgiyi kullanır. Bing genellikle yeni özellikler ve geliştirmeler uçuşlar ve istemci kimliği farklı uçuşlar trafiğinde atamak için bir anahtar olarak kullanır. Birden çok istekte bir kullanıcı için aynı istemci kimliği kullanmıyorsanız Bing kullanıcı için birden çok çakışan uçuşlar atayabilir. İçin birden çok çakışan uçuşlar atanmasını tutarsız kullanıcı deneyimi için yol açabilir. Örneğin, ikinci istek ilk daha farklı uçuş atama sahipse deneyimi beklenmeyen olabilir. Bing web sonuçlarını o istemciye uyarlamak için Ayrıca, istemci Kimliğini kullanabilirsiniz kimliğin arama geçmişi, kullanıcı için daha zengin bir deneyim sağlar.<br /><br /> Bing bir istemci kimliği tarafından oluşturulan etkinliğini çözümleyerek sonuç derecelendirmeleri geliştirmeye yardımcı olmak için bu üst de kullanır. Bing API'leri ve sırayla tıklatın aracılığıyla etkinleştirir yüksek hızları için API tüketici teslim sonuçlarının daha iyi kalitesiyle Yardım ilgi geliştirmeleri.<br /><br /> **Önemli:** isteğe bağlı olsa da bu başlığı gerekli göz önünde bulundurmanız gerekir. İstemci kimliği aynı son kullanıcı ve aygıt birleşimi için birden çok istekte kalıcı tutarlı bir kullanıcı deneyimi ve daha iyi sonuçlar kalitesini aracılığıyla 2) daha yüksek tıklatın aracılığıyla ücretlerin Bing API'lerden almak 1) API tüketici sağlar.<br /><br /> Bu başlığına uygulanan temel kullanım kuralları aşağıda verilmiştir.<br /><ul><li>Cihazda uygulamanızın kullandığı her bir kullanıcı bir benzersiz olması gerekir, istemci kimliği Bing oluşturulan<br /><br/>Bu üst istekte eklemezseniz Bing kimliği oluşturur ve X MSEdge ClientID yanıt üstbilgisinde döndürür. Bu üst bir istekte içermemelidir yalnızca kullanıcı uygulamanızı bu cihaz üzerinde ilk kullandığında saattir.<br /><br/></li><li>**DİKKAT:** bu istemci kimliği için tüm kimliği doğrulanmış kullanıcı hesabı bilgilerini linkable değil emin olmalısınız.</li><li>Cihazda bu kullanıcı için uygulamanızı yapar her Bing API'si isteği için istemci Kimliğini kullanın.<br /><br/></li><li>İstemci kimliği Sürdür Bir tarayıcı uygulaması kimliği sürdürmek için tüm oturumlarında kullanılan kimliği emin olmak için kalıcı bir HTTP tanımlama bilgisi kullanın. Bir oturum tanımlama bilgisi kullanmayın. Mobil uygulamalar gibi diğer uygulamalar için cihazın kalıcı depolama kimliği sürdürmek için kullanın.<br /><br/>Kullanıcı bu aygıtta uygulamanızı kullanan sonraki açışınızda, kalıcı istemci kimliği alın.</li></ul><br /> **Not:** Bing yanıtlar olabilir ya da bu başlığı içermeyebilir. Yanıt bu üst bilgisi içeriyorsa, istemci kimliği yakalayın ve bu cihazdaki kullanıcı için tüm sonraki Bing istekler için kullanın.<br /><br /> **Not:** X MSEdge ClientID eklerseniz, tanımlama bilgileri istek içermemesi gerekir.|  
|<a name="clientip" />X MSEdge ClientIP|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemci aygıtı IPv4 veya IPv6 adresi. IP adresi, kullanıcının konumunu bulmak için kullanılır. Bing konum bilgileri güvenli arama davranışını belirlemek için kullanır.<br /><br /> **Not:** isteğe bağlı olsa da, her zaman bu başlığı ve X arama konum üstbilgisi belirtmeniz önerilir.<br /><br /> Adres (örneğin, son sekizli 0 olarak değiştirerek) belirsizleştirirseniz değil. Herhangi bir yere gerçek konumuna cihazın olmaması konumu adresi sonuçlarında obfuscating, hangi hatalı sonuçları hizmet veren Bing neden olabilir.|  
|<a name="location" />X arama konumu|İsteğe bağlı isteği üstbilgisi.<br /><br /> İstemcinin coğrafi konumunu tanımlayan anahtar/değer çiftleri noktalı virgülle ayrılmış listesi. Bing güvenli arama davranışını belirlemek için ve ilgili yerel içerik döndürmek için konum bilgileri kullanır. Anahtar/değer çifti olarak belirtmek \<anahtar\>:\<değeri\>. Kullanıcının konumunu belirtmek üzere kullanacak anahtarları şunlardır:<br /><br /><ul><li>LAT&mdash;gerekli. Derece cinsinden istemcinin konumun enlem. Enlem-90.0 eşit veya daha büyük olmalıdır ve +90.0 küçük veya buna eşit. Negatif değerler Güney latitudes belirtmek ve Kuzey latitudes pozitif değer belirtin.<br /><br /></li><li>uzun&mdash;gerekli. İstemcinin konumda derece boylam. Boylam-180.0 eşit veya daha büyük olmalıdır ve +180.0 küçük veya buna eşit. Negatif değerler Batı longitudes belirtmek ve Doğu longitudes pozitif değer belirtin.<br /><br /></li><li>RE&mdash;gerekli. Yatay koordinatları doğruluğunu belirten RADIUS, ölçümler içinde. Cihazın konum hizmeti tarafından döndürülen değer geçirin. Tipik değerleri GPS/Wi-Fi için 22 m, hücre kule Üçlü 380 m ve geriye doğru IP arama için 18, 000 m olabilir.<br /><br /></li><li>TS&mdash;isteğe bağlı. Ne zaman istemci konumunda oluştu, UTC UNIX zaman damgası. (UNIX zaman damgası 1 Ocak 1970'ten beri geçen saniye sayısıdır.)<br /><br /></li><li>HEAD&mdash;isteğe bağlı. İstemcinin göreli başlığını veya seyahat yönü. 0'dan göre doğru Kuzey yönünde sayım 360 derece olarak seyahat yönünü belirtin. Bu anahtar yalnızca, belirtin `sp` anahtarıdır sıfır olmayan bir değer.<br /><br /></li><li>SP&mdash;isteğe bağlı. İstemci aygıtı dolaşan saniye başına metre içinde Yatay hız (hızlı).<br /><br /></li><li>alt&mdash;isteğe bağlı. İstemci aygıtı ölçümler yüksekliği.<br /><br /></li><li>olan&mdash;isteğe bağlı. Koordinatları dikey doğruluğunu belirten RADIUS, ölçümler içinde. Yalnızca belirtirseniz, bu anahtarı belirtmek `alt` anahtarı.<br /><br /></li></ul> **Not:** birçok anahtarların isteğe bağlı, ancak daha doğru konuma sonucu olan sağlayan daha fazla bilgi.<br /><br /> **Not:** isteğe bağlı olsa da, her zaman kullanıcının coğrafi konumu belirtmek için önerilir. Konum (örneğin, istemci VPN kullanıyorsa) istemcinin IP adresini kullanıcının fiziksel konumunu doğru şekilde yansıtmaz durumunda özellikle önemlidir. En iyi sonuçlar için bu başlığı ve X MSEdge ClientIP başlığı içermelidir, ancak en azından, bu başlığı içermelidir.|

> [!NOTE] 
> Kullanım Koşulları'nı bu üstbilgileri kullanımıyla dahil olmak üzere tüm geçerli yasaları ile uyumluluğu gerekli olduğunu unutmayın. Örneğin, Avrupa gibi belirli daireleri de kullanıcı aygıtları üzerinde belirli izleme cihazlarını yerleştirme önce kullanıcı onayı almak için gereksinimi yoktur.


<a name="content-form-types" />

### <a name="content-form-types"></a>İçerik form türleri

Her istek Content-Type üstbilgisi eklemeniz gerekir. Üstbilgi ayarlanmalıdır: multipart/form-data; sınır =\<sınır dize\>, burada \<sınır dize\> form verilerini sınır tanımlayan benzersiz, donuk bir dizedir. Örneğin, sınır boundary_1234 abcd =.


Bir görüntü belirteç veya URL Visual arama gönderirseniz, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini içermelidir ve kendi `name` "knowledgeRequest" parametresini ayarlayın. Hakkındaki ayrıntılar için `imageInfo` nesne için bkz: [isteği](#the-request).


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

Yerel görüntü yüklerseniz, aşağıdaki form verilerini POST gövdesinde içermelidir gösterir. Form verileri içerik düzeni üstbilgisini eklemeniz gerekir. Kendi `name` parametresini ayarlayın, görüntü"için" ve `filename` parametresi için herhangi bir dize ayarlanmış olabilir. Content-Type üstbilgisi herhangi yaygın olarak kullanılan resim MIME türü için ayarlanabilir. Form içeriğini ikili görüntünün olur. Karşıya yükleme en büyük görüntü boyutu 1 MB'tır. 


```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Karşıya yüklenen görüntü ilgi bölge belirtme gösterir.

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

Bir görüntü belirteci ve bölge ilgi geçen bir tam görüntü Öngörüler isteği gösterir. /İmages/search önceki çağrısından Öngörüler belirteci alın.


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

Görüntü için kullanılabilir Öngörüler varsa, bir veya daha fazla yanıt içeriyor `tags` bilgileri içerir. `image` Alan girdi görüntüsü Öngörüler belirteci içerir.

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

`tags` Alan bir görünen ad ve eylemlerin (Öngörüler) listesini içerir. Etiketlerinden birini içeren bir `displayName` alan boş bir dize olarak ayarlayın. Bu etiket görüntü, görsel olarak benzer görüntüler ve görüntüde bulunan öğeler için alışveriş kaynakları içeren Web sayfaları gibi varsayılan Öngörüler içerir. Görüntünün tümünü ilgi olduğundan, varsayılan Öngörüler etiket sınırlayıcı bölgeler için kutular ilgi içermez.


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

Varsayılan Öngörüler listesi için bkz: [varsayılan Öngörüler](./default-insights-tag.md).



Kalan etiketler kullanıcı ilgilendirebilecek diğer Öngörüler içeriyor. Örneğin, görüntünün metin içeriyorsa, etiketlerinden birini tanınan metni içeren bir TextResults Insight içerebilir. Veya, Bing görüntüde (kişi, yer veya şey) tanısa etiketlerinden birini varlık tanımlayabilir. Görsel arama da giriş görüntüden türetilmiş koşullarını (etiketleri) farklı bir dizi döndürür. Bu etiketler görüntüde bulunan kavramları keşfetmek kullanıcıların izin verin. Girdi görüntüsü ünlü athlete ise, örneğin, etiketleri spor, spor görüntülerini bağlantılar içeren biri olabilir.

Her etiket Insight uygulandığı ilgi, Öngörüler ve görüntünün küçük resim bölgelerini tanımlayan sınırlayıcı Insight sınıflandırmak için kullanabileceğiniz bir görünen ad içerir. Örneğin, görüntü Spor bölgesi kartı bir kişinin ise etiketlerinden birini bölgesi bounds ve VisualSearch ve ProductVisualSearch Öngörüler içeren bir sınırlayıcı kutu içerebilir. Ve başka bir etiket topically ilgili görüntüleri almak bir /images/search API isteği için bir URL veya aratıp resim arama sonuçları kullanıcıyı götürür aratıp arama URL'si içeren bir ImageResults öngörü içerebilir.

Varsayılan Öngörüler etiket dışındaki tüm etiketleri sınırlayıcı görüntüdeki ilgi bölgeleri tanımlayan kutular içerir. Örneğin, görüntüyü birden fazla tanınan kişi içeriyorsa, etiketleri her kişi için sınırlayıcı kutular içerebilir veya görüntü tanınan giysisinin öğeler içeriyorsa, etiketleri kutuları her tanınan giysisinin öğesi için sınırlayıcı içerebilir. Etkin noktalar görüntünün üzerine tıklatıldığında oluşturun, görüntünün bu bölgede içeriği hakkında ayrıntılı bilgileri sağlamak için sınırlayıcı kutularını kullanabilirsiniz. Etkin noktalar görüntünün tamamını tanımlamak sınırlayıcı kutuları için görüntü içermemesi gerekir.

### <a name="text-recognition"></a>Metin tanıma

Görüntü hizmet tanıdığı metni içeriyorsa, etiketlerinden birini TextResults Insight (Eylem) içerir. Insight's `displayName` tanınan metni içerir. 

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

Çünkü etiketin `displayName` alan ##TextRecognition kullanmayın bir kategori olarak başlık UX içerir Gelecek herhangi görüntülemek için ad ile başlayan ##. Bunun yerine eylemin görünen adı kullanın.


Metin tanıma telefon numarası ve e-posta adresleri gibi belirli bir kartvizitler ile irtibat bilgilerini de tanıyabilirsiniz. Sınırlama kutusu kartındaki kişi bilgileri konumunu tanımlar. 

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

Görüntüde tanınan bir varlık kişi, yer veya şey gibi varsa etiketlerinden birini bir varlık öngörü içerebilir. Aşağıdaki örnekte gösterildiği gibi varlıkları trivia da içerebilir:

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
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "Trivia",
          "data" : {
            "value" : [
              {
                "name" : "Where was the cornerstone of the statue of liberty laid",
                "text" : "<the answer>",
                "hostPageUrl" : "http:\/\/contoso.com\/history\/...",
              },
              {
                "name" : "Why Is the Statue of Liberty Green",
                "text" : "<the answer>",
                "hostPageUrl" : "https:\/\/www.contoso.com\/why-statue-of-liberty-is-green",
              },
              {
                "name" : "What is the Statue of Liberty made of",
                "text" : "<the answer>",
                "hostPageUrl" : "https:\/\/www.contoso.com\/art-literature\/statue-liberty-made",
              }
            ]
          }
        }
      ]
    }
```



## <a name="next-steps"></a>Sonraki adımlar

Quickstarts ilk isteklerinize hızlı bir şekilde başlamak için bkz: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md).

Out API deneyin. Git [Visual arama API sınama Konsolu'nu](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac). 


İle öğrenmeniz [Visual arama API Başvurusu](https://aka.ms/bingvisualsearchreferencedoc). Başvuru uç noktaları, üstbilgi ve arama sonuçlarını istemek için kullanacağınız sorgu parametreleri listesini içerir. Ayrıca yanıt nesnelerin tanımları içerir. 

Okuduğunuzdan emin olun [Bing kullanın ve görüntü gereksinimleri](./use-and-display-requirements.md) arama sonuçlarını kullanma hakkında kurallardan herhangi birinin kesmeyin şekilde.


