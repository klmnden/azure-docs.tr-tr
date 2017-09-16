---
title: "Azure için MongoDB, Angular ve Node öğreticisi - 5. Bölüm | Microsoft Belgeleri"
description: "Azure Cosmos DB üzerinde Angular ve Node ile MongoDB için kullandığınız aynı API'leri kullanarak bir MongoDB uygulaması oluşturma öğreticisi dizisinin 5. bölümü."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 09/05/2017
ms.author: mimig
ms.translationtype: HT
ms.sourcegitcommit: 4eb426b14ec72aaa79268840f23a39b15fee8982
ms.openlocfilehash: e752e18f6d579633c0cf553224ae7617b774ad0f
ms.contentlocale: tr-tr
ms.lasthandoff: 09/06/2017

---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-5-use-mongoose-to-connect-to-azure-cosmos-db"></a>Angular ve Azure Cosmos DB ile bir MongoDB uygulaması oluşturma - 5. Bölüm: Azure Cosmos DB’ye bağlanmak için Mongoose kullanma

Bu çok bölümlü öğretici, Express, Angular ve Azure Cosmos DB veritabanınız ile Node.js kullanılarak yazılmış yeni bir [MongoDB API](mongodb-introduction.md) uygulamasının nasıl oluşturulacağını gösterir.

Öğreticinin 5. bölümünde [4. bölümdeki](tutorial-develop-mongodb-nodejs-part4.md) konular genişletilir ve aşağıdaki görevler yer alır:

> [!div class="checklist"]
> * Azure Cosmos DB’ye bağlanmak için Mongoose kullanma
> * Azure Cosmos DB'den bağlantı dizesi bilgilerini alma
> * Hero modelini oluşturma
> * Hero verilerini almak için hero hizmetini oluşturma
> * Uygulamayı yerel olarak çalıştırma

## <a name="video-walkthrough"></a>Görüntülü kılavuz

