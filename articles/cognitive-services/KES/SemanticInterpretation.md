---
title: Bilgi Bankası araştırması hizmeti API'si anlamsal yorumu | Microsoft Docs
description: İçinde bilgi araştırması hizmet (KES) API Bilişsel Services anlam yorumlama kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 022188464eb7269b69f96a058b444167b587387c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351809"
---
# <a name="semantic-interpretation"></a>Anlam yorumlama
Anlam yorumlama dilbilgisi yorumlanan her yolundan anlamsal çıkış ilişkilendirir.  Özellikle, hizmet deyimlerinde dizisini değerlendirir `tag` öğeleri yorumlama son çıkışı işlem tarafından geçiş.  

Bir deyim bir hazır değer veya başka bir değişkene değişkeni ataması olabilir.  Bir değişkene 0 veya daha fazla parametre işleviyle çıktısını da atayabilir.  Her işlev parametresi, bir hazır değer veya değişkeni kullanılarak belirtilebilir.  İşlev herhangi bir çıktı döndürmezse atama atlanır.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Bir değişkenin bir harfle başlayan ve yalnızca harf (A-Z), sayılar (0-9) ve alt çizgi oluşan bir ad tanımlayıcısı kullanılarak belirtilir (\_).  Türü örtük olarak sabit olayla veya işlevi çıktı atanmış değeri. 

Şu anda desteklenen veri türleri listesi aşağıdadır:

|Tür|Açıklama|Örnekler|
|----|----|----|
|Dize|0 veya daha çok karakter dizisi|"Hello World!"<br/>""|
|bool|Boole değeri|true<br/>false|
|Int32|32 bit işaretli tamsayıyı.  -2.1e9 için 2.1e9|123<br/>-321|
|Int64|64 bit işaretli tamsayıyı. -9.2e18 ve 9.2e18|9876543210|
|çift|Çift duyarlıklı kayan nokta. 1.7E +/-308 (15 basamak)|123.456789<br/>1.23456789e2|
|Guid|Genel benzersiz tanımlayıcı|"602DD052-CC47-4B23-A16A-26B52D30C05B"|
|Sorgu|Veri nesneleri bir alt dizinde belirtir sorgu ifadesi|All()<br/>Ve (*S1*, *S2*)|

<a name="semantic-functions"></a>
## <a name="semantic-functions"></a>Anlam işlevleri
Yerleşik bir anlam işlevler kümesi yok.  Bunlar karmaşık sorgular yapımı izin ve dilbilgisi yorumlar üzerinde bağlama duyarlı denetim sağlar.

### <a name="and-function"></a>Ve işlevi
`query = And(query1, query2);`

İki giriş sorguları kesişimi oluşan bir sorgu döndürür.

### <a name="or-function"></a>Ya da işlevi
`query = Or(query1, query2);`

İki giriş sorguları birleşimi oluşan bir sorgu döndürür.

### <a name="all-function"></a>Tüm işlevi
`query = All();`

Tüm veri nesneleri içeren bir sorgu döndürür.

Aşağıdaki örnekte, 1 veya daha fazla anahtar sözcükleri kesişimi dayalı bir sorgu yukarı tekrarlayarak oluşturmak için All() işlevini kullanın.

```
<tag>query = All();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = And(query, keyword);</tag>
</item>
```

### <a name="none-function"></a>Hiçbiri işlevi
`query = None();`

Hiçbir veri nesneleri içeren bir sorgu döndürür.

Aşağıdaki örnekte, 1 veya daha fazla anahtar sözcüğü birleşimi dayalı bir sorgu yukarı tekrarlayarak oluşturmak için None() işlevini kullanın.

```
<tag>query = None();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = Or(query, keyword);</tag>
</item>
```

### <a name="query-function"></a>Sorgu işlevi
```
query = Query(attrName, value)
query = Query(attrName, value, op)
```

Yalnızca veri nesneleri içeren bir sorgu, öznitelik döndürür *attrName* eşleşen değeri *değeri* belirtilen işlem göre *op*, "eq" varsayılan olarak.  Genellikle, bir `attrref` öğesi eşleşen giriş sorgu dizesine dayalı bir sorgu oluşturun.  Bir değer verilen veya diğer yollarla elde bulunuyorsa, Query() işlevi bu değerle eşleşen bir sorgu oluşturmak için kullanılabilir.

