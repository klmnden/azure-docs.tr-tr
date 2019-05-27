---
title: Azure Cosmos DB için SQL sorguları
description: Azure Cosmos DB SQL söz dizimi, veritabanı kavramlarını ve SQL sorguları hakkında bilgi edinin. SQL, bir Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: bbca0239053b8f3164055a07b376abc597b0348f
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954122"
---
# <a name="sql-query-examples-for-azure-cosmos-db"></a>Azure Cosmos DB için SQL sorgu örnekleri

Azure Cosmos DB SQL API hesabı, bir JSON sorgu dili yapılandırılmış sorgu dili (SQL) kullanarak sorgulama öğeleri destekler. Azure Cosmos DB Sorgu dilinin tasarım hedefleri vardır:

* Yeni bir sorgu dili inventing yerine en bilinen ve en popüler sorgu dillerinden biri, SQL destekler. SQL, JSON öğelerinin üzerine zengin sorgu için biçimsel bir programlama modeli sağlar.  

* JavaScript'in programlama modeli için sorgu dili temel olarak kullanın. JavaScript'in tür sistemi, ifade değerlendirmesi ve işlev çağrısını SQL API'sinin kökleri var. Projeksiyonlar ilişkisel, hiyerarşik gezinme JSON öğeleri arasında kendinden birleştirmeler, uzamsal sorgular ve gibi kullanıcı tanımlı işlevler (UDF'ler) çağrılmasını tamamen JavaScript'te yazılmış bu kökleri özellikler için doğal bir programlama modeli sağlar.

Bu makalede bazı örnek SQL sorguları basit JSON öğeleri gösterilmektedir. Azure Cosmos DB SQL dili sözdizimi hakkında daha fazla bilgi için bkz: [SQL söz dizimi başvurusu](sql-api-query-reference.md).

## <a id="GettingStarted"></a>SQL sorguları kullanmaya başlama

SQL API Cosmos DB hesabınızdaki adlı bir kapsayıcı oluşturma `Families`. İki basit JSON öğelerinin kapsayıcısında oluşturun ve bunlara karşı birkaç basit sorgu çalıştırın.

### <a name="create-json-items"></a>JSON öğeleri oluşturma

Aşağıdaki kod, iki basit JSON öğeleri aileleri hakkında oluşturur. Andersen ve Wakefield ailesi için basit JSON öğelerinin üst, alt öğeleri ve bunların Evcil Hayvanlar, adresi ve kayıt bilgileri içerir. İlk öğenin dizeleri, sayı, Boole, diziler ve iç içe özellikler vardır.


```json
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow",
         "gender": "female",
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "Seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

İkinci öğe kullanan `givenName` ve `familyName` yerine `firstName` ve `lastName`.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female", 
        "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

### <a name="query-the-json-items"></a>JSON öğeleri sorgu

JSON verilerini Azure Cosmos DB SQL sorgu dili önemli yönlerini bazıları anlamak için bazı sorguları deneyin.

Aşağıdaki sorgu öğeleri döndürür. burada `id` alan eşleşme `AndersenFamily`. Olduğundan bir `SELECT *` sorgu, sorgu çıktısı olan tam JSON öğesi. SELECT söz dizimi hakkında daha fazla bilgi için bkz. [SELECT deyimi](sql-api-query-reference.md#select-query). 

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sorgu sonuçlarını şunlardır: 

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

Aşağıdaki sorgu, farklı bir şekle JSON çıkışını yeniden biçimlendirir. Yeni bir JSON sorgu projeleri `Family` nesne iki seçilen alan ile `Name` ve `City`, Adres Şehir durumu aynı olduğunda. Bu durumda "NY, NY" eşleşir.

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

Sorgu sonuçlarını şunlardır:

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

Aşağıdaki sorgu ailedeki verilen adlarını, çocukların döndürür, `id` eşleşen `WakefieldFamily`ve şehre göre sıralı.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

Sonuçlar şu şekildedir:

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

Önceki örneklerde, Cosmos sorgu dili çeşitli yönlerini gösterir:  

* SQL API'si, JSON değerleri üzerinde çalışır olduğundan, satır ve sütun yerine ağaç şeklinde varlıklarla ilgilenir. Rastgele herhangi derinliği ağaç düğümleri gibi başvurabilirsiniz `Node1.Node2.Node3…..Nodem`iki parçalı başvurusunu benzer `<table>.<column>` ANSI SQL.

* Sorgu dili şemasız verilerle çalışır çünkü tür sistemi dinamik olarak bağlı olmalıdır. Farklı türlerde farklı öğeye aynı ifadesi üretebilir. Bir sorgunun sonucu, geçerli bir JSON değer, ancak bir sabit şemasına olmasını garanti yoktur.  

* Azure Cosmos DB, yalnızca Katı JSON öğelerini destekler. Tür sistemi ve ifadeleri yalnızca JSON türleri ile dağıtılacak kısıtlanır. Daha fazla bilgi için [JSON belirtimi](https://www.json.org/).  

* Bir Cosmos DB kapsayıcısında JSON öğelerinin şemasız bir koleksiyondur. Birincil anahtar ve yabancı anahtar ilişkileri tarafından değil, içinde ve kapsayıcı öğeleri arasında ilişkiler kapsamlarına göre örtük olarak yakalanır. Bu özellik, bu makalenin sonraki bölümlerinde ele alınan içi öğesi birleştirme için önemlidir.

## <a id="SelectClause"></a>SELECT yan tümcesi

Her sorgu bir SELECT yan tümcesi ve isteğe bağlı FROM oluşur ve ANSI SQL standartları başına WHERE yan tümcelerini. Genellikle, kaynak FROM yan tümcesindeki numaralandırılana ve WHERE yan tümcesi JSON öğelerinin kümesini almak için kaynak bir filtre uygular. SELECT yan tümcesi, ardından istenen JSON değerleri seçim listesinde yansıtıyor. Sözdizimi hakkında daha fazla bilgi için bkz. [SELECT deyimi](sql-api-query-reference.md#select-query).

Aşağıdaki SELECT sorgu örneği döndürür `address` gelen `Families` olan `id` eşleşen `AndersenFamily`:

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şu şekildedir:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

## <a id="EscapingReservedKeywords"></a>Tırnak işaretli bir özellik erişimcisi
Tırnak işaretli özelliği [] işleci kullanılarak özelliklerine erişebilirsiniz. Örneğin, `SELECT c.grade` ve `SELECT c["grade"]` eşdeğerdir. Bu sözdizimi, kaçış özel karakterleri, boşluk içeren veya SQL anahtar sözcüğü ya da ayrılmış sözcük aynı ada sahip bir özellik kullanışlıdır.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

## <a name="nested-properties"></a>İç içe Özellikler

Aşağıdaki örnek iki iç içe özellikler projeleri `f.address.state` ve `f.address.city`.

```sql
    SELECT f.address.state, f.address.city
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şu şekildedir:

```json
    [{
      "state": "WA",
      "city": "Seattle"
    }]
```

## <a name="json-expressions"></a>JSON ifadeleri

Projeksiyon, ayrıca aşağıdaki örnekte gösterildiği gibi JSON ifadeleri destekler:

```sql
    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şu şekildedir:

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle",
        "name": "AndersenFamily"
      }
    }]
```

Önceki örnekte, bir JSON nesnesi oluşturmak SELECT yan tümcesi gerekir ve örnek, herhangi bir anahtar sağlar. bu yana yan tümcesi örtük bağımsız değişken adını kullanır. `$1`. Aşağıdaki sorguda iki örtük bağımsız değişkenlerini döndürür: `$1` ve `$2`.

