---
title: Azure Cosmos DB’de veritabanı aktarım hızını sağlama
description: Azure Cosmos DB’de aktarım hızını veritabanı düzeyinde sağlamayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: c648522e689c64de8e7e09b85ca3b6eb26b6945b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55477208"
---
# <a name="provision-throughput-on-an-azure-cosmos-database"></a>Bir Azure Cosmos veritabanı aktarım hızını sağlama

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
