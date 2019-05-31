---
title: Azure Cosmos DB için SQL alt sorgular
description: SQL alt sorgular ve Azure Cosmos DB'de, genel kullanım örnekleri hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: tisande
ms.openlocfilehash: 68465f4ca195930ce08bb579e68d0227e9c18dd6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242853"
---
# <a name="sql-subquery-examples-for-azure-cosmos-db"></a>Azure Cosmos DB için SQL alt sorgu örnekleri

Alt sorgu başka bir sorgu içinde yuvalanmış bir sorgudur. Alt sorgu, bir iç sorgu veya iç seçin olarak da bilinir. Alt sorgu içeren ifade genellikle dış bir sorgu olarak adlandırılır.

Bu makalede, SQL alt sorgular ve Azure Cosmos DB'de, ortak kullanım durumları açıklanmaktadır.

## <a name="types-of-subqueries"></a>Alt sorgular türleri

Alt sorgular iki ana türü vardır:

* **Bağıntılı**: Dış sorgudan değerler başvuran alt sorgu. Alt sorgu, dış sorgu işlemleri her satır için bir kez değerlendirilir.
* **Bağıntılı olmayan**: Dış sorgu bağımsız bir alt sorgu. Dış üzerinde bağlı olmadan kendi sorgu üzerinde çalıştırılabilir.

> [!NOTE]
> Azure Cosmos DB bağıntılı alt sorgular destekler.

Daha fazla alt sorgularda, döndürmeleri satır ve sütun sayısına göre sınıflandırılabilir. Üç tür vardır:
* **Tablo**: Birden çok satır ve birden çok sütun döndürür.
* **Birden çok değerli**: Birden çok satır ve tek bir sütun döndürür.
* **Skaler**: Tek bir satır ve tek bir sütun döndürür.

Azure Cosmos DB'de SQL sorguları her zaman tek bir sütun (basit bir değer veya karmaşık bir belgeyi) döndürür. Bu nedenle, yalnızca birden çok değerli ve skaler alt sorgular, Azure Cosmos DB içinde geçerlidir. FROM yan tümcesinde yalnızca bir çok değer alt sorgusu ilişkisel bir ifade olarak kullanabilirsiniz. Skaler bir alt sorguda bir skaler ifade seçin veya WHERE yan tümcesi veya FROM yan tümcesindeki ilişkisel bir ifade olarak kullanabilirsiniz.


## <a name="multi-value-subqueries"></a>Birden çok değerli alt sorgular

Birden çok değerli alt sorgularda, bir belge kümesi dönün ve FROM yan tümcesi içinde her zaman kullanılır. İçin kullanıldıklarından:

* Birleştirme ifadeleri en iyi duruma getirme. 
* Pahalı ifadeleri kez değerlendirme ve birden çok kez başvuruyor.

### <a name="optimize-join-expressions"></a>Birleştirme ifadeleri en iyi duruma getirme

Birden çok değerli alt sorgular, birleştirme ifadeleri her seçin-çok ifade sonra yerine WHERE yan tümcesindeki tüm arası birleştirmeler sonra doğrulamaları ileterek iyileştirebilirsiniz.

Şu sorguyu inceleyin:

```sql
SELECT Count(1) AS Count
FROM c
JOIN t IN c.tags
JOIN n IN c.nutrients
JOIN s IN c.servings
WHERE t.name = 'infant formula' AND (n.nutritionValue > 0 
AND n.nutritionValue < 10) AND s.amount > 1
```

Bu sorgu için dizin adı "çocuk formül." içeren bir etiket var olan herhangi bir belge eşleşir Bu bir nutrient öğesi 0 ile 10 arasında bir değer ile ve 1'den büyük bir miktarını sunma öğesiyle olur. Herhangi bir filtre uygulanmadan önce birleştirme ifadesi burada etiketler ve nutrients servis dizilerinin tüm öğelerin çapraz ürün eşleşen her belge için gerçekleştirin. 

WHERE yan tümcesi, ardından filtre koşulu her < c, t, n, s > demet uygulanır. Örneği için eşleşen bir belgeyi her üç diziden oluşan 10 öğe varsa, 1 x 10 x 10 x 10'a (diğer bir deyişle, 1000) genişletecek tanımlama grubu. Burada alt sorgular kullanarak sonraki ifade ile birleştirilmeden önce birleştirilmiş bir dizi öğeleri filtreleyerek de yardımcı olabilir.

Bu sorgu için bir eşdeğerdir, ancak alt sorgular kullanır:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

