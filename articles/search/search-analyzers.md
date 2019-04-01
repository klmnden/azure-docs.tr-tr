---
title: Çözümleyici için dil ve metin işleme - Azure Search
description: Standart Lucene özel, önceden tanımlanmış veya dile özgü alternatif metin aranabilir alanları değiştirmek için bir dizin için Ata çözümleyicileri varsayılan.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: heidist
manager: cgronlun
author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: e3738980206277587ca367339d75da4f3faa643a
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651830"
---
# <a name="analyzers-for-text-processing-in-azure-search"></a>Metin işleme Azure Search'te çözümleyiciler

Bir *Çözümleyicisi* bir bileşeni olan [tam metin araması motoru](search-lucene-query-architecture.md) sorgu dizeleri ve dizinli belgelerde metin işleme için sorumlu. Farklı Çözümleyicileri metin senaryoya bağlı olarak farklı şekillerde düzenler. Dil Çözümleyicileri diğer Çözümleyicileri karakterleri küçük harflere örneğin dönüştürme gibi daha fazla temel görevleri gerçekleştirirken arama kalitesini iyileştirmek için dil kuralları kullanarak metin işlemenizi. 

Dil Çözümleyicileri en çok kullanıldığını ve bir Azure arama dizininde aranabilir her alana atanan varsayılan dil Çözümleyicisi yoktur. Aşağıdaki Dil Dönüşümleri metin analizi sırasında tipik şunlardır:

+ Gerekli olmayan sözcükler (stopword) ve noktalama işaretleri kaldırılır.
+ İfadeleri ve tire ile ayrılmış sözcüklerin bileşeni parçalara ayrılır.
+ Büyük küçük harfleri sözcüklerdir.
+ Böylece bir eşleşme şimdiki bağımsız olarak bulunabilir sözcükleri kök formları azaltılır.

Dil Çözümleyicileri dönüştürme basit bir metin giriş ya da kök formları bilgi depolanması ve alınması için verimli olan. Dönüştürme, dizini oluşturulurken, dizin oluşturma sırasında ortaya çıkar ve daha sonra yeniden dizin okunduğunda arama sırasında. Büyük olasılıkla hem işlemi için aynı Çözümleyicisi'ni kullanırsanız, beklediğiniz sonuçları edinin.

## <a name="default-analyzer"></a>Varsayılan Çözümleyicisi  

