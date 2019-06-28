---
title: LINQ to SQL çeviri Azure Cosmos DB'de
description: LINQ sorguları, Azure Cosmos DB SQL sorguları eşleme.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: tisande
ms.openlocfilehash: 057614da8fd29e1208c2788049c5d6d1a985eed5
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342720"
---
# <a name="linq-to-sql-translation"></a>LINQ to SQL çeviri

Azure Cosmos DB sorgu sağlayıcısı bir en iyi çaba eşleme bir Cosmos DB SQL sorgusu bir LINQ Sorgu gerçekleştirir. Aşağıdaki açıklama LINQ temel olarak bilindiğini varsayar.

Sorgu sağlayıcısı tür sistemi yalnızca JSON ilkel türlerini destekler: sayısal, Boole, dize ve null.

Sorgu sağlayıcısı aşağıdaki skaler ifadelerin destekler:

- Sorgu değerlendirme zamanında sabit değerler ilkel veri türleri dahil olmak üzere sabit değerler.
  
- Bir nesne veya dizi öğesi özelliğine başvuran özelliği/dizi dizini ifadeleri. Örneğin:
  
  ```
    family.Id;
    family.children[0].familyName;
    family.children[0].grade;
    family.children[n].grade; //n is an int variable
  ```
  
- Sayısal ve Boolean değerleri ortak aritmetik ifadeler de dahil olmak üzere aritmetik ifadeler. Tam liste için bkz. [Azure Cosmos DB SQL belirtimi](https://go.microsoft.com/fwlink/p/?LinkID=510612).
  
  ```
    2 * family.children[0].grade;
    x + y;
  ```
  
- Bazı sabit dize değeri bir dize değeri karşılaştırma içeren dize karşılaştırma ifadeleri.  
  
  ```
    mother.familyName == "Wakefield";
    child.givenName == s; //s is a string variable
  ```
  
- Bileşik değer türü veya anonim türdeki bir nesne veya bu tür nesneler dizisi döndürür oluşturma ifadeleri, nesne veya dizi. Bu değerleri iç içe yerleştirebilirsiniz.
  
  ```
    new Parent { familyName = "Wakefield", givenName = "Robin" };
    new { first = 1, second = 2 }; //an anonymous type with two fields  
    new int[] { 3, child.grade, 5 };
  ```

## <a id="SupportedLinqOperators"></a>Desteklenen LINQ işleçleri

SQL .NET SDK'sı ile dahil LINQ sağlayıcısı aşağıdaki işleçleri destekler:

- **Seçin**: Projeksiyonlar nesne oluşturmayı da dahil olmak üzere SQL SEÇMEK için çevir.
- **Burada**: Filtreler SQL WHERE için çevir ve Destek arasında çeviri `&&`, `||`, ve `!` SQL işleçlere değiştirme
- **SelectMany**: SQL JOIN yan tümcesine dizi geriye doğru izleme sağlar. Zincir veya iç içe dizi öğeleri üzerinde filtre ifadeleri için kullanın.
- **OrderBy** ve **OrderByDescending**: Sıralama ölçütü ASC veya DESC için çevir.
- **Sayısı**, **toplam**, **Min**, **Max**, ve **ortalama** işleçleri toplama ve zaman uyumsuz eşdeğerlerine **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, ve **AverageAsync**.
- **CompareTo**: Aralık karşılaştırmalar çevirir. . NET'te karşılaştırılabilir değilseniz bu yana dizeler için yaygın olarak kullanılır.
- **Ele**: Bir sorgunun sonuçlarını sınırlamak için SQL en üste çevirir.
- **Matematik işlevleri**: Çeviri net'ten destekler `Abs`, `Acos`, `Asin`, `Atan`, `Ceiling`, `Cos`, `Exp`, `Floor`, `Log`, `Log10`, `Pow`, `Round`, `Sign`, `Sin`, `Sqrt`, `Tan`, ve `Truncate` eşdeğer SQL yerleşik işlevleri.
- **Dize işlevleri**: Çeviri net'ten destekler `Concat`, `Contains`, `Count`, `EndsWith`,`IndexOf`, `Replace`, `Reverse`, `StartsWith`, `SubString`, `ToLower`, `ToUpper`, `TrimEnd`, ve `TrimStart` eşdeğer SQL yerleşik işlevleri.
- **Dizi işlevleri**: Çeviri net'ten destekler `Concat`, `Contains`, ve `Count` eşdeğer SQL yerleşik işlevleri.
- **Jeo-uzamsal uzantı işlevleri**: Saptama yöntemleri çevrilmesi destekler `Distance`, `IsValid`, `IsValidDetailed`, ve `Within` eşdeğer SQL yerleşik işlevleri.
- **Kullanıcı tanımlı işlev uzantı işlevi**: Çeviri saplama yönteminden destekler `UserDefinedFunctionProvider.Invoke` karşılık gelen kullanıcı tanımlı işlev.
- **Çeşitli**: Çeviri destekler `Coalesce` ve Koşullu işleçler. Çevirebilir `Contains` dize İÇERİYOR, ARRAY_CONTAINS veya bağlama bağlı olarak SQL IN.

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler nasıl bazı standart LINQ Sorgu işleçleri için Cosmos DB sorgular Çevir gösterir.

### <a name="select-operator"></a>İşleç seçin

Söz dizimi `input.Select(x => f(x))`burada `f` skaler bir ifade.

**Örnek 1 bir işleç seçin:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.parents[0].familyName);
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
    ```
  
**Örnek 2 bir işleç seçin:** 

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.children[0].grade + c); // c is an int variable
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.children[0].grade + c
      FROM Families f
  ```
  
