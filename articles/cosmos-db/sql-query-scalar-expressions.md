---
title: Azure Cosmos DB SQL sorgularında skaler ifade
description: Azure Cosmos DB için skaler ifade SQL söz dizimi hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: 4464c39a45c47c680a13f3ebc34841b47ee0d7c6
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342746"
---
# <a name="scalar-expressions-in-azure-cosmos-db-sql-queries"></a>Azure Cosmos DB SQL sorgularında skaler ifade

[SELECT yan tümcesi](sql-query-select.md) skaler ifadeleri destekler. Skaler bir ifade, semboller ve işleçleri tek bir değer almak için değerlendirilen bir birleşimidir. Skaler ifade örnekleri: sabitleri, özellik başvuru, dizi öğesi başvuruları, diğer başvurular veya işlev çağrıları. Skaler ifadelerin işleçleri kullanarak karmaşık ifadelere birleştirilebilir.

## <a name="syntax"></a>Sözdizimi
  
```sql  
<scalar_expression> ::=  
       <constant>
     | input_alias
     | parameter_name  
     | <scalar_expression>.property_name  
     | <scalar_expression>'['"property_name"|array_index']'  
     | unary_operator <scalar_expression>  
     | <scalar_expression> binary_operator <scalar_expression>    
     | <scalar_expression> ? <scalar_expression> : <scalar_expression>  
     | <scalar_function_expression>  
     | <create_object_expression>
     | <create_array_expression>  
     | (<scalar_expression>)
  
<scalar_function_expression> ::=  
        'udf.' Udf_scalar_function([<scalar_expression>][,…n])  
        | builtin_scalar_function([<scalar_expression>][,…n])  
  
<create_object_expression> ::=  
   '{' [{property_name | "property_name"} : <scalar_expression>][,…n] '}'  
  
<create_array_expression> ::=  
   '[' [<scalar_expression>][,…n] ']'  
  
```

## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<constant>`  
  
   Sabit bir değeri temsil eder. Bkz: [sabitleri](sql-query-constants.md) ayrıntıları bölümü.  
  
- `input_alias`  
  
   Tarafından tanımlanan bir değeri temsil `input_alias` sunulan `FROM` yan tümcesi.  
  Bu değer olmaması garanti **tanımlanmamış** –**tanımlanmamış** giriş değerleri atlanır.  
  
- `<scalar_expression>.property_name`  
  
   Bir nesnenin özellik değerini temsil eder. Özellik mevcut değil veya özelliği bir nesne değil, bir değer üzerinde başvurulan ardından ifadenin değerlendirdiği **tanımlanmamış** değeri.  
  
- `<scalar_expression>'['"property_name"|array_index']'`  
  
   Bir ada sahip özelliğin değerini temsil eden `property_name` veya dizi öğesi `array_index` dizi. Özelliği/dizi dizini mevcut değil veya özellik/dizi dizininden başvurulan bir değeri bir nesne/dizisi olmayan ve ardından ifadeyi tanımlanmamış değerini hesaplar.  
  
- `unary_operator <scalar_expression>`  
  
   Tek bir değer için uygulanan bir işleç temsil eder. Bkz: [işleçleri](sql-query-operators.md) ayrıntıları bölümü.  
  
- `<scalar_expression> binary_operator <scalar_expression>`  
  
   İki değer için uygulanan bir işleç temsil eder. Bkz: [işleçleri](sql-query-operators.md) ayrıntıları bölümü.  
  
- `<scalar_function_expression>`  
  
   Bir işlev çağrısı sonucunu tarafından tanımlanan bir değeri temsil eder.  
  
- `udf_scalar_function`  
  
   Kullanıcı tanımlı skaler işlevin adı.  
  
- `builtin_scalar_function`  
  
   Yerleşik skaler işlevin adı.  
  
- `<create_object_expression>`  
  
   Belirtilen özelliklere sahip yeni bir nesne oluşturarak elde ettiği değerle ve bunların değerlerini temsil eder.  
  
- `<create_array_expression>`  
  
   Öğeleri olarak belirtilen değerlerle yeni bir dizi oluşturarak elde ettiği değerle temsil eder  
  
- `parameter_name`  
  
   Belirtilen parametre adı değerini temsil eder. Tek bir parametre adları olmalıdır \@ ilk karakteri olarak.  
  
## <a name="remarks"></a>Açıklamalar
  
  Tüm bağımsız değişkenleri, yerleşik veya kullanıcı tanımlı skaler bir işlev çağrılırken tanımlanmalıdır. Bağımsız değişken tanımlanmamış, işlev çağrılmaz ve sonuç tanımsız olur.  
  
  Bir nesne oluştururken, tanımlanmamış değeri atanır. herhangi bir özelliği atlandı ve oluşturulan nesneyi dahil değildir.  
  
  Zaman dizisi, herhangi bir öğe değer oluşturma atandığı **tanımlanmamış** değeri atlandı ve oluşturulan nesneyi dahil değildir. Bu şekilde oluşturulan dizi dizinleri atlandı değil, onun yerine almak sonraki tanımlanan öğe neden olur.  

## <a name="examples"></a>Örnekler

```sql
    SELECT ((2 + 11 % 7)-2)/3
```

Sonuçlar şu şekildedir:

```json
    [{
      "$1": 1.33333
    }]
```

Aşağıdaki sorguda, skaler ifade sonucu bir Boolean verilmiştir:

```sql
    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "AreFromSameCityState": false
      },
      {
        "AreFromSameCityState": true
      }
    ]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Alt sorgular](sql-query-subquery.md)