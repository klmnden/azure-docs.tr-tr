---
title: Azure Cosmos DB MongoDB API'si ile MongoDB okuma tercihini kullanma | Microsoft Docs
description: Azure Cosmos DB MongoDB API'si ile MongoDB okuma tercihini kullanmayı öğrenin
services: cosmos-db
author: vidhoonv
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: ''
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: sclyon
ms.openlocfilehash: 90c8d73e32f4c99c6871ce9cdb7839cd1d380b9b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42061734"
---
# <a name="how-to-globally-distribute-reads-using-read-preference-with-the-azure-cosmos-db-mongodb-api"></a>Genel olarak dağıtma ile Azure Cosmos DB MongoDB API'si ile okuma tercihini kullanma okur. 

Bu makalede, okuma işlemleri kullanarak Global olarak dağıtma işlemi gösterilmektedir [MongoDB okuma tercihini](https://docs.mongodb.com/manual/core/read-preference/) Azure Cosmos DB'nin MongoDB API'si ile ayarlar. 

## <a name="prerequisites"></a>Önkoşullar 
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Bu [hızlı](tutorial-global-distribution-mongodb.md) makale Azure portalını kullanarak Azure Cosmos DB hesabı ile genel dağıtım ayarlayın ve sonra MongoDB API'sini kullanarak bağlanma yönergeleri.

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
Uygulama bir MongoDB kaynağına bağlanmayı dener ve bağlantı dizesi geçersiz olduğu için başarısız olur. Bağlantı dizesini güncellemek için Benioku adımları `url`. Ayrıca, güncelleştirme `readFromRegion` Azure Cosmos DB hesabınızdaki bir okuma bölgesine. Aşağıdaki yönergeler, NodeJS örnekten şunlardır:

```
* Next, substitute the `url`, `readFromRegion` in App.Config with your Cosmos DB account's values. 
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

MongoDB, istemcilerin kullanmak aşağıdaki okuma tercihini modları sağlar:

1. BİRİNCİL
2. PRIMARY_PREFERRED
3. İKİNCİL
4. SECONDARY_PREFERRED
5. EN YAKIN

Ayrıntılı başvuru [MongoDB okuma tercihini davranışı](https://docs.mongodb.com/manual/core/read-preference-mechanics/#replica-set-read-preference-behavior) her birinin davranışı hakkında ayrıntılar için belgeleri okuyun tercih modları. Azure Cosmos DB'de birincil yazma bölgesi ve ikincil okuma bölgesi eşlenir eşlenir.

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

Diğer platformlar için karşılık gelen örnek uygulama depoları gibi başvurmak [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) ve [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

## <a name="read-using-tags"></a>Etiketleri kullanarak okuma

Okuma tercihini modu ek olarak, MongoDB, okuma işlemleri yönlendirmek için etiketleri kullanmanıza izin verir. MongoDB API'si için Azure Cosmos DB'de `region` etiketi varsayılan olarak bir parçası olarak dahil `isMaster` yanıt:

```json
"tags": {
         "region": "West US"
      }
```

Bu nedenle, MongoClient kullanabilirsiniz `region` etiketi okuma işlemlerini belirli bölgelere yönlendirmek için bölge adı. Azure Cosmos DB hesapları için bölge adlarının Azure Portal'da sol altında bulunabilir **ayarlar çoğaltma verilerini genel ->**. Bu ayar, elde etmek için yararlıdır **yalıtım okuma** -yalnızca belirli bir bölgeye okuma işlemleri doğrudan istediğiniz durumlarda hangi istemci uygulaması. Bu ayar, olmayan-üretim/analiz için idealdir, arka planda çalışan ve üretim kritik Hizmetleri senaryoları yazın.

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

Bu makalede, Azure Cosmos DB'nin MongoDB API'si ile okuma tercihini kullanma okuma işlemleri genel olarak dağıtmayı öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları uygulayarak Azure portalında bu makalede tarafından oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
* [Genel olarak çoğaltılan bir Azure Cosmos DB hesabı kurulumu ve MongoDB API'si ile kullanma](tutorial-global-distribution-mongodb.md)
* [Öykünücü ile yerel olarak geliştirme](local-emulator.md)
