---
title: "Azure Databricks'ten farklı veri kaynaklarına bağlanın "
description: Azure Databricks'ten farklı veri kaynaklarına bağlanmayı öğreneceksiniz.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: mamccrea
ms.openlocfilehash: f2b7136ec21416e31c2af658974577023a4494de
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60240343"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Azure Databricks'ten veri kaynaklarına bağlanın

Bu makalede, azure'da Azure Databricks bağlı tüm farklı veri kaynaklarına bağlantılar sağlar. Örnek verileri Azure veri kaynaklarından (örneğin, Azure Blob Depolama, Azure Event Hubs, vb.) bir Azure Databricks kümesine ayıklamak için bu bağlantıları izleyin ve analiz işleri üzerinde çalıştırın. 

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Databricks çalışma alanı ve bir Spark kümesi olması gerekir. Konumundaki yönergeleri [Azure Databricks ile çalışmaya başlama](quickstart-create-databricks-workspace-portal.md).

## <a name="data-sources-for-azure-databricks"></a>Azure Databricks için veri kaynakları

Aşağıdaki liste, Azure Databricks ile kullanabileceğiniz bir Azure veri kaynakları sağlar. Azure Databricks ile kullanılabilecek veri kaynaklarını tam bir listesi için bkz. [veri kaynakları için Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).

- [Azure SQL veritabanı](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html)

    Bu bağlantı, JDBC ve paralellik okumalar JDBC arabirimi üzerinden denetleme kullanarak SQL veritabanlarına bağlanmak için veri çerçevesi API sağlar. Bu konu ile kısaltılmış Python Scala API'yi kullanarak ayrıntılı örnekler ve sonunda Spark SQL örnekleri sağlar.
- [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)

    Bu bağlantı, Azure Active Directory Hizmet sorumlusu, Data Lake Store ile kimlik doğrulaması için kullanma hakkında örnekler sağlar. Ayrıca Azure Databricks'ten Data Lake Store verilere erişim hakkında yönergeler açıklanmaktadır.

- [Azure Blob Depolama](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)

    Bu bağlantı için belirli bir kapsayıcı erişim anahtarı veya SAS kullanarak doğrudan Azure Databricks'ten Azure Blob depolamaya erişmek nasıl örnekler sağlar. Bağlantıyı da bilgileri Azure Databricks'ten Azure Blob depolamaya erişmek nasıl RDD API'sini kullanarak sağlar.

- [Azure Cosmos DB](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

    Bu bağlantının nasıl kullanılacağı hakkında yönergeler sağlar. [Azure Cosmos DB Spark Bağlayıcısı](https://github.com/Azure/azure-cosmosdb-spark) gelen Azure Databricks, Azure Cosmos DB'de verilere erişme.

- [Azure Event Hubs](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/eventhubs-connector.html)

    Bu bağlantının nasıl kullanılacağı hakkında yönergeler sağlar. [Azure olay hub'ları Spark Bağlayıcısı](https://github.com/Azure/azure-event-hubs-spark) gelen Azure Databricks, Azure Event Hubs verilerine erişim için.

- [Azure SQL Veri Ambarı](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/sql-data-warehouse.html)

    Bu bağlantı, Azure Databricks'ten bağlanmak için Azure SQL veri ambarı Bağlayıcısı'nı kullanmak hakkında yönergeler açıklanmaktadır.
    

## <a name="next-steps"></a>Sonraki adımlar

Burada Azure Databricks'e verileri içeri aktarabilirsiniz kaynaklardan hakkında bilgi edinmek için bkz. [veri kaynakları için Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#).


