---
title: Dil bilgisi biçimi - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Dil bilgisi biçimi, bilgi keşfetme hizmeti (KES) API hakkında daha fazla bilgi edinin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 844bd9a88c52fd398fc66c71e59da513c0d7d90d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60814865"
---
# <a name="grammar-format"></a>Dil bilgisi biçimi

Dil bilgisi şu doğal dil sorguları anlam sorgusu ifadelere nasıl dönüştürüleceğini yanı sıra hizmet yorumlayabilir doğal dil sorguları ağırlıklı kümesini belirten bir XML dosyasıdır.  Dilbilgisi söz dizimi dayanır [SRGS](https://www.w3.org/TR/speech-grammar/), veri dizin tümleştirmesi ve anlam işlevleri desteklemek için uzantılara sahip konuşma tanıma dilbilgisi için W3C standart.

Dilbilgisi içinde kullanılabilen söz dizimsel öğelerin her biri aşağıda açıklanmıştır.  Bkz: [Bu örnek](#example) bağlamında bu öğeleri kullanımını gösteren tam bir dil bilgisi için.

### <a name="grammar-element"></a>Dilbilgisi öğesi

`grammar` Öğesidir üst düzey XML dilbilgisi belirtimi.  Gerekli `root` özniteliği dilbilgisi başlangıç noktasını tanımlayan kök kuralının adını belirtir.

```xml
<grammar root="GetPapers">
```

### <a name="import-element"></a>İçeri aktarma öğesi

`import` Öğesi bir şema tanımı özniteliği başvuruları etkinleştirmek için bir dış dosyasından içeri aktarır. Üst düzey bir alt öğesi olmalıdır `grammar` öğesi önce görünür `attrref` öğeleri. Gerekli `schema` öznitelik dilbilgisi XML dosyasıyla aynı dizinde bulunan bir şema dosyası adını belirtir. Gerekli `name` şemanın diğer adı, sonraki öğeyi belirten `attrref` öğeleri, bu şema içinde tanımlanan öznitelikleri başvururken kullanın.

```xml
  <import schema="academic.schema" name="academic"/>
```

### <a name="rule-element"></a>Kural öğesi

`rule` Öğesi, bir dil bilgisi kuralı bir sistem yorumlayabilir sorgu ifadeleri belirtir bir Yapısal birim tanımlar.  Üst düzey bir alt öğesi olmalıdır `grammar` öğesi.  Gerekli `id` özniteliği içinden başvuruda kuralının adını belirtir `grammar` veya `ruleref` öğeleri.

A `rule` öğe yasal genişletmeleri kümesini tanımlar.  Metin belirteçlerini doğrudan giriş sorgusu eşleştirin.  `item` öğeleri yineler belirtin ve yorumu olasılıklar alter.  `one-of` öğeleri diğer seçenekleri belirtin.  `ruleref` öğeleri daha basit olanları öğesinden daha karmaşık genişletmeleri oluşumunu etkinleştirin.  `attrref` öğeleri, öznitelik değerleri dizinden karşı eşleşme izin verir.  `tag` öğeleri yorumu semantiği belirtin ve yorumu olasılık değiştirebilirsiniz.

```xml
<rule id="GetPapers">...</rule>
```

### <a name="example-element"></a>Örnek öğesi

İsteğe bağlı `example` öğesi belirtir içeren tarafından kabul edilebilen örnek ifadeleri `rule` tanımı.  Bu belge için kullanılabilir ve/veya otomatik test.

```xml
<example>papers about machine learning by michael jordan</example>
```

### <a name="item-element"></a>öğe öğesi

`item` Dilbilgisi yapıları bir dizi öğe gruplandırır.  Genişletme dizisi tekrarlanıyor belirtmek için ya da birlikte alternatifleri belirlemek için kullanılabilir `one-of` öğesi.

Olduğunda bir `item` öğesi alt öğesi değil bir `one-of` öğesi kapalı dizisi tekrarını atayarak belirtebilirsiniz `repeat` özniteliği için bir sayı değeri.  Sayısı değerini "*n*" (burada *n* bir tamsayıdır) dizisi tam olarak gerçekleşmesi gerektiğini belirtir *n* kez.  Sayısı değerini "*m*-*n*" dizisi arasında görünmesini sağlar *m* ve *n* zaman aralığında.  Sayısı değerini "*m*-" dizisi en az olması gerektiğini belirtir *m* kez.  İsteğe bağlı `repeat-logprob` özniteliği, yorumu olasılığını en düşük ötesinde ek her yineleme için alter için kullanılabilir.

```xml
<item repeat="1-" repeat-logprob="-10">...</item>
```

Zaman `item` öğeleri alt öğeleri görünür bir `one-of` öğesi genişletmesi seçenekleri kümesi tanımlarlar.  İsteğe bağlı bu kullanımı `logprob` özniteliği farklı seçenekler arasında göreli günlük olasılığını belirtir.  Verilen bir olasılık *p* karşılık gelen günlük olasılık 0 ile 1 arasında günlük hesaplanabilir (*p*), doğal logaritmayı işlevi log() olduğu.  Belirtilmezse, `logprob` yorumu olasılık değiştirmez 0, varsayılan değeri.  Günlük olasılık her zaman negatif bir kayan nokta değer veya 0 olduğuna dikkat edin.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="one-of-element"></a>bir-öğe

`one-of` Öğesi alt arasında alternatif genişletmeleri belirtir `item` öğeleri.  Yalnızca `item` öğe içindeki görünebilir bir `one-of` öğesi.  Aracılığıyla farklı seçenekler arasında göreli olasılıklar belirtilebilir `logprob` her alt özniteliğinde `item`.

```xml
<one-of>
  <item>by</item>
  <item logprob="-0.5">written by</item>
  <item logprob="-1">authored by</item>
</one-of>
```

### <a name="ruleref-element"></a>ruleref öğesi

`ruleref` Öğesi geçerli genişletmeleri aracılığıyla başka bir başvuru belirtir `rule` öğesi.  Kullanımının `ruleref` öğeleri, daha karmaşık ifadeler daha basit kuralları oluşturulabilir.  Gerekli `uri` özniteliği başvurulan adını gösterir `rule` söz dizimi kullanarak "#*rulename*".  Başvurulan kural anlam çıktısını yakalamak için isteğe bağlı kullanın `name` anlam çıkış atandığı değişkenin adı belirtmek için özniteliği.
 
```xml
<ruleref uri="#GetPaperYear" name="year"/>
```

### <a name="attrref-element"></a>attrref öğesi

`attrref` Eşleşen öznitelik değerleri karşı gözlemlenen dizinde izin vererek bir dizin özniteliği öğeye başvuruyor.  Gerekli `uri` özniteliği, dizin şema adı ve öznitelik adı sözdizimini kullanarak belirtir "*schemaName*#*attrName*".  Olmalıdır bir önceki `import` adlı şemanın Imports öğesi *schemaName*.  Öznitelik adı karşılık gelen şemasında tanımlanan öznitelik adıdır.

Kullanıcı girişi ile eşleşen yanı sıra `attrref` öğe ayrıca bir yapılandırılmış sorgu nesnesi döndürür giriş değeri ile eşleşen dizin nesneleri kümesini seçen bir çıkış olarak.  İsteğe bağlı `name` sorgu nesnesi çıkış burada depolanmalıdır değişkeninin adını belirtmek için özniteliği.  Sorgu nesnesi diğer sorgu nesneleri ile daha fazla bilgi formu kullanılıp kullanılamayacağı karmaşık ifadeler.  Bkz: [anlam yorumlama](SemanticInterpretation.md) Ayrıntılar için.  

```xml
<attrref uri="academic#Keyword" name="keyword"/>
```

#### <a name="query-completion"></a>Sorgu tamamlama

Kısmi bir kullanıcı sorguları yorumlarken sorgu tamamlamaları desteklemek için her başvurulan öznitelik "starts_with" şema tanımı bir işlem olarak içermelidir.  Bir kullanıcı sorgu önek verilen `attrref` önek tamamlamak dizindeki tüm değerlerin eşleşmesi ve tam her değer ayrı bir yorumu dilbilgisi yield.  

Örnekler:
* Eşleşen `<attrref uri="academic#Keyword" name="keyword"/>` karşı sorgu "dat" ön eki oluşturur bir yorumu "veritabanı" hakkında daha fazla inceleme için "veri madenciliği", vb. hakkında daha fazla inceleme için bir yorumu.
* Eşleşen `<attrref uri="academic#Year" name="year"/>` karşı sorgu bir yorumu incelemeler "2000", "2001" incelemeler, vb. bir yorumu "200" ön eki oluşturur.

#### <a name="matching-operations"></a>Eşleşen işlemler

Tam eşleşme ek olarak, select de destek öneki öznitelik türleri ve eşitsizlik eşleşen isteğe bağlı `op` özniteliği.  Hiçbir dizin nesnesinde eşleşen bir değeri varsa, dil bilgisi yolu engellenir ve hizmet bu dilbilgisi yolu geçme herhangi bir yorum oluşturmaz.   `op` Varsayılanları "için eq" özniteliği.

```xml
in <attrref uri="academic#Year" name="year"/>
before <attrref uri="academic#Year" op="lt" name="year"/
```

Aşağıdaki tablo desteklenen listeler `op` her bir öznitelik türü için değer.  Kullanımları şema öznitelik tanımını dahil edilecek karşılık gelen dizin işlemi gerektirir.

| Öznitelik türü | OP değeri | Açıklama | Dizin işlemi
|----|----|----|----|
| String | EQ | Dize tam eşleşme | eşittir |
| String | starts_with | Dize ön ek eşleştirmesi | starts_with |
| Int32, Int64, çift | EQ |  Sayısal eşitlik eşleştir | eşittir |
| Int32, Int64, çift | lt, le, gt, ge | Sayısal eşitsizlik eşleştir (<, < =, >, > =) | is_between |
| Int32, Int64, çift | starts_with | Değer ondalık gösteriminde ön ek eşleştirmesi | starts_with |

Örnekler:
* `<attrref uri="academic#Year" op="lt" name="year"/>` "2000" giriş dizesi ile eşleştiğini ve 2000, yıl önce özel olarak yayımlanan tüm raporları döndürür.
* `<attrref uri="academic#Year" op="lt" name="year"/>` 20 yıldan önce yayımlanan dizini yok incelemeler olduğundan, Giriş dizesinin "20" eşleşmiyor.
* `<attrref uri="academic#Keyword" op="starts_with" name="keyword"/>` "dat" giriş dizesi ile eşleştiğini ve "veritabanı", "veri madenciliği" hakkında bir yorumu tek incelemeler döndürür.  Nadir kullanım durumu budur.
* `<attrref uri="academic#Year" op="starts_with" name="year"/>` tek bir yorumu incelemeler döndürür ve Giriş dizesinin "20" 200-299, 2000-2999, vb. yayımlanan eşleşir.  Nadir kullanım durumu budur.

### <a name="tag-element"></a>Etiket öğesi

`tag` Öğesi nasıl yorumlanacağını dilbilgisi bir yol olduğunu belirtir.  Noktalı virgül ile sonlandırılmış ifadeler içeriyor.  Bir ifade bir sabit değer ya da başka bir değişkene bir değişken ataması olabilir.  Bir değişkene, 0 veya daha fazla parametre ile bir işlevin çıktısı de atayabilir.  Her işlev parametresi, bir sabit değer veya bir değişkeni kullanılarak belirtilebilir.  İşlev herhangi bir çıktı döndürmezse ataması yoksayılır.  Değişken kapsamı kuralını için yereldir.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Her `rule` dilbilgisi kuralın anlam çıktıyı temsil eden "dışarı" olmak adlı önceden tanımlanmış bir değişkene sahip.  Değeri her yolu geçiş anlam deyimlerinin değerlendirilerek hesaplanan `rule` kullanıcı eşleşen sorgu girişi.  Değerlendirme sonuna "dışarı" değişkenine atanan değeri, kuralın anlam çıktıdır.  Bir kullanıcı sorgu dil bilgisi yorumlanırken anlam çıktı kök kuralın anlam çıkış alınır.

Bazı deyimleri, ek günlük olasılık uzaklık sunarak bir yorumu yolu olasılığını değiştirebilir.  Bazı deyimleri yorumu reddedebilir koşullar karşılanmadı belirtilmişse toptan.

Desteklenen anlam işlevlerin bir listesi için bkz. [anlam işlevleri](SemanticInterpretation.md#semantic-functions).

## <a name="interpretation-probability"></a>Yorumu olasılık

Tüm toplu günlük olasılığı dil bilgisi aracılığıyla bir yorumu yolu olasılığı `<item>` öğeleri ve anlam işlevleri süreç boyunca karşılaşıldı.  Bu, belirli bir giriş sırası eşleşen göreli olasılığını açıklar.

Verilen bir olasılık *p* karşılık gelen günlük olasılık 0 ile 1 arasında günlük hesaplanabilir (*p*), doğal logaritmayı işlevi log() olduğu.  Günlük olasılıklar kullanarak basit bir yorumu yolundan Eklem olasılığını ulaşıncaya kadar sistem sağlar.  Ayrıca, kayan nokta yetersizliği gibi ek olasılık hesaplamaları için ortak önler.  Tasarım gereği, günlük olasılık her zaman negatif bir kayan nokta değer veya 0 ise, burada olasılığı daha büyük değerler belirtin olduğuna dikkat edin.

## <a name="example"></a>Örnek

XML'bir dil bilgisi çeşitli öğelerini gösteren akademik yayınlar etki alanından bir örnek verilmiştir:

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
