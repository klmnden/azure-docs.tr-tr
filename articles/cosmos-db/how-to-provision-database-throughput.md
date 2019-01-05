---
title: Azure Cosmos DB’de veritabanı aktarım hızını sağlama
description: Azure Cosmos DB’de aktarım hızını veritabanı düzeyinde sağlamayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 7c253141e0e6e76f845d08e68a1d79949fe811e8
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034242"
---
# <a name="provision-throughput-for-a-database-in-azure-cosmos-db"></a>Azure Cosmos DB’de bir veritabanı için aktarım hızını sağlama

Bu makalede Azure Cosmos DB’de bir veritabanına nasıl aktarım hızı sağlandığı açıklanır. Tek bir [kapsayıcıya](how-to-provision-container-throughput.md) aktarım hızı sağlayabilir veya aktarım hızını veritabanına sağlayıp içindeki kapsayıcılar arasında paylaştırabilirsiniz. Azure portalını veya Cosmos DB SDK’larını kullanarak veritabanı düzeyinde aktarım hızı sağlayabilirsiniz.

## <a name="provision-throughput-using-azure-portal"></a>Azure portalını kullanarak aktarım hızı sağlama

### <a id="portal-sql"></a>SQL (Core) API

1. [Azure portalda](https://portal.azure.com/) oturum açın.

1. [Yeni bir Cosmos DB hesabı oluşturun](create-sql-api-dotnet.md#create-a-database-account) veya mevcut hesabı seçin.

1. **Veri Gezgini** bölmesini açın ve **Yeni Veritabanı**'nı seçin. Ardından aşağıdaki ayrıntılarla formu doldurun:

   * Veritabanı Kimliği girin. 
   * Aktarım hızı sağla'yı seçin.
   * Bir aktarım hızı (örneğin 1000 RU) girin.
   * **Tamam**’ı seçin.

![SQL API veritabanı aktarım hızı sağlama](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-net-sdk"></a>.NET SDK’sını kullanarak aktarım hızı sağlama

> [!Note]
> Tüm API’lere aktarım hızı sağlamak için SQL API’yi kullanın. İsteğe bağlı olarak Cassandra API için aşağıdaki örneği de kullanabilirsiniz.

### <a id="dotnet-all"></a>Tüm API’ler

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 10000
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

### <a id="dotnet-cassandra"></a>Cassandra API

```csharp
// Create a Cassandra keyspace and provision throughput of 10000 RU/s
session.Execute(CREATE KEYSPACE IF NOT EXISTS myKeySpace WITH cosmosdb_provisioned_throughput=10000);
```

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB'de aktarım hızı sağlama hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Kapsayıcı için aktarım hızı sağlama](how-to-provision-container-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