**Örnek 3 bir işleç seçin:**

- **LINQ lambda ifadesi**
  
  ```csharp
    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });
  ```
  
- **SQL** 
  
  ```sql
      SELECT VALUE {"name":f.children[0].familyName,
                    "grade": f.children[0].grade + 3 }
      FROM Families f
  ```

### <a name="selectmany-operator"></a>SelectMany işleci

Söz dizimi `input.SelectMany(x => f(x))`burada `f` bir kapsayıcı türü döndüren bir skaler ifade.

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.children);
  ```
  
- **SQL**

  ```sql
      SELECT VALUE child
      FROM child IN Families.children
  ```

### <a name="where-operator"></a>Burada işleci

Söz dizimi `input.Where(x => f(x))`burada `f` bir Boole değeri döndüren bir skaler ifade.

**Burada işleç, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(family=> family.parents[0].familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```
  
**Burada işleç, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(
          family => family.parents[0].familyName == "Wakefield" &&
          family.children[0].grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
      AND f.children[0].grade < 3
  ```

## <a name="composite-sql-queries"></a>Bileşik SQL sorguları

Daha güçlü sorgular oluşturmak için yukarıdaki işleçleri oluşturabilirsiniz. Cosmos DB, iç içe geçmiş kapsayıcılar desteklediğinden, birleştirme veya iç içe birleşim.

### <a name="concatenation"></a>Birleştirme

Söz dizimi `input(.|.SelectMany())(.Select()|.Where())*`. Birleştirilmiş bir sorgu ile isteğe bağlı olarak başlatabilirsiniz `SelectMany` sorgu, birden fazla izlenen `Select` veya `Where` işleçleri.

**Birleştirme, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family=>family.parents[0])
          .Where(familyName == "Wakefield");
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```

**Birleştirme, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(family => family.children[0].grade > 3)
          .Select(family => family.parents[0].familyName);
  ```

- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
      WHERE f.children[0].grade > 3
  ```

**Birleştirme, örnek 3:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => new { grade=family.children[0].grade}).
          Where(anon=> anon.grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE ({grade: f.children[0].grade}.grade > 3)
  ```

**Birleştirme, örnek 4:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.parents)
          .Where(parent => parents.familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM p IN Families.parents
      WHERE p.familyName = "Wakefield"
  ```

### <a name="nesting"></a>İç içe geçirme

Söz dizimi `input.SelectMany(x=>x.Q())` burada `Q` olduğu bir `Select`, `SelectMany`, veya `Where` işleci.

İç içe bir sorgu iç sorgu dış kapsayıcı her öğeye uygulanır. Bir önemli özelliği, iç sorgu alanları, kendi kendine birleşme gibi dış kapsayıcıdaki öğelerin başvurabilir.

**İç içe geçme, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family=>
          family.parents.Select(p => p.familyName));
  ```

- **SQL**
  
  ```sql
      SELECT VALUE p.familyName
      FROM Families f
      JOIN p IN f.parents
  ```

**İç içe geçme, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family =>
          family.children.Where(child => child.familyName == "Jeff"));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = "Jeff"
  ```

**İç içe geçme, örnek 3:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.children.Where(
          child => child.familyName == family.parents[0].familyName));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = f.parents[0].familyName
  ```


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Belge verilerini modelleme](modeling-data.md)
