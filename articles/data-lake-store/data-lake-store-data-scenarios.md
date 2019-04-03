---
title: Data Lake depolama Gen1 içeren veri senaryoları | Microsoft Docs
description: Hangi veri kullanarak araçları ve farklı senaryoları alınan, işlenen, indirilen ve Data Lake depolama Gen1 (daha önce Azure Data Lake Store da bilinir) olarak görselleştirilir anlama
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 0b16154edbda4bedfd4e9b680ba4311e7a235212
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878766"
---
# <a name="using-azure-data-lake-storage-gen1-for-big-data-requirements"></a>Azure Data Lake depolama Gen1 büyük veri gereksinimleri için kullanma

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Büyük veri işleme, dört anahtar aşaması vardır:

* Bir veri deposuna toplu veya gerçek zamanlı büyük miktarlarda veri almak
* Veri işleme
* Veri yükleme
* Verileri Görselleştirme

Bu makalede, bu aşamasında, büyük veri ihtiyaçlarınızı karşılamak kullanılabilen araçlar ve Seçenekler anlamak için Azure Data Lake depolama Gen1 göre bakacağız.

## <a name="ingest-data-into-data-lake-storage-gen1"></a>Data Lake depolama Gen1 veri alma
Bu bölüm, farklı veri kaynaklarına ve bu verileri bir Data Lake depolama Gen1 hesaba aktarılabilir farklı yollarını vurgular.

![Data Lake depolama Gen1 alabilen](./media/data-lake-store-data-scenarios/ingest-data.png "verileri Data Lake depolama Gen1 alma")

### <a name="ad-hoc-data"></a>Geçici veri
Bu, daha küçük veri kümeleri temsil eder bir büyük veri uygulaması prototip oluşturma için kullanılır. Veri kaynağı bağlı olarak geçici veri almak için farklı yolu vardır.

