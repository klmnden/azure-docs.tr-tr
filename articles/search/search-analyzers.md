---
title: Çözümleyiciler Azure Search'te | Microsoft Docs
description: Ata çözümleyiciler değiştirmek için bir dizin aranabilir metin alanları için özel, önceden tanımlanmış veya dile özgü Alternatiflerle birlikte standart Lucene varsayılan.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: heidist
manager: cgronlun
author: HeidiSteen
ms.openlocfilehash: e858966fb5a15b84af1952399a5eff3ca50d0d59
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31795706"
---
# <a name="analyzers-in-azure-search"></a>Azure Search'te çözümleyiciler

Bir *Çözümleyicisi* bir bileşeni olan [tam metin araması](search-lucene-query-architecture.md) sorgu dizeleri ve dizinlenmiş belgeleri metin işleme sorumlu. Şu dönüşümleri Çözümleme sırasında tipik şunlardır:

+ Gerekli olmayan sözcükler (Durma sözcükleri) ve noktalama kaldırılır.
+ Tümcecikleri ve Tireli sözcükler bileşen bölümlere ayrılmıştır.
+ Alt ortası büyük harf sözcükler.
+ Böylece bir eşleşme zamanın bağımsız olarak bulunabilir sözcükler kök formlara azaltılır.

Dil Çözümleyicileri dönüştürme ilkel metin girişine ya da kök formlar, bilgi depolama ve alma için verimlidir. Dönüştürme gerçekleşir dizini yapılandırıldığında, dizin oluşturma sırasında ve daha sonra yeniden dizin okunduğunda arama sırasında. Her iki işlemleri için aynı metin Çözümleyicisi'ni kullanırsanız beklediğiniz arama sonuçları almak büyük olasılıkla.

Azure Search kullanan [standart Lucene Çözümleyicisi](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) varsayılan olarak. Alan alanını temelinde Varsayılanı geçersiz kılabilirsiniz. Bu makalede seçim aralığını ve özel analiz için en iyi yöntemler sunar. Ayrıca, temel senaryolar için örnek yapılandırmaları sağlar.

## <a name="supported-analyzers"></a>Desteklenen çözümleyiciler

Aşağıdaki liste, hangi çözümleyiciler Azure Search'te desteklenen açıklar.

