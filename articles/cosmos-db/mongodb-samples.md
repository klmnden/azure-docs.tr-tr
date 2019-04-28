---
title: Bir Node.js uygulaması oluşturmak için Azure Cosmos DB'nin MongoDB için API kullanın
description: Azure Cosmos DB'nin MongoDB kullanarak çevrimiçi bir veritabanı oluşturan öğretici.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: sample
origin.date: 12/26/2018
ms.date: 03/04/2019
author: rockboyfor
ms.author: v-yeche
ms.openlocfilehash: 28ee64f70cd281a2563a855fb1fca91f229ec7bd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61330607"
---
# <a name="build-an-app-using-nodejs-and-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için node.js ve Azure Cosmos DB'nin API'sini kullanarak uygulama oluşturma 
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [MongoDB için Node.js](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Bu örnek, Node.js ve Azure Cosmos DB'nin MongoDB kullanarak bir konsol uygulaması oluşturma işlemini göstermektedir.

Bu örneği kullanmak için yapmanız gerekenler:

* [Oluşturma](create-mongodb-dotnet.md#create-account) MongoDB için Azure Cosmos DB'nin API'sini kullanmak üzere yapılandırılmış bir Cosmos hesabı.
* Almak, [bağlantı dizesi](connect-mongodb-account.md) bilgileri.

## <a name="create-the-app"></a>Uygulama oluşturma

1. Bir *app.js* dosyası oluşturun ve aşağıdaki kodu kopyalayıp yapıştırın.

    ```javascript
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<username>:<password>@<endpoint>.documents.azure.cn:10255/?ssl=true';

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

    ```javascript
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

    ```javascript
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
    > Örnek: The password *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv==* encodes to *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv%3D%3D*
    >
    > **MongoDB Node.js 2.2 sürücüsü** için Cosmos DB parolasındaki özel karakterlerin kodlanması gerekmez.
    >
    >

    ```javascript
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.cn:10255/?ssl=true';
    ```

3. Tercih ettiğiniz terminali açın, **npm install mongodb --save** komutunu çalıştırın ve sonra da uygulamanızı **node app.js** ile çalıştırın

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [Studio 3T kullanma](mongodb-mongochef.md) Azure Cosmos DB'nin MongoDB API'si ile.
- Bilgi edinmek için nasıl [Robo 3T kullanma](mongodb-robomongo.md) Azure Cosmos DB'nin MongoDB API'si ile.
- MongoDB keşfedin [örnekleri](mongodb-samples.md) Azure Cosmos DB'nin MongoDB API'si ile.

<!-- Update_Description: update meta properties, wording update -->