Aşağıdaki örnekte, belirli bir on akademik yayınlardan belirtmek için destek uygulamak için Query() işlevini kullanın.

```xml
written in the 90s
<tag>
  beginYear = Query("academic#Year", 1990, "ge");
  endYear = Query("academic#Year", 2000, "lt");
  year = And(beginYear, endYear);
</tag>
```

<a name="composite-function"/>
### <a name="composite-function"></a>Bileşik işlevi
`query = Composite(innerQuery);`

Yalıtan bir sorgunun döndürdüğü bir *innerQuery* eşleşen bir ortak bileşik özniteliği alt özniteliklerini karşı oluşan *attr*.  Bileşik öznitelik kapsülleme gerektirir *attr* herhangi eşleşen veri nesnesinin tek tek karşılayan en az bir değere sahip *innerQuery*.  Bileşik bir öznitelik alt özniteliklerini sorguya diğer sorgular ile birleştirilebilir önce Composite() işlevini kullanarak saklanmasını olduğuna dikkat edin.

Örneğin, kendisinin "microsoft" ederken aşağıdaki sorguyu göre akademik yayınlar "shum harry" döndürür:
```
Composite(And(Query("academic#Author.Name", "harry shum"), 
              Query("academic#Author.Affiliation", "microsoft")));
```

Öte yandan, aşağıdaki sorguyu yazarlar birini olduğu akademik yayınlar "shum harry" ve "microsoft" üyelikler, biri döndürür:
```
And(Composite(Query("academic#Author.Name", "harry shum"), 
    Composite(Query("academic#Author.Affiliation", "microsoft")));
```

### <a name="getvariable-function"></a>GetVariable işlevi
`value = GetVariable(name, scope);`

Değişkenin değerini döndürür *adı* belirtilen altında tanımlanan *kapsam*.  *ad* bir harfle başlayan ve yalnızca harf (A-Z), sayılar (0-9) ve alt çizgi (_) oluşan bir tanımlayıcıdır.  *Kapsam* "talep" veya "Sistem" olarak ayarlayabilirsiniz.  Farklı kapsamları tanımlanan değişkenler birbirinden olanları anlamsal işlevleri çıkış tanımlanan dahil olmak üzere ayrı olduğunu unutmayın.

İstek kapsamı değişkenleri geçerli yorumlama istek içinde tüm yorumlar arasında paylaşılır.  Yorumlar için arama dilbilgisi denetlemek için kullanılabilir.

Sistem değişkenleri hizmeti tarafından önceden tanımlanmış ve sistem geçerli durumuyla ilgili çeşitli istatistikleri almak için kullanılır.  Şu anda desteklenen sistem değişkenleri kümesinin aşağıdadır:

|Ad|Tür|Açıklama|
|----|----|----|
|IsAtEndOfQuery|bool|Geçerli yorumlama tüm giriş sorgu metni eşleşti true|
|IsBeyondEndOfQuery|bool|Geçerli yorumlama giriş sorgu metnini ötesinde tamamlamalar önerdiği true|

### <a name="setvariable-function"></a>SetVariable işlevi
`SetVariable(name, value, scope);`

Atar *değeri* değişkenine *adı* belirtilen adla *kapsam*.  *ad* bir harfle başlayan ve yalnızca harf (A-Z), sayılar (0-9) ve alt çizgi (_) oluşan bir tanımlayıcıdır.  Şu an için tek geçerli değer *kapsam* "istek" değil.  Hiçbir ayarlanabilir sistem değişkenleri vardır.

İstek kapsamı değişkenleri geçerli yorumlama istek içinde tüm yorumlar arasında paylaşılır.  Yorumlar için arama dilbilgisi denetlemek için kullanılabilir.

### <a name="assertequals-function"></a>AssertEquals işlevi
`AssertEquals(value1, value2);`

Varsa *value1* ve *value2* eşdeğerdir işlevi başarılı ve hiçbir yan etkisi vardır,.  Aksi takdirde, işlev başarısız olur ve yorumlama reddeder.

### <a name="assertnotequals-function"></a>AssertNotEquals işlevi
`AssertNotEquals(value1, value2);`

Varsa *value1* ve *value2* eşdeğerdir işlevi başarılı ve hiçbir yan etkisi sahip değil,.  Aksi takdirde, işlev başarısız olur ve yorumlama reddeder.


