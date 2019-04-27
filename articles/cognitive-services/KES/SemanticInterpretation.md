---
title: Anlam yorumlama - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Bilgi keşfetme hizmeti (KES içinde) API anlam yorumlama kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 26f8d885f8cf85ab849ba221392df206e492aac4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60814477"
---
# <a name="semantic-interpretation"></a>Anlam yorumlama

Anlam yorumlama dilbilgisi yorumlanan her yolundan anlam çıkış ilişkilendirir.  Özellikle, bir dizi deyimlerinde hizmet tarafından değerlendirilir `tag` öğeleri geçiş son çıktı işlem tarafından yorumu.  

Bir ifade bir sabit değer ya da başka bir değişkene bir değişken ataması olabilir.  Bir değişkene, 0 veya daha fazla parametre ile bir işlevin çıktısı de atayabilir.  Her işlev parametresi, bir sabit değer veya bir değişkeni kullanılarak belirtilebilir.  İşlev herhangi bir çıktı döndürmezse ataması yoksayılır.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Bir değişken, bir harfle başlar ve yalnızca harfler (A-Z), sayılar (0-9) ve alt çizgi oluşan bir ad tanımlayıcısı kullanılarak belirtilen (\_).  Türü örtük olarak değişmez değer algılanır veya işlevi için atanan değer çıktı. 

Şu anda desteklenen veri türlerinin bir listesi aşağıdadır:

|Tür|Açıklama|Örnekler|
|----|----|----|
|String|0 veya daha fazla karakter dizisi|"Hello World!"<br/>""|
|Bool|Boole değeri|true<br/>false|
|Int32|32 bitlik işaretli tamsayı.  -2.1e9 için 2.1e9|123<br/>-321|
|Int64|64-bit işaretli tamsayı. -9.2e18 ve 9.2e18|9876543210|
|Double|Çift duyarlıklı kayan nokta. 1.7E +/-308 (15 basamak)|123.456789<br/>1.23456789e2|
|Guid|Genel benzersiz tanıtıcısı|"602DD052-CC47-4B23-A16A-26B52D30C05B"|
|Sorgu|Veri nesneleri bir alt dizinde belirten sorgu ifadesi|All()<br/>Ve (*S1*, *S2*)|

## <a name="semantic-functions"></a>Anlam işlevleri

Yerleşik bir anlam işlevler kümesi yok.  Bunlar karmaşık sorgular oluşumu izin ve bağlama duyarlı denetime dilbilgisi yorumlar sağlayın.

### <a name="and-function"></a>Ve işlevi

`query = And(query1, query2);`

İki giriş sorguları kesişimi oluşan bir sorgu döndürür.

### <a name="or-function"></a>Veya işlevi

`query = Or(query1, query2);`

İki giriş sorguları unıon'dan oluşan bir sorgu döndürür.

### <a name="all-function"></a>Tüm işlevi

`query = All();`

Tüm veri nesneleri içeren bir sorgu döndürür.

Aşağıdaki örnekte, 1 veya daha fazla anahtar sözcükleri kesişimi alarak sorguyu ayarlama yinelemeli olarak oluşturulacak All() işlevi kullanın.

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

Aşağıdaki örnekte, 1 veya daha fazla anahtar sözcükleri birleşimini alarak sorguyu ayarlama yinelemeli olarak oluşturulacak None() işlevi kullanın.

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

Yalnızca veri nesneleri içeren bir sorgu, öznitelik döndürür *attrName* değeri ile eşleşen *değer* belirtilen işlem göre *op*, "eq için" varsayılan olarak.  Genellikle, bir `attrref` eşleşen giriş sorgu dizesine göre bir sorgu oluşturmak için öğesi.  Bir değer verilen veya başka yollarla elde Query() işlevi bu değeri ile eşleşen bir sorgu oluşturmak için kullanılabilir.

Aşağıdaki örnekte, belirli bir on akademik yayınları belirtme desteği uygulamak için Query() işlevi kullanın.

