---
title: Azure Cosmos DB’de veritabanı aktarım hızını sağlama
description: Azure Cosmos DB’de aktarım hızını veritabanı düzeyinde sağlamayı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 759adf95604e66209cf3ec5083246d16e952114a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884198"
---
# <a name="provision-throughput-for-a-database-in-azure-cosmos-db"></a>Azure Cosmos DB’de bir veritabanı için aktarım hızını sağlama

Bu makalede Azure Cosmos DB’de bir veritabanına nasıl aktarım hızı sağlandığı açıklanır. Tek bir [kapsayıcıya](how-to-provision-container-throughput.md) aktarım hızı sağlayabilir veya aktarım hızını veritabanına sağlayıp içindeki kapsayıcılar arasında paylaştırabilirsiniz. Azure portal veya Azure Cosmos DB SDK'ları kullanarak veritabanı düzeyinde verimliliği sağlayabilirsiniz.

## <a name="provision-throughput-by-using-azure-portal"></a>Azure portalını kullanarak hazırlama aktarım hızı

### <a id="portal-sql"></a>SQL (Core) API

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos DB hesabı oluşturmayı](create-sql-api-dotnet.md#create-a-database-account), ya da mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve select **yeni veritabanı**. Şu bilgileri sağlayın:

   * Bir veritabanı kimliği girin. 
   * Seçin **sağlama aktarım hızı**.
   * Aktarım hızı (örneğin, 1000 RU) girin.
   * **Tamam**’ı seçin.

![Yeni veritabanı ekran iletişim kutusu](./media/how-to-provision-database-throughput/provision-database-throughput-portal-all-api.png)

## <a name="provision-throughput-by-using-net-sdk"></a>.NET SDK kullanarak sağlama aktarım hızı

> [!Note]
> Tüm API’lere aktarım hızı sağlamak için SQL API’yi kullanın. İsteğe bağlı olarak aşağıdaki örnekte Cassandra API için de kullanabilirsiniz.

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

Azure Cosmos DB'de aktarım hızı sağlama hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Kapsayıcı için aktarım hızı sağlama](how-to-provision-container-throughput.md)
* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
