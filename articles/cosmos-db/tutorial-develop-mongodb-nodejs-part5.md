---
title: MongoDB - kullanım Cosmos DB'ye bağlanmak için Mongoose için Azure Cosmos DB API'si ile Angular uygulama oluşturma
titleSuffix: Azure Cosmos DB
description: Bu öğreticide, Cosmos DB'de depolanan verileri yönetmek için Angular ve Express kullanarak bir Node.js uygulaması oluşturma işlemini açıklamaktadır. Bu bölümünde Azure Cosmos DB'ye bağlanmak için Mongoose kullanma.
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: jopapa
ms.custom: seodec18
ms.reviewer: sngun
Customer intent: As a developer, I want to build a Node.js application, so that I can manage the data stored in Cosmos DB.
ms.openlocfilehash: c8cab3c723b7e507b0f3b05b933cca9e2c24fb39
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60767329"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb---use-mongoose-to-connect-to-cosmos-db"></a>MongoDB - kullanım Cosmos DB'ye bağlanmak için Mongoose için Azure Cosmos DB API'si ile Angular uygulama oluşturma

Bu çok bölümlü öğretici, Express ve Angular ile Node.js uygulaması oluşturma ve buna bağlanmak gösterilmektedir, [MongoDB için Cosmos DB API'si ile yapılandırılan Cosmos hesabı](mongodb-introduction.md). Bu makalede öğreticinin 5. bölüm ve yapılar [bölüm 4](tutorial-develop-mongodb-nodejs-part4.md).

Öğreticinin bu bölümünde şunları yapacaksınız:

> [!div class="checklist"]
> * Cosmos DB'ye bağlanmak için Mongoose kullanma.
> * Cosmos DB bağlantı dizenizi alın.
> * Hero modelini oluşturun.
> * Hero verilerini almak için Hero hizmetini oluşturma.
> * Uygulamayı yerel olarak çalıştırın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* Bu öğreticiye başlamadan önce bölümündeki adımları tamamlamanız [bölüm 4](tutorial-develop-mongodb-nodejs-part4.md).

* Bu öğretici, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Azure CLI 2.0 veya sonraki bir sürümünü yüklemiş olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

* Bu öğretici adım adım uygulaması oluşturma adımlarında size kılavuzluk eder. Tamamlanmış projeyi indirmek isterseniz, tamamlanmış uygulamayı github'daki [angular-cosmosdb deposundan](https://github.com/Azure-Samples/angular-cosmosdb) alabilirsiniz.

## <a name="use-mongoose-to-connect"></a>Bağlanmak için Mongoose kullanma

Mongoose MongoDB ve Node.js (ODM) kitaplığı modelleme nesnesi verilerdir. Azure Cosmos DB hesabınıza bağlanmak için Mongoose kullanabilirsiniz. Mongoose yüklemek ve Azure Cosmos DB'ye bağlanmak için aşağıdaki adımları kullanın:

1. MongoDB ile anlaşmak için kullanılan bir API'dir mongoose npm modülünü yükleyin.

    ```bash
    npm i mongoose --save
    ```

1. İçinde **sunucu** klasöründe adlı bir dosya oluşturun **mongo.js**. Bağlantı ayrıntılarını Azure Cosmos DB hesabınızın bu dosyaya ekleyeceksiniz.

1. Aşağıdaki kodu kopyalayın **mongo.js** dosya. Kod, aşağıdaki işlevleri sağlar:

   * Mongoose gerektirir.
   * Mongo'daki promise ES6/ES2015 ve üzeri sürümlere yerleşik olarak bulunan temel promise kullanılacak.
   * Hazırlama, üretim veya geliştirme olmanıza üzerinde dayalı belirli şeyleri olanak sağlayan bir env dosyası çağrılarda ayarlayın. Sonraki bölümde bu dosyayı oluşturacaksınız.
   * Env dosyasında ayarlanan MongoDB bağlantı dizesini içerir.
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
    
1. Explorer bölmesinde altında **sunucu**, adlı bir klasör oluşturun **ortam**. İçinde **ortam** klasöründe adlı bir dosya oluşturun **environment.js**.

1. İçin değerler içerecek şekilde ihtiyacımız mongo.js dosyasından `dbName`, `key`ve `cosmosPort` parametreleri. Aşağıdaki kodu kopyalayın **environment.js** dosyası:

    ```javascript
    // TODO: replace if yours are different
    module.exports = {
      accountName: 'your-cosmosdb-account-name-goes-here',
      databaseName: 'admin', 
      key: 'your-key-goes-here',
      port: 10255
    };
    ```

## <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Uygulamanızı Azure Cosmos DB'ye bağlanmak için uygulama yapılandırma ayarları güncelleştirmeniz gerekiyor. Ayarları güncelleştirmek için aşağıdaki adımları kullanın: 

1. Azure portalında Azure Cosmos DB hesabınız için bağlantı noktası numarası, Azure Cosmos DB hesap adı ve birincil anahtar değerlerini alın.

1. İçinde **environment.js** dosyası, değiştirin `port` değerini 10255 olarak. 

    ```javascript
    const port = 10255;
    ```

1. İçinde **environment.js** dosyası, değiştirin `accountName` oluşturduğunuz Azure Cosmos DB hesabı adını [bölüm 4](tutorial-develop-mongodb-nodejs-part4.md) Öğreticisi. 

1. Terminal penceresinde aşağıdaki CLI komutunu kullanarak Azure Cosmos DB hesabı için birincil anahtarı alın: 

    ```azure-cli-interactive
    az cosmosdb list-keys --name <cosmosdb-name> -g myResourceGroup
    ```    
    
    \<cosmosdb-name > oluşturduğunuz Azure Cosmos DB hesabının adıdır [bölüm 4](tutorial-develop-mongodb-nodejs-part4.md) Öğreticisi.

1. Birincil anahtarı kopyalayın **environment.js** Dosyala `key` değeri.

Artık uygulamanızı Azure Cosmos DB'ye bağlanmak için gerekli tüm bilgileri var. 

## <a name="create-a-hero-model"></a>Hero modeli oluşturma

Ardından, bir model dosyası tanımlayarak Azure Cosmos DB'de depolamak için veri şemasını tanımlamak gerekir. Oluşturmak için aşağıdaki adımları kullanın. bir _Hero modelini_ veri şeması tanımlayan:

1. Explorer bölmesinde altında **sunucu** klasöründe adlı bir dosya oluşturun **hero.model.js**.

1. Aşağıdaki kodu kopyalayın **hero.model.js** dosya. Kod, aşağıdaki işlevleri sağlar:

   * Mongoose gerektirir.
   * Bir kimliği, adı ve deyişi olan yeni bir şema oluşturur.
   * Şemayı kullanarak bir model oluşturur.
   * Modeli dışarı aktarır. 
   * Koleksiyon adları **hero'ları** (yerine **Heros**, Mongoose çoğul adlandırma kurallarına göre koleksiyonunun varsayılan adı).

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

Hero modelini oluşturduktan sonra verileri okur ve listede gerçekleştirmek için bir hizmet tanımlama oluşturma, silme ve güncelleştirme işlemlerinin. Oluşturmak için aşağıdaki adımları kullanın. bir _Hero hizmetini_ , Azure Cosmos DB'den verileri sorgular:

1. Explorer bölmesinde altında **sunucu** klasöründe adlı bir dosya oluşturun **hero.service.js**.

1. Aşağıdaki kodu kopyalayın **hero.service.js** dosya. Kod, aşağıdaki işlevleri sağlar:

   * Oluşturduğunuz modeli alır.
   * Veritabanına bağlar.
   * Oluşturur bir `docquery` kullanan değişkeni `hero.find` tüm hero'ları döndüren bir sorgu tanımlamak için yöntemi.
   * Bir sorgu çalıştırır `docquery.exec` yanıt durumu 200 bulunduğu bir liste tüm hero'ları alma promise ile işlevi. 
   * Durum 500 ise hata iletisini geri gönderir.
   * Modüller kullandığımızdan hero'ları alır. 

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

## <a name="configure-routes"></a>Yolları Yapılandır

Ardından, Get URL'leri işlemek için rotalar ayarlayabilir için oluşturma, okuma ve istekleri silin. Geri çağırma işlevleri yönlendirme yöntemleri belirtin (olarak da adlandırılan _işleyici işlevleri_). Bu işlevler uygulamanın belirtilen uç noktası ve HTTP yöntemi için bir istek aldığında çağrılır. Hero hizmetini ekleyip yollarınızı tanımlamak için aşağıdaki adımları kullanın:

1. Visual Studio Code içinde **routes.js** dosya, açıklama `res.send` örnek hero verilerini gönderen bir işlev. Çağırmak için bir satır ekleyin `heroService.getHeroes` işlevini.

    ```javascript
    router.get('/heroes', (req, res) => {
      heroService.getHeroes(req, res);
    //  res.send(200, [
    //      {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    //  ])
    });
    ```

1. İçinde **routes.js** dosyası `require` hero hizmeti:

    ```javascript
    const heroService = require('./hero.service'); 
    ```

1. İçinde **hero.service.js** dosyası, güncelleştirme `getHeroes` gerçekleştirilecek işlevi `req` ve `res` aşağıdaki gibi parametreleri:

    ```javascript
    function getHeroes(req, res) {
    ```

Şimdi gözden geçirmek ve önceki kod açıklaması için bir dakikanızı ayırın. İlk olarak biz düğüm sunucuyu ayarlar index.js dosyasına gelir. Ayarlar ve yollarınızı tanımlar olduğuna dikkat edin. Ardından, routes.js dosyanızı hero hizmetine hakkında konuşuyor ve gibi işlevlerinizin almak için söyler **getHeroes**, istek ve yanıt geçirin. Hero.service.js dosya modeli alır ve Mongo için bağlanır. Yürütür, ardından **getHeroes** zaman adlandırılır ve döndürür yeniden 200 yanıtını döndürür. 

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Ardından, aşağıdaki adımları kullanarak uygulamayı çalıştırın:

1. Visual Studio Code'da tüm değişikliklerinizi kaydedin. Sol tarafta, seçin **hata ayıklama** düğmesi ![Visual Studio code'da Hata Ayıkla simgesi](./media/tutorial-develop-mongodb-nodejs-part5/debug-button.png)ve ardından **hata ayıklamayı Başlat** düğmesi ![Visual Studio code'da Hata Ayıkla simgesi ](./media/tutorial-develop-mongodb-nodejs-part5/start-debugging-button.png).

1. Şimdi tarayıcıya geçin. Açık **Geliştirici Araçları** ve **Ağ sekmesi**. Git `http://localhost:3000`, ve uygulamamızı orada gördüğünüz.

    ![Azure Portal’daki yeni Azure Cosmos DB hesabı](./media/tutorial-develop-mongodb-nodejs-part5/azure-cosmos-db-heroes-app.png)

Uygulamada henüz depolanan hero yok vardır. Bu öğreticinin sonraki bölümünde ekleyeceğiz put, push ve silme işlevi. Ardından biz ekleyebilir, güncelleştirme ve Azure Cosmos DB veritabanımızdaki Mongoose bağlantıları kullanarak kullanıcı Arabiriminden hero'ları silin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynaklara artık ihtiyacınız olmadığında, kaynak grubu, Azure Cosmos DB hesabı ve tüm ilgili kaynakları silin. Kaynak grubunu silmek için aşağıdaki adımları kullanın:

 1. Azure Cosmos DB hesabı oluşturduğunuz kaynak grubuna gidin.
 1. **Kaynak grubunu sil**'i seçin.
 1. Silin ve kaynak grubu adını onaylayın **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Post, eklemek için öğreticinin Bölüm 6'için devam Put ve Delete işlevleri uygulamaya:

> [!div class="nextstepaction"]
> [6. Bölüm: POST, Put ekleyin ve Delete işlevleri uygulamaya](tutorial-develop-mongodb-nodejs-part6.md)
