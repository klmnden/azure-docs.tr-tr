---
title: Azure - 5. bölüm için MongoDB, Angular ve Node Öğreticisi
description: Azure Cosmos DB üzerinde Angular ve Node ile MongoDB için kullandığınız aynı API'leri kullanarak bir MongoDB uygulaması oluşturma öğreticisi dizisinin 5. bölümü.
services: cosmos-db
author: johnpapa
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: jopapa
ms.custom: mvc
ms.openlocfilehash: 303c3553f3f1209aba99f28cb27308bd96347e42
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53087839"
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-5-use-mongoose-to-connect-to-azure-cosmos-db"></a>Angular ve Azure Cosmos DB ile bir MongoDB uygulaması oluşturma - 5. Bölüm: Azure Cosmos DB’ye bağlanmak için Mongoose kullanma

Bu çok bölümlü öğreticide Express ve Angular ile Node.js uygulaması oluşturma ve bunu bir [Azure Cosmos DB MongoDB API](mongodb-introduction.md) hesabına bağlama adımları gösterilmektedir.

Öğreticinin 5. bölümünde [4. bölümdeki](tutorial-develop-mongodb-nodejs-part4.md) konular genişletilir ve aşağıdaki görevler yer alır:

> [!div class="checklist"]
> * Azure Cosmos DB’ye bağlanmak için Mongoose kullanma
> * Cosmos DB bağlantı dizesi bilgilerinizi alma
> * Hero modelini oluşturma
> * Hero verilerini almak için hero hizmetini oluşturma
> * Uygulamayı yerel olarak çalıştırma

## <a name="video-walkthrough"></a>Görüntülü kılavuz

Bu belgede anlatılan adımları hızla öğrenmek için şu videoya göz atabilirsiniz: 

