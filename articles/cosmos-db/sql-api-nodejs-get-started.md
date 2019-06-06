---
title: SQL API'si için Azure Cosmos DB için node.js Öğreticisi
description: SQL API ile Azure Cosmos DB bağlantısı kurma ve sorgulama yapma adımlarını gösteren bir Node.js öğreticisi
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/05/2019
ms.author: dech
Customer intent: As a developer, I want to build a Node.js console application to access and manage SQL API account resources in Azure Cosmos DB, so that customers can better use the service.
ms.openlocfilehash: 61569159d83493bb5338f8eda5b9201ef9164143
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734575"
---
# <a name="tutorial-build-a-nodejs-console-app-with-the-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Öğretici: Azure Cosmos DB SQL API verileri yönetmek için JavaScript SDK ile bir Node.js konsol uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (Önizleme)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET core (Önizleme)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Bir geliştirici olarak, NoSQL belge verileri kullanan uygulamalar olabilir. Azure Cosmos DB SQL API hesabı, depolamak ve bu belge verilere erişmek için kullanabilirsiniz. Bu öğreticide, Azure Cosmos DB kaynaklarını oluşturmak ve bunları sorgulamak için bir Node.js konsol uygulaması oluşturma işlemini göstermektedir.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Oluşturma ve bir Azure Cosmos DB hesabına bağlanın.
> * Uygulamanızı ayarlama.
> * Bir veritabanı oluşturun.
> * Bir kapsayıcı oluşturun.
> * Öğeleri kapsayıcıya ekleyin.
> * Temel öğeleri, kapsayıcı ve veritabanı işlemleri.

## <a name="prerequisites"></a>Önkoşullar 

Aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [Ücretsiz Azure Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) v6.0.0 veya üzeri.

## <a name="create-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayın. 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>Node.js uygulamanızı ayarlama

Uygulamayı oluşturmak için kod yazmaya başlamadan önce uygulamanız için framework oluşturabilirsiniz. Framework kod Node.js uygulamanızı ayarlama için aşağıdaki adımları çalıştırın:

1. Sık kullandığınız terminali açın.
2. Node.js uygulamanızı kaydetmek istediğiniz klasör veya dizini bulun.
3. Aşağıdaki komutlarla iki adet boş JavaScript dosyası oluşturun:

   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```

   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```

4. Oluşturma ve başlatma bir `package.json` dosya. Aşağıdaki komutu kullanın:
   * ```npm init -y```

5. npm aracılığıyla @azure/cosmos modülünü yükleyin. Aşağıdaki komutu kullanın:
   * ```npm install @azure/cosmos --save```

## <a id="Config"></a>Uygulamanızın yapılandırmalarını ayarlama

Uygulamanızı var, Azure Cosmos DB'ye konuşabilirsiniz emin olmanız gerekir. Birkaç yapılandırma ayarlarını güncelleştirerek, aşağıdaki adımlarda gösterildiği gibi Azure Cosmos DB'ye konuşmasını ister ayarlayabilirsiniz:

1. Sık kullandığınız metin düzenleyicisinde ```config.js``` öğesini açın.

