---
title: Azure Cosmos DB’de SQL ile sorgulama
description: Azure Cosmos DB’de SQL ile sorgulamayı öğrenin
author: rafats
ms.author: rafats
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.topic: tutorial
ms.date: 05/10/2017
ms.reviewer: sngun
ms.openlocfilehash: 329f50341649c353fae21d92e8d5f25b1e45a632
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54043592"
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-sql-api"></a>Öğretici: SQL API'sini kullanarak Azure Cosmos DB'yi sorgulama

Azure Cosmos DB [SQL API’si](documentdb-introduction.md), SQL kullanılarak belgelerin sorgulanmasını destekler. Bu makalede, örnek bir belge ve iki örnek SQL sorgusu ve sonuçları sağlanmaktadır.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * SQL ile verileri sorgulama

## <a name="sample-document"></a>Örnek belge

Bu makaledeki SQL sorguları aşağıdaki örnek belgeyi kullanır.

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
## <a name="where-can-i-run-sql-queries"></a>SQL sorgularını nerede çalıştırabilirim?

Mevcut örnek veri kümesinde sorgular çalıştıran [Sorgu oyun alanı](https://www.documentdb.com/sql/demo) ve [REST API’si ve SDK’ları](sql-api-sdk-dotnet.md) aracılığıyla, Azure portalındaki Veri Gezgini’ni kullanarak sorgular çalıştırabilirsiniz.

SQL sorguları hakkında daha fazla bilgi için bkz:
* [SQL sorgusu ve SQL sözdizimi](how-to-sql-query.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, bir Azure Cosmos DB hesabınız ve koleksiyonunuz olduğu varsayılır. Bunlardan biri yok mu? [5 dakikalık hızlı başlangıcı](create-mongodb-nodejs.md) tamamlayın.

## <a name="example-query-1"></a>Örnek sorgu 1

Yukarıda verilen örnek aile belgesiyle aşağıdaki SQL sorgusu, kimlik alanının `WakefieldFamily` ile eşleştiği belgeleri döndürür. Bu bir `SELECT *` deyimi olduğundan, sorgunun çıkışı eksiksiz JSON belgesidir:

**Sorgu**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Sonuçlar**

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

## <a name="example-query-2"></a>Örnek sorgu 2

Sonraki sorgu, ailede kimlikleri `WakefieldFamily` ile eşleşen çocukların tüm adlarını, çocukların sınıfına göre sıralanmış şekilde döndürür.

**Sorgu**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'

**Sonuçlar**

[{"givenName": "Jesse"}, {"givenName": "Lisa"}]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * SQL kullanarak sorgulamayı öğrendiniz  

Artık verilerinizi genel olarak nasıl dağıtacağınızı öğrenmek için sonraki öğreticiye ilerleyebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel olarak dağıtma](tutorial-global-distribution-sql-api.md)

