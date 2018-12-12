---
title: Azure Cosmos DB’de kapsayıcının aktarım hızını sağlama
description: Azure Cosmos DB’de kapsayıcı düzeyinde aktarım hızını sağlamayı öğrenin
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: dd47976bca75569142f1912eee06c66061e92fa6
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097674"
---
# <a name="provision-throughput-for-an-azure-cosmos-db-container"></a>Azure Cosmos DB kapsayıcısına aktarım hızı sağlama

Bu makalede bir Azure Cosmos DB’deki kapsayıcıya (koleksiyon, grafik, tablo) nasıl aktarım hızı sağlandığı açıklanır. Tek bir kapsayıcıya aktarım hızı sağlayabilir veya aktarım hızını [veritabanına sağlayıp](how-to-provision-database-throughput.md) veritabanındaki kapsayıcılar arasında paylaştırabilirsiniz. Azure portalı, Azure CLI veya CosmosDB SDK’larını kullanarak bir kapsayıcıya aktarım hızı sağlayabilirsiniz.

## <a name="provision-throughput-using-azure-portal"></a>Azure portalını kullanarak aktarım hızı sağlama

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. [Yeni bir Cosmos DB hesabı oluşturun](create-sql-api-dotnet.md#create-a-database-account) veya mevcut hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni Koleksiyon**'u seçin. Ardından aşağıdaki ayrıntılarla formu doldurun:

   * Yeni bir veritabanı oluşturun veya var olanlardan birini kullanın.
   * Koleksiyon (veya tablo ya da grafik) kimliği girin.
   * Bir bölüm anahtarı değerini girin, örneğin `/userid`.
   * Bir aktarım hızı (örneğin 1000 RU) girin.
   * **Tamam**’ı seçin.

![SQL API'si kapsayıcı aktarım hızı sağlama](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli"></a>Azure CLI kullanarak aktarım hızı sağlama

```azurecli-interactive
# Create a container with a partition key and provision throughput of 1000 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

MongoDB API’si hesabı için aktarım hızı sağlıyorsanız bölüm anahtarı yolu olarak '/myShardKey' kullanın ve Cassandra API hesabı için aktarım hızı sağlıyorsanız bölüm anahtarı yolu olarak '/myPrimaryKey' kullanın.

## <a name="provision-throughput-using-net-sdk"></a>.NET SDK’sını kullanarak aktarım hızı sağlama

> [!Note]
> Cassandra API dışında tüm API’lere aktarım hızı sağlamak için SQL API’sini kullanın.

### <a id="dotnet-most"></a>SQL, MongoDB, Gremlin ve Tablo API'leri

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 1000 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB'de aktarım hızı sağlama hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Veritabanı için aktarım hızı sağlama](how-to-provision-database-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