1. Aşağıdaki kod parçacığını kopyalayıp yapıştırın ve Azure Cosmos DB uç noktası URI ve birincil anahtarınıza ```config.endpoint``` ve ```config.primaryKey``` özelliklerini ayarlayın. Bu yapılandırmaların her ikisini de [Azure portalında](https://portal.azure.com) bulabilirsiniz.

   ![Azure portaldan anahtarları alma ekran görüntüsü][keys]

   ```javascript
   // ADD THIS PART TO YOUR CODE
   var config = {}

   config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
   config.primaryKey = "~your primary key here~";
   ``` 

1. Aşağıdaki ```database```, ```container``` ve ```items``` verilerini kopyalayıp ```config.endpoint``` ve ```config.primaryKey``` özelliklerini ayarladığınız ```config``` nesnenize yapıştırın. Veritabanınızda depolamak istediğiniz veriler zaten varsa, verileri burada tanımlamak yerine Azure Cosmos DB veri geçiş aracı kullanabilirsiniz. Aşağıdaki kod, config.js. dosyasına sahip olmalıdır:

   [!code-javascript[nodejs-get-started](~/cosmosdb-nodejs-get-started/config.js)]

   JavaScript SDK'sını kullanan genel koşulları *kapsayıcı* ve *öğesi*. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. 
   
   `module.exports = config;` dışarı aktarmak için isused kod, ```config``` nesne içinde başvurabilir, böylece ```app.js``` dosya.

## <a id="Connect"></a>Bir Azure Cosmos DB hesabına bağlanma

1. Bir metin düzenleyicisinde boş ```app.js``` dosyanızı açın. ```@azure/cosmos``` modülünü ve yeni oluşturduğunuz ```config``` modülünü içeri aktarmak için aşağıdaki kodu kopyalayıp yapıştırın.

   ```javascript
   // ADD THIS PART TO YOUR CODE
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   ```

1. Yeni bir CosmosClient oluşturmak için, önceden kaydedilen ```config.endpoint``` ve ```config.primaryKey``` öğelerini kullanmak amacıyla kodu kopyalayıp yapıştırın.

   ```javascript
   const config = require('./config');

   // ADD THIS PART TO YOUR CODE
   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
   ```
   
> [!Note]
> Bağlanma, **Cosmos DB öykünücüsü'nü**, özel bir bağlantı ilkesi oluşturarak SSL doğrulamayı devre dışı.
>   ```
>   const connectionPolicy = new cosmos.ConnectionPolicy ()
>   connectionPolicy.DisableSSLVerification = true
>
>   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey }, connectionPolicy });
>   ```

Artık Azure Cosmos DB istemcisini başlatmaya yarayacak koda sahip olduğunuza göre, Azure Cosmos DB kaynaklarıyla çalışmaya bakalım.

## <a name="create-a-database"></a>Veritabanı oluşturma

1. Veritabanı kimliği ve kapsayıcı kimliği ayarlamak için aşağıdaki kodu kopyalayıp yapıştırın Bu kimlikleri, Azure Cosmos DB istemcisinin doğru veritabanı ve kapsayıcı nasıl bulma şeklidir.

   ```javascript
   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   // ADD THIS PART TO YOUR CODE
   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };
   ```

   Bir veritabanını kullanarak oluşturulabilir `createIfNotExists` veya işlevi oluşturma **veritabanları** sınıfı. Veritabanı, kapsayıcılar genelinde bölümlenmiş öğelerin mantıksal bir kapsayıcısıdır. 

2. **createDatabase** ve **readDatabase** yöntemlerini kopyalayıp app.js dosyasında ```databaseId``` ve ```containerId``` tanımlarının altına yapıştırın. **createDatabase** işlevi mevcut değilse ```config``` nesnesiyle belirtilen ```FamilyDatabase``` kimliğine sahip yeni bir veritabanı oluşturur. **readDatabase** işlevi, veritabanının mevcut olduğundan emin olmak için veritabanı tanımını okur.

   ```javascript
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

3. Aşağıdaki kodu kopyalayın ve çıkış iletisini yazdıracak **exit** yardımcı işlevini eklemek için **createDatabase** ve **readDatabase** işlevlerini ayarladığınız yere yapıştırın. 

   ```javascript
   // ADD THIS PART TO YOUR CODE
   function exit(message) {
      console.log(message);
      console.log('Press any key to exit');
      process.stdin.setRawMode(true);
      process.stdin.resume();
      process.stdin.on('data', process.exit.bind(process, 0));
   };
   ```

4. Aşağıdaki kodu kopyalayıp **createDatabase** ve **readDatabase** işlevlerini çağırmak için **exit** işlevini ayarladığınız yere yapıştırın.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
   ```

   Bu noktada, ```app.js``` kodunuz şöyle görünmelidir:

   ```javascript
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };

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
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error) }`) });
   ```

5. Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

   ```bash 
   node app.js
   ```

## <a id="CreateContainer"></a>Bir kapsayıcı oluşturma

Böylece depolamak ve sorgulamak, ardından Azure Cosmos DB hesabı içinde bir kapsayıcı oluşturun. 

> [!WARNING]
> Fiyatlandırmaya olan bir kapsayıcı oluşturma. Ziyaret bizim [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/) beklenmesi gerekenler bilmesi.

Bir kapsayıcı kullanarak oluşturulabilir `createIfNotExists` veya işlevden oluşturma **kapsayıcıları** sınıfı.  Kapsayıcı öğelerden (SQL API kullanıldığında JSON belgeleri) ve ilişkili JavaScript uygulama mantığından oluşur.

1. **createContainer** ve **readContainer** işlevini kopyalayıp app.js dosyasında **readDatabase** işlevinin altına yapıştırın. **createContainer** işlevi mevcut değilse ```config``` nesnesiyle belirtilen ```containerId``` bilgisine sahip yeni bir kapsayıcı oluşturur. **readContainer** işlevi, kapsayıcının mevcut olduğundan emin olmak için kapsayıcı tanımını okur.

   ```javascript
   /**
   * Create the container if it does not exist
   */

   async function createContainer() {

    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId, partitionKey }, { offerThroughput: 400 });
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