```sql
    SELECT { "state": f.address.state, "city": f.address.city },
           { "name": f.id }
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şu şekildedir:

```json
    [{
      "$1": {
        "state": "WA",
        "city": "Seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]
```

## <a id="ValueKeyword"></a>VALUE anahtar sözcüğü

VALUE anahtar sözcüğü, JSON değerinin başına döndürmek için bir yol sağlar. Örneğin, aşağıda gösterilen sorguyu skaler bir ifade döndürür `"Hello World"` yerine `{$1: "Hello World"}`:

```sql
    SELECT VALUE "Hello World"
```

JSON değerleri olmadan aşağıdaki sorguyu döndürür `address` etiketi:

```sql
    SELECT VALUE f.address
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan",
        "city": "NY"
      }
    ]
```

Aşağıdaki örnek, JSON basit değerlerin (yaprak düzey JSON ağacı) döndürülecek gösterilmektedir:


```sql
    SELECT VALUE f.address.state
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
    [
      "WA",
      "NY"
    ]
```

## <a id="DistinctKeyword"></a>DISTINCT anahtar sözcüğü

DISTINCT anahtar sözcüğüne çoğaltmaları sorgunun projeksiyon olarak ortadan kaldırır.

```sql
SELECT DISTINCT VALUE f.lastName
FROM Families f
```

Bu örnekte, her bir soyadı için değerler sorgu yansıtıyor.

Sonuçlar şu şekildedir:

```json
[
    "Andersen"
]
```

Benzersiz nesne da yansıtabilirsiniz. Bu durumda, sorgu boş bir nesne döndürecek şekilde lastName alanlarını iki belge birinde yok.

```sql
SELECT DISTINCT f.lastName
FROM Families f
```

Sonuçlar şu şekildedir:

```json
[
    {
        "lastName": "Andersen"
    },
    {}
]
```

DISTINCT projeksiyon alt sorgu içinde de kullanılabilir:

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

Bu sorgu, yinelenenleri kaldırılan ile her çocuğun givenName içeren bir dizi yansıtıyor. Bu dizi ChildNames olarak bilinir ve dış sorguda yansıtılır.

Sonuçlar şu şekildedir:

```json
[
    {
        "id": "AndersenFamily",
        "ChildNames": []
    },
    {
        "id": "WakefieldFamily",
        "ChildNames": [
            "Jesse",
            "Lisa"
        ]
    }
]
```

## <a name="aliasing"></a>Diğer ad kullanımı

Açıkça diğer ad değer sorguları içinde kullanabilirsiniz. Bir sorgu aynı ada sahip iki özellik varsa, diğer ad kullanımı birini veya her ikisini özelliklerini öngörülen sonucunda disambiguated şekilde yeniden adlandırmak için kullanın.

İkinci değer olarak yansıtılırken aşağıdaki örnekte gösterildiği gibi diğer ad kullanımı için kullanılan anahtar sözcüğü isteğe bağlı, olduğu gibi `NameInfo`:

```sql
    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo,
           { "name": f.id } NameInfo
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şu şekildedir:

```json
    [{
      "AddressInfo": {
        "state": "WA",
        "city": "Seattle"
      },
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]
```

## <a id="FromClause"></a>FROM yan tümcesi

Kimden (`FROM <from_specification>`) kaynak filtre veya sorguyu daha sonra öngörülen sürece yan tümcesinin isteğe bağlıdır. Sözdizimi hakkında daha fazla bilgi için bkz. [SÖZDİZİMİNDEN](sql-api-query-reference.md#bk_from_clause). Bir sorgu ister `SELECT * FROM Families` tüm numaralandırır `Families` kapsayıcı. Özel tanımlayıcısı kök kapsayıcı adı yerine kapsayıcısı için de kullanabilirsiniz.

FROM yan tümcesi sorgu başına aşağıdaki kuralları uygular:

* Kapsayıcı gibi diğer adı, olabilir `SELECT f.id FROM Families AS f` ya da yalnızca `SELECT f.id FROM Families f`. Burada `f` için diğer ad `Families`. Diğer isteğe bağlı bir anahtar sözcük tanımlayıcı olarak.  

* Diğer adlı bir kez, özgün kaynak adına bağımlı olamaz. Örneğin, `SELECT Families.id FROM Families f` sözdizimsel olarak geçersiz olduğundan tanımlayıcı `Families` diğer adlı yapıldı ve artık çözümlenemiyor.  

* Başvurulan tüm özellikleri, katı şema bağlılığı olmaması belirsiz herhangi bir bağlamayı önlemek tam olarak nitelenmiş olmalıdır. Örneğin, `SELECT id FROM Families f` sözdizimsel olarak geçersiz olduğundan özelliği `id` bağlı değil.

### <a name="get-subitems-by-using-the-from-clause"></a>FROM yan tümcesi kullanarak alt öğelerini alma

FROM yan tümcesi için daha küçük bir alt kaynak azaltabilir. Yalnızca her bir öğenin alt ağacı Numaralandırılacak subroot aşağıdaki örnekte gösterildiği gibi kaynak hale gelebilir:

```sql
    SELECT *
    FROM Families.children
```

Sonuçlar şu şekildedir:

```json
    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
       {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

Önceki sorgunun kaynağı olarak bir dizi kullanılan, ancak nesne kaynağı olarak kullanabilirsiniz. Sorgu, tüm geçerli, tanımlı bir JSON değeri sonuç ekleme için kaynak olarak değerlendirir. Aşağıdaki örnek hariç tutmak `Families` zorunda kalmaz bir `address.state` değeri.

```sql
    SELECT *
    FROM Families.address.state
```

Sonuçlar şu şekildedir:

```json
    [
      "WA",
      "NY"
    ]
```

## <a id="WhereClause"></a>WHERE yan tümcesi

İsteğe bağlı WHERE yan tümcesi (`WHERE <filter_condition>`) koşulları belirtir kaynak JSON öğeleri sorgu sonuçlarında eklemek için karşılaması gerekir. Bir JSON öğesi için belirtilen koşullar değerlendirmelidir `true` sonucu olarak kabul edilmesi için. Dizin katman WHERE yan tümcesi, sonuç bir parçası olabilir kaynak öğeleri en küçük kümesini belirlemek için kullanır. Sözdizimi hakkında daha fazla bilgi için bkz. [nerede söz dizimi](sql-api-query-reference.md#bk_where_clause).

Aşağıdaki sorguda içeren istekleri öğelerini bir `id` özellik değeri `AndersenFamily`. Sahip olmayan herhangi bir öğeyi hariç bir `id` özelliği veya değeri eşleşmiyor `AndersenFamily`.

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sonuçlar şu şekildedir:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>WHERE yan tümcesinde skaler ifade

Önceki örnekte, bir basit eşitlik sorgu gösterdi. SQL API'si ayrıca çeşitli destekler [skaler ifadelerin](#scalar-expressions). En sık kullanılan ikili ve birli ifadelerdir. Kaynak JSON nesne özelliği başvurularından da geçerli ifadelerdir.

Aşağıdaki desteklenen ikili işleçler kullanabilirsiniz:  

|**İşleç türü**  | **Değerler** |
|---------|---------|
|Aritmetik | +,-,*,/,% |
|bit düzeyinde    | \|, &, ^, <<>>,, >>> (sıfır dolgu sağa kaydırma) |
|Mantıksal    | VE, VEYA DEĞİL      |
|Karşılaştırma | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|Dize     |  \|\| (birleştirme) |

Aşağıdaki sorgularda ikili işleçler kullanın:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

Birli işleçleri kullanabilirsiniz +,-, ~, aşağıdaki örneklerde gösterildiği gibi sorgu değil ve:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

Sorgularda başvuran bir özelliğe de kullanabilirsiniz. Örneğin, `SELECT * FROM Families f WHERE f.isRegistered` özelliği içeren JSON öğeyi döndürür `isRegistered` eşit bir değer ile `true`. Gibi diğer değer `false`, `null`, `Undefined`, `<number>`, `<string>`, `<object>`, veya `<array>`, öğe sonuçtan dışlar. 

### <a name="equality-and-comparison-operators"></a>Eşitlik ve Karşılaştırma işleçleri

Aşağıdaki tabloda, her iki JSON türünden SQL API eşitlik karşılaştırmaları sonucunu gösterir.

| **OP** | **Tanımsız** | **Null** | **Boole değeri** | **Sayı** | **dize** | **Nesne** | **Dizi** |
|---|---|---|---|---|---|---|---|
| **Tanımsız** | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı |
| **Null** | Tanımlanmadı | **Tamam** | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı |
| **Boole değeri** | Tanımlanmadı | Tanımlanmadı | **Tamam** | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı |
| **Sayı** | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | **Tamam** | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı |
| **dize** | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | **Tamam** | Tanımlanmadı | Tanımlanmadı |
| **Nesne** | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | **Tamam** | Tanımlanmadı |
| **Dizi** | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | Tanımlanmadı | **Tamam** |

Karşılaştırma işleçleri gibi `>`, `>=`, `!=`, `<`, ve `<=`, karşılaştırma türleri arasında veya iki arasında nesneleri veya dizi üretir `Undefined`.  

Skaler ifade sonucu ise `Undefined`, öğe sonucunda, çünkü bulunmayan `Undefined` eşit değildir `true`.

### <a name="logical-and-or-and-not-operators"></a>Mantıksal (AND, OR ve NOT) işleçleri

Mantıksal işleçler Boole değerleri üzerinde çalışır. Aşağıdaki tablolarda, bu işleçler için mantıksal gerçekte tabloları göster:

**OR işleci**

| OR | True | False | Tanımlanmadı |
| --- | --- | --- | --- |
| True |True |True |True |
| False |True |False |Tanımlanmadı |
| Tanımlanmadı |True |Tanımlanmadı |Tanımlanmadı |

**AND işleci**

| VE | True | False | Tanımlanmadı |
| --- | --- | --- | --- |
| True |True |False |Tanımlanmadı |
| False |False |False |False |
| Tanımlanmadı |Tanımlanmadı |False |Tanımlanmadı |

**NOT işleci**

| DEĞİL |  |
| --- | --- |
| True |False |
| False |True |
| Tanımlanmadı |Tanımlanmadı |

## <a name="between-keyword"></a>ARASINDA anahtar sözcüğü

ANSI SQL olduğu gibi BETWEEN anahtar sözcüğü, dize veya sayısal değer aralık sorguları ifade etmek için kullanabilirsiniz. Örneğin, aşağıdaki sorgu ilk alt öğenin sınıf 1-5, dahil olan tüm öğeleri döndürür.

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5
```

Aksine, ANSI SQL BETWEEN yan tümcesi aşağıdaki örnekte olduğu gibi FROM yan tümcesinde kullanabilirsiniz.

```sql
    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c
```

ANSI SQL aksine SQL API'si de aralık sorguları karma türlerin özelliklerine karşı ifade edebilir. Örneğin, `grade` sayı benzer olabilir `5` bazı öğeler ve bir dize gibi `grade4` bazılarında. JavaScript, olduğu gibi bu gibi durumlarda, iki farklı türler arasında karşılaştırma sonuçları içinde `Undefined`, öğe atlanır.

> [!TIP]
> Daha hızlı sorgu yürütme süreleri için hiçbir BETWEEN yan tümcesi sayısal özelliği veya yolları karşı bir aralık dizin türü kullanan bir dizin oluşturma ilkesi oluşturun.

## <a name="in-keyword"></a>Anahtar SÖZCÜĞÜ

IN anahtar sözcüğü, bir listedeki herhangi bir değer belirtilen bir değerle eşleşip eşleşmediğini kontrol etmek için kullanın. Örneğin, aşağıdaki sorgu tüm ailesi öğeleri döndürür burada `id` olduğu `WakefieldFamily` veya `AndersenFamily`.

```sql
    SELECT *
    FROM Families
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')
```

Aşağıdaki örnek, durum belirtilen değerlerden herhangi birini olduğu tüm öğeleri döndürür:

```sql
    SELECT *
    FROM Families
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")
```

## <a name="-operator"></a>* işleci

Özel işleç * olduğu gibi tüm öğe projeleri. Kullanıldığında yansıtılan tek alan olması gerekir. Bir sorgu ister `SELECT * FROM Families f` geçerlidir, ancak `SELECT VALUE * FROM Families f` ve `SELECT *, f.id FROM Families f` geçerli değildir. [Önce bu makaledeki sorgu](#query-the-json-items) kullanılan * işleci. 

## <a name="-and--operators"></a>? ve?? İşleçleri

Ternary (?) kullanın ve programlama dilleri gibi olduğu gibi koşullu ifadeleri oluşturmak için (?) işleçleri birleşim C# ve JavaScript. 

Kullanabilirsiniz? Yeni JSON özellikleri hareket halindeyken oluşturmak için işleç. Örneğin, aşağıdaki sorguyu sınıf düzeylerde sınıflandıran `elementary` veya `other`:

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

Çağrı Ayrıca iç içe yerleştirebilirsiniz? Aşağıdaki sorgu olduğu gibi işleç: 

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high") AS gradeLevel
    FROM Families.children[0] c
```

Diğer sorgu işleçleri olduğu gibi mi? işleci, başvurulan özellikleri eksik olan veya karşılaştırılan türleri farklı öğeleri hariç tutar.

Kullanım?? verimli bir şekilde bir öğede bir özellik karşı yarı yapılandırılmış veya karma tür veriler sorgulanırken olup olmadığını denetlemek için işleci. Örneğin, aşağıdaki döndürür sorgu `lastName` varsa veya `surname` varsa `lastName` bulunmaz.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a id="TopKeyword"></a>TOP işleci

İlk üst anahtar sözcüğü döner `N` tanımlanmamış bir sırada sorgu sonuç sayısı. İlk sonuçlarını sınırlamak için en iyi uygulama, üst ile ORDER BY yan tümcesi kullanın `N` sıralı değerleri sayısı. Bu iki yan tümceyi birleştirme üst etkiler, satırları tahmin edilebilir bir biçimde belirtmek için tek yoludur.

Aşağıdaki örnekte olduğu gibi sabit bir değer ile veya bir değişken değerle parametreli sorgular kullanma üst kullanabilirsiniz. Daha fazla bilgi için [parametreli sorgular](#parameterized-queries) bölümü.

```sql
    SELECT TOP 1 *
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

## <a id="OrderByClause"></a>ORDER BY yan tümcesi

ANSI SQL olduğu gibi sorgular, isteğe bağlı bir ORDER BY yan tümcesi ekleyebilirsiniz. İsteğe bağlı ASC veya DESC bağımsız değişken artan veya azalan düzende sonuçları almak belirtir. ASC varsayılan değerdir.

Örneğin, yerleşik şehir adı, artan aileleri alan bir sorgu aşağıdadır:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

Aşağıdaki sorgu ailesi alır `id`kendi öğesi oluşturma tarih sırasına göre s. Öğe `creationDate` temsil eden sayı olan *dönem zamanı*, ya da 1 Ocak 1970 saniye beri geçen süre.

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.creationDate DESC
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472
      }
    ]
```

Ayrıca, birden çok özelliklerine göre sıralayabilirsiniz. Birden çok özelliklerine göre sıralayan bir sorgu gerektiren bir [bileşik dizin](index-policy.md#composite-indexes). Şu sorguyu inceleyin:

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.address.city ASC, f.creationDate DESC
```

Bu sorgu ailesi alır `id` artan düzende şehir adı. Birden çok öğe aynı şehir adı varsa, sorgu tarafından sırayla `creationDate` azalan sırayla düzenleyin.

## <a id="OffsetLimitClause"></a>UZAKLIK sınırlama yan tümcesi

UZAKLIK bazı sayısı değerleri sorgudan Al atlamak için isteğe bağlı bir yan tümcesi sınırdır. UZAKLIK sayımı ve sınırı sayısı sınırı UZAKLIĞI yan tümcesinde gereklidir.

UZAKLIK sınırı, ORDER BY yan tümcesi ile birlikte kullanıldığında, sonuç kümesi Atla yaparak oluşturulur ve sıralı değerlerine gerçekleştirin. ORDER BY yan tümce kullandıysanız, değer belirleyici bir sırada neden olur.

Örneğin, ilk değer atlar ve ikinci değer (yerleşik şehir adı sırasına göre) döndüren bir sorgu aşağıdadır:

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
    OFFSET 1 LIMIT 1
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

İlk değer atlar ve ikinci değer (sıralama olmadan) döndüren bir sorgu aşağıda verilmiştir:

```sql
   SELECT f.id, f.address.city
    FROM Families f
    OFFSET 1 LIMIT 1
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "Seattle"
      }
    ]
```




## <a name="scalar-expressions"></a>Skaler ifade

SELECT yan tümcesi skaler ifadeler sabitler, aritmetik ifadeler ve mantıksal ifadeleri gibi destekler. Aşağıdaki sorgu, skaler bir ifade kullanır:


```sql
    SELECT ((2 + 11 % 7)-2)/3
```

Sonuçlar şu şekildedir:

```json
    [{
      "$1": 1.33333
    }]
```

Aşağıdaki sorguda, skaler ifade sonucu bir Boolean verilmiştir:


```sql
    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "AreFromSameCityState": false
      },
      {
        "AreFromSameCityState": true
      }
    ]
```

## <a name="object-and-array-creation"></a>Nesne ve dizi oluşturma

Bir anahtar SQL API'si dizi ve nesne oluşturma özelliğidir. Önceki örnekte oluşturulan yeni bir JSON nesnesi `AreFromSameCityState`. Diziler, ayrıca, aşağıdaki örnekte gösterildiği gibi da oluşturabilirsiniz:


```sql
    SELECT [f.address.city, f.address.state] AS CityState
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "CityState": [
          "Seattle",
          "WA"
        ]
      },
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]
```

Aşağıdaki SQL sorgusunu kullanarak alt sorgularda içindeki dizi, başka bir örnektir. Bu sorgu alt öğeleri bir dizideki tüm farklı verilen adlarını alır.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```


## <a id="Iteration"></a>Yineleme

SQL API'si, FROM kaynak ın anahtar sözcük aracılığıyla eklenen yeni bir yapısı ile JSON diziler yineleme için destek sağlar. Aşağıdaki örnekte:

```sql
    SELECT *
    FROM Families.children
```

Sonuçlar şu şekildedir:

```json
    [
      [
        {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        }, 
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]
```

Sonraki sorgu, üzerinden yineleme gerçekleştirir `children` içinde `Families` kapsayıcı. Çıkış dizisi, önceki sorgudan farklıdır. Bu örnekte böler `children`ve sonuçları tek bir dizide düzleştirir:  

```sql
    SELECT *
    FROM c IN Families.children
```

Sonuçlar şu şekildedir:

```json
    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]
```

Filtre uygulayabilirsiniz dizinin tek tek her girişinde aşağıdaki örnekte gösterildiği gibi daha fazla:

```sql
    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8
```

Sonuçlar şu şekildedir:

```json
    [{
      "givenName": "Lisa"
    }]
```

Ayrıca, bir dizi yineleme sonucu üzerinde de toplayabilirsiniz. Örneğin, aşağıdaki sorgu tüm aileleri arasında alt öğeleri sayar:

```sql
    SELECT COUNT(child)
    FROM child IN Families.children
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "$1": 3
      }
    ]