> [!VIDEO https://www.youtube.com/embed/sI5hw6KPPXI]


## <a name="prerequisites"></a>Ön koşullar

Öğreticinin bu bölümüne başlamadan önce öğreticinin [4. bölümündeki](tutorial-develop-mongodb-nodejs-part4.md) adımları tamamladığınızdan emin olun.

> [!TIP]
> Bu öğretici, uygulamanızı adım adım oluşturmanızı sağlayacak adımlarla size yol gösterir. Tamamlanmış projeyi indirmek isterseniz, tamamlanmış uygulamayı github'daki [angular-cosmosdb deposundan](https://github.com/Azure-Samples/angular-cosmosdb) alabilirsiniz.

## <a name="use-mongoose-to-connect-to-azure-cosmos-db"></a>Azure Cosmos DB’ye bağlanmak için Mongoose kullanma

1. mongoose npm modülünü yükleyin. Bu modül normalde MongoDB ile anlaşmak için kullanılan bir API'dir.

    ```bash
    npm i mongoose --save
    ```

2. Şimdi **server** klasörünüzde **mongo.js** adlı yeni bir dosya oluşturun. Bu dosyada, tüm Azure Cosmos DB veritabanı bağlantısı bilgilerinizi eklersiniz.

3. **mongo.js**’ye aşağıdaki kodları kopyalayın: Bu kod:
    * Mongoose gerektirir.
    * Mongo’daki promise yaklaşımını ES6/ES2015 ve sonrasında yerleşik olarak bulunan temel promise yaklaşımıyla değiştirir.
    * Hazırlama, üretim veya geliştirme aşamalarında olmanıza bağlı olarak belirli seçenekleri ayarlamanızı sağlayan bir env dosyası çağırır. Bu dosyayı yakında oluşturacağız.
    * Env dosyasında ayarlanacak MongoDB bağlantı dizemizi içerir.
    * Mongoose’u çağıran bir bağlanma işlevi oluşturur.

    ```javascript
    const mongoose = require('mongoose');
    /**
     * Set to Node.js native promises
     * Per http://mongoosejs.com/docs/promises.html
     */
    mongoose.Promise = global.Promise;

    const env = require('./env/environment');

    // eslint-disable-next-line max-len
    const mongoUri = `mongodb://${env.dbName}:${env.key}@${env.dbName}.documents.azure.com:${env.cosmosPort}/?ssl=true`; //&replicaSet=globaldb`;

    function connect() {
     mongoose.set('debug', true);
     return mongoose.connect(mongoUri, { useMongoClient: true });
    }

    module.exports = {
      connect,
      mongoose
    };
    ```
    
4. Explorer bölmesinde, **server** altında **environment** adlı bir klasör oluşturun ve **environment** klasöründe **environment.js** adlı yeni dosyayı oluşturun.

5. Mongo.js dosyasına `dbName`, `key` ve `cosmosPort` bölümlerini dahil etmemiz gerektiğini biliyoruz, bu nedenle aşağıdaki kodu **environment.js** dosyasına kopyalayın.

    ```javascript
    const cosmosPort = 1234; // replace with your port
    const dbName = 'your-cosmos-db-name-goes-here';
    const key = 'your-key-goes-here';

    module.exports = {
      dbName,
      key,
      cosmosPort
    };
    ```

## <a name="get-the-connection-string-information"></a>Bağlantı dizesi bilgilerini alın

1. **environment.js** içinde `cosmosPort` değerini 10255 olarak değiştirin. (Cosmos DB bağlantı noktanızı Azure Portalında bulabilirsiniz)

    ```javascript
    const cosmosPort = 10255;
    ```

2. **environment.js** içinde `dbName` değerini [4. adımda](tutorial-develop-mongodb-nodejs-part4.md)oluşturduğunuz Azure Cosmos DB hesabıyla değiştirin. 

3. Terminal penceresinde aşağıdaki CLI komutunu kullanarak Azure Cosmos DB hesabı için birincil anahtarı alın: 

    ```azure-cli-interactive
    az cosmosdb list-keys --name <cosmosdb-name> -g myResourceGroup
    ```    
    
    * `<cosmosdb-name>`, [4. adımda](tutorial-develop-mongodb-nodejs-part4.md)oluşturduğunuz Azure Cosmos DB hesabının adıdır.

4. Birincil anahtarı `key` değeri olarak environment.js dosyasına kopyalayın.

    Uygulamanız artık Azure Cosmos DB’ye bağlanmak için gerekli tüm bilgilere sahiptir. Bu bilgiler portaldan da alınabilir. Daha fazla bilgi için bkz. [Özelleştirilecek MongoDB bağlantı dizesini alma](connect-mongodb-account.md#GetCustomConnection). Portaldaki kullanıcı adı, environments.js içindeki dbName değerine karşılık gelir. 

## <a name="create-a-hero-model"></a>Hero modeli oluşturma

1.  Explorer bölmesinde **server** altında **hero.model.js** dosyasını oluşturun.

2. **hero.model.js**’ye aşağıdaki kodları kopyalayın: Bu kod:
   * Mongoose gerektirir.
   * Bir kimliği, adı ve deyişi olan yeni bir şema oluşturur.
   * Şemayı kullanarak bir model oluşturur.
   * Modeli dışarı aktarır. 
   * Koleksiyonu Heroes (Mongoose çoğul adlandırma kurallarına göre koleksiyonunun varsayılan adı Heros olduğundan) olarak adlandırın.

   ```javascript
   const mongoose = require('mongoose');

   const Schema = mongoose.Schema;

   const heroSchema = new Schema(
     {
       id: { type: Number, required: true, unique: true },
       name: String,
       saying: String
     },
     {
       collection: 'Heroes'
     }
   );

   const Hero = mongoose.model('Hero', heroSchema);

   module.exports = Hero;
   ```

## <a name="create-a-hero-service"></a>Hero hizmeti oluşturma

1.  Explorer bölmesinde **server** altında **hero.service.js** dosyasını oluşturun.

2. **hero.service.js**’ye aşağıdaki kodları kopyalayın: Bu kod:
   * Yeni oluşturduğunuz modeli alır
   * Veritabanına bağlar
   * Tüm hero'ları döndüren bir sorgu tanımlamak için hero.find yöntemini kullanan bir docquery değişkeni oluşturur.
   * docquery.exec ile tüm hero'ları alma promise yaklaşımına sahip, yanıt durumu 200 olan bir sorgu çalıştırır. 
   * Durum 500 ise, hata iletisini geri gönderir
   * Modüller kullandığımızdan, tüm heroları alır. 

   ```javascript
   const Hero = require('./hero.model');

   require('./mongo').connect();

   function getHeroes() {
     const docquery = Hero.find({});
     docquery
       .exec()
       .then(heroes => {
         res.status(200).json(heroes);
       })
       .catch(error => {
         res.status(500).send(error);
         return;
       });
   }

   module.exports = {
     getHeroes
   };
   ```

## <a name="add-the-hero-service-to-routesjs"></a>Hero hizmetini routes.js dosyasına ekleme

1. Visual Studio Code içinde **routes.js**’de, örnek hero verilerini gönderen `res.send` işlevini yorum ekleyerek devre dışı bırakın ve `heroService.getHeroes` işlevini çağıracak bir satır ekleyin.

    ```javascript
    router.get('/heroes', (req, res) => {
      heroService.getHeroes(req, res);
    //  res.send(200, [
    //      {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    //  ])
    });
    ```

2. **routes.js** dosyasında hero’yu gerekli kılın:

    ```javascript
    const heroService = require('./hero.service'); 
    ```

3. **hero.service.js** dosyasında, getHeroes işlevini `req` ve `res` parametrelerini alacak şekilde aşağıdaki gibi güncelleştirin:

    ```javascript
    function getHeroes(req, res) {
    ```

    Bu aşamada çağrı zincirini gözden geçirip hazırlayalım. İlk olarak `index.js` dosyasına geliyoruz, burada node sunucusu ayarlanıyor. Rotalarımızı ayarlayıp tanımladığına dikkat edin. routes.js dosyamız bundan sonra hero hizmetiyle etkileşimde bulunur ve getHeroes gibi işlevlerimizi almasını, isteği ve yanıtı geçirmesini belirtir. Burada her.service.js modeli alacak ve Mongo’ya bağlanacaktır. Ardından çağırdığımızda getHeroes’u başlatıp 200 yanıtını döndürür. Sonrasında da zincirden çıkar. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Şimdi uygulamayı yeniden çalıştırın. Visual Studio Code’da tüm değişikliklerinizi kaydedin, sol taraftaki **Hata ayıkla** düğmesine ![Visual Studio Code’da hata ayıkla simgesi](./media/tutorial-develop-mongodb-nodejs-part5/debug-button.png) tıklayın, ardından **Hata Ayıklamayı Başlat** düğmesine ![ Visual Studio Code’da Hata ayıklama simgesi](./media/tutorial-develop-mongodb-nodejs-part5/start-debugging-button.png) tıklayın.

3. Şimdi tarayıcıya geçelim, Geliştirici araçlarını ve Ağ sekmesini açıp ardından http://localhost:3000 hedefine gidelim. İşte uygulamamız burada.

    ![Azure Portal’daki yeni Azure Cosmos DB hesabı](./media/tutorial-develop-mongodb-nodejs-part5/azure-cosmos-db-heroes-app.png)

   Uygulamada henüz depolanan hero yok, ancak öğreticinin sonraki adımında put, push ve delete işlevlerini ekleyeceğiz. Böylece Azure Cosmos DB veritabanımıza Mongoose bağlantıları kullanarak kullanıcı arabiriminden hero'ları eklememiz, güncelleştirmemiz ve silmemiz mümkün olacak. 

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Heroes uygulamanızı Azure Cosmos DB’ye bağlamak için Mongoose API'lerini kullandınız. 
> * Uygulamaya get heroes işlevini eklediniz

Uygulamaya Post, Put ve Delete işlevlerini eklemek için öğreticinin sonraki bölümüne geçebilirsiniz.

> [!div class="nextstepaction"]
> [Uygulamaya Post, Put ve Delete işlevleri ekleme](tutorial-develop-mongodb-nodejs-part6.md)

