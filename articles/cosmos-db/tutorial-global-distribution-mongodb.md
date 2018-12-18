---
title: MongoDB için Azure Cosmos DB API hesabı, genel dağıtım Öğreticisi
description: MongoDB için Azure Cosmos DB API hesabı kullanarak Azure Cosmos DB genel dağıtımını ayarlama konusunda bilgi edinin.
services: cosmos-db
keywords: genel dağıtım, MongoDB
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 4703b8b5c866b4bcd70cad38e4fd3d0a65ee250f
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53541600"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API kullanarak Azure Cosmos DB genel dağıtımını ayarlama

Bu makalede, Azure portalında Azure Cosmos DB genel dağıtımını ayarlama ve sonra MongoDB için Azure Cosmos DB API kullanarak bağlanma nasıl kullanılacağını göstereceğiz.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * Kullanarak genel dağıtımı yapılandırma [Azure Cosmos DB MongoDB API'si](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup"></a>Bölgesel kurulumunuzu doğrulama 
MongoDB API’sindeki genel yapılandırmanızı iki kez denetlemenin en basit yolu, Mongo Kabuğundan *isMaster()* komutunu çalıştırmaktır.

Mongo Kabuğunuzdan:

   ```
      db.isMaster()
   ```
   
Örnek sonuçlar:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region"></a>Tercih edilen bir bölgeye bağlanma 

MongoDB için Azure Cosmos DB API, Global olarak dağıtılmış bir veritabanı için koleksiyonunuzun okuma tercihini belirtmenize olanak sağlar. Hem düşük gecikmeli okumalar hem de genel yüksek kullanılabilirlik için, koleksiyonunuzun okuma tercihini *en yakın* olarak ayarlamanızı öneririz. En yakın bölgeden okumak için *en yakın* okuma tercihi yapılandırılır.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Birincil okuma/yazma bölgesi ve olağanüstü durum kurtarma (DR) senaryoları için ikincil bölge içeren uygulamalar için, koleksiyonunuzun okuma tercihini *ikincil tercih edilen* olarak ayarlamanızı öneririz. Birincil bölgenin kullanılamadığı ikincil bölgeden okumak için *ikincil tercih edilen* okuma tercihi yapılandırılır.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Son olarak, okuma bölgelerinizi kendiniz belirtmek istiyorsanız. Okuma tercihiniz içinde bölge Etiketini ayarlayabilirsiniz.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Hepsi bu kadar. Böylece bu öğretici tamamlanmış olur. [Azure Cosmos DB’deki tutarlılık düzeyleri](consistency-levels.md) bölümünü okuyarak genel olarak çoğaltılan hesabınızın tutarlılığının nasıl yönetileceğini öğrenebilirsiniz. Ayrıca genel veritabanı çoğaltmasının Azure Cosmos DB’de nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Cosmos DB ile verileri genel olarak dağıtma](distribute-data-globally.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * SQL API’lerini kullanarak genel dağıtımı yapılandırma

Artık Azure Cosmos DB yerel öykünücüsünü kullanarak yerel olarak geliştirme konusunda bilgi almak için sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [Öykünücü ile yerel olarak geliştirme](local-emulator.md)
