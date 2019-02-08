---
title: Azure Data Lake depolama Gen2 diğer Azure Hizmetleri ile tümleştirme | Microsoft Docs
description: Azure Data Lake depolama Gen2 diğer Azure Hizmetleri ile nasıl tümleştirildiğini anlamak
documentationcenter: ''
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: nitinme
ms.openlocfilehash: 7bc4889403e1d52cfa3b18792a554f0faf605132
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55885691"
---
# <a name="integrate-azure-data-lake-storage-gen2-with-other-azure-services"></a>Azure Data Lake depolama Gen2 diğer Azure hizmetleriyle tümleştirme

Azure Hizmetleri, içe alma, analiz edin ve Azure Data Lake depolama Gen2'ye depolama hesabınızdaki verileri görselleştirmek için kullanabilirsiniz. En iyi yapmaya çalıştığınızı bu görevleri uygun hizmetler seçin.

## <a name="ingest-data-into-your-data-lake"></a>İçinde veri gölü veri alma

Bu hizmetler, veri gölü veri ile doldurmak yardımcı olur.

### <a name="azure-data-factory"></a>Azure Data Factory

Kullanabileceğiniz [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) bu kaynaklardan veri almak için:

* Azure tabloları
* Azure SQL veritabanları
* Azure SQL veri ambarı
* Azure depolama BLOB'ları
* Şirket içi veritabanları

Bkz: [Data Lake depolama Gen2'ye gelen ve giden veri taşıma Data Factory kullanarak](../../data-factory/connector-azure-data-lake-store.md).

## <a name="analyze-and-visualize-data-in-your-data-lake"></a>Analiz edin ve verileri, veri gölü'nde görselleştirin

Bu hizmetler, Data Lake depolama Gen2'ye bir depolama uç noktası olarak kullanabilirsiniz.

### <a name="azure-hdinsight"></a>Azure HDInsight

Sağlayabileceğiniz bir [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) HDFS uyumlu depolama alanı olarak Data Lake depolama Gen2 kullanan kümesi. Bu sürümde, Windows ve Linux, Hadoop ve Storm kümeleri için Data Lake depolama Gen2'ye yalnızca bir ek depolama kullanabilirsiniz. Bu tür kümeler, varsayılan depolama alanı olarak Azure Storage (WASB) hala kullanın. Ancak, Windows ve Linux üzerinde HBase kümeleri için Data Lake depolama Gen2 varsayılan depolama alanı ve ek depolama olarak kullanabilirsiniz.

Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

Kullanabileceğiniz [Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-overview.md) bulut ölçeğinde büyük verilerle çalışmak için. Dinamik olarak kaynak sağlar ve eksabaytlarca veriyi analiz etmenize olanak tanır. Data Lake Analytics'i Data Lake depolama Gen2'ye bir veri kaynağı olarak kullanmak için optimize edilmiştir. 

Bkz: [Data Lake depolama Gen2'ı kullanarak Data Lake Analytics ile çalışmaya başlama](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="copy-data-to-other-repositories"></a>Diğer depolarda için veri kopyalama

Bu hizmetler, veri gölü verileri kopyalayın ve bunları diğer depoya yerleştirmek için kullanın.

### <a name="sql-data-warehouse"></a>SQL Veri Ambarı

PolyBase, verileri Data Lake depolama Gen2 ' SQL Data Warehouse'a veri yüklemek için kullanabilirsiniz. Daha fazla bilgi için [kullanım Data Lake depolama Gen2 SQL veri ambarı ile](../../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Data Lake depolama Gen2'ye genel bakış](data-lake-storage-introduction.md)
* [Azure Data Lake depolama Gen2 büyük veri gereksinimleri için kullanma](data-lake-storage-data-scenarios.md)
