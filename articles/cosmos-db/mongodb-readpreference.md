---
title: MongoDB API'si ile Azure Cosmos DB'nin MongoDB okuma tercihini kullanarak
description: Okuma tercihini MongoDB API'si ile Azure Cosmos DB'nin MongoDB için kullanmayı öğrenin
author: sivethe
ms.author: sivethe
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 8fc66d70b840578bff086519a7b39e5f389a3de3
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479622"
---
# <a name="how-to-globally-distribute-reads-using-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB kullanarak genel olarak nasıl dağıtacağınızı okur

Bu makalede, okuma işlemleri ile Global olarak dağıtma işlemi gösterilmektedir [MongoDB okuma tercihini](https://docs.mongodb.com/manual/core/read-preference/) Azure Cosmos DB'nin MongoDB kullanarak ayarlar.

## <a name="prerequisites"></a>Önkoşullar 
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Bu [hızlı](tutorial-global-distribution-mongodb.md) ayarlamak için Azure portalını kullanarak yönergeleri yukarı genel dağıtım Cosmos hesabıyla ve buna bağlanmak için makalesi.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

Örnek depoyu kopyalamak için aşağıdaki komutları çalıştırın. İlgilendiğiniz platformunuza bağlı olarak, aşağıdaki örnek depoları birini kullanın:

1. [.NET örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference)
2. [NodeJS örnek uygulaması]( https://github.com/Azure-Samples/azure-cosmos-db-mongodb-node-geo-readpreference)
3. [Mongoose örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-mongoose-geo-readpreference)
4. [Java örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)
5. [SpringBoot örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-spring)


```bash
git clone <sample repo url>
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kullanılan platforma bağlı olarak gerekli paketleri yükleyin ve uygulamayı başlatın. Bağımlılıkları yüklemek için örnek uygulama depoya Benioku DOSYASINA izleyin. Örneğin, NodeJS örnek uygulamada, gerekli paketleri yükleyin ve uygulamayı başlatmak için aşağıdaki komutları kullanın.

```bash
cd mean
npm install
node index.js
```
Uygulama bir MongoDB kaynağına bağlanmayı dener ve bağlantı dizesi geçersiz olduğu için başarısız olur. Bağlantı dizesini güncellemek için Benioku adımları `url`. Ayrıca, güncelleştirme `readFromRegion` Cosmos hesabınızdaki bir okuma bölgesine. Aşağıdaki yönergeler, NodeJS örnekten şunlardır:

```
* Next, substitute the `url`, `readFromRegion` in App.Config with your Cosmos account's values. 
```

Sonra aşağıdaki adımları, örnek uygulama çalışır ve aşağıdaki çıktıyı üretir:

```
connected!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromSecondaryfunc query completed!
```

## <a name="read-using-read-preference-mode"></a>Okuma tercihini modu kullanılarak okuyun

MongoDB protokolü, istemcilerin kullanmak aşağıdaki okuma tercihini modları sağlar:

1. PRIMARY
2. PRIMARY_PREFERRED
3. İKİNCİL
4. SECONDARY_PREFERRED
5. EN YAKIN

Ayrıntılı başvuru [MongoDB okuma tercihini davranışı](https://docs.mongodb.com/manual/core/read-preference-mechanics/#replica-set-read-preference-behavior) her birinin davranışı hakkında ayrıntılar için belgeleri okuyun tercih modları. Cosmos DB'de birincil yazma bölgesi ve ikincil okuma bölgesi eşlenir eşlenir.

Genel senaryolara bağlı olarak, aşağıdaki ayarları kullanmanızı öneririz:

1. Varsa **düşük gecikme süreli okuma** olan gerekli kullanın **NEAREST** tercih modu okuyun. Okuma işlemleri için bu ayarı yönlendirir en yakın bölgede kullanılabilir. En yakın bölgeyi yazma bölgesi ise, ardından bu işlemleri ilgili bölgeye yönlendirilir olduğunu unutmayın.
2. Varsa **okumalar yüksek kullanılabilirlik ve coğrafi dağıtım** gereklidir (gecikme değil bir kısıtlama), ardından **ikincil tercih edilen** tercih modu okuyun. Bu ayar, okuma işlemleri kullanılabilir bir okuma bölgesine yönlendirir. Ardından, hiçbir okuma bölgesi varsa, istekleri yazma bölgesinden yönlendirilir.

Örnek uygulamayı alınan aşağıdaki kod, en yakın okuma tercihini Nodejs'de yapılandırma işlemi gösterilmektedir:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.NEAREST});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromNearestfunc query completed!");
  });
```

Benzer şekilde, aşağıdaki kod parçacığında SECONDARY_PREFERRED okuma tercihini Nodejs'de yapılandırma işlemi gösterilmektedir:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.SECONDARY_PREFERRED});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromSecondaryPreferredfunc query completed!");
  });
```

Okuma tercihini geçirerek de ayarlanabilir `readPreference` bağlantı dizesi URI seçeneklerinin bir parametre olarak:

```javascript
const MongoClient = require('mongodb').MongoClient;
const assert = require('assert');

// Connection URL
const url = 'mongodb://localhost:27017?ssl=true&replicaSet=globaldb&readPreference=nearest';

// Database Name
const dbName = 'myproject';

// Use connect method to connect to the Server
MongoClient.connect(url, function(err, client) {
  console.log("Connected correctly to server");

  const db = client.db(dbName);

  client.close();
});
```

Diğer platformlar için karşılık gelen örnek uygulama depoları gibi başvurmak [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) ve [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

## <a name="read-using-tags"></a>Etiketleri kullanarak okuma

Okuma tercihini modu ek olarak, MongoDB protokolü, okuma işlemleri yönlendirmek için etiketleri kullanımına izin verir. MongoDB, Cosmos DB'nin API'sindeki `region` etiketi varsayılan olarak bir parçası olarak dahil `isMaster` yanıt:

```json
"tags": {
         "region": "West US"
      }
```

Bu nedenle, MongoClient kullanabilirsiniz `region` etiketi okuma işlemlerini belirli bölgelere yönlendirmek için bölge adı. Cosmos hesapları için bölge adlarının Azure Portal'da sol altında bulunabilir **ayarlar çoğaltma verilerini genel ->** . Bu ayar, elde etmek için yararlıdır **yalıtım okuma** -yalnızca belirli bir bölgeye okuma işlemleri doğrudan istediğiniz durumlarda hangi istemci uygulaması. Bu ayar, olmayan-üretim/analiz için idealdir, arka planda çalışan ve üretim kritik Hizmetleri senaryoları yazın.

Örnek uygulamayı alınan aşağıdaki kod, nodejs'de etiketlerle okuma tercihini yapılandırma işlemi gösterilmektedir:

```javascript
 var query = {};
  var readcoll = client.db('regionDB').collection('regionTest',{readPreference: new ReadPreference(ReadPreference.SECONDARY_PREFERRED, {"region": "West US"})});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromRegionfunc query completed!");
  });
```

Diğer platformlar için karşılık gelen örnek uygulama depoları gibi başvurmak [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) ve [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

Bu makalede, genel okuma işlemleri için MongoDB API'si ile Azure Cosmos DB'nin okuma tercihini kullanma dağıtmayı öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları uygulayarak Azure portalında bu makalede tarafından oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
* [MongoDB için Azure Cosmos DB API'si ile Global olarak dağıtılmış bir veritabanı Kurulumu](tutorial-global-distribution-mongodb.md)
* [Azure Cosmos DB öykünücüsü ile yerel olarak geliştirme](local-emulator.md)