```

## <a id="Joins"></a>Birleşimler

İlişkisel bir veritabanında tabloları arasında birleştirmeler normalleştirilmiş şemaları tasarlamaya mantıksal corollary ' dir. Buna karşılık, SQL API'si mantıksal şemasız öğeleri normalleştirilmişlikten çıkarılmış veri modelini kullanan, eşdeğer bir *kendi kendine birleşme*.

Sözdizimi dili destekleyen `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`. Bu sorgu içeren başlık kümesi döndürür `N` değerleri. Her bir tanımlama grubunu tüm kapsayıcı diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor. Diğer bir deyişle, bu sorgu birleştirme işleminde katılan kümelerin tam bir çapraz ürün yapar.

Aşağıdaki örnekler, JOIN yan tümcesi nasıl çalıştığını gösterir. Aşağıdaki örnekte, her bir kaynaktan gelen öğenin çapraz ürün beri sonucu boş olduğundan ve boş boştur:

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

Sonuç olur:

```json
    [{
    }]
```

Aşağıdaki örnekte, iki JSON nesnesi, öğe kök arasında çapraz ürün birleşimi olan `id` ve `children` subroot. Olgu, `children` olan tek bir kök ile ilgilenen çünkü bir dizi birleşimde, etkin olmayan olduğu `children` dizisi. Her bir öğeyle diziye çapraz çarpımını tam olarak yalnızca bir öğe sağladığından sonucu yalnızca iki sonuçlarını içerir.

```sql
    SELECT f.id
    FROM Families f
    JOIN f.children
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

Aşağıdaki örnek, daha geleneksel bir birleştirme gösterir:

