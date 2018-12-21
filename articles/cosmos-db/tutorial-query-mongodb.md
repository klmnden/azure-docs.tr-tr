---
title: MongoDB API'si için Azure Cosmos DB'den verileri Sorgulama
description: Azure Cosmos DB ile MongoDB API'si için sorgu yürütmeyi öğrenirsiniz.
services: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/29/2018
ms.openlocfilehash: bcfbb0115274c8b0d330426a454530dcd7c170ab
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724666"
---
# <a name="tutorial-query-data-using-azure-cosmos-db-for-mongodb-api"></a>Öğretici: Azure Cosmos DB için MongoDB API'sini kullanarak verileri Sorgulama

[MongoDB için Azure Cosmos DB API’si](mongodb-introduction.md), [MongoDB kabuk sorgularını](https://docs.mongodb.com/manual/tutorial/query-documents/) destekler. 

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * MongoDB Kabuğu'nu kullanarak Cosmos veritabanında depolanan QQuerying veri

Başlamak için bu belgedeki örnekleri kullanabilir ve [Query Azure Cosmos DB with MongoDB shell](https://azure.microsoft.com/resources/videos/query-azure-cosmos-db-data-by-using-the-mongodb-shell/) (Azure Cosmos DB'yi MongoDB kabuğu ile sorgulama) videosunu izleyebilirsiniz.

## <a name="sample-document"></a>Örnek belge

Bu makaledeki sorgular aşağıdaki örnek belgeyi kullanır.

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
        "gender": "female", "grade": 1,
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
## <a id="examplequery1"></a> Örnek sorgu 1 

Yukarıda verilen örnek aile belgesiyle aşağıdaki sorgu, kimlik alanının `WakefieldFamily` ile eşleştiği belgeleri döndürür.

**Sorgu**
    
    db.families.find({ id: "WakefieldFamily"})

**Sonuçlar**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
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
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a id="examplequery2"></a>Örnek sorgu 2 

Sonraki sorgu, ailedeki tüm çocukları döndürür. 

**Sorgu**
    
    db.families.find( { id: "WakefieldFamily" }, { children: true } )

**Sonuçlar**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
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
        "grade": 8
      }
    ]
    }


## <a id="examplequery3"></a>Örnek sorgu 3 

Sonraki sorgu, kayıtlı olan tüm aileleri döndürür. 

**Sorgu**
    
    db.families.find( { "isRegistered" : true })
**Sonuçlar** Herhangi bir belge döndürülmez. 

## <a id="examplequery4"></a>Örnek sorgu 4

Sonraki sorgu, kayıtlı olmayan tüm aileleri döndürür. 

**Sorgu**
    
    db.families.find( { "isRegistered" : false })
**Sonuçlar**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery5"></a>Örnek sorgu 5

Sonraki sorgu, kayıtlı olmayan ve durumu NY olan tüm aileleri döndürür. 

**Sorgu**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Sonuçlar**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a id="examplequery6"></a>Örnek sorgu 6

Sonraki sorgu, çocukları 8. sınıfta olan tüm aileleri döndürür.

**Sorgu**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Sonuçlar**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery7"></a>Örnek sorgu 7

Sonraki sorgu, çocuk dizisi boyutu 3 olan tüm aileleri döndürür.

**Sorgu**
  
      db.Family.find( {children: { $size:3} } )

**Sonuçlar**

İkiden fazla çocuğu olan bir aile olmadığından herhangi bir sonuç döndürülmez. Yalnızca parametre 2 olduğunda bu sorgu başarılı olur ve tam belgeyi döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * MongoDB API'si için yerel destek Cosmos DB kullanarak sorgulamayı öğrendiniz

Artık verilerinizi genel olarak nasıl dağıtacağınızı öğrenmek için sonraki öğreticiye ilerleyebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel olarak dağıtma](tutorial-global-distribution-sql-api.md)

