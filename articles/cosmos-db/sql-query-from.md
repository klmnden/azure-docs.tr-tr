---
title: Azure Cosmos DB'de FROM yan tümcesi
description: Azure Cosmos DB için SQL FROM yan tümcesi hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: tisande
ms.openlocfilehash: 6bc93569dc9a0405ec3a8dfd719c89ede01df84d
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342837"
---
# <a name="from-clause"></a>FROM yan tümcesi

Kimden (`FROM <from_specification>`) kaynak filtre veya sorguyu daha sonra öngörülen sürece yan tümcesinin isteğe bağlıdır. Bir sorgu ister `SELECT * FROM Families` tüm numaralandırır `Families` kapsayıcı. Özel tanımlayıcısı kök kapsayıcı adı yerine kapsayıcısı için de kullanabilirsiniz.

FROM yan tümcesi sorgu başına aşağıdaki kuralları uygular:

* Kapsayıcı gibi diğer adı, olabilir `SELECT f.id FROM Families AS f` ya da yalnızca `SELECT f.id FROM Families f`. Burada `f` için diğer ad `Families`. İsteğe bağlı bir anahtar sözcük olduğundan [diğer](sql-query-aliasing.md) tanımlayıcısı.  

* Diğer adlı bir kez, özgün kaynak adına bağımlı olamaz. Örneğin, `SELECT Families.id FROM Families f` sözdizimsel olarak geçersiz olduğundan tanımlayıcı `Families` diğer adlı yapıldı ve artık çözümlenemiyor.  

* Başvurulan tüm özellikleri, katı şema bağlılığı olmaması belirsiz herhangi bir bağlamayı önlemek tam olarak nitelenmiş olmalıdır. Örneğin, `SELECT id FROM Families f` sözdizimsel olarak geçersiz olduğundan özelliği `id` bağlı değil.

## <a name="syntax"></a>Sözdizimi
  
```sql  
FROM <from_specification>  
  
<from_specification> ::=   
        <from_source> {[ JOIN <from_source>][,...n]}  
  
<from_source> ::=   
          <container_expression> [[AS] input_alias]  
        | input_alias IN <container_expression>  
  
<container_expression> ::=   
        ROOT   
     | container_name  
     | input_alias  
     | <container_expression> '.' property_name  
     | <container_expression> '[' "property_name" | array_index ']'  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
- `<from_source>`  
  
  Bir veri kaynağı olan veya olmayan bir diğer ad belirtir. Diğer ad belirtilmezse, içinden gösterilen `<container_expression>` kuralları kullanarak:  
  
  -  İfade bir container_name ise container_name diğer ad olarak kullanılır.  
  
  -  İfade ise `<container_expression>`, property_name sonra property_name diğer ad olarak kullanılır. İfade bir container_name ise container_name diğer ad olarak kullanılır.  
  
- FARKLI `input_alias`  
  
  Belirten `input_alias` temel alınan kapsayıcı ifadesi tarafından döndürülen değerler kümesidir.  
 
- `input_alias` GİRİŞ  
  
  Belirten `input_alias` temel alınan kapsayıcı ifadesi tarafından döndürülen her dizinin tüm dizi öğeleri üzerinde yineleme tarafından alınan değerler kümesini temsil etmelidir. Bir dizi değil temel alınan kapsayıcı ifadesi tarafından döndürülen herhangi bir değer yoksayılır.  
  
- `<container_expression>`  
  
  Belgeleri almak için kullanılacak kapsayıcı ifadesini belirtir.  
  
- `ROOT`  
  
  Bu belge, varsayılan, bağlı durumda kapsayıcı alınması gerektiğini belirtir.  
  
- `container_name`  
  
  Bu belgede sağlanan kapsayıcıdan alınacağını belirtir. Kapsayıcının adı şu anda bağlı kapsayıcısının adı eşleşmelidir.  
  
- `input_alias`  
  
  Bu belgede sağlanan diğer ad tarafından tanımlanan diğer kaynaktan alınması gerektiğini belirtir.  
  
- `<container_expression> '.' property_`  
  
  Bu belge erişerek alınabilir belirtir `property_name` özelliği veya dizi_dizini dizi öğesi tarafından alınan tüm belgeler için belirtilen kapsayıcı ifade.  
  
- `<container_expression> '[' "property_name" | array_index ']'`  
  
  Bu belge erişerek alınabilir belirtir `property_name` özelliği veya dizi_dizini dizi öğesi tarafından alınan tüm belgeler için belirtilen kapsayıcı ifade.  
  
## <a name="remarks"></a>Açıklamalar
  
Tüm diğer adları içinde çıkarımı yapılan veya sağlanan `<from_source>(`s) benzersiz olmalıdır. Söz dizimi `<container_expression>.`property_name aynıdır `<container_expression>' ['"property_name"']'`. Ancak, bir özellik adı bir tanımlayıcı olmayan karakter içeriyorsa, ikinci sözdizimi kullanılabilir.  
  
### <a name="handling-missing-properties-missing-array-elements-and-undefined-values"></a>Dizi öğeleri yanı sıra, tanımsız değerler eksik özellikler eksik işleme
  
Bir kapsayıcı ifadesi özellikleri veya dizi öğeleri ve değeri yok erişirse, bu değer yoksayılır ve daha fazla işlenmedi.  
  
### <a name="container-expression-context-scoping"></a>Kapsayıcı bağlamı ifadesi kapsamı  
  
Bir kapsayıcı ifade kapsayıcı kapsamlı veya belge kapsamlı olabilir:  
  
-   Kapsayıcı-kapsayıcı ifadenin temel alınan kaynak ya da kök ise kapsamı, bir ifade veya `container_name`. Böyle bir ifade, bir kapsayıcıdan doğrudan alınan belge kümesini temsil eder ve diğer kapsayıcı ifadeler işleme bağımlı değildir.  
  
-   Bir ifade belge temel alınan kaynak kapsayıcı ifadenin ise kapsamlı, `input_alias` sorgu daha önce sunulan. Böyle bir ifade, diğer adlı kapsayıcı ile ilişkili bir kümeye ait her bir belgenin kapsamında kapsayıcı ifadesinin değerlendirilmesi elde belgeleri kümesini temsil eder.  Sonuç kümesi, her bir temel alınan belgeler için kapsayıcı ifadesinin değerlendirilmesi elde edilen kümeleri birleşimi olacaktır. 

## <a name="examples"></a>Örnekler

### <a name="get-subitems-by-using-the-from-clause"></a>FROM yan tümcesi kullanarak alt öğelerini alma

FROM yan tümcesi için daha küçük bir alt kaynak azaltabilir. Yalnızca her bir öğenin alt ağacı Numaralandırılacak subroot aşağıdaki örnekte gösterildiği gibi kaynak hale gelebilir:

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
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
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

Önceki sorgunun kaynağı olarak bir dizi kullanılan, ancak nesne kaynağı olarak kullanabilirsiniz. Sorgu, tüm geçerli, tanımlı bir JSON değeri sonuç ekleme için kaynak olarak değerlendirir. Aşağıdaki örnek hariç tutmak `Families` zorunda kalmaz bir `address.state` değeri.

```sql
    SELECT *
    FROM Families.address.state
```

Sonuçlar şu şekildedir:

```json
    [
      "WA",
      "NY"
    ]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [SELECT yan tümcesi](sql-query-select.md)
- [WHERE yan tümcesi](sql-query-where.md)