Yalnızca bir öğe etiketleri dizideki eşleşen filtre ve nutrients hem servis diziler için beş öğe varsayılır. Birleştirme ifadeleri ardından 1 x 1 x 5 x 5 = 25 olarak genişletilecektir öğeli ilk sorgudaki 1000 öğe.

### <a name="evaluate-once-and-reference-many-times"></a>Bir kez ve başvurusu birden çok kez değerlendir

Alt sorgular, kullanıcı tanımlı işlevlerle (UDF), karmaşık dizeler veya aritmetik ifadeler gibi pahalı ifadeler içeren sorguların iyileştirilmesine yardımcı olabilir. İfade bir kez ancak birden çok kez başvurmak için bir alt sorguda birleştirme ifadesi ile birlikte kullanabilirsiniz.

Aşağıdaki sorgu UDF çalışır `GetMaxNutritionValue` iki kez:

```sql
SELECT c.id, udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue
FROM c
WHERE udf.GetMaxNutritionValue(c.nutrients) > 100
```

UDF yalnızca bir kez çalışır eşdeğer bir sorgu aşağıda verilmiştir:

```sql
SELECT TOP 1000 c.id, MaxNutritionValue
FROM c
JOIN (SELECT VALUE udf.GetMaxNutritionValue(c.nutrients)) MaxNutritionValue
WHERE MaxNutritionValue > 100
``` 

> [!NOTE] 
> Birleştirme ifadeleri çapraz ürün davranışı göz önünde bulundurun. UDF ifade için tanımlanmamış değerlendirebilirsiniz, bir nesne alt sorgu değeri yerine doğrudan döndürerek birleştirme ifade her zaman tek bir satır üretir emin olmanız gerekir.
>

Bir değer yerine bir nesne döndürür benzer bir örnek aşağıda verilmiştir:

```sql
SELECT TOP 1000 c.id, m.MaxNutritionValue
FROM c
JOIN (SELECT udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue) m
WHERE m.MaxNutritionValue > 100
```

Yaklaşım UDF için sınırlı değildir. Bu, herhangi bir yüksek maliyetlere neden olabilecek ifade geçerli olur. Örneğin, bir matematiksel işlev aynı yaklaşımı uygulayabileceğiniz `avg`:

```sql
SELECT TOP 1000 c.id, AvgNutritionValue
FROM c
JOIN (SELECT VALUE avg(n.nutritionValue) FROM n IN c.nutrients) AvgNutritionValue
WHERE AvgNutritionValue > 80
```

### <a name="mimic-join-with-external-reference-data"></a>Dış başvuru verileriyle birleştirme taklit

Genellikle başvuru nadiren değiştirir, ölçüm veya ülke kodları birimleri gibi statik verileri gerekebilir. Bu tür veriler her belge için yinelenen değil daha iyidir. Bu çoğaltma önleme depolama üzerinde kaydedin ve belge boyutunu daha küçük tutarak yazma performansı. Başvuru verilerinin bir koleksiyonunu iç birleşim semantiğiyle taklit etmek üzere sorgu kullanabilirsiniz.

Örneğin, bu başvuru veri kümesi göz önünde bulundurun:

| **Birim** | **Ad**            | **Çarpanı** | **Temel birim** |
| -------- | ------------------- | -------------- | ------------- |
| NG       | Nanogram            | 1.00E-09       | Dilbilgisi          |
| µg       | Microgram           | 1.00E-06       | Dilbilgisi          |
| Yönetim grubu       | Miligram           | 1.00E-03       | Dilbilgisi          |
| G        | Dilbilgisi                | 1.00E + 00       | Dilbilgisi          |
| kg       | Kilogram            | 1.00E + 03       | Dilbilgisi          |
| Yönetim grubu       | Megagram            | 1.00E + 06       | Dilbilgisi          |
| Gg       | Gigagram            | 1.00E + 09       | Dilbilgisi          |
| NJ       | Nanojoule           | 1.00E-09       | Joule         |
| ΜJ       | Microjoule          | 1.00E-06       | Joule         |
| mJ       | Millijoule          | 1.00E-03       | Joule         |
| J        | Joule               | 1.00E + 00       | Joule         |
| kJ       | Kilojoule           | 1.00E + 03       | Joule         |
| MJ       | Megajoule           | 1.00E + 06       | Joule         |
| GJ       | Gigajoule           | 1.00E + 09       | Joule         |
| CAL      | Kalori             | 1.00E + 00       | Kalori       |
| kcal     | Kalori             | 1.00E + 03       | Kalori       |
| IU       | Uluslararası birimleri |                |               |


Aşağıdaki sorgu, böylece çıkışı biriminin adını eklemek bu verilerle birleştirmek taklit eder:

