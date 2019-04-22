---
title: Azure Data Lake depolama Gen1 diğer Azure Hizmetleri ile tümleştirme | Microsoft Docs
description: Azure Data Lake depolama Gen1 diğer Azure Hizmetleri ile nasıl tümleştirildiğini anlamak
documentationcenter: ''
services: data-lake-store
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: e28863f9980d6403bef1f88de01b7a9b5271b444
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58879315"
---
# <a name="integrating-azure-data-lake-storage-gen1-with-other-azure-services"></a>Azure Data Lake depolama Gen1 diğer Azure hizmetleriyle tümleştirme
Azure Data Lake depolama Gen1 diğer Azure Hizmetleri ile birlikte geniş bir senaryoları etkinleştirmek için kullanılabilir. Aşağıdaki makalede Data Lake depolama Gen1 ile tümleşik hizmetler listelenir.

## <a name="use-data-lake-storage-gen1-with-azure-hdinsight"></a>Azure HDInsight ile Data Lake depolama Gen1 kullanın
Sağlayabileceğiniz bir [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) HDFS uyumlu depolama alanı olarak Data Lake depolama Gen1 kullanan kümesi. Bu sürümde, Windows ve Linux, Hadoop ve Storm kümeleri için Data Lake depolama Gen1 yalnızca ek depolama alanı kullanabilirsiniz. Bu tür kümeler, varsayılan depolama alanı olarak Azure Storage (WASB) hala kullanın. Ancak, Windows ve Linux üzerinde HBase kümeleri için Data Lake depolama Gen1 varsayılan depolama alanı veya ek depolama veya her ikisi de olarak kullanabilirsiniz.

Bir HDInsight kümesi ile Data Lake depolama Gen1 sağlama konusunda yönergeler için bkz:

