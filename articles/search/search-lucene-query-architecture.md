---
title: Tam metin arama motoru (Lucene) mimarisi - Azure Search
description: İlgili Azure arama için tam metin araması için Lucene sorgu işleme ve belge alma kavramları açıklaması.
manager: jlembicz
author: yahnoosh
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: jlembicz
ms.custom: seodec2018
ms.openlocfilehash: bc183cb8ac2155b8dd31dc603d70506ad3d5e20a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65797474"
---
# <a name="how-full-text-search-works-in-azure-search"></a>Metin arama Azure Search'te tam nasıl çalışır

Bu makalede Azure Search'te Lucene tam metin araması çalışmasını daha derin bir anlayış isteyen geliştiricilere yöneliktir. Metin sorguları için Azure Search, sorunsuz bir şekilde beklenen sonuçları Çoğu senaryoda verecektir ancak bazen "kapalı" şekilde görünen bir sonuç alabilirsiniz. Bu durumda, bir arka plan Lucene sorgu yürütme dört aşamada sahip (ayrıştırma, sözcük analizi sorgu, belge Puanlama eşleşen) belirli sorgu parametreleri ya da istenen dağıtılacak dizin yapılandırma değişiklikleri belirlemenize yardımcı olabilir sonucu. 

> [!Note] 
> Azure arama için tam metin araması Lucene kullanır, ancak Lucene tümleştirme kapsamlı değildir. Biz seçmeli olarak kullanıma sunar ve Azure Search'e önemli senaryoları etkinleştirmek için Lucene işlevselliğini genişletme. 

## <a name="architecture-overview-and-diagram"></a>Mimariye genel bakış ve diyagramı

Tam metin arama sorgusu işleme arama terimlerini ayıklamak için sorgu metni ayrıştırma ile başlar. Arama motoru, eşleşen terimleri belgelerle almak için bir dizin kullanır. Tek tek sorgu terimleri bazen ayrılmış ve olası bir eşleşme olarak ne değerlendirilebilir üzerinde daha geniş bir net atanacak yeni formlar halinde yeniden oluşturulur. Bir sonuç kümesi, ardından her bir eşleşen belgeyi için atanmış bir ilgi puanı olarak sıralanır. Bu Sıralı listenin üst kısmındaki arama uygulamaya döndürülür.

Açıklandı, sorgu yürütme dört aşamadan oluşur: 

1. Sorgu ayrıştırma 
2. Sözcük analizi 
3. Belge alma 
4. Puanlama 

