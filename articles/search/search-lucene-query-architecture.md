---
title: Tam metin arama motoru (Lucene) mimarisi Azure Search'te | Microsoft Docs
description: Lucene sorgu işleme ve belge alma kavramları açıklaması olarak Azure arama ile ilgili tam metin araması için.
manager: jlembicz
author: yahnoosh
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: jlembicz
ms.openlocfilehash: 4382c3001f6b0a9227407beccb483347bccb387c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32195030"
---
# <a name="how-full-text-search-works-in-azure-search"></a>Azure Search'te nasıl tam metin araması çalışır

Bu makale, Azure Search'te Lucene tam metin araması nasıl çalıştığını daha derin bir anlayış isteyen geliştiriciler için yazılmıştır. Metin sorguları için sorunsuz bir şekilde beklenen sonuçları Azure Search Çoğu senaryoda teslim eder, ancak bazen "kapalı" şekilde görünen bir sonuç almak. Bu durumda, bir arka plan Lucene sorgu yürütme dört aşamalarında sahip (ayrıştırma, sözcük analiz sorgusu, belge eşleşen, Puanlama), sorgu parametreleri ya da istenen sonuca teslim eder dizini yapılandırmasını belirli değişiklikleri belirlemenize yardımcı olabilir. 

> [!Note] 
> Azure arama için tam metin araması Lucene kullanır, ancak Lucene tümleştirme geniş kapsamlı değildir. Biz seçmeli olarak kullanıma sunmak ve Azure Search önemli senaryoları etkinleştirmek için Lucene işlevselliğini genişleten. 

## <a name="architecture-overview-and-diagram"></a>Mimarisine genel bakış ve diyagramı

Bir tam metin arama sorgusu işlemeye başlıyor arama terimleri ayıklamak için sorgu metni ayrıştırma ile. Arama motoru dizin eşleşen terimleri içeren belgeleri almak için kullanır. Tek tek sorgu terimleri bazen ayrıntılarıyla ve ne olası bir eşleşme olarak kabul edilebilir üzerinde daha geniş bir net dönüştürmek için yeni formları içine reconstituted. Bir sonuç kümesi sonra her bir eşleşen belgeyi atanmış bir ilgi puana göre sıralanır. Bu dereceli listenin çağrı yapan uygulamaya döndürülür.

Sorgu yürütme açıklandı, dört aşamadan oluşur: 

1. Sorgu ayrıştırma 
2. Sözcük çözümleme 
3. Belge alma 
4. Puanlama 

