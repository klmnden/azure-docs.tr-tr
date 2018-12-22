---
title: Azure Cosmos DB'de kapsayıcı oluşturma
description: Azure Cosmos DB'de kapsayıcı oluşturmayı öğrenin
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: bf91377b544328ed63d3767ce9b0064a6c4eeaff
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53711899"
---
# <a name="create-a-container-in-azure-cosmos-db"></a>Azure Cosmos DB'de kapsayıcı oluşturma

Bu makalede kapsayıcı oluşturmanın farklı yolları (koleksiyon, tablo, graf) açıklanır. Azure portalı, Azure CLI veya desteklenen SDK'lar kullanılarak kapsayıcı oluşturulabilir. Bu makalede kapsayıcıyı oluşturma, bölüm anahtarını belirtme ve aktarım hızını sağlama işlemleri gösterilir.

## <a name="create-a-container-using-azure-portal"></a>Azure portalını kullanarak kapsayıcı oluşturma

### <a id="portal-sql"></a>SQL API'Sİ

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. [Yeni bir Cosmos DB hesabı oluşturun](create-sql-api-dotnet.md#create-a-database-account) veya mevcut hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni Koleksiyon**'u seçin. Ardından aşağıdaki ayrıntılarla formu doldurun:

   * Yeni bir veritabanı oluşturun veya var olanlardan birini kullanın.
   * Koleksiyon Kimliğini girin.
   * Bölüm anahtarını girin.
   * Bir aktarım hızı (örneğin 1000 RU) girin.
   * **Tamam**’ı seçin.

![SQL API'si bir koleksiyon oluşturur](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>Azure Cosmos DB MongoDB API'si için

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. [Yeni bir Cosmos DB hesabı oluşturun](create-mongodb-dotnet.md#create-a-database-account) veya mevcut hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni Koleksiyon**'u seçin. Ardından aşağıdaki ayrıntılarla formu doldurun:

   * Yeni bir veritabanı oluşturun veya var olanlardan birini kullanın.
   * Koleksiyon Kimliğini girin.
   * **Sınırsız** depolama kapasitesi seçin.
   * Parça anahtarı girin.
   * Bir aktarım hızı (örneğin 1000 RU) girin.
   * **Tamam**’ı seçin.

![Azure Cosmos DB MongoDB API'si için bir koleksiyon oluşturur.](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra API

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. [Yeni bir Cosmos DB hesabı oluşturun](create-cassandra-dotnet.md#create-a-database-account) veya mevcut hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni Tablo**'yu seçin. Ardından aşağıdaki ayrıntılarla formu doldurun:

   * Yeni bir Keyspace oluşturun veya var olanlardan birini kullanın.
   * Tablo adı girin.
   * Özellikleri girin ve BİRİNCİL ANAHTAR belirtin.
   * Bir aktarım hızı (örneğin 1000 RU) girin.
   * **Tamam**’ı seçin.

![Cassandra API'si bir koleksiyon oluşturur](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Cassandra API'si için, bölüm anahtarı birincil anahtar olarak kullanılır.

### <a id="portal-gremlin"></a>Gremlin API'si

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. [Yeni bir Cosmos DB hesabı oluşturun](create-graph-dotnet.md#create-a-database-account) veya mevcut hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni Graf**'ı seçin. Ardından aşağıdaki ayrıntılarla formu doldurun:

   * Yeni bir veritabanı oluşturun veya var olanlardan birini kullanın.
   * Graf kimliğini girin.
   * **Sınırsız** depolama kapasitesi seçin.
   * Köşeler için Bölüm anahtarı girin.
   * Bir aktarım hızı (örneğin 1000 RU) girin.
   * **Tamam**’ı seçin.

![Gremlin API'si bir koleksiyon oluşturur](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Tablo API’si

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. [Yeni bir Cosmos DB hesabı oluşturun](create-table-dotnet.md#create-a-database-account) veya mevcut hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni Tablo**'yu seçin. Ardından aşağıdaki ayrıntılarla formu doldurun:

   * Tablo Kimliği girin.
   * **Sınırsız** depolama kapasitesi seçin.
   * Bir aktarım hızı (örneğin 1000 RU) girin.
   * **Tamam**’ı seçin.

![Tablo API'si bir koleksiyon oluşturur](./media/how-to-create-container/partitioned-collection-create-table.png)

> [!Note]
> Tablo API'si için, her yeni satır eklediğinizde bölüm anahtarı belirtilir.

## <a name="create-a-container-using-azure-cli"></a>Azure CLI kullanarak kapsayıcı oluşturma

### <a id="cli-sql"></a>SQL API'Sİ

```azurecli-interactive
# Create a container with a partition key and provision 1000 RU/s throughput.

az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-mongodb"></a>Azure Cosmos DB MongoDB API'si için

```azurecli-interactive
# Create a collection with a shard key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $collectionName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myShardKey \
    --throughput 1000
```

### <a id="cli-cassandra"></a>Cassandra API

```azurecli-interactive
# Create a table with a partition/primary key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $keyspaceName \
    --partition-key-path /myPrimaryKey \
    --throughput 1000
```

### <a id="cli-gremlin"></a>Gremlin API'si

```azurecli-interactive
# Create a graph with a partition key and provision 1000 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $graphName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 1000
```

### <a id="cli-table"></a>Tablo API’si

```azurecli-interactive
# Create a table with 1000 RU/s
# Note: you don't need to specify partition key in the following command because the partition key is set on each row.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $databaseName \
    --throughput 1000
```

## <a name="create-a-container-using-net-sdk"></a>.NET SDK'sını kullanarak kapsayıcı oluşturma

### <a id="dotnet-sql-graph"></a>SQL API'si ve Gremlin API'si

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

### <a id="dotnet-mongodb"></a>Azure Cosmos DB MongoDB API'si için

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
İstek birimi kavramı MongoDB kablo protokolüne sahip değil. İşleme ile yeni bir koleksiyon oluşturmak için Azure portalı veya SQL API kullandınız örneklerde gösterildiği gibi kullanın.

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB'de bölümleme hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
