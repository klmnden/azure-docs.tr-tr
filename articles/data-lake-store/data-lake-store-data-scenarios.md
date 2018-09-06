---
title: Data Lake depolama Gen1 içeren veri senaryoları | Microsoft Docs
description: Hangi veri kullanarak araçları ve farklı senaryoları alınan, işlenen, indirilen ve Data Lake depolama Gen1 (daha önce Azure Data Lake Store da bilinir) olarak görselleştirilir anlama
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 19743dcd2866b8fc7b6ad1fdf387b134f7b3ca50
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783085"
---
# <a name="using-azure-data-lake-storage-gen1-for-big-data-requirements"></a>Azure Data Lake depolama Gen1 büyük veri gereksinimleri için kullanma

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Büyük veri işleme, dört anahtar aşaması vardır:

* Bir veri deposuna toplu veya gerçek zamanlı büyük miktarlarda veri almak
* Veri işleme
* Veri yükleme
* Verileri Görselleştirme

Bu makalede, Azure Data Lake Store, büyük veri ihtiyaçlarınızı karşılamak kullanılabilen araçlar ve Seçenekler anlamak için göre bu aşamasında bakacağız.

## <a name="ingest-data-into-data-lake-store"></a>Data Lake Store ile veri alma
Bu bölüm, farklı veri kaynaklarına ve bu verileri bir Data Lake Store hesabına aktarılabilir farklı yollarını vurgular.

![Data Lake Store alabilen](./media/data-lake-store-data-scenarios/ingest-data.png "verileri Data Lake Store alma")

### <a name="ad-hoc-data"></a>Geçici veri
Bu, daha küçük veri kümeleri temsil eder bir büyük veri uygulaması prototip oluşturma için kullanılır. Veri kaynağı bağlı olarak geçici veri almak için farklı yolu vardır.

| Veri Kaynağı | Kullanarak ayırın |
| --- | --- |
| Yerel bilgisayar |<ul> <li>[Azure Portal](data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure platformlar arası CLI 2.0](data-lake-store-get-started-cli-2.0.md)</li> <li>[Visual Studio için Data Lake Araçları'nı kullanarak](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Azure depolama blobu |<ul> <li>[Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)</li> <li>[AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[HDInsight küme üzerinde çalışan DistCp](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Akış verileri
Bu uygulamalar, cihazlardan, sensörlerden, vb. gibi çeşitli kaynaklar tarafından oluşturulan verileri temsil eder. Bu veriler, çeşitli araçlar tarafından bir Data Lake Store aktarılabilir. Bu araçlar genellikle yakalamak ve bir olay tarafından olay temelinde verileri işleyen gerçek zamanlı olarak ve böylece daha fazla işlenebilir olayları toplu olarak Data Lake Store yazın.

Kullanabileceğiniz araçlar şunlardır:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -olayları Event hubs'a giren yazılabilir için Azure Data Lake kullanarak bir Azure Data Lake Store çıktı.
* [Azure HDInsight Storm](../hdinsight/storm/apache-storm-write-data-lake-store.md) -Storm kümelerini doğrudan Data Lake Store için veri yazabilirsiniz.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – olayları Event Hubs'dan olay alma ve Data Lake Store kullanarak yazma [Data Lake Store .NET SDK'sını](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>İlişkisel veriler
Ayrıca ilişkisel veritabanlarından veri kaynağı. Bir süre, önemli öngörüleri büyük veri işlem hattı işlediğinde sağlayabilen veri çok büyük miktarda ilişkisel veritabanları toplayın. Bu tür verileri Data Lake Store taşımak için aşağıdaki araçları kullanabilirsiniz.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web sunucusu günlüğü verilerini (karşıya yükleme özel uygulamaları kullanarak)
Web sunucusu günlüğü verilerinin analizi büyük veri uygulamaları için yaygın bir kullanım örneği ve günlük dosyaları için Data Lake Store yüklenmek üzere büyük birimleri gerektiriyor çünkü bu tür bir veri kümesi özellikle çağırılır. Betikleri veya bu tür verileri yüklemek için uygulamaları yazmak için aşağıdaki araçlardan herhangi birini kullanabilirsiniz.

* [Azure platformlar arası CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK'sı](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

Web sunucusu günlüğü verilerini karşıya yükleme ve ayrıca başka türden veriler (örneğin, sosyal yaklaşımları veriler) yükleme, bu bileşenin bir parçası olarak karşıya yükleme, verileri içerecek şekilde esnekliği sunar çünkü kendi özel betikler/uygulamaları yazmak için iyi bir yaklaşım olacaktır. daha büyük, büyük veri uygulaması. Bazı durumlarda bu kod bir betik veya basit bir komut satırı yardımcı programını biçiminde. Diğer durumlarda, bir iş uygulaması veya çözüm, büyük veri işleme tümleştirmenize olanak kodu kullanılıyor olabilir.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile ilişkili verileri
Çoğu HDInsight küme türleri (Hadoop, HBase, Storm) Data Lake Store, veri depolama havuzu destekler. HDInsight kümeleri, Azure depolama BLOB'ları (WASB) veri erişim. Daha iyi performans için kümeyle ilişkili bir Data Lake Store hesabına WASB veri kopyalayabilirsiniz. Verileri kopyalamak için aşağıdaki araçları kullanabilirsiniz.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy hizmeti](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Şirket içi veya Iaas Hadoop kümeleri depolanan veri
HDFS kullanarak makinede yerel olarak mevcut Hadoop kümelerindeki büyük miktarlarda verinin depolanabilir. Hadoop kümeleri, bir şirket içi dağıtımda olabilir veya bir Azure Iaas kümesinde içinde olabilir. Bu tür veriler Azure Data Lake Store için tek seferlik bir yaklaşım veya yinelenen bir şekilde kopyalamak için gereksinimleri olabilir. Bunu başarmak için kullanabileceğiniz çeşitli seçenek vardır. Alternatifleri ve ilişkili dezavantajlarına listesi aşağıda verilmiştir.

| Yaklaşım | Ayrıntılar | Avantajları | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| Azure Data Lake Store için doğrudan Hadoop kümelerdeki veri kopyalamak için Azure Data Factory (ADF) kullanın |[ADF HDFS veri kaynağı olarak destekler.](../data-factory/connector-hdfs.md) |ADF HDFS ve birinci sınıf için uçtan uca yönetim ve izleme için kullanıma hazır destek sağlar. |Veri Yönetimi ağ geçidi şirket içinde dağıtılabilir veya Iaas küme gerektirir |
| Verilerin Hadoop'tan dosyaları olarak dışarı aktarın. Ardından, Azure Data Lake uygun mekanizma kullanılarak Store için dosyaları kopyalayın. |Azure Data Lake Store kullanmaya dosyalarını kopyalayabilirsiniz: <ul><li>[Windows işletim sistemi için Azure PowerShell](data-lake-store-get-started-powershell.md)</li><li>[Azure platformlar arası CLI 2.0 için Windows işletim sistemi olmayan](data-lake-store-get-started-cli-2.0.md)</li><li>Her Data Lake Store SDK'sını kullanarak özel uygulama</li></ul> |Başlamak hızlı. Özelleştirilmiş karşıya yükleme yapabilirsiniz |Çok adımlı işlem birden çok teknoloji içerir. Yönetim ve İzleme Araçları'nın özelleştirilmiş yapısı gereği zaman içinde bir mücadele haline şekilde büyür |
| Azure depolama için Hadoop veri kopyalamak için Distcp kullanma. Ardından verileri uygun mekanizmayı kullanarak Data Lake Store için Azure Storage'dan kopyalayacak. |Data Lake Store kullanarak Azure Depolama'ya veri kopyalayabilirsiniz: <ul><li>[Azure Data Factory](../data-factory/copy-activity-overview.md)</li><li>[AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp HDInsight kümelerinde çalıştırma](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Açık kaynak araçları kullanabilirsiniz. |Birden çok teknoloji kapsayan çok adımlı bir işlem |

### <a name="really-large-datasets"></a>Çok büyük veri kümeleri
Aralık birkaç terabayt veri kümelerini karşıya yükleme için yukarıda anlatılan yöntemlerden kullanarak bazen yavaş ve pahalı olabilir. Böyle durumlarda, aşağıdaki seçenekleri kullanabilirsiniz.

* **Azure ExpressRoute kullanarak**. Azure ExpressRoute, şirket içi altyapı ile Azure veri merkezleri arasında özel bağlantılar oluşturmanızı sağlar. Bu, büyük miktarlarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için [Azure ExpressRoute belgeleri](../expressroute/expressroute-introduction.md).
* **"Çevrimdışı", verilerin karşıya**. Azure ExpressRoute kullanarak herhangi bir nedenden dolayı uygun değil ise, kullanabileceğiniz [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) verilerinizi bir Azure veri merkezine sabit disk sürücüleri göndermeye. Verilerinizi Azure depolama BLOB'ları için önce yüklenir. Ardından [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) veya [AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md) Data Lake Store için Azure depolama Bloblarından veri kopyalamak için.

  > [!NOTE]
  > Olsa da Azure veri merkezine gönderin diskleri dosya boyutları içeri/dışarı aktarma hizmetini kullanarak 195 GB'den büyük olmamalıdır.
  >
  >

## <a name="process-data-stored-in-data-lake-store"></a>Data Lake Store içinde depolanan verileri işleme
Verileri Data Lake Store içinde kullanılabilir olduğunda, desteklenen büyük veri uygulamaları kullanarak bu veriler üzerinde analiz çalıştırabilirsiniz. Şu anda, Data Lake Store içinde depolanan veriler üzerinde veri analizi işlerini çalıştırmak için Azure HDInsight ve Azure Data Lake Analytics kullanabilirsiniz.

![Data Lake Store verileri analiz etme](./media/data-lake-store-data-scenarios/analyze-data.png "Data Lake Store verileri analiz etme")

Aşağıdaki örneklere bakabilirsiniz.

* [Bir HDInsight kümesi, depolama alanı olarak Data Lake Store ile oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-store"></a>Verileri Data Lake Store ' indirin
İndirme veya gibi senaryolar için verileri Azure Data Lake Store ' taşımak isteyebilirsiniz:

* Diğer depolarda, mevcut veri işleme komut zincirini ile arabirim oluşturmak için veri taşıyın. Örneğin, Azure SQL veritabanı veya şirket içi SQL Server için Data Lake Store ' veri taşımak isteyebilirsiniz.
* Veri, uygulama prototipleri oluştururken IDE ortamlarından işleme için yerel bilgisayarınıza indirin.

![Çıkış verileri Data Lake Store](./media/data-lake-store-data-scenarios/egress-data.png "çıkış verileri Data Lake Store")

Bu gibi durumlarda, aşağıdaki seçeneklerden herhangi birini kullanabilirsiniz:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Ayrıca, kendi betik/Data Lake Store ' verileri indirmek için uygulamanızı yazmak için aşağıdaki yöntemleri kullanabilirsiniz.

* [Azure platformlar arası CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK'sı](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Data Lake Store verileri görselleştirin
Bir karışımını hizmetler, Data Lake Store içinde depolanan verilerin görsel bir temsilini oluşturmak için kullanabilirsiniz.

![Data Lake Store verileri görselleştirin](./media/data-lake-store-data-scenarios/visualize-data.png "Data Lake Store verileri görselleştirin")

* Kullanarak başlatabilirsiniz [Azure Data Factory, verileri Data Lake Store ' Azure SQL veri ambarı'na taşımak için](../data-factory/copy-activity-overview.md)
* Bundan sonra yapabilecekleriniz [Power BI ile Azure SQL veri ambarı tümleştirme](../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) verilerin görsel bir temsilini oluşturmak için.
