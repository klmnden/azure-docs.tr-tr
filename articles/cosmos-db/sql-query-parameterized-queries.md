---
title: Azure Cosmos DB'de parametreli sorgular
description: Parametreli SQL sorguları hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: 2bfc22346c1dd43d7d3c2937ffc286e48ae774d0
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342841"
---
# <a name="parameterized-queries-in-azure-cosmos-db"></a>Azure Cosmos DB'de parametreli sorgular

Cosmos DB, tanıdık gösterimi ' @'ile ifade edilen parametrelerle sorguları destekler. Parametreli SQL sağlam işleme ve kullanıcı girdisini kaçış sağlar ve SQL ekleme üzerinden verilerin yanlışlıkla açığa çıkmaya engeller.

## <a name="examples"></a>Örnekler

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


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Belge verilerini modelleme](modeling-data.md)
