---
title: Azure Cosmos DB'de UZAKLIĞI sınırı yan tümcesi
description: Azure Cosmos DB için UZAKLIK sınırlama yan tümcesi hakkında öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 60ac28c80e9f7cc72f4d6005c12cb5f68671341e
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342494"
---
# <a name="offset-limit-clause"></a>UZAKLIK sınırlama yan tümcesi

UZAKLIK sınırlama yan tümcesi bazı sayısı değerleri sorgudan Al atlamak için isteğe bağlı bir yan tümcesi ' dir. UZAKLIK sayımı ve sınırı sayısı sınırı UZAKLIĞI yan tümcesinde gereklidir.

UZAKLIK sınırı, ORDER BY yan tümcesi ile birlikte kullanıldığında, sonuç kümesi Atla yaparak oluşturulur ve sıralı değerlerine gerçekleştirin. ORDER BY yan tümce kullandıysanız, değer belirleyici bir sırada neden olur.

## <a name="syntax"></a>Sözdizimi
  
```sql  
OFFSET <offset_amount> LIMIT <limit_amount>
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler

- `<offset_amount>`

   Sorgu sonuçları atlamalısınız öğeleri tamsayı sayısını belirtir.

- `<limit_amount>`
  
   Sorgu sonuçları içermelidir bir öğe tamsayı sayısını belirtir

## <a name="remarks"></a>Açıklamalar
  
  UZAKLIK sayımı hem sınırı sayısı sınırı UZAKLIĞI yan tümcesinde gerekir. İsteğe bağlı ise `ORDER BY` yan tümcesi kullanıldığında, sonuç kümesi sıralı değerleri Atla yaparak üretilir. Aksi takdirde, sorgu bir sabit değerlerin sırasını döndürür. Şu anda yalnızca tek bir bölüm içinde sorguları için bu yan tümce desteklenir, bölümler arası sorgu henüz bunu desteklemez.

## <a name="examples"></a>Örnekler

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

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [SELECT yan tümcesi](sql-query-select.md)
- [ORDER BY yan tümcesi](sql-query-order-by.md)
