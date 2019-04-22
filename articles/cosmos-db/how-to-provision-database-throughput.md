---
title: Azure Cosmos DB’de veritabanı aktarım hızını sağlama
description: Azure Cosmos DB’de aktarım hızını veritabanı düzeyinde sağlamayı öğrenin
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: da56c06e215e02ee3eefe3d0552c962a8c59011e
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683486"
---
# <a name="provision-throughput-on-a-database-in-azure-cosmos-db"></a>Azure Cosmos DB veritabanı aktarım hızını sağlama

Bu makalede, Azure Cosmos DB veritabanı aktarım hızını sağlamak açıklanmaktadır. Tek bir [kapsayıcıya](how-to-provision-container-throughput.md) aktarım hızı sağlayabilir veya aktarım hızını veritabanına sağlayıp içindeki kapsayıcılar arasında paylaştırabilirsiniz. Ne zaman kapsayıcı düzeyinde ve veritabanı düzeyinde aktarım hızı kullanılacağını öğrenmek için bkz [aktarım hızı kapsayıcılar ve veritabanları hakkında sağlamaya yönelik kullanım](set-throughput.md) makalesi. Azure portal veya Azure Cosmos DB SDK'ları kullanarak veritabanı düzeyinde verimliliği sağlayabilirsiniz.

## <a name="provision-throughput-using-azure-portal"></a>Azure portalını kullanarak aktarım hızı sağlama

### <a id="portal-sql"></a>SQL (Core) API

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-sql-api-dotnet.md#create-account), ya da mevcut bir Azure Cosmos hesabını seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni veritabanı**. Şu bilgileri sağlayın:

   * Bir veritabanı kimliği girin. 
   * Seçin **sağlama aktarım hızı**.
   * Aktarım hızı (örneğin, 1000 RU) girin.
   * **Tamam**’ı seçin.

![Yeni veritabanı ekran iletişim kutusu](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-using-net-sdk"></a>.NET SDK’sını kullanarak aktarım hızı sağlama

> [!Note]
> Tüm API'ler için sağlama aktarım hızı için SQL API'si için Cosmos Sdk'leri kullanabilirler. İsteğe bağlı olarak aşağıdaki örnekte Cassandra API için de kullanabilirsiniz.

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

Azure Cosmos DB'de sağlanan aktarım hızı hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Genel olarak sağlanan aktarım hızını ölçeklendirme](scaling-throughput.md)
* [Kapsayıcılar ve veritabanları sağlama aktarım hızı](set-throughput.md)
* [Kapsayıcı için aktarım hızı sağlama](how-to-provision-container-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)