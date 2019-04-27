---
title: MongoDB kullanarak Azure Cosmos DB'nin genel dağıtım Öğreticisi
description: MongoDB kullanarak Azure Cosmos DB'nin genel dağıtımını ayarlama konusunda bilgi edinin.
author: rimman
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 5ae5923253575fc3dea6b90b599b9fa3d79a85b8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60578814"
---
# <a name="set-up-global-distributed-database-using-azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB'nin MongoDB kullanarak küresel dağıtılmış veritabanı ayarlama

Bu makalede, global olarak dağıtılmış bir veritabanı kurulumu ve bunu Azure Cosmos DB'nin MongoDB kullanarak bağlanmak için Azure portalını kullanmayı göstereceğiz.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * Kullanarak genel dağıtımı yapılandırma [Azure Cosmos DB'nin MongoDB API'si](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup"></a>Bölgesel kurulumunuzu doğrulama 
MongoDB çalıştırmak için genel yapılandırmayı Cosmos DB API'si ile denetlemek için basit bir yol *isMaster()* Mongo kabuğundan komutu.

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

Azure Cosmos DB MongoDB API'si, Global olarak dağıtılmış bir veritabanı için koleksiyonunuzun okuma tercihini belirtmenize olanak sağlar. Hem düşük gecikmeli okumalar hem de genel yüksek kullanılabilirlik için, koleksiyonunuzun okuma tercihini *en yakın* olarak ayarlamanızı öneririz. En yakın bölgeden okumak için *en yakın* okuma tercihi yapılandırılır.

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
> * Cosmos DB'nin MongoDB kullanarak genel dağıtımı yapılandırma

Artık Azure Cosmos DB yerel öykünücüsünü kullanarak yerel olarak geliştirme konusunda bilgi almak için sonraki öğreticiye geçebilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB öykünücüsü ile yerel olarak geliştirme](local-emulator.md)
