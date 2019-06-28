---
title: Azure Cosmos DB SQL anahtar sözcükleri
description: SQL anahtar sözcükleri hakkında Azure Cosmos DB için öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: c9024f120e0a55162a1f6dba0cd9cbda97f5eebc
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342967"
---
# <a name="keywords-in-azure-cosmos-db"></a>Azure cosmos DB anahtar sözcükleri
Bu makalede, Azure Cosmos DB SQL sorgularında kullanılan anahtar sözcükler ayrıntıları.

## <a name="between"></a>ARASINDA

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

## <a name="distinct"></a>DISTINCT

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
## <a name="in"></a> GİRİŞ

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

SQL API'si için destek sağlar [JSON diziler yineleme](sql-query-object-array.md#Iteration), FROM kaynak içinde anahtar sözcük aracılığıyla eklenen yeni bir yapısı ile. 

## <a name="top"></a>SAYFANIN ÜSTÜ

İlk üst anahtar sözcüğü döner `N` tanımlanmamış bir sırada sorgu sonuç sayısı. İlk sonuçlarını sınırlamak için en iyi uygulama, üst ile ORDER BY yan tümcesi kullanın `N` sıralı değerleri sayısı. Bu iki yan tümceyi birleştirme üst etkiler, satırları tahmin edilebilir bir biçimde belirtmek için tek yoludur.

Aşağıdaki örnekte olduğu gibi sabit bir değer ile veya bir değişken değerle parametreli sorgular kullanma üst kullanabilirsiniz.

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

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Birleşimler](sql-query-join.md)
- [Alt sorgular](sql-query-subquery.md)