```sql
SELECT TOP 10 n.id, n.description, n.nutritionValue, n.units, r.name
FROM food
JOIN n IN food.nutrients
JOIN r IN (
    SELECT VALUE [
        {unit: 'ng', name: 'nanogram', multiplier: 0.000000001, baseUnit: 'gram'},
        {unit: 'µg', name: 'microgram', multiplier: 0.000001, baseUnit: 'gram'},
        {unit: 'mg', name: 'milligram', multiplier: 0.001, baseUnit: 'gram'},
        {unit: 'g', name: 'gram', multiplier: 1, baseUnit: 'gram'},
        {unit: 'kg', name: 'kilogram', multiplier: 1000, baseUnit: 'gram'},
        {unit: 'Mg', name: 'megagram', multiplier: 1000000, baseUnit: 'gram'},
        {unit: 'Gg', name: 'gigagram', multiplier: 1000000000, baseUnit: 'gram'},
        {unit: 'nJ', name: 'nanojoule', multiplier: 0.000000001, baseUnit: 'joule'},
        {unit: 'µJ', name: 'microjoule', multiplier: 0.000001, baseUnit: 'joule'},
        {unit: 'mJ', name: 'millijoule', multiplier: 0.001, baseUnit: 'joule'},
        {unit: 'J', name: 'joule', multiplier: 1, baseUnit: 'joule'},
        {unit: 'kJ', name: 'kilojoule', multiplier: 1000, baseUnit: 'joule'},
        {unit: 'MJ', name: 'megajoule', multiplier: 1000000, baseUnit: 'joule'},
        {unit: 'GJ', name: 'gigajoule', multiplier: 1000000000, baseUnit: 'joule'},
        {unit: 'cal', name: 'calorie', multiplier: 1, baseUnit: 'calorie'},
        {unit: 'kcal', name: 'Calorie', multiplier: 1000, baseUnit: 'calorie'},
        {unit: 'IU', name: 'International units'}
    ]
)
WHERE n.units = r.unit
```

## <a name="scalar-subqueries"></a>Skaler alt sorgular

Tek bir değer veren bir alt sorguda bir skaler alt sorgu ifadesidir. Skaler alt sorgu ifade değeri ' % s'yansıtma (SELECT yan tümcesi) alt değeridir.  Pek çok yerde skaler bir ifade geçerli olduğu bir skaler alt sorgu ifade kullanabilirsiniz. Örneğin, herhangi bir ifade içinde her iki seçin ve WHERE yan tümceleri sorguda skaler kullanabilirsiniz.

Skaler bir alt sorgu kullanarak her zaman, ancak en iyi duruma getirmek değil. Örneğin, skaler bir alt sorguda bir sistem veya kullanıcı tanımlı işlevler için bağımsız değişken olarak geçirerek kaynak birimi (RU) tüketimi veya gecikme süresi hiçbir avantajı sağlar.

Skaler alt sorgulara daha ayrıntılı olarak sınıflandırılabilir:
* İfade basit skaler alt sorgular
* Toplama skaler alt sorgular

### <a name="simple-expression-scalar-subqueries"></a>İfade basit skaler alt sorgular

Bir ifade basit skaler alt sorgu herhangi bir toplama ifadesi içermeyen bir SELECT yan tümcesi olan ilişkili bir alt sorgu olduğu. Derleyici bunları daha büyük bir basit ifadeye dönüştürür olduğundan bu alt sorgular hiçbir iyileştirme yararlar sağlar. İç ve dış sorguları arasında bağıntılı bağlamı yok.

Bazı örnekler şunlardır:

**Örnek 1**

```sql
SELECT 1 AS a, 2 AS b
```

Bu sorgu için bir ifade basit skaler alt kullanarak yazabilirsiniz:

```sql
SELECT (SELECT VALUE 1) AS a, (SELECT VALUE 2) AS b
```

Her iki sorguları Bu çıktıyı üretir:

```json
[
  { "a": 1, "b": 2 }
]
```

**Örnek 2**

```sql
SELECT TOP 5 Concat('id_', f.id) AS id
FROM food f
```

Bu sorgu için bir ifade basit skaler alt kullanarak yazabilirsiniz:

```sql
SELECT TOP 5 (SELECT VALUE Concat('id_', f.id)) AS id
FROM food f
```

Sorgu çıktısı:

```json
[
  { "id": "id_03226" },
  { "id": "id_03227" },
  { "id": "id_03228" },
  { "id": "id_03229" },
  { "id": "id_03230" }
]
```

**Örnek 3**