```sql
    SELECT f.id
    FROM Families f
    JOIN c IN f.children
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

FROM JOIN yan tümcesi bir yineleyici kaynağıdır. Bu nedenle, önceki örnekte akışı şöyledir:  

1. Her alt öğeyi Genişlet `c` dizi.
2. Kök öğesi ile bir çapraz ürün `f` her alt öğe `c` , ilk adım düzleştirilmiş.
3. Son olarak, kök nesnenin proje `f` `id` tek başına bir özellik.

İlk öğeyi `AndersenFamily`, yalnızca bir tane var. `children` sonuç kümesi tek bir nesne içerecek şekilde öğesi. İkinci öğe `WakefieldFamily`, iki tane `children`çapraz her iki nesne oluşturur. Bu nedenle `children` öğesi. Bir çapraz ürün beklediğiniz gibi bu her iki öğe kök alanları aynıdır.

Gerçek JOIN yan tümcesinin form diziler için Aksi takdirde projeye zor olan Şekil arası ürünün programıdır. Kullanıcının sağlayan bir tanımlama grubu birleşimi filtreleri aşağıdaki örnekte genel diziler memnun bir koşul seçin.

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "familyName": "AndersenFamily",
        "childFirstName": "Henriette Thaulow",
        "petName": "Fluffy"
      },
      {
        "familyName": "WakefieldFamily",
        "childGivenName": "Jesse",
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

Önceki örnekte bulunan aşağıdaki uzantı double JOIN gerçekleştirir. Çapraz aşağıdaki sözde kod görüntüleyebilir:

```
    for-each(Family f in Families)
    {
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName,
                  c.givenName AS childGivenName,
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }
```

`AndersenFamily` Çapraz bir satır üretir. Bu nedenle bir evcil hayvan olan bir alt öğesi var (1\*1\*1) bu aile öğesinden. `WakefieldFamily` iki alt öğe, yalnızca bunlardan birinin Evcil Hayvanlar sahiptir, ancak bu alt iki Evcil Hayvanlar içeriyor. Çapraz ürün bu aile için 1 verir\*1\*2 = 2 satır.

Sonraki örnekte olduğundan bir ek filtre `pet`, evcil hayvan adı olduğu tüm diziler dışlar `Shadow`. Diziler dizilerden tanımlama grubu öğelerinin herhangi bir filtre oluşturun ve proje öğeleri herhangi bir birleşimini kullanabilirsiniz.

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"
```

Sonuçlar şu şekildedir:

```json
    [
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

## <a id="UserDefinedFunctions"></a>Kullanıcı tanımlı işlevler (UDF'ler)

SQL API'si, kullanıcı tanımlı işlevler (UDF'ler) için destek sağlar. Skaler UDF ile sıfır veya daha fazla bağımsız değişkenleri geçirmek ve tek bir bağımsız değişken sonuç döndürür. API'nin her bağımsız değişken geçerli JSON değerleri teşekkür denetler.  

API'si UDF'leri kullanarak özel uygulama mantığını destekleyecek şekilde SQL söz dizimi genişletir. SQL API'si ile UDF'ler kaydetmek ve bunları SQL sorgularında başvurun. Aslında, UDF'ler exquisitely sorgularından çağırmak için tasarlanmıştır. Bir corollary UDF'ler saklı yordamları ve Tetikleyicileri gibi diğer JavaScript türleri gibi bağlam nesnesi için erişiminiz yok. Sorgular salt okunurdur ve birincil veya ikincil çoğaltma üzerinde çalıştırabilirsiniz. Diğer JavaScript türlerinin aksine, UDF'ler, ikincil çoğaltmalarda çalışacak şekilde tasarlanmıştır.

Aşağıdaki örnek bir öğe kapsayıcısı altında bir UDF Cosmos DB veritabanına kaydeder. Örnek adı olan bir UDF oluşturur `REGEX_MATCH`. İki JSON dizesi değerini kabul `input` ve `pattern`, ve ilk eşleşme ikinci desen belirtilmişse denetimleri kullanarak JavaScript'in `string.match()` işlevi.

```javascript
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) {
                      return input.match(pattern) !== null;
                   };",
       };

       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
           regexMatchUdf).Result;  
```

Şimdi bu UDF bir sorgu projeksiyon kullanın. Büyük/küçük harfe ön ekine sahip UDF nitelemelidir `udf.` bunları sorgulara çağırırken.

```sql
    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "$1": true
      },
      {
        "$1": false
      }
    ]
```

İle tam UDF kullanabileceğiniz `udf.` aşağıdaki örnekteki gibi bir filtre içinde öneki:

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")
```

Sonuçlar şu şekildedir:

```json
    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]
```

Esas olarak, UDF'ler izdüşümler ve filtreler kullandığınız geçerli bir skaler ifadelerin ' dir.

UDF'ler gücüyle genişletmek için koşullu mantık ile başka bir örneğe bakın:

```javascript
       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                   switch (city) {
                       case 'Seattle':
                           return 520;
                       case 'NY':
                           return 410;
                       case 'Chicago':
                           return 673;
                       default:
                           return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("myDatabase", "families"),
                seaLevelUdf);
```

Aşağıdaki örnek UDF uygular:

```sql
    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
     [
      {
        "city": "Seattle",
        "seaLevel": 520
      },
      {
        "city": "NY",
        "seaLevel": 410
      }
    ]
```

Başvurulan özellikleri tarafından UDF parametreler JSON değeri kullanılamaz, parametre olarak kabul edilir tanımsız ve UDF çağrısı atlandı. Benzer şekilde, UDF sonucu tanımlanmamış ise, sonuçta bulunmaz.

Önceki örneklerde gösterildiği gibi UDF'ler JavaScript dilinin gücünü SQL API'si ile tümleştirin. UDF yerleşik JavaScript çalışma zamanı Özellikleri'nın yardımıyla karmaşık yordam, koşullu mantık yapmak için zengin bir programlanabilir arabirimi sağlar. SQL API'si bağımsız değişkenler UDF için her kaynak öğesi için geçerli nerede veya SELECT yan tümcesi sağlar aşaması. Sonuç, sorunsuz bir şekilde genel yürütme işlem hattında eklenmiştir. Özet olarak, karmaşık iş mantığı sorguları bir parçası olarak yapmak için harika Araçlar UDF'ler var.

## <a id="Aggregates"></a>Toplama işlevleri

Toplama işlevleri, SELECT yan tümcesinde değerleri kümesi üzerinde bir hesaplama gerçekleştirmek ve tek bir değer döndürür. Örneğin, aşağıdaki sorgu öğelerin sayısını döndürür `Families` kapsayıcı:

```sql
    SELECT COUNT(1)
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
    [{
        "$1": 2
    }]
```

Ayrıca, değer anahtar sözcüğü kullanarak toplama yalnızca skaler değer döndürebilir. Örneğin, aşağıdaki sorgu, tek bir sayı değerlerin sayısını döndürür:

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
    [ 2 ]
```

Ayrıca, filtrelerle toplamalar birleştirebilirsiniz. Örneğin, aşağıdaki sorguyu adresi durumuna sahip öğelerin sayısını döndürür `WA`.

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

Sonuçlar şu şekildedir:

```json
    [ 1 ]
