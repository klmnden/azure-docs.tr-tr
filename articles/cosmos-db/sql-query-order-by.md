---
title: Azure Cosmos DB'de ORDER BY yan tümcesi
description: ORDER BY tümcesi hakkında Azure Cosmos DB için öğrenin. SQL, bir Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: d0a1ed33d5848c3ed8d5f83af8b320d77fe0dc65
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342792"
---
# <a name="order-by-clause"></a>ORDER BY yan tümcesi

İsteğe bağlı ORDER BY yan tümcesi, sorgu tarafından döndürülen sonuçları sıralama düzenini belirtir.

## <a name="syntax"></a>Sözdizimi
  
```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
```  

## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<sort_specification>`  
  
   Bir özellik ya da üzerinde sorgu sonucu kümesini sıralama yapılacak ifade belirtir. Sıralama sütununu bir ad veya özelliği diğer adı belirtilebilir.  
  
   Birden çok özellikler belirtilebilir. Özellik adlarının benzersiz olması gerekir. Kuruluş sıralanmış sonuç kümesinin özellikleri Sırala ORDER BY yan tümcesinde sırasını tanımlar. Diğer bir deyişle, sonuç kümesi ilk özelliği tarafından sıralanır ve ardından bu sıralı liste ikinci özelliği vb. göre sıralanır.  
  
   ORDER BY yan tümcesinde başvurulan özellik adlarını özelliği seçim listesinde veya FROM yan tümcesi olmadan tüm belirsizlikleri belirtilen koleksiyonda tanımlanan bir özellik karşılık gelmelidir.  
  
- `<sort_expression>`  
  
   Bir veya daha fazla özellikleri ya da, sorgu sonucu kümesini sıralama ifadelerini belirtir.  
  
- `<scalar_expression>`  
  
   Bkz: [skaler ifadelerin](sql-query-scalar-expressions.md) ayrıntıları bölümü.  
  
- `ASC | DESC`  
  
   Belirtilen sütundaki değerleri artan veya azalan olarak sıralanması gerektiğini belirtir. ASC en düşük değerden yüksek değer sıralar. DESC en yüksek değerden en düşük değere doğru sıralar. ASC varsayılan sıralama sırasıdır. Null değerler, en düşük olası değerler kabul edilir.  
  
## <a name="remarks"></a>Açıklamalar  
  
   ORDER BY yan tümcesi, dizin oluşturma ilkesini sıralanan alanları için dizin eklemenizi gerektirir. Azure Cosmos DB sorgu çalışma zamanı, bir özellik adı değil, hesaplanan özellikler adlarla ve sıralamayı destekler. Azure Cosmos DB, birden çok ORDER BY özelliklerini destekler. ORDER BY birden çok özelliklerle bir sorgu çalıştırmak için tanımlamalıdır bir [bileşik dizin](index-policy.md#composite-indexes) sıralanan alanları.

## <a name="examples"></a>Örnekler

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

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [SELECT yan tümcesi](sql-query-select.md)
- [UZAKLIK sınırlama yan tümcesi](sql-query-offset-limit.md)