Aşağıdaki diyagramda bir arama isteği işlemek için kullanılan bileşenleri göstermektedir. 

 ![Azure Search'te Lucene sorgu mimarisi diyagramı][1]


| Başlıca bileşenler | İşlev açıklaması | 
|----------------|------------------------|
|**Sorgu Çözümleyicileri** | Sorgu işleçleri sorgu terimleriyle ayırmak ve arama motoru için gönderilecek sorgu yapısı (sorgu ağacı) oluşturun. |
|**Çözümleyiciler** | Sorgu koşullarınızda sözcük analizi gerçekleştirin. Bu işlem, dönüştürme, kaldırma veya sorgu koşullarını genişletme içerebilir. |
|**Dizin** | Depolama ve dizinli belgelerden ayıklanan aranabilir koşulları düzenlemek için kullanılan bir verimli veri yapısı. |
|**Arama motoru** | Alır ve ters dizin içeriğine göre eşleşen belgelerin puanlar. |

## <a name="anatomy-of-a-search-request"></a>Arama isteği anatomisi

Arama sonuç kümesinde döndürülmesi gereken tam bir belirtim isteğidir. Hiçbir ölçüt içermeyen boş bir sorgu olduğu basit haliyle. Daha gerçekçi bir örnek, belki de büyük olasılıkla bir filtre ifadesi ve sıralama kuralları ile belirli alanlar için kapsamlı, birkaç sorgu terimleriyle parametrelerini içerir.  

Aşağıdaki örnek, Azure Search kullanarak gönderebilir arama isteği [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).  

~~~~
POST /indexes/hotels/docs/search?api-version=2019-05-06
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

1. Fiyat en az 60 ABD Doları ve az 300 ABD Doları olduğu belgeleri filtreler.
2. Sorguyu yürütür. Bu örnekte, arama sorgu ifadeleri ve koşulları oluşur: `"Spacious, air-condition* +\"Ocean view\""` (kullanıcılar genellikle noktalama işareti girin yoktur, ancak örnekte dahil olmak üzere, verir bize nasıl Çözümleyicileri ele açıklamak). Bu sorgu için arama motoru açıklama tarar ve belirtilen başlık alanları `searchFields` "Okyanusu görünümü" içeren belgeleri ve ayrıca "spacious" terimi veya ön ekine sahip koşulları "air-condition". `searchMode` Parametresi herhangi bir terim (varsayılan) ya da bir terim olduğu açıkça gerekli durumlarda bunların tümünde eşleştirmek için kullanılır (`+`).
3. Yakınlık belirli coğrafi konuma göre Oteller ayarlayın ve ardından arama uygulamaya döndürülen sonuç sıralar. 

Bu makalede çoğunu hakkında işlemesidir *arama sorgusu*: `"Spacious, air-condition* +\"Ocean view\""`. Filtreleme ve sıralama kapsam dışına çıkmadan. Daha fazla bilgi için [arama API başvuru belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a>1\. Aşama: Sorgu ayrıştırma 

Belirtildiği gibi sorgu dizesi isteğin ilk satırı verilmiştir: 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

Sorgu ayrıştırıcı işleçleri ayıran (gibi `*` ve `+` örnekte) aramaya ilişkin koşulları ve arama sorguyu deconstructs *alt sorgular* desteklenen bir türde: 

+ *Terim sorgu* (gibi spacious) tek başına şartları
+ *ifade sorgu* tırnak işaretli koşulların (gibi Okyanusu görünümü)
+ *ön ek sorgu* koşulları bir önek işleci için `*` (air-condition gibi)

Desteklenen sorgu türleri tam listesi için bkz. [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

Alt sorgu ile ilişkili işleçleri sorgu "olmalıdır" veya "olmalıdır" belirlemek için bir eşleşme olarak kabul edilmesi bir belge sırayla memnun. Örneğin, `+"Ocean view"` "" nedeniyle zorunluluktur `+` işleci. 

Sorgu ayrıştırıcı, alt sorgularda halinde yeniden yapılandırır bir *sorgu ağacına* arama altyapısı, (sorguyu temsil eden bir iç yapısı) geçirir. Sorgu ayrıştırma ilk aşamada, sorgu ağacına şöyle görünür.  

 ![Boolean searchmode her sorgu][2]

### <a name="supported-parsers-simple-and-full-lucene"></a>Desteklenen Çözümleyicileri: Basit ve tam Lucene 

 Azure arama, iki farklı bir sorgu dili kullanıma sunan `simple` (varsayılan) ve `full`. Ayarlayarak `queryType` parametresi arama isteğinizle birlikte size sorgu ayrıştırıcı hangi sorgu dili, bilebilmesi işleçler ve söz dizimi yorumlama seçin. [Basit bir sorgu dili](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) sezgisel ve sağlam, genellikle, kullanıcı giriş olarak yorumlamak uygun olan-istemci tarafı işleme olmadan. Bu web arama motorlarından tanıdık sorgu işleçleri destekler. [Tam Lucene sorgu dili](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), ayarlayarak erişmenizi `queryType=full`, daha fazla işleçler ve sorgu türleri gibi joker karakter, belirsiz, normal ifade ve alan kapsamlı sorgular için destek ekleyerek varsayılan Basit Sorgu Dili genişletir. Örneğin, Basit Sorgu söz dizimi içinde gönderilen bir normal ifade, bir sorgu dizesi ve bir ifade yorumlanacağını. Bu makaledeki örnek istek tam Lucene sorgu dili kullanır.

### <a name="impact-of-searchmode-on-the-parser"></a>Ayrıştırıcının üzerinde searchMode etkisini 

Ayrıştırma etkileyen başka bir arama istek parametresi `searchMode` parametresi. Boole sorgular için varsayılan işleç denetler: (varsayılan) veya tüm.  

Zaman `searchMode=any`, alan sınırlayıcı spacious arasında varsayılan davranıştır ve air-condition veya (`||`), örnek sorgu metni eşdeğer hale getirme: 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

Açık işleçler gibi `+` içinde `+"Ocean view"`, Boole sorgu oluşturma, kesindir (terimi *gerekir* eşleşen). Kalan koşulları yorumlama daha az belirgin: spacious ve air-condition. Arama motoru Okyanusu görünümünde eşleşen bulmalısınız *ve* spacious *ve* air-condition? Veya Okyanusu görünümü bulmalısınız yanı sıra *tek* kalan koşulları? 

Varsayılan olarak (`searchMode=any`), arama motoru daha geniş yorumu varsayar. Her iki alan *gereken* eşlenmesi, yansıtma "veya" semantiği. İlk sorgu ağacına daha önce gösterilen iki gerekir"işlemler", varsayılan gösterir.  

Artık ayarladığımız varsayalım `searchMode=all`. Bu durumda, alan "ve" bir işlem olarak yorumlanır. Her kalan koşulları hem de bir eşleşme olarak nitelemek için belge içinde bulunmalıdır. Sonuçta elde edilen örnek sorgu şu şekilde yorumlanır: 

~~~~
+Spacious,+air-condition*+"Ocean view"
~~~~

Bu sorgu için bir değiştirilen sorguyu ağacı eşleşen bir belge, tüm üç alt sorgular kesişimi olduğu şu şekilde olacaktır: 

 ![Tüm Boole sorgu searchmode][3]

> [!Note] 
> Seçme `searchMode=any` üzerinden `searchMode=all` en iyi bir karar temsili sorgu çalıştırarak gelen olduğu. İşleçler (arama belge depoladığında ortak) dahil olasılığı olan kullanıcılar sonuçları daha sezgisel, bulabileceğiniz `searchMode=all` Boole sorgu yapıları bildirir. Etkileşim özelliği hakkında daha fazla bilgi için `searchMode` ve işleçler [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a>2\. Aşama: Sözcük analizi 

Sözcük temelli çözümleyiciler işlem *süreli sorgular* ve *tümcecik sorguları* sorgu ağacına yapılandırılmış sonra. Bir çözümleyici ayrıştırıcı tarafından izin verilen metin girişleri kabul eder, metin ve ardından geri simgeleştirilmiş gönderir koşulları sorgu ağacına genişletilebilecek şekilde işler. 

En yaygın sözcük analizi biçimindedir *dil analizi* hangi sorgu dönüştürmeler için belirli bir dile özgü kurallar temel alınarak koşulları: 

* Bir sözcük kökü forma bir sorgu terimine azaltma 
* Gerekli olmayan sözcükler kaldırma ("" gibi bir Durma sözcükleri veya "ve" İngilizce) 
* Bir bileşik sözcük bölme bileşeni parçalara 
* Küçük büyük harf sözcük büyük/küçük harfleri 

Tüm bu işlemler, kullanıcı tarafından sağlanan metin girişi ve dizinde depolanan koşulları arasındaki farklar silmek için eğilimindedir. Bu işlemler, metin işleme ötesine gidin ve dil bilgisi gerektirir. Bu dil tanıma katmanı eklemek için Azure Search uzun listesini destekler [dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene hem de Microsoft gelen.

> [!Note]
> Analiz gereksinimleri en az senaryonuz üzerinde ayrıntılı olarak değişebilir. Önceden tanımlanmış Çözümleyicileri seçme birini veya kendi oluşturarak sözcük analizi karmaşıklığını denetleyebilirsiniz [özel çözümleyici](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search). Çözümleyiciler aranabilir alanları için kapsamlı ve alan tanımının bir parçası belirtilir. Bu, alan başına temelinde sözcük analizi değiştirmenizi sağlar. Belirtilmezse, *standart* Lucene çözümleyici kullanılır.

Bizim örneğimizde, analiz önce ilk sorgu ağacına terimi "Spacious," bir büyük harf "S" ve sorgu ayrıştırıcı (virgül bir sorgu dili işleci olarak kabul edilmez) sorgu terimine bir parçası olarak yorumlar virgül ile sahiptir.  

Varsayılan Çözümleyicisi terimi işlediğinde, "Okyanusu görünümü" ve "spacious" küçük ve virgül karakteri kaldırmak. Değiştirilen sorguyu ağacı aşağıdaki gibi görünür: 

 ![Analiz edilen koşullarıyla Boole Sorgusu][4]

### <a name="testing-analyzer-behaviors"></a>Sınama Çözümleyicisi davranışları 

Bir çözümleyici davranışını kullanılarak sınanabilir [analiz API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer). Ne Çözümleyicisi verilen koşulları oluşturacak görmek için analiz etmek istediğiniz metni belirtin. Örneğin, standart Çözümleyicisi metin "air-condition" nasıl işleyeceğini görmek için aşağıdaki istek gönderebilir:

~~~~
{
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

Standart Çözümleyicisi, bunları konumlarına (tümcecik eşleştirmek için kullanılır) yanı sıra başlangıç ve bitiş kaydırmalar (isabet vurgulama için kullanılan) gibi özniteliklerle yorumlama aşağıdaki iki belirteçler içine giriş metni keser:

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

### <a name="exceptions-to-lexical-analysis"></a>Sözcük analizi için özel durumlar 

Sözcük temelli analiz tüm koşullar – bir terim sorgu ya da bir deyim sorgu gerektiren sorgu türleri için geçerlidir. Sorgu türleri – ön ek sorgu, joker karakter sorgu, normal ifade sorgu – tamamlanmamış koşullarıyla veya belirsiz bir sorgu için geçerli değildir. Bu terim ile ön ek sorgu gibi türleri sorgu `air-condition*` örneğimizde analysis aşaması atlayarak doğrudan sorgu ağaç olarak eklenir. Yalnızca dönüşümü gerçekleştirdiği sorgu türlerine koşullarınızda harfe.

<a name="stage3"></a>

## <a name="stage-3-document-retrieval"></a>3\. Aşama: Belge alma 

Belge alma dizini terimleriyle eşleşen belgeleri bulmak için ifade eder. Bu aşama, en iyi bir örnek anlaşılır. Basit aşağıdaki şemayı içerecek bir Oteller dizinini ile başlayalım: 

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

Daha fazla bu dizin aşağıdaki dört belgeler içerdiğini varsayın: 

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

Alma anlamak için dizin oluşturma hakkında bazı temel bilgileri öğrenmenize yardımcı olur. Bir ters dizin, aranabilir her alan için bir depolama birimidir. Ters dizin içinde tüm belgelerden tüm koşulları sıralı bir listesidir. Bunu, aşağıdaki örnekte dikkati oluştuğu belgelerin listesini her terim eşlenir.

Ters dizin koşullarını oluşturmak için arama motoru sorgu işleme sırasında neler için benzer belgelerin içeriğini üzerinden sözcük temelli analiz gerçekleştirir:

1. *Metin girişi* bir Çözümleyicisi, küçük harfleri, noktalama işaretleri ve Çözümleyicisi yapılandırmasına bağlı olarak belirli bir benzeri çıkartılır geçirilir. 
2. *Belirteçleri* metin analizi çıktısı alınır.
3. *Koşulları* dizine eklenir.

Bu ortak, ancak gerekli değildir, arama ve böylece sorgu terimleriyle dizini içinde koşulları gibi daha fazla konum operations dizin oluşturma için aynı çözümleyici kullanılacak.

> [!Note]
> Azure arama sayesinde dizinini oluşturmak için farklı Çözümleyicileri belirtin ve arama aracılığıyla ek `indexAnalyzer` ve `searchAnalyzer` alan parametreleri. Belirtilmemişse, çözümleyici kümesi `analyzer` özelliği, hem dizinleme ve arama için kullanılır.  

**Örnek Belge ters dizini**

İçin Örneğimiz için döndüren **başlık** alanı, ters dizinini aşağıdaki gibi görünür:

| Terim | Belge listesi |
|------|---------------|
| atman | 1 |
| Plaj | 2 |
| Otel | 1, 3 |
| Okyanusu | 4  |
| playa | 3 |
| çare | 3 |
| Retreat | 4 |

Yalnızca başlık alanında *otel* iki belgelerde gösterilir: 1, 3.

İçin **açıklama** alanı dizini şu şekildedir:

| Terim | Belge listesi |
|------|---------------|
| Air | 3
| ve | 4
| Plaj | 1
| koşuluna | 3
| deneyimli | 3
| distance | 1
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
| şunu | 1, 2
| - | 1
| görünüm | 1, 2, 3
| Yürüyen | 1
| örneklerini şununla değiştirin: | 3


**Dizini oluşturulan terimler karşı sorgu terimleriyle eşleşen**

Şimdi örnek sorgu için yukarıdaki ters dizinleri göz önünde bulundurulduğunda ve nasıl eşleşen belgeleri görmek için örnek sorgumuzu bulunamadı. Geri çağırma son sorgu ağacına şöyle görünür: 

 ![Analiz edilen koşullarıyla Boole Sorgusu][4]

Sorgu yürütme işlemi sırasında her sorgu karşı aranabilir alanları bağımsız olarak yürütülür. 

+ 1 (otel Atman) TermQuery, "spacious" eşleşme belgeleyin. 

+ PrefixQuery "air-condition *", tüm belgelerin eşleşmiyor. 

  Bu, bazen geliştiriciler kafasını karıştırabilirler bir davranıştır. Belgede Air-conditioned terimi mevcut olsa da varsayılan çözümleyici tarafından iki terimleri ayrılır. Kısmi koşulları içeren önek sorguları çözümlenmediğini geri çağırma. Bu nedenle "ön eki air-condition" ile koşulları ters dizininde aranabilir ve bulunamadı.

+ "Okyanusu görünümü" PhraseQuery, koşulları "Okyanusu" ve "görüntüleme" arar ve yakınlık özgün belgede koşulları denetler. 1, 2 ve 3 belgeleri, Açıklama alanına bu sorguyla eşleşen. Belge 4 terimi Okyanusu başlık olsa da bir eşleşme olarak değil "Okyanusu görünümü" tümceciği için yerine kelimeler bekliyoruz gibi dikkat edin. 

> [!Note]
> Kümesi alanları sınırlayacak sürece bir arama sorgusu bağımsız olarak Azure Search dizini içindeki tüm aranabilir alanları karşı yürütülür `searchFields` örnek arama istekte gösterildiği gibi parametre. Seçili alanları hiçbirinde eşleşen belgeler döndürülür. 

Tüm, söz konusu sorgu için eşleşen belgelerdir 1, 2, 3. 

## <a name="stage-4-scoring"></a>4\. Aşama: Puanlama  

Arama sonuç kümesinde her belgenin bir ilgi puanı atanır. İlgi puanı işlevi, en iyi bir kullanıcı arama sorgusu ifade edilen sorusunu bu belgeleri boyut sayısı daha yüksek olduğu. Puan eşleşen terimleri istatistiksel özelliklerine göre hesaplanır. Puanlama formülün temel [TF/IDF (terimi sıklığı ters belge sıklık düzeyi)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf). Nadir ve yaygın terimlerin bulunduğu sorgularda sonuçları nadir terimini içeren TF/IDF yükseltir. Örneğin, tüm Wikipedia makalelerde kuramsal bir dizinde belgelerden sorguyla eşleşen *Başkanı*, üzerinde eşleşen belgeler *Başkanı* üzerinde eşleşen belgeler daha fazla ilgili kabul edilip edilmediğini *.*


### <a name="scoring-example"></a>Örnek Puanlama

Bizim örnek sorgu eşleşen üç belgeleri Hatırla:
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

Çünkü belge 1 sorgu en iyi eşleşen. her iki terim *spacious* ve gerekli tümcecik *Okyanusu görünümü* Açıklama alanına oluşur. Sonraki iki belge ifadesini yalnızca eşleşen *Okyanusu görünümü*. Şaşırtıcı rağmen bunların aynı şekilde sorguyla eşleşen belge 2 ve 3 ilgi puanı farklı olabilir. Puanlama formül yalnızca TF/IDF değerinden daha fazla bileşen sahip olmasıdır. Bu durumda, belge 3 kısa bir açıklamasını olduğundan biraz daha yüksek bir puan atandı. Hakkında bilgi edinin [Lucene'nın pratik Puanlama formül](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) ilgi puanı alan uzunluğu ve diğer faktörlere nasıl etkileyebilir anlamak için.

Bazı sorgu türü (joker karakter öneki, normal ifade) her zaman sabit bir puan genel belge puana katkıda bulunun. Bu sonuçları, dahil edilecek eşleşme sorgu genişletme bulunamadı sağlar ancak sıralamasını etkilemeden. 

Neden bu önemli bir örnek gösterilmektedir. Giriş çok büyük birkaç farklı koşulları olası eşleşme ile kısmi dize olduğundan önek aramaları da dahil olmak üzere, joker karakter aramalarını tanımına göre belirsiz (eşleşme bulunamadı "turları üzerinde", "tourettes" ile "Turu *" girdisi göz önünde bulundurun, ve " tourmaline"). Bu sonuçları gereği, makul hangi koşulları diğerlerine göre daha değerli çıkarsamak için hiçbir yolu yoktur. Bu nedenle, biz türleri joker karakter, önek ve normal ifade sorgularda sonuçlarını Puanlama, terim frekansları yoksayın. Kısmi ve tam koşulları içeren bir çok bölümlü arama istekte kısmi giriş sonuçlardan büyük olasılıkla beklenmeyen eşleşme doğrultusunda sapması önlemek için sabit bir puan ile birleştirilir.

### <a name="score-tuning"></a>Puan ayarlama

Azure Search'te ilgi puanları ayarlamak için iki yolu vardır:

1. **Puanlama profilleri** birtakım kurallara göre sonuçları kişilerinin sıralı bir listesini belgelerde tanıtın. Bizim örneğimizde, biz Açıklama alanına eşleşen belgelerin daha ilgili Başlık alanında eşleşen belgelerin düşünebilirsiniz. Ayrıca, dizinimizi her otel için bir fiyat alanı olsaydı, biz daha düşük fiyatla belgelerle Yükselt. Daha fazla öğrenin [bir arama dizini için Puanlama profilleri ekleyin.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)
2. **Terim artırma** artırma işleci (yalnızca tam Lucene sorgu söz dizimi içinde kullanılabilir) sağlayan `^` sorgu ağacına herhangi bir bölümü için uygulanabilir. Ön ek arama yerine örneğimizde *air-condition*\*, aşağıdakilerden arama için tam terimi *air-condition* veya ön ek, ancak tam koşulu ile eşleşen belgeleri Terim sorguya boost uygulayarak daha yüksek derece: * hava durumu ^ 2 || Air-Condition **. Daha fazla bilgi edinin [terim artırma](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).


### <a name="scoring-in-a-distributed-index"></a>Dağıtılmış bir dizine Puanlama

Azure Search'te tüm dizinleri otomatik olarak birden çok parçalara bölmek yukarı veya ölçeği birden çok düğüm arasında dizin hizmet ölçeklendirme sırasında hızlı bir şekilde dağıtmak bize izin verme. Arama isteği verildiğinde, her parça karşı bağımsız olarak verilir. Her parça sonuçlardan ardından birleştirilmiş ve (diğer sıralama yok tanımlandıysa) puana göre sıralanmış. Puanlama işlevi ağırlıkları terimi sıklığı ters belge sıklığının parça içindeki tüm belgelerde karşı değil tüm parçalar arasında sorgu bilmek önemlidir!

İlgi puanı başka bir deyişle *verebilir* bunlar üzerinde farklı parçalardaki bulunuyorsa aynı belgeler için farklı olabilir. Neyse ki, bu farklara daha fazla bile terimi dağıtım nedeniyle belge dizinde sayısı arttıkça kayboluyor eğilimindedir. Varsaymak mümkün değildir verilen herhangi bir belge üzerinde hangi parçanın yerleştirilir. Ancak, bir belge anahtarını değişmez varsayıldığında, bu her zaman aynı parçaya atanır.

Genel olarak, belge puanı sipariş kararlılık önemliyse belgeleri sıralama için en iyi öznitelik değil. Örneğin, iki belgeyle aynı bir puan göz önünde bulundurulduğunda, hangisinin sonraki çalışmaları aynı sorgu içinde ilk olarak görünür garantisi yoktur. Belge puanı sonuçları kümesindeki belge ilgi diğer belgelere göre genel bir fikir yalnızca vermeniz gerekir.

## <a name="conclusion"></a>Sonuç

İnternet arama motorları başarısını beklentileri tam metin araması için özel veriler üzerinde harekete. Arama deneyimi hemen hemen her tür için artık şartları yazılmış veya tamamlanmamış olsa bile, Amacımız anlamak için altyapı bekliyoruz. Biz bile göre eşdeğer terimler veya asla gerçekten belirtilen eş anlamlılar yakın eşleşmeler bekleyebilirsiniz.

Teknik açıdan, tam metin arama Gelişmiş dil analizi ve işleme görüntülerden ayıklayın, genişletin ve ilgili bir sonuç sunmak için sorgu terimleriyle dönüştürme yollarla sistematik bir yaklaşım gerektiren son derece karmaşık olur. Devralınan karmaşıklıkları göz önünde bulundurulduğunda, çok sayıda sorgu sonucunu etkileyen faktörler vardır. Bu nedenle, tam metin araması mekanikleri anlamak için zaman harcayarak, beklenmeyen sonuçlar iş çalışırken somut avantaj sunar.  

Bu makalede, Azure Search bağlamında tam metin araması incelediniz. Olası nedenler ve çözümler yaygın sorgu sorunları çözmeye yönelik tanımak için yeterli arka plan size umuyoruz. 

## <a name="next-steps"></a>Sonraki adımlar

+ Örnek dizini oluşturma, farklı sorguları deneyin ve sonuçları gözden geçirin. Yönergeler için [oluşturun ve Portalı'nda dizin sorgulama](search-get-started-portal.md#query-index).

+ Ek sorgu söz dizimi gelen deneyin [arama belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) örnek bölümünde veya [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) portalında arama Gezgini.

+ Gözden geçirme [Puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) derecelendirme arama uygulamanızdaki ayarlamak istiyorsanız.

+ Nasıl uygulayabileceğinizi öğrenin [dile özel sözcük temelli çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Özel çözümleyiciler yapılandırma](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) için en az işleme ya da belirli alanlarda özel işleme.

## <a name="see-also"></a>Ayrıca bkz.

[Search belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

[Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

[Tam Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 

[Arama sonuçlarını işleme](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