```

SQL API'si aşağıdaki toplama işlevleri destekler. Toplam ve ortalama sayısal değerlerle çalışır ve sayısı, en az ve en yüksek sayılar, dizeler, Boole değerleri ve null değerlere çalışır.

| İşlev | Açıklama |
|-------|-------------|
| COUNT | İfade öğe sayısını döndürür. |
| SUM   | İfadedeki tüm değerlerin toplamını döndürür. |
| MIN   | İfadedeki en küçük değeri döndürür. |
| MAX   | İfadedeki en büyük değeri döndürür. |
| AVG   | İfadedeki değerlerin ortalamasını döndürür. |

Ayrıca, bir dizi yineleme sonuçları üzerinde de toplayabilirsiniz. Daha fazla bilgi için [yineleme](#Iteration) bölümü.

> [!NOTE]
> Azure portalında Veri Gezgini içinde toplama sorguları kısmi sonuçlar üzerinde yalnızca bir sorgu sayfası toplanabilir. SDK, tüm sayfalara tek bir toplu değer oluşturur. Kod kullanarak toplama sorguları gerçekleştirmek için .NET SDK'sı 1.12.0, .NET Core SDK'sı 1.1.0 veya Java SDK'sı 1.9.5 gerekir veya üzeri.
>

## <a id="BuiltinFunctions"></a>Yerleşik işlevler

Cosmos DB, kullanıcı tanımlı işlevler (UDF'ler) gibi sorguları içinde kullanabilirsiniz ortak işlemler için çok sayıda yerleşik işlev de destekler.

| İşlev grubu | İşlemler |
|---------|----------|
| Matematik işlevleri | ABS, TAVAN, EXP, FLOOR, GÜNLÜK, LOG10, GÜÇ, HEPSİNİ, OTURUM, SQRT, KARE, TRUNC, ACOS, ASIN, ATAN, ATN2, COT, DERECE, PI, RADIANS, SIN COS, TAN |
| Tür denetleme işlevleri | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, IS_PRIMITIVE |
| Dize işlevleri | CONCAT, İÇERİR, ENDSWITH, INDEX_OF, SOL, UZUNLUĞU, DÜŞÜK LTRIM, DEĞİŞTİR, ÇOĞALTMAK, SAĞ TERS RTRIM, STARTSWITH, ALT, ÜST |
| Dizi işlevleri | ARRAY_CONCAT, ARRAY_CONTAINS ARRAY_LENGTH ve ARRAY_SLICE |
| Uzamsal İşlevler | ST_DISTANCE ST_WITHIN, ST_INTERSECTS ST_ISVALID, ST_ISVALIDDETAILED |

Şu anda yerleşik işlevi artık kullanılabilir olduğu bir kullanıcı tanımlı işlev (UDF) kullanıyorsanız, karşılık gelen yerleşik işlev çalıştırmak daha hızlı ve daha verimli olacaktır.

Cosmos DB işlevleri ve ANSI SQL işlevleri arasındaki temel fark, Cosmos DB işlevleri de şemasız hem de karma şema verilerle çalışacak şekilde tasarlanmıştır ' dir. Örneğin, bir özellik eksik veya varsa bir sayısal olmayan değer gibi `unknown`, bir hata döndürmek yerine öğenin atlandı.

### <a name="mathematical-functions"></a>Matematik işlevleri

Matematiksel işlevler her bağımsız değişken olarak sağlanan ve sayısal bir değer döndürmesi giriş değerlerini temel alan, bir hesaplama gerçekleştirir. Desteklenen yerleşik matematiksel işlevler tablosu aşağıdadır.

| Kullanım | Açıklama |
|----------|--------|
| ABS (num_expr) | Belirtilen sayısal ifade (pozitif) mutlak değerini döndürür. |
| TAVAN (num_expr) | Büyüktür veya eşittir, belirtilen sayısal ifadenin en küçük tamsayı değerini döndürür. |
| KATI (num_expr) | Belirtilen sayısal ifade küçük veya eşit en büyük tamsayı döndürür. |
| EXP (num_expr) | Belirtilen sayısal ifadenin üssünü döndürür. |
| Günlük (num_expr, temel) | Belirtilen sayısal ifade veya kullanarak belirtilen tabanda logaritmasını doğal logaritmasını döndürür. |
| Log10 (num_expr) | 10 tabanında Logaritmik belirtilen sayısal ifadenin değerini döndürür. |
| ROUND (num_expr) | En yakın tamsayı değerine yuvarlanır sayısal bir değer döndürür. |
| TRUNC (num_expr) | En yakın tamsayı değerine kesilmiş sayısal bir değer döndürür. |
| SQRT (num_expr) | Belirtilen sayısal ifadenin kare kökünü döndürür. |
| KARE (num_expr) | Belirtilen sayısal ifade karesini döndürür. |
| POWER (num_expr, num_expr) | Belirtilen sayısal ifade gücünü belirtilen değeri döndürür. |
| OTURUM (num_expr) | Oturum (-1, 0, 1) belirtilen sayısal ifadenin değerini döndürür. |
| ACOS (num_expr) | Kosinüsü belirtilen sayısal ifadesidir radyan cinsinden açı döndürür; arkkosinüsünü olarak da adlandırılır. |
| ASIN (num_expr) | Açının sinüsü belirtilen sayısal ifadesidir radyan cinsinden döndürür. Bu işlev, arksinüsünü olarak da adlandırılır. |
| ATAN (num_expr) | Tanjantı belirtilen sayısal ifadesidir radyan cinsinden açı döndürür. Bu işlev, arktanjantını olarak da adlandırılır. |
| ATN2 (num_expr) | Burada açıyı pozitif x ekseni ve kaynaktan ray noktasına (y, x) arasında radyan cinsinden döndürür x ve y iki belirtilen float ifadelerin değerlerdir. |
| COS (num_expr) | Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının kosinüsünü döndürür. |
| COT (num_expr) | Trigonometrik belirtilen bir açının kotanjantını radyan cinsinden belirtilen bir sayısal ifade döndürür. |
| DEGREES (num_expr) | Karşılık gelen açıyı derece için radyan cinsinden belirtilen bir açı cinsinden döndürür. |
| PI () | PI sayısının sabit değerini döndürür. |
| RADYAN (num_expr) | Derece sayısal bir ifadenin girildiğinde radyan cinsinden döndürür. |
| SIN (num_expr) | Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının sinüsünü döndürür. |
| TAN (num_expr) | Belirtilen ifadedeki giriş ifadesi tanjantını döndürür. |

Aşağıdaki örnekte olduğu gibi sorguları çalıştırabilirsiniz:

```sql
    SELECT VALUE ABS(-4)
```

Sonuç olur:

```json
    [4]
```

### <a name="type-checking-functions"></a>Tür denetleme işlevleri

Bir SQL sorgusu içindeki bir ifadenin türünü kontrol tür denetimi işlevlerini sağlar. Değişken veya bilinmeyen olduklarında özellikleri içinde hareket halindeyken öğeleri türlerini belirlemek için tür denetimi işlevlerini kullanabilirsiniz. Desteklenen yerleşik tür denetimi işlevler tablosu şu şekildedir:

| **Kullanım** | **Açıklama** |
|-----------|------------|
| [IS_ARRAY (ifade)](sql-api-query-reference.md#bk_is_array) | Değer türü bir dizi olup olmadığını gösteren bir Boole değeri döndürür. |
| [IS_BOOL (ifade)](sql-api-query-reference.md#bk_is_bool) | Bir Boolean değer türü olup olmadığını gösteren bir Boole değeri döndürür. |
| [IS_NULL (ifade)](sql-api-query-reference.md#bk_is_null) | Değerin türü null olup olmadığını gösteren bir Boole değeri döndürür. |
| [IS_NUMBER (ifade)](sql-api-query-reference.md#bk_is_number) | Değerin türü, bir sayı olup olmadığını gösteren bir Boole değeri döndürür. |
| [IS_OBJECT (ifade)](sql-api-query-reference.md#bk_is_object) | Değerin türü, bir JSON nesnesi olup olmadığını gösteren bir Boole değeri döndürür. |
| [IS_STRING (ifade)](sql-api-query-reference.md#bk_is_string) | Bir dize değerinin türü olup olmadığını gösteren bir Boole değeri döndürür. |
| [IS_DEFINED (ifade)](sql-api-query-reference.md#bk_is_defined) | Özellik değeri atanıp atanmadığını gösteren bir Boole değeri döndürür. |
| [IS_PRIMITIVE (ifade)](sql-api-query-reference.md#bk_is_primitive) | Değer türü bir dize, sayı, Boole veya null olup olmadığını gösteren bir Boole değeri döndürür. |

Bu işlevleri kullanarak, aşağıdaki örnekte olduğu gibi sorguları çalıştırabilirsiniz:

```sql
    SELECT VALUE IS_NUMBER(-4)
```

Sonuç olur:

```json
    [true]
```

### <a name="string-functions"></a>Dize işlevleri

Aşağıdaki skaler İşlevler, bir dize giriş değeri bir işlem gerçekleştirmek ve bir dize, sayısal veya Boolean değeri döndürür. Yerleşik dize işlevleri tablosu şu şekildedir:

| Kullanım | Açıklama |
| --- | --- |
| [UZUNLUK (str_expr)](sql-api-query-reference.md#bk_length) | Belirtilen dize ifadesinin karakter sayısını döndürür. |
| [CONCAT (str_expr str_expr [, str_expr])](sql-api-query-reference.md#bk_concat) | İki veya daha fazla dize değerlerini birleştirirken sonucu olan bir dize döndürür. |
| [Alt dize (str_expr, num_expr, num_expr)](sql-api-query-reference.md#bk_substring) | Parçası olan bir dize ifadesi döndürür. |
| [STARTSWITH (str_expr, str_expr)](sql-api-query-reference.md#bk_startswith) | Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci başlatır. |
| [ENDSWITH (str_expr, str_expr)](sql-api-query-reference.md#bk_endswith) | Boole döndürüp döndüremeyeceğini belirten döndürür ilk dize ifade olup olmadığını ve ikinci sona erer. |
| [İÇERİR (str_expr, str_expr)](sql-api-query-reference.md#bk_contains) | Döndürür bir Boolean gösteren ikinci ilk dize ifade olup olmadığını içerir. |
| [INDEX_OF (str_expr, str_expr)](sql-api-query-reference.md#bk_index_of) | İkinci dizenin başlangıç konumunu döndürür dize bulunamazsa ilk belirtilen dize ifadesi veya -1 içindeki ifadenin dize. |
| [Sol (str_expr, num_expr)](sql-api-query-reference.md#bk_left) | Belirtilen sayıda karakteri içeren bir dize sol bölümünü döndürür. |
| [SAĞ (str_expr, num_expr)](sql-api-query-reference.md#bk_right) | Belirtilen sayıda karakteri içeren bir dize sağ bölümünü döndürür. |
| [LTRIM (str_expr)](sql-api-query-reference.md#bk_ltrim) | Baştaki boşluklar kaldırdıktan sonra bir dize ifadesi döndürür. |
| [RTRIM (str_expr)](sql-api-query-reference.md#bk_rtrim) | Sonundaki tüm boşlukları kesilmesi sonrasında bir dize ifadesi döndürür. |
| [DÜŞÜK (str_expr)](sql-api-query-reference.md#bk_lower) | Büyük harf karakter verileri küçük harfe dönüştürmenin sonra bir dize ifadesi döndürür. |
| [ÜST (str_expr)](sql-api-query-reference.md#bk_upper) | Küçük harf karakter verileri büyük harfe dönüştürmenin sonra bir dize ifadesi döndürür. |
| [Değiştir (str_expr, str_expr, str_expr)](sql-api-query-reference.md#bk_replace) | Belirtilen dize değeri tüm oluşumlarını başka bir dize değeri ile değiştirir. |
| [Çoğaltma (str_expr, num_expr)](sql-api-query-reference.md#bk_replicate) | Bir dize değeri, belirtilen sayıda yineler. |
| [REVERSE (str_expr)](sql-api-query-reference.md#bk_reverse) | Bir dize değerinin ters sırada döndürür. |

Bu işlevler kullanarak sorguları ailesi döndüren aşağıdaki gibi çalıştırabilirsiniz `id` büyük:

```sql
    SELECT VALUE UPPER(Families.id)
    FROM Families