Aşağıdaki diyagramda bir arama isteği işlemek için kullanılan bileşenleri göstermektedir. 

 ![Azure Search'te Lucene sorgu mimarisi diyagramı][1]


| Başlıca bileşenler | İşlev açıklaması | 
|----------------|------------------------|
|**Sorgu ayrıştırıcıları** | Sorgu işleçleri sorgu terimlerinin ayırın ve arama motoru gönderilmek üzere sorgu yapısını (sorgu ağacı) oluşturun. |
|**Çözümleyiciler** | Sözcük analiz sorgu terimlerinin üzerinde gerçekleştirin. Bu işlem, dönüştürme, kaldırma veya sorgu koşullarını genişletme içerebilir. |
|**Dizin** | Depolamak ve dizinli belgelerden ayıklanan aranabilir koşulları düzenlemek için kullanılan bir verimli veri yapısı. |
|**Arama motoru** | Alır ve ters dizin içeriğine göre eşleşen belgeleri puanlar. |

## <a name="anatomy-of-a-search-request"></a>Bir arama isteğine anatomisi

Bir arama isteğine bir sonuç kümesi döndürdü, tam bir özelliğidir. Basit haliyle, hiçbir ölçüt içermeyen boş bir sorgu değil. Daha gerçekçi bir örnek, belki de büyük olasılıkla bir filtre ifadesi ve kuralları sıralama ile bazı alanlar için kapsamlı, birkaç sorgu terimlerinin parametrelerini içerir.  

Aşağıdaki örnek Gönder Azure Search kullanmaya arama istektir [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).  

~~~~
POST /indexes/hotels/docs/search?api-version=2017-11-11 
{  
    "search": "Spacious, air-condition* +\"Ocean view\"",  
    "searchFields": "description, title",  
    "searchMode": "any",
    "filter": "price ge 60 and price lt 300",  
    "orderby": "geo.distance(location, geography'POINT(-159.476235 22.227659)')", 
    "queryType": "full" 
 } 
~~~~

Bu istek için arama motoru şunları yapar:

1. Fiyat en az 60 ve değerinden 300 olduğu giden belgeleri filtreler.
2. Sorguyu yürütür. Bu örnekte, arama sorgusu tümcecikleri ve şartları oluşur: `"Spacious, air-condition* +\"Ocean view\""` (kullanıcılar genellikle noktalama girin yoktur, ancak örnekte dahil olmak üzere, verir bize nasıl çözümleyiciler ele açıklamak). Bu sorgu için arama motoru açıklama tarar ve belirtilen başlık alanları `searchFields` "Okyanusu Görünüm" içeren belgeleri ve ayrıca "spacious" terimini veya önek ile başlayan koşullarınızda "air-condition". `searchMode` Parametresi herhangi terim (varsayılan) veya bir terim olduğu kesinlikle gerekli durumlarda bunların tümünün eşleştirmek için kullanılır (`+`).
3. Yakınlık verilen Coğrafya konuma göre Oteller ayarlayın ve çağıran uygulamada döndürülen sonuç sıralar. 

Bu makalede çoğunluğu hakkında işlemesidir *arama sorgusu*: `"Spacious, air-condition* +\"Ocean view\""`. Filtreleme ve Sıralama kapsamının dışına markalarıdır. Daha fazla bilgi için bkz: [arama API başvuru belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a>1. Aşama: ayrıştırma sorgu 

Belirtildiği gibi sorgu dizesi isteğin ilk satır şöyledir: 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

Sorgu ayrıştırıcı işleçleri ayıran (gibi `*` ve `+` örnekte) arama koşulları ve arama sorguyu deconstructs *alt sorgulara* desteklenen bir türde: 

+ *Terim sorgu* (gibi spacious) tek başına koşulları için
+ *Tümcecik sorgusu* tırnak işaretli şartlarının (gibi Okyanusu görünümü)
+ *önek sorgu* önek işleci tarafından izlenen koşulları için `*` (air-condition gibi)

Desteklenen sorgu türlerinin tam listesi için bkz: [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

Bir alt sorgu ile ilişkili işleçleri belirlemek sorgu "olmalıdır" veya "olmalıdır" belge bir eşleşme olarak kabul edilmesi sırayla memnun. Örneğin, `+"Ocean view"` "" nedeniyle gerekliliktir `+` işleci. 

Alt sorgular içine sorgu ayrıştırıcı yeniden yapılandırır bir *sorgu ağaç* arama motoru, (sorgu temsil eden bir iç yapısı) geçirir. Sorgu ayrıştırma, ilk aşamada, sorgu ağaç şöyle görünür.  

 ![Boolean sorgu searchmode herhangi][2]

### <a name="supported-parsers-simple-and-full-lucene"></a>Ayrıştırıcıları desteklenen: Basit ve tam Lucene 

 Azure arama gösteren iki farklı sorgu dili, `simple` (varsayılan) ve `full`. Ayarlayarak `queryType` arama isteğinizi parametresi, size sorgu ayrıştırıcı sorgu dili böylece bunu bilen işleçler ve sözdizimi yorumlama seçin. [Basit sorgu dili](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) olan sezgisel ve güçlü, kullanıcı giriş olarak yorumlamak genellikle uygun-istemci tarafı işleme olmadan. Sorgu işleçleri web arama motorlarından tanıdık destekler. [Tam Lucene sorgu dili](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), hangi ayarlayarak alma `queryType=full`, daha fazla işleçler ve joker karakter, benzer gibi sorgu türleri, regex ve alan kapsamlı sorgular için destek ekleyerek varsayılan Basit Sorgu Dili genişletir. Örneğin, Basit Sorgu söz dizimi gönderilen bir normal ifade bir sorgu dizesi ve bir ifade yorumlanır. Bu makaledeki örnek istek tam Lucene sorgu dilini kullanır.

### <a name="impact-of-searchmode-on-the-parser"></a>Ayrıştırıcının üzerinde searchMode etkisi 

Ayrıştırma etkiler başka bir arama istek parametresi `searchMode` parametresi. Boole sorgular için varsayılan işleç denetler: herhangi bir (varsayılan) veya tüm.  

Zaman `searchMode=any`, varsayılan, spacious alanı bölücüyü ve air-condition veya (`||`), örnek sorgu metnini eşdeğer yapma: 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

Açık işleçleri gibi `+` içinde `+"Ocean view"`, boolean sorgu yapısında anlaşılır olan (terimi *gerekir* eşleşen). Kalan koşulları yorumlama daha az açıktır: spacious ve air-condition. Arama motoru Okyanusu görünümünde eşleşen bulmalısınız *ve* spacious *ve* air-condition? Veya Okyanusu görünüm bulmalısınız artı *herhangi birini* kalan koşullarını? 

Varsayılan olarak (`searchMode=any`), arama motoru daha geniş yorumlama varsayar. Her iki alan *gereken* eşleştirilmesi, yansıtma "veya" semantiği. İlk sorguyu ağaç daha önce gösterilen ile iki "gereken işlemleri", varsayılan gösterir.  

Şimdi ayarlarız varsayalım `searchMode=all`. Bu durumda, alan bir "ve" işlem olarak yorumlanır. Her kalan koşullarını hem de bir eşleşme olarak nitelemek için belgedeki bulunması gerekir. Sonuçta elde edilen örnek sorgu şu şekilde yorumlanacağını: 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

Bu sorgu için değiştirilmiş sorgu ağacı eşleşen belgenin tüm üç alt sorgulara kesişimi olduğu aşağıdaki gibi olur: 

 ![Boole Sorgusu searchmode tüm][3]

> [!Note] 
> Seçme `searchMode=any` üzerinden `searchMode=all` en iyi kararı temsilcisi sorguları çalıştırarak gelen değil. İşleçler (arama belge depoladığında ortak) dahil olasılığı olan kullanıcıların sonuçları daha sezgisel, bulabileceğiniz `searchMode=all` boolean sorgu yapıları bildirir. Arasında Interplay hakkında daha fazla bilgi için `searchMode` ve işleçleri [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a>2. Aşama: Sözcük çözümleme 

Sözcük çözümleyiciler işlem *terim sorguları* ve *tümcecik sorguları* sorgu ağaç yapılandırılmış sonra. Bir Çözümleyicisi için ayrıştırıcı tarafından verilen metin girişleri kabul eder, metin ve ardından geri simgeleştirilmiş gönderir koşulları sorgu ağacına yapılması işler. 

En sık kullanılan sözcük analiz biçimidir *dil analiz* hangi dönüşümler sorgulamak için belirli bir dile özel kurallar temel alınarak koşulları: 

* Bir sorgu Terime bir sözcük kökü forma azaltma 
* Gerekli olmayan sözcükler kaldırma ("" gibi bir Durma sözcükleri veya "ve" İngilizce) 
* Bileşen parçalara bileşik bir sözcük bölme 
* Küçük büyük harf word büyük/küçük harfleri 

Bu işlemlerin tümü, kullanıcı tarafından sağlanan metin giriş dizininde depolanan koşulları arasındaki farklar silinecek eğilimlidir. Bu tür işlemler metin işleme ötesine gidin ve dilinin kapsamlı bilgi gerektirir. Bu dil tanıma katmanı eklemek için uzun bir listesi Azure Search destekler [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene ve Microsoft tarafından.

> [!Note]
> Analiz gereksinimleri en az karmaşık üzerinde senaryonuza bağlı olarak için değişebilir. Önceden tanımlanmış çözümleyiciler seçme birini veya kendi oluşturarak sözcük analiz karmaşıklığını denetleyebilirsiniz [özel Çözümleyicisi](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search). Çözümleyiciler aranabilir alanlara kapsamlı ve alan tanımının bir parçası belirtilir. Bu, alan başına temelinde sözcük analiz değiştirmenizi sağlar. Belirtilmezse, *standart* Lucene Çözümleyicisi kullanılır.

Örneğimizde, analiz önce ilk sorgu ağacı terimi "bir büyük harf"S"ve sorgu ayrıştırıcı (virgül bir sorgu dili işleci sayılmaz) sorgu Terime bir parçası olarak yorumlar virgül ile" Spacious sahiptir.  

Varsayılan çözümleyici terimi işlediğinde, bunu "Okyanusu Görünüm" ve "spacious" küçük harf ve virgül karakteri kaldırın. Değiştirilen sorgu ağacında aşağıdaki gibi görünür: 

 ![Çözümlenen koşullarla Boole Sorgusu][4]

### <a name="testing-analyzer-behaviors"></a>Test Çözümleyicisi davranışları 

Bir Çözümleyicisi davranışını kullanılarak sınanabilir [analiz API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer). Ne Çözümleyicisi verilen koşulları oluşturacak görmek için analiz etmek istediğiniz metni girin. Örneğin, standart Çözümleyicisi metin "air-condition" işleminin nasıl görmek için aşağıdaki isteği verebilir:

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

Standart Çözümleyicisi bunları konumlarını (tümcecik eşleştirmek için kullanılır) yanı sıra başlangıç ve bitiş kaydırmalar (isabet vurgulama için kullanılan) gibi özniteliklerle yorumlama aşağıdaki iki belirteçler içine giriş metni keser:

~~~~
{  
  "tokens": [
    {
      "token": "air",
      "startOffset": 0,
      "endOffset": 3,
      "position": 0
    },
    {
      "token": "condition",
      "startOffset": 4,
      "endOffset": 13,
      "position": 1
    }
  ]
}
~~~~

<a name="exceptions"></a>

### <a name="exceptions-to-lexical-analysis"></a>Sözcük analiz için özel durumlar 

Sözcük analiz tüm koşullar – bir terim sorgu veya tümcecik sorgusu gerektiren sorgu türleri için geçerlidir. Sorgu türleri – önek sorgu, joker karakter sorgu, regex sorgu – tamamlanmamış koşullarla veya benzer bir sorgu için geçerli değildir. Bu terim önek sorguyla dahil olmak üzere türü, sorgu *air-condition\**  Bizim örneğimizde, analiz aşaması atlayarak doğrudan sorgu ağacında için eklenir. Bu türlerde sorgu koşullarını gerçekleştirilen yalnızca dönüştürmeyi lowercasing.

<a name="stage3"></a>

## <a name="stage-3-document-retrieval"></a>3. Aşama: Belge alma 

Belge alma dizini terimleriyle eşleşen belgeleri bulmak için ifade eder. Bu aşama örneği arasında en iyi anlaşılmalıdır. Aşağıdaki basit şemasına sahip Oteller diziniyle başlayalım: 

~~~~
{   
    "name": "hotels",     
    "fields": [     
        { "name": "id", "type": "Edm.String", "key": true, "searchable": false },     
        { "name": "title", "type": "Edm.String", "searchable": true },     
        { "name": "description", "type": "Edm.String", "searchable": true }
    ] 
} 
~~~~

Daha fazla bu dizini aşağıdaki dört belge içerir varsayın: 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance to the beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on the north shore of the island of Kauaʻi. Ocean view."     
        },       
        {         
            "id": "3",         
            "title": "Playa Hotel",         
            "description": "Comfortable, air-conditioned rooms with ocean view."
        },       
        {         
            "id": "4",         
            "title": "Ocean Retreat",         
            "description": "Quiet and secluded"
        }    
    ]
}
~~~~

**Koşulları nasıl dizini**

Alma anlamak için dizin oluşturma hakkındaki birkaç temel kavramları biliyorsanız yardımcı olabilir. Depolama Birimi ters, aranabilir her alan için bir tane dizinidir. Ters dizin tüm belgelerden tüm koşulları sıralanmış bir listesidir. Bunu, aşağıdaki örnekte güvenli oluştuğu belgelerin listesini her terim eşler.

Ters dizin koşullarını üretmek için belgeler, sorgu işleme sırasında neler için benzer içerik üzerinde sözcük analiz arama motoru gerçekleştirir:

1. *Metin girişleri* bir Çözümleyicisi için alt ortası noktalama işaretleri ve Çözümleyicisi yapılandırmasına bağlı olarak belirli bir benzeri atılmış geçirilir. 
2. *Belirteçleri* metin analiz çıktısı alınır.
3. *Koşulları* dizine eklenir.

Ortak, ancak gerekli değildir, aynı çözümleyiciler arama ve böylece sorgu terimlerinin dizini içinde koşulları gibi daha fazla ara işlemleri dizin oluşturma için kullanılacak.

> [!Note]
> Azure arama sağlar, dizin oluşturma için farklı çözümleyiciler belirtin ve arama aracılığıyla ek `indexAnalyzer` ve `searchAnalyzer` alan parametreleri. Belirtilmezse, Çözümleyicisi kümesiyle `analyzer` özelliği, hem dizin oluşturma ve arama için kullanılır.  

**Örnek belgeler için ters dizin**

Bizim örneğimizde için için döndürme **başlık** alanı, ters dizini şu şekilde görünür:

| Sözleşme Dönemi | Belge listesi |
|------|---------------|
| atman | 1 |
| Plaj | 2 |
| Otel | 1, 3 |
| Okyanusu | 4  |
| playa | 3 |
| çare | 3 |
| Retreat | 4 |

Başlık alanında, yalnızca *otel* iki belgelerde görüntülenir: 1, 3.

İçin **açıklama** alan, dizinde aşağıdaki gibidir:

| Sözleşme Dönemi | Belge listesi |
|------|---------------|
| Hava | 3
| ve | 4
| Plaj | 1
| söylediğinizde | 3
| rahat | 3
| uzaklık | 1
| Adası | 2
| kauaʻi | 2
| bulunan | 2
| Kuzey | 2
| Okyanusu | 1, 2, 3
| / | 2
| açık |2
| Sessiz | 4
| odaları  | 1, 3
| secluded | 4
| Deniz Kıyısı | 2
| spacious | 1
| , | 1, 2
| - | 1
| görünüm | 1, 2, 3
| taramasını | 1
| with | 3


**Dizinli koşulları karşı sorgu terimlerinin eşleştirme**

Şimdi örnek sorgu döndürme yukarıdaki ters dizinlerini verildiğinde ve nasıl eşleşen belgeleri görmek için bizim örnek sorgu bulunamadı. Son sorgu ağaç şöyle geri çağırma: 

 ![Çözümlenen koşullarla Boole Sorgusu][4]

Sorgu yürütme sırasında tek tek sorguları karşı aranabilir alanlara bağımsız olarak yürütülür. 

+ TermQuery, "spacious" eşleşmeleri 1 (otel Atman) belgesi. 

+ PrefixQuery "air-condition *", herhangi bir belgeniz eşleşmiyor. 

  Bazen geliştiriciler kafasını karıştırabilirler bir davranış budur. Belgede Air-conditioned terim var ancak iki terimleri varsayılan Çözümleyicisi tarafından ayrılır. Kısmi terimleri içeren, önek sorguları çözümlenmediğini geri çağırma. Bu nedenle önek "air-condition" ile koşulları ters dizinde arama ve bulunamadı.

+ "Okyanusu Görünüm" PhraseQuery koşulları "Okyanusu" ve "görüntüle" arar ve özgün belgede terimlerin yakınlık denetler. Belgeleri 1, 2 ve 3 Bu sorguyu Açıklama alanına eşleşmesi. "Okyanusu Görünüm" tümceciği için yerine tek tek sözcükleri bekliyoruz gibi belge 4 başlığında terim Okyanusu sahiptir, ancak bir eşleşme olarak değil dikkat edin. 

> [!Note]
> İle ayarlanan alanların sınırlamak sürece bir arama sorgusu bağımsız olarak Azure arama dizini içindeki tüm aranabilir alanları karşı yürütülür `searchFields` örnek arama isteğinde gösterildiği gibi parametre. Seçilen alanlar hiçbirinde eşleşen belgeleri döndürülür. 

Tüm, söz konusu sorgu eşleşen belgelerdir 1, 2, 3. 

## <a name="stage-4-scoring"></a>4. Aşama: Puanlama  

Her belge arama sonuç kümesinde bir ilgi puan atanır. İlgi puanının derece yüksek en iyi bir kullanıcı tarafından arama sorgusu ifade edilen sorunuzu bu belgeleri işlevdir. Puan eşleşen koşulları istatistiksel özelliklerini göre hesaplanır. Puanlama formülü çekirdeği [TF/IDF (terim sıklığı ters belge sıklığı)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf). Nadir ve sık kullanılan terimleri içeren sorgularda TF/IDF nadir terimi içeren sonuçları yükseltir. Örneğin, tüm Wikipedia makalelerde kuramsal bir dizinde belgelerden sorguyla eşleşen *Başkanı*, üzerinde eşleşen belgeler *Başkanı* üzerinde eşleşen belgeler daha fazla ilgili kabul edilip edilmediğini.


### <a name="scoring-example"></a>Puanlama örneği

Bizim örnek sorgu eşleşen üç belgeleri geri çağırma:
~~~~
search=Spacious, air-condition* +"Ocean view"  
~~~~
~~~~
{  
  "value": [
    {
      "@search.score": 0.25610128,
      "id": "1",
      "title": "Hotel Atman",
      "description": "Spacious rooms, ocean view, walking distance to the beach."
    },
    {
      "@search.score": 0.08951007,
      "id": "3",
      "title": "Playa Hotel",
      "description": "Comfortable, air-conditioned rooms with ocean view."
    },
    {
      "@search.score": 0.05967338,
      "id": "2",
      "title": "Ocean Resort",
      "description": "Located on a cliff on the north shore of the island of Kauai. Ocean view."
    }
  ]
}
~~~~

Çünkü belge 1 sorgu en iyi eşleşen. her iki terim *spacious* ve gerekli deyimi *Okyanusu Görünüm* Açıklama alanına oluşur. Sonraki iki belgeleri yalnızca tümcecik eşleşen *Okyanusu Görünüm*. Bunlar aynı şekilde sorguyla eşleşen olsa bile belge 2 ve 3 ilgi puanını farklıdır şaşırtıcı olabilir. Puanlama formülü yalnızca TF/IDF'den daha fazla bileşenlere sahip olmasıdır. Bu durumda, belge 3 açıklamasını kısa olduğundan biraz daha yüksek bir puan atanmıştır. Hakkında bilgi edinin [Lucene'nın pratik Puanlama formülü](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) ilgi puan alan uzunluğu ve diğer etkenlere bağlı nasıl etkileyebilir anlamak için.

Bazı sorgu türü (joker karakter öneki, regex) her zaman genel belge puan için sabit bir puan katkıda bulunun. Bu sorgu genişletme bulunan eşleşmeler sonuçlarında dahil edilecek sağlar ancak derecelendirme etkilemeden. 

Bu önemli neden bir örnek gösterilmektedir. Giriş farklı koşullarını ("tur", "tourettes" ve "tourmaline" bulunan eşleşmeleri ile girişi "tur *" düşünebilirsiniz) çok sayıda olası eşleşmeleri ile kısmi dize olduğundan önek aramaları da dahil olmak üzere, joker karakter aramaları tanımına göre belirsiz. Bu sonuçları yapısını verildiğinde, hangi koşulları diğerlerinden daha değerli makul çıkarsamak için bir yolu yoktur. Bu nedenle, biz terim sıklıklarını sonuçları türleri joker karakter, önek ve regex sorgularda Puanlama zaman yoksay. Kısmi ve tam koşulları içeren bir çok parçalı arama isteğinde kısmi giriş sonuçlarından büyük olasılıkla beklenmeyen eşleşen doğru sapması önlemek için sabit bir puan ile birleştirilir.

### <a name="score-tuning"></a>Puan ayarlama

Azure Search'te ilgi puanları ayarlamak için iki yolu vardır:

1. **Profilleri Puanlama** bir dizi kurala göre sonuçları dereceli listesi belgelerde Yükselt. Bizim örneğimizde, biz Açıklama alanına eşleşen belgeleri daha ilgili Başlık alanında eşleşen belgeleri düşünebilirsiniz. Ayrıca, dizinimizi her otel için bir fiyat alan olsaydı, biz alt fiyat belgelerle yükseltmek. Daha fazla öğrenmek için [Puanlama profilleri arama dizinine ekleyin.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)
2. **Terim** (yalnızca kullanılabilir tam Lucene sorgu söz dizimi) artırma işleci sağlar `^` sorgu ağaç herhangi bir kısmını uygulanabilir. Önek arama yerine örneğimizde *air-condition*\*, bir arama için tam terimi *air-condition* veya önek, ancak tam terimini eşleşen belgeleri Terim sorguya artırma uygulayarak daha yüksek derece: * hava durumu ^ 2 || Air-Condition **. Daha fazla bilgi edinmek [terim](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).


### <a name="scoring-in-a-distributed-index"></a>Dağıtılmış bir dizinde Puanlama

Azure Search'te tüm dizinleri otomatik olarak birden çok parça bölünür bize birden çok düğüm arasında dizin hizmeti ölçek sırasında hızlı bir şekilde dağıtmak veren yukarı veya ölçeklendirin. Bir arama isteğine verildiğinde, her parça karşı bağımsız olarak verilir. Her parça sonuçlarından ardından birleştirilmiş ve (diğer sıralamaya tanımlanmışsa) puana göre sıralanmış. Puanlama işlevi ağırlıkları değil tüm parça terim sıklığı parça tüm belgelerde ters belge sıklığının karşı sorgu bilmeniz önemlidir!

Bu bir ilgi puan anlamına gelir *verebilir* üzerinde farklı parça bulunuyorsa aynı belgeler için farklı olabilir. Neyse ki, bu farklara belge dizinde sayısı daha fazla bile terim dağıtım nedeniyle büyüdükçe kayboluyor eğilimindedir. Varsaymak mümkün değildir üzerinde hangi parça herhangi belirli bir belgeye yerleştirilir. Ancak, bir belge anahtarı değiştirmez varsayıldığında, bu her zaman aynı parça atanır.

Genel olarak, belge puan sipariş kararlılık önemliyse belgeleri sıralama için en iyi öznitelik değil. Örneğin, iki özdeş bir puan belgelerle verildiğinde, hangisinin aynı sorgu sonraki çalıştırmalarında ilk olarak görünür garantisi yoktur. Belge puan sonuçları kümesindeki diğer belgeleri göre belge ilgi genel bir fikir yalnızca vermesi gerekir.

## <a name="conclusion"></a>Sonuç

Internet arama motorları başarısını tam metin araması için beklentiler özel veriler üzerinde yükseltilmiş. Arama deneyimi neredeyse her türlü için şimdi koşulları yanlış yazılmış veya eksik olduğunda bile bizim hedefi anlamak için altyapısı bekliyoruz. Hatta eşdeğer terimler veya hiçbir zaman gerçekte belirtilen eş anlamlıları dayalı eşleşmeler bekliyoruz.

Teknik açıdan, tam metin araması Gelişmiş bir dil analiz ve biçimlendirebilir, genişletin ve ilgili bir sonuç sunmak için sorgu terimlerinin dönüştürme şekilde işlemeye sistematik bir yaklaşım gerektiren yüksek oranda karmaşık bir işlemdir. Devralınan karmaşıklık göz önüne alındığında, bir sorgu sonucunu etkileyen faktörler çok vardır. Bu nedenle, tam metin araması mekanikleri anlamak için zaman yatırım, beklenmeyen sonuçlara çalışmaya çalışırken somut avantajları sunar.  

Bu makalede, Azure Search bağlamında tam metin araması incelediniz. Olası nedenleri ve çözümlemeleri sorgu karşılaşılan adresleme tanımak için yeterli arka plan verir umuyoruz. 

## <a name="next-steps"></a>Sonraki adımlar

+ Örnek dizini oluşturun, farklı sorguları deneyin ve sonuçları gözden geçirin. Yönergeler için bkz: [oluşturmak ve Portalı'nda bir dizin sorgu](search-get-started-portal.md#query-index).

+ Ek sorgu sözdiziminde deneyin [Search belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) örnek bölümüne veya [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) Portalı'nda arama Gezgini'nde.

+ Gözden geçirme [profilleri Puanlama](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) sıralama, arama uygulamanızda ayarlamak istiyorsanız.

+ Uygulamayı öğrenin [dile özgü sözcük çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Özel çözümleyiciler yapılandırma](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) en az işleme veya belirli alanları üzerindeki özel işleme için.

+ [Standart ve İngilizce çözümleyiciler karşılaştırmak](http://alice.unearth.ai/)) yan yana bu demo web sitesinde. 

## <a name="see-also"></a>Ayrıca bkz.

[REST API belgelerde arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

[Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

[Tam Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 

[Arama sonuçlarını işleme](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
