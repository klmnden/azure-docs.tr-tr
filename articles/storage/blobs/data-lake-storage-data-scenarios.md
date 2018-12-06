---
title: Azure Data Lake depolama Gen2 içeren veri senaryoları | Microsoft Docs
description: Hangi veri kullanarak araçları ve farklı senaryoları alınan, işlenen, indirilen ve Data Lake depolama Gen2'ye (daha önce Azure Data Lake Store da bilinir) olarak görselleştirilir anlama
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.openlocfilehash: 978f86141d72cc7be43f24909f9780ab9570605d
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52974895"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Azure Data Lake depolama Gen2 büyük veri gereksinimleri için kullanma

Büyük veri işleme, dört anahtar aşaması vardır:

* Bir veri deposuna toplu veya gerçek zamanlı büyük miktarlarda veri almak
* Veri işleme
* Veri yükleme
* Verileri Görselleştirme

Bu makalede, bu aşamasında, büyük veri ihtiyaçlarınızı karşılamak kullanılabilen araçlar ve Seçenekler anlamak için Azure Data Lake depolama Gen2 göre bakacağız.

## <a name="ingest-data-into-data-lake-storage-gen2"></a>Data Lake depolama Gen2 veri alma
Bu bölüm, farklı veri kaynaklarına ve bu verileri bir Data Lake depolama Gen2 hesaba aktarılabilir farklı yollarını vurgular.

![Data Lake depolama Gen2 alabilen](./media/data-lake-storage-data-scenarios/ingest-data.png "verileri Data Lake depolama Gen2 alma")

### <a name="ad-hoc-data"></a>Geçici veri
Bu, daha küçük veri kümeleri temsil eder bir büyük veri uygulaması prototip oluşturma için kullanılır. Veri kaynağı bağlı olarak geçici veri almak için farklı yolu vardır.

| Veri Kaynağı | Kullanarak ayırın |
| --- | --- |
| Yerel bilgisayar |<ul> <li>[Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)</ul> |
| Azure depolama blobu |<ul> <li>[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)</li> <li>[AzCopy aracı](../common/storage-use-azcopy-v10.md)</li><li>[HDInsight küme üzerinde çalışan DistCp](data-lake-storage-use-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Akış verileri
Bu uygulamalar, cihazlardan, sensörlerden, vb. gibi çeşitli kaynaklar tarafından oluşturulan verileri temsil eder. Bu veriler, çeşitli araçlar tarafından Data Lake depolama Gen2 aktarılabilir. Bu araçlar genellikle yakalamak ve bir olay tarafından olay temelinde verileri işleyen gerçek zamanlı olarak ve böylece daha fazla işlenebilir olayları toplu olarak Data Lake depolama Gen2 yazın.

Kullanabileceğiniz araçlar şunlardır:

* [Azure HDInsight Storm](../../hdinsight/storm/apache-storm-write-data-lake-store.md) -Storm kümelerini doğrudan Data Lake depolama Gen2 için veri yazabilirsiniz.

### <a name="relational-data"></a>İlişkisel veriler
Ayrıca ilişkisel veritabanlarından veri kaynağı. Bir süre, önemli öngörüleri büyük veri işlem hattı işlediğinde sağlayabilen veri çok büyük miktarda ilişkisel veritabanları toplayın. Bu tür verileri Data Lake depolama Gen2 taşımak için aşağıdaki araçları kullanabilirsiniz.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web sunucusu günlüğü verilerini (karşıya yükleme özel uygulamaları kullanarak)
Web sunucusu günlüğü verilerinin analizi büyük veri uygulamaları için yaygın bir kullanım örneği ve günlük dosyaları için Data Lake depolama Gen2 yüklenmek üzere büyük birimleri gerektiriyor çünkü bu tür bir veri kümesi özellikle çağırılır. Betikleri veya bu tür verileri yüklemek için uygulamaları yazmak için aşağıdaki araçlardan herhangi birini kullanabilirsiniz.

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)

Web sunucusu günlüğü verilerini karşıya yükleme ve ayrıca başka türden veriler (örneğin, sosyal yaklaşımları veriler) yükleme, bu bileşenin bir parçası olarak karşıya yükleme, verileri içerecek şekilde esnekliği sunar çünkü kendi özel betikler/uygulamaları yazmak için iyi bir yaklaşım olacaktır. daha büyük, büyük veri uygulaması. Bazı durumlarda bu kod bir betik veya basit bir komut satırı yardımcı programını biçiminde. Diğer durumlarda, bir iş uygulaması veya çözüm, büyük veri işleme tümleştirmenize olanak kodu kullanılıyor olabilir.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile ilişkili verileri
Çoğu HDInsight küme türleri (Hadoop, HBase, Storm), bir veri depolama deposu olarak Data Lake depolama Gen2 destekler. HDInsight kümeleri, Azure depolama BLOB'ları (WASB) veri erişim. Daha iyi performans için kümeyle ilişkili bir Data Lake depolama Gen2 hesabına WASB veri kopyalayabilirsiniz. Verileri kopyalamak için aşağıdaki araçları kullanabilirsiniz.

* [Apache DistCp](data-lake-storage-use-distcp.md)
* [AzCopy aracı](../common/storage-use-azcopy-v10.md)
* [Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Şirket içi veya Iaas Hadoop kümeleri depolanan veri
HDFS kullanarak makinede yerel olarak mevcut Hadoop kümelerindeki büyük miktarlarda verinin depolanabilir. Hadoop kümeleri, bir şirket içi dağıtımda olabilir veya bir Azure Iaas kümesinde içinde olabilir. Bu tür veriler Azure Data Lake depolama 2. nesil için tek seferlik bir yaklaşım veya yinelenen bir şekilde kopyalamak için gereksinimleri olabilir. Bunu başarmak için kullanabileceğiniz çeşitli seçenek vardır. Alternatifleri ve ilişkili dezavantajlarına listesi aşağıda verilmiştir.

| Yaklaşım | Ayrıntılar | Avantajları | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| Azure Data Lake depolama Gen2 doğrudan Hadoop kümelerdeki veri kopyalamak için Azure Data Factory (ADF) kullanın |[ADF HDFS veri kaynağı olarak destekler.](../../data-factory/connector-hdfs.md) |ADF HDFS ve birinci sınıf için uçtan uca yönetim ve izleme için kullanıma hazır destek sağlar. |Veri Yönetimi ağ geçidi şirket içinde dağıtılabilir veya Iaas küme gerektirir |
| Azure depolama için Hadoop veri kopyalamak için Distcp kullanma. Ardından verileri Azure uygun mekanizma kullanılarak depolamadan Data Lake depolama Gen2'ye kopyalayın. |Data Lake depolama Gen2 kullanarak Azure Depolama'dan veri kopyalayabilirsiniz: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[AzCopy aracı](../common/storage-use-azcopy-v10.md)</li><li>[Apache DistCp HDInsight kümelerinde çalıştırma](data-lake-storage-use-distcp.md)</li></ul> |Açık kaynak araçları kullanabilirsiniz. |Birden çok teknoloji kapsayan çok adımlı bir işlem |

### <a name="really-large-datasets"></a>Çok büyük veri kümeleri
Aralık birkaç terabayt veri kümelerini karşıya yükleme için yukarıda anlatılan yöntemlerden kullanarak bazen yavaş ve pahalı olabilir. Böyle durumlarda, aşağıdaki seçenekleri kullanabilirsiniz.

* **Azure ExpressRoute kullanarak**. Azure ExpressRoute, şirket içi altyapı ile Azure veri merkezleri arasında özel bağlantılar oluşturmanızı sağlar. Bu, büyük miktarlarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için [Azure ExpressRoute belgeleri](../../expressroute/expressroute-introduction.md).

## <a name="process-data-stored-in-data-lake-storage-gen2"></a>Data Lake depolama 2. nesil'deki depolanan verileri işleme
Verileri Data Lake depolama 2. nesil'deki kullanılabilir olduğunda, desteklenen büyük veri uygulamaları kullanarak bu veriler üzerinde analiz çalıştırabilirsiniz. Şu anda, Data Lake depolama 2. nesil'deki depolanan veriler üzerinde veri analizi işlerini çalıştırmak için Azure HDInsight ve Azure Databricks kullanabilirsiniz.

![Data Lake depolama Gen2 verileri analiz etme](./media/data-lake-storage-data-scenarios/analyze-data.png "Data Lake depolama Gen2 verileri analiz etme")


## <a name="download-data-from-data-lake-storage-gen2"></a>Verileri Data Lake depolama Gen2 ' indirin
İndirme veya gibi senaryolar için Azure Data Lake depolama Gen2 ' veri taşımak isteyebilirsiniz:

* Diğer depolarda, mevcut veri işleme komut zincirini ile arabirim oluşturmak için veri taşıyın. Örneğin, Azure SQL veritabanı veya şirket içi SQL Server için Data Lake depolama Gen2 ' veri taşımak isteyebilirsiniz.
* Veri, uygulama prototipleri oluştururken IDE ortamlarından işleme için yerel bilgisayarınıza indirin.

![Çıkış verileri Data Lake depolama Gen2](./media/data-lake-storage-data-scenarios/egress-data.png "çıkış verileri Data Lake depolama 2. nesil")

Bu gibi durumlarda, aşağıdaki seçeneklerden herhangi birini kullanabilirsiniz:

* [Azure Data Factory](../../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-storage-use-distcp.md)

## <a name="visualize-data-in-data-lake-storage-gen2"></a>Data Lake depolama Gen2 verileri görselleştirin
Bir karışımını hizmetler, Data Lake depolama 2. nesil'deki depolanan verilerin görsel bir temsilini oluşturmak için kullanabilirsiniz.

![Data Lake depolama Gen2 verileri görselleştirin](./media/data-lake-storage-data-scenarios/visualize-data.png "Data Lake depolama Gen2 verileri görselleştirin")

* Kullanarak başlatabilirsiniz [Azure Data Factory, verileri Azure SQL veri ambarı'na Data Lake depolama Gen2'ye taşımak için](../../data-factory/copy-activity-overview.md)
* Bundan sonra yapabilecekleriniz [Power BI ile Azure SQL veri ambarı tümleştirme](../../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) verilerin görsel bir temsilini oluşturmak için.