> [!VIDEO https://www.youtube.com/embed/sI5hw6KPPXI]


## <a name="prerequisites"></a>Önkoşullar

Öğreticinin bu bölümüne başlamadan önce öğreticinin [4. bölümündeki](tutorial-develop-mongodb-nodejs-part4.md) adımları tamamladığınızdan emin olun.

> [!TIP]
> Bu öğretici, uygulamanızı adım adım oluşturmanızı sağlayacak adımlarla size yol gösterir. Tamamlanmış projeyi indirmek isterseniz, tamamlanmış uygulamayı github'daki [angular-cosmosdb deposundan](https://github.com/Azure-Samples/angular-cosmosdb) alabilirsiniz.

## <a name="use-mongoose-to-connect-to-azure-cosmos-db"></a>Azure Cosmos DB’ye bağlanmak için Mongoose kullanma

1. mongoose npm modülünü yükleyin. Bu modül MongoDB ile anlaşmak için kullanılan bir API'dir.

    ```bash
    npm i mongoose --save
    ```

2. Şimdi **server** klasöründe **mongo.js** adlı yeni bir dosya oluşturun. Cosmos DB hesabınızın bağlantı bilgilerini bu dosyaya ekleyeceksiniz.

3. **mongo.js**’ye aşağıdaki kodları kopyalayın: Bu kod:

    * Mongoose gerektirir.

    * Mongo’daki promise yaklaşımını ES6/ES2015 ve üzeri sürümlerde yerleşik olarak bulunan temel promise yaklaşımıyla değiştirir.

    * Hazırlama, üretim veya geliştirme aşamalarında olmanıza bağlı olarak belirli seçenekleri ayarlamanızı sağlayan bir env dosyası çağırır. Bir sonraki bölümde bu dosyayı oluşturacaksınız.

    * env dosyasında ayarlanan MongoDB bağlantı dizemizi içerir.

    * Mongoose’u çağıran bir bağlanma işlevi oluşturur.

    ```javascript
    const mongoose = require('mongoose');
    /**
     * Set to Node.js native promises
     * Per https://mongoosejs.com/docs/promises.html
     */
    mongoose.Promise = global.Promise;

    const env = require('./env/environment');

    // eslint-disable-next-line max-len
    const mongoUri = `mongodb://${env.accountName}:${env.key}@${env.accountName}.documents.azure.com:${env.port}/${env.databaseName}?ssl=true`;

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
    // TODO: replace if yours are different
    module.exports = {
      accountName: 'your-cosmosdb-account-name-goes-here',
      databaseName: 'admin', 
      key: 'your-key-goes-here',
      port: 10255
    };
    ```

## <a name="get-the-connection-string-information"></a>Bağlantı dizesi bilgilerini alın

1. **environment.js** içinde `port` değerini 10255 olarak değiştirin. (Cosmos DB bağlantı noktanızı Azure portalda bulabilirsiniz)

    ```javascript
    const port = 10255;
    ```

2. **environment.js** içinde `accountName` değerini [4. adımda](tutorial-develop-mongodb-nodejs-part4.md)oluşturduğunuz Azure Cosmos DB hesabıyla değiştirin. 

3. Terminal penceresinde aşağıdaki CLI komutunu kullanarak Azure Cosmos DB hesabı için birincil anahtarı alın: 

    ```azure-cli-interactive
    az cosmosdb list-keys --name <cosmosdb-name> -g myResourceGroup
    ```    
    
    * `<cosmosdb-name>`, [4. adımda](tutorial-develop-mongodb-nodejs-part4.md)oluşturduğunuz Azure Cosmos DB hesabının adıdır.

4. Birincil anahtarı `key` değeri olarak environment.js dosyasına kopyalayın.

    Uygulamanız artık Azure Cosmos DB’ye bağlanmak için gerekli tüm bilgilere sahiptir. Bu bilgiler portaldan da alınabilir. Daha fazla bilgi için bkz. [Özelleştirilecek MongoDB bağlantı dizesini alma](connect-mongodb-account.md#GetCustomConnection). Portaldaki kullanıcı adı, environments.js içindeki dbName değerine karşılık gelir. 

## <a name="create-a-hero-model"></a>Hero modeli oluşturma

1. Explorer bölmesinde **server** altında **hero.model.js** dosyasını oluşturun.

2. **hero.model.js**’ye aşağıdaki kodları kopyalayın: Bu kod aşağıdaki işlevleri sunar:

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

1. Explorer bölmesinde **server** altında **hero.service.js** dosyasını oluşturun.

2. **hero.service.js**’ye aşağıdaki kodları kopyalayın: Bu kod:

   * Yeni oluşturduğunuz modeli alır
   * Veritabanına bağlar
   * Tüm hero'ları döndüren bir sorgu tanımlamak için hero.find yöntemini kullanan bir docquery değişkeni oluşturur.
   * docquery.exec ile tüm hero'ları alma promise yaklaşımına sahip, yanıt durumu 200 olan bir sorgu çalıştırır. 
   * Durum 500 ise, hata iletisini geri gönderir
   * Modülleri kullandığımızdan, heroları alır. 

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

    Bu aşamada çağrı zincirini gözden geçirip hazırlayalım. İlk olarak düğüm sunucusu ayarlarının bulunduğu `index.js` dosyasına bakalım. Burada rotaların ayarlandığını ve tanımlandığını görebilirsiniz. routes.js dosyanız bundan sonra hero hizmetiyle etkileşimde bulunur ve getHeroes gibi işlevlerimizi almasını, isteği ve yanıtı geçirmesini belirtir. Burada her.service.js modeli alacak ve Mongo’ya bağlanacaktır. Ardından çağırdığımızda getHeroes’u başlatıp 200 yanıtını döndürür. Sonrasında da zincirden çıkar. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Şimdi uygulamayı yeniden çalıştırın. Visual Studio Code’da tüm değişikliklerinizi kaydedin, sol taraftaki **Hata ayıkla** düğmesine ![Visual Studio Code’da hata ayıkla simgesi](./media/tutorial-develop-mongodb-nodejs-part5/debug-button.png) tıklayın, ardından **Hata Ayıklamayı Başlat** düğmesine ![ Visual Studio Code’da Hata ayıklama simgesi](./media/tutorial-develop-mongodb-nodejs-part5/start-debugging-button.png) tıklayın.

3. Şimdi tarayıcıya geçelim, Geliştirici araçlarını ve Ağ sekmesini açıp ardından http://localhost:3000 hedefine gidelim. İşte uygulamamız burada.

    ![Azure Portal’daki yeni Azure Cosmos DB hesabı](./media/tutorial-develop-mongodb-nodejs-part5/azure-cosmos-db-heroes-app.png)

   Uygulamada henüz depolanan hero yok, ancak öğreticinin sonraki adımında put, push ve delete işlevlerini ekleyeceğiz. Böylece Azure Cosmos DB veritabanımıza Mongoose bağlantıları kullanarak kullanıcı arabiriminden hero'ları eklememiz, güncelleştirmemiz ve silmemiz mümkün olacak. 

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde şu görevleri tamamladınız:

> [!div class="checklist"]
> * Heroes uygulamanızı Azure Cosmos DB’ye bağlamak için Mongoose API'lerini kullandınız. 
> * Uygulamaya get heroes işlevini eklediniz

Uygulamaya Post, Put ve Delete işlevlerini eklemek için öğreticinin sonraki bölümüne geçebilirsiniz.

> [!div class="nextstepaction"]
> [Uygulamaya Post, Put ve Delete işlevleri ekleme](tutorial-develop-mongodb-nodejs-part6.md)
