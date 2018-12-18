---
title: Bir Node.js uygulaması oluşturmak için Azure Cosmos DB MongoDB API'si kullanma
description: MongoDB için Azure Cosmos API’lerini kullanarak bir çevrimiçi veritabanı oluşturan öğretici.
keywords: mongodb örnekleri
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: sample
ms.date: 03/23/2018
ms.author: sngun
ms.openlocfilehash: 994371f716a2171622c6c534329764bcf7ac585c
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53534782"
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a>Bir Azure Cosmos DB oluşturun: Node.js kullanarak bir MongoDB uygulaması için API
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [MongoDB için Node.js](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Bu örnek, bir Azure Cosmos DB oluşturma işlemini göstermektedir: Node.js kullanarak MongoDB konsol uygulaması için API.

Bu örneği kullanmak için yapmanız gerekenler:

* [Oluşturma](create-mongodb-dotnet.md#create-account) bir Azure Cosmos DB: MongoDB hesabı için API.
* MongoDB [bağlantı dizesi](connect-mongodb-account.md) bilgilerinizi alın.

## <a name="create-the-app"></a>Uygulama oluşturma

1. Bir *app.js* dosyası oluşturun ve aşağıdaki kodu kopyalayıp yapıştırın.

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
    
    **İsteğe bağlı**: Kullanıyorsanız **MongoDB Node.js 2.2 sürücü**, lütfen aşağıdaki kod parçacığı değiştirin:

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
    
    Şununla değiştirilmelidir:

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
    
2. *app.js* dosyasında aşağıdaki değişkenleri hesap ayarlarınıza göre değiştirin ([Bağlantı dizenizi](connect-mongodb-account.md) nasıl bulacağınızı öğrenin):

    > [!IMPORTANT]
    > **MongoDB Node.js 3.0 sürücüsü** için Cosmos DB parolasındaki özel karakterlerin kodlanması gerekir. '=' karakterlerini %3D olarak kodladığınızdan emin olun
    >
    > Örnek: Parola *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv ==* için kodlar *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv 3B % 3B*
    >
    > **MongoDB Node.js 2.2 sürücüsü** için Cosmos DB parolasındaki özel karakterlerin kodlanması gerekmez.
    >
    >
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Tercih ettiğiniz terminali açın, **npm install mongodb --save** komutunu çalıştırın ve sonra da uygulamanızı **node app.js** ile çalıştırın

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [MongoChef kullanma](mongodb-mongochef.md) Azure Cosmos DB ile: MongoDB hesabı için API.