1. Çağrının altındaki kodu kopyalayıp **readDatabase** hedefine yapıştırarak **createContainer** ve **readContainer** işlevlerini yürütün.

   ```javascript
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

   ```javascript
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };

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

    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId, partitionKey }, { offerThroughput: 400 });
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

1. Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

   ```bash 
   node app.js
   ```

## <a id="CreateItem"></a>Bir öğe oluşturun

Bir öğe oluşturma işlevi kullanılarak oluşturulabilir. **öğeleri** sınıfı. SQL API'sini kullanırken, öğeleri kullanıcı tanımlı (rastgele) JSON içeriği olan belgeleri olarak yansıtılan. Artık Azure Cosmos DB'ye bir öğe ekleyebilirsiniz.

1. **createFamilyItem** işlevini kopyalayıp **readContainer** işlevinin altına yapıştırın. **createFamilyItem** işlevi, ```config``` nesnesinde kaydedilen JSON verilerini içeren öğeleri oluşturur. Öğe oluşturulmadan önce aynı kimliğe sahip başka bir öğenin olup olmadığı kontrol edilir.

   ```javascript
   /**
   * Create family item
   */
   async function createFamilyItem(itemBody) {
      const { item } = await client.database(databaseId).container(containerId).items.upsert(itemBody);
      console.log(`Created family item with id:\n${itemBody.id}\n`);
   };
   ```

1. **createFamilyItem** işlevini yürütmek için **readContainer** çağrısının altına kodu kopyalayıp yapıştırın.

   ```javascript
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

1. Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

   ```bash 
   node app.js
   ```


## <a id="Query"></a>Azure Cosmos DB kaynaklarını sorgulama

Azure Cosmos DB, her bir kapsayıcıda depolanan JSON belgeleri zengin sorguları destekler. Aşağıdaki örnek kod, kapsayıcınızdaki belgeler için çalıştırabileceğiniz bir sorguyu gösterir.

1. **queryContainer** işlevini kopyalayıp app.js dosyasındaki **createFamilyItem** işlevinin altına yapıştırın. Azure Cosmos DB, aşağıda gösterildiği gibi SQL benzeri sorguları destekler.

   ```javascript
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

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec, {enableCrossPartitionQuery:true}).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
   };
   ```

1. **queryContainer** işlevini yürütmek için **createFamilyItem** çağrısının altına kodu kopyalayıp yapıştırın.

   ```javascript
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

1. Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın:

   ```bash 
   node app.js
   ```


## <a id="ReplaceItem"></a>Öğeyi değiştirin
Azure Cosmos DB, öğelerin içeriğini değiştirmeyi destekler.

1. **replaceFamilyItem** işlevini kopyalayıp app.js dosyasındaki **queryContainer** işlevinin altına yapıştırın. Alt öğenin 'grade' özelliğini 6 yerine 5 olarak değiştirdiğimize dikkat edin.

   ```javascript
   // ADD THIS PART TO YOUR CODE
   /**
   * Replace the item by ID.
   */
   async function replaceFamilyItem(itemBody) {
      console.log(`Replacing item:\n${itemBody.id}\n`);
      // Change property 'grade'
      itemBody.children[0].grade = 6;
     const { item } = await client.database(databaseId).container(containerId).item(itemBody.id, itemBody.Country).replace(itemBody);
   };
   ```