```sql
SELECT TOP 5 f.id, Contains(f.description, 'fruit') = true ? f.description : undefined
FROM food f
```

Bu sorgu için bir ifade basit skaler alt kullanarak yazabilirsiniz:

```sql
SELECT TOP 10 f.id, (SELECT f.description WHERE Contains(f.description, 'fruit')).description
FROM food f
```

Sorgu çıktısı:

```json
[
  { "id": "03230" },
  { "id": "03238", "description":"Babyfood, dessert, tropical fruit, junior" },
  { "id": "03229" },
  { "id": "03226", "description":"Babyfood, dessert, fruit pudding, orange, strained" },
  { "id": "03227" }
]
```

### <a name="aggregate-scalar-subqueries"></a>Toplama skaler alt sorgular

Toplama skaler alt sorgu projeksiyon ya da tek bir değer veren filtresi toplama işlevine sahip alt sorgu olduğu.

**Örnek 1:**

Alt sorgu içeren kendi projeksiyon tek bir toplama işlevi ifadesinde şu şekildedir:

```sql
SELECT TOP 5 
    f.id, 
    (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg'
) AS count_mg
FROM food f
```

Sorgu çıktısı:

```json
[
  { "id": "03230", "count_mg": 13 },
  { "id": "03238", "count_mg": 14 },
  { "id": "03229", "count_mg": 13 },
  { "id": "03226", "count_mg": 15 },
  { "id": "03227", "count_mg": 19 }
]
```

**Örnek 2**

Alt sorgu birden fazla toplama işlevi ifadelerle şu şekildedir:

```sql
SELECT TOP 5 f.id, (
    SELECT Count(1) AS count, Sum(n.nutritionValue) AS sum 
    FROM n IN f.nutrients 
    WHERE n.units = 'mg'
) AS unit_mg
FROM food f
```

Sorgu çıktısı:

```json
[
  { "id": "03230","unit_mg": { "count": 13,"sum": 147.072 } },
  { "id": "03238","unit_mg": { "count": 14,"sum": 107.385 } },
  { "id": "03229","unit_mg": { "count": 13,"sum": 141.579 } },
  { "id": "03226","unit_mg": { "count": 15,"sum": 183.91399999999996 } },
  { "id": "03227","unit_mg": { "count": 19,"sum": 94.788999999999987 } }
]
```

**Örnek 3**

Aşağıda, bir toplama sorguda projeksiyon hem Filtresi ile bir sorgu verilmiştir:

```sql
SELECT TOP 5 
    f.id, 
    (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
FROM food f
WHERE (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') > 20
```

Sorgu çıktısı:

```json
[
  { "id": "03235", "count_mg": 27 },
  { "id": "03246", "count_mg": 21 },
  { "id": "03267", "count_mg": 21 },
  { "id": "03269", "count_mg": 21 },
  { "id": "03274", "count_mg": 21 }
]
```

Bu sorgu yazmak için daha uygun bir şekilde alt sorgu üzerinde katılın ve her iki SELECT diğer sorguda ve WHERE yan tümcelerini başvuru sağlamaktır. Bu sorgu yansıtma ve filtresi içinde değil ve join deyimi içinde yalnızca alt sorgu yürütmek gerektiğinden daha verimlidir.

```sql
SELECT TOP 5 f.id, count_mg
FROM food f
JOIN (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
WHERE count_mg > 20
```

#### <a name="exists-expression"></a>EXISTS ifadesi

Azure Cosmos DB EXISTS ifadeleri destekler. Azure Cosmos DB SQL API ile oluşturulmuş bir toplama skaler alt sorgu budur. EXISTS, bir alt sorgu ifadesi alır ve alt sorgu herhangi bir satır döndürürse, true döndürür. mantıksal bir ifadedir. Aksi takdirde false döndürür.

Azure Cosmos DB SQL API'sine Boolean ifadeler ve herhangi bir skaler ifade arasında ayrım yapmaz çünkü EXISTS hem seçin ve WHERE yan tümcelerini kullanabilirsiniz. T-SQL (örneğin, EXISTS, içinde ve arasında) bir Boolean ifadesinin filtre sınırlı olduğu aksine budur.

EXISTS tanımsız, var olan tek bir değer döndürürse yanlış olarak değerlendirilir. Örneğin, yanlış değerini aşağıdaki sorguyu göz önünde bulundurun:
```sql
SELECT EXISTS (SELECT VALUE undefined)
```   


VALUE anahtar sözcüğü önceki sorgudaki atlanırsa, sorgu doğru olarak değerlendirilir:
```sql
SELECT EXISTS (SELECT undefined) 
```