```

Sonuçlar şu şekildedir:

```json
    [
        "WAKEFIELDFAMILY",
        "ANDERSENFAMILY"
    ]
```

Veya bu örnekte gibi dizeleri birleştirir:

```sql
    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families
```

Sonuçlar şu şekildedir:

```json
    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "Seattle,WA"
    }]
```

Dize işlevleri filtrelemek için WHERE yan tümcesi, gibi aşağıdaki örnekte sonuç olarak da kullanabilirsiniz:

```sql
    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")
```

Sonuçlar şu şekildedir:

```json
    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]
```

### <a name="array-functions"></a>Dizi işlevleri

Aşağıdaki skaler işlevler bir dizi giriş değeri bir işlem gerçekleştirmek ve bir sayısal, Boole değeri veya dizi değeri döndürür. Yerleşik bir dizi işlev bir tablo şu şekildedir:

| Kullanım | Açıklama |
| --- | --- |
| [ARRAY_LENGTH (arr_expr)](sql-api-query-reference.md#bk_array_length) |Belirtilen bir dizi ifadesinin öğelerin sayısını döndürür. |
| [ARRAY_CONCAT (arr_expr arr_expr [, arr_expr])](sql-api-query-reference.md#bk_array_concat) |İki veya daha fazla dizi değerlerini birleştirirken sonucu olan bir dizi döndürür. |
| [ARRAY_CONTAINS (arr_expr, ifade [, bool_expr])](sql-api-query-reference.md#bk_array_contains) |Dizi belirtilen değeri içerip içermediğini gösteren bir Boole değeri döndürür. Tam veya kısmi eşleşme olup olmadığını belirtebilirsiniz. |
| [ARRAY_SLICE (arr_expr num_expr [, num_expr])](sql-api-query-reference.md#bk_array_slice) |Bir dizi ifadesi bölümünü döndürür. |

Dizileri JSON içinde işlemek için dizi işlevlerini kullanın. Örneğin, tüm öğe döndüren bir sorgu işte `id`s bir yerde, `parents` olduğu `Robin Wakefield`: 

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })
```

Sonuç olur:

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Diziden öğeleri eşleştirmek için kısmi bir parçası olarak belirtebilirsiniz. Aşağıdaki sorgu tüm öğesi bulur `id`sahip s `parents` ile `givenName` , `Robin`:

```sql
    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin" }, true)
```

Sonuç olur:

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Sayısını almak için ARRAY_LENGTH kullanan başka bir örnek `children` ailesi başına:

```sql
    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 
```

Sonuçlar şu şekildedir:

```json
    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]
```

### <a name="spatial-functions"></a>Uzamsal İşlevler

Cosmos DB, Jeo-uzamsal sorgulamak için aşağıdaki açık Jeo-uzamsal Consortium (OGC) yerleşik işlevleri destekler: 

| Kullanım | Açıklama |
| --- | --- |
| ST_DISTANCE (point_expr, point_expr) | İki GeoJSON uzaklığı döndürür `Point`, `Polygon`, veya `LineString` ifadeler. |
| T_WITHIN (point_expr, polygon_expr) | Belirten bir Boole ifadesi döndürür olup olmadığını ilk GeoJSON nesne (`Point`, `Polygon`, veya `LineString`) ikinci GeoJSON nesne (`Point`, `Polygon`, veya `LineString`). |
| ST_INTERSECTS (spatial_expr, spatial_expr) | GeoJSON nesnesi belirtilen iki olup olmadığını belirten bir Boole ifadesi döndürür (`Point`, `Polygon`, veya `LineString`) kesişen. |
| ST_ISVALID | Belirten bir Boole değeri döndürür olup belirtilen GeoJSON `Point`, `Polygon`, veya `LineString` ifade geçerlidir. |
| ST_ISVALIDDETAILED | Bir Boole değeri içeren bir JSON değeri döndürür belirtilen GeoJSON `Point`, `Polygon`, veya `LineString` ifade geçerli ve geçersiz ise, bir dize değeri olarak nedeni. |

Uzamsal veriler yakınlık sorguları gerçekleştirmek için uzamsal işlevleri kullanabilirsiniz. Örneğin, ST_DISTANCE yerleşik işlevini kullanarak belirli bir konumun içinde 30 KM ailesi tüm öğeleri döndüren bir sorgu aşağıdadır:

```sql
    SELECT f.id
    FROM Families f
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000
```

Sonuç olur:

```json
    [{
      "id": "WakefieldFamily"
    }]
```

Cosmos DB'de Jeo-uzamsal destek hakkında daha fazla bilgi için bkz. [Azure Cosmos DB Jeo-uzamsal verilerle çalışmaya](geospatial.md). 

## <a name="parameterized-queries"></a>Parametreli sorgular

Cosmos DB, tanıdık gösterimi ' @'ile ifade edilen parametrelerle sorguları destekler. Parametreli SQL sağlam işleme ve kullanıcı girdisini kaçış sağlar ve SQL ekleme üzerinden verilerin yanlışlıkla açığa çıkmaya engeller.

Örneğin, alan bir sorgu yazabileceğiniz `lastName` ve `address.state` parametre olarak ve çeşitli değerleri için yürütme `lastName` ve `address.state` kullanıcı girişini temel alarak.

```sql
    SELECT *
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState
```

Ardından, aşağıdaki gibi bir JSON sorgusu olarak Cosmos DB'ye bu istek gönderebilirsiniz:

```sql
    {
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",
        "parameters": [
            {"name": "@lastName", "value": "Wakefield"},
            {"name": "@addressState", "value": "NY"},
        ]
    }
```

Aşağıdaki örnekte, ilk bağımsız değişkeni parametreli bir sorgu ile ayarlar: 

```sql
    {
        "query": "SELECT TOP @n * FROM Families",
        "parameters": [
            {"name": "@n", "value": 10},
        ]
    }
```

Parametre değerleri geçerli bir JSON olabilir: dizeler, sayılar ve Boole değerlerini, null, hatta diziler veya JSON iç içe geçmiş. Cosmos DB, şemasız olduğu parametreleri karşı herhangi bir tür doğrulanmış değil.

## <a id="JavaScriptIntegration"></a>JavaScript tümleştirme

Azure Cosmos DB bir programlama modeli doğrudan kapsayıcılarında JavaScript tabanlı uygulama mantığını yürütmek için saklı yordamları ve Tetikleyicileri kullanarak sağlar. Bu model destekler:

* Yüksek performanslı işlem CRUD işlemleri ve veritabanı altyapısının içinde JavaScript çalışma zamanı kapsamlı tümleştirme sayesinde, bir kapsayıcı öğeleri karşı sorgular.
* Doğal bir modelleme denetim akışı, değişken kapsamı, atama ve özel durum işleme temelleri veritabanı işlemleri ile tümleştirilmesi. 