* [Azure portalını kullanarak bir HDInsight kümesi ile Data Lake depolama Gen1 sağlayın](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure PowerShell kullanarak varsayılan depolama alanı olarak Data Lake depolama Gen1 ile bir HDInsight kümesi sağlama](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Azure PowerShell kullanarak ek depolama alanı olarak Data Lake depolama Gen1 ile bir HDInsight kümesi sağlama](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-storage-gen1-with-azure-data-lake-analytics"></a>Data Lake depolama Gen1 Azure Data Lake Analytics ile kullanma
[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) bulut ölçeğinde büyük verilerle çalışmanıza olanak tanır. Dinamik olarak kaynak sağlar ve terabayt veya hatta eksabayt boyutlarındaki desteklenen veri kaynakları, bir Data Lake depolama Gen1 olan bir tanesine bir süre içinde depolanan veriler üzerinde analiz gerçekleştirmenize olanak tanır. Özel, Data Lake Analytics'i Data Lake depolama en yüksek düzeyde performans, aktarım hızı ve paralelleştirme, büyük veri iş yükleri sağlama Gen1 ile - çalışmak için optimize edilmiştir.

Data Lake Analytics ile Data Lake depolama Gen1 kullanma hakkında yönergeler için bkz: [Data Lake depolama Gen1 kullanarak Data Lake Analytics ile çalışmaya başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="use-data-lake-storage-gen1-with-azure-data-factory"></a>Data Lake depolama Gen1 Azure Data Factory ile kullanma
Kullanabileceğiniz [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) Azure tabloları, Azure SQL veritabanı, Azure SQL veri ambarı, Azure depolama BLOB'ları ve şirket içi veritabanlarına alabilen için. Öncelikli bir yere Azure ekosistemindeki olduğundan, Azure Data Factory'ye Data Lake depolama Gen1 bu kaynaktan veri alımını düzenleme için kullanılabilir.

Azure Data Factory ile Data Lake depolama Gen1 kullanma hakkında yönergeler için bkz: [Data Lake depolama Gen1 gelen ve giden veri taşıma Data Factory kullanarak](../data-factory/connector-azure-data-lake-store.md).

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-storage-gen1"></a>Verileri Azure depolama Bloblarından Data Lake depolama Gen1 kopyalayın.
Azure Data Lake depolama Gen1 sağlayan, verileri Azure Blob depolama alanından bir Data Lake depolama Gen1 hesabına kopyalamak AdlCopy bir komut satırı aracı sağlar. Daha fazla bilgi için [veri kopyalama Azure depolama Bloblarından Data Lake depolama Gen1 için](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-storage-gen1"></a>Azure SQL veritabanı ile Data Lake depolama Gen1 arasında veri kopyalama
Apache Sqoop alma ve Azure SQL veritabanı ve Data Lake depolama Gen1 arasında verileri dışarı aktarmak için kullanabilirsiniz. Daha fazla bilgi için [Data Lake depolama Gen1 ve Sqoop kullanarak Azure SQL veritabanı arasında veri kopyalama](data-lake-store-data-transfer-sql-sqoop.md).

## <a name="use-data-lake-storage-gen1-with-stream-analytics"></a>Data Lake depolama Gen1 Stream Analytics ile kullanma
Data Lake depolama Gen1 çıkışları biri olarak Azure Stream Analytics'i kullanarak akış verilerini depolamak için kullanabilirsiniz. Daha fazla bilgi için [Stream verileri Azure depolama blobundan Azure Stream Analytics'i kullanarak Data Lake depolama Gen1](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-storage-gen1-with-power-bi"></a>Data Lake depolama Gen1 Power BI ile kullanma
Power BI, verileri analiz etmek ve görselleştirmek için bir Data Lake depolama Gen1 hesabından içeri aktarmak için kullanabilirsiniz. Daha fazla bilgi için [Power BI'ı kullanarak Data Lake depolama Gen1 verileri analiz etme](data-lake-store-power-bi.md).

## <a name="use-data-lake-storage-gen1-with-data-catalog"></a>Data Lake depolama Gen1 veri Kataloğu ile kullanma
Data Lake depolama Gen1 verileri, verilerin kuruluşunuz genelinde bulunabilir olması için Azure veri Kataloğu'na kaydedebilirsiniz. Daha fazla bilgi için [Data Lake depolama Gen1 verileri Azure veri Kataloğu'nda kaydetme](data-lake-store-with-data-catalog.md).

## <a name="use-data-lake-storage-gen1-with-sql-server-integration-services-ssis"></a>SQL Server Integration Services (SSIS) ile Data Lake depolama Gen1 kullanın
SSIS paketi ile Data Lake depolama Gen1 bağlanmak için SSIS Data Lake depolama Gen1 Bağlantı Yöneticisi'ni kullanabilirsiniz. Daha fazla bilgi için [kullanım Data Lake depolama Gen1 SSIS ile](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager).

## <a name="use-data-lake-storage-gen1-with-sql-data-warehouse"></a>SQL veri ambarı ile Data Lake depolama Gen1 kullanın
PolyBase, verileri Data Lake depolama Gen1 SQL Data Warehouse'a veri yüklemek için kullanabilirsiniz. Daha fazla bilgi için [kullanım Data Lake depolama Gen1 SQL veri ambarı ile](../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="use-data-lake-storage-gen1-with-azure-event-hubs"></a>Azure Event Hubs ile Data Lake depolama Gen1 kullanın
Azure Event hubs'ı tarafından alınan veri arşivleme ve yakalama için Azure Data Lake depolama Gen1 kullanabilirsiniz. Daha fazla bilgi için [kullanım Data Lake depolama Gen1 Azure Event Hubs ile](data-lake-store-archive-eventhub-capture.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake depolama Gen1 genel bakış](data-lake-store-overview.md)
* [Get başlangıç portalını kullanarak Data Lake depolama Gen1](data-lake-store-get-started-portal.md)
* [Data Lake depolama Gen1 ile çalışmaya başlama PowerShell'i kullanma](data-lake-store-get-started-powershell.md)  

