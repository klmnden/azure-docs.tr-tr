---
title: Azure Cosmos DB ile Mongoose çerçevesini kullanma | Microsoft Docs
description: Node.js Mongoose uygulamasını Azure Cosmos DB’ye bağlama hakkında bilgi edinin
services: cosmos-db
author: slyons
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: sclyon
ms.openlocfilehash: 8cfa53a1792d8e01c05aad8e4a1a0b5239a092c1
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857420"
---
# <a name="azure-cosmos-db-using-the-mongoose-framework-with-azure-cosmos-db"></a>Azure Cosmos DB: Azure Cosmos DB ile Mongoose çerçevesini kullanma

Bu öğreticide, veriler Azure Cosmos DB'de depolanırken [Mongoose Çerçevesi](http://mongoosejs.com/)'nin nasıl kullanılacağı gösterilir. Bu kılavuzda Azure Cosmos DB için MongoDB API'sini kullanıyoruz. Henüz tanımayanlar için, Mongoose MongoDB için Node.js'de bir nesne modelleme çerçevesidir ve uygulama verilerinizi modellemeniz için rahat, şema tabanlı bir çözüm sağlar.

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) v0.10.29 sürümü veya sonraki bir sürüm.

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Node.js uygulamanızı ayarlama](#SetupNode) adımına atlayın.

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Node.js uygulamanızı ayarlama

>[!Note]
> Uygulamanın kendisini ayarlamak yerine yalnızca örnek kodun üzerinden geçmek istiyorsanız, bu öğreticide kullanılan [örneği](https://github.com/Azure-Samples/Mongoose_CosmosDB) kopyalayın ve Azure Cosmos DB'de Node.js Mongoose uygulamanızı oluşturun.

1. İstediğiniz klasörde bir Node.js uygulaması oluşturmak için, düğümün komut isteminde aşağıdaki komutu çalıştırın.

    ```npm init```

    Soruları yanıtladığınızda projeniz kullanıma hazır olacaktır.

1. Klasöre yeni bir dosya ekleyin ve bu dosyayı ```index.js``` olarak adlandırın.
1. ```npm install``` seçeneklerinden birini kullanarak gerekli paketleri yükleyin:
    * Mongoose: ```npm install mongoose@5 --save```

    > [!Note]
    > Aşağıdaki Mongoose örnek bağlantı önceki sürümlerden bu yana değişti Mongoose 5 + temel alır.
    
    * Dotenv (gizli dizilerinizi bir .env dosyasında yüklemek isterseniz): ```npm install dotenv --save```

    >[!Note]
    > ```--save``` bayrağı package.json dosyasına bağımlılık ekler.

1. Bağımlılıkları index.js dosyanıza aktarın.
    ```JavaScript
    var mongoose = require('mongoose');
    var env = require('dotenv').load();    //Use the .env file to load the variables
    ```

1. Cosmos DB bağlantı dizenizi ve Cosmos DB Adını ```.env``` dosyasına ekleyin.

    ```JavaScript
    COSMOSDB_CONNSTR=mongodb://{cosmos-user}.documents.azure.com:10255/{dbname}
    COSMODDB_USER=cosmos-user
    COSMOSDB_PASSWORD=cosmos-secret
    ```

1. Aşağıdaki kodu index.js dosyasının sonuna ekleyin ve Mongoose çerçevesini kullanarak Azure Cosmos DB'ye bağlanın.
    ```JavaScript
    mongoose.connect(process.env.COSMOSDB_CONNSTR+"?ssl=true&replicaSet=globaldb", {
      auth: {
        user: process.env.COSMODDB_USER,
        password: process.env.COSMOSDB_PASSWORD
      }
    })
    .then(() => console.log('Connection to CosmosDB successful'))
    .catch((err) => console.error(err));
    ```
    >[!Note]
    > Burada, 'dotenv' npm paketiyle process.env.{variableName} kullanılarak ortam değişkenleri yüklenir.

    Azure Cosmos DB'ye bağlandıktan sonra, artık Mongoose'da nesne modellerini ayarlamaya başlayabilirsiniz.

## <a name="caveats-to-using-mongoose-with-azure-cosmos-db"></a>Azure Cosmos DB ile Mongoose kullanma konusunda uyarılar

Oluşturduğunuz her model için, Mongoose aslında arka planda yeni bir MongoDB koleksiyonu oluşturur. Bununla birlikte, Azure Cosmos DB'nin koleksiyon başına faturalama modeli alındığında, pek dolu olmayan çok sayıda nesne modeliniz varsa maliyet açısından izlenecek en verimli yol bu olmayabilir.

Bu yönergeler her iki modeli de kapsar. Önce koleksiyon başına tek veri türünü depolama yönergelerini vereceğiz. Mongoose'un genel davranışı budur.

Mongoose'da ayrıca [Ayırıcı](http://mongoosejs.com/docs/discriminators.html) (Discriminator) adlı bir kavram vardır. Ayırıcılar bir şema devralma mekanizmasıdır. Bunlar aynı temel MongoDB koleksiyonu üzerinde örtüşen şemalar içeren birden çok modelinizin olmasına olanak tanır.

Çeşitli veri modellerini aynı koleksiyonda depolayabilir ve ardından sorgu zamanında bir filtre yan tümcesi kullanarak yalnızca gerekli verileri aşağı çekebilirsiniz.

### <a name="one-collection-per-object-model"></a>Nesne modeli başına bir koleksiyon

Varsayılan Mongoose davranışı, her Nesne modeli oluşturduğunuzda bir MongoDB koleksiyonu oluşturmaktır. Bu bölümde Azure Cosmos DB ile MongoDB'de bunun nasıl başarılacağı incelenir. Çok fazla miktarda veri içeren nesne modelleriniz olduğunda Azure Cosmos DB ile bu yöntemin kullanılması önerilir. Bu, Mongoose için varsayılan çalışma modelidir; dolayısıyla Mongoose'u tanıyorsanız bu yöntemi de biliyor olabilirsiniz.

1. ```index.js``` dosyanızı yeniden açın.

1. 'Family' (Aile) için şema tanımını oluşturun.

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

1. 'Family' için bir nesne oluşturun.

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

1. Son olarak, nesneyi Azure Cosmos DB'ye kaydedin. Bu işlem, perde arkasında bir koleksiyon oluşturur.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. Şimdi de, başka bir şema ve nesne oluşturalım. Bu kez, oluşturduğumuz nesne ailelerin ilgisini çekebilecek 'Vacation Destinations' (Tatil Yerleri) için olsun.
    1. Aynı geçen seferki gibi, şemayı oluşturalım
    ```JavaScript
    const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
        name: String,
        country: String
    }));
    ```

    1. Örnek bir nesne oluşturun (bu şemaya birden çok nesne ekleyebilirsiniz) ve kaydedin.
    ```JavaScript
    const vacaySpot = new VacationDestinations({
        name: "Honolulu",
        country: "USA"
    });

    vacaySpot.save((err, saveVacay) => {
        console.log(JSON.stringify(saveVacay));
    });
    ```

1. Şimdi, Azure Portal'a gittiğinizde Azure Cosmos DB'de iki koleksiyon oluşturulduğunu fark edeceksiniz.

    ![Node.js öğreticisi - Birden çok koleksiyon adının vurgulandığı Azure Cosmos DB hesabını gösteren Azure Portal ekran görüntüsü - Node veritabanı][mutiple-coll]

1. Son olarak, Azure Cosmos DB'den verileri okuyalım. Varsayılan Mongoose çalışma modelini kullandığımızdan, okuma işlemleri Mongoose'la yapılan diğer okuma işlemleriyle aynıdır.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Mongoose ayırıcılarını kullanarak verileri tek koleksiyonda depolama

Bu yöntemde, her Azure Cosmos DB koleksiyonunun maliyetini iyileştirmemize yardımcı olması için [Mongoose Ayırıcıları](http://mongoosejs.com/docs/discriminators.html) kullanırız. Ayırıcılar, farklı nesne modellerinde depolama, ayırt etme ve filtreleme işlemleri yapmanıza olanak tanıyan ayırt edici bir 'Key' (Anahtar) tanımlamanızı sağlar.

Burada temel bir nesne modeli oluşturuyor, ayırt edici anahtar tanımlıyor ve temel modele 'Family' ile 'VacationDestinations' nesneleri uzantı olarak ekliyoruz.

1. Şimdi temel yapılandırmayı ayarlayalım ve ayırıcı anahtarı tanımlayalım.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Ardından da ortak nesne modelini tanımlayalım

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. Şimdi 'Family' modelini tanımlıyoruz. Burada ```mongoose.model``` yerine ```commonModel.discriminator``` kullandığımıza dikkat edin. Ayrıca, mongoose şemasına temel yapılandırmayı da ekliyoruz. Dolayısıyla, burada discriminatorKey değeri ```FamilyType``` oluyor.

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

1. Benzer biçimde, şimdi bir şema daha, bu kez 'VacationDestinations' ekleyelim. Burada, DiscriminatorKey değeri ```VacationDestinationsType``` oluyor.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Son olarak da model için nesneleri oluşturalım ve kaydedelim.
    1. 'Family' modeline nesneleri ekleyelim.
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

    1. Ardından, 'VacationDestinations' modeline nesneleri ekleyelim ve kaydedelim.
    ```JavaScript
    const vacay_common = new Vacation_common({
        name: "Honolulu",
        country: "USA"
    });

    vacay_common.save((err, saveVacay) => {
        console.log("Saved: " + JSON.stringify(saveVacay));
    });
    ```

1. Artık Azure Portal'a dönerseniz, hem 'Family' hem de 'VacationDestinations' verilerini içeren ```alldata``` adlı tek bir koleksiyonunuz olduğunu görürsünüz.

    ![Node.js öğreticisi - Koleksiyon adının vurgulandığı Azure Cosmos DB hesabını gösteren Azure Portal ekran görüntüsü - Node veritabanı][alldata]

1. Ayrıca, iki farklı nesne modeli arasında ayırt etmenize yardımcı olmak üzere her nesnenin ```__type``` olarak adlandırılan başka bir özniteliği vardır.

1. Son olarak, Azure Cosmos DB'de depolanan verileri okuyalım. Mongoose model temelinde verileri filtreleme işlemini üstlenir. Dolayısıyla, verileri okurken farklı bir şey yapmanız gerekmez. Modelinizi belirtmeniz yeterlidir (bu örnekte ```Family_common```); Mongoose 'DiscriminatorKey' ile filtrelemeyi yapar.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Sizin de görebileceğiniz gibi, Mongoose ayırıcılarıyla çalışmak kolaydır. Bu nedenle, Mongoose çerçevesini kullanan bir uygulamanız varsa, bu öğretici çok fazla değişiklik yapmadan Azure Cosmos DB üzerinde MongoDB API'sinde uygulamanızı hızla çalıştırmaya başlamanız için bir yol sunar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB MongoDB API'si tarafından desteklenen MongoDB işlemleri, işleçleri, aşamaları, komutları ve seçenekleri hakkında daha fazla bilgi edinmek için [MongoDB özellikleri ve söz dizimi için MongoDB API'si desteği](mongodb-feature-support.md) konusuna bakın.

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[mutiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png
