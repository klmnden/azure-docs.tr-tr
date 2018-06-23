---
title: Bilgi Bankası araştırması hizmeti API'si dilbilgisi biçiminde | Microsoft Docs
description: İçinde bilgi araştırması hizmet (KES) API Bilişsel Hizmetleri'ndeki dilbilgisi biçimi hakkında bilgi edinin.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 27202379b8c36696a380049336229cac040b0108
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351881"
---
# <a name="grammar-format"></a>Dilbilgisi biçimi
Dilbilgisi hizmet bu doğal dil sorguları anlam sorgusu ifadelere nasıl dönüştürüleceğini yanı sıra yorumlaması için doğal dil sorguları ağırlıklı kümesini belirten bir XML dosyasıdır.  Dilbilgisi sözdizimi dayanır [SRGS](http://www.w3.org/TR/speech-grammar/), veri dizin tümleştirmesi ve anlam işlevleri desteklemek için uzantıları olan konuşma tanıma aynı için W3C standart.

Aşağıda bir dilbilgisi kullanılabilir söz dizimi öğelerin her biri açıklanmaktadır.  Bkz: [Bu örnek](#example) bağlamında bu öğelerin kullanımını gösteren tam bir dilbilgisi için.

### <a name="grammar-element"></a>Dilbilgisi öğesi 
`grammar` XML dilbilgisi belirtiminde en üst düzey öğesi bir öğedir.  Gerekli `root` özniteliği dilbilgisi başlangıç noktası tanımlar kök kuralının adını belirtir.

```xml
<grammar root="GetPapers">
```

### <a name="import-element"></a>içeri aktarma öğesi
`import` Öğesi bir şema tanımı özniteliği başvuruları etkinleştirmek için harici bir dosyadan içeri aktarır. Üst düzey bir alt öğesi olması gerekir `grammar` öğesi ve herhangi bir önce görünür `attrref` öğeleri. Gerekli `schema` özniteliği dilbilgisi XML dosyası ile aynı dizinde bulunan bir şema dosyası adını belirtir. Gerekli `name` öğesi belirttiğinden diğer adı bu sonraki şema `attrref` öğeleri bu şemasında tanımlanan özniteliklere başvururken kullanın.

```xml
  <import schema="academic.schema" name="academic"/>
```

### <a name="rule-element"></a>Kural öğesi
`rule` Öğesi dilbilgisi kural, sistem çevirebilir sorgu ifadeleri kümesini belirtir bir Yapısal birim tanımlar.  Üst düzey bir alt öğesi olması gerekir `grammar` öğesi.  Gerekli `id` özniteliği içinden başvuruda kuralının adını belirtir `grammar` veya `ruleref` öğeleri.

A `rule` öğesi yasal genişletmeleri kümesini tanımlar.  Metin belirteçleri giriş sorgusu doğrudan eşleşir.  `item` öğeleri yorumlama olasılıklar alter ve yineler belirtin.  `one-of` öğeleri diğer seçenekleri belirtin.  `ruleref` öğeleri daha basit nitelik kodundan daha karmaşık genişletmeleri yapımı etkinleştirin.  `attrref` öğeleri eşleşen öznitelik değerleri dizinden karşı izin verir.  `tag` öğeleri yorumlama semantiği belirtin ve yorumlama olasılık değiştirebilirsiniz.

```xml
<rule id="GetPapers">...</rule>
```

### <a name="example-element"></a>Örnek öğesi
İsteğe bağlı `example` öğesi belirttiğinden içeren tarafından kabul edilebilen örnek tümcecikleri `rule` tanımı.  Bu belge için kullanılabilir ve/veya otomatikleştirilmiş test.

```xml
<example>papers about machine learning by michael jordan</example>
```

### <a name="item-element"></a>öğe öğesi
`item` Öğesi grupları dilbilgisi yapıları dizisi.  Genişletme dizisi tekrarına belirtmek veya alternatifleri birlikte belirlemek için kullanılabilir `one-of` öğesi.

Zaman bir `item` öğesi bir alt öğesi değil bir `one-of` öğesi, ekteki dizisi yinelenmesinin atayarak belirtebilirsiniz `repeat` öznitelik için bir sayı değeri.  Bir sayı değeri "*n*" (burada *n* bir tamsayıdır) dizisi tam olarak gerçekleşmelidir gösterir *n* kez.  Bir sayı değeri "*m*-*n*" arasında görünmesi dizisi sağlar *m* ve *n* zaman, ikisi de dahil olmak.  Bir sayı değeri "*m*-" dizisi en az görünmelidir belirtir *m* kez.  İsteğe bağlı `repeat-logprob` özniteliği, en düşük ötesinde ek her yineleme yorumlama olasılık değiştirmek için kullanılabilir.

```xml
<item repeat="1-" repeat-logprob="-10">...</item>
```

Zaman `item` öğeleri alt olarak görünür bir `one-of` öğesi, genişletme alternatifleri kümesini tanımlar.  İsteğe bağlı bu kullanımı `logprob` özniteliği farklı seçenekler arasında göreceli günlük olasılığını belirtir.  Bir olasılık verilen *p* 0 ile 1 arasında karşılık gelen günlük olasılık günlük hesaplanabilir (*p*), log() olduğu doğal günlük işlevi.  Belirtilmezse, `logprob` yorumlama olasılık değiştirmez 0 varsayılanlara.  Günlük olasılık her zaman negatif bir kayan nokta değer veya 0 olduğuna dikkat edin.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="one-of-element"></a>bir-öğesi
`one-of` Öğesi belirttiğinden alt arasında alternatif genişletmeleri `item` öğeleri.  Yalnızca `item` öğeleri içinde görünebilir bir `one-of` öğesi.  Farklı seçenekler arasında göreceli olasılıklar aracılığıyla belirtilebilir `logprob` her alt özniteliğinde `item`.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="ruleref-element"></a>ruleref öğesi
`ruleref` Öğesi belirttiğinden diğerine başvurular aracılığıyla geçerli genişletmeleri `rule` öğesi.  Kullanım yoluyla `ruleref` öğeleri, daha karmaşık ifadeler daha basit kurallardan oluşturulabilir.  Gerekli `uri` öznitelik başvurulan adını gösterir `rule` sözdizimini kullanarak "#*rulename*".  Başvurulan kuralın anlamsal çıktısı yakalamak için isteğe bağlı kullanmak `name` özniteliği anlamsal çıkış atanmış bir değişken adını belirtin.
 
```xml
<ruleref uri="#GetPaperYear" name="year"/>
```

### <a name="attrref-element"></a>attrref öğesi
`attrref` Eşleşen öznitelik değerleri karşı gözlenen dizinde izin vererek bir dizin özniteliği öğesine başvuruyor.  Gerekli `uri` özniteliği, dizin şeması adı ve öznitelik adı sözdizimini kullanarak belirtir "*schemaName*#*attrName*".  Olmalıdır bir önceki `import` adlı şemayı içe aktaran öğesi *schemaName*.  Öznitelik adı karşılık gelen şemada tanımlanan bir özniteliği adıdır.

Kullanıcı girişi ile eşleşen yanı sıra `attrref` öğesi de bir yapılandırılmış sorgu nesnesini döndürür giriş değeri ile eşleşen dizini alt nesnelerinin seçer çıktı olarak.  İsteğe bağlı kullanmak `name` özniteliği burada sorgu nesne çıktısını saklanması gereken değişkeninin adını belirtin.  Daha fazla oluşturmak için diğer sorgu nesneleri sorgu nesnesi birleştirilebilen karmaşık ifadeler.  Bkz: [anlamsal yorumlama](SemanticInterpretation.md) Ayrıntılar için.  

```xml
<attrref uri="academic#Keyword" name="keyword"/>
```

#### <a name="query-completion"></a>Sorgu tamamlama 
Kısmi kullanıcı sorgularının yorumlanırken sorgu tamamlamalar desteklemek için başvurulan her özniteliği "starts_with" şema tanımı'ndaki bir işlem olarak eklemeniz gerekir.  Bir kullanıcı sorgu önek verilen `attrref` öneki tamamlamak dizindeki tüm değerlerin eşleşmesi ve tam her değer ayrı yorumu dilbilgisi verecek.  

Örnekler:
* Eşleşen `<attrref uri="academic#Keyword" name="keyword"/>` sorgusu önek "verilerinin" oluşturur yazıları "veritabanı" hakkında bir yorumu, "veri madenciliği", vb. hakkında raporlar için bir yorum.
* Eşleşen `<attrref uri="academic#Year" name="year"/>` bir yorumu bir yorumu yazıları "2001", vb. için "2000" yazıları için önek "200" sorgusu oluşturur.

#### <a name="matching-operations"></a>İşlem eşleştirme
Tam eşleşme ek olarak, select de destek öneki öznitelik türleri ve eşitsizlik eşleşen isteğe bağlı `op` özniteliği.  Hiçbir dizin nesnesinde eşleşen bir değeri varsa, dilbilgisi yolu engellenir ve hizmet bu dilbilgisi yolu üzerinden çapraz geçiş yapan yorumlar oluşturmaz.   `op` Varsayılanları "eq" özniteliği.

```xml
in <attrref uri="academic#Year" name="year"/>
before <attrref uri="academic#Year" op="lt" name="year"/
```

Aşağıdaki tabloda desteklenen listeler `op` her öznitelik türü için değerler.  Kullanımlarını şema öznitelik tanımı'nda dahil edilecek karşılık gelen dizin işlemi gerektirir.

| Öznitelik Türü | OP değeri | Açıklama | Dizin işlemi
|----|----|----|----|
| Dize | EQ | Dize tam eşleşme | şuna eşittir: |
| Dize | starts_with | Dize öneki eşleştirme | starts_with |
| Int32, Int64, çift | EQ |  Sayısal eşitlik eşleşmesi | şuna eşittir: |
| Int32, Int64, çift | lt, le, gt, ge | Sayısal eşitsizlik eşleşme (<, < =, >, > =) | is_between |
| Int32, Int64, çift | starts_with | Değerin ondalık gösterimde öneki eşleştirme | starts_with |

Örnekler:
* `<attrref uri="academic#Year" op="lt" name="year"/>` Giriş dizesi "2000" ile eşleşen ve yılın 2000'de, özel olarak yayımlanan tüm raporları döndürür.
* `<attrref uri="academic#Year" op="lt" name="year"/>` 20 yıl önce yayımlanan dizini yok yazıları olduğundan "20" giriş dizesi eşleşmiyor.
* `<attrref uri="academic#Keyword" op="starts_with" name="keyword"/>` Giriş dizesi "verilerinin" eşleşir ve "veritabanı", "veri madenciliği" vb. hakkında tek yorumlama yazıları döndürür.  Nadir kullanılan bir örnek budur.
* `<attrref uri="academic#Year" op="starts_with" name="year"/>` Giriş dizesi "20" ve tek yorumlama yazıları döndürür 200 299, 2000-2999, vb. yayımlanan eşleşir.  Nadir kullanılan bir örnek budur.

### <a name="tag-element"></a>Etiket öğesi
`tag` Öğesi belirttiğinden nasıl yorumlanacağını dilbilgisi bir yol değil.  Noktalı virgül ile sonlandırılmış deyimleri dizisi içerir.  Bir deyim bir hazır değer veya başka bir değişkene değişkeni ataması olabilir.  Bir değişkene 0 veya daha fazla parametre işleviyle çıktısını da atayabilir.  Her işlev parametresi, bir hazır değer veya değişkeni kullanılarak belirtilebilir.  İşlev herhangi bir çıktı döndürmezse atama atlanır.  Değişken kapsamı içeren kuralı yereldir.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Her `rule` dilbilgisi kuralın anlamsal çıktısı temsil eden "out" adlı önceden tanımlanmış bir değişken vardır.  Her yol üzerinden tarafından geçiş anlamsal deyimlerinin değerlendirerek değerini hesaplanan `rule` kullanıcı eşleştirme sorgu giriş.  Değerlendirme sonunda "out" değişkenine atanan değeri kuralın anlamsal çıktısı ' dir.  Bir kullanıcı sorgu dilbilgisi yorumlama anlamsal çıktı kök kuralın anlamsal çıktısı ' dir.

Bazı deyimleri yorumlama yolu olasılığını ek günlük olasılık uzaklığı sunarak değiştirebilir.  Bazı deyimleri yorumlama reddedebilir koşullar uyulmadığını belirtilmişse değerlerinin.

Desteklenen anlamsal işlevlerin listesi için bkz: [anlamsal işlevleri](SemanticInterpretation.md#semantic-functions).

## <a name="interpretation-probability"></a>Yorumu olasılık
Tüm toplu günlük olasılık dilbilgisi yorumlama yolundan olasılığı `<item>` öğeleri ve anlam işlevleri yol boyunca karşılaştı.  Belirli bir giriş sırası eşleşen göreli olasılığını açıklar.

Bir olasılık verilen *p* 0 ile 1 arasında karşılık gelen günlük olasılık günlük hesaplanabilir (*p*), log() doğal günlük işlevini olduğu.  Günlük olasılıklar kullanarak sistemin basit toplama yorumlama yolundan Eklem olasılığını birikmesini olanak tanır.  Kayan nokta yetersizliği gibi Eklem olasılık hesaplamaları ortak ortadan kaldırır.  Tasarım gereği, günlük olasılık her zaman negatif bir kayan nokta değer veya 0 ise, burada olasılığı daha büyük değerler belirtin olduğuna dikkat edin.

<a name="example"></a>
## <a name="example"></a>Örnek
Aşağıdaki XML dilbilgisi çeşitli öğelerin göstermektedir akademik yayınlar etki alanından örneğidir:

```xml
<grammar root="GetPapers">

  <!-- Import academic data schema-->
  <import schema="academic.schema" name="academic"/>
  
  <!-- Define root rule-->
  <rule id="GetPapers">
    <example>papers about machine learning by michael jordan</example>
    
    papers
    <tag>
      yearOnce = false;
      isBeyondEndOfQuery = false;
      query = All();
    </tag>
  
    <item repeat="1-" repeat-logprob="-10">
      <!-- Do not complete additional attributes beyond end of query -->
      <tag>AssertEquals(isBeyondEndOfQuery, false);</tag>
        
      <one-of>
        <!-- about <keyword> -->
        <item logprob="-0.5">
          about <attrref uri="academic#Keyword" name="keyword"/>
          <tag>query = And(query, keyword);</tag>
        </item>
        
        <!-- by <authorName> [while at <authorAffiliation>] -->
        <item logprob="-1">
          by <attrref uri="academic#Author.Name" name="authorName"/>
          <tag>authorQuery = authorName;</tag>
          <item repeat="0-1" repeat-logprob="-1.5">
            while at <attrref uri="academic#Author.Affiliation" name="authorAffiliation"/>
            <tag>authorQuery = And(authorQuery, authorAffiliation);</tag>
          </item>
          <tag>
            authorQuery = Composite(authorQuery);
            query = And(query, authorQuery);
          </tag>
        </item>
        
        <!-- written (in|before|after) <year> -->
        <item logprob="-1.5">
          <!-- Allow this grammar path to be traversed only once -->
          <tag>
            AssertEquals(yearOnce, false);
            yearOnce = true;
          </tag>
          <ruleref uri="#GetPaperYear" name="year"/>
          <tag>query = And(query, year);</tag>
        </item>
      </one-of>

      <!-- Determine if current parse position is beyond end of query -->
      <tag>isBeyondEndOfQuery = GetVariable("IsBeyondEndOfQuery", "system");</tag>
    </item>
    <tag>out = query;</tag>
  </rule>
  
  <rule id="GetPaperYear">
    <tag>year = All();</tag>
    written
    <one-of>
      <item>
        in <attrref uri="academic#Year" name="year"/>
      </item>
      <item>
        before
        <one-of>
          <item>[year]</item>
          <item><attrref uri="academic#Year" op="lt" name="year"/></item>
        </one-of>
      </item>
      <item>
        after
        <one-of>
          <item>[year]</item>
          <item><attrref uri="academic#Year" op="gt" name="year"/></item>
        </one-of>
      </item>
    </one-of>
    <tag>out = year;</tag>
  </rule>
</grammar>
```
