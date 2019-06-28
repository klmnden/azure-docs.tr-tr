---
title: Azure cosmos DB'de diğer ad kullanımı
description: Azure Cosmos DB SQL sorgularında yumuşatma değerler hakkında bilgi edinin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: mjbrown
ms.openlocfilehash: e532fb7180af8a21de6ae9a2e4d798abd9e93e7b
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342914"
---
# <a name="aliasing-in-azure-cosmos-db"></a>Azure cosmos DB'de diğer ad kullanımı

Açıkça diğer ad değer sorguları içinde kullanabilirsiniz. Bir sorgu aynı ada sahip iki özellik varsa, diğer ad kullanımı birini veya her ikisini özelliklerini öngörülen sonucunda disambiguated şekilde yeniden adlandırmak için kullanın.

## <a name="examples"></a>Örnekler

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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [SELECT yan tümcesi](sql-query-select.md)
- [FROM yan tümcesi](sql-query-from.md)
