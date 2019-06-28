---
title: Azure Cosmos DB'de toplama işlevleri
description: SQL toplama işlevi söz dizimi hakkında Azure Cosmos DB için öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: a6937e9e811ea8e44eda6f2bcb5d2c7d78db4934
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342940"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Azure Cosmos DB'de toplama işlevleri

Toplama işlevleri, SELECT yan tümcesinde değerleri kümesi üzerinde bir hesaplama gerçekleştirmek ve tek bir değer döndürür. Örneğin, aşağıdaki sorgu öğelerin sayısını döndürür `Families` kapsayıcı:

## <a name="examples"></a>Örnekler

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

## <a name="types-of-aggregate-functions"></a>Toplama işlevleri türleri

SQL API'si aşağıdaki toplama işlevleri destekler. Toplam ve ortalama sayısal değerlerle çalışır ve sayısı, en az ve en yüksek sayılar, dizeler, Boole değerleri ve null değerlere çalışır.

| İşlev | Açıklama |
|-------|-------------|
| COUNT | İfade öğe sayısını döndürür. |
| SUM   | İfadedeki tüm değerlerin toplamını döndürür. |
| MIN   | İfadedeki en küçük değeri döndürür. |
| MAX   | İfadedeki en büyük değeri döndürür. |
| AVG   | İfadedeki değerlerin ortalamasını döndürür. |

Ayrıca, bir dizi yineleme sonuçları üzerinde de toplayabilirsiniz.

> [!NOTE]
> Azure portalında Veri Gezgini içinde toplama sorguları kısmi sonuçlar üzerinde yalnızca bir sorgu sayfası toplanabilir. SDK, tüm sayfalara tek bir toplu değer oluşturur. Kod kullanarak toplama sorguları gerçekleştirmek için .NET SDK'sı 1.12.0, .NET Core SDK'sı 1.1.0 veya Java SDK'sı 1.9.5 gerekir veya üzeri.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye giriş](introduction.md)
- [Sistem işlevleri](sql-query-system-functions.md)
- [Kullanıcı tanımlı işlevler](sql-query-udfs.md)