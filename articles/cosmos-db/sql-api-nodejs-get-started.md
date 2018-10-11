---
title: Azure Cosmos DB için SQL DB API’si Node.js öğreticisi | Microsoft Docs
description: SQL API ile Azure Cosmos DB bağlantısı kurma ve sorgulama yapma adımlarını gösteren bir Node.js öğreticisi
services: cosmos-db
author: deborahc
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: dech
ms.openlocfilehash: affa302c7fd2a0cb05a6d599050e72c75ef74479
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969076"
---
# <a name="nodejs-tutorial-create-a-nodejs-console-application-with-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Node.js öğreticisi: Azure Cosmos DB SQL API verilerini yönetmek için JavaScript SDK ile bir Node.js konsol uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 


Azure Cosmos DB JavaScript SDK'sı için Node.js öğreticisine hoş geldiniz! Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Şu konulara değineceğiz:

* Azure Cosmos DB hesabı oluşturma ve hesaba bağlanma
* Uygulamanızı kurma
* Veritabanı oluşturma
* Kapsayıcı oluşturma
* Kapsayıcıya JSON öğelerini ekleme
* Kapsayıcıyı sorgulama
* Bir öğeyi değiştirme
* Bir öğeyi silme
* Veritabanını silme

Zamanınız yok mu? Endişelenmeyin! Eksiksiz çözümü [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started )'da bulabilirsiniz. Hızlı yönergeler için bkz. [Eksiksiz çözüm edinme](#GetSolution).

Node.js öğreticisini tamamladıktan sonra, bize geri bildirim sağlamak için lütfen bu sayfanın üst ve alt kısmındaki oylama düğmelerini kullanın. Doğrudan sizinle iletişim kurmamızı isterseniz yorumlarınıza e-posta adresinizi ekleyin.

Şimdi başlayalım!

## <a name="prerequisites-for-the-nodejs-tutorial"></a>Node.js öğreticisi için önkoşullar

Aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [Ücretsiz Azure Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) sürüm v6.0.0 veya üzeri.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayın. 

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
4. npm aracılığıyla @azure/cosmos modülünü yükleyin. Aşağıdaki komutu kullanın:
   * ```npm install @azure/cosmos --save```

Harika! Kurulumu tamamladığınıza göre, biraz kod yazmaya başlayalım.

## <a id="Config"></a>3. Adım: Uygulamanızın yapılandırmalarını ayarlama

Sık kullandığınız metin düzenleyicisinde ```config.js``` öğesini açın.

