---
title: MongoDB API’si için Azure Cosmos DB genel dağıtım öğreticisi | Microsoft Docs
description: MongoDB API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlamayı öğrenin.
services: cosmos-db
keywords: genel dağıtım, MongoDB
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 1885c979fe2532d26b2e7b59111675bebee8ec05
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38668097"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a>MongoDB API’sini kullanarak Azure Cosmos DB genel dağıtımını ayarlama

Bu makalede, Azure portalını kullanarak Azure Cosmos DB genel dağıtımını ayarlama ve sonra MongoDB API’sini kullanarak bağlanma işlemlerinin nasıl yapılacağı gösterilmektedir.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtımı yapılandırma
> * [MongoDB API](mongodb-introduction.md)’sini kullanarak genel dağıtımı yapılandırma

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>MongoDB API’sini kullanarak bölgesel kurulumunuzu doğrulama
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

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>MongoDB API’sini kullanarak tercih ettiğiniz bir bölgeye bağlanma

MongoDB API’si, genel olarak dağıtılan bir veritabanı için koleksiyonunuzun okuma tercihini belirtmenize olanak sağlar. Hem düşük gecikmeli okumalar hem de genel yüksek kullanılabilirlik için, koleksiyonunuzun okuma tercihini *en yakın* olarak ayarlamanızı öneririz. En yakın bölgeden okumak için *en yakın* okuma tercihi yapılandırılır.

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
