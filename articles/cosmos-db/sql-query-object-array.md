---
title: Diziler ve Azure Cosmos DB'de nesneler ile çalışma
description: Dizi ve nesne oluşturma SQL söz dizimi için Azure Cosmos DB hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: tisande
ms.openlocfilehash: 338f3b51edf38d20a963992e121b7e2dbd0c6873
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342725"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Diziler ve Azure Cosmos DB'de nesneler ile çalışma

Bir anahtar Azure Cosmos DB SQL API'sine dizi ve nesne oluşturma özelliğidir.

## <a name="arrays"></a>Diziler

Diziler, aşağıdaki örnekte gösterildiği gibi oluşturabilirsiniz:

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

Ayrıca [dizi ifadesi](sql-query-subquery.md#array-expression) dizisinden oluşturulacağını [sorgunun](sql-query-subquery.md) sonuçları. Bu sorgu alt öğeleri bir dizideki tüm farklı verilen adlarını alır.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

## <a id="Iteration"></a>Yineleme

SQL API'si aracılığıyla eklenen yeni bir yapısı ile JSON diziler yineleme için destek sağlar. [anahtar SÖZCÜĞÜ](sql-query-keywords.md#in) FROM kaynak. Aşağıdaki örnekte:

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

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Birleşimler](sql-query-join.md)