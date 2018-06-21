---
title: Azure Cosmos DB için SQL DB API’si Node.js öğreticisi | Microsoft Docs
description: SQL API’si ile Cosmos DB oluşturan Node.js öğreticisi.
keywords: node.js öğreticisi, düğüm veritabanı
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/14/2017
ms.author: sngun
ms.openlocfilehash: 70bedfc26c900521dba8c6b211a4d4e4eda24e9c
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823699"
---
# <a name="nodejs-tutorial-use-the-sql-api-in-azure-cosmos-db-to-create-a-nodejs-console-application"></a>Node.js öğreticisi: Node.js konsol uygulaması oluşturmak için Azure Cosmos DB'de SQL API'si kullanma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [MongoDB için Node.js](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [C++](sql-api-cpp-get-started.md)

Azure Cosmos DB Node.js SDK'sı için Node.js öğreticisine hoş geldiniz! Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Şu konulara değineceğiz:

* Azure Cosmos DB hesabı oluşturma ve hesaba bağlanma
* Uygulamanızı kurma
* Düğüm veritabanı oluşturma
* Koleksiyon oluşturma
* JSON belgeleri oluşturma
* Koleksiyonu sorgulama
* Bir belgeyi değiştirme
* Bir belgeyi silme
* Düğüm veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started)'da bulabilirsiniz. Hızlı yönergeler için bkz. [Eksiksiz çözüm edinme](#GetSolution).

Node.js öğreticisini tamamladıktan sonra, bize geri bildirim sağlamak için lütfen bu sayfanın üst ve alt kısmındaki oylama düğmelerini kullanın. Doğrudan sizinle iletişim kurmamızı isterseniz yorumlarınıza e-posta adresinizi ekleyin.

Şimdi başlayalım!

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Node.js öğreticisi için önkoşullar

