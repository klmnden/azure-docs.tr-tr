---
title: Çözümleyici için dil ve metin işleme - Azure Search
description: Standart Lucene özel, önceden tanımlanmış veya dile özgü alternatif metin aranabilir alanları değiştirmek için bir dizin için Ata çözümleyicileri varsayılan.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: heidist
manager: cgronlun
author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 121b5542f9388355b97744aa224ac824dd8d8728
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867214"
---
# <a name="analyzers-for-text-processing-in-azure-search"></a>Metin işleme Azure Search'te çözümleyiciler

Bir *Çözümleyicisi* bir bileşeni olan [tam metin araması](search-lucene-query-architecture.md) sorgu dizeleri ve dizinli belgelerde metin işleme için sorumlu. Aşağıdaki dönüştürmeleri analiz sırasında tipik şunlardır:

+ Gerekli olmayan sözcükler (stopword) ve noktalama işaretleri kaldırılır.
+ İfadeleri ve tire ile ayrılmış sözcüklerin bileşeni parçalara ayrılır.
+ Büyük küçük harfleri sözcüklerdir.
+ Böylece bir eşleşme şimdiki bağımsız olarak bulunabilir sözcükleri kök formları azaltılır.

Dil Çözümleyicileri dönüştürme basit bir metin giriş ya da kök formları bilgi depolanması ve alınması için verimli olan. Dönüştürme, dizini oluşturulurken, dizin oluşturma sırasında ortaya çıkar ve daha sonra yeniden dizin okunduğunda arama sırasında. Büyük olasılıkla her iki işlemleri için aynı metin Çözümleyicisi'ni kullanırsanız, beklediğiniz sonuçları edinin.

Azure Search kullanan [standart olarak Lucene çözümleyici](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) varsayılan olarak. Alan alanlı temelinde Varsayılanı geçersiz kılabilirsiniz. Bu makalede, seçim aralığını açıklar ve özel analiz için en iyi yöntemleri sunar. Ayrıca, anahtar senaryolar için örnek yapılandırma sağlar.

## <a name="supported-analyzers"></a>Desteklenen çözümleyiciler

Aşağıdaki listede, hangi çözümleyiciler Azure arama'yı desteklediği açıklanmaktadır.

| Kategori | Açıklama |
|----------|-------------|
| [Standart olarak Lucene çözümleyici](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Varsayılan. Belirtiminin ya da yapılandırma gereklidir. Bu genel amaçlı Çözümleyicisi çoğu diller ve senaryoları için iyi gerçekleştirir.|
| Önceden tanımlanmış çözümleyiciler | Tamamlanmış bir ürün olarak kullanılmaya yönelik olarak sunulan-, ile sınırlı özelleştirme bulunur. <br/>İki tür vardır: özelleştirilmiş ve dili. Bunları "önceden tanımlanmış" kılan, bunları hiçbir özelleştirme adıyla başvurduğunu olduğu. <br/><br/>[Özelleştirilmiş (dil belirsiz) Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable) metin girişleri özel işleme ya da en az işleme gerektirdiğinde kullanılır. Önceden tanımlı olmayan dil Çözümleyicileri dahil **Asciifolding**, **anahtar sözcüğü**, **deseni**, **basit**, **Durdur**, **Boşluk**.<br/><br/>[Dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) , zengin dil desteği için tek tek dillerin gerektiğinde kullanılır. Azure Search, Lucene dil çözümleyicilerini 35 ve 50 Microsoft doğal dil işleme Çözümleyicileri destekler. |
|[Özel çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Kullanıcı tanımlı bir yapılandırma mevcut öğelerin bir belirteç Oluşturucu (gerekli) ve isteğe bağlı filtreler (char veya belirteç) oluşan bir birleşimi.|

Önceden tanımlanmış bir çözümleyici gibi özelleştirebilirsiniz **deseni** veya **Durdur**kısmında belgelenen diğer seçenekleri kullanmak için [önceden tanımlanmış çözümleyici başvurusu](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable). Yalnızca bir önceden tanımlanmış Çözümleyicileri ayarlayabileceğiniz seçenekleri vardır. Herhangi bir özelleştirme ile gibi bir ad ile yeni yapılandırmanızı sağlamak *myPatternAnalyzer* Lucene deseni Çözümleyicisi'nden ayırmak için.

## <a name="how-to-specify-analyzers"></a>Çözümleyiciler belirtme

1. (yalnızca özel çözümleyiciler için) Oluşturma bir **Çözümleyicisi** dizin bölümü. Daha fazla bilgi için [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) ve ayrıca [özel çözümleyiciler > oluşturma](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search#create-a-custom-analyzer).

2. Üzerinde bir [alan tanımı](https://docs.microsoft.com/rest/api/searchservice/create-index) dizinde ayarlamak **Çözümleyicisi** özelliğini hedef analyzer'ın adı (örneğin, `"analyzer" = "keyword"`. Geçerli değerler, önceden tanımlanmış Çözümleyicisi, dil Çözümleyicisi veya Ayrıca dizin şemasında tanımlanan özel çözümleyici adını içerir.

3. İsteğe bağlı olarak, bir yerine **Çözümleyicisi** özelliği, dizin oluşturma ve kullanarak sorgulama için farklı Çözümleyicileri ayarlayabilirsiniz **indexAnalyzer** ve **searchAnalyzer'** alan Parametreler. 

3. Bir alan tanımı için bir çözümleyici ekleme dizini bir yazma işlemi artmasına neden olur. Eklerseniz bir **Çözümleyicisi** mevcut bir dizine aşağıdakileri unutmayın:
 
 | Senaryo | Etki | Adımlar |
 |----------|--------|-------|
 | Yeni alan ekleme | En az | Alan şemada henüz yoksa, alanın henüz fiziksel olarak bulunmayı dizininizdeki olmadığından yapmak için hiçbir alan düzeltme yoktur. Kullanım [dizin güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) ve [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bu görev için.|
 | Bir çözümleyici varolan dizinli alana ekleyin. | Yeniden oluşturma | Bu alan için ters dizini baştan ayarlama oluşturulması gerekir ve bu alanların içeriğini reindexed gerekir. <br/> <br/>Etkin geliştirme aşamasındaki dizinler için [Sil](https://docs.microsoft.com/rest/api/searchservice/delete-index) ve [oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) dizinin yeni alanın tanımını seçin. <br/> <br/>Üretimde dizinler için düzeltilmiş tanımı sağlamak ve kullanmaya başlamak için yeni bir alan oluşturmalısınız. Kullanım [dizin güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) ve [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) yeni alan birleştirmek için. Daha sonra planlanan bir dizin hizmeti, bir parçası olarak kullanılmayan alanları kaldırmak için dizin temizleyebilirsiniz. |

## <a name="tips-and-best-practices"></a>İpuçları ve en iyi uygulamalar

Bu bölümde, çözümleyiciler ile çalışma konusunda öneriler sunar.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Okuma-yazma için bir çözümleyici belirli gereksinimleriniz yoksa

Azure arama sayesinde dizinini oluşturmak için farklı Çözümleyicileri belirtin ve arama aracılığıyla ek `indexAnalyzer` ve `searchAnalyzer` alan parametreleri. Belirtilmemişse, çözümleyici kümesi `analyzer` özelliği, hem dizinleme ve arama için kullanılır. Varsa `analyzer` olan belirtilmemişse, varsayılan olarak standart Lucene çözümleyici kullanılır.

Genel bir kural belirli gereksinimleri dair sakınca yoksa aynı Çözümleyicisi hem dizin oluşturma ve sorgulama, için kullanmaktır. Baştan sona test etmeyi unutmayın. Arama ve dizin oluşturma zamanında metin işleme farklılık gösterdiğinde sorgu terimleri ve arama ve dizin oluşturma Çözümleyicisi yapılandırmaları zaman değil hizalanır dizini oluşturulan terimler arasında uyumsuzluk riskini çalıştırın.

### <a name="test-during-active-development"></a>Etkin geliştirme sırasında test etme

Standart Çözümleyicisi geçersiz kılan bir dizini yeniden derleme gerektirir. Mümkünse, üretim ortamına dizin çalışırken önce etkin geliştirme sırasında kullanmak için hangi Çözümleyicileri karar verin.

### <a name="inspect-tokenized-terms"></a>Parçalanmış koşullarını inceleyin

Beklenen sonuçları döndürmek bir arama başarısız olursa, büyük olasılıkla terim giriş sorguda ve dizindeki parçalanmış hüküm arasında belirteç tutarsızlıklar senaryodur. Belirteçler aynı olmayan iyileştirilene eşleşme başarısız. Simgeleştirici çıkış incelemek için kullanmanızı öneririz [analiz API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) araştırma aracı olarak. Yanıt belirteçleri, belirli bir çözümleyici tarafından oluşturulan oluşur.

### <a name="compare-english-analyzers"></a>İngilizce Çözümleyicileri karşılaştırın

[Arama Çözümleyicisi Tanıtımı](https://alice.unearth.ai/) standart olarak Lucene Çözümleyici, Lucene'nın İngilizce dil Çözümleyicisi ve Microsoft'un doğal dil İngilizce işlemci yan yana karşılaştırmasını gösteren bir üçüncü taraf tanıtım uygulaması. Dizin sabittir; Bu yaygın bir hikaye metni içerir. İçin her bir arama giriş sağlarsanız, her çözümleyici sonuçları bitişik bölmelerinde görüntülenen her Çözümleyicisi aynı dize nasıl işlediği hakkında bir fikir verir. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler birkaç önemli senaryolar için Çözümleyicisi tanımları gösterir.

<a name="Example1"></a>
### <a name="example-1-custom-options"></a>Örnek 1: Özel seçenekleri

Bu örnekte, özel seçeneklerle bir çözümleyici tanımı gösterilmektedir. Özel seçenekleri char filtreleri ve oluşturma denenmeden belirteci filtreleri için ayrı ayrı adlandırılmış yapıları belirtilen ve ardından Çözümleyicisi tanımında başvurulan. Önceden tanımlanmış öğeleri olarak kullanılan-olduğunu ve yalnızca adı tarafından başvurulan.

Bu örnekte yürüyen:

* Çözümleyiciler aranabilir bir alanı için alan sınıfının bir özelliği var.
* Özel bir çözümleyici dizin tanımı'nın bir parçasıdır. Bunu hafifçe (örneğin, bir filtre içinde tek bir seçenek özelleştirme) özelleştirilebilen veya birden fazla yerde özelleştirilebilir.
* Bu durumda, özel çözümleyici sırayla bir özelleştirilmiş standart simgeleştirici "my_standard_tokenizer" ve belirteç iki filtre kullanır "my_analyzer" olduğu: küçük harf ve özelleştirilmiş asciifolding filtre "my_asciifolding".
* Ayrıca, 2 özel char Filtreler "map_dash" ve "remove_whitespace" tanımlar. İkincisi tüm boşlukları kaldırır ancak ilk tüm tire alt çizgi ile değiştirir. UTF-8 kodlamalı eşleme kuralları boşluk olması gerekir. Char filtreleri önce simgeleştirme uygulanır ve sonuçta elde edilen belirteçleri (standart simgeleştirici sonlarını tire ve boşluk ancak alt çizgi) etkiler.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"my_analyzer"
        }
     ],
     "analyzers":[
        {
           "name":"my_analyzer",
           "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
           "charFilters":[
              "map_dash",
              "remove_whitespace"
           ],
           "tokenizer":"my_standard_tokenizer",
           "tokenFilters":[
              "my_asciifolding",
              "lowercase"
           ]
        }
     ],
     "charFilters":[
        {
           "name":"map_dash",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["-=>_"]
        },
        {
           "name":"remove_whitespace",
           "@odata.type":"#Microsoft.Azure.Search.MappingCharFilter",
           "mappings":["\\u0020=>"]
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizerV2",
           "maxTokenLength":20
        }
     ],
     "tokenFilters":[
        {
           "name":"my_asciifolding",
           "@odata.type":"#Microsoft.Azure.Search.AsciiFoldingTokenFilter",
           "preserveOriginal":true
        }
     ]
  }
~~~~

<a name="Example2"></a>
### <a name="example-2-override-the-default-analyzer"></a>Örnek 2: Varsayılan Çözümleyicisi geçersiz kıl

Standart Çözümleyicisi varsayılandır. Desen Çözümleyicisi gibi farklı bir önceden tanımlanmış Çözümleyicisi varsayılan yerine istediğinizi varsayalım. Özel seçenekleri emin değilseniz, yalnızca alan tanımı adı belirtmeniz gerekir.

"Çözümleyicisi" öğesi, alanlar tarafından temelinde standart Çözümleyicisi geçersiz kılar. Genel geçersiz kılma yoktur. Bu örnekte, `text1` deseni Çözümleyicisi kullanır ve `text2`, hangi bir çözümleyici belirtmeyen varsayılan kullanır.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text1",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"pattern"
        },
        {
           "name":"text2",
           "type":"Edm.String",
           "searchable":true
        }
     ]
  }
~~~~

<a name="Example3"></a>
### <a name="example-3-different-analyzers-for-indexing-and-search-operations"></a>Örnek 3: Dizin oluşturma ve arama işlemleri için farklı çözümleyiciler

API'ler farklı Çözümleyicileri için dizin oluşturma ve arama belirtmek için ek dizin özniteliklerini içerir. `searchAnalyzer` Ve `indexAnalyzer` öznitelikleri değiştirerek tek bir çift olarak belirtilmelidir `analyzer` özniteliği.


~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
     ],
  }
