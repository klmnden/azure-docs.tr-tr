---
title: Azure Databricks farklı veri kaynaklarına bağlanmak | Microsoft Docs
description: Azure Databricks farklı veri kaynaklarına bağlanmak öğrenin.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: nitinme
ms.openlocfilehash: 865313a7c6eabd847529b88ff5fff0b7db438fa5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30174280"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>Azure Databricks veri kaynaklarına bağlanmak

Bu makalede Azure Databricks bağlı Azure tüm farklı veri kaynakları için bağlantılar sağlar. Bir Azure Databricks kümesine (örneğin, Azure Blob Storage, Azure Event Hubs, vb.) Azure veri kaynaklarından veri ayıklamak için bu bağlantıları örneklerde izleyin ve analitik işleri bunlar üzerinde çalıştırın. 

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Databricks çalışma ve Spark kümesi olması gerekir. Bölümündeki yönergeleri izleyin [Azure Databricks ile çalışmaya başlama](quickstart-create-databricks-workspace-portal.md).

## <a name="data-sources-for-azure-databricks"></a>Azure Databricks için veri kaynakları

Aşağıdaki listede, Azure Databricks ile kullanabileceğiniz Azure veri kaynaklarında sağlar. Azure Databricks ile kullanılabilir veri kaynaklarını tam bir listesi için bkz: [veri kaynakları için Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).

- [Azure SQL veritabanı](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html)

    Bu bağlantıyı JDBC ve okuma JDBC arabirimi aracılığıyla paralellik denetleme kullanarak SQL veritabanlarına bağlanmak için DataFrame API sağlar. Bu konu ile kısaltılmış Python Scala API kullanarak ayrıntılı örnekler ve sonunda Spark SQL örnekleri sağlar.
- [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html)

    Bu bağlantı, Data Lake Store ile kimlik doğrulaması için Azure Active Directory hizmet asıl kullanma hakkında örnekler verilmektedir. Ayrıca Data Lake Store'da verileri Azure Databricks erişim hakkında yönergeler sağlar.

- [Azure Blob Depolama](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)

    Bu bağlantı için belirli bir kapsayıcı erişim tuşu veya SAS kullanarak doğrudan Azure Blob Depolama Azure Databricks erişim hakkında örnekler verilmektedir. Bağlantı da bilgi Azure Blob Depolama Azure Databricks erişmek nasıl RDD API kullanarak sağlar.

- [Azure Cosmos DB](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

    Bu bağlantıyı kullanmak hakkında yönergeler sağlar [Azure Cosmos DB Spark bağlayıcı](https://github.com/Azure/azure-cosmosdb-spark) Azure Cosmos veritabanındaki verilere erişmek için Azure Databricks gelen.

- [Azure Event Hubs](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/eventhubs-connector.html)

    Bu bağlantıyı kullanmak hakkında yönergeler sağlar [Azure olay hub'ları Spark bağlayıcı](https://github.com/Azure/azure-event-hubs-spark) Azure Databricks Azure Event Hubs erişim verilerde gelen.

- [Azure SQL Veri Ambarı](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/sql-data-warehouse.html)

    Bu bağlantı, Azure Databricks bağlanmak için Azure SQL Data Warehouse Bağlayıcısı'nı kullanma hakkında yönergeler sağlar.
    

## <a name="next-steps"></a>Sonraki adımlar

Burada Azure Databricks veri alabilirsiniz kaynaklardan hakkında bilgi edinmek için [veri kaynakları için Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#).


