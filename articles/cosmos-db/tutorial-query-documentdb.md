---
title: "SQL Azure Cosmos veritabanı ile nasıl? | Microsoft Belgeleri"
description: "SQL Azure Cosmos veritabanı ile sorgu öğrenin"
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.openlocfilehash: ae2d7c4ecdeeb88714cdaa0558950ea52fab99d3
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cosmos-db-how-to-query-using-sql"></a>Azure Cosmos DB: SQL kullanarak nasıl?

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure Cosmos DB [SQL API](documentdb-introduction.md) belgeleri SQL kullanarak sorgulama destekler. Bu makalede, örnek bir belge ve iki örnek SQL sorguları ve sonuçları sağlar.

Bu makalede aşağıdaki görevleri içerir: 

> [!div class="checklist"]
> * SQL ile veri sorgulama

## <a name="sample-document"></a>Örnek bir belge

Bu makalede SQL sorguları aşağıdaki örnek belge kullanın.

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
## <a name="where-can-i-run-sql-queries"></a>SQL sorguları yeri çalıştırabilir miyim?

Aracılığıyla Azure portalında Veri Gezgini'ni kullanarak sorguları çalıştırabilirsiniz [REST API ve SDK](documentdb-sdk-dotnet.md)ve hatta [Query playground](https://www.documentdb.com/sql/demo), var olan bir örnek veri kümesini temel sorguları çalıştırır.

SQL sorguları hakkında daha fazla bilgi için bkz:
* [SQL sorgusu ve SQL söz dizimi](documentdb-sql-query.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici bir Azure Cosmos DB hesap ve koleksiyon olduğunu varsayar. Bu yok? Tamamlamak [5 dakikalık quickstart](create-mongodb-nodejs.md) veya [Geliştirici öğretici](tutorial-develop-mongodb.md) bir hesap ve koleksiyonu oluşturmak için.

## <a name="example-query-1"></a>Örnek sorgu 1

Yukarıdaki örnek ailesi belge verilen, SQL sorgusu aşağıdaki belgeleri ID alanı eşleştiği döndürür `WakefieldFamily`. Olduğundan bir `SELECT *` ifadesi, sorgu çıktısı tam JSON belgesi şöyledir:

**Sorgu**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Sonuçları**

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

## <a name="example-query-2"></a>Örnek Sorgu 2

Sonraki sorgu kimliğine eşleşen ailesinde alt tüm verilen adlarını döndürür `WakefieldFamily` kendi sınıf tarafından sıralanan.

**Sorgu**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Sonuçları**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * SQL kullanarak sorgulamak öğrendiniz  

Verilerinizi Genel dağıtma konusunda bilgi almak için sonraki öğretici şimdi devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel Dağıt](tutorial-global-distribution-documentdb.md)

