---
title: Azure Cosmos DB için sorgular SQL katılın
description: Azure Cosmos DB için birleştirme SQL söz dizimi hakkında öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: 408ee11b318143b3128833a741e04dd68f3816ed
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342644"
---
# <a name="joins-in-azure-cosmos-db"></a>Azure Cosmos DB birleştirir

İlişkisel bir veritabanında tabloları arasında birleştirmeler normalleştirilmiş şemaları tasarlamaya mantıksal corollary ' dir. Buna karşılık, SQL API'si mantıksal şemasız öğeleri normalleştirilmişlikten çıkarılmış veri modelini kullanan, eşdeğer bir *kendi kendine birleşme*.

İç birleştirmeler birleştirme işleminde katılan kümeleri, eksiksiz bir çapraz ürün sonuçlanır. Çok yönlü birleştirme sonucunu, burada her bir tanımlama grubu değeri katılan birleştirme işleminde ayarlamak diğer adlı ile ilişkilendirilir ve bu diğer ad diğer yan tümceleri içinde başvurarak erişilebilir N-öğe dizilerini kümesidir.

## <a name="syntax"></a>Sözdizimi

Sözdizimi dili destekleyen `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`. Bu sorgu içeren başlık kümesi döndürür `N` değerleri. Her bir tanımlama grubunu tüm kapsayıcı diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor. 

Aşağıdaki FROM yan tümcesi göz atalım: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Her kaynak tanımlamak olanak `input_alias1, input_alias2, …, input_aliasN`. Bu FROM yan tümcesi, N-tanımlama grubu (N değeri ile tanımlama grubu) kümesini döndürür. Her bir tanımlama grubunu tüm kapsayıcı diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor.  
  
**Örnek 1** -2 kaynakları  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- İzin `<from_source2>` input_alias1 başvuran belge kapsamlı ve kümeleri temsil eder:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
**Örnek 2** -3 kaynakları  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- İzin `<from_source2>` belge kapsamlı başvuran olması `input_alias1` ve kümeleri temsil eder:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- İzin `<from_source3>` belge kapsamlı başvuran olması `input_alias2` ve kümeleri temsil eder:  
  
    {100, 200} için `input_alias2 = 1,`  
  
    {300} için `input_alias2 = 3,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (BİR, 1, 100), (A, 1, 200), (B, 3, 300)  
  
  > [!NOTE]
  > Diğer değerler için demetleri eksikliği `input_alias1`, `input_alias2`, kendisi için `<from_source3>` herhangi bir değer döndürmedi.  
  
**Örnek 3** -3 kaynakları  
  
- < {A, B, C} kümesini temsil eder ve kapsayıcı kapsamlı from_source1 > sağlar.  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- < Belge kapsamlı başvuru input_alias1 olması ve kümelerini göstermek from_source2 > sağlar:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- İzin `<from_source3>` kapsamı `input_alias1` ve kümeleri temsil eder:  
  
    {100, 200} için `input_alias2 = A,`  
  
    {300} için `input_alias2 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (BİR, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300), (C, 5, 300)  
  
  > [!NOTE]
  > Bu arasında çapraz ürün içinde sonuçlanan `<from_source2>` ve `<from_source3>` her ikisi de aynı belirlenir çünkü `<from_source1>`.  (2 x 2) 4'te bu durum diziler değerini 0 tanımlama grubu B (1 x 0) değerine sahip olan ve 2 (2 x 1) değeri c diziler  
  
## <a name="examples"></a>Örnekler

Aşağıdaki örnekler, JOIN yan tümcesi nasıl çalıştığını gösterir. Aşağıdaki örnekte, her bir kaynaktan gelen öğenin çapraz ürün beri sonucu boş olduğundan ve boş boştur:

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

Sonuç olur:

```json
    [{
    }]
```

Aşağıdaki örnekte, iki JSON nesnesi, öğe kök arasında çapraz ürün birleşimi olan `id` ve `children` subroot. Olgu, `children` olan tek bir kök ile ilgilenen çünkü bir dizi birleşimde, etkin olmayan olduğu `children` dizisi. Her bir öğeyle diziye çapraz çarpımını tam olarak yalnızca bir öğe sağladığından sonucu yalnızca iki sonuçlarını içerir.

```sql
    SELECT f.id
    FROM Families f
    JOIN f.children
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

Aşağıdaki örnek, daha geleneksel bir birleştirme gösterir:

```sql
    SELECT f.id
    FROM Families f
    JOIN c IN f.children
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

FROM JOIN yan tümcesi bir yineleyici kaynağıdır. Bu nedenle, önceki örnekte akışı şöyledir:  

1. Her alt öğeyi Genişlet `c` dizi.
2. Kök öğesi ile bir çapraz ürün `f` her alt öğe `c` , ilk adım düzleştirilmiş.
3. Son olarak, kök nesnenin proje `f` `id` tek başına bir özellik.

İlk öğeyi `AndersenFamily`, yalnızca bir tane var. `children` sonuç kümesi tek bir nesne içerecek şekilde öğesi. İkinci öğe `WakefieldFamily`, iki tane `children`çapraz her iki nesne oluşturur. Bu nedenle `children` öğesi. Bir çapraz ürün beklediğiniz gibi bu her iki öğe kök alanları aynıdır.

Gerçek JOIN yan tümcesinin form diziler için Aksi takdirde projeye zor olan Şekil arası ürünün programıdır. Kullanıcının sağlayan bir tanımlama grubu birleşimi filtreleri aşağıdaki örnekte genel diziler memnun bir koşul seçin.

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
```

Sonuçlar şu şekildedir:

```json
    [
      {
        "familyName": "AndersenFamily",
        "childFirstName": "Henriette Thaulow",
        "petName": "Fluffy"
      },
      {
        "familyName": "WakefieldFamily",
        "childGivenName": "Jesse",
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

Önceki örnekte bulunan aşağıdaki uzantı double JOIN gerçekleştirir. Çapraz aşağıdaki sözde kod görüntüleyebilir:

```
    for-each(Family f in Families)
    {
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName,
                  c.givenName AS childGivenName,
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }
```

`AndersenFamily` Çapraz bir satır üretir. Bu nedenle bir evcil hayvan olan bir alt öğesi var (1\*1\*1) bu aile öğesinden. `WakefieldFamily` iki alt öğe, yalnızca bunlardan birinin Evcil Hayvanlar sahiptir, ancak bu alt iki Evcil Hayvanlar içeriyor. Çapraz ürün bu aile için 1 verir\*1\*2 = 2 satır.

Sonraki örnekte olduğundan bir ek filtre `pet`, evcil hayvan adı olduğu tüm diziler dışlar `Shadow`. Diziler dizilerden tanımlama grubu öğelerinin herhangi bir filtre oluşturun ve proje öğeleri herhangi bir birleşimini kullanabilirsiniz.

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"
```

Sonuçlar şu şekildedir:

```json
    [
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Alt sorgular](sql-query-subquery.md)