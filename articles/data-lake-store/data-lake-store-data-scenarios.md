---
title: "Data Lake Store içeren veri senaryoları | Microsoft Docs"
description: "Farklı senaryolar ve hangi veri kullanarak araçları alınan, işlenen, yüklenen ve bir Data Lake Store'da görselleştirilen anlama"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 37409a71-a563-4bb7-bc46-2cbd426a2ece
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 6428c6d9fcb577f18221ee48a61456c460bd8176
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Azure Data Lake Store için büyük veri gereksinimleri kullanma
Büyük veri işleme, dört anahtar aşamaları şunlardır:

* Bir veri deposuna gerçek zamanlı veya toplu büyük miktarlarda veri alma
* Veri işleme
* Veri indirme
* Verileri Görselleştirme

Bu makalede, bu aşamalar seçenekleri ve büyük veri gereksinimlerinizi karşılamak için kullanılabilen araçlar anlamak için Azure Data Lake Store göre ele.

## <a name="ingest-data-into-data-lake-store"></a>Data Lake Store veri alma
Bu bölüm, farklı kaynaklardan veri ve hangi verilerin Data Lake Store dikkate alınan farklı yolları vurgular.

![Data Lake Store veri alma](./media/data-lake-store-data-scenarios/ingest-data.png "Data Lake Store veri alma")

### <a name="ad-hoc-data"></a>Geçici verileri
Bu, daha küçük veri kümeleri temsil eden bir büyük veri uygulaması için prototipi oluşturulurken kullanılan. Geçici veri alma veri kaynağını bağlı olarak farklı yolu vardır.