Ardından, aşağıdaki kod parçacığını kopyalayıp yapıştırın ve Azure Cosmos DB uç noktası URI ve birincil anahtarınıza ```config.endpoint``` ve ```config.primaryKey``` özelliklerini ayarlayın. Bu yapılandırmaların her ikisini de [Azure portalında](https://portal.azure.com) bulabilirsiniz.

![Node.js öğreticisi - ETKİN hub'ı vurgulanmış, Azure Cosmos DB hesabı dikey penceresinde ANAHTARLAR düğmesi vurgulanmış ve Anahtarlar dikey penceresinde URI, BİRİNCİL ANAHTAR ve İKİNCİL ANAHTAR değerleri vurgulanmış bir Azure Cosmos DB hesabını gösteren Azure portalının ekran görüntüsü - Node veritabanı][keys]

```nodejs
// ADD THIS PART TO YOUR CODE
var config = {}

config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
config.primaryKey = "~your primary key here~";
``` 

Aşağıdaki ```database```, ```container``` ve ```items``` verilerini kopyalayıp ```config.endpoint``` ve ```config.primaryKey``` özelliklerini ayarladığınız ```config``` nesnenize yapıştırın. Veritabanınızda depolamak istediğiniz veriler zaten varsa buradaki verileri tanımlamak yerine Azure Cosmos DB'nin [Veri Geçiş Aracı](import-data.md)'nı kullanabilirsiniz.

```nodejs
var config = {}

config.endpoint = "~your Azure Cosmos DB account endpoint uri here~";
config.primaryKey = "~your primary key here~";

config.database = {
    "id": "FamilyDatabase"
};

config.container = {
    "id": "FamilyContainer"
};

config.items = {
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
```
JavaScript SDK'sının eski sürümlerini kullandıysanız 'koleksiyon' ve 'belge' terimlerine aşina olabilirsiniz. Azure Cosmos DB [birden çok API modelini](https://docs.microsoft.com/azure/cosmos-db/introduction#key-capabilities) desteklediğinden JavaScript SDK'sının 2.0 ve üzeri sürümleri 'kapsayıcı' ve 'öğe' genel terimlerini kullanır. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. 

Son olarak, ```app.js``` dosyasının içinde başvurabilmek için ```config``` nesnenizi dışarı aktarın.
```nodejs
        },
        "isRegistered": false
    }
};

// ADD THIS PART TO YOUR CODE
module.exports = config;
```
## <a id="Connect"></a> 4. Adım: Azure Cosmos DB hesabına bağlanma

Bir metin düzenleyicisinde boş ```app.js``` dosyanızı açın. ```@azure/cosmos``` modülünü ve yeni oluşturduğunuz ```config``` modülünü içeri aktarmak için aşağıdaki kodu kopyalayıp yapıştırın.

```nodejs
// ADD THIS PART TO YOUR CODE
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');
```

Yeni bir CosmosClient oluşturmak için, önceden kaydedilen ```config.endpoint``` ve ```config.primaryKey``` öğelerini kullanmak amacıyla kodu kopyalayıp yapıştırın.

```nodejs
const url = require('url');

// ADD THIS PART TO YOUR CODE
const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
```

Artık Azure Cosmos DB istemcisini başlatmaya yarayacak koda sahip olduğunuza göre, Azure Cosmos DB kaynaklarıyla çalışmaya bakalım.

## <a name="step-5-create-a-database"></a>5. Adım: Veritabanı oluşturma

Not Found, database id ve container id için HTTP durumunu ayarlamak üzere aşağıdaki kodu kopyalayıp yapıştırın. Bu id değerleri, Azure Cosmos DB istemcisinin doğru veritabanı ve kapsayıcıyı bulma şeklidir.

```nodejs
const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

// ADD THIS PART TO YOUR CODE
const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;
```

[Veritabanı](sql-api-resources.md#databases), **Databases** sınıfının [createIfNotExists](/javascript/api/%40azure/cosmos/databases) veya [create](/javascript/api/%40azure/cosmos/databases) işlevi kullanılarak oluşturulabilir. Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır. 

**createDatabase** ve **readDatabase** işlevlerini kopyalayıp app.js dosyasında ```databaseId``` ve ```containerId``` altına yapıştırın. **createDatabase** işlevi mevcut değilse ```config``` nesnesiyle belirtilen ```FamilyDatabase``` kimliğine sahip yeni bir veritabanı oluşturur. **readDatabase** işlevi, veritabanının mevcut olduğundan emin olmak için veritabanı tanımını okur.

```nodejs
/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}
```

Aşağıdaki kodu kopyalayın ve çıkış iletisini yazdıracak **exit** yardımcı işlevini eklemek için **createDatabase** ve **readDatabase** işlevlerini ayarladığınız yere yapıştırın. 

```nodejs
// ADD THIS PART TO YOUR CODE
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
};
```
Aşağıdaki kodu kopyalayıp **createDatabase** ve **readDatabase** işlevlerini çağırmak için **exit** işlevini ayarladığınız yere yapıştırın.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
```

Bu noktada, ```app.js``` kodunuz şöyle görünmelidir:

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;


/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 
```bash 
node app.js
```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB veritabanı oluşturdunuz.

## <a id="CreateContainer"></a>6. Adım: Kapsayıcı oluşturma

> [!WARNING]
> **createContainer** işlevini çağırdığınızda yeni bir kapsayıcı oluşturulur ve ücret tahsil edilir. Daha ayrıntılı bilgi için [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.

Kapsayıcı, **Containers** sınıfının [createIfNotExists](/javascript/api/%40azure/cosmos/containers) veya [create](/javascript/api/%40azure/cosmos/containers) işlevi kullanılarak oluşturulabilir. 

Kapsayıcı öğelerden (SQL API kullanıldığında JSON belgeleri) ve ilişkili JavaScript uygulama mantığından oluşur.

**createContainer** ve **readContainer** işlevini kopyalayıp app.js dosyasında **readDatabase** işlevinin altına yapıştırın. **createContainer** işlevi mevcut değilse ```config``` nesnesiyle belirtilen ```containerId``` bilgisine sahip yeni bir kapsayıcı oluşturur. **readContainer** işlevi, kapsayıcının mevcut olduğundan emin olmak için kapsayıcı tanımını okur.

```nodejs
/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}
```

Çağrının altındaki kodu kopyalayıp **readDatabase** hedefine yapıştırarak **createContainer** ve **readContainer** işlevlerini yürütün.
```nodejs
createDatabase()
    .then(() => readDatabase())

    // ADD THIS PART TO YOUR CODE
    .then(() => createContainer())
    .then(() => readContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Bu noktada, ```app.js``` kodunuz şöyle görünmelidir:

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

```bash 
node app.js
```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB kapsayıcısı oluşturdunuz.

## <a id="CreateItem"></a>7. Adım: Öğe oluşturma

**Items** sınıfının [create](/javascript/api/%40azure/cosmos/items) işlevini kullanarak bir öğe oluşturabilirsiniz. SQL API'sini kullanarak öğeler, kullanıcı tanımlı (rastgele) JSON içeriği olan belgeler olarak görüntülenir. Artık Azure Cosmos DB'ye bir öğe ekleyebilirsiniz.

**createFamilyItem** işlevini kopyalayıp **readContainer** işlevinin altına yapıştırın. **createFamilyItem** işlevi, ```config``` nesnesinde kaydedilen JSON verilerini içeren öğeleri oluşturur. Öğe oluşturulmadan önce aynı kimliğe sahip başka bir öğenin olup olmadığı kontrol edilir.

```nodejs
/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};
```

**createFamilyItem** işlevini yürütmek için **readContainer** çağrısının altına kodu kopyalayıp yapıştırın.
```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

```bash 
node app.js
```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB öğesi oluşturdunuz.


## <a id="Query"></a>8. Adım: Azure Cosmos DB kaynaklarını sorgulama
Azure Cosmos DB, her bir kapsayıcıda depolanan JSON belgeleri için [zengin sorguların](sql-api-sql-query.md) gerçekleştirilmesini destekler. Aşağıdaki örnek kod, kapsayıcınızdaki belgeler için çalıştırabileceğiniz bir sorguyu gösterir.

**queryContainer** işlevini kopyalayıp app.js dosyasındaki **createFamilyItem** işlevinin altına yapıştırın. Azure Cosmos DB, aşağıda gösterildiği gibi SQL benzeri sorguları destekler. Karmaşık sorgular derleme hakkında daha fazla bilgi için [Query Playground](https://www.documentdb.com/sql/demo) sayfasını ve [sorgu belgelerini](sql-api-sql-query.md) inceleyin.

```nodejs
/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};
```

**queryContainer** işlevini yürütmek için **createFamilyItem** çağrısının altına kodu kopyalayıp yapıştırın.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın:

```bash 
node app.js
```

Tebrikler! Başarılı bir şekilde Azure Cosmos DB öğelerinizi sorguladınız.

## <a id="ReplaceItem"></a>9. Adım: Bir öğeyi değiştirme
Azure Cosmos DB, öğelerin içeriğini değiştirmeyi destekler.

**replaceFamilyItem** işlevini kopyalayıp app.js dosyasındaki **queryContainer** işlevinin altına yapıştırın. Alt öğenin 'grade' özelliğini 6 yerine 5 olarak değiştirdiğimize dikkat edin.

```nodejs
// ADD THIS PART TO YOUR CODE
/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};
```
**replaceFamilyItem** işlevini yürütmek için **queryContainer** çağrısının altına kodu kopyalayıp yapıştırın. Ayrıca, öğenin başarılı bir şekilde değiştiğini doğrulamak üzere **queryContainer**'ı tekrar çağırmak için kodu ekleyin.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```
Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın:

```bash 
node app.js
```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB öğesini değiştirdiniz.

## <a id="DeleteItem"></a>10. Adım: Bir öğeyi silme

Azure Cosmos DB, JSON öğelerini silmeyi destekler.

**deleteFamilyItem** işlevini **replaceFamilyItem** işlevinin altına kopyalayıp yapıştırın.

```nodejs
/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};
```

**deleteFamilyItem** işlevini yürütmek için ikinci **queryContainer** çağrısının altına kodu kopyalayın ve yapıştırın.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer
    ())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyItem(config.items.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

```bash 
node app.js
```

Tebrikler! Başarılı bir şekilde bir Azure Cosmos DB öğesini sildiniz.

## <a id="DeleteDatabase"></a>11. Adım: Veritabanını silme

Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar (kapsayıcılar, öğeler vb.) kaldırılır.

Veritabanını ve tüm alt kaynaklarını kaldırmak için **cleanup** işlevini kopyalayıp **deleteFamilyItem** işlevinin altına yapıştırın.

```nodejs
/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}
```

**cleanup** işlevini yürütmek için **deleteFamilyItem** çağrısının altına kodu kopyalayıp yapıştırın.

```nodejs
createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

## <a id="Run"></a>12. Adım: Node.js uygulamanızı hep birlikte çalıştırın!

Kodunuzun son hali şu şekilde olmalıdır:
```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};

