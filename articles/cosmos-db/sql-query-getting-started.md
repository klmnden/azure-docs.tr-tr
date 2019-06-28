---
title: Azure Cosmos DB'de SQL sorguları kullanmaya başlama
description: SQL sorgulara giriş
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: tisande
ms.openlocfilehash: 87b275806c06443e37e9e92c35a38b4cde378322
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342697"
---
# <a name="getting-started-with-sql-queries"></a>SQL sorguları ile çalışmaya başlama

Azure Cosmos DB SQL API hesabı, bir JSON sorgu dili yapılandırılmış sorgu dili (SQL) kullanarak sorgulama öğeleri destekler. Azure Cosmos DB Sorgu dilinin tasarım hedefleri vardır:

* Yeni bir sorgu dili inventing yerine en bilinen ve en popüler sorgu dillerinden biri, SQL destekler. SQL, JSON öğelerinin üzerine zengin sorgu için biçimsel bir programlama modeli sağlar.  

* JavaScript'in programlama modeli için sorgu dili temel olarak kullanın. JavaScript'in tür sistemi, ifade değerlendirmesi ve işlev çağrısını SQL API'sinin kökleri var. Projeksiyonlar ilişkisel, hiyerarşik gezinme JSON öğeleri arasında kendinden birleştirmeler, uzamsal sorgular ve gibi kullanıcı tanımlı işlevler (UDF'ler) çağrılmasını tamamen JavaScript'te yazılmış bu kökleri özellikler için doğal bir programlama modeli sağlar.

## <a name="upload-sample-data"></a>Örnek verileri karşıya yükleme

SQL API Cosmos DB hesabınızdaki adlı bir kapsayıcı oluşturma `Families`. İki basit JSON öğelerinin kapsayıcısında oluşturun. Bu veri kümesi kullanarak Azure Cosmos DB sorgu docs çoğu örnek sorgu çalıştırabilirsiniz.

### <a name="create-json-items"></a>JSON öğeleri oluşturma

Aşağıdaki kod, iki basit JSON öğeleri aileleri hakkında oluşturur. Andersen ve Wakefield ailesi için basit JSON öğelerinin üst, alt öğeleri ve bunların Evcil Hayvanlar, adresi ve kayıt bilgileri içerir. İlk öğenin dizeleri, sayı, Boole, diziler ve iç içe özellikler vardır.


```json
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow",
         "gender": "female",
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "Seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

İkinci öğe kullanan `givenName` ve `familyName` yerine `firstName` ve `lastName`.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller",
         "givenName": "Lisa",
         "gender": "female",
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

### <a name="query-the-json-items"></a>JSON öğeleri sorgu

JSON verilerini Azure Cosmos DB SQL sorgu dili önemli yönlerini bazıları anlamak için bazı sorguları deneyin.

Aşağıdaki sorgu öğeleri döndürür. burada `id` alan eşleşme `AndersenFamily`. Olduğundan bir `SELECT *` sorgu, sorgu çıktısı olan tam JSON öğesi. SELECT söz dizimi hakkında daha fazla bilgi için bkz. [SELECT deyimi](sql-query-select.md). 

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sorgu sonuçlarını şunlardır: 

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

Aşağıdaki sorgu, farklı bir şekle JSON çıkışını yeniden biçimlendirir. Yeni bir JSON sorgu projeleri `Family` nesne iki seçilen alan ile `Name` ve `City`, Adres Şehir durumu aynı olduğunda. Bu durumda "NY, NY" eşleşir.

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

Sorgu sonuçlarını şunlardır:

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

Aşağıdaki sorgu ailedeki verilen adlarını, çocukların döndürür, `id` eşleşen `WakefieldFamily`ve şehre göre sıralı.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

Sonuçlar şu şekildedir:

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

## <a name="remarks"></a>Açıklamalar

Önceki örneklerde, Cosmos sorgu dili çeşitli yönlerini gösterir:  

* SQL API'si, JSON değerleri üzerinde çalışır olduğundan, satır ve sütun yerine ağaç şeklinde varlıklarla ilgilenir. Rastgele herhangi derinliği ağaç düğümleri gibi başvurabilirsiniz `Node1.Node2.Node3…..Nodem`iki parçalı başvurusunu benzer `<table>.<column>` ANSI SQL.

* Sorgu dili şemasız verilerle çalışır çünkü tür sistemi dinamik olarak bağlı olmalıdır. Farklı türlerde farklı öğeye aynı ifadesi üretebilir. Bir sorgunun sonucu, geçerli bir JSON değer, ancak bir sabit şemasına olmasını garanti yoktur.  

* Azure Cosmos DB, yalnızca Katı JSON öğelerini destekler. Tür sistemi ve ifadeleri yalnızca JSON türleri ile dağıtılacak kısıtlanır. Daha fazla bilgi için [JSON belirtimi](https://www.json.org/).  

* Bir Cosmos DB kapsayıcısında JSON öğelerinin şemasız bir koleksiyondur. Birincil anahtar ve yabancı anahtar ilişkileri tarafından değil, içinde ve kapsayıcı öğeleri arasında ilişkiler kapsamlarına göre örtük olarak yakalanır. Bu özellik, bu makalenin sonraki bölümlerinde ele alınan içi öğesi birleştirme için önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [SELECT yan tümcesi](sql-query-select.md)
