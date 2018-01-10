---
title: Mongoose framework Azure Cosmos DB ile kullanma | Microsoft Docs
description: "Bir Node.js Mongoose uygulaması Azure Cosmos Veritabanına bağlanın öğrenin"
services: cosmos-db
documentationcenter: 
author: romitgirdhar
manager: jhubbard
editor: 
ms.assetid: de5eea58-ee7c-4609-b1c9-4af3e61a5883
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: rogirdh
ms.openlocfilehash: 9878936b5dd76730633dec16b1c3a3eaac78e95a
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="azure-cosmos-db-using-the-mongoose-framework-with-azure-cosmos-db"></a>Azure Cosmos DB: Mongoose framework Azure Cosmos DB ile kullanma

Bu öğretici nasıl kullanılacağını gösterir [Mongoose Framework](http://mongoosejs.com/) verileri Azure Cosmos DB'de depolarken. MongoDB API için Azure Cosmos DB bu kılavuz için kullanırız. Bu da tanınmayan için Mongoose Node.js içinde MongoDB için bir nesne modelleme çerçevesi ve uygulama verilerinizi modellemek için düz İleri, şema tabanlı bir çözüm sağlar.

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) v0.10.29 sürümü veya sonraki bir sürüm.

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa, atlayabilirsiniz [Node.js uygulamanızı ayarlama](#SetupNode). Azure Cosmos DB öykünücüsü kullanıyorsanız, verilen adımları izleyin [Azure Cosmos DB öykünücüsü](local-emulator.md) için İleri atlayabilirsiniz ve öykünücüsü ayarlamak için [Node.js uygulamanızı ayarlama](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Node.js uygulamanızı ayarlama

>[!Note]
> Kurulum uygulamanın kendisinin yerine örnek kod yalnızca izlenecek kullanmak isterseniz, kopyalama [örnek](https://github.com/Azure-Samples/Mongoose_CosmosDB) Node.js Mongoose uygulamanızı Azure Cosmos DB üzerinde ve Bu öğretici için kullanılır.

1. Tercih ettiğiniz klasöründe bir Node.js uygulaması oluşturmak için bir düğüm komut isteminde aşağıdaki komutu çalıştırın.

    ```npm init```

    Soruları yanıtlayın ve projenizin geçmeye hazır olacaktır.

1. Yeni bir dosya klasörüne ekleyin ve adını ```index.js```.
1. Aşağıdakilerden birini kullanarak gerekli paketleri yüklemek ```npm install``` seçenekleri:
    * Mongoose:```npm install mongoose --save```
    * (Bir .env dosyasından sırlarınızı yüklemek istiyorsanız) Dotenv:```npm install dotenv --save```
    
    >[!Note]
    > ```--save``` Bayrağı bağımlılık package.json dosyasına ekler.

1. İndex.js dosyanızdaki bağımlılıkları içeri aktarın.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Cosmos DB bağlantı dizesi ve Cosmos DB adına ekleme ```.env``` dosya.

    ```JavaScript
    COSMOSDB_CONNSTR={Your MongoDB Connection String Here}
    COSMOSDB_DBNAME={Your DB Name Here}
    ```

1. Azure Cosmos DB index.js sonuna aşağıdaki kodu ekleyerek Mongoose framework kullanarak bağlanın.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+process.env.COSMOSDB_DBNAME+"?ssl=true&replicaSet=globaldb"); //Creates a new DB, if it doesn't already exist

    var db = mongoose.connection;
    db.on('error', console.error.bind(console, 'connection error:'));
    db.once('open', function() {
    console.log("Connected to DB");
    });
    ```
    >[!Note]
    > Burada, ortam değişkenleri process.env kullanılarak yüklenir. {variableName} 'dotenv' npm paket kullanma.

    Azure Cosmos Veritabanına bağlandıktan sonra şimdi Mongoose nesne modelleri ayarlama başlatabilirsiniz.
    
## <a name="caveats-to-using-mongoose-with-azure-cosmos-db"></a>Azure Cosmos DB ile Mongoose kullanarak uyarılar

Oluşturduğunuz her model için Mongoose kapsar altında yeni bir MongoDB koleksiyonu oluşturur. Seyrek doldurulur birden çok nesne modelleri ekranınız varsa ancak Azure Cosmos DB koleksiyon başına fatura modelini verildiğinde, onu gitmek için en ekonomik şekilde olmayabilir.

Bu kılavuz, her iki modeli kapsar. Biz öncelikle veri koleksiyonunun başına bir tür depolama üzerinde Kılavuzu ele alacağız. Mongoose defacto davranış budur.

Mongoose de adlı bir kavram vardır [Discriminators](http://mongoosejs.com/docs/discriminators.html). Discriminators bir şema devralma sistemdir. Çakışan şemalarda aynı temel MongoDB koleksiyon en üstünde birden fazla modeli olmasını sağlar.

Çeşitli veri modelleri aynı koleksiyonunda depolayın ve sonra gereken verileri çekmek için sorgu zamanında bir filtre yan tümcesi kullanın.

### <a name="one-collection-per-object-model"></a>Bir koleksiyon başına nesne modeli

Varsayılan Mongoose nesne modeli oluşturduğunuz her zaman bir MongoDB koleksiyonu oluşturmak için bir davranıştır. Bu bölümde Azure Cosmos DB için MongoDB ile bunun nasıl araştırır. Nesne modelleri büyük miktarda veri varsa, bu yöntem ile Azure Cosmos DB önerilir. Varsayılan değer budur Mongoose ile aşinaysanız modeli için Mongoose işletim, bu nedenle, bu, bilgi sahibi olmanız.

1. Açık, ```index.js``` yeniden.

1. 'Family' için şema tanımı oluşturun.

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Bir nesne 'Ailesini' oluşturun.

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Son olarak, şirketinizdeki nesne Azure Cosmos DB kaydedin. Bu, perde altındaki bir koleksiyon oluşturur.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Şimdi, başka bir şemasına ve nesne oluşturalım. Bu kez, aileleri de ilginizi çekiyor 'tatil hedefleri' için bir tane oluşturalım.
    1. Yalnızca son zamanı gibi düzeni oluşturalım
    ```JavaScript
    const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
        name: String,
        country: String
    }));
    ```

    1. (Ekleyebilirsiniz birden fazla nesne bu şemaya) örnek nesnesi oluşturun ve kaydedin.
    ```JavaScript
    const vacaySpot = new VacationDestinations({
        name: "Honolulu",
        country: "USA"
    });

    vacaySpot.save((err, saveVacay) => {
        console.log(JSON.stringify(saveVacay));
    });
    ```

1. Şimdi, Azure Portalı'na giderek, size Azure Cosmos DB içinde oluşturulan iki koleksiyonlar dikkat edin.

    ![Node.js Öğreticisi - koleksiyon adı Node veritabanı vurgulanmış - bir Azure Cosmos DB hesabını gösteren Azure portal ekran görüntüsü][alldata]

1. Son olarak, şirketinizdeki verileri Azure Cosmos DB'den okuyun. Varsayılan Mongoose işletim modeli de kullandığımızdan, okumaları başka bir okuma Mongoose ile aynıdır.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Tek bir koleksiyonda bulunan verileri depolamak için Mongoose discriminators kullanma

Bu yöntemde kullanırız [Mongoose Discriminators](http://mongoosejs.com/docs/discriminators.html) her Azure Cosmos DB koleksiyonu maliyetleri için en iyi duruma getirmek için. Discriminators depolamak, ayırmak ve farklı nesne modellerinde filtre olanak tanıyan, bir farklılaştırıcı 'anahtar', tanımlamanızı sağlar.

Burada, temel nesne oluşturuyoruz model, farklılaştırıcı bir anahtar tanımlayın ve 'Family' ve 'VacationDestinations' temel modele bir uzantısı olarak ekleyin.

1. Şimdi temel yapılandırma kümesi ve Ayrıştırıcıyı anahtar tanımlayın.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Ardından, şimdi ortak nesne modeli tanımlayın

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Biz şimdi 'Family' model tanımlayın. Burada size olduğunu fark ```commonModel.discriminator``` yerine ```mongoose.model```. Ayrıca, biz de temel yapılandırma mongoose şemaya ekliyoruz. Bu nedenle, discriminatorKey işte ```FamilyType```.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Bu süre 'VacationDestinations' benzer şekilde, başka bir şema ekleyelim. İşte, DiscriminatorKey ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Son olarak, şimdi modelin nesneleri oluşturun ve kaydedin.
    1. Nesneleri 'Family' modeline ekleyelim.
    ```JavaScript
    const family_common = new Family_common({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });

    family_common.save((err, saveFamily) => {
        console.log("Saved: " + JSON.stringify(saveFamily));
    });
    ```

    1. Ardından, şimdi nesneleri 'VacationDestinations' modele eklemek ve kaydedin.
    ```JavaScript
    const vacay_common = new Vacation_common({
        name: "Honolulu",
        country: "USA"
    });

    vacay_common.save((err, saveVacay) => {
        console.log("Saved: " + JSON.stringify(saveVacay));
    });
    ```

1. Azure portalına geri dönün, şimdi adında yalnızca bir koleksiyonu olması fark ```alldata``` 'Family' ve 'VacationDestinations' verilerle.

    ![Node.js Öğreticisi - koleksiyon adı Node veritabanı vurgulanmış - bir Azure Cosmos DB hesabını gösteren Azure portal ekran görüntüsü][mutiple-coll]

1. Ayrıca, her bir nesne olarak adlı başka bir öznitelik olduğunu fark ```__type```, iki farklı nesne modelleri arasında ayırt hangi Yardım.

1. Son olarak, Azure Cosmos DB içinde depolanan verileri şimdi okuyun. Mongoose veri modelini temel alan filtre mvc'deki. Bu nedenle, farklı bir şey veri okunurken yapmanız gerekir. Yalnızca modelinizi belirtin (Bu durumda, ```Family_common```) ve 'DiscriminatorKey' filtreleme Mongoose işler.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Gördüğünüz gibi Mongoose discriminators ile çalışmak kolaydır. Mongoose framework kullanan bir uygulamanız varsa, bu nedenle, Bu öğretici bir sizin uygulamanız alınacağı ve çok fazla değişiklik gerektirmeden MongoDB API Azure Cosmos DB üzerinde çalışan yoludur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

MongoDB işlemleri, işleçler, aşamaları, komutlar ve Azure Cosmos DB MongoDB API'si tarafından desteklenen seçenekler hakkında daha fazla bilgi [MongoDB özellikleri ve sözdizimi için MongoDB API desteği](mongodb-feature-support.md).

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[mutiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png