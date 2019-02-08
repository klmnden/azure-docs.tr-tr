---
title: Azure Cosmos DB’de kapsayıcının aktarım hızını sağlama
description: Azure Cosmos DB’de kapsayıcı düzeyinde aktarım hızını sağlamayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 4df8a12581b5d71a76964ca1e3d40c6c53185f67
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860329"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcısında aktarım hızını sağlama

Bu makalede, Azure Cosmos DB'de bir kapsayıcı (koleksiyon, graf veya tablo) için aktarım hızına açıklanmaktadır. Tek bir kapsayıcı için aktarım hızı sağlayabilir veya [bir veritabanı için sağlama](how-to-provision-database-throughput.md) ve veritabanı içindeki kapsayıcılar arasında paylaşabilirsiniz. Bir kapsayıcı için aktarım hızı, Azure portalı, Azure CLI veya Azure Cosmos DB SDK'ları kullanarak sağlayabilirsiniz.

## <a name="provision-throughput-by-using-azure-portal"></a>Azure portalını kullanarak hazırlama aktarım hızı

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos DB hesabı oluşturmayı](create-sql-api-dotnet.md#create-a-database-account), ya da mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni koleksiyon**. Ardından, şu bilgileri sağlayın:

   * Yeni bir veritabanı oluşturur veya mevcut bir kullanarak olup olmadığını gösterir.
   * Bir koleksiyon kimliği (veya tablo veya grafik) girin.
   * Bir bölüm anahtarı değerini girin (örneğin, `/userid`).
   * Aktarım hızı (örneğin, 1000 RU) girin.
   * **Tamam**’ı seçin.

![Veri Gezgini'nin ekran vurgulanmış yeni bir koleksiyon](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-azure-cli"></a>Azure CLI kullanarak sağlama aktarım hızı

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

MongoDB için Azure Cosmos DB API'si ile yapılandırılan bir Azure Cosmos DB hesabı için aktarım hızı sağlıyorsanız kullanın `/myShardKey` bölüm anahtar yolu. Cassandra API'si için yapılandırılmış bir Azure Cosmos DB hesabı için aktarım hızı sağlıyorsanız kullanın `/myPrimaryKey` bölüm anahtar yolu.

## <a name="provision-throughput-by-using-net-sdk"></a>.NET SDK kullanarak sağlama aktarım hızı

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

Azure Cosmos DB'de aktarım hızı sağlama hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Veritabanı için aktarım hızı sağlama](how-to-provision-database-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