Alt sorgu nesne içinde seçili listedeki değerlerin listesini alın. Seçili listedeki herhangi bir değer varsa, alt sorgu tek bir değer döndürür '{}'. Bu değer, EXISTS true olarak değerlendirilen şekilde tanımlanır.

#### <a name="example-rewriting-arraycontains-and-join-as-exists"></a>Örnek: Yeniden yazma ARRAY_CONTAINS ve EXISTS olarak birleştirme

ARRAY_CONTAINS yaygın bir kullanım örneği, belgeye bir dizide bir öğe varlığını filtre sağlamaktır. Bu durumda, biz etiketler dizisi "turuncu." adlı bir öğe içerip içermediğini görmek için denetimi

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE ARRAY_CONTAINS(f.tags, {name: 'orange'})
```

EXISTS kullanmak için aynı sorgu yazabilirsiniz:

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE EXISTS(SELECT VALUE t FROM t IN f.tags WHERE t.name = 'orange')
```

Ayrıca, ARRAY_CONTAINS yalnızca bir değer bir dizi içindeki herhangi bir öğeye eşit olup olmadığını kontrol edebilirsiniz. Dizin özellikleri daha karmaşık filtreler gerekiyorsa, birleşim kullanın.

Birimlerine göre filtreleyen şu sorguyu inceleyin ve `nutritionValue` dizideki özellikleri: 

```sql
SELECT VALUE c.description
FROM c
JOIN n IN c.nutrients
WHERE n.units= "mg" AND n.nutritionValue > 0
```

Her koleksiyondaki belgeleri için bir çapraz ürün dizi öğeleri ile gerçekleştirilir. Bu birleştirme işlemi, dizi içinde özelliklere filtre uygulamak için mümkün kılar. Ancak, bu sorgunun RU kullanımını önemli olacaktır. Örneği için 1.000 belge her dizide 100 öğe varsa, 1000 x 100 (diğer bir deyişle, 100.000) genişletecek tanımlama grubu.

EXISTS kullanarak bu pahalı bir çapraz ürün önlemeye yardımcı olabilir:

```sql
SELECT VALUE c.description
FROM c
WHERE EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0
)
```

Bu durumda, dizi öğelerinin EXISTS içinde filtreleme. Bir dizi öğesine filtre eşleşirse, ardından Proje ve EXISTS true olarak değerlendirilir.

Diğer ad EXISTS ayrıca ve projeksiyon başvurun:

```sql
SELECT TOP 1 c.description, EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0) as a
FROM c
```

Sorgu çıktısı:

```json
[
    {
        "description": "Babyfood, dessert, fruit pudding, orange, strained",
        "a": true
    }
]
```

#### <a name="array-expression"></a>DİZİ ifadesi

DİZİ ifadesi, bir dizi olarak bir sorgunun sonuçlarını proje için kullanabilirsiniz. Bu ifade yalnızca SELECT yan tümcesi içinde sorgu kullanabilirsiniz.

```sql
SELECT TOP 1   f.id, ARRAY(SELECT VALUE t.name FROM t in f.tags) AS tagNames
FROM  food f
```

Sorgu çıktısı:

```json
[
    {
        "id": "03238",
        "tagNames": [
            "babyfood",
            "dessert",
            "tropical fruit",
            "junior"
        ]
    }
]
```

İle diğer alt olduğu gibi bir dizi ifadesi filtrelerle mümkündür.

```sql
SELECT TOP 1 c.id, ARRAY(SELECT VALUE t FROM t in c.tags WHERE t.name != 'infant formula') AS tagNames
FROM c
```

Sorgu çıktısı:

```json
[
    {
        "id": "03226",
        "tagNames": [
            {
                "name": "babyfood"
            },
            {
                "name": "dessert"
            },
            {
                "name": "fruit pudding"
            },
            {
                "name": "orange"
            },
            {
                "name": "strained"
            }
        ]
    }
]
```

Dizi ifadeleri, FROM yan tümcesinde alt sorgulara sonrasında de gelebilir.

```sql
SELECT TOP 1 c.id, ARRAY(SELECT VALUE t.name FROM t in c.tags) as tagNames
FROM c
JOIN n IN (SELECT VALUE ARRAY(SELECT t FROM t in c.tags WHERE t.name != 'infant formula'))
```

Sorgu çıktısı:

```json
[
    {
        "id": "03238",
        "tagNames": [
            "babyfood",
            "dessert",
            "tropical fruit",
            "junior"
        ]
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [SQL sorgu örnekleri](how-to-sql-query.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Belge verilerini modelleme](modeling-data.md)