| Kategori | Açıklama |
|----------|-------------|
| [Standart Lucene Çözümleyicisi](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Varsayılan. Hiçbir belirtimi veya yapılandırma gerekli değildir. Bu genel amaçlı Çözümleyicisi çoğu diller ve senaryolar için iyi gerçekleştirir.|
| Önceden tanımlanmış çözümleyiciler | Tamamlanmış bir ürün olarak kullanılmaya olarak sunulan-, ile sınırlı özelleştirme bulunur. <br/>İki tür vardır: özel ve dili. Bunları "önceden tanımlanmış" kılan bunları özelleştirme yok ile ada göre başvuru emin olur. <br/><br/>[Özelleştirilmiş (dil belirsiz) çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable) metin girişleri özel işleme veya en az işlem gerektiğinde kullanılır. Önceden tanımlanmış olmayan dil Çözümleyicileri dahil **Asciifolding**, **anahtar sözcüğü**, **düzeni**, **basit**, **Durdur**, **Boşluk**.<br/><br/>[Dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) zengin dil desteği için ayrı ayrı dilleri gerektiğinde kullanılır. Azure arama 35 Lucene dil Çözümleyicileri ve 50 Microsoft doğal dil işleme çözümleyiciler destekler. |
|[Özel çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Kullanıcı tanımlı bir yapılandırmayı varolan öğeleri, bir belirteç Oluşturucu (gerekli) ve isteğe bağlı filtreler (char veya belirteç) oluşan bir bileşimi.|

Önceden tanımlanmış bir Çözümleyicisi gibi özelleştirebilirsiniz **düzeni** veya **durdurmak**kısmında belgelenen diğer seçenekleri kullanmasını [önceden tanımlanmış Çözümleyicisi başvuru](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search#AnalyzerTable). Yalnızca önceden tanımlanmış çözümleyiciler bazılarını ayarlayabileceğiniz seçeneğiniz vardır. Hiçbir özelleştirme bir ada sahip yeni yapılandırmanızı gibi sağlayın *myPatternAnalyzer* Lucene düzeni Çözümleyicisi'nden ayırt etmek için.

## <a name="how-to-specify-analyzers"></a>Çözümleyiciler belirtme

1. (yalnızca özel Çözümleyicileri için) Oluşturma bir **Çözümleyicisi** dizin tanımı bölümünde. Daha fazla bilgi için bkz: [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) ve ayrıca [özel çözümleyiciler > oluşturma](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search#create-a-custom-analyzer).

2. Üzerinde bir [alan tanımı](https://docs.microsoft.com/rest/api/searchservice/create-index) dizinde ayarlamak **Çözümleyicisi** özelliğini bir hedef Çözümleyicisi adına (örneğin, `"analyzer" = "keyword"`. Geçerli değerler önceden tanımlanmış analyzer, dil Çözümleyicisi ya da aynı zamanda dizin şemasında tanımlanan özel Çözümleyicisi adını içerir.

3. İsteğe bağlı olarak, bir yerine **Çözümleyicisi** özelliği, dizin oluşturma ve kullanma sorgulama için farklı çözümleyiciler ayarlayabilirsiniz **indexAnalyzer** ve **searchAnalyzer'** alan parametreleri. 

3. Bir alan tanımı için bir çözümleyici ekleme dizini bir yazma işlemi oluşturur. Eklerseniz bir **Çözümleyicisi** varolan bir dizini için aşağıdaki adımları not edin:
 
 | Senaryo | Etki | Adımlar |
 |----------|--------|-------|
 | Yeni bir alan ekleyin | en az | Alan şemada henüz yoksa, alanın henüz fiziksel olarak bulunmayı dizininizdeki olmadığından yapmak için hiçbir alan düzeltmesi yoktur. Kullanım [güncelleştirme dizin](https://docs.microsoft.com/rest/api/searchservice/update-index) ve [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bu görev için.|
 | Bir Çözümleyicisi varolan bir dizini oluşturulmuş alana ekleyin. | Yeniden oluşturma | Bu alan için ters dizini sıfırdan yukarı yeniden oluşturulması gerekir ve bu alanları içeriğini reindexed gerekir. <br/> <br/>Etkin geliştirilme dizinler için [silmek](https://docs.microsoft.com/rest/api/searchservice/delete-index) ve [oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) yeni alan tanımını oluşturan çekme için dizini. <br/> <br/>Üretimde dizinler için yeniden düzenlenen bir tanımını sağlamak ve kullanmaya başlamak için yeni bir alan oluşturmanız gerekir. Kullanım [güncelleştirme dizin](https://docs.microsoft.com/rest/api/searchservice/update-index) ve [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) yeni alanı içerecek şekilde. Daha sonra planlanan dizin hizmeti, bir parçası olarak eski alanları kaldırmak için dizin oluşturan temizleyebilirsiniz. |

## <a name="tips-and-best-practices"></a>İpuçları ve en iyi yöntemler

Bu bölümde çözümleyicilerini ile çalışma konusunda öneriler sunar.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Okuma-yazma için bir çözümleyici belirli gereksinimleri yoksa

Azure arama sağlar, dizin oluşturma için farklı çözümleyiciler belirtin ve arama aracılığıyla ek `indexAnalyzer` ve `searchAnalyzer` alan parametreleri. Belirtilmezse, Çözümleyicisi kümesiyle `analyzer` özelliği, hem dizin oluşturma ve arama için kullanılır. Varsa `analyzer` olan belirtilmezse, varsayılan standart Lucene Çözümleyicisi kullanılır.

Genel bir kural belirli gereksinimleri sakınca yoksa aynı Çözümleyicisi dizin hem sorgulama, kullanmaktır. Sınamanız emin olun. Arama ve dizin oluşturma zamanında metin işleme farklı olduğu durumlarda, sorgu terimleri ve ne zaman arama ve dizin oluşturma Çözümleyicisi yapılandırmaları hizalanmadıysa dizinli koşulları arasında uyuşmazlık riskini çalıştırın.

### <a name="test-during-active-development"></a>Etkin geliştirme sırasında test

Standart Çözümleyicisi geçersiz kılma bir dizini yeniden oluşturma gerektirir. Mümkünse, bir dizin üretime çalışırken önce etkin geliştirme sırasında kullanmak için hangi çözümleyiciler karar verin.

### <a name="inspect-tokenized-terms"></a>Parçalanmış koşullarını inceleyin.

Beklenen sonuçları döndürmek bir arama başarısız olursa, en olası senaryo terim girişler için sorgu ve dizin içindeki parçalanmış koşulları arasında belirteci tutarsızlık ' dir. Belirteçleri aynı değilse, eşleşmeleri gerçekleştirmeye tutulamadı. Belirteç Oluşturucu çıkış incelemek için kullanmanızı öneririz [analiz API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) bir araştırma aracı olarak. Yanıt belirteçleri, belirli bir Çözümleyicisi tarafından oluşturulan gibi oluşur.

### <a name="compare-english-analyzers"></a>İngilizce çözümleyiciler Karşılaştır

[Arama Çözümleyicisi Demo](http://alice.unearth.ai/) standart Lucene analyzer, Lucene'nın İngilizce dil Çözümleyicisi ve Microsoft'un İngilizce doğal dil işlemci yan yana karşılaştırmasını gösteren bir üçüncü taraf demo uygulama. Dizin sabit; popüler Öykü metinden içerir. Her arama giriş sağlarsanız, her Çözümleyicisi sonuçlarından bitişik bölmelerinde görüntülenen her Çözümleyicisi aynı dize nasıl işlediği bir fikir verir. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler, birkaç önemli senaryoları için Çözümleyicisi tanımları gösterir.

<a name="Example1"></a>
### <a name="example-1-custom-options"></a>Örnek 1: Özel seçenekleri

Bu örnekte, özel seçenekleri Çözümleyicisi tanımıyla gösterilmektedir. Char filtreleri, tokenizers ve belirteç filtreleri için özel seçenekleri ayrı ayrı adlandırılmış yapıları belirtilen ve Çözümleyicisi tanımında başvurulan. Önceden tanımlanmış öğeleri olarak kullanılan-yalnızca ada göre başvurulan.

Bu örnek taramasını:

* Çözümleyicileri yapılabilen bir alan için alanı sınıfının bir özelliği var.
* Özel bir Çözümleyicisi dizin tanımı bir parçasıdır. Bu hafifçe (örneğin, bir filtre içinde tek bir seçeneği özelleştirme) özelleştirilmiş veya birden fazla yerde özelleştirilmiş.
* Bu durumda, özel Çözümleyicisi sırayla bir özelleştirilmiş standart belirteç Oluşturucu "my_standard_tokenizer" ve iki belirteci filtre kullanır "my_analyzer" olur: küçük harf ve özelleştirilmiş asciifolding filtre "my_asciifolding".
* Ayrıca, alt çizgi simgeleştirme (standart belirteç Oluşturucu sonlarını dash üzerinde ancak alt çizgi) önce tüm çizgiler değiştirmek için bir özel "map_dash" char filtre tanımlar.

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
              "map_dash"
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
        }
     ],
     "tokenizers":[
        {
           "name":"my_standard_tokenizer",
           "@odata.type":"#Microsoft.Azure.Search.StandardTokenizer",
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
### <a name="example-2-override-the-default-analyzer"></a>Örnek 2: varsayılan Çözümleyicisi geçersiz kıl

Standart Çözümleyicisi varsayılandır. Desen Çözümleyicisi gibi farklı bir önceden tanımlanmış Çözümleyicisi varsayılan yerine istediğinizi varsayalım. Özel seçeneklerini ayarlama emin değilseniz, yalnızca alan tanımındaki adı olarak belirtmeniz gerekir.

"Çözümleyicisi" öğesi, alan alanını temelinde standart Çözümleyicisi geçersiz kılar. Genel geçersiz kılma yok. Bu örnekte, `text1` düzeni Çözümleyicisi kullanır ve `text2`, bir Çözümleyicisi belirtmek değil, varsayılan kullanır.

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
### <a name="example-3-different-analyzers-for-indexing-and-search-operations"></a>Örnek 3: Farklı çözümleyiciler dizin oluşturma ve arama işlemleri için

API'ler dizin oluşturma ve arama için farklı çözümleyiciler belirtmek için ek dizin öznitelikleri içerir. `searchAnalyzer` Ve `indexAnalyzer` öznitelikleri tek değiştirerek bir çift belirtilmelidir `analyzer` özniteliği.


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

Diğer alanları varsayılan korumak (veya diğer bazı önceden tanımlanmış veya özel Çözümleyicisi'ni kullanın) farklı dillerde dizeleri içeren alanlar bir dil Çözümleyicisi kullanabilirsiniz. Bir dil Çözümleyicisi kullanırsanız, dizin oluşturma ve arama işlemleri için kullanılması gerekir. Bir dil Çözümleyicisi kullanan alanları dizin oluşturma için farklı çözümleyiciler sahip ve arayın.

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

+ Bizim kapsamlı açıklaması gözden [nasıl tam metin araması Azure Search'te çalışır](search-lucene-query-architecture.md). Bu makalede örnekler yüzeyine counter-intuitive görünebilir davranışları açıklamak için kullanır.

+ Ek sorgu sözdiziminde deneyin [Search belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) örnek bölümüne veya [Basit Sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) Portalı'nda arama Gezgini'nde.

+ Uygulamayı öğrenin [dile özgü sözcük çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Özel çözümleyiciler yapılandırma](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) en az işleme veya tek tek alanlarda özel işleme için.

+ [Standart ve İngilizce çözümleyiciler karşılaştırmak](http://alice.unearth.ai/) bu demo web sitesinde bitişik bölmelerinde. 

## <a name="see-also"></a>Ayrıca bkz.

 [REST API belgelerde arama](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

 [Tam Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 
 [Arama sonuçlarını işleme](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
