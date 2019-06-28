---
title: Azure Cosmos DB'de WHERE yan tümcesi
description: Azure Cosmos DB için SQL WHERE yan tümcesi hakkında bilgi edinin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mjbrown
ms.openlocfilehash: 6a942e48ffea7785fe971cc2f8fa66e8569ed672
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342702"
---
# <a name="where-clause"></a>WHERE yan tümcesi

İsteğe bağlı WHERE yan tümcesi (`WHERE <filter_condition>`) koşulları belirtir kaynak JSON öğeleri sorgu sonuçlarında eklemek için karşılaması gerekir. Bir JSON öğesi için belirtilen koşullar değerlendirmelidir `true` sonucu olarak kabul edilmesi için. Dizin katman WHERE yan tümcesi, sonuç bir parçası olabilir kaynak öğeleri en küçük kümesini belirlemek için kullanır.
  
## <a name="syntax"></a>Sözdizimi
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler

- `<filter_condition>`  
  
   Döndürülecek belgeler için karşılanması gereken bir koşulu belirtir.  
  
- `<scalar_expression>`  
  
   Hesaplanmasını değeri gösteren ifade. Bkz: [skaler ifadelerin](sql-query-scalar-expressions.md) Ayrıntılar için.  
  

## <a name="remarks"></a>Açıklamalar
  
  Filtre olarak belirtilen bir ifade döndürülecek belge sırada koşul true olarak değerlendirilmelidir. Başka bir değer koşulu, Boole değeri true yerine getirecek yalnızca: tanımsız, null, false, sayı, dizi veya nesne karşılamaz koşul. 

## <a name="examples"></a>Örnekler

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

Önceki örnekte, bir basit eşitlik sorgu gösterdi. SQL API'si ayrıca çeşitli destekler [skaler ifadelerin](sql-query-scalar-expressions.md). En sık kullanılan ikili ve birli ifadelerdir. Kaynak JSON nesne özelliği başvurularından da geçerli ifadelerdir.

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

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [FROM yan tümcesi](sql-query-from.md)