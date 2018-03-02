---
title: Azure Cosmos DB MongoDB API MongoDB okuma tercih kullanarak | Microsoft Docs
description: "Azure Cosmos DB MongoDB API MongoDB okuma tercih kullanmayı öğrenin"
services: cosmos-db
documentationcenter: 
author: vidhoonv
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: 
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 02/26/2018
ms.author: viviswan
ms.openlocfilehash: 488206e81e483fa2006d774cd4e7d60f7017d9cb
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-globally-distribute-reads-using-read-preference-with-the-azure-cosmos-db-mongodb-api"></a>Genel olarak dağıtmak nasıl Azure Cosmos DB MongoDB API'si ile okuma tercih kullanarak okur 

Bu makalede, genel olarak okuma işlemleri kullanarak dağıtmak gösterilmiştir [MongoDB okuma tercih](https://docs.mongodb.com/manual/core/read-preference/) Azure Cosmos veritabanı MongoDB API ile ayarları. 

## <a name="prerequisites"></a>Önkoşullar 
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Bu başvuruda [Hızlı Başlangıç](tutorial-global-distribution-mongodb.md) makale genel dağıtım Azure Cosmos DB hesabıyla ayarlayın ve MongoDB API kullanarak bağlanmak için Azure portalını kullanma hakkında yönergeler için.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

Örnek depoyu kopyalamak için aşağıdaki komutları çalıştırın. Platformunuz ilgi bağlı olarak, aşağıdaki örnek depoları birini kullanın:

1. [.NET örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference)
2. [NodeJS örnek uygulama]( https://github.com/Azure-Samples/azure-cosmos-db-mongodb-node-geo-readpreference)
3. [Java örnek uygulaması](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)


```bash
git clone <sample repo url>
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kullanılan platforma bağlı olarak gereken paketleri yüklemek ve uygulamasını başlatın. Bağımlılıklar yüklemek için örnek uygulama havuzunda dahil Benioku izleyin. Örneğin, NodeJS örnek uygulamasında gerekli paketleri yüklemek ve uygulamayı başlatmak için aşağıdaki komutları kullanın.

```bash
cd mean
npm install
node index.js
```
Uygulama bir MongoDB kaynağına bağlanmaya çalışır ve bağlantı dizesi geçersiz olduğu için başarısız olur. Bağlantı dizesi güncelleştirmek için Benioku adımları `url`. Ayrıca, güncelleştirme `readFromRegion` Azure Cosmos DB hesabınızda okuma bir bölgeye. Aşağıdaki yönergeler NodeJS örnekten şunlardır:

```
* Next, substitute the `url`, `readFromRegion` in App.Config with your Cosmos DB account's values. 
```

Aşağıdaki adımları sonra örnek uygulamayı çalıştırır ve şu çıkışı üretir:

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

## <a name="read-using-read-preference-mode"></a>Okuma tercih modu kullanılarak okuma

MongoDB istemcilerin kullanmak aşağıdaki okuma tercih modlarını sağlar:

1. BİRİNCİL
2. PRIMARY_PREFERRED
3. İKİNCİL
4. SECONDARY_PREFERRED
5. YAKIN

Ayrıntılı başvuru [MongoDB okuma tercih davranışı](https://docs.mongodb.com/manual/core/read-preference-mechanics/#replica-set-read-preference-behavior) bunların davranışı hakkında ayrıntılı bilgi için belgeleri okuyun tercih modları. Azure Cosmos DB'de birincil yazma bölge ve okuma bölgeye ikincil eşlemeleri eşler.

Ortak senaryolarını temel alarak, aşağıdaki ayarları kullanmanız önerilir:

1. Varsa **düşük gecikme süresi okur** olan gerekli kullanmak **NEAREST** tercih modu okuyun. Okuma işlemleri için bu ayarı yönlendirir kullanılabilir bölge en yakın. En yakın bölgeyi yazma bölgesi ise, ardından bu işlemler bu bölgeye yönlendirilmesi unutmayın.
2. Varsa **okuma yüksek kullanılabilirlik ve coğrafi dağıtımını** gereklidir (gecikme değil bir kısıtlama), ardından **ikincil tercih edilen** okuma tercih modu. Bu ayar, okuma işlemleri için kullanılabilir bir okuma bölge yönlendirir. Okuma bölge kullanılabilir durumdaysa, istekleri yazma bölgesine yönlendirilir.

Aşağıdaki kod parçacığında örnek uygulamadan nasıl NodeJS içinde yakın okuma tercih yapılandırılacağını göstermektedir:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.NEAREST});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromNearestfunc query completed!");
  });
```

Benzer şekilde, aşağıdaki kod parçacığında nasıl NodeJS içinde SECONDARY_PREFERRED okuma tercih yapılandırılacağını göstermektedir:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.SECONDARY_PREFERRED});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromSecondaryPreferredfunc query completed!");
  });
```

Diğer platformlar için karşılık gelen örnek uygulama depoları gibi başvurmak [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) ve [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

## <a name="read-using-tags"></a>Etiketleri kullanarak okuyun

Okuma tercih modu ek olarak, MongoDB okuma işlemleri yönlendirmek için etiketler kullanılmasına izin verir. API MongoDB, Azure Cosmos veritabanı `region` etiketi varsayılan olarak bir parçası olarak dahil `isMaster` yanıtı:

```json
"tags": {
         "region": "West US"
      }
```

Bu nedenle, MongoClient kullanabilirsiniz `region` etiketi yanı sıra belirli bölgelerine okuma işlemleri yönlendirmek için bölge adı. Solda'nin altında Azure portalında Azure Cosmos DB hesapları için bölge adları bulunabilir **ayarlar çoğaltma verileri genel ->**. Bu ayar ulaşmak için yararlıdır **yalıtım okuma** -hangi istemci uygulaması durumlarda yalnızca belirli bir bölge için okuma işlemleri doğrudan istiyor. Bu ayarı olmayan-üretim/analizi için idealdir arka planda çalışır ve üretim kritik Hizmetleri senaryoları yazın.

Aşağıdaki kod parçacığında örnek uygulamadan nasıl nodejs'de etiketlerle okuma tercih yapılandırılacağını göstermektedir:

```javascript
 var query = {};
  var readcoll = client.db('regionDB').collection('regionTest',{readPreference: new ReadPreference(ReadPreference.SECONDARY_PREFERRED, {"region": "West US"})});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromRegionfunc query completed!");
  });
```

Diğer platformlar için karşılık gelen örnek uygulama depoları gibi başvurmak [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) ve [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

Bu makalede, genel olarak okuma işlemleri okuma tercih Azure Cosmos veritabanı MongoDB API'si ile kullanarak dağıtmak nasıl öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmek için değil kullanacaksanız, bu makalede aşağıdaki adımlarla Azure portal tarafından oluşturulan tüm kaynakları silin:

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB’ye MongoDB verileri aktarma](mongodb-migrate.md)
* [Genel olarak çoğaltılmış bir Azure Cosmos DB hesabı kurulumu ve MongoDB API'si ile kullanma](tutorial-global-distribution-mongodb.md)
* [Yerel olarak öykünücü ile geliştirme](local-emulator.md)