| Veri Kaynağı | Kullanarak alma |
| --- | --- |
| Yerel bilgisayar |<ul> <li>[Azure Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure platformlar arası CLI 2.0](data-lake-store-get-started-cli-2.0.md)</li> <li>[Visual Studio için Data Lake Araçları'nı kullanarak](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Azure depolama blobunu |<ul> <li>[Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)</li> <li>[AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Hdınsight küme üzerinde çalışan Distcp'yi](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Akışa
Bu uygulamalar, cihazlar, algılayıcılar, vb. gibi çeşitli kaynaklardan tarafından oluşturulan verileri temsil eder. Bu verileri bir Data Lake Store çeşitli araçları tarafından alınan. Bu araçlar genellikle yakalamak ve verileri bir olay olayı olarak işlem gerçek zamanlı ve böylece daha fazla işlenebilir olayları toplu olarak Data Lake Store yazın.

Kullanabileceğiniz araçlar şunlardır:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -Event Hubs'a alınan olayları yazılabilir Azure Data Lake Azure Data Lake Store çıkış kullanan.
* [Azure Hdınsight Storm](../hdinsight/storm/apache-storm-write-data-lake-store.md) -Storm kümeden doğrudan Data Lake Store'a veri yazabilirsiniz.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – olayları olay hub'larından almak ve Data Lake Store kullanmaya yazma [Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>İlişkisel veri
Ayrıca, ilişkisel veritabanlarından veri kaynağı. Bir süre boyunca, büyük miktarlarda verinin büyük veri kanalı yoluyla işlenen anahtar Öngörüler sağlayabilen ilişkisel veritabanları toplayın. Data Lake Store içinde bu tür veri taşımak için aşağıdaki araçları kullanabilirsiniz.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web sunucu günlüğü verileri (karşıya yükleme kullanarak özel uygulamalar)
Web sunucu günlüğü verileri analizini büyük veri uygulamaları için ortak bir kullanım örneği olduğundan ve günlük dosyaları için Data Lake Store karşıya yüklenecek büyük miktarlarda gerektirir bu dataset türü özellikle çağırılır. Kendi komut dosyaları veya bu tür veriler karşıya yüklemek için uygulamaları yazmak için aşağıdaki araçlardan birini kullanabilirsiniz.

* [Azure platformlar arası CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK'sı](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

Web sunucu günlüğü verileri karşıya yükleme için ve ayrıca verileri (örneğin sosyal düşüncelerin) diğer tür karşıya yükleme için bileşen büyük büyük veri uygulamanız bir parçası olarak karşıya yükleme, verileri içerecek şekilde esneklik verdiğinden, kendi özel komut dosyaları/uygulamaları yazmak için iyi bir yaklaşımdır. Bazı durumlarda bu kodu bir komut dosyası veya basit komut satırı yardımcı programı şeklinde. Diğer durumlarda, kodu bir iş uygulaması veya çözümü içine büyük veri işleme tümleştirmek için kullanılabilir.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure Hdınsight kümeleri ile ilgili veriler
Çoğu Hdınsight küme türleri (Hadoop, HBase, Storm) Data Lake Store veri depolama depo olarak destekler. Hdınsight kümeleri, Azure Storage Blobları (WASB) veri erişim. Daha iyi performans için verileri WASB kümeyle ilişkili bir Data Lake Store hesabı kopyalayabilirsiniz. Verileri kopyalamak için aşağıdaki araçları kullanabilirsiniz.

* [Apache Distcp'yi](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy hizmeti](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Şirket içi veya Iaas Hadoop kümeleri veriler
Büyük miktarlarda verinin yerel olarak HDFS kullanarak makinelerde varolan Hadoop kümeleri depolanabilir. Hadoop kümeleri bir şirket içi dağıtımda olabilir veya Azure Iaas kümede içinde olabilir. Bu tür veriler Azure Data Lake Store için tek seferlik bir yaklaşım için yinelenen bir şekilde mi kopyalanacağını gereksinimleri olabilir. Bunu başarmak için kullanabileceğiniz çeşitli seçenekler vardır. Alternatifleri ve ilişkili dengelemeler listesi aşağıdadır.

| Yaklaşım | Ayrıntılar | Avantajları | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| Azure veri fabrikası (ADF) doğrudan Hadoop kümelerdeki Azure Data Lake Store'a veri kopyalamak için kullanın |[ADF HDFS bir veri kaynağı olarak destekler.](../data-factory/connector-hdfs.md) |ADF HDFS ve ilk sınıfı uçtan uca yönetim ve izleme Giden kutusu desteği sağlar |Veri Yönetimi ağ geçidi dağıtılan şirket içinde veya Iaas küme gerektirir |
| Verilerin Hadoop'tan dosyaları olarak dışarı aktarın. Ardından Azure Data Lake uygun mekanizma kullanılarak Store'a dosyalarını kopyalayın. |Azure Data Lake Store kullanarak dosyaları kopyalayabilirsiniz: <ul><li>[Windows işletim sistemi için Azure PowerShell](data-lake-store-get-started-powershell.md)</li><li>[Azure platformlar arası CLI 2.0 Windows olmayan işletim için](data-lake-store-get-started-cli-2.0.md)</li><li>Tüm Data Lake Store SDK kullanarak özel uygulama</li></ul> |Hızlı başlamak. Özelleştirilmiş yüklemeleri yapabilirsiniz |Birden çok teknoloji içerir çok adımlı işlemi. Yönetim ve izleme araçları özelleştirilmiş yapısını verilen zaman içinde bir sınama olmasını büyüyecektir |
| Verileri Azure depolama birimine Hadoop'tan kopyalamak için Distcp'yi kullanın. Ardından verileri Azure depolama biriminden uygun mekanizmayı kullanarak Data Lake Store için kopyalayabilirsiniz. |Data Lake Store kullanarak verileri Azure depolama biriminden kopyalayabilirsiniz: <ul><li>[Azure Data Factory](../data-factory/copy-activity-overview.md)</li><li>[AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache Hdınsight kümelerinde çalışan Distcp'yi](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Açık kaynaklı araçları kullanabilirsiniz. |Birden çok teknoloji içerir çok adımlı işlemi |

### <a name="really-large-datasets"></a>Gerçekten büyük veri kümeleri
Birkaç terabayta aralığı veri kümelerini yüklemek için yukarıda açıklanan yöntemlerle bazen yavaş ve pahalı olabilir. Böyle durumlarda, aşağıdaki seçenekleri kullanabilirsiniz.

* **Azure ExpressRoute kullanarak**. Azure ExpressRoute, şirket içinde Azure veri merkezleri ile altyapı arasında özel bağlantılar oluşturmanızı sağlar. Bu, büyük miktarlarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için bkz: [Azure ExpressRoute belgeleri](../expressroute/expressroute-introduction.md).
* **"Çevrimdışı", verilerin karşıya**. Azure ExpressRoute kullanarak herhangi bir nedenle uygun değilse, kullanabileceğiniz [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md) verilerinizi bir Azure veri merkezi için sabit disk sürücülerini dağıtmayı. Verilerinizi ilk Azure Storage Bloblarında yüklenir. Daha sonra [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) veya [AdlCopy aracı](data-lake-store-copy-data-azure-storage-blob.md) Azure Storage Bloblarından Data Lake Store'a veri kopyalamak için.

  > [!NOTE]
  > İçeri/dışarı aktarma hizmeti, Azure veri merkezine sevk disklerde dosya boyutları kullanarak 195 GB'den büyük olmamalıdır yapılırken.
  >
  >

## <a name="process-data-stored-in-data-lake-store"></a>Data Lake Store içinde depolanan verileri işleme
Data Lake Store'da verileri kullanılabilir hale geldiğinde desteklenen büyük veri uygulamaları kullanarak bu veriler üzerinde analiz çalıştırabilirsiniz. Şu anda, Data Lake Store içinde depolanan veriler üzerinde verileri analiz işlerini çalıştırmak için Azure Hdınsight ve Azure Data Lake Analytics kullanabilirsiniz.

![Data Lake Store'da verileri çözümlemek](./media/data-lake-store-data-scenarios/analyze-data.png "Data Lake Store'da verileri analiz etme")

Aşağıdaki örnekler bakabilirsiniz.

* [Hdınsight kümesi Data Lake Store ile depolama alanı olarak oluşturun.](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-store"></a>Data Lake Deposu'ndan veri veri indirin
İndirme veya gibi senaryolar için veri Azure Data Lake Deposu'ndan veri taşımak isteyebilirsiniz:

* Mevcut veri işlem hatlarınızı ile arabirim oluşturmak için diğer depoları veri taşıyın. Örneğin, verileri Azure SQL Database veya şirket içi SQL Server için Data Lake Deposu'ndan veri taşımak isteyebilirsiniz.
* Veri işleme uygulama prototipleri oluştururken IDE ortamlarda yerel bilgisayarınıza indirin.

![Çıkış Data Lake Store verilerden](./media/data-lake-store-data-scenarios/egress-data.png "çıkış Data Lake Store verileri")

Böyle durumlarda, aşağıdaki seçeneklerden birini kullanabilirsiniz:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)
* [Apache Distcp'yi](data-lake-store-copy-data-wasb-distcp.md)

Kendi komut dosyası/Data Lake Deposu'ndan veri veri indirmek için uygulama yazmak için aşağıdaki yöntemleri de kullanabilirsiniz.

* [Azure platformlar arası CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK'sı](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Data Lake Store'da verileri görselleştirin
Data Lake Store içinde depolanan verileri görsel gösterimi oluşturmak için bir karışımını Hizmetleri kullanabilirsiniz.

![Data Lake Store'da verileri görselleştirin](./media/data-lake-store-data-scenarios/visualize-data.png "Data Lake Store'da verileri görselleştirin")

* Kullanarak başlatabilirsiniz [verileri Azure SQL Data Warehouse için Data Lake Deposu'ndan veri taşımak için Azure veri fabrikası](../data-factory/copy-activity-overview.md)
* Bundan sonra şunları yapabilirsiniz [Power BI ile Azure SQL veri ambarı tümleştirmek](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) verileri görsel temsilini oluşturmak için.