Azure Cosmos DB JavaScript tümleştirmesi hakkında daha fazla bilgi için bkz. [JavaScript sunucu tarafı API'si](#JavaScriptServerSideApi) bölümü.

### <a name="operator-evaluation"></a>Değerlendirme işleci

Bir JSON veritabanı olan da cosmos DB, parallels JavaScript işleçleri ve değerlendirme semantiği çizer. Cosmos DB, JSON desteği açısından JavaScript sematiğini korumak dener, ancak bazı durumlarda işlemi değerlendirme farklılık göstermesi.

API değerleri veritabanından alır kadar SQL API'SİNDE aksine geleneksel SQL değer türleri bilinen değildir. Verimli bir şekilde sorguları yürütmek için işleçlerin en katı tür gereksinimleri vardır.

JavaScript, SQL API'si örtük dönüştürmelerin gerçekleştirmez. Örneğin, bir sorgu ister `SELECT * FROM Person p WHERE p.Age = 21` eşleşen öğeleri içeren bir `Age` özellik değeri `21`. Tüm eşleşmiyorsa diğer öğe ayarlanmış `Age` özelliği, büyük olasılıkla sınırsız çeşitlemeleri gibi eşleşen `twenty-one`, `021`, veya `21.0`. Bu JavaScript, burada dize değerleri örtük olarak cast sayılara bir operatöre, örneğin dayanan ile karşılaştırır: `==`. SQL API'si bu davranışı çok verimli dizin eşleştirmek için önemlidir.

## <a id="ExecutingSqlQueries"></a>SQL sorgusu yürütme

HTTP/HTTPS istekleri yapabilen herhangi bir dilde, Cosmos DB REST API'si çağırabilirsiniz. Cosmos DB ayrıca .NET, Node.js, JavaScript ve Python programlama dilde programlama kitaplıkları sunar. SQL ile sorgulama tüm REST API ve kitaplıkları destekleyen ve .NET SDK'yı da destekler [LINQ sorgusu](#Linq).

Aşağıdaki örnekler bir sorgu oluşturun ve Cosmos DB veritabanı hesabını karşı gönderme işlemini göstermektedir.

### <a id="RestAPI"></a>REST API

Cosmos DB, HTTP üzerinden açık bir RESTful programlama modeli sunar. Kaynak modeli, bir veritabanı hesabı altında kaynak kümesinden oluşur, bir Azure aboneliği sağlar. Veritabanı hesabı kümesinden oluşur *veritabanları*, her biri birden çok içerebilir *kapsayıcıları*, hangi sırayla içeren *öğeleri*, UDF'ler ve diğer kaynak türleri. Her Cosmos DB mantıksal ve kararlı bir URI kullanılarak adreslenebilir bir kaynaktır. Bir kaynak kümesi olarak adlandırılan bir *akış*. 

HTTP fiilleri temel etkileşim modelidir bu kaynaklarla `GET`, `PUT`, `POST`, ve `DELETE`, kendi standart ınterpretations ile. Kullanım `POST` yeni bir kaynak oluşturmak için bir saklı yordamı yürütme veya bir Cosmos DB sorgusu yayınlanacak. Sorgu her zaman salt okunur yan etkileri olan işlemlerdir.

Aşağıdaki örneklerde gösterildiği bir `POST` örnek öğeleri bir API SQL sorgusu için. Sorgu üzerinde JSON basit bir filtre sahip `name` özelliği. `x-ms-documentdb-isquery` Ve Content-Type: `application/query+json` üstbilgileri belirtmek olduğu sorgu işlemi. Değiştirin `mysqlapicosmosdb.documents.azure.com:443` ile Cosmos DB hesabınız için URI.

```json
    POST https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",
        "parameters": [
            {"name": "@familyId", "value": "AndersenFamily"}
        ]
    }
```

Sonuçlar şu şekildedir:

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"Seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

İleri, daha karmaşık bir sorgu birleştirme sonucu birden çok sonuç döndürür:

```json
    POST https://https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT
                     f.id AS familyName,
                     c.givenName AS childGivenName,
                     c.firstName AS childFirstName,
                     p.givenName AS petName
                  FROM Families f
                  JOIN c IN f.children
                  JOIN p in c.pets",
        "parameters": [] 
    }
```

Sonuçlar şu şekildedir: 

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Bir sorgunun sonuçlarını tek bir sayfasına sığdıramazsanız REST API aracılığıyla bir devamlılık belirteci döndürür. `x-ms-continuation-token` yanıtı üstbilgisi. İstemcileri, sonraki sonuçları üst bilgisi dahil olmak üzere sonuçlarını sayfalandırma. İle ilgili sayfa başına sonuç sayısı de denetleyebilirsiniz `x-ms-max-item-count` sayı başlığı. 

Bir sorgu sayısı gibi bir toplama işlevi varsa, sorgu sayfası yalnızca bir sayfalık sonuç kısmen toplanan bir değer döndürebilir. İstemciler, son sonuçlar için bu sonuçlar üzerinde ikinci düzey toplama gerçekleştirmeniz gerekir. Örneğin, her bir sayfayı toplam sayısını döndürmek için döndürülen sayıları üzerinden toplayın.

Sorgular için veri tutarlılık ilkesi yönetmek için kullandığınız `x-ms-consistency-level` tüm REST API istekleri olduğu gibi üst bilgisi. Oturum tutarlılığı ayrıca gerektirir en son Yankı `x-ms-session-token` sorgu istekteki tanımlama bilgisi üstbilgisi. Sorgulanan kapsayıcının dizin oluşturma ilkesini tutarlılığını sorgu sonuçlarını da etkileyebilir. İle dizin oluşturma ilkesi ayarları kapsayıcılar için varsayılan dizin her zaman geçerli öğe içeriğiyle ve sorgu sonuçları için veri seçilen tutarlılık eşleşen. Daha fazla bilgi için [Azure Cosmos DB tutarlılık düzeyleri][consistency-levels].

Belirtilen sorgu kapsayıcı üzerindeki yapılandırılmış dizin oluşturma ilkesini destekleyemiyorsa, Azure Cosmos DB sunucusu 400 "Bad Request" döndürür. Bu hata iletisini sorgular için açıkça dizine elmadan hariç yollarla döndürür. Belirtebileceğiniz `x-ms-documentdb-query-enable-scan` dizin kullanılamadığında bir tarama gerçekleştirmek sorgu izni başlığı.

Ayarlayarak, sorgu yürütme ayrıntılı ölçümleri alabilirsiniz `x-ms-documentdb-populatequerymetrics` başlığına `true`. Daha fazla bilgi için [Azure Cosmos DB için SQL sorgu ölçümleri](sql-api-query-metrics.md).

### <a id="DotNetSdk"></a>C#(.NET SDK)

LINQ hem SQL .NET SDK'yı destekleyen sorgulama. Aşağıdaki örnek, .NET ile önceki filtre sorgusu gerçekleştirmeye gösterilmektedir:

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(containerLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(containerLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(containerLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Aşağıdaki örnek, her öğenin eşitlik için iki özellik karşılaştırır ve anonim projeksiyonlar kullanır.

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family
        FROM Families f
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(containerLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(containerLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Sonraki örnek, birleşimler, LINQ ifade gösterir `SelectMany`.

```csharp
    foreach (var pet in client.CreateDocumentQuery(containerLink,
          @"SELECT p
            FROM Families f
                 JOIN c IN f.children
                 JOIN p in c.pets
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions:
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

.NET istemci otomatik olarak sorgu sonuçlarında tüm sayfaları aracılığıyla yinelenir `foreach` , yukarıdaki örnekte gösterildiği gibi engeller. Sorgu seçenekleri de kullanıma sunulan [REST API](#RestAPI) bölüm de mevcuttur .NET SDK kullanarak `FeedOptions` ve `FeedResponse` sınıfları `CreateDocumentQuery` yöntemi. Sayfa sayısı kullanarak denetleyebilirsiniz `MaxItemCount` ayarı.

Disk belleği oluşturarak de açıkça denetleyebilirsiniz `IDocumentQueryable` kullanarak `IQueryable` okuyarak sonra nesne, `ResponseContinuationToken` değerleri ve bunları geçirmeden geri olarak `RequestContinuationToken` içinde `FeedOptions`. Ayarlayabileceğiniz `EnableScanInQuery` sorgu tarafından yapılandırılan dizin oluşturma ilkesini sunulmaması halinde taramaları etkinleştirmek için. Bölümlenmiş kapsayıcılar için kullanabileceğiniz `PartitionKey` Azure Cosmos DB otomatik olarak bu sorgu metni ayıklayabilir olsa da tek bir bölüm karşı sorgu çalıştırmak için. Kullanabileceğiniz `EnableCrossPartitionQuery` birden çok bölüm karşı sorguları çalıştırmak için.

Sorgular sayesinde daha fazla .NET örnekleri için bkz. [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet) github'da.

### <a id="JavaScriptServerSideApi"></a>JavaScript sunucu tarafı API'si

Cosmos DB, JavaScript tabanlı uygulama mantığını saklı yordamları ve Tetikleyicileri kullanarak kapsayıcılarında doğrudan yürütmek için bir programlama modeli sağlar. Kapsayıcı düzeyinde kayıtlı JavaScript mantığının ardından öğelerinde belirtilen kapsayıcının içinde çevresel ACID işlemlerini sarmalanmış veritabanı işlemleri verebilir.

Aşağıdaki örnek nasıl kullanılacağını gösterir `queryDocuments` JavaScript Server sorgularından yapmak için API iç saklı yordamları ve Tetikleyicileri:

```javascript
    function findName(givenName, familyName) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { givenName: givenName, familyName: familyName },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by familyName
                var filterQuery = "SELECT * from root r WHERE r.familyName = 'Wakefield'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the familyName for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].familyName = "Robin Wakefield";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a id="Linq"></a>LINQ to SQL API'si

LINQ sorguları nesne akışlarında olarak hesaplama ifade bir .NET programlama modelidir. Cosmos DB, JSON ve .NET nesneleri ve LINQ sorguları kümesini bir eşleme Cosmos DB sorgular arasındaki bir dönüştürmenin kolaylaştırarak LINQ ile arabirim oluşturmak için bir istemci tarafı kitaplık sağlar.

Aşağıdaki diyagramda, Cosmos DB kullanarak LINQ sorgularını destekleyen mimarisi gösterilmektedir. Oluşturabileceğiniz Cosmos DB İstemcisi'ni kullanarak bir `IQueryable` nesnesini doğrudan Cosmos DB sorgu sağlayıcısı sorgular ve Cosmos DB bir sorguda LINQ sorgusu çevirir. Bir JSON biçiminde sonuç kümesini alır. Cosmos DB sunucuya sorgu geçirin. JSON seri durumdan çıkarıcı sonuçları .NET nesneleri istemci tarafında bir akışa dönüştürür.

![SQL söz dizimi, JSON sorgu dili, veritabanı kavramlarını ve SQL sorguları - SQL API'sini kullanarak LINQ sorgularını destekleyen mimarisi][1]

### <a name="net-and-json-mapping"></a>.NET ve JSON eşleme

.NET nesneleri ve JSON öğeleri arasındaki eşleme, doğal bir. Alan adı burada eşlenen bir JSON nesnesi, her veri üyesi alanı eşlendiği *anahtarı* nesne ve değer yinelemeli olarak parçası eşlendiğini *değer* nesnenin bir parçası. Aşağıdaki kod haritaları `Family` sınıfı bir JSON öğesine ve ardından oluşturan bir `Family` nesnesi:

```csharp
    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };

    public struct Parent
    {
        public string familyName;
        public string givenName;
    };

    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };

    public class Pet
    {
        public string givenName;
    };

    public class Address
    {
        public string state;
        public string county;
        public string city;
    };

    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };
```

Yukarıdaki örnekte, aşağıdaki JSON öğesi oluşturur:

```json
    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                "givenName": "Jesse",
                "gender": "female",
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller",
              "givenName": "Lisa",
              "gender": "female",
              "grade": 8
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };
```

### <a name="linq-to-sql-translation"></a>LINQ to SQL çeviri

Cosmos DB sorgu sağlayıcısı bir en iyi çaba eşleme bir Cosmos DB SQL sorgusu bir LINQ Sorgu gerçekleştirir. Aşağıdaki açıklama LINQ temel olarak bilindiğini varsayar.

Sorgu sağlayıcısı tür sistemi yalnızca JSON ilkel türlerini destekler: sayısal, Boole, dize ve null. 

Sorgu sağlayıcısı aşağıdaki skaler ifadelerin destekler:

- Sorgu değerlendirme zamanında sabit değerler ilkel veri türleri dahil olmak üzere sabit değerler.
  
- Bir nesne veya dizi öğesi özelliğine başvuran özelliği/dizi dizini ifadeleri. Örneğin:
  
  ```
    family.Id;
    family.children[0].familyName;
    family.children[0].grade;
    family.children[n].grade; //n is an int variable
  ```
  
- Sayısal ve Boolean değerleri ortak aritmetik ifadeler de dahil olmak üzere aritmetik ifadeler. Tam liste için bkz. [Azure Cosmos DB SQL belirtimi](https://go.microsoft.com/fwlink/p/?LinkID=510612).
  
  ```
    2 * family.children[0].grade;
    x + y;
  ```
  
- Bazı sabit dize değeri bir dize değeri karşılaştırma içeren dize karşılaştırma ifadeleri.  
  
  ```
    mother.familyName == "Wakefield";
    child.givenName == s; //s is a string variable
  ```
  
- Bileşik değer türü veya anonim türdeki bir nesne veya bu tür nesneler dizisi döndürür oluşturma ifadeleri, nesne veya dizi. Bu değerleri iç içe yerleştirebilirsiniz.
  
  ```
    new Parent { familyName = "Wakefield", givenName = "Robin" };
    new { first = 1, second = 2 }; //an anonymous type with two fields  
    new int[] { 3, child.grade, 5 };
  ```

### <a id="SupportedLinqOperators"></a>Desteklenen LINQ işleçleri

SQL .NET SDK'sı ile dahil LINQ sağlayıcısı aşağıdaki işleçleri destekler:

- **Seçin**: Projeksiyonlar nesne oluşturmayı da dahil olmak üzere SQL SEÇMEK için çevir.
- **Burada**: Filtreler SQL WHERE için çevir ve Destek arasında çeviri `&&`, `||`, ve `!` SQL işleçlere değiştirme
- **SelectMany**: SQL JOIN yan tümcesine dizi geriye doğru izleme sağlar. Zincir veya iç içe dizi öğeleri üzerinde filtre ifadeleri için kullanın.
- **OrderBy** ve **OrderByDescending**: Sıralama ölçütü ASC veya DESC için çevir.
- **Sayısı**, **toplam**, **Min**, **Max**, ve **ortalama** işleçleri toplama ve zaman uyumsuz eşdeğerlerine **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, ve **AverageAsync**.
- **CompareTo**: Aralık karşılaştırmalar çevirir. . NET'te karşılaştırılabilir değilseniz bu yana dizeler için yaygın olarak kullanılır.
- **Ele**: Bir sorgunun sonuçlarını sınırlamak için SQL en üste çevirir.
- **Matematik işlevleri**: Çeviri net'ten destekler `Abs`, `Acos`, `Asin`, `Atan`, `Ceiling`, `Cos`, `Exp`, `Floor`, `Log`, `Log10`, `Pow`, `Round`, `Sign`, `Sin`, `Sqrt`, `Tan`, ve `Truncate` eşdeğer SQL yerleşik işlevleri.
- **Dize işlevleri**: Çeviri net'ten destekler `Concat`, `Contains`, `Count`, `EndsWith`,`IndexOf`, `Replace`, `Reverse`, `StartsWith`, `SubString`, `ToLower`, `ToUpper`, `TrimEnd`, ve `TrimStart` eşdeğer SQL yerleşik işlevleri.
- **Dizi işlevleri**: Çeviri net'ten destekler `Concat`, `Contains`, ve `Count` eşdeğer SQL yerleşik işlevleri.
- **Jeo-uzamsal uzantı işlevleri**: Saptama yöntemleri çevrilmesi destekler `Distance`, `IsValid`, `IsValidDetailed`, ve `Within` eşdeğer SQL yerleşik işlevleri.
- **Kullanıcı tanımlı işlev uzantı işlevi**: Çeviri saplama yönteminden destekler `UserDefinedFunctionProvider.Invoke` karşılık gelen kullanıcı tanımlı işlev.
- **Çeşitli**: Çeviri destekler `Coalesce` ve Koşullu işleçler. Çevirebilir `Contains` dize İÇERİYOR, ARRAY_CONTAINS veya bağlama bağlı olarak SQL IN.

### <a name="sql-query-operators"></a>SQL sorgu işleçleri

Aşağıdaki örnekler nasıl bazı standart LINQ Sorgu işleçleri için Cosmos DB sorgular Çevir gösterir.

#### <a name="select-operator"></a>İşleç seçin

Söz dizimi `input.Select(x => f(x))`burada `f` skaler bir ifade.

**Örnek 1 bir işleç seçin:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.parents[0].familyName);
  ```
  
- **SQL** 
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
    ```
  
**Örnek 2 bir işleç seçin:** 

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.children[0].grade + c); // c is an int variable
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.children[0].grade + c
      FROM Families f
  ```
  
**Örnek 3 bir işleç seçin:**

- **LINQ lambda ifadesi**
  
  ```csharp
    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });
  ```
  
- **SQL** 
  
  ```sql
      SELECT VALUE {"name":f.children[0].familyName,
                    "grade": f.children[0].grade + 3 }
      FROM Families f
  ```

#### <a name="selectmany-operator"></a>SelectMany işleci

Söz dizimi `input.SelectMany(x => f(x))`burada `f` bir kapsayıcı türü döndüren bir skaler ifade.

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.children);
  ```
  
- **SQL**

  ```sql
      SELECT VALUE child
      FROM child IN Families.children
  ```

#### <a name="where-operator"></a>Burada işleci

Söz dizimi `input.Where(x => f(x))`burada `f` bir Boole değeri döndüren bir skaler ifade.

**Burada işleç, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(family=> family.parents[0].familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```
  
**Burada işleç, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(
          family => family.parents[0].familyName == "Wakefield" &&
          family.children[0].grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
      AND f.children[0].grade < 3
  ```

### <a name="composite-sql-queries"></a>Bileşik SQL sorguları

Daha güçlü sorgular oluşturmak için yukarıdaki işleçleri oluşturabilirsiniz. Cosmos DB, iç içe geçmiş kapsayıcılar desteklediğinden, birleştirme veya iç içe birleşim.

#### <a name="concatenation"></a>Birleştirme

Söz dizimi `input(.|.SelectMany())(.Select()|.Where())*`. Birleştirilmiş bir sorgu ile isteğe bağlı olarak başlatabilirsiniz `SelectMany` sorgu, birden fazla izlenen `Select` veya `Where` işleçleri.

**Birleştirme, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family=>family.parents[0])
          .Where(familyName == "Wakefield");
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```

**Birleştirme, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(family => family.children[0].grade > 3)
          .Select(family => family.parents[0].familyName);
  ```

- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
      WHERE f.children[0].grade > 3
  ```

**Birleştirme, örnek 3:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => new { grade=family.children[0].grade}).
          Where(anon=> anon.grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE ({grade: f.children[0].grade}.grade > 3)
  ```

**Birleştirme, örnek 4:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.parents)
          .Where(parent => parents.familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM p IN Families.parents
      WHERE p.familyName = "Wakefield"
  ```

#### <a name="nesting"></a>İç içe geçirme

Söz dizimi `input.SelectMany(x=>x.Q())` burada `Q` olduğu bir `Select`, `SelectMany`, veya `Where` işleci.

İç içe bir sorgu iç sorgu dış kapsayıcı her öğeye uygulanır. Bir önemli özelliği, iç sorgu alanları, kendi kendine birleşme gibi dış kapsayıcıdaki öğelerin başvurabilir.

**İç içe geçme, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family=>
          family.parents.Select(p => p.familyName));
  ```

- **SQL**
  
  ```sql
      SELECT VALUE p.familyName
      FROM Families f
      JOIN p IN f.parents
  ```

**İç içe geçme, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family =>
          family.children.Where(child => child.familyName == "Jeff"));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = "Jeff"
  ```

**İç içe geçme, örnek 3:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.children.Where(
          child => child.familyName == family.parents[0].familyName));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = f.parents[0].familyName
  ```

## <a id="References"></a>Başvuruları

- [Azure Cosmos DB SQL belirtimi](https://go.microsoft.com/fwlink/p/?LinkID=510612)
- [ANSI SQL 2011](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
- [JSON](https://json.org/)
- [JavaScript belirtimi](https://www.ecma-international.org/publications/standards/Ecma-262.htm) 
- [LINQ](/previous-versions/dotnet/articles/bb308959(v=msdn.10)) 
- Graefe, Goetz. [Sorgu büyük veritabanları için değerlendirme tekniklerini](https://dl.acm.org/citation.cfm?id=152611). *Anketler bilgi işlem ACM* 25 yok. 2 (1993).
- Graefe, G. "Sorgu iyileştirme basamaklar framework." *IEEE veri Müh Bull.* 18 yok. 3 (1995).
- Lu, Ooi, Bronz. "Sorgu paralel ilişkisel veritabanı sistemleri işleme." *IEEE bilgisayarda topluluğu tuşuna* (1994).
- Olston, Christopher, Benjamin Reed, Utkarsh Srivastava, Ravi Kumar ve Andrew Tomkins. "Pig Latin: Bir Not şekilde yabancı dil veri işleme için." *SIGMOD* (2008).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye giriş][introduction]
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Azure Cosmos DB tutarlılık düzeyleri][consistency-levels]

[1]: ./media/how-to-sql-query/sql-query1.png
[introduction]: introduction.md
[consistency-levels]: consistency-levels.md