Azure Search kullanan [Apache Lucene standart Çözümleyicisi (standart lucene)](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) aşağıdaki öğelere metin sonu varsayılan olarak ["Unicode metin Segment"](https://unicode.org/reports/tr29/) kuralları. Ayrıca, standart Çözümleyicisi tüm karakterleri, küçük harf biçimine dönüştürür. Dizini oluşturulan belgeler hem arama terimlerini analiz dizin oluşturma ve sorgu işleme sırasında gidin.  

Ayrıca, her aranabilir alan üzerinde otomatik olarak kullanılır. Alan alanlı temelinde Varsayılanı geçersiz kılabilirsiniz. Alternatif Çözümleyicileri olabilir bir [dil Çözümleyicisi](index-add-language-analyzers.md), [özel çözümleyici](index-add-custom-analyzers.md), ya da önceden tanımlanmış bir Çözümleyicisi'nden [kullanılabilir Çözümleyicileri listesi](index-add-custom-analyzers.md#AnalyzerTable).


## <a name="types-of-analyzers"></a>Çözümleyici türü

Aşağıdaki liste, Azure Search'te çözümleyiciler hangi kullanılabilir açıklar.

| Kategori | Açıklama |
|----------|-------------|
| [Standart olarak Lucene çözümleyici](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) | Varsayılan. Belirtiminin ya da yapılandırma gereklidir. Bu genel amaçlı Çözümleyicisi çoğu diller ve senaryoları için iyi gerçekleştirir.|
| Önceden tanımlanmış çözümleyiciler | Tamamlanmış bir ürün olarak kullanılmaya yönelik olarak sunulan-olduğu. <br/>İki tür vardır: özelleştirilmiş ve dili. Bunları "önceden tanımlanmış" kılan, bunları bir yapılandırma veya özelleştirme ile adıyla başvurduğunu olduğu. <br/><br/>[Özelleştirilmiş (dilden) Çözümleyicileri](index-add-custom-analyzers.md#AnalyzerTable) metin girişleri özel işleme ya da en az işleme gerektirdiğinde kullanılır. Önceden tanımlı olmayan dil Çözümleyicileri dahil **Asciifolding**, **anahtar sözcüğü**, **deseni**, **basit**, **Durdur**, **Boşluk**.<br/><br/>[Dil Çözümleyicileri](index-add-language-analyzers.md) , zengin dil desteği için tek tek dillerin gerektiğinde kullanılır. Azure Search, Lucene dil çözümleyicilerini 35 ve 50 Microsoft doğal dil işleme Çözümleyicileri destekler. |
|[Özel çözümleyiciler](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search) | Bir kullanıcı tanımlı yapılandırmasını var olan öğeleri, bir belirteç Oluşturucu (gerekli) ve isteğe bağlı filtreler (char veya belirteç) oluşan bir bileşimi ifade eder.|

Birkaç Çözümleyici, gibi önceden tanımlanmış **deseni** veya **Durdur**, sınırlı sayıda yapılandırma seçeneği destekler. Bu seçenekleri ayarlamak için etkili bir şekilde özel bir çözümleyici önceden tanımlanmış Çözümleyicisi oluşan oluşturmanız ve diğer seçeneklerden birini belgelenen [önceden tanımlanmış çözümleyici başvurusu](index-add-custom-analyzers.md#AnalyzerTable). Herhangi bir özel yapılandırma ile yeni bir ad yapılandırmanızı gibi sağlamak *myPatternAnalyzer* Lucene deseni Çözümleyicisi'nden ayırmak için.

## <a name="how-to-specify-analyzers"></a>Çözümleyiciler belirtme

1. (yalnızca özel çözümleyiciler için) Adlandırılmış bir oluşturma **Çözümleyicisi** dizin bölümü. Daha fazla bilgi için [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) ve ayrıca [özel çözümleyiciler ekleme](index-add-custom-analyzers.md).

2. Üzerinde bir [alan tanımı](https://docs.microsoft.com/rest/api/searchservice/create-index) dizinde ayarlamak alanın **Çözümleyicisi** özelliğini hedef analyzer'ın adı (örneğin, `"analyzer" = "keyword"`. Geçerli değerler, önceden tanımlanmış Çözümleyicisi, dil Çözümleyicisi veya Ayrıca dizin şemasında tanımlanan özel çözümleyici adını içerir. Çözümleyici, dizin hizmetinde oluşturulmadan önce dizin tanımı aşamada atama planlayın.

3. İsteğe bağlı olarak, bir yerine **Çözümleyicisi** özelliği, dizin oluşturma ve kullanarak sorgulama için farklı Çözümleyicileri ayarlayabilirsiniz **indexAnalyzer** ve **searchAnalyzer** alan Parametreler. Bu etkinliklerden birini tarafından gerekmeyen belirli bir dönüştürme gerekliyse, farklı Çözümleyicileri veri hazırlığı ve alımı için kullanırsınız.

Atama **Çözümleyicisi** veya **indexAnalyzer** fiziksel olarak önceden oluşturulmuş bir alan için izin verilmiyor. Bu olduğunda belirsiz, Eylemler bir yeniden oluşturma gerektiren bir çözümleme için aşağıdaki tabloya gözden geçirin ve neden.
 
 | Senaryo | Etki | Adımlar |
 |----------|--------|-------|
 | Yeni alan ekleme | En az | Alan şemada henüz yoksa, alanın henüz fiziksel olarak bulunmayı dizininizdeki olmadığından yapmak için hiçbir alan düzeltme yoktur. Kullanabileceğiniz [dizin güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) mevcut bir dizine yeni bir alan eklemek ve [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bunu doldurmak üzere.|
 | Ekleme bir **Çözümleyicisi** veya **indexAnalyzer** varolan dizinli alana. | [Yeniden oluşturma](search-howto-reindex.md) | Bu alan için ters dizini sıfırdan oluşturulması gerekir ve bu alanların içeriğini reindexed gerekir. <br/> <br/>Etkin geliştirme aşamasındaki dizinler için [Sil](https://docs.microsoft.com/rest/api/searchservice/delete-index) ve [oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) dizinin yeni alanın tanımını seçin. <br/> <br/>Üretimde dizinler için düzeltilmiş tanımı sağlamak ve eskisinin yerine kullanmaya başlamak için yeni bir alan oluşturarak yeniden yayımlanmalarından sonra. Kullanım [dizin güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) yeni alan eklemek ve [mergeOrUpload](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) bunu doldurmak üzere. Daha sonra planlanan bir dizin hizmeti, bir parçası olarak kullanılmayan alanları kaldırmak için dizin temizleyebilirsiniz. |

## <a name="when-to-add-analyzers"></a>Çözümleyiciler ekleme zamanı

En iyi zamanı eklemek ve çözümleyiciler atamak için active geliştirme sırasında bırakarak zaman ve dizinleri yeniden yordamı.

Bir dizin tanımını solidifies yeni analiz yapıları için bir dizin ekleyebilir, ancak iletmeniz gerekir **allowIndexDowntime** bayrak [dizin güncelleştirme](https://docs.microsoft.com/rest/api/searchservice/update-index) bu hatadan kaçınmak istiyorsanız:

*"Dizin güncelleştirme kapalı kalma süresi neden olacağından izin verilmiyor. Mevcut bir dizine yeni Çözümleyicileri, oluşturma denenmeden, belirteç filtreleri veya karakter filtre eklemek için dizin güncelleştirme isteğinde 'allowIndexDowntime' sorgu parametresi 'true' olarak ayarlayın. Bu işlem, dizini çevrimdışı, dizin oluşturma neden en az birkaç saniye ve başarısız sorgu istekleri için sokar unutmayın. Dizin performans ve yazma kullanılabilirliğini engelliler için dizin güncelleştirildikten sonra birkaç dakika veya daha çok büyük dizinler için uzun olabilir."*

Aynı alana bir çözümleyici atamasını yaparken geçerlidir. Bir çözümleyici alanın tanımını bir parçası olduğundan alanı oluştururken, yalnızca ekleyebilirsiniz. Varolan alanlara Çözümleyicileri eklemek istiyorsanız, gerekecektir [bırakın ve yeniden](search-howto-reindex.md) dizini veya istediğiniz çözümleyiciyi ile yeni bir alan ekleyin.

Belirtildiği gibi bir özel durumdur **searchAnalyzer** değişken. Çözümleyiciler belirtmek için üç yolunuz (**Çözümleyicisi**, **indexAnalyzer**, **searchAnalyzer**), yalnızca **searchAnalyzer** özniteliği Varolan bir alanı değiştirilebilir.

## <a name="recommendations-for-working-with-analyzers"></a>Çözümleyicileriyle çalışmaya yönelik öneriler

Bu bölümde, çözümleyiciler ile çalışma konusunda öneriler sunar.

### <a name="one-analyzer-for-read-write-unless-you-have-specific-requirements"></a>Okuma-yazma için bir çözümleyici belirli gereksinimleriniz yoksa

Azure arama sayesinde dizinini oluşturmak için farklı Çözümleyicileri belirtin ve arama aracılığıyla ek **indexAnalyzer** ve **searchAnalyzer** alan parametreleri. Belirtilmemişse, çözümleyici kümesi **Çözümleyicisi** özelliği, hem dizinleme ve arama için kullanılır. Varsa `analyzer` olan belirtilmemişse, varsayılan olarak standart Lucene çözümleyici kullanılır.

Genel bir kural belirli gereksinimleri dair sakınca yoksa aynı Çözümleyicisi hem dizin oluşturma ve sorgulama, için kullanmaktır. Baştan sona test etmeyi unutmayın. Arama ve dizin oluşturma zamanında metin işleme farklılık gösterdiğinde sorgu terimleri ve arama ve dizin oluşturma Çözümleyicisi yapılandırmaları zaman değil hizalanır dizini oluşturulan terimler arasında uyumsuzluk riskini çalıştırın.

### <a name="test-during-active-development"></a>Etkin geliştirme sırasında test etme

Standart Çözümleyicisi geçersiz kılan bir dizini yeniden derleme gerektirir. Mümkünse, üretim ortamına dizin çalışırken önce etkin geliştirme sırasında kullanmak için hangi Çözümleyicileri karar verin.

### <a name="inspect-tokenized-terms"></a>Parçalanmış koşullarını inceleyin

Beklenen sonuçları döndürmek bir arama başarısız olursa, büyük olasılıkla terim giriş sorguda ve dizindeki parçalanmış hüküm arasında belirteç tutarsızlıklar senaryodur. Belirteçler aynı olmayan iyileştirilene eşleşme başarısız. Simgeleştirici çıkış incelemek için kullanmanızı öneririz [analiz API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) araştırma aracı olarak. Yanıt belirteçleri, belirli bir çözümleyici tarafından oluşturulan oluşur.

### <a name="compare-english-analyzers"></a>İngilizce Çözümleyicileri karşılaştırın

[Arama Çözümleyicisi Tanıtımı](https://alice.unearth.ai/) standart olarak Lucene Çözümleyici, Lucene'nın İngilizce dil Çözümleyicisi ve Microsoft'un doğal dil İngilizce işlemci yan yana karşılaştırmasını gösteren bir üçüncü taraf tanıtım uygulaması. Dizin sabittir; Bu yaygın bir hikaye metni içerir. İçin her bir arama giriş sağlarsanız, her çözümleyici sonuçları bitişik bölmelerinde görüntülenen her Çözümleyicisi aynı dize nasıl işlediği hakkında bir fikir verir. 

<a name="examples"></a>

## <a name="rest-examples"></a>KALAN örnekler

Aşağıdaki örnekler birkaç önemli senaryolar için Çözümleyicisi tanımları gösterir.

+ [Özel bir çözümleyici örneği](#Custom-analyzer-example)
+ [Bir alan örneğe Çözümleyicileri atayın](#Per-field-analyzer-assignment-example)
+ [Dizin oluşturma ve arama için Çözümleyicileri karıştırma](#Mixing-analyzers-for-indexing-and-search-operations)
+ [Dil Çözümleyicisi örneği](#Language-analyzer-example)

<a name="Custom-analyzer-example"></a>

### <a name="custom-analyzer-example"></a>Özel bir çözümleyici örneği

Bu örnekte, özel seçeneklerle bir çözümleyici tanımı gösterilmektedir. Özel seçenekleri char filtreleri ve oluşturma denenmeden belirteci filtreleri için ayrı ayrı adlandırılmış yapıları belirtilen ve ardından Çözümleyicisi tanımında başvurulan. Önceden tanımlanmış öğeleri olarak kullanılan-olduğunu ve yalnızca adı tarafından başvurulan.

Bu örnekte yürüyen:

* Çözümleyiciler aranabilir bir alanı için alan sınıfının bir özelliği var.
* Özel bir çözümleyici dizin tanımı'nın bir parçasıdır. Bunu hafifçe (örneğin, bir filtre içinde tek bir seçenek özelleştirme) özelleştirilebilen veya birden fazla yerde özelleştirilebilir.
* Bu durumda, özel çözümleyici sırayla bir özelleştirilmiş standart simgeleştirici "my_standard_tokenizer" ve belirteç iki filtre kullanır "my_analyzer" olduğu: küçük harf ve özelleştirilmiş asciifolding filtre "my_asciifolding".
* Ayrıca, 2 özel char Filtreler "map_dash" ve "remove_whitespace" tanımlar. İkincisi tüm boşlukları kaldırır ancak ilk tüm tire alt çizgi ile değiştirir. Alanları eşleme kuralları kodlanmış UTF-8 gerekir. Char filtreleri önce simgeleştirme uygulanır ve sonuçta elde edilen belirteçleri (standart simgeleştirici sonlarını tire ve boşluk ancak alt çizgi) etkiler.

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

<a name="Per-field-analyzer-assignment-example"></a>

### <a name="per-field-analyzer-assignment-example"></a>Alan başına Çözümleyicisi atama örneği

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

<a name="Mixing-analyzers-for-indexing-and-search-operations"></a>

### <a name="mixing-analyzers-for-indexing-and-search-operations"></a>Dizin oluşturma ve arama işlemleri için Çözümleyicileri karıştırma

API'ler farklı Çözümleyicileri için dizin oluşturma ve arama belirtmek için ek dizin özniteliklerini içerir. **SearchAnalyzer** ve **indexAnalyzer** öznitelikleri değiştirerek tek bir çift olarak belirtilmelidir **Çözümleyicisi** özniteliği.


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

<a name="Language-analyzer-example"></a>

### <a name="language-analyzer-example"></a>Dil Çözümleyicisi örneği

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

## <a name="c-examples"></a>C#örnekleri

.NET SDK kod örneklerini kullanıyorsanız kullanın veya Çözümleyicileri yapılandırmak için bu örnekleri ekleyebilir.

+ [Yerleşik Çözümleyicisi Ata](#Assign-a-language-analyzer)
+ [Bir çözümleyici yapılandırın](#Define-a-custom-analyzer)

<a name="Assign-a-language-analyzer"></a>

### <a name="assign-a-language-analyzer"></a>Bir dil Çözümleyicisi Ata

Olarak kullanılan tüm Çözümleyicisi-, yapılandırma gerektirmeden ise, bir alan tanımında belirtilir. Bir çözümleyici yapısı oluşturmak için bir gereksinim değildir. 

Bu örnek Microsoft English ve Fransızca Çözümleyicileri açıklama alanlarına atar. Daha büyük bir hotels.cs dosyasında otel sınıfını kullanarak oluşturmak Oteller dizinini tanımını alındığı bir kod parçacığı olan [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) örnek.

Çağrı [Çözümleyicisi](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzer?view=azure-dotnet), belirten [AnalyzerName](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet) Azure arama'yı desteklenen bir metin Çözümleyicisi sağlayan tür.

```csharp
    public partial class Hotel
    {
       . . . 

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
        [JsonProperty("description")]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrLucene)]
        [JsonProperty("description_fr")]
        public string DescriptionFr { get; set; }

      . . .
    }
```
<a name="Define-a-custom-analyzer"></a>

### <a name="define-a-custom-analyzer"></a>Özel bir çözümleyici tanımlayın

Özelleştirme veya yapılandırma gerekli olduğunda bir çözümleyici yapısı için bir dizin eklemek gerekir. Tanımladığınız sonra önceki örnekte gösterildiği gibi bu alan tanımı ekleyebilirsiniz.

Oluşturma bir [CustomAnalyzer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.customanalyzer?view=azure-dotnet) nesne. Daha fazla örnek için bkz. [CustomAnalyzerTests.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/src/SDKs/Search/DataPlane/Search.Tests/Tests/CustomAnalyzerTests.cs).

```csharp
{
   var definition = new Index()
   {
         Name = "hotels",
         Fields = FieldBuilder.BuildForType<Hotel>(),
         Analyzers = new[]
            {
               new CustomAnalyzer()
               {
                     Name = "url-analyze",
                     Tokenizer = TokenizerName.UaxUrlEmail,
                     TokenFilters = new[] { TokenFilterName.Lowercase }
               }
            },
   };

   serviceClient.Indexes.Create(definition);
```

## <a name="next-steps"></a>Sonraki adımlar

+ Sunduğumuz kapsamlı açıklaması gözden [nasıl tam metin araması Azure Search'te çalışır](search-lucene-query-architecture.md). Bu makalede örnekler yüzeyine counter-intuitive görünebilir davranışları açıklamak için kullanılır.

+ Ek sorgu söz dizimi gelen deneyin [arama belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) örnek bölümünde veya [Basit Sorgu söz dizimi](query-simple-syntax.md) portalında arama Gezgini.

+ Nasıl uygulayabileceğinizi öğrenin [dile özel sözcük temelli çözümleyiciler](index-add-language-analyzers.md).

+ [Özel çözümleyiciler yapılandırma](index-add-custom-analyzers.md) için en az işleme ya da tek tek alanlarda özel işleme.

+ [Standart ve İngilizce Çözümleyicileri karşılaştırma](https://alice.unearth.ai/) bitişik bölmelerindeki bu demo web sitesinde. 

## <a name="see-also"></a>Ayrıca bkz.

 [Search belgeleri REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

 [Basit sorgu söz dizimi](query-simple-syntax.md) 

 [Tam Lucene sorgu söz dizimi](query-lucene-syntax.md) 
 
 [Arama sonuçlarını işleme](search-pagination-page-layout.md)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