~~~~

<a name="Example4"></a>
### <a name="example-4-language-analyzer"></a>Örnek 4: Dil Çözümleyicisi

Diğer alanları varsayılan korumak (veya diğer bazı önceden tanımlanmış ya da özel Çözümleyicisi'ni kullanın ancak) farklı dillerde dizeleri içeren alanlar bir dil Çözümleyicisi kullanabilirsiniz. Bir dil Çözümleyicisi kullanırsanız, dizin oluşturma ve arama işlemleri için kullanılmalıdır. Bir dil Çözümleyicisi'ni kullanın alanlar dizinini oluşturmak için farklı Çözümleyicileri ve arama yapamazsınız.

~~~~
  {
     "name":"myindex",
     "fields":[
        {
           "name":"id",
           "type":"Edm.String",
           "key":true,
           "searchable":false
        },
        {
           "name":"text",
           "type":"Edm.String",
           "searchable":true,
           "indexAnalyzer":"whitespace",
           "searchAnalyzer":"simple"
        },
        {
           "name":"text_fr",
           "type":"Edm.String",
           "searchable":true,
           "analyzer":"fr.lucene"
        }
     ],
  }
~~~~

## <a name="next-steps"></a>Sonraki adımlar

+ Sunduğumuz kapsamlı açıklaması gözden [nasıl tam metin araması Azure Search'te çalışır](search-lucene-query-architecture.md). Bu makalede örnekler yüzeyine counter-intuitive görünebilir davranışları açıklamak için kullanılır.

+ Ek sorgu söz dizimi gelen deneyin [arama belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) örnek bölümünde veya [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) portalında arama Gezgini.

+ Nasıl uygulayabileceğinizi öğrenin [dile özel sözcük temelli çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Özel çözümleyiciler yapılandırma](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) için en az işleme ya da tek tek alanlarda özel işleme.

+ [Standart ve İngilizce Çözümleyicileri karşılaştırma](https://alice.unearth.ai/) bitişik bölmelerindeki bu demo web sitesinde. 

## <a name="see-also"></a>Ayrıca bkz.

 [Search belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

 [Tam Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 
 [Arama sonuçlarını işleme](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
