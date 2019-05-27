---
title: Özel çözümleyiciler - Azure Search Ekle
description: Metin oluşturma denenmeden ve Azure arama tam metin arama sorgularında kullanılan karakter filtreleri değiştirin.
ms.date: 02/14/2019
services: search
ms.service: search
ms.topic: conceptual
author: Yahnoosh
ms.author: jlembicz
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: e9daebf46093e38858feff87ca5c4ba89638aa74
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65951896"
---
# <a name="add-custom-analyzers-to-an-azure-search-index"></a>Azure Search dizini için özel çözümleyiciler ekleme

A *özel çözümleyici* belirli bir tür [metin Çözümleyicisi](search-analyzers.md) bir kullanıcı tanımlı bir birleşimini mevcut simgeleştirici ve isteğe bağlı filtreler oluşur. Yeni yollarla oluşturma denenmeden ve filtreleri birleştirerek metin arama motoruna belirli sonuçlar elde etmek için işleme özelleştirebilirsiniz. Örneğin, ile özel bir çözümleyici oluşturabilirsiniz bir *char filtre* metin girişi simgeleştirilmiş önce HTML biçimlendirme kaldırmak için.

 Filtreler birleşimi değiştirmek için birden çok özel çözümleyiciler tanımlayabilirsiniz, ancak her alanı yalnızca bir Çözümleyicisi çözümleme ve arama çözümleme için dizin oluşturma için kullanabilirsiniz. Bir müşteri Çözümleyicisi nasıl göründüğüne ilişkin bir çizim için bkz [özel çözümleyici örneği](search-analyzers.md#Custom-analyzer-example).

## <a name="overview"></a>Genel Bakış

 Rolü bir [tam metin arama motoru](search-lucene-query-architecture.md), basit bir deyişle, belgeleri, etkili sorgulamayı ve alma sağlayan bir şekilde depolamak ve işlemek için olan. Yüksek bir düzeyde tüm belgeleri önemli sözcüklerin ayıklanması, bunları bir dizinde yerleştirme ve ardından belirli bir sorgu, sözcükleri eşleştirmeyi belgeleri bulmak için dizin kullanarak gelir. Belgeler ve arama sorgularından sözcükleri ayıklama işleminin adlı *sözcük analizi*. Sözcük temelli analiz bileşenleri çağrılır *Çözümleyicileri*.

 Azure Search'te bir dizi önceden tanımlanmış dilden Çözümleyicileri aralarından seçim yapabileceğiniz [Çözümleyicileri](#AnalyzerTable) tablo veya dile özel çözümleyiciler listelenen [dil Çözümleyicileri &#40;Azure arama hizmeti REST API'si&#41;](index-add-language-analyzers.md). Ayrıca kendi özel çözümleyiciler tanımlamak için bir seçeneğiniz de vardır.  

 Özel bir çözümleyici dizinlenebilir ve aranabilir belirteçlere metin dönüştürme işlemi denetime olanak sağlar. Önceden tanımlanmış tek bir belirteç Oluşturucu, bir veya daha fazla belirteç filtreleri ve bir veya daha fazla karakter filtre oluşan kullanıcı tanımlı bir yapılandırma var. Belirteç Oluşturucu simgeleri ve simgeleştirici tarafından yayınlanan belirteçleri değiştirmek için belirteç filtreler için metin parçalamak sorumludur. Char filtreleri simgeleştirici tarafından işlenmeden önce giriş metni hazırlamak için geçerlidir. Örneğin, char filtre belirli karakterler ve semboller değiştirebilirsiniz.

 Özel çözümleyiciler tarafından etkinleştirilen yaygın senaryolar şunlardır:  

- Ses arama. Nasıl bir word sesleri üzerinde bağlı olarak, değil nasıl bunu yazıldığından aramayı etkinleştirmek için fonetik bir filtre ekleyin.  

- Sözcük temelli analiz devre dışı bırakın. Anahtar sözcüğü Çözümleyicisi değil analiz arama yapılabilir alanlar oluşturmak için kullanın.  

- Hızlı önek/sonek arama. Edge N-gram belirteci filtresi için dizin önekleri hızlı ön eki eşleşen etkinleştirmek için sözcük ekleyin. Bu, eşleşen soneki için geriye doğru belirteci filtre ile birleştirin.  

- Özel simgeleştirme. Örneğin, cümle belirteçleri boşluk ayırıcı olarak kullanarak bölüneceği boşluk simgeleştirici kullanın  

- ASCII Katlama. Standart filtre Katlama Aksanları ö veya ê gibi arama terimlerini Normalleştirilecek ASCII ekleyin.  

  Bu sayfa, desteklenen Çözümleyicileri, oluşturma denenmeden, belirteç filtreleri ve char filtreleri listesini sağlar. Dizin tanımını bir kullanım örneği ile yapılan değişikliklerin bir açıklaması da bulabilirsiniz. Azure Search uygulamasında kullanmalarını sağlayan temel teknoloji hakkında daha fazla bilgiler için bkz. [analiz paketi özeti (Lucene)](https://lucene.apache.org/core/4_10_0/core/org/apache/lucene/codecs/lucene410/package-summary.html). Çözümleyici yapılandırmaları örnekler için bkz [Azure Search'te çözümleyiciler ekleme](search-analyzers.md#examples).

## <a name="validation-rules"></a>Doğrulama kuralları  
 Çözümleyiciler, oluşturma denenmeden, belirteç filtreleri ve char filtreleri adları benzersiz olmalıdır ve önceden tanımlanmış Çözümleyicileri, oluşturma denenmeden, belirteç filtreleri veya char filtreleri herhangi biri ile aynı olamaz. Bkz: [Özellik Başvurusu](#PropertyReference) adları zaten kullanılıyor.

## <a name="create-custom-analyzers"></a>Özel çözümleyiciler oluşturma
 Özel çözümleyiciler, dizin oluşturma sırasında tanımlayabilirsiniz. Özel bir çözümleyici belirtmek için sözdizimi, bu bölümde açıklanmıştır. Ayrıca sözdizimi ile örnek tanımlarını gözden geçirerek planladığınızdan [Azure Search'te çözümleyiciler ekleme](search-analyzers.md#examples).  

 Bir ad, bir tür, bir veya daha fazla karakter filtreleri, en fazla bir belirteç oluşturucu ve sonrası simgeleştirme işleme için bir veya daha fazla belirteç filtreleri Çözümleyicisi tanımı içerir. Char filtrelerin simgeleştirme önce uygulanır. Belirteç filtre ve karakter filtreleri soldan sağa uygulanır.

 `tokenizer_name` Bir belirteç oluşturucu adı `token_filter_name_1` ve `token_filter_name_2` belirteci filtreleri adlarının ve `char_filter_name_1` ve `char_filter_name_2` char filtreleri adlarıdır (bkz [oluşturma denenmeden](#Tokenizers), [ Filtreler Token](#TokenFilters) ve Char filtreler için geçerli değerler tabloları).

Çözümleyici tanımı, daha büyük bir dizin parçasıdır. Bkz: [dizin API oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) dizinin rest hakkında bilgi için.

```
"analyzers":(optional)[
   {
      "name":"name of analyzer",
      "@odata.type":"#Microsoft.Azure.Search.CustomAnalyzer",
      "charFilters":[
         "char_filter_name_1",
         "char_filter_name_2"
      ],
      "tokenizer":"tokenizer_name",
      "tokenFilters":[
         "token_filter_name_1",
         "token_filter_name_2"
      ]
   },
   {
      "name":"name of analyzer",
      "@odata.type":"#analyzer_type",
      "option1":value1,
      "option2":value2,
      ...
   }
],
"charFilters":(optional)[
   {
      "name":"char_filter_name",
      "@odata.type":"#char_filter_type",
      "option1":value1,
      "option2":value2,
      ...
   }
],
"tokenizers":(optional)[
   {
      "name":"tokenizer_name",
      "@odata.type":"#tokenizer_type",
      "option1":value1,
      "option2":value2,
      ...
   }
],
"tokenFilters":(optional)[
   {
      "name":"token_filter_name",
      "@odata.type":"#token_filter_type",
      "option1":value1,
      "option2":value2,
      ...
   }
]
```

> [!NOTE]  
>  Azure portalında oluşturduğunuz özel çözümleyiciler açık değildir. Dizin tanımlarken API çağrıları yapan kod özel bir çözümleyici eklemek için tek yoludur.  

 İçinde bir dizin tanımını, bu bölümde oluşturma dizini istek gövdesinde herhangi bir yere yerleştirebilirsiniz, ancak genellikle bu sonuna kadar:  

```
{
  "name": "name_of_index",
  "fields": [ ],
  "suggesters": [ ],
  "scoringProfiles": [ ],
  "defaultScoringProfile": (optional) "...",
  "corsOptions": (optional) { },
  "analyzers":(optional)[ ],
  "charFilters":(optional)[ ],
  "tokenizers":(optional)[ ],
  "tokenFilters":(optional)[ ]
}
```

Yalnızca özel seçenekleri ayarlıyorsanız, char filtreleri ve oluşturma denenmeden belirteci filtreleri için tanımları dizine eklenir. Bir var olan bir filtre veya belirteç Oluşturucu olarak kullanılacak-olduğundan, çözümleyici tanımı adı belirtin.

<a name="Testing custom analyzers"></a>

## <a name="test-custom-analyzers"></a>Test özel çözümleyiciler

Kullanabileceğiniz **Testi Çözümleyicisi işlemi** içinde [REST API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) nasıl bir çözümleyici metin belirteçlere böler görmek için.

**İstek**
```
  POST https://[search service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
  Content-Type: application/json
    api-key: [admin key]

  {
     "analyzer":"my_analyzer",
     "text": "Vis-à-vis means Opposite"
  }
```
**Yanıt**
```
  {
    "tokens": [
      {
        "token": "vis_a_vis",
        "startOffset": 0,
        "endOffset": 9,
        "position": 0
      },
      {
        "token": "vis_à_vis",
        "startOffset": 0,
        "endOffset": 9,
        "position": 0
      },
      {
        "token": "means",
        "startOffset": 10,
        "endOffset": 15,
        "position": 1
      },
      {
        "token": "opposite",
        "startOffset": 16,
        "endOffset": 24,
        "position": 2
      }
    ]
  }
```

## <a name="update-custom-analyzers"></a>Güncelleştirme özel çözümleyiciler

Bir çözümleyici, bir belirteç Oluşturucu, bir belirteç filtre veya char filtre tanımlandıktan sonra değiştirilemez. Yeni bir tane varsa, yalnızca mevcut bir dizine eklenebilir `allowIndexDowntime` bayrağı ayarlandığında dizin güncelleştirme isteğinde true:

```
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true
```

Bu işlem, dizin oluşturma ve sorgu isteklerin başarısız olmasına neden olan en az birkaç saniye boyunca dizininiz çevrimdışı alır. Birkaç dakika sonra güncelleştirilmiş ya da çok büyük dizinler için uzun dizinidir, ancak bu etkileri geçicidir ve sonuçta kendi çözmek için dizin performans ve yazma kullanılabilirliğini engelli olabilir.

 <a name="ReferenceIndexAttributes"></a>

## <a name="analyzer-reference"></a>Çözümleyici başvurusu

Aşağıdaki tablolarda Çözümleyicileri, oluşturma denenmeden belirteci filtreleri yapılandırma özellikleri listesi ve bir dizin tanımını filtre bölümüne karakter. Bu öznitelikler bir çözümleyici, belirteç oluşturucu veya filtre dizininizdeki yapısını oluşur. Değer atama için bilgi [Özellik Başvurusu](#PropertyReference).

### <a name="analyzers"></a>Çözümleyiciler

Çözümleyici için dizin özniteliklerini mi bağlı olarak farklılık gösterir, önceden tanımlanmış ya da özel çözümleyiciler kullanıyorsanız.

#### <a name="predefined-analyzers"></a>Önceden tanımlanmış çözümleyiciler

|||  
|-|-|  
|Ad|Yalnızca harf, rakam, boşluk, kısa çizgi veya alt çizgi, can yalnızca başlangıç ve bitiş alfasayısal karakterler ile içermelidir ve 128 karakterle sınırlıdır.|  
|Tür|Çözümleyici türü listesinden Çözümleyicileri desteklenmiyor. Bkz: **analyzer_type** sütununda [Çözümleyicileri](#AnalyzerTable) aşağıdaki tabloda.|  
|Seçenekler|Listelenen önceden tanımlanmış bir çözümleyici seçenekleri geçerli olmalıdır [Çözümleyicileri](#AnalyzerTable) aşağıdaki tabloda.|  

#### <a name="custom-analyzers"></a>Özel çözümleyiciler

|||  
|-|-|  
|Ad|Yalnızca harf, rakam, boşluk, kısa çizgi veya alt çizgi, can yalnızca başlangıç ve bitiş alfasayısal karakterler ile içermelidir ve 128 karakterle sınırlıdır.|  
|Tür|"#Microsoft.Azure.Search.CustomAnalyzer" olmalıdır.|  
|CharFilters|Listelenen önceden tanımlanmış karakter filtreleri birini ayarlayın [Char filtreleri](#char-filters-reference) tablo veya dizin tanımında belirtilen bir özel karakter filtre.|  
|Belirteç Oluşturucu|Gereklidir. Listelenen önceden tanımlanmış oluşturma denenmeden birini ayarlayın [oluşturma denenmeden](#Tokenizers) aşağıdaki tablo veya dizin tanımında belirtilen özel bir belirteç Oluşturucu.|  
|TokenFilters|Listelenen önceden tanımlanmış simge filtreleri birini ayarlayın [filtreler Token](#TokenFilters) tablo veya dizin tanımında belirtilen özel bir belirteç Filtresi.|  

<a name="CharFilter"></a>

### <a name="char-filters"></a>Char filtreleri

 Bir char filtre simgeleştirici tarafından işlenmeden önce giriş metni hazırlamak için kullanılır. Örneğin, bunlar belirli karakterler ve semboller değiştirebilirsiniz. Özel bir Çözümleyicisi'nde birden çok karakter filtre olabilir. Char filtreleri içinde listelendikleri sırayla çalıştırın.  

|||  
|-|-|  
|Ad|Yalnızca harf, rakam, boşluk, kısa çizgi veya alt çizgi, can yalnızca başlangıç ve bitiş alfasayısal karakterler ile içermelidir ve 128 karakterle sınırlıdır.|  
|Tür|Char desteklenen char filtreler listeden türünü filtreleyin. Bkz: **char_filter_type** sütununda [Char filtreleri](#char-filters-reference) aşağıdaki tabloda.|  
|Seçenekler|Geçerli seçenekleri olmalıdır bir verilen [Char filtreleri](#char-filters-reference) türü.|  

### <a name="tokenizers"></a>Oluşturma denenmeden

 Bir belirteç Oluşturucu bir dizi bir cümle sözcüklere bölme gibi belirteçleri, sürekli bir metin böler.  

 Özel çözümleyici başına tam olarak bir belirteç Oluşturucu belirtebilirsiniz. Birden fazla belirteç Oluşturucu ihtiyacınız varsa birden çok özel çözümleyiciler oluşturup dizin Şemanızda bir alan olarak alanı temelinde atamanız.  
Özel bir çözümleyici önceden tanımlanmış bir belirteç Oluşturucu varsayılan veya özelleştirilmiş seçenekleri ile kullanabilirsiniz.  

|||  
|-|-|  
|Ad|Yalnızca harf, rakam, boşluk, kısa çizgi veya alt çizgi, can yalnızca başlangıç ve bitiş alfasayısal karakterler ile içermelidir ve 128 karakterle sınırlıdır.|  
|Tür|Desteklenen oluşturma denenmeden listesinden belirteç oluşturucu adı. Bkz: **tokenizer_type** sütununda [oluşturma denenmeden](#Tokenizers) aşağıdaki tabloda.|  
|Seçenekler|Listelenen belirli simgeleştirici türü geçerli seçenekler olmalıdır [oluşturma denenmeden](#Tokenizers) aşağıdaki tabloda.|  

### <a name="token-filters"></a>Belirteç filtreleri

 Bir belirteç filtre filtrelemek veya simgeleştirici tarafından oluşturulan belirteçleri değiştirmek için kullanılır. Örneğin, tüm karakterleri küçük harfe dönüştürür, küçük bir filtre belirtebilirsiniz.   
Özel bir Çözümleyicisi'nde birden fazla belirteç filtreye sahip olabilir. Belirteç filtreleri içinde listelendikleri sırayla çalıştırın.  

|||  
|-|-|  
|Ad|Yalnızca harf, rakam, boşluk, kısa çizgi veya alt çizgi, can yalnızca başlangıç ve bitiş alfasayısal karakterler ile içermelidir ve 128 karakterle sınırlıdır.|  
|Tür|Desteklenen belirteç filtreleri listesinden belirteci filtre adı. Bkz: **token_filter_type** sütununda [filtreler Token](#TokenFilters) aşağıdaki tabloda.|  
|Seçenekler|Olmalıdır [filtreler Token](#TokenFilters) verilen belirteç filtre türü.|  

<a name="PropertyReference"></a>  

## <a name="property-reference"></a>Özellik Başvurusu

Bu bölüm, özel Çözümleyici, tokenizer, char filtre veya belirteç filtre dizininizdeki tanımında belirtilen öznitelikler için geçerli değerler sağlar. Çözümleyiciler, oluşturma denenmeden ve Apache Lucene kullanılarak uygulanan filtreleri Lucene API belgelerine bağlantılar var.

<a name="AnalyzerTable"></a>

###  <a name="predefined-analyzers-reference"></a>Önceden tanımlanmış çözümleyici başvurusu

|**analyzer_name**|**analyzer_type**<sup>1  </sup>|**Açıklamayı ve seçenekleri**|  
|-|-|-|  
|[Anahtar sözcüğü](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/KeywordAnalyzer.html)| (türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir) |Bir alanın tüm içeriği tek bir belirteç değerlendirir. Bu, posta kodları, kimlikleri ve bazı ürün adları gibi veriler için kullanışlıdır.|  
|[Düzeni](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/PatternAnalyzer.html)|PatternAnalyzer|Esnek bir şekilde metin koşulları bir normal ifade deseni ile halinde ayırır.<br /><br /> **Seçenekler**<br /><br /> küçük harf (tür: bool)-koşulları küçük harf yapılmış olup olmadığını belirler. Varsayılan değer True'dur.<br /><br /> [deseni](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html?is-external=true) (tür: string)-belirteç ayırıcı eşleştirilecek normal ifade deseni. \W+ varsayılandır.<br /><br /> [bayrakları](https://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html#field_summary) (tür: string)-normal ifade bayrakları. Varsayılan değer boş bir dizedir. İzin verilen değerler: CANON_EQ CASE_INSENSITIVE, YORUMLAR, DOTALL, LITERAL, ÇOK SATIRLI, UNICODE_CASE, UNIX_LINES<br /><br /> stopword (tür: dize dizisi)-stopword listesi. Varsayılan değer boş bir listedir.|  
|[Basit](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/SimpleAnalyzer.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir) |Harf olmayan metin böler ve bunları küçük harflere dönüştürür. |  
|[Standart](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardAnalyzer.html) <br />(Standard.lucene da bilinir)|StandardAnalyzer|Standart olarak Lucene Çözümleyici, standart tokenizer, küçük harf filtre ve durdurma filtre oluşur.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (tür: int)-en fazla belirteç uzunluğu. Varsayılan 255'tir. Belirteçleri uzunluk üst sınırından daha uzun bölünür. Kullanılabilen en yüksek belirteç 300 karakter uzunluğundadır.<br /><br /> stopword (tür: dize dizisi)-stopword listesi. Varsayılan değer boş bir listedir.|  
|standardasciifolding.lucene|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir) |Standart ASCII Katlama filtre Analyzer'da. |  
|[Durdur](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/StopAnalyzer.html)|StopAnalyzer|Harf olmayan metin ayıran, küçük ve durma sözcüğü belirteci filtreler uygular.<br /><br /> **Seçenekler**<br /><br /> stopword (tür: dize dizisi)-stopword listesi. Önceden tanımlı bir listeye İngilizce için varsayılandır. |  
|[Boşluk](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/WhitespaceAnalyzer.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir) |Boşluk belirteç Oluşturucu kullanan bir çözümleyici. 255 karakterden uzun belirteçleri bölünür.|  

 <sup>1</sup> Çözümleyicisi türleri her zaman ön eki bulunur "#Microsoft.Azure.Search" ile koddaki "PatternAnalyzer" aslında "#Microsoft.Azure.Search.PatternAnalyzer" belirtilecek şekilde. Uzatmamak için önek kaldırdık, ancak önek kodunuzda gereklidir. 
 
Analyzer_type yalnızca özelleştirilebilir Çözümleyicileri için sağlanır. Anahtar sözcüğü Çözümleyicisi ile olduğu gibi hiçbir seçenek varsa ilişkili #Microsoft.Azure.Search türü yoktur.


<a name="CharFilter"></a>

###  <a name="char-filters-reference"></a>Başvuru char filtreleri

Aşağıdaki tabloda, Apache Lucene uygulanan karakter filtreleri Lucene API belgeleriyle bağlanır.

|**char_filter_name**|**char_filter_type** <sup>1</sup>|**Açıklamayı ve seçenekleri**|  
|-|-|-|
|[html_strip](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/charfilter/HTMLStripCharFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |HTML Şerit çalışır bir char filtre oluşturur.|  
|[Eşleme](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/charfilter/MappingCharFilter.html)|MappingCharFilter|Char filtre eşlemeleri seçeneğiyle tanımlanmış eşlemeleri için geçerlidir. Eşleştirme (bir belirli noktaya kazandı doyumsuz uzun desen) değildir. Yedek, boş dize izin verilmez.<br /><br /> **Seçenekler**<br /><br /> eşlemeleri (tür: dize dizisi)-aşağıdaki biçimde eşlemeleri listesi: "bir = > b" (tüm örneklerini "a" değiştirilir "b" karakteri ile karakter). Gereklidir.|  
|[pattern_replace](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/pattern/PatternReplaceCharFilter.html)|PatternReplaceCharFilter|Giriş dizesindeki bir karakter değiştirir char filtre. Değiştirilecek korumak için karakter dizileri tanımlamak için normal bir ifade ve karakter tanımlamak için bir değiştirme deseni kullanır. Örneğin, metin girişi "aa bb aa bb", = pattern="(aa)\\\s+(bb)" değiştirme = "$1 # 2$" sonuç "aa #bb aa #bb" =.<br /><br /> **Seçenekler**<br /><br /> Desen (tür: string) - gerekli.<br /><br /> değiştirme (tür: string) - gerekli.|  

 <sup>1</sup> filtre türleri char her zaman ön eki bulunur "#Microsoft.Azure.Search" ile koddaki "MappingCharFilter" "#Microsoft.Azure.Search.MappingCharFilter. gerçekten belirtilecek şekilde Tablonun genişliğini azaltır, ancak gönderdiğinizden kodunuza ekleyin için önek kaldırdık. Bu char_filter_type yalnızca özelleştirilebilir filtreleri için sağlanan dikkat edin. Varsa hiçbir seçenek html_strip olduğu gibi ilişkili #Microsoft.Azure.Search türü yoktur.

<a name="Tokenizers"></a>

###  <a name="tokenizers-reference"></a>Oluşturma denenmeden başvurusu

Aşağıdaki tabloda, Apache Lucene kullanılarak gerçekleştirilen oluşturma denenmeden Lucene API belgeleriyle bağlanır.

|**tokenizer_name**|**tokenizer_type** <sup>1</sup>|**Açıklamayı ve seçenekleri**|  
|-|-|-|  
|[Klasik](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/ClassicTokenizer.html)|ClassicTokenizer|Çoğu Avrupa dil belgeleri işlemek için uygun olan temel dil bilgisi belirteç Oluşturucu.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (tür: int)-en fazla belirteç uzunluğu. Varsayılan: 255, en fazla: 300. Belirteçleri uzunluk üst sınırından daha uzun bölünür.|  
|[edgeNGram](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenizer.html)|EdgeNGramTokenizer|Bir edge girişini n-gram tokenizes size(s) verilir.<br /><br /> **Seçenekler**<br /><br /> minGram (tür: int)-varsayılan: 1, en fazla: 300.<br /><br /> maxGram (tür: int)-varsayılan: 2, en fazla: 300. MinGram büyük olmalıdır.<br /><br /> tokenChars (tür: dize dizisi)-karakter belirteçleri tutmak sınıfları. İzin verilen değerler: <br />"harfi", "basamaklı", "boşluk", "noktalama", "sembol". Boş bir dizi için varsayılan olarak - tüm karakterleri tutar. |  
|[keyword_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/KeywordTokenizer.html)|KeywordTokenizerV2|Tüm giriş tek bir belirteç verir.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (tür: int)-en fazla belirteç uzunluğu. Varsayılan: 256, en fazla: 300. Belirteçleri uzunluk üst sınırından daha uzun bölünür.|  
|[Harf](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/LetterTokenizer.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Harf olmayan metin böler. 255 karakterden uzun belirteçleri bölünür.|  
|[Küçük](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/LowerCaseTokenizer.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Harf olmayan metin böler ve bunları küçük harflere dönüştürür. 255 karakterden uzun belirteçleri bölünür.|  
| microsoft_language_tokenizer| MicrosoftLanguageTokenizer| Dile özgü kurallarını kullanarak metin böler.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (tür: int)-varsayılan, belirteç en fazla uzunluk: 255, en fazla: 300. Belirteçleri uzunluk üst sınırından daha uzun bölünür. Belirteçleri 300 karakterden önce uzunluğu 300 belirteçlere bölündüğünde ve ardından her biri, bu belirteçleri bölünür daha uzun maxTokenLength kümesi temel alınarak.<br /><br />isSearchTokenizer (tür: bool) - ayarlanmış arama belirteç Oluşturucu kullanılması durumunda true olarak false dizinleme simgeleştirici kullandıysanız ayarlayın. <br /><br /> Dil (tür: string)-varsayılan "İngilizce" dili kullanmak için. İzin verilen değerler şunlardır:<br />"bangla", "Bulgarca", "Katalanca", "chineseSimplified", "chineseTraditional", "Hırvatça", "Çekçe", "Danca", "Felemenkçe", "İngilizce", "Fransızca", "Almanca", "Yunanca", "Gucerat dili", "Hint", "İzlanda", "Endonezya", "İtalyanca", "Japonca", "Kannada dili" " Kore dili","malay","Malayalam dili","Marathi dili","norwegianBokmaal","Lehçe","Portekizce","portugueseBrazilian","punjabi","Rumence","Rusça","serbianCyrillic","serbianLatin","Slovence","İspanyolca","İsveççe","Tamilce","telugu","Tay dili"" Ukrayna","urdu","Vietnam" |
| microsoft_language_stemming_tokenizer | MicrosoftLanguageStemmingTokenizer| Dile özgü kurallarını kullanarak metin böler ve temel formlarına sözcükleri azaltır<br /><br /> **Seçenekler**<br /><br />maxTokenLength (tür: int)-varsayılan, belirteç en fazla uzunluk: 255, en fazla: 300. Belirteçleri uzunluk üst sınırından daha uzun bölünür. Belirteçleri 300 karakterden önce uzunluğu 300 belirteçlere bölündüğünde ve ardından her biri, bu belirteçleri bölünür daha uzun maxTokenLength kümesi temel alınarak.<br /><br /> isSearchTokenizer (tür: bool) - ayarlanmış arama belirteç Oluşturucu kullanılması durumunda true olarak false dizinleme simgeleştirici kullandıysanız ayarlayın.<br /><br /> Dil (tür: string)-varsayılan "İngilizce" dili kullanmak için. İzin verilen değerler şunlardır:<br />"Arapça", "bangla", "Bulgarca", "Katalanca", "Hırvatça", "Çekçe", "Danca", "Felemenkçe", "İngilizce", "Estonca", "Fince", ", Fransızca" "Almanca", "Yunanca", "Gucerat dili", "İbranice", "Hint", "Macarca", "İzlanda", "Endonezya", "İtalyanca", "Kannada dili" " Letonca","Litvanca","malay","Malayalam dili","Marathi dili","norwegianBokmaal","Lehçe","Portekizce","portugueseBrazilian","punjabi","Rumence","Rusça","serbianCyrillic","serbianLatin","Slovakça","Slovence","İspanyolca","İsveççe"" Tamilce","telugu","Türkçe","Ukrayna","Urduca" |
|[nGram](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ngram/NGramTokenizer.html)|NGramTokenizer|N-gram, verilen size(s) giriş tokenizes.<br /><br /> **Seçenekler**<br /><br /> minGram (tür: int)-varsayılan: 1, en fazla: 300.<br /><br /> maxGram (tür: int)-varsayılan: 2, en fazla: 300. MinGram büyük olmalıdır. <br /><br /> tokenChars (tür: dize dizisi)-karakter belirteçleri tutmak sınıfları. İzin verilen değerler: "harfi", "basamaklı", "boşluk", "noktalama", "sembol". Boş bir dizi için varsayılan olarak - tüm karakterleri tutar. |  
|[path_hierarchy_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/path/PathHierarchyTokenizer.html)|PathHierarchyTokenizerV2|Yol benzeri Hiyerarşiler için belirteç Oluşturucu.<br /><br /> **Seçenekler**<br /><br /> sınırlayıcı (tür: string)-varsayılan: ' /.<br /><br /> değiştirme (tür: string) - ayarlayın, sınırlayıcı karakter değiştirir. Aynı sınırlayıcı değeri olarak varsayılan.<br /><br /> maxTokenLength (tür: int)-en fazla belirteç uzunluğu. Varsayılan: 300, en fazla: 300. MaxTokenLength uzun yollar yoksayılır.<br /><br /> Ters (tür: bool) - true ise ters sırada belirteci oluşturur. Varsayılan: false.<br /><br /> Atla (tür: bool)-ilk atlamak için belirteçleri. Varsayılan değer 0'dır.|  
|[Düzeni](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/pattern/PatternTokenizer.html)|PatternTokenizer|Bu belirteç Oluşturucu, ayrı belirteçleri oluşturmak için normal ifade deseniyle eşleşen kullanır.<br /><br /> **Seçenekler**<br /><br /> Desen (tür: string)-normal ifade deseni. \W+ varsayılandır.<br /><br /> [bayrakları](https://docs.oracle.com/javase/6/docs/api/java/util/regex/Pattern.html#field_summary) (tür: string)-normal ifade bayrakları. Varsayılan değer boş bir dizedir. İzin verilen değerler: CANON_EQ CASE_INSENSITIVE, YORUMLAR, DOTALL, LITERAL, ÇOK SATIRLI, UNICODE_CASE, UNIX_LINES<br /><br /> Grup (tür: int)-belirteçlere ayıklamak için hangi Grup. -1 (bölme) varsayılandır.|  
|[standard_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/StandardTokenizer.html)|StandardTokenizerV2|Aşağıdaki metni keser [Unicode metin Segment kuralları](https://unicode.org/reports/tr29/).<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (tür: int)-en fazla belirteç uzunluğu. Varsayılan: 255, en fazla: 300. Belirteçleri uzunluk üst sınırından daha uzun bölünür.|  
|[uax_url_email](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/UAX29URLEmailTokenizer.html)|UaxUrlEmailTokenizer|URL'ler ve e-postaları bir belirteç olarak tokenizes.<br /><br /> **Seçenekler**<br /><br /> maxTokenLength (tür: int)-en fazla belirteç uzunluğu. Varsayılan: 255, en fazla: 300. Belirteçleri uzunluk üst sınırından daha uzun bölünür.|  
|[Boşluk](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/WhitespaceTokenizer.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir) |Boşlukta metin ayıran. 255 karakterden uzun belirteçleri bölünür.|  

 <sup>1</sup> simgeleştirici türlerini her zaman ön eki bulunur "#Microsoft.Azure.Search" ile koddaki "ClassicTokenizer" aslında "#Microsoft.Azure.Search.ClassicTokenizer" belirtilecek şekilde. Tablonun genişliğini azaltır, ancak gönderdiğinizden kodunuza ekleyin için önek kaldırdık. Bu tokenizer_type yalnızca özelleştirilebilen oluşturma denenmeden sağlanan dikkat edin. Varsa hiçbir seçenek harf simgeleştirici olduğu gibi ilişkili #Microsoft.Azure.Search türü yoktur.

<a name="TokenFilters"></a>

###  <a name="token-filters-reference"></a>Belirteç filtreleri başvurusu

Aşağıdaki tabloda, Apache Lucene kullanılarak uygulanan belirteç filtreleri Lucene API belgeleriyle bağlanır.

|**token_filter_name**|**token_filter_type** <sup>1</sup>|**Açıklamayı ve seçenekleri**|  
|-|-|-|  
|[arabic_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ar/ArabicNormalizationFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Arapça normalizer orthography'leri normalleştirmek için geçerli belirteç filtre.|  
|[Kesme işareti](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/tr/ApostropheFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Tüm karakterleri (kesme işareti kendisi dahil) bir kesme işareti sonra kaldırır. |  
|[asciifolding](https://lucene.apache.org/core/4_0_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html)|AsciiFoldingTokenFilter|Varsa, ilk 127 ASCII karakterler ("Temel Latin" Unicode bloğu) ASCII eşdeğerlerine olmayan alfabetik, sayısal ve sembolik Unicode karakterler dönüştürür.<br /><br /> **Seçenekler**<br /><br /> preserveOriginal (tür: bool) - true, özgün belirteç tutulur. Varsayılan değer false'dur.|  
|[cjk_bigram](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/cjk/CJKBigramFilter.html)|CjkBigramTokenFilter|CJK koşulları StandardTokenizer oluşturulan bigrams oluşturur.<br /><br /> **Seçenekler**<br /><br /> ignoreScripts (tür: dize dizisi)-betikler yok sayılacak. İzin verilen değerler: "han", "hiragana", "katakana", "hangul". Varsayılan değer boş bir listedir.<br /><br /> outputUnigrams (tür: bool) - her zaman unigrams hem bigrams çıkış istiyorsanız true olarak ayarlayın. Varsayılan değer false'dur.|  
|[cjk_width](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/cjk/CJKWidthFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |CJK genişliği farklar normalleştirir. Büyük Katlama tam genişlik ASCII çeşitleri eşdeğer temel latin içine ve eşdeğer kana içine yarım genişlikteki Katakana çeşitleri. |  
|[Klasik](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/standard/ClassicFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |İngilizce iyelik kaldırır ve kısaltmalar nokta. |  
|[common_grams](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/commongrams/CommonGramsFilter.html)|CommonGramTokenFilter|Dizin oluşturma sırasında koşulları hangi sıklıkta ortaya çıktığını için bigrams oluşturun. Tek koşulları yine de yayılan bigrams ile dizinlenir.<br /><br /> **Seçenekler**<br /><br /> commonWords (tür: dize dizisi)-ortak bir kelimelerin kümesi. Varsayılan değer boş bir listedir. Gereklidir.<br /><br /> ignoreCase (tür: bool) - true ise eşleştirme büyük küçük harfe duyarlı değildir. Varsayılan değer false'dur.<br /><br /> queryMode (tür: bool) - bigrams oluşturur sonra ortak kelimeler ve ortak bir sözcük tek koşulları kaldırır. Varsayılan değer false'dur.|  
|[dictionary_decompounder](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/compound/DictionaryCompoundWordTokenFilter.html)|DictionaryDecompounderTokenFilter|Bileşik sözcüklerin cermen dillerinde bulunan çözer.<br /><br /> **Seçenekler**<br /><br /> sözcük listesi (tür: dize dizisi)-eşleştirilecek sözcüklerin listesi. Varsayılan değer boş bir listedir. Gereklidir.<br /><br /> minWordSize (tür: int)-yalnızca bundan daha uzun sözcük işlendi. Varsayılan değer 5'tir.<br /><br /> minSubwordSize (tür: int)-yalnızca subwords bundan daha uzun yüzdelik. Varsayılan değer 2'dir.<br /><br /> maxSubwordSize (tür: int)-yalnızca subwords bu değerden daha kısa yüzdelik. Varsayılan değer 15 dakikadır.<br /><br /> onlyLongestMatch (tür: bool)-çıktısını almak için yalnızca eşleşen en uzun alt sözcüğe ekleyin. Varsayılan değer false'dur.|  
|[edgeNGram_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ngram/EdgeNGramTokenFilter.html)|EdgeNGramTokenFilterV2|N-gram, verilen size(s) önüne veya arkasına bir giriş belirteci başlatılmasını oluşturur.<br /><br /> **Seçenekler**<br /><br /> minGram (tür: int)-varsayılan: 1, en fazla: 300.<br /><br /> maxGram (tür: int)-varsayılan: 2, en fazla 300. MinGram büyük olmalıdır.<br /><br /> yan (tür: string)-giriş hangi tarafta, n-gram gelen oluşturulacağını belirtir. İzin verilen değerler: ","geri"ön" |  
|[Eleme](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/util/ElisionFilter.html)|ElisionTokenFilter|Elisions kaldırır. Örneğin, "l'avion" (düz) "avion" (düz) dönüştürülür.<br /><br /> **Seçenekler**<br /><br /> makaleler (tür: dize dizisi)-makaleleri kaldırmak için bir dizi. Varsayılan değer boş bir listedir. Makaleler kümesi listesi varsa, varsayılan olarak tüm Fransızca makaleleri kaldırılır.|  
|[german_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/de/GermanNormalizationFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Buluşsal yöntemler göre Almanca karakterleri normalleştirir [German2 Snowball'un algoritması](https://snowballstem.org/algorithms/german2/stemmer.html) .|  
|[hindi_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/hi/HindiNormalizationFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Metinde yazım çeşitlemeleri bazı farklılıklar kaldırmak için Hintçe normalleştirir. |  
|[indic_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/in/IndicNormalizationFilter.html)|IndicNormalizationTokenFilter|Hint dilleri metinde Unicode gösterimi normalleştirir.
|[Koru](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeepWordFilter.html)|KeepTokenFilter|Yalnızca belirteçleri sözcük belirtilen listede yer alan metinle tutar belirteci filtre.<br /><br /> **Seçenekler**<br /><br /> keepWords (tür: dize dizisi)-tutmak sözcükler listesi. Varsayılan değer boş bir listedir. Gereklidir.<br /><br /> keepWordsCase (tür: bool) - true ise alt tüm sözcükleri ilk durum. Varsayılan değer false'dur.|  
|[keyword_marker](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeywordMarkerFilter.html)|KeywordMarkerTokenFilter|Koşulları anahtar sözcükler olarak işaretler.<br /><br /> **Seçenekler**<br /><br /> anahtar sözcükler (tür: dize dizisi)-olan anahtar sözcükler işaretlemek için bir kelimelerin listesini. Varsayılan değer boş bir listedir. Gereklidir.<br /><br /> ignoreCase (tür: bool) - true ise alt tüm sözcükleri ilk durum. Varsayılan değer false'dur.|  
|[keyword_repeat](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/KeywordRepeatFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Her gelen belirteci iki kez bir kez anahtar sözcüğü ve anahtar sözcük olmayan olarak bir kez olarak yayar. |  
|[kstem](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/en/KStemFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |İngilizce için yüksek performanslı kstem filtre. |  
|[Uzunluğu](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/LengthFilter.html)|LengthTokenFilter|Çok uzun veya kısa kelimeler kaldırır.<br /><br /> **Seçenekler**<br /><br /> Min (tür: int)-en az. Varsayılan: 0, en fazla: 300.<br /><br /> max (tür: int)-en fazla. Varsayılan: 300, en fazla: 300.|  
|[Sınırı](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/LimitTokenCountFilter.html)|Microsoft.Azure.Search.LimitTokenFilter|Dizin oluştururken belirteçleri sayısını sınırlar.<br /><br /> **Seçenekler**<br /><br /> maxTokenCount (tür: int)-belirteçleri oluşturmak için en fazla sayısı. Varsayılan değer 1'dir.<br /><br /> consumeAllTokens (tür: bool) - olmadığını maxTokenCount ulaşıldığında bile tüm belirteçlerin girdisinden kullanılması gerekir. Varsayılan değer false'dur.|  
|[Küçük](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/LowerCaseFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Belirteci metni küçük harfe normalleştirir. |  
|[nGram_v2](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ngram/NGramTokenFilter.html)|NGramTokenFilterV2|N-gram, verilen size(s) oluşturur.<br /><br /> **Seçenekler**<br /><br /> minGram (tür: int)-varsayılan: 1, en fazla: 300.<br /><br /> maxGram (tür: int)-varsayılan: 2, en fazla 300. MinGram büyük olmalıdır.|  
|[pattern_capture](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/pattern/PatternCaptureGroupTokenFilter.html)|PatternCaptureTokenFilter|Her bir yakalama grubu bir veya daha fazla desenleri için birden çok belirteç yaymak için Java normal ifadeler kullanır.<br /><br /> **Seçenekler**<br /><br /> desenler (tür: dize dizisi)-her belirteç karşı eşleştirilecek desenlerinin bir listesi. Gereklidir.<br /><br /> preserveOriginal (tür: bool) - desenlerden biriyle eşleşen varsayılan bile özgün belirteç döndürmek için true olarak ayarlayın: true |  
|[pattern_replace](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/pattern/PatternReplaceFilter.html)|PatternReplaceTokenFilter|Belirtilen değiştirme dizesiyle eşleşen oluşum değiştirerek her belirteç akıştaki bir desen uygulandığı bir belirteç Filtresi.<br /><br /> **Seçenekler**<br /><br /> Desen (tür: string) - gerekli.<br /><br /> değiştirme (tür: string) - gerekli.|  
|[persian_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/fa/PersianNormalizationFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir) |Normalleştirme Farsça için geçerlidir. |  
|[Ses](https://lucene.apache.org/core/4_10_3/analyzers-phonetic/org/apache/lucene/analysis/phonetic/package-tree.html)|PhoneticTokenFilter|Belirteçleri fonetik eşleşmeleri için oluşturun.<br /><br /> **Seçenekler**<br /><br /> Kodlayıcı (tür: string)-kullanılacak fonetik Kodlayıcı. İzin verilen değerler: "metaphone", "doubleMetaphone", "soundex", "refinedSoundex", "caverphone1", "caverphone2", "cologne", "nysiis", "koelnerPhonetik", "haasePhonetik", "beiderMorse". Varsayılan: "metaphone". Metaphone varsayılandır.<br /><br /> Bkz: [Kodlayıcı](https://commons.apache.org/proper/commons-codec/archives/1.10/apidocs/org/apache/commons/codec/language/package-frame.html) daha fazla bilgi için.<br /><br /> Değiştir (tür: bool) - True eş anlamlılar eklenecekse kodlanmış belirteçleri özgün belirteç, yanlış değiştirmeniz gerekir. Varsayılan değer True'dur.|  
|[porter_stem](https://lucene.apache.org/core/4_10_3/analyzers-common/org/tartarus/snowball/ext/PorterStemmer.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Belirteç akışı olarak başına dönüştüren [bağlantısı kökünü ayırmayı algoritması](https://tartarus.org/~martin/PorterStemmer/). |  
|[geriye doğru](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/reverse/ReverseStringFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Belirteç dizesini tersine çevirir. |  
|[scandinavian_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/ScandinavianNormalizationFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Birbirinin yerine İskandinav karakter normalleştirir. |  
|[scandinavian_folding](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/ScandinavianFoldingFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Folds Scandinavian characters åÅäæÄÆ->a and öÖøØ->o. Ayrıca çift sesli harfler kullanımına karşı discriminates aa, ae, saniye başına ao, oe ve paylaşımlarınızda, yalnızca ilk eşleşmenin çıkılıyor. |  
|[astık](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/shingle/ShingleFilter.html)|ShingleTokenFilter|Belirteçleri birleşimleri tek bir belirteç oluşturur.<br /><br /> **Seçenekler**<br /><br /> maxShingleSize (tür: int)-varsayılan olarak, 2.<br /><br /> minShingleSize (tür: int)-varsayılan olarak, 2.<br /><br /> outputUnigrams (tür: bool) - true ise çıkış akışına kaplama karolarını yanı sıra giriş belirteçleri (unigrams) içeriyorsa. Varsayılan değer True'dur.<br /><br /> outputUnigramsIfNoShingles (tür: bool) - true ise outputUnigrams davranışını geçersiz kılmak için hiçbir kaplama karolarını kullanılabilir olduğunda bu süreleri == false. Varsayılan değer false'dur.<br /><br /> tokenSeparator (tür: string)-komşu belirteçlerini birleştirilirken bir astık oluşturmak için kullanılacak dize. Varsayılan değer "".<br /><br /> filterToken (tür: string)-var olduğu hiçbir belirteç her konum için eklenecek dize. Varsayılan değer "_" dir.|  
|[Snowball'un](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/snowball/SnowballFilter.html)|SnowballTokenFilter|Snowball'un belirteci Filtresi.<br /><br /> **Seçenekler**<br /><br /> Dil (tür: string)-izin verilen değerler: "Ermenice", "Bask dili", "Katalanca", "Danca", "Felemenkçe", "İngilizce", "Fince", "Fransızca", "Almanca", "german2", "Macarca", "İtalyanca", "kp", "lovins", "Norveç", "bağlantısı", "Portekizce", "Rumence" " Rusça","İspanyolca","İsveççe","Türkçe"|  
|[sorani_normalization](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/ckb/SoraniNormalizationFilter.html)|SoraniNormalizationTokenFilter|Unicode gösterimi Sorani metin normalleştirir.<br /><br /> **Seçenekler**<br /><br /> Yok.|  
|Ayırıcı|StemmerTokenFilter|Dile özgü kökünü ayırmayı Filtresi.<br /><br /> **Seçenekler**<br /><br /> Dil (tür: string)-izin verilen değerler şunlardır: <br /> -   ["Arapça"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/ar/ArabicStemmer.html)<br />-   ["Ermenice"](https://snowballstem.org/algorithms/armenian/stemmer.html)<br />-   ["Bask dili"](https://snowballstem.org/algorithms/basque/stemmer.html)<br />-   ["Brezilya"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/br/BrazilianStemmer.html)<br />-"Bulgarca"<br />-   ["Katalanca"](https://snowballstem.org/algorithms/catalan/stemmer.html)<br />-   ["Çekçe"](https://portal.acm.org/citation.cfm?id=1598600)<br />-   ["Danca"](https://snowballstem.org/algorithms/danish/stemmer.html)<br />-   ["Felemenkçe"](https://snowballstem.org/algorithms/dutch/stemmer.html)<br />-   ["dutchKp"](https://snowballstem.org/algorithms/kraaij_pohlmann/stemmer.html)<br />-   ["İngilizce"](https://snowballstem.org/algorithms/porter/stemmer.html)<br />-   ["lightEnglish"](https://ciir.cs.umass.edu/pubfiles/ir-35.pdf)<br />-   ["minimalEnglish"](https://www.researchgate.net/publication/220433848_How_effective_is_suffixing)<br />-   ["possessiveEnglish"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/en/EnglishPossessiveFilter.html)<br />-   ["porter2"](https://snowballstem.org/algorithms/english/stemmer.html)<br />-   ["lovins"](https://snowballstem.org/algorithms/lovins/stemmer.html)<br />-   ["Fince"](https://snowballstem.org/algorithms/finnish/stemmer.html)<br />-   "lightFinnish"<br />-   ["Fransızca"](https://snowballstem.org/algorithms/french/stemmer.html)<br />-   ["lightFrench"](https://dl.acm.org/citation.cfm?id=1141523)<br />-   ["minimalFrench"](https://dl.acm.org/citation.cfm?id=318984)<br />-"Galiçya"<br />-"minimalGalician"<br />-   ["Almanca"](https://snowballstem.org/algorithms/german/stemmer.html)<br />-   ["german2"](https://snowballstem.org/algorithms/german2/stemmer.html)<br />-   ["lightGerman"](https://dl.acm.org/citation.cfm?id=1141523)<br />-"minimalGerman"<br />-   ["Yunanca"](https://sais.se/mthprize/2007/ntais2007.pdf)<br />-"Hintçe"<br />-   ["Macarca"](https://snowballstem.org/algorithms/hungarian/stemmer.html)<br />-   ["lightHungarian"](https://dl.acm.org/citation.cfm?id=1141523&dl=ACM&coll=DL&CFID=179095584&CFTOKEN=80067181)<br />-   ["Endonezya"](https://www.illc.uva.nl/Publications/ResearchReports/MoL-2003-02.text.pdf)<br />-   ["İrlanda"](https://snowballstem.org/otherapps/oregan/)<br />-   ["İtalyanca"](https://snowballstem.org/algorithms/italian/stemmer.html)<br />-   ["lightItalian"](https://www.ercim.eu/publication/ws-proceedings/CLEF2/savoy.pdf)<br />-   ["sorani"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/ckb/SoraniStemmer.html)<br />-   ["Letonca"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/lv/LatvianStemmer.html)<br />-   ["Norveç"](https://snowballstem.org/algorithms/norwegian/stemmer.html)<br />-   ["lightNorwegian"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/no/NorwegianLightStemmer.html)<br />-   ["minimalNorwegian"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/no/NorwegianMinimalStemmer.html)<br />-   ["lightNynorsk"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/no/NorwegianLightStemmer.html)<br />-   ["minimalNynorsk"](https://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/no/NorwegianMinimalStemmer.html)<br />-   ["Portekizce"](https://snowballstem.org/algorithms/portuguese/stemmer.html)<br />-   ["lightPortuguese"](https://dl.acm.org/citation.cfm?id=1141523&dl=ACM&coll=DL&CFID=179095584&CFTOKEN=80067181)<br />-   ["minimalPortuguese"](https://www.inf.ufrgs.br/~buriol/papers/Orengo_CLEF07.pdf)<br />-   ["portugueseRslp"](https://www.inf.ufrgs.br//~viviane/rslp/index.htm)<br />-   ["Rumence"](https://snowballstem.org/otherapps/romanian/)<br />-   ["Rusça"](https://snowballstem.org/algorithms/russian/stemmer.html)<br />-   ["lightRussian"](https://doc.rero.ch/lm.php?url=1000%2C43%2C4%2C20091209094227-CA%2FDolamic_Ljiljana_-_Indexing_and_Searching_Strategies_for_the_Russian_20091209.pdf)<br />-   ["İspanyolca"](https://snowballstem.org/algorithms/spanish/stemmer.html)<br />-   ["lightSpanish"](https://www.ercim.eu/publication/ws-proceedings/CLEF2/savoy.pdf)<br />-   ["İsveççe"](https://snowballstem.org/algorithms/swedish/stemmer.html)<br />-   "lightSwedish"<br />-   ["Türkçe"](https://snowballstem.org/algorithms/turkish/stemmer.html)|  
|[stemmer_override](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/StemmerOverrideFilter.html)|StemmerOverrideTokenFilter|Tüm sözlük Stemmed koşullarını zinciri dallanma önleyen anahtar sözcükler olarak işaretlenir. Herhangi bir sözcük kökü ayırmayı filtrelerden önce yerleştirilmelidir.<br /><br /> **Seçenekler**<br /><br /> Kuralları (tür: dize dizisi)-kuralları aşağıdaki biçimde dallanma "word = > kökü" Örneğin "çalıştı = > Çalıştır". Varsayılan değer boş bir listedir.  Gereklidir.|  
|[durdurma sözcükleri](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/StopFilter.html)|StopwordsTokenFilter|Belirteç akışı sözcükleri kaldırır durdurun. Varsayılan olarak, filtre İngilizce için önceden tanımlanmış Durma sözcüğü listesini kullanır.<br /><br /> **Seçenekler**<br /><br /> stopword (tür: dize dizisi)-stopword listesi. Bir stopwordsList belirtilmezse belirtilemez.<br /><br /> stopwordsList (tür: string)-stopword önceden tanımlanmış bir listesi. Durdurma sözcükleri belirtilmezse belirtilemez. İzin verilen değerler: "Arapça", "Ermenice", "Bask dili", "Brezilya", "Bulgarca", "Katalanca", "Çekçe", "Danca", "Felemenkçe", "İngilizce", "Fince", "Fransızca", "Galiçya dili", "Almanca", "Yunanca", "Hint", "Macarca", "Endonezya", "İrlanda", "İtalyanca", "Letonca ","Norveççe","fars","Portekizce","Rumence","Rusça","sorani","İspanyolca","İsveççe","Tay dili","Türkçe", varsayılan:"İngilizce". Durdurma sözcükleri belirtilmezse belirtilemez. <br /><br /> ignoreCase (tür: bool) - true ise tüm sözcüklerin büyük/küçük harfleri daha düşük bir ilk olan. Varsayılan değer false'dur.<br /><br /> removeTrailing (tür: bool) - true ise bir Durma sözcüğü ise son arama terimi yoksay. Varsayılan değer True'dur.
|[synonym](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/synonym/SynonymFilter.html)|SynonymTokenFilter|Eşleşmeleri tek veya birden çok sözcük anlamlı bir belirteç akışı.<br /><br /> **Seçenekler**<br /><br /> Eş Anlamlılar (tür: dize dizisi) - gerekli. Aşağıdaki iki biçimlerden birinde listesi:<br /><br /> -inanılmaz, inanılmaz, harika = > - = sol tarafındaki tüm koşulları harika > Sembol, sağ taraftaki tüm koşulları ile değiştirilir.<br /><br /> -inanılmaz, inanılmaz, harika, harika - eşdeğer bir kelimelerin bir virgülle ayrılmış listesi. Bu listeyi nasıl yorumlanacağını değiştirmek için genişletme seçeneğini ayarlayın.<br /><br /> ignoreCase (tür: bool)-eşleşen servis talebi hatları giriş. Varsayılan değer false'dur.<br /><br /> genişletin (tür: bool) - true ise tüm sözcükleri eş anlamlılar listesinde (varsa = > gösterimi kullanılmaz) birbirine eşleme. <br />Aşağıdaki listede: inanılmaz, inanılmaz, harika, harika eşdeğerdir: inanılmaz, inanılmaz, harika, harika = > inanılmaz, inanılmaz, harika, harika.<br /><br />-Eğer false, aşağıdaki listede: inanılmaz, inanılmaz, harika, harika eşdeğerdir: inanılmaz, inanılmaz, harika, harika = > inanılmaz.|  
|[Kırpma](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/TrimFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Baştaki ve sondaki gelen belirteçleri kırpar. |  
|[Kes](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/TruncateTokenFilter.html)|TruncateTokenFilter|Koşulları, belirli bir süre içinde keser.<br /><br /> **Seçenekler**<br /><br /> uzunluk (tür: int)-varsayılan: 300, en fazla: 300. Gereklidir.|  
|[benzersiz](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/RemoveDuplicatesTokenFilter.html)|UniqueTokenFilter|Aynı metni olan önceki belirtece belirteçleriyle filtreler.<br /><br /> **Seçenekler**<br /><br /> onlyOnSamePosition (tür: bool) - ayarlamak, yalnızca aynı konumda Yinelenenleri Kaldır. Varsayılan değer True'dur.|  
|[büyük harf](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/core/UpperCaseFilter.html)|(türü yalnızca seçenekleri kullanılabilir olduğunda geçerlidir)  |Belirteç metni büyük harfe normalleştirir. |  
|[word_delimiter](https://lucene.apache.org/core/4_10_3/analyzers-common/org/apache/lucene/analysis/miscellaneous/WordDelimiterFilter.html)|WordDelimiterTokenFilter|Subwords sözcükleri böler ve alt sözcüğe gruplarında isteğe bağlı dönüşümleri gerçekleştirir.<br /><br /> **Seçenekler**<br /><br /> generateWordParts (tür: bool)-bölümleri sözcük "Azure Search", "Azure" "arama" olur örneğin oluşturulmasına neden olur. Varsayılan değer True'dur.<br /><br /> generateNumberParts (tür: bool)-sayı subwords oluşturulmasına neden olur. Varsayılan değer True'dur.<br /><br /> catenateWords (tür: bool)-örnek "Azure Search", "Azure Search" haline gelir word bölümlerinin catenated için en fazla çalıştırma neden olur. Varsayılan değer false'dur.<br /><br /> catenateNumbers (tür: bool)-neden en fazla sayı bölümleri catenated için çalışır, örneğin "1-2" "12" olur. Varsayılan değer false'dur.<br /><br /> catenateAll (tür: bool)-tüm alt sözcüğe nedenleri bölümleri catenated için ör. "Azure-Search-1", "AzureSearch1" olur. Varsayılan değer false'dur.<br /><br /> splitOnCaseChange (tür: bool) - bölmelerini kelimeler caseChange, "Azure"Search üzerinde true olur, "Azure" "arama". Varsayılan değer True'dur.<br /><br /> preserveOriginal - korunur ve alt sözcüğe listeye eklenen için özgün sözcükleri neden olur. Varsayılan değer false'dur.<br /><br /> splitOnNumerics (tür: bool) - true ise bölmelerini sayılar, örneğin "Azure1Search" haline gelir "Azure" "1" üzerindeki "araması yaparsanız". Varsayılan değer True'dur.<br /><br /> stemEnglishPossessive (tür: bool) - her alt sözcüğe için kaldırılacak "kullanıcının" sondaki neden olur. Varsayılan değer True'dur.<br /><br /> protectedWords (tür: dize dizisi)-ayrılmış korumak için belirteçleri. Varsayılan değer boş bir listedir.|  

 <sup>1</sup> belirteç filtre türleri her zaman ön eki bulunur "#Microsoft.Azure.Search" ile koddaki "ArabicNormalizationTokenFilter" aslında "#Microsoft.Azure.Search.ArabicNormalizationTokenFilter" belirtilecek şekilde.  Tablonun genişliğini azaltır, ancak gönderdiğinizden kodunuza ekleyin için önek kaldırdık.  

> [!NOTE]
> Belirteç 300 karakterden uzun olmayan üretmek için özel çözümleyici yapılandırma zorunludur. Bu tür belirteçleri ile ilgili belgeler için dizin oluşturma başarısız olur. Bunları trim veya bunların yoksayılması için kullanın **TruncateTokenFilter** ve **LengthTokenFilter** sırasıyla.


## <a name="see-also"></a>Ayrıca bkz.  
 [Azure Search Service REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [Azure Search'te çözümleyiciler > örnekleri](search-analyzers.md#examples)    
 [Dizin oluşturma &#40;Azure arama hizmeti REST API'si&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  
