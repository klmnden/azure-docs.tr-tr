---
title: Azure Cosmos DB için SQL sorgu işleçleri
description: Azure Cosmos DB için SQL işleçleri hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: eecc1522f8c260286c7dd7fc4c2e58d5d8caa8fb
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342493"
---
# <a name="operators-in-azure-cosmos-db"></a>Azure cosmos DB'de işleçleri

Bu makalede Azure Cosmos DB tarafından desteklenen çeşitli işleçler ayrıntılı olarak açıklanmaktadır.

## <a name="equality-and-comparison-operators"></a>Eşitlik ve Karşılaştırma işleçleri

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

## <a name="logical-and-or-and-not-operators"></a>Mantıksal (AND, OR ve NOT) işleçleri

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


## <a name="-operator"></a>* işleci

Özel işleç * olduğu gibi tüm öğe projeleri. Kullanıldığında yansıtılan tek alan olması gerekir. Bir sorgu ister `SELECT * FROM Families f` geçerlidir, ancak `SELECT VALUE * FROM Families f` ve `SELECT *, f.id FROM Families f` geçerli değildir.

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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [anahtar sözcükler](sql-query-keywords.md)
- [SELECT yan tümcesi](sql-query-select.md)
