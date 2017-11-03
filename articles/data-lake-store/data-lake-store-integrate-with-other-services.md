---
title: "Data Lake Store diğer Azure hizmetleriyle tümleştirme | Microsoft Docs"
description: "Data Lake Store diğer Azure hizmetleriyle nasıl tümleşik çalıştığını anlamak"
documentationcenter: 
services: data-lake-store
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: de7aff6b31d937576da65498c5fcce2ae9abdbf1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="integrating-data-lake-store-with-other-azure-services"></a>Data Lake Store’u diğer Azure Hizmetleri ile tümleştirme
Azure Data Lake Store diğer Azure hizmetleriyle birlikte daha geniş çeşitli senaryoları etkinleştirmek için kullanılabilir. Aşağıdaki makalede Data Lake Store ile tümleşik hizmetler listelenmektedir.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Kullanım Data Lake Store Azure Hdınsight ile
Kaynak sağlayabilirsiniz bir [Azure Hdınsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) Data Lake Store, HDFS uyumlu depolama alanı olarak kullanan kümesi. Bu sürümde, Windows ve Linux, Hadoop ve Storm kümeleri için Data Lake Store yalnızca ek depolama alanı kullanabilirsiniz. Bu tür kümeler hala varsayılan depolama alanı olarak Azure Storage (WASB) kullanın. Ancak, Windows ve Linux üzerinde HBase kümeleri için Data Lake Store varsayılan depolama veya ek depolama alanı ya da her ikisini de kullanabilirsiniz.

Bir Hdınsight kümesini Data Lake Store sağlama hakkında daha fazla yönerge için bkz:

* [Azure Portal'ı kullanarak Data Lake Store ile Hdınsight kümesi sağlama](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure PowerShell kullanarak varsayılan depolama alanı olarak bir Hdınsight kümesini Data Lake Store sağlama](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Azure PowerShell kullanarak ek depolama alanı bir Hdınsight kümesini Data Lake Store sağlama](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Azure Data Lake Analytics ile kullanım veri Gölü deposu
[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) bulut ölçeğinde büyük verilerle çalışmanıza olanak tanır. Dinamik olarak kaynaklar sağlar ve terabayt ve hatta eksabayt desteklenen veri kaynakları, bir Data Lake Store olan tanesine sayısında depolanan veriler üzerinde analiz gerçekleştirmenize olanak tanır. Data Lake Analytics özellikle Azure Data Lake performansı, verimliliği ve paralelleştirmeyi sizin için en yüksek düzeyde büyük veri iş yüklerini sağlayan Store ile çalışmak için optimize edilmiştir.

Data Lake Analytics Data Lake Store ile kullanma hakkında daha fazla yönerge için bkz: [Data Lake Store'ı kullanarak Data Lake Analytics ile çalışmaya başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="use-data-lake-store-with-azure-data-factory"></a>Kullanım Data Lake Store ile Azure veri fabrikası
Kullanabileceğiniz [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) Azure tabloları, Azure SQL Database, Azure SQL veri ambarı, Azure Storage Bloblarında ve şirket içi veritabanlarından veri alma için. Birinci sınıf vatandaşı Azure ekosistemindeki olmaya, Azure Data Factory Azure Data Lake Store bu kaynaktan gelen veri alım düzenlemek için kullanılabilir.

Azure Data Factory Data Lake Store ile kullanma hakkında daha fazla yönerge için bkz: [Data Factory kullanarak Data Lake Store gelen ve veri taşıma](../data-factory/connector-azure-data-lake-store.md).

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Verileri Azure Storage Bloblarından Data Lake Store kopyalayın.
Azure Data Lake Store komut satırı aracı, verileri Azure Blob depolama alanından bir Data Lake Store hesabına kopyalamak sağlayan AdlCopy sağlar. Daha fazla bilgi için bkz: [veri kopyalama Azure Storage Bloblarından Data Lake Store'a](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Azure SQL veritabanı ve Data Lake Store arasında veri kopyalama
Apache Sqoop içeri aktarın ve Azure SQL veritabanı ve Data Lake Store arasında veri dışarı aktarmak için kullanabilirsiniz. Daha fazla bilgi için bkz: [Data Lake Store ile Sqoop kullanarak Azure SQL veritabanı arasında veri kopyalama](data-lake-store-data-transfer-sql-sqoop.md).

## <a name="use-data-lake-store-with-stream-analytics"></a>Kullanım Data Lake Store ile akış analizi
Data Lake Store çıkışları biri olarak kullanarak Azure Stream Analytics akış verilerini depolamak için kullanabilirsiniz. Daha fazla bilgi için bkz: [akış Azure depolama Blob verileri Azure akış analizi kullanarak Data Lake Store](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Kullanım Data Lake Store Power BI ile
Power BI çözümlemek ve verileri görselleştirmek için bir Data Lake Store hesabından veri almak için kullanabilirsiniz. Daha fazla bilgi için bkz: [Power BI kullanarak Data Lake Store verileri çözümlemek](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Kullanım Data Lake Store veri Kataloğu ile
Data Lake Store verilerden veri kuruluşunuz genelinde bulunabilir duruma getirmek için Azure veri Kataloğu içine kaydedebilirsiniz. Daha fazla bilgi için bkz: [Data Lake Store verileri Azure veri Kataloğu'nda kaydetmek](data-lake-store-with-data-catalog.md).

## <a name="use-data-lake-store-with-sql-server-integration-services-ssis"></a>Kullanım Data Lake Store ile SQL Server Integration Services (SSIS)
Azure Data Lake Store ile bir SSIS paketi bağlanmak için SSIS Azure Data Lake Store Bağlantı Yöneticisi'ni kullanın. Daha fazla bilgi için bkz: [kullanım Data Lake Store SSIS ile](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager).

## <a name="use-data-lake-store-with-sql-data-warehouse"></a>Kullanım Data Lake Store ile SQL veri ambarı
PolyBase, Azure Data Lake Deposu'ndan veri SQL Data Warehouse'a veri yüklemek için kullanabilirsiniz. Daha fazla bilgi için bkz: [kullanım Data Lake Store ile SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="use-data-lake-store-with-azure-event-hubs"></a>Azure Event Hubs ile kullanım veri Gölü deposu
Azure Event Hubs tarafından alınan Arşiv ve yakalama verileri için Azure Data Lake Store kullanabilirsiniz. Daha fazla bilgi için bkz: [kullanım Data Lake Store Azure Event Hubs ile](data-lake-store-archive-eventhub-capture.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Portal kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* [PowerShell kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-powershell.md)  