/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};

/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};

/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};

/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

```bash 
node app.js
```

Başlarken uygulamanızın çıktısını görmeniz gerekir. Çıktı aşağıdaki örnek metinle eşleşmelidir.

    Created database:
    FamilyDatabase

    Reading database:
    FamilyDatabase

    Created container:
    FamilyContainer

    Reading container:
    FamilyContainer

    Created family item with id:
    Anderson.1

    Created family item with id:
    Wakefield.7

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing item:
    Anderson.1

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleted item:
    Anderson.1

    Completed successfully
    Press any key to exit

Tebrikler! Node.js öğreticisini tamamladınız ve ilk Azure Cosmos DB konsol uygulamanızı oluşturdunuz!

## <a id="GetSolution"></a>Eksiksiz Node.js öğreticisi çözümünü edinme

Bu öğreticideki adımları tamamlama fırsatınız olmadıysa veya yalnızca kodu indirmek isterseniz [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started )'dan ulaşabilirsiniz.

Bu makaledeki tüm kodları içeren başlangıç çözümünü çalıştırmak için aşağıdakilere ihtiyacınız vardır:

* Bir [Azure Cosmos DB hesabı][create-account].
* GitHub'da bulunan [Başlangıç](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started) çözümü.

npm aracılığıyla **@azure/cosmos** modülünü yükleyin. Aşağıdaki komutu kullanın:

* ```npm install @azure/cosmos --save```

Ardından, ```config.js``` dosyasında config.endpoint ve config.primaryKey değerlerini [3. Adım: Uygulamanızın yapılandırmalarını ayarlama](#Config) bölümünde açıklandığı gibi güncelleştirin. 

Ardından terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın: 

```bash 
node app.js
```

Hepsi bu kadar, devam edebilirsiniz! 

## <a name="next-steps"></a>Sonraki adımlar
* Daha karmaşık bir Node.js örneği ister misiniz? Bkz. [Azure Cosmos DB kullanarak bir Node.js web uygulaması oluşturma](sql-api-nodejs-application.md).
* [Azure Cosmos DB hesabını nasıl izleyebileceğinizi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png