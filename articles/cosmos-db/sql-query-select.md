---
title: Azure Cosmos DB'de SELECT yan tümcesi
description: Azure Cosmos DB için SQL SELECT yan tümcesi hakkında öğrenin. SQL, bir Azure Cosmos DB JSON sorgu dili olarak kullanın.
author: ginarobinson
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: girobins
ms.openlocfilehash: 84d0212f7f212b4554b506726e027fe51f795eea
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342627"
---
# <a name="select-clause"></a>SELECT yan tümcesi

SELECT yan tümcesi ve isteğe bağlı her sorgu oluşur [FROM](sql-query-from.md) ve [burada](sql-query-where.md) ANSI SQL standartları başına yan tümceleri. Genellikle, kaynak FROM yan tümcesindeki numaralandırılana ve WHERE yan tümcesi JSON öğelerinin kümesini almak için kaynak bir filtre uygular. SELECT yan tümcesi, ardından istenen JSON değerleri seçim listesinde yansıtıyor.

## <a name="syntax"></a>Sözdizimi

```sql
SELECT <select_specification>  

<select_specification> ::=   
      '*'   
      | [DISTINCT] <object_property_list>   
      | [DISTINCT] VALUE <scalar_expression> [[ AS ] value_alias]  
  
<object_property_list> ::=   
{ <scalar_expression> [ [ AS ] property_alias ] } [ ,...n ]  
  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<select_specification>`  

  Özellikleri veya sonuç kümesi için seçilecek değeri.  
  
- `'*'`  

  Değer herhangi bir değişiklik yapmadan alınması gerektiğini belirtir. İşlenen değer bir nesne ise, özellikle, tüm özellikleri alınır.  
  
- `<object_property_list>`  
  
  Alınacak özelliklerinin listesini belirtir. Her döndürülen değer, belirtilen özellikleri içeren bir nesne olacaktır.  
  
- `VALUE`  

  JSON değerinin yerine tam JSON nesne alınacağını belirtir. Bunun aksine `<property_list>` Öngörülen Değer nesneyi kaydırma değil.  
 
- `DISTINCT`
  
  Yinelenen öngörülen özelliklerinin kaldırılması gerektiğini belirtir.  

- `<scalar_expression>`  

  Hesaplanmasını değeri gösteren ifade. Bkz: [skaler ifadelerin](sql-query-scalar-expressions.md) ayrıntıları bölümü.  

## <a name="remarks"></a>Açıklamalar

`SELECT *` Söz dizimi FROM yan tümcesi tam olarak bir diğer ad bildirilmişlerse geçerli yalnızca. `SELECT *` projeksiyon gerekirse yararlı olabilecek bir kimlik yansıtma sağlar. SEÇİN * yalnızca FROM yan tümcesi belirtilmiş ise geçerlidir ve yalnızca tek bir giriş kaynağı kullanıma sunulmuştur.  
  
Her ikisi de `SELECT <select_list>` ve `SELECT *` "söz dizimi sugar" olan ve aşağıda gösterildiği gibi basit bir SELECT deyimi kullanarak alternatif olarak belirtilebilir.  
  
1. `SELECT * FROM ... AS from_alias ...`  
  
   eşdeğerdir:  
  
   `SELECT from_alias FROM ... AS from_alias ...`  
  
2. `SELECT <expr1> AS p1, <expr2> AS p2,..., <exprN> AS pN [other clauses...]`  
  
   eşdeğerdir:  
  
   `SELECT VALUE { p1: <expr1>, p2: <expr2>, ..., pN: <exprN> }[other clauses...]`  
  
## <a name="examples"></a>Örnekler

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

### <a name="quoted-property-accessor"></a>Tırnak işaretli bir özellik erişimcisi
Tırnak işaretli özelliği [] işleci kullanılarak özelliklerine erişebilirsiniz. Örneğin, `SELECT c.grade` ve `SELECT c["grade"]` eşdeğerdir. Bu sözdizimi, kaçış özel karakterleri, boşluk içeren veya SQL anahtar sözcüğü ya da ayrılmış sözcük aynı ada sahip bir özellik kullanışlıdır.

```sql
    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"
```

### <a name="nested-properties"></a>İç içe Özellikler

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
### <a name="json-expressions"></a>JSON ifadeleri

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

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [WHERE yan tümcesi](sql-query-where.md)