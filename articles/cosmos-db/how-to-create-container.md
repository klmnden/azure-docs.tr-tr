---
title: Azure Cosmos DB'de kapsayıcı oluşturma
description: Azure Cosmos DB'de kapsayıcı oluşturmayı öğrenin
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/17/2019
ms.author: rimman
ms.openlocfilehash: c075a801a877309709258dd6466e68e46d802eff
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680443"
---
# <a name="create-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcısı oluşturma

Bu makalede bir Azure Cosmos kapsayıcısı (koleksiyon, tablo veya grafik) oluşturmanın farklı yollarını açıklar. Bunun için desteklenen SDK'larını veya Azure portalı, Azure CLI'yı kullanabilirsiniz. Bu makalede, bir kapsayıcı oluşturma, bölüm anahtarı belirtin ve aktarım hızına gösterilmektedir.

## <a name="create-a-container-using-azure-portal"></a>Azure portalını kullanarak kapsayıcı oluşturma

### <a id="portal-sql"></a>SQL API'Sİ

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-sql-api-dotnet.md#create-account), ya da mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni koleksiyon**. Ardından, şu bilgileri sağlayın:

   * Yeni bir veritabanı oluşturur veya mevcut bir kullanarak olup olmadığını gösterir.
   * Bir koleksiyon kimliği girin.
   * Bir bölüm anahtarı girin.
   * (Örneğin, 1000 RU) sağlanacak bir aktarım hızı girin.
   * **Tamam**’ı seçin.

![Vurgulanmış yeni bir koleksiyon ile veri Gezgini'nin ekran görüntüsü bölmesi](./media/how-to-create-container/partitioned-collection-create-sql.png)

### <a id="portal-mongodb"></a>MongoDB için Azure Cosmos DB API'si

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-mongodb-dotnet.md#create-a-database-account), ya da mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni koleksiyon**. Ardından, şu bilgileri sağlayın:

   * Yeni bir veritabanı oluşturur veya mevcut bir kullanarak olup olmadığını gösterir.
   * Bir koleksiyon kimliği girin.
   * Bir parça anahtarı girin.
   * (Örneğin, 1000 RU) sağlanacak bir aktarım hızı girin.
   * **Tamam**’ı seçin.

![Ekran görüntüsü, Azure Cosmos DB API için MongoDB, koleksiyon Ekle iletişim kutusu](./media/how-to-create-container/partitioned-collection-create-mongodb.png)

### <a id="portal-cassandra"></a>Cassandra API

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-cassandra-dotnet.md#create-a-database-account), ya da mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni tablo**. Ardından, şu bilgileri sağlayın:

   * Yeni bir anahtar alanı oluşturuyorsanız, veya mevcut bir kullanarak olup olmadığını gösterir.
   * Tablo adı girin.
   * Özelliklerini girin ve birincil anahtar belirtin.
   * (Örneğin, 1000 RU) sağlanacak bir aktarım hızı girin.
   * **Tamam**’ı seçin.

![Ekran görüntüsü, Cassandra API, Tablo Ekle iletişim kutusu](./media/how-to-create-container/partitioned-collection-create-cassandra.png)

> [!NOTE]
> Cassandra API'si için, bölüm anahtarı birincil anahtar olarak kullanılır.

### <a id="portal-gremlin"></a>Gremlin API'si

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-graph-dotnet.md#create-a-database-account), ya da mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni graf**. Ardından, şu bilgileri sağlayın:

   * Yeni bir veritabanı oluşturuyorsanız, veya mevcut bir kullanarak olup olmadığını gösterir.
   * Graf kimliğini girin.
   * **Sınırsız** depolama kapasitesi seçin.
   * Köşe için bölüm anahtarı girin.
   * (Örneğin, 1000 RU) sağlanacak bir aktarım hızı girin.
   * **Tamam**’ı seçin.

![Ekran görüntüsü, Gremlin API, Grafik Ekle iletişim kutusu](./media/how-to-create-container/partitioned-collection-create-gremlin.png)

### <a id="portal-table"></a>Tablo API’si

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-table-dotnet.md#create-a-database-account), ya da mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni tablo**. Ardından, şu bilgileri sağlayın:

   * Tablo kimliği girin.
   * (Örneğin, 1000 RU) sağlanacak bir aktarım hızı girin.
   * **Tamam**’ı seçin.

![Ekran görüntüsü, tablo API'si, Tablo Ekle iletişim kutusu](./media/how-to-create-container/partitioned-collection-create-table.png)

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

### <a id="cli-mongodb"></a>MongoDB için Azure Cosmos DB API'si

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

### <a id="dotnet-mongodb"></a>MongoDB için Azure Cosmos DB API'si

```csharp
// Create a collection with a partition key by using Mongo Shell:
db.runCommand( { shardCollection: "myDatabase.myCollection", key: { myShardKey: "hashed" } } )
```

> [!Note]
> MongoDB kablo protokolüne kavramı anlamak [istek birimi](request-units.md). Sağlanan aktarım hızı, üzerinde yeni bir koleksiyon oluşturmak için SQL API'si için Azure portalı veya Cosmos DB SDK'ları kullanın.

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition/primary key and provision 1000 RU/s throughput.
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=1000);
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
- [İstek birimleri Azure cosmos DB](request-units.md)
- [Kapsayıcılar ve veritabanları sağlama aktarım hızı](set-throughput.md)
- [Azure Cosmos hesabıyla çalışma](account-overview.md)
