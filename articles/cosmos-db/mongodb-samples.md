---
title: Bir Azure Cosmos DB uygulamanızı oluşturmak için MongoDB API'leri kullanan | Microsoft Docs
description: MongoDB için Azure Cosmos DB API'lerini kullanarak çevrimiçi bir veritabanı oluşturan Öğreticisi.
keywords: mongodb örnekleri
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: ''
documentationcenter: ''
ms.assetid: fb38bc53-3561-487d-9e03-20f232319a87
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: anhoh
ms.openlocfilehash: 1571ed8bc3146a6351d0010a9f072cad986d6dc7
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Bir Azure Cosmos DB yapı: Node.js kullanarak MongoDB uygulaması için API
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [MongoDB için Node.js](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [C++](sql-api-cpp-get-started.md)
>  
>

Bu örnek bir Azure Cosmos DB nasıl oluşturulacağını gösterir: Node.js kullanarak MongoDB konsol uygulaması için API.

Bu örneği kullanmak için yapmanız gerekir:

* [Oluşturma](create-mongodb-dotnet.md#create-account) bir Azure Cosmos DB: API MongoDB hesabı.
* MongoDB almak [bağlantı dizesi](connect-mongodb-account.md) bilgi.

## <a name="create-the-app"></a>Uygulama oluşturma

1. Oluşturma bir *app.js* dosya ve kopyalayın ve aşağıdaki kodu yapıştırın.

    ```nodejs
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<username>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';

    var insertDocument = function(db, callback) {
    db.collection('families').insertOne( {
            "id": "AndersenFamily",
            "lastName": "Andersen",
            "parents": [
                { "firstName": "Thomas" },
                { "firstName": "Mary Kay" }
            ],
            "children": [
                { "firstName": "John", "gender": "male", "grade": 7 }
            ],
            "pets": [
                { "givenName": "Fluffy" }
            ],
            "address": { "country": "USA", "state": "WA", "city": "Seattle" }
        }, function(err, result) {
        assert.equal(err, null);
        console.log("Inserted a document into the families collection.");
        callback();
    });
    };
    
    var findFamilies = function(db, callback) {
    var cursor =db.collection('families').find( );
    cursor.each(function(err, doc) {
        assert.equal(err, null);
        if (doc != null) {
            console.dir(doc);
        } else {
            callback();
        }
    });
    };
    
    var updateFamilies = function(db, callback) {
    db.collection('families').updateOne(
        { "lastName" : "Andersen" },
        {
            $set: { "pets": [
                { "givenName": "Fluffy" },
                { "givenName": "Rocky"}
            ] },
            $currentDate: { "lastModified": true }
        }, function(err, results) {
        console.log(results);
        callback();
    });
    };
    
    var removeFamilies = function(db, callback) {
    db.collection('families').deleteMany(
        { "lastName": "Andersen" },
        function(err, results) {
            console.log(results);
            callback();
        }
    );
    };
    
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    **İsteğe bağlı**: kullanıyorsanız **MongoDB Node.js 2.2 sürücü**, lütfen aşağıdaki kod parçacığını değiştirin:

    Özgün:

    ```nodejs
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    İle değiştirilmelidir:

    ```nodejs
    MongoClient.connect(url, function(err, db) {
    assert.equal(null, err);
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                db.close();
            });
        });
        });
    });
    });
    ```
    
2. Aşağıdaki değişkenler değiştirme *app.js* hesap ayarlarınızı başına dosyası (nasıl bulacağınızı öğrenin, [bağlantı dizesi](connect-mongodb-account.md)):

    > [!IMPORTANT]
    > **MongoDB Node.js 3.0 sürücüsü** Cosmos DB parola özel karakter kodlama gerektirir. Karakter '=' % olarak kodlanacak emin olun 3B
    >
    > Örnek: Parola *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv ==* için kodlar *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv 3B % 3B*
    >
    > **MongoDB Node.js 2.2 sürücü** Cosmos DB parola özel karakter kodlama gerektirmez.
    >
    >
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Sık kullandığınız Terminali açın, çalıştırmak **npm kaydetme mongodb--yükleme**, uygulamanızı çalıştırın **düğümü app.js**

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi nasıl [MongoChef kullanmak](mongodb-mongochef.md) Azure Cosmos DB ile: API MongoDB hesabı.
