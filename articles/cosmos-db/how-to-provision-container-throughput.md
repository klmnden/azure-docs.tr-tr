---
title: Azure Cosmos DB’de kapsayıcının aktarım hızını sağlama
description: Azure Cosmos DB’de kapsayıcı düzeyinde aktarım hızını sağlamayı öğrenin
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/03/2019
ms.author: rimman
ms.openlocfilehash: 945bff075828bdbddd2a31642b35a5c592216b93
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565878"
---
# <a name="provision-throughput-on-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcısında aktarım hızını sağlama

Bu makalede, bir kapsayıcı (koleksiyon, graf veya tablo) Azure Cosmos DB'de aktarım hızını sağlamak açıklanmaktadır. Tek bir kapsayıcısında aktarım hızını sağlayabilir veya [bir veritabanı sağlama aktarım hızını](how-to-provision-database-throughput.md) ve veritabanı içindeki kapsayıcılar arasında paylaşabilirsiniz. Azure portalı, Azure CLI veya Azure Cosmos DB SDK'larını kullanarak bir kapsayıcısında aktarım hızını sağlayabilirsiniz.

## <a name="provision-throughput-using-azure-portal"></a>Azure portalını kullanarak aktarım hızı sağlama

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-sql-api-dotnet.md#create-account), ya da mevcut bir Azure Cosmos hesabını seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni koleksiyon**. Ardından, şu bilgileri sağlayın:

   * Yeni bir veritabanı oluşturur veya mevcut bir kullanarak olup olmadığını gösterir.
   * Bir kapsayıcı (veya tablo veya grafik) kimliği girin.
   * Bir bölüm anahtarı değerini girin (örneğin, `/userid`).
   * İstediğiniz bir aktarım hızı sağlamak (örneğin, 1000 RU) girin.
   * **Tamam**’ı seçin.

![Veri Gezgini'nin ekran vurgulanmış yeni bir koleksiyon](./media/how-to-provision-container-throughput/provision-container-throughput-portal-all-api.png)

## <a name="provision-throughput-using-azure-cli"></a>Azure CLI kullanarak aktarım hızı sağlama

```azurecli-interactive
# Create a container with a partition key and provision throughput of 400 RU/s
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 400
```

## <a name="provision-throughput-using-powershell"></a>PowerShell kullanarak sağlama aktarım hızı

```azurepowershell-interactive
# Create a container with a partition key and provision throughput of 400 RU/s
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{ "Throughput"= 400 }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

MongoDB için Azure Cosmos DB API'si ile yapılandırılan bir Azure Cosmos hesabındaki bir kapsayıcıya aktarım hızını sağlıyorsanız kullanın `/myShardKey` bölüm anahtar yolu. Cassandra API ile yapılandırılmış bir Azure Cosmos hesabındaki bir kapsayıcıya aktarım hızını sağlıyorsanız kullanın `/myPrimaryKey` bölüm anahtar yolu.

## <a name="provision-throughput-by-using-net-sdk"></a>.NET SDK kullanarak sağlama aktarım hızı

> [!Note]
> Cosmos SDK'ları, Cassandra API dışındaki tüm Cosmos DB API'leri için sağlama aktarım hızı için SQL API'si için kullanın.

### <a id="dotnet-most"></a>SQL, MongoDB, Gremlin ve Tablo API'leri

```csharp
// Create a container with a partition key and provision throughput of 400 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 400 });
```

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra table with a partition (primary) key and provision throughput of 400 RU/s
session.Execute(CREATE TABLE myKeySpace.myTable(
    user_id int PRIMARY KEY,
    firstName text,
    lastName text) WITH cosmosdb_provisioned_throughput=400);
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de aktarım hızı sağlama hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Veritabanı aktarım hızını sağlamasını yapma](how-to-provision-database-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