1. **replaceFamilyItem** işlevini yürütmek için **queryContainer** çağrısının altına kodu kopyalayıp yapıştırın. Ayrıca, öğenin başarılı bir şekilde değiştiğini doğrulamak üzere **queryContainer**'ı tekrar çağırmak için kodu ekleyin.

   ```javascript
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

1. Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın:

   ```bash 
   node app.js
   ```


## <a id="DeleteItem"></a>Öğeyi Sil

Azure Cosmos DB, JSON öğelerini silmeyi destekler.

1. **deleteFamilyItem** işlevini **replaceFamilyItem** işlevinin altına kopyalayıp yapıştırın.

   ```javascript
   /**
   * Delete the item by ID.
   */
   async function deleteFamilyItem(itemBody) {
     await client.database(databaseId).container(containerId).item(itemBody.id, itemBody.Country).delete(itemBody);
      console.log(`Deleted item:\n${itemBody.id}\n`);
   };
   ```

1. **deleteFamilyItem** işlevini yürütmek için ikinci **queryContainer** çağrısının altına kodu kopyalayın ve yapıştırın.

   ```javascript
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

1. Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

   ```bash 
   node app.js
   ```


## <a id="DeleteDatabase"></a>Veritabanını silme

Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar (kapsayıcılar, öğeler vb.) kaldırılır.

1. Veritabanını ve tüm alt kaynaklarını kaldırmak için **cleanup** işlevini kopyalayıp **deleteFamilyItem** işlevinin altına yapıştırın.

   ```javascript
   /**
   * Cleanup the database and container on completion
   */
   async function cleanup() {
     await client.database(databaseId).delete();
   }
   ```

1. **cleanup** işlevini yürütmek için **deleteFamilyItem** çağrısının altına kodu kopyalayıp yapıştırın.

   ```javascript
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

## <a id="Run"></a>Node.js uygulamanızı çalıştırın

Kodunuzun son hali şu şekilde olmalıdır:

[!code-javascript[nodejs-get-started](~/cosmosdb-nodejs-get-started/app.js)]

Terminalinizde ```app.js``` dosyanızı bulun ve komutu çalıştırın: 

```bash 
node app.js
```

Başlarken uygulamanızın çıktısını görmeniz gerekir. Çıktı aşağıdaki örnek metinle eşleşmelidir.

   ```
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
   ```

## <a id="GetSolution"></a>Eksiksiz Node.js öğreticisi çözümünü edinme 

Bu öğreticideki adımları tamamlama fırsatınız olmadıysa veya yalnızca kodu indirmek isterseniz [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started )'dan ulaşabilirsiniz. 

Bu makaledeki tüm kodu içeren alınırken başlangıç çözümü çalıştırmak için ihtiyacınız olacak: 

* Bir [Azure Cosmos DB hesabı][create-account]. 
* GitHub'da bulunan [Başlangıç](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started) çözümü. 

Proje bağımlılıklarınızı npm aracılığıyla yükleyin. Aşağıdaki komutu kullanın: 

* ```npm install``` 

Ardından ```config.js``` dosyasında, config.endpoint ve config.primaryKey açıklandığı gibi güncelleştirin [3. adım: Uygulamanızın yapılandırmalarını ayarlama](#Config).  

Ardından terminalinizde ```app.js``` dosyanızı bulun ve şu komutu çalıştırın:  

```bash  
node app.js 
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklar artık gerekli olmadığında kaynak grubunu, Azure Cosmos DB hesabı ve tüm ilgili kaynakları silin. Bunu yapmak için Azure Cosmos DB hesabı için select kullandığınız kaynak grubunu seçin. **Sil**ve ardından silmek için kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir Azure Cosmos DB hesabı izleme](monitor-accounts.md)

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png
