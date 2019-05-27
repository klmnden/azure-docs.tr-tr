---
title: Azure Cosmos DB'de kapsayıcı oluşturma
description: Azure Cosmos DB'de kapsayıcı oluşturmayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 7768f94051c6882f08dce360e35bf673c7b1103d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241015"
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
# Create a container with a partition key and provision 400 RU/s throughput.

az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $containerName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 400
```

### <a id="cli-mongodb"></a>MongoDB için Azure Cosmos DB API'si

```azurecli-interactive
# Create a collection with a shard key and provision 400 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $collectionName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myShardKey \
    --throughput 400
```

### <a id="cli-cassandra"></a>Cassandra API

```azurecli-interactive
# Create a table with a partition/primary key and provision 400 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $keyspaceName \
    --partition-key-path /myPrimaryKey \
    --throughput 400
```

### <a id="cli-gremlin"></a>Gremlin API'si

```azurecli-interactive
# Create a graph with a partition key and provision 400 RU/s throughput.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $graphName \
    --name $accountName \
    --db-name $databaseName \
    --partition-key-path /myPartitionKey \
    --throughput 400
```

### <a id="cli-table"></a>Tablo API’si

```azurecli-interactive
# Create a table with 400 RU/s
# Note: you don't need to specify partition key in the following command because the partition key is set on each row.
az cosmosdb collection create \
    --resource-group $resourceGroupName \
    --collection-name $tableName \
    --name $accountName \
    --db-name $databaseName \
    --throughput 400
```

## <a name="create-a-container-using-powershell"></a>PowerShell kullanarak bir kapsayıcı oluşturun

Aşağıdaki örnekler gösteren bir Azure Cosmos DB'de kapsayıcı düzeyinde kaynak sağlanması için gerekli tüm destekleyici kaynakları oluşturma

### <a id="ps-sql"></a>SQL API'Sİ

```azurepowershell-interactive
# Create an Azure Cosmos Account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.
$databaseName = "database1"
$databaseResourceName = $accountName + "/sql/" + $databaseName
$containerName = "container1"
$containerResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

# Create account
$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$accountProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Kind "GlobalDocumentDB" -Name $accountName -PropertyObject $accountProperties


# Create database with shared throughput
$databaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
}
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName -PropertyObject $databaseProperties

# Create a container with default policies
$containerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }
    }; 
    "options"=@{}
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $containerResourceName -PropertyObject $containerProperties 
```

### <a id="ps-cassandra"></a>Cassandra API

```azurepowershell-interactive
# Create an Azure Cosmos Account for Cassandra API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.
$keyspaceName = "keyspace1"
$keyspaceResourceName = $accountName + "/cassandra/" + $keyspaceName
$tableName = "table1"
$tableResourceName = $accountName + "/cassandra/" + $keyspaceName + "/" + $tableName

# Create account
$locations = @(
    @{ "locationName"="West US"; "failoverPriority"=0 },
    @{ "locationName"="East US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$accountProperties = @{
    "capabilities"= @( @{ "name"="EnableCassandra" } );
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Kind "GlobalDocumentDB" -Name $accountName -PropertyObject $accountProperties

# Create keyspace with shared throughput
$keyspaceProperties = @{
    "resource"=@{ "id"=$keyspaceName };
    "options"=@{ "Throughput"="400" }
}
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/keyspaces" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $keyspaceResourceName -PropertyObject $keyspaceProperties

# Create a table
$tableProperties = @{
    "resource"=@{
        "id"=$tableName; 
        "schema"= @{
            "columns"= @(
                @{ "name"= "loadid"; "type"= "uuid" };
                @{ "name"= "machine"; "type"= "uuid" };
                @{ "name"= "cpu"; "type"= "int" };
                @{ "name"= "mtime"; "type"= "int" };
                @{ "name"= "load"; "type"= "float" };
            );
            "partitionKeys"= @(
                @{ "name"= "machine" };
                @{ "name"= "cpu" };
                @{ "name"= "mtime" }; 
            );
            "clusterKeys"= @( 
                @{ "name"= "loadid"; "orderBy"= "asc" }
            )
        }
    }; 
    "options"=@{}
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/keyspaces/tables" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $tableResourceName -PropertyObject $tableProperties 

```

### <a id="ps-mongodb"></a>MongoDB API’si

```azurepowershell-interactive
# Create a collection for an Azure Cosmos Account for MongoDB API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.
$databaseName = "database1"
$databaseResourceName = $accountName + "/mongodb/" + $databaseName
$collectionName = "collection1"
$collectionResourceName = $accountName + "/mongodb/" + $databaseName + "/" + $collectionName

# Create account
$locations = @(
    @{ "locationName"="East US 2"; "failoverPriority"=0 },
    @{ "locationName"="North Central US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$accountProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Kind "MongoDB" -Name $accountName -PropertyObject $accountProperties


# Create database with shared throughput
$databaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName -PropertyObject $databaseProperties


# Create Collection
$collectionProperties = @{
    "resource"=@{
        "id"=$collectionName; 
        "shardKey"= @{ "user_id"="Hash" };
        "indexes"= @(
            @{
                "key"= @{ "keys"=@("user_id", "user_address") };
                "options"= @{ "unique"= "true" }
            };
            @{
                "key"= @{ "keys"=@("_ts") };
                "options"= @{ "expireAfterSeconds"= "1000" }
            }
        )
    }; 
    "options"=@{}
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/collections" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $collectionResourceName -PropertyObject $collectionProperties 
```

### <a id="ps-gremlin"></a>Gremlin API'si

```azurepowershell-interactive
# Create an Azure Cosmos Account for Gremlin API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.
$databaseName = "database1"
$databaseResourceName = $accountName + "/gremlin/" + $databaseName
$graphName = "graph1"
$graphResourceName = $accountName + "/gremlin/" + $databaseName + "/" + $graphName

# Create account
$locations = @(
    @{ "locationName"="East US 2"; "failoverPriority"=0 },
    @{ "locationName"="North Central US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$accountProperties = @{
    "capabilities"= @( @{ "name"="EnableGremlin" } );
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Kind "GlobalDocumentDB" -Name $accountName -PropertyObject $accountProperties


# Create database with shared throughput
$databaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
}
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseResourceName -PropertyObject $databaseProperties


# Create a graph with defaults
$graphProperties = @{
    "resource"=@{
        "id"=$graphName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }
    }; 
    "options"=@{}
} 
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/graphs" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $graphResourceName -PropertyObject $graphProperties 
```

### <a id="ps-table"></a>Tablo API’si

```azurepowershell-interactive
# Create an Azure Cosmos account for Table API
$resourceGroupName = "myResourceGroup"
$location = "West US"
$accountName = "mycosmosaccount" # must be lower case.
$tableName = "table1"
$tableResourceName = $accountName + "/table/" + $tableName

# Create account
$locations = @(
    @{ "locationName"="East US 2"; "failoverPriority"=0 },
    @{ "locationName"="North Central US"; "failoverPriority"=1 }
)

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$accountProperties = @{
    "capabilities"= @( @{ "name"="EnableTable" } );
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="true"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Kind "GlobalDocumentDB" -Name $accountName -PropertyObject $accountProperties


# Create table
$tableProperties = @{
    "resource"=@{ "id"=$tableName };
    "options"=@{ "Throughput"="400" }
}
New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/tables" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $tableResourceName -PropertyObject $tableProperties
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