| Veri Kaynağı | Kullanarak ayırın |
| --- | --- |
| Yerel bilgisayar |<ul> <li>[Azure Portal](data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure CLI](data-lake-store-get-started-cli-2.0.md)</li> <li>[Visual Studio için Data Lake Araçları'nı kullanarak](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Azure depolama blobu |<ul> <li>[Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)</li> <li>[AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[HDInsight küme üzerinde çalışan DistCp](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Akış verileri
Bu uygulamalar, cihazlardan, sensörlerden, vb. gibi çeşitli kaynaklar tarafından oluşturulan verileri temsil eder. Bu veriler, çeşitli araçlar tarafından Data Lake depolama Gen1 aktarılabilir. Bu araçlar genellikle yakalamak ve bir olay tarafından olay temelinde verileri işleyen gerçek zamanlı olarak ve böylece daha fazla işlenebilir olayları toplu olarak Data Lake depolama Gen1 yazın.

Kullanabileceğiniz araçlar şunlardır:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -Event Hubs'a alınan olaylar için Azure Data Lake depolama Gen1 yazılabilir kullanarak bir Azure Data Lake depolama Gen1 çıktı.
* [Azure HDInsight Storm](../hdinsight/storm/apache-storm-write-data-lake-store.md) -Storm kümelerini doğrudan Data Lake depolama Gen1 için veri yazabilirsiniz.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – olayları Event Hubs'dan olay alma ve Data Lake depolama Gen1 kullanarak yazma [Data Lake depolama Gen1 .NET SDK'sını](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>İlişkisel veriler
Ayrıca ilişkisel veritabanlarından veri kaynağı. Bir süre, önemli öngörüleri büyük veri işlem hattı işlediğinde sağlayabilen veri çok büyük miktarda ilişkisel veritabanları toplayın. Bu tür verileri Data Lake depolama Gen1 taşımak için aşağıdaki araçları kullanabilirsiniz.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web sunucusu günlüğü verilerini (karşıya yükleme özel uygulamaları kullanarak)
Web sunucusu günlüğü verilerinin analizi büyük veri uygulamaları için yaygın bir kullanım örneği ve günlük dosyaları için Data Lake depolama Gen1 yüklenmek üzere büyük birimleri gerektiriyor çünkü bu tür bir veri kümesi özellikle çağırılır. Betikleri veya bu tür verileri yüklemek için uygulamaları yazmak için aşağıdaki araçlardan herhangi birini kullanabilirsiniz.

* [Azure CLI](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake depolama Gen1 .NET SDK'sı](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

Web sunucusu günlüğü verilerini karşıya yükleme ve ayrıca başka türden veriler (örneğin, sosyal yaklaşımları veriler) yükleme, bu bileşenin bir parçası olarak karşıya yükleme, verileri içerecek şekilde esnekliği sunar çünkü kendi özel betikler/uygulamaları yazmak için iyi bir yaklaşım olacaktır. daha büyük, büyük veri uygulaması. Bazı durumlarda bu kod bir betik veya basit bir komut satırı yardımcı programını biçiminde. Diğer durumlarda, bir iş uygulaması veya çözüm, büyük veri işleme tümleştirmenize olanak kodu kullanılıyor olabilir.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile ilişkili verileri
Çoğu HDInsight küme türleri (Hadoop, HBase, Storm), bir veri depolama deposu olarak Data Lake depolama Gen1 destekler. HDInsight kümeleri, Azure depolama BLOB'ları (WASB) veri erişim. Daha iyi performans için kümeyle ilişkili bir Data Lake depolama Gen1 hesabına WASB veri kopyalayabilirsiniz. Verileri kopyalamak için aşağıdaki araçları kullanabilirsiniz.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy hizmeti](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Şirket içi veya Iaas Hadoop kümeleri depolanan veri
HDFS kullanarak makinede yerel olarak mevcut Hadoop kümelerindeki büyük miktarlarda verinin depolanabilir. Hadoop kümeleri, bir şirket içi dağıtımda olabilir veya bir Azure Iaas kümesinde içinde olabilir. Bu tür veri için Azure Data Lake depolama Gen1 tek seferlik bir yaklaşım veya yinelenen bir şekilde kopyalamak için gereksinimleri olabilir. Bunu başarmak için kullanabileceğiniz çeşitli seçenek vardır. Alternatifleri ve ilişkili dezavantajlarına listesi aşağıda verilmiştir.

| Yaklaşım | Ayrıntılar | Yararları | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| Azure Data Lake depolama Gen1 doğrudan Hadoop kümelerdeki veri kopyalamak için Azure Data Factory (ADF) kullanın |[ADF HDFS veri kaynağı olarak destekler.](../data-factory/connector-hdfs.md) |ADF HDFS ve birinci sınıf için uçtan uca yönetim ve izleme için kullanıma hazır destek sağlar. |Veri Yönetimi ağ geçidi şirket içinde dağıtılabilir veya Iaas küme gerektirir |
| Verilerin Hadoop'tan dosyaları olarak dışarı aktarın. Ardından uygun mekanizma kullanılarak Azure Data Lake depolama Gen1 dosyaları kopyalayın. |Azure Data Lake depolama Gen1 kullanmaya dosyalarını kopyalayabilirsiniz: <ul><li>[Windows işletim sistemi için Azure PowerShell](data-lake-store-get-started-powershell.md)</li><li>[Azure CLI](data-lake-store-get-started-cli-2.0.md)</li><li>Her Data Lake depolama Gen1 SDK'sını kullanarak özel uygulama</li></ul> |Başlamak hızlı. Özelleştirilmiş karşıya yükleme yapabilirsiniz |Çok adımlı işlem birden çok teknoloji içerir. Yönetim ve İzleme Araçları'nın özelleştirilmiş yapısı gereği zaman içinde bir mücadele haline şekilde büyür |
| Azure depolama için Hadoop veri kopyalamak için Distcp kullanma. Ardından verileri Azure uygun mekanizma kullanılarak depolamadan Data Lake depolama Gen1 için kopyalayın. |Data Lake depolama Gen1 kullanarak Azure Depolama'ya veri kopyalayabilirsiniz: <ul><li>[Azure Data Factory](../data-factory/copy-activity-overview.md)</li><li>[AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp HDInsight kümelerinde çalıştırma](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Açık kaynak araçları kullanabilirsiniz. |Birden çok teknoloji kapsayan çok adımlı bir işlem |

### <a name="really-large-datasets"></a>Çok büyük veri kümeleri
Aralık birkaç terabayt veri kümelerini karşıya yükleme için yukarıda anlatılan yöntemlerden kullanarak bazen yavaş ve pahalı olabilir. Böyle durumlarda, aşağıdaki seçenekleri kullanabilirsiniz.

* **Azure ExpressRoute kullanarak**. Azure ExpressRoute, şirket içi altyapı ile Azure veri merkezleri arasında özel bağlantılar oluşturmanızı sağlar. Bu, büyük miktarlarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için [Azure ExpressRoute belgeleri](../expressroute/expressroute-introduction.md).
* **"Çevrimdışı", verilerin karşıya**. Azure ExpressRoute kullanarak herhangi bir nedenden dolayı uygun değil ise, kullanabileceğiniz [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) verilerinizi bir Azure veri merkezine sabit disk sürücüleri göndermeye. Verilerinizi Azure depolama BLOB'ları için önce yüklenir. Ardından [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) veya [AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md) Data Lake depolama Gen1 için Azure depolama Bloblarından veri kopyalamak için.

  > [!NOTE]
  > Olsa da Azure veri merkezine gönderin diskleri dosya boyutları içeri/dışarı aktarma hizmetini kullanarak 195 GB'den büyük olmamalıdır.
  >
  >

## <a name="process-data-stored-in-data-lake-storage-gen1"></a>Data Lake depolama Gen1 içinde depolanan verileri işleme
Verileri Data Lake depolama Gen1 içinde kullanılabilir olduğunda, desteklenen büyük veri uygulamaları kullanarak bu veriler üzerinde analiz çalıştırabilirsiniz. Şu anda, Data Lake depolama Gen1 içinde depolanan veriler üzerinde veri analizi işlerini çalıştırmak için Azure HDInsight ve Azure Data Lake Analytics kullanabilirsiniz.

![Data Lake depolama Gen1 verileri analiz etme](./media/data-lake-store-data-scenarios/analyze-data.png "Data Lake depolama Gen1 verileri analiz etme")

Aşağıdaki örneklere bakabilirsiniz.

* [Bir HDInsight kümesi, depolama alanı olarak Data Lake depolama Gen1 ile oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-storage-gen1"></a>Data Lake depolama Gen1 verileri indirme
İndirme veya gibi senaryolar için Azure Data Lake depolama Gen1 verileri taşımak isteyebilirsiniz:

* Diğer depolarda, mevcut veri işleme komut zincirini ile arabirim oluşturmak için veri taşıyın. Örneğin, Azure SQL veritabanı veya şirket içi SQL Server için Data Lake depolama Gen1 ' veri taşımak isteyebilirsiniz.
* Veri, uygulama prototipleri oluştururken IDE ortamlarından işleme için yerel bilgisayarınıza indirin.

![Çıkış verileri Data Lake depolama Gen1](./media/data-lake-store-data-scenarios/egress-data.png "çıkış verileri Data Lake depolama Gen1")

Bu gibi durumlarda, aşağıdaki seçeneklerden herhangi birini kullanabilirsiniz:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Ayrıca, kendi betik/Data Lake depolama Gen1 verileri indirmek için uygulamanızı yazmak için aşağıdaki yöntemleri kullanabilirsiniz.

* [Azure CLI](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake depolama Gen1 .NET SDK'sı](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-storage-gen1"></a>Data Lake depolama Gen1 verileri görselleştirin
Bir karışımını hizmetler, Data Lake depolama Gen1 içinde depolanan verilerin görsel bir temsilini oluşturmak için kullanabilirsiniz.

![Data Lake depolama Gen1 verileri görselleştirin](./media/data-lake-store-data-scenarios/visualize-data.png "Data Lake depolama Gen1 verileri görselleştirin")

* Kullanarak başlatabilirsiniz [Azure Data Factory, verileri Data Lake depolama Gen1 Azure SQL veri ambarı'na taşımak için](../data-factory/copy-activity-overview.md)
* Bundan sonra yapabilecekleriniz [Power BI ile Azure SQL veri ambarı tümleştirme](../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) verilerin görsel bir temsilini oluşturmak için.