Lütfen aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [Ücretsiz Azure Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) v0.10.29 sürümü veya sonraki bir sürüm.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için lütfen [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayın.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>2. Adım: Node.js uygulamanızı ayarlama

1. Sık kullandığınız terminali açın.
2. Node.js uygulamanızı kaydetmek istediğiniz klasör veya dizini bulun.
3. Aşağıdaki komutlarla iki adet boş JavaScript dosyası oluşturun:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Npm aracılığıyla documentdb modülünü yükleyin. Aşağıdaki komutu kullanın:
   * ```npm install documentdb --save```

Harika! Kurulumu tamamladığınıza göre, biraz kod yazmaya başlayalım.

## <a id="Config"></a>3. Adım: Uygulamanızın yapılandırmalarını ayarlama

Sık kullandığınız metin düzenleyicisinde ```config.js``` öğesini açın.

Ardından, aşağıdaki kod parçacığını kopyalayıp yapıştırın ve Azure Cosmos DB uç noktası uri ve birincil anahtarınıza ```config.endpoint``` ve ```config.primaryKey``` özelliklerini ayarlayın. Bu yapılandırmaların her ikisini de [Azure portalında](https://portal.azure.com) bulabilirsiniz.

![Node.js öğreticisi - ETKİN hub'ı vurgulanmış, Azure Cosmos DB hesabı dikey penceresinde ANAHTARLAR düğmesi vurgulanmış ve Anahtarlar dikey penceresinde URI, BİRİNCİL ANAHTAR ve İKİNCİL ANAHTAR değerleri vurgulanmış bir Azure Cosmos DB hesabını gösteren Azure portalının ekran görüntüsü - Node veritabanı][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Aşağıdaki ```config.endpoint``` ve ```config.primaryKey``` özelliklerinizi ayarladığınız ```config``` nesnenize ```database id```, ```collection id``` ve ```JSON documents``` öğelerini kopyalayıp yapıştırın. Veritabanınızda depolamak istediğiniz veriler zaten varsa belge tanımları eklemek yerine Azure Cosmos DB'nin [Veri Geçiş Aracı](import-data.md)'nı kullanabilirsiniz.

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };

Veritabanı, koleksiyon ve belge tanımları Azure Cosmos DB ```database id```, ```collection id``` ve belgelerin verileri görevini görür.

Son olarak, ```app.js``` dosyasının içinde başvurabilmek için ```config``` nesnenizi dışarı aktarın.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

## <a id="Connect"></a> 4. Adım: Azure Cosmos DB hesabına bağlanma

Bir metin düzenleyicisinde boş ```app.js``` dosyanızı açın. ```documentdb``` modülünü ve yeni oluşturduğunuz ```config``` modülünü içeri aktarmak için aşağıdaki kodu kopyalayıp yapıştırın.

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    const uriFactory = require('documentdb').UriFactory;
    var config = require("./config");

Yeni bir DocumentClient oluşturmak için, önceden kaydedilen ```config.endpoint``` ve ```config.primaryKey``` öğelerini kullanmak amacıyla kodu kopyalayıp yapıştırın.

    var config = require("./config");

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Artık Azure Cosmos DB istemcisini başlatmaya yarayacak koda sahip olduğunuza göre, Azure Cosmos DB kaynaklarıyla çalışmaya bakalım.

## <a name="step-5-create-a-node-database"></a>5. Adım: Düğüm veritabanı oluşturma

Not Found, database id ve collection id için HTTP durumunu ayarlamak üzere aşağıdaki kodu kopyalayıp yapıştırın. Bu id değerleri, Azure Cosmos DB istemcisinin doğru veritabanı ve koleksiyonu bulma şeklidir.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseId = config.database.id;
    var collectionId = config.collection.id;

Bir [veritabanı](sql-api-resources.md#databases), **DocumentClient** sınıfının [createDatabase](/javascript/api/documentdb/documentclient) işlevi kullanılarak oluşturulabilir. Veritabanı, koleksiyonlar genelinde bölümlenmiş belge depolama alanının mantıksal bir kapsayıcısıdır.

```config``` nesnesinden ```databaseId``` belirtilmiş şekilde app.js dosyasında yeni veritabanınızı oluşturmak için **getDatabase** işlevini kopyalayın ve yapıştırın. İşlev, aynı ```FamilyRegistry``` kimliğine sahip bir veritabanının zaten var olup olmadığını denetler. Varsa yeni bir veritabanı oluşturmak yerine var olan veritabanını getireceğiz.

    // ADD THIS PART TO YOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${databaseId}\n`);
        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase({ id: databaseId }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Aşağıdaki kodu kopyalayın ve çıkış iletisini ve **getDatabase** işlevine çağrıyı yazdıracak **exit** yardımcı işlevini eklemek için **getDatabase** işlevini ayarladığınız yere yapıştırın.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    };

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: ```node app.js```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanı oluşturdunuz.

## <a id="CreateColl"></a>6. Adım: Koleksiyon oluşturma

> [!WARNING]
> **createCollection**, fiyatlandırmaya yönelik etkilere sahip yeni bir koleksiyon oluşturur. Daha ayrıntılı bilgi için lütfen [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.

Bir [koleksiyon](sql-api-resources.md#collections), **DocumentClient** sınıfının [createCollection](/javascript/api/documentdb/documentclient) işlevi kullanılarak oluşturulabilir. Koleksiyon, JSON belgelerinin ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır.

```config``` nesnesinden belirtilmiş ```collectionId``` ile yeni koleksiyonunuzu oluşturmak üzere **getCollection** işlevini kopyalayıp app.js dosyasındaki **getDatabase** işlevinin altına yapıştırın. Yine aynı ```FamilyCollection``` kimliğine sahip bir koleksiyonun zaten var olup olmadığını denetleyeceğiz. Varsa yeni bir koleksiyon oluşturmak yerine var olan koleksiyonu getireceğiz.

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${collectionId}\n`);
        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
                        client.createCollection(databaseUrl, { id: collectionId }, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

**getCollection** işlevini yürütmek için **getDatabase**'e yönelik çağrının altında bulunan kodu kopyalayın ve yapıştırın.

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: ```node app.js```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB koleksiyonu oluşturdunuz.

## <a id="CreateDoc"></a>7. Adım: Belge oluşturma

Bir [belge](sql-api-resources.md#documents), **DocumentClient** sınıfının [createDocument](/javascript/api/documentdb/documentclient) işlevi kullanılarak oluşturulabilir. Belgeler, kullanıcı tanımlı (rastgele) JSON içeriğidir. Artık Azure Cosmos DB'ye bir belge yerleştirebilirsiniz.

```config``` nesnesinde kaydedilen JSON verilerini içeren belgeleri oluşturmak için **getFamilyDocument** işlevini **getCollection** işlevinin altına kopyalayıp yapıştırın. Yine aynı kimliğe sahip bir belgenin zaten var olup olmadığını denetleyeceğiz.

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function getFamilyDocument(document) {
        console.log(`Getting document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

**getFamilyDocument** işlevini yürütmek için **getCollection** işlevine yönelik çağrının altına kodu kopyalayıp yapıştırın.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: ```node app.js```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belgesi oluşturdunuz.

![Node.js öğreticisi - Hesap, veritabanı, koleksiyon ve belgeler arasındaki hiyerarşik ilişkiyi gösteren diyagram - Node veritabanı](./media/sql-api-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <a id="Query"></a>8. Adım: Azure Cosmos DB kaynaklarını sorgulama
Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](sql-api-sql-query.md) gerçekleştirilmesini destekler. Aşağıdaki örnek kod, koleksiyonunuzdaki belgeler için çalıştırabileceğiniz bir sorguyu gösterir.

**queryCollection** işlevini kopyalayıp app.js dosyasındaki **getFamilyDocument** işlevinin altına yapıştırın. Azure Cosmos DB, aşağıda gösterildiği gibi SQL benzeri sorguları destekler. Karmaşık sorgular derleme hakkında daha fazla bilgi için [Query Playground](https://www.documentdb.com/sql/demo) sayfasını ve [sorgu belgelerini](sql-api-sql-query.md) inceleyin.

                } else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${collectionId}`);
        let collectionUrl = uriFactory.createDocumentCollectionUri(databaseId, collectionId);
        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };

Aşağıdaki diyagramda, Azure Cosmos DB SQL sorgusu söz diziminin oluşturduğunuz koleksiyonda nasıl çağrıldığı gösterilmektedir.

![Node.js öğreticisi - Sorgunun kapsamını ve sorgunun anlamını gösteren diyagram - Node veritabanı](./media/sql-api-nodejs-get-started/node-js-tutorial-collection-documents.png)

Azure Cosmos DB sorguları zaten tek bir koleksiyon kapsamında olduğundan, sorgudaki [FROM](sql-api-sql-query.md#FromClause) anahtar sözcüğü isteğe bağlıdır. Bu nedenle, "FROM Families f", "FROM root r" veya seçtiğiniz herhangi bir başka değişken adıyla değiştirilebilir. Azure Cosmos DB; Families, root veya seçtiğiniz değişken adının varsayılan olarak geçerli koleksiyona başvurduğu sonucuna varır.

**queryCollection** işlevini yürütmek için kodu kopyalayıp **getFamilyDocument**'a yönelik çağrının altına yapıştırın.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: ```node app.js```

Tebrikler! Başarılı bir şekilde Azure Cosmos DB belgelerini sorguladınız.

## <a id="ReplaceDocument"></a>9. Adım: Bir belgeyi değiştirme
Azure Cosmos DB, JSON belgelerini değiştirmeyi destekler.

**replaceFamilyDocument** işlevini kopyalayıp app.js dosyasındaki **queryCollection** işlevinin altına yapıştırın.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function replaceFamilyDocument(document) {
        console.log(`Replacing document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        document.children[0].grade = 6;
        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

**replaceDocument** işlevini yürütmek için **queryCollection**'a çağrının altına kodu kopyalayıp yapıştırın. Ayrıca, belgenin başarılı bir şekilde değiştiğini doğrulamak üzere **queryCollection**'ı tekrar çağırmak için kodu ekleyin.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: ```node app.js```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belgesini değiştirdiniz.

## <a id="DeleteDocument"></a>10. Adım: Bir belgeyi silme

Azure Cosmos DB, JSON belgelerini silmeyi destekler.

**deleteFamilyDocument** işlevini **replaceFamilyDocument** işlevinin altına kopyalayıp yapıştırın.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function deleteFamilyDocument(document) {
        console.log(`Deleting document:\n${document.id}\n`);
        let documentUrl = uriFactory.createDocumentUri(databaseId, collectionId, document.id);
        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

**deleteDocument** işlevini yürütmek için ikinci **queryCollection**'a çağrının altına kodu kopyalayın ve yapıştırın.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: ```node app.js```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB belgesini sildiniz.

## <a id="DeleteDatabase"></a>11. Adım: Node veritabanını silme

Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar (koleksiyonlar, belgeler vb.) kaldırılır.

Veritabanını ve tüm alt kaynaklarını kaldırmak için **cleanup** işlevini kopyalayıp **deleteFamilyDocument** işlevinin altına yapıştırın.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${databaseId}`);
        let databaseUrl = uriFactory.createDatabaseUri(databaseId);
        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    };

**cleanup** işlevini yürütmek için **deleteFamilyDocument**'a çağrının altına kodu kopyalayıp yapıştırın.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <a id="Run"></a>12. Adım: Node.js uygulamanızı hep birlikte çalıştırın!

İşlevlerinizi çağırma dizisinin bütünüyle şu şekilde görünmesi gerekir:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

Terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: ```node app.js```

Başlarken uygulamanızın çıktısını görmeniz gerekir. Çıktı aşağıdaki örnek metinle eşleşmelidir.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key to exit

Tebrikler! Node.js öğreticisini tamamladınız ve ilk Azure Cosmos DB konsol uygulamanızı oluşturdunuz!

## <a id="GetSolution"></a>Eksiksiz Node.js öğreticisi çözümünü edinme

Bu öğreticideki adımları tamamlama fırsatınız olmadıysa veya yalnızca kodu indirmek isterseniz [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started)'dan ulaşabilirsiniz.

Bu makaledeki tüm örnekleri içeren GetStarted çözümünü çalıştırmak için aşağıdakilere ihtiyacınız vardır:

* [Azure Cosmos DB hesabı][create-account].
* GitHub'da bulunan [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) çözümü.

Npm aracılığıyla **documentdb** modülünü yükleyin. Aşağıdaki komutu kullanın:

* ```npm install documentdb --save```

Ardından, ```config.js``` dosyasında config.endpoint ve config.primaryKey değerlerini [3. Adım: Uygulamanızın yapılandırmalarını ayarlama](#Config) bölümünde açıklandığı gibi güncelleştirin. 

Ardından terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: ```node app.js```.

Hepsi bu kadar, derleyin ve devam edin! 

## <a name="next-steps"></a>Sonraki adımlar
* Daha karmaşık bir Node.js örneği ister misiniz? Bkz. [Azure Cosmos DB kullanarak bir Node.js web uygulaması oluşturma](sql-api-nodejs-application.md).
* [Azure Cosmos DB hesabını nasıl izleyebileceğinizi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png