```xml
written in the 90s
<tag>
  beginYear = Query("academic#Year", 1990, "ge");
  endYear = Query("academic#Year", 2000, "lt");
  year = And(beginYear, endYear);
</tag>
```

### <a name="composite-function"></a>Bileşik işlevi

`query = Composite(innerQuery);`

Kapsülleyen bir sorgunun döndürdüğü bir *innerQuery* eşleşen ortak bir bileşik özniteliği alt özniteliklerini karşı oluşan *attr*.  Bileşik öznitelik kapsüllemesi gerekiyor *attr* ayrı ayrı karşılayan en az bir değere sahip olması için tüm eşleşen veri nesnesinin *innerQuery*.  Bir sorgu bileşik bir öznitelik alt özniteliklerini diğer sorgular ile birleştirilebilir önce Composite() işlevi kullanarak kapsüllenmiş olduğuna dikkat edin.

Örneğin, "microsoft" he etkinken aşağıdaki sorguyu göre akademik yayınlar "harry shum" döndürür:
```
Composite(And(Query("academic#Author.Name", "harry shum"), 
              Query("academic#Author.Affiliation", "microsoft")));
```

Öte yandan, aşağıdaki sorgu, akademik yayınlar burada yazarlar, "harry shum" ve "microsoft" ise ilişkileri birini döndürür:
```
And(Composite(Query("academic#Author.Name", "harry shum"), 
    Composite(Query("academic#Author.Affiliation", "microsoft")));
```

### <a name="getvariable-function"></a>GetVariable işlevi

`value = GetVariable(name, scope);`

Değişkenin değerini döndürür *adı* altında belirtilen tanımlanan *kapsam*.  *adı* bir harf ile başlayan ve yalnızca harfler (A-Z), sayılar (0-9) ve alt çizgi (_) oluşan bir tanımlayıcıdır.  *Kapsam* "istek" veya "Sistem" olarak ayarlayabilirsiniz.  Farklı kapsamlarda altında tanımlanan değişkenler birbirinden olanları anlam işlevler çıkışını tanımlanan dahil olmak üzere farklı olduğunu unutmayın.

İstek kapsam değişkenleri geçerli yorumlama istek içinde tüm yorumlar arasında paylaşılır.  Yorumlar için arama dilbilgisi denetlemek için kullanılabilir.

Sistem değişkenleri, hizmet tarafından önceden tanımlanan ve çeşitli İstatistikler sistemin geçerli durumunu almak için kullanılabilir.  Şu anda desteklenen sistem değişkenleri kümesi aşağıdadır:

|Ad|Tür|Açıklama|
|----|----|----|
|IsAtEndOfQuery|Bool|Tüm giriş sorgu metni geçerli yorumu eşleşti true|
|IsBeyondEndOfQuery|Bool|Geçerli yorumu giriş sorgu metni ötesinde tamamlamaları önerdiği true|

### <a name="setvariable-function"></a>SetVariable işlevi

`SetVariable(name, value, scope);`

Atar *değer* değişkene *adı* belirtilen adla *kapsam*.  *adı* bir harf ile başlayan ve yalnızca harfler (A-Z), sayılar (0-9) ve alt çizgi (_) oluşan bir tanımlayıcıdır.  Şu anda, yalnızca geçerli değeri *kapsam* "request" olan.  Hiçbir ayarlanabilir sistem değişkenleri vardır.

İstek kapsam değişkenleri geçerli yorumlama istek içinde tüm yorumlar arasında paylaşılır.  Yorumlar için arama dilbilgisi denetlemek için kullanılabilir.

### <a name="assertequals-function"></a>AssertEquals işlevi

`AssertEquals(value1, value2);`

Varsa *value1* ve *value2* işlevi başarılı olur ve yan etkileri olan eşdeğer olan.  Aksi halde, işlev başarısız olur ve yorumu reddeder.

### <a name="assertnotequals-function"></a>AssertNotEquals işlevi

`AssertNotEquals(value1, value2);`

Varsa *value1* ve *value2* eşdeğerdir işlevi başarılı olur ve yan etkileri olan değil,.  Aksi halde, işlev başarısız olur ve yorumu reddeder.


