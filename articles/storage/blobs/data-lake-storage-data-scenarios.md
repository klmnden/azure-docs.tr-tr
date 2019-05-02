---
title: Azure Data Lake depolama Gen2 içeren veri senaryoları | Microsoft Docs
description: Hangi veri kullanarak araçları ve farklı senaryoları alınan, işlenen, indirilen ve Data Lake depolama Gen2'ye (daha önce Azure Data Lake Store da bilinir) olarak görselleştirilir anlama
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: normesta
ms.openlocfilehash: c5b6287757f6b71cfd60687f463673f142db04d9
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939304"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Azure Data Lake depolama Gen2 büyük veri gereksinimleri için kullanma

Büyük veri işleme, dört anahtar aşaması vardır:

> [!div class="checklist"]
> * Bir veri deposuna toplu veya gerçek zamanlı büyük miktarlarda veri almak
> * Veri işleme
> * Veri yükleme
> * Verileri Görselleştirme

Bir depolama hesabı ve bir dosya sistemi oluşturarak başlayın. Ardından, verilere erişim verin. Bu makalenin ilk birkaç bölümleri, bu görevleri gerçekleştirmenize yardımcı olur. Geriye kalan bölümlerinde, size her işleme aşama için seçenekleri ve araçları çekeceğiz.

## <a name="create-a-data-lake-storage-gen2-account"></a>Bir Data Lake depolama Gen2 hesabı oluşturun

Hiyerarşik ad alanı olan bir depolama hesabı bir Data Lake depolama Gen2 hesabıdır. 

Oluşturmak için bkz: [hızlı başlangıç: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="create-a-file-system"></a>Bir dosya sistemi oluşturun

A *dosya sistemi* klasörler ve dosyalar için bir kapsayıcıdır. En az bir tanesi depolama hesabınızdaki veri alma başlamak için ihtiyacınız.  Bunları oluşturmak için kullanabileceğiniz araçlar listesi aşağıda verilmiştir.

|Tool | Rehber |
|---|--|
|Azure Depolama Gezgini | [Depolama Gezgini'ni kullanarak bir dosya sistemi oluşturun](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-explorer#create-a-filesystem) |
|AzCopy | [AzCopyV10 kullanarak dosya paylaşımını veya bir Blob kapsayıcısı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-blob-container-or-file-share)|
|HDInsight ile Hadoop dosya sistemi (HDFS) komut satırı arabirimi (CLI) |[HDInsight ile HDFS kullanarak bir dosya sistemi oluşturun](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-hdfs-data-lake-storage?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-file-system) |
|Azure Databricks not defteri kodda|[Bir depolama hesabı dosya sistemi (Scala) oluşturun](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-storage-account-file-system) <br><br> [Bir dosya sistemi oluşturun ve bunu (Python) bağlama](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-databricks-spark?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-file-system-and-mount-it)|

Depolama Gezgini veya AzCopy kullanarak dosya sistemleri oluşturmak daha kolaydır. Bu, HDInsight ve Databricks kullanarak dosya sistemleri oluşturmak için biraz daha fazla iş götürür. Ancak, HDInsight kullanmayı planlıyorsanız veya verilerinizi yine de işlemek için Databricks kümeleri, ardından, kümelerinizi ilk oluşturabilir ve oluşturma dosya sistemlerine HDFS CLI'yı kullanma.  

## <a name="grant-access-to-the-data"></a>Verilere erişim izni ver

Veri alma başlamadan önce hesabınızı ve hesabınızdaki verilere uygun erişim izinlerini ayarlayın.

Erişim için üç yol vardır:

* Bu rollerden biri, bir kullanıcı, Grup, kullanıcı tarafından yönetilen kimlik veya hizmet sorumlusu atayın:

  [Depolama Blob verileri okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)

  [Depolama Blob verileri katkıda bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)

  [Depolama Blob verileri sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner-preview)

* Paylaşılan erişim imzası (SAS) belirteci kullanın.

* Bir depolama hesabı anahtarını kullanın.

Bu tablo her bir Azure hizmeti veya aracı için erişimi nasıl gösterir.

|Tool | Erişim vermek için | Rehber |
|---|--|---|
|Depolama Gezgini| Kullanıcılar ve gruplar için rol atama | [Azure Active Directory ile kullanıcılara yönetici ve yönetici olmayan rol atayın](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal) |
|AzCopy| Kullanıcılar ve gruplar için rol atama <br>**veya**<br> Bir SAS belirteci kullanabilir| [Azure Active Directory ile kullanıcılara yönetici ve yönetici olmayan rol atayın](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal)<br><br>[Kolayca Azure depolama – kullanarak bir dosyayı indirmek için SAS oluşturma Azure Depolama Gezgini](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/)|
|Apache DistCp | Kullanıcı tarafından atanan bir yönetilen kimlik rol atama | [Data Lake depolama Gen2'ile bir HDInsight kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Data Factory| Rol atamak için kullanıcı tarafından atanan-yönetilen bir kimlik<br>**veya**<br> Bir hizmet sorumlusuna bir rol atanıyor<br>**veya**<br> Bir depolama hesabı anahtarını kullanın | [Bağlı hizmeti özellikleri](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage#linked-service-properties) |
|Azure HDInsight| Kullanıcı tarafından atanan bir yönetilen kimlik rol atama | [Data Lake depolama Gen2'ile bir HDInsight kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)|
|Azure Databricks| Bir hizmet sorumlusuna bir rol atayın | [Nasıl yapılır: Bir Azure AD uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)|

Belirli dosya ve klasörlere erişim izni vermek için şu makalelere bakın.

* [Azure Data Lake depolama 2. nesil ile Azure Depolama Gezgini'ni kullanarak dosya ve dizin düzeyi izinleri ayarlayın](https://review.docs.microsoft.com/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

* [Erişim denetim listelerini dosyalar ve dizinler](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control#access-control-lists-on-files-and-directories)

Güvenlik diğer yönleri hakkında bilgi edinmek için bkz. [Azure Data Lake depolama Gen2 Güvenlik Kılavuzu](https://review.docs.microsoft.com/azure/storage/common/storage-data-lake-storage-security-guide?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="ingest-the-data"></a>Veri alma

Bu bölüm, farklı veri kaynaklarına ve bu verileri bir Data Lake depolama Gen2 hesaba aktarılabilir farklı yollarını vurgular.

![Data Lake depolama Gen2 alabilen](./media/data-lake-storage-data-scenarios/ingest-data.png "verileri Data Lake depolama Gen2 alma")

### <a name="ad-hoc-data"></a>Geçici veri

Bu, daha küçük veri kümeleri temsil eder bir büyük veri uygulaması prototip oluşturma için kullanılır. Veri kaynağı bağlı olarak geçici veri almak için farklı yolu vardır. 

Geçici veri almak için kullanabileceğiniz araçlar listesi aşağıda verilmiştir.

| Veri Kaynağı | Kullanarak ayırın |
| --- | --- |
| Yerel bilgisayar |[Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)<br><br>[AzCopy aracı](../common/storage-use-azcopy-v10.md)|
| Azure depolama blobu |[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)<br><br>[AzCopy aracı](../common/storage-use-azcopy-v10.md)<br><br>[HDInsight küme üzerinde çalışan DistCp](data-lake-storage-use-distcp.md)|

### <a name="streamed-data"></a>Akış verileri

Bu uygulamalar, cihazlardan, sensörlerden, vb. gibi çeşitli kaynaklar tarafından oluşturulan verileri temsil eder. Bu veriler, çeşitli araçlar tarafından Data Lake Sorage Gen2 aktarılabilir. Bu araçlar genellikle yakalamak ve bir olay tarafından olay temelinde verileri işleyen gerçek zamanlı olarak ve böylece daha fazla işlenebilir olayları toplu olarak Data Lake depolama Gen2 yazın.

Akış verilerini almak için kullanabileceğiniz araçlar listesi aşağıda verilmiştir.

|Tool | Rehber |
|---|--|
|Azure HDInsight Storm | [Apache Hadoop HDFS'ye HDInsight üzerinde Apache Storm yazma](https://docs.microsoft.com/azure/hdinsight/storm/apache-storm-write-data-lake-store) |

### <a name="relational-data"></a>İlişkisel veriler

Ayrıca ilişkisel veritabanlarından veri kaynağı. Bir süre, önemli öngörüleri büyük veri işlem hattı işlediğinde sağlayabilen veri çok büyük miktarda ilişkisel veritabanları toplayın. Bu tür verileri Data Lake depolama Gen2 taşımak için aşağıdaki araçları kullanabilirsiniz.

İlişkisel verileri almak için kullanabileceğiniz araçlar listesi aşağıda verilmiştir.

|Tool | Rehber |
|---|--|
|Azure Data Factory | [Azure Data Factory’de Kopyalama Etkinliği](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web sunucusu günlüğü verilerini (karşıya yükleme özel uygulamaları kullanarak)

Web sunucusu günlüğü verilerinin analizi büyük veri uygulamaları için yaygın bir kullanım örneği ve günlük dosyaları için Data Lake depolama Gen2 yüklenmek üzere büyük birimleri gerektiriyor çünkü bu tür bir veri kümesi özellikle çağırılır. Betikleri veya bu tür verileri yüklemek için uygulamaları yazmak için aşağıdaki araçlardan herhangi birini kullanabilirsiniz.

Web sunucusu günlüğü verileri almak için kullanabileceğiniz araçlar listesi aşağıda verilmiştir.

|Tool | Rehber |
|---|--|
|Azure Data Factory | [Azure Data Factory’de Kopyalama Etkinliği](https://docs.microsoft.com/azure/data-factory/copy-activity-overview)  |

Web sunucusu günlüğü verilerini karşıya yükleme ve ayrıca başka türden veriler (örneğin, sosyal yaklaşımları veriler) yükleme, bu bileşenin bir parçası olarak karşıya yükleme, verileri içerecek şekilde esnekliği sunar çünkü kendi özel betikler/uygulamaları yazmak için iyi bir yaklaşım olacaktır. daha büyük, büyük veri uygulaması. Bazı durumlarda bu kod bir betik veya basit bir komut satırı yardımcı programını biçiminde. Diğer durumlarda, bir iş uygulaması veya çözüm, büyük veri işleme tümleştirmenize olanak kodu kullanılıyor olabilir.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile ilişkili verileri

Çoğu HDInsight küme türleri (Hadoop, HBase, Storm), bir veri depolama deposu olarak Data Lake depolama Gen2 destekler. HDInsight kümeleri, Azure depolama BLOB'ları (WASB) veri erişim. Daha iyi performans için kümeyle ilişkili bir Data Lake depolama Gen2 hesabına WASB veri kopyalayabilirsiniz. Verileri kopyalamak için aşağıdaki araçları kullanabilirsiniz.

HDInsight kümeleri ile ilişkili veri almak için kullanabileceğiniz araçlar listesi aşağıda verilmiştir.

|Tool | Rehber |
|---|--|
|Apache DistCp | [Azure depolama BLOB'ları ile Azure Data Lake depolama Gen2 arasında veri kopyalamak için DistCp kullanma](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|AzCopy aracı | [AzCopy ile veri aktarma](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10) |
|Azure Data Factory | [Azure Data Factory kullanarak verileri için veya Azure Data Lake depolama Gen2'ye kopyalayın](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2) |

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Şirket içi veya Iaas Hadoop kümeleri depolanan veri

HDFS kullanarak makinede yerel olarak mevcut Hadoop kümelerindeki büyük miktarlarda verinin depolanabilir. Hadoop kümeleri, bir şirket içi dağıtımda olabilir veya bir Azure Iaas kümesinde içinde olabilir. Bu tür veriler Azure Data Lake depolama 2. nesil için tek seferlik bir yaklaşım veya yinelenen bir şekilde kopyalamak için gereksinimleri olabilir. Bunu başarmak için kullanabileceğiniz çeşitli seçenek vardır. Alternatifleri ve ilişkili dezavantajlarına listesi aşağıda verilmiştir.

| Yaklaşım | Ayrıntılar | Yararları | Dikkat edilmesi gerekenler |
| --- | --- | --- | --- |
| Azure Data Lake depolama Gen2 doğrudan Hadoop kümelerdeki veri kopyalamak için Azure Data Factory (ADF) kullanın |[ADF HDFS veri kaynağı olarak destekler.](../../data-factory/connector-hdfs.md) |ADF HDFS ve birinci sınıf için uçtan uca yönetim ve izleme için kullanıma hazır destek sağlar. |Veri Yönetimi ağ geçidi şirket içinde dağıtılabilir veya Iaas küme gerektirir |
| Azure depolama için Hadoop veri kopyalamak için Distcp kullanma. Ardından verileri Azure uygun mekanizma kullanılarak depolamadan Data Lake depolama Gen2'ye kopyalayın. |Data Lake depolama Gen2 kullanarak Azure Depolama'dan veri kopyalayabilirsiniz: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[AzCopy aracı](../common/storage-use-azcopy-v10.md)</li><li>[Apache DistCp HDInsight kümelerinde çalıştırma](data-lake-storage-use-distcp.md)</li></ul> |Açık kaynak araçları kullanabilirsiniz. |Birden çok teknoloji kapsayan çok adımlı bir işlem |

### <a name="really-large-datasets"></a>Çok büyük veri kümeleri

Aralık birkaç terabayt veri kümelerini karşıya yükleme için yukarıda anlatılan yöntemlerden kullanarak bazen yavaş ve pahalı olabilir. Böyle durumlarda, Azure ExpressRoute kullanabilirsiniz.  

Azure ExpressRoute, şirket içi Azure veri merkezleri ve altyapısı arasında özel bağlantılar oluşturmanızı sağlar. Bu, büyük miktarlarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için bkz. [Azure ExpressRoute belgeleri](../../expressroute/expressroute-introduction.md).

## <a name="process-the-data"></a>İşlem verileri

Verileri Data Lake depolama 2. nesil'deki kullanılabilir olduğunda, desteklenen büyük veri uygulamaları kullanarak bu veriler üzerinde analiz çalıştırabilirsiniz. 

![Data Lake depolama Gen2 verileri analiz etme](./media/data-lake-storage-data-scenarios/analyze-data.png "Data Lake depolama Gen2 verileri analiz etme")

Data Lake depolama 2. nesil'deki depolanan veriler üzerinde veri analizi işlerini çalıştırmak için kullanabileceğiniz araçlar listesi aşağıda verilmiştir.

|Tool | Rehber |
|---|--|
|Azure HDInsight | [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Databricks | [Azure Data Lake depolama 2. nesil](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)<br><br>[Hızlı Başlangıç: Azure Databricks kullanarak Azure Data Lake depolama Gen2 verileri çözümleme](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br><br>[Öğretici: Ayıklama, dönüştürme ve Azure Databricks kullanarak verileri yüklemek](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="visualize-the-data"></a>Verileri görselleştirme

Bir karışımını hizmetler, Data Lake depolama 2. nesil'deki depolanan verilerin görsel bir temsilini oluşturmak için kullanabilirsiniz.

![Data Lake depolama Gen2 verileri görselleştirin](./media/data-lake-storage-data-scenarios/visualize-data.png "Data Lake depolama Gen2 verileri görselleştirin")

* Kullanarak başlatabilirsiniz [Azure Data Factory, verileri Azure SQL veri ambarı'na Data Lake depolama Gen2'ye taşımak için](../../data-factory/copy-activity-overview.md)
* Bundan sonra yapabilecekleriniz [Power BI ile Azure SQL veri ambarı tümleştirme](../../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) verilerin görsel bir temsilini oluşturmak için.

## <a name="download-the-data"></a>Verileri yükle

İndirme veya gibi senaryolar için Azure Data Lake depolama Gen2 ' veri taşımak isteyebilirsiniz:

* Diğer depolarda, mevcut veri işleme komut zincirini ile arabirim oluşturmak için veri taşıyın. Örneğin, Azure SQL veritabanı veya şirket içi SQL Server için Data Lake depolama Gen2 ' veri taşımak isteyebilirsiniz.

* Veri, uygulama prototipleri oluştururken IDE ortamlarından işleme için yerel bilgisayarınıza indirin.

![Çıkış verileri Data Lake depolama Gen2](./media/data-lake-storage-data-scenarios/egress-data.png "çıkış verileri Data Lake depolama 2. nesil")

Data Lake depolama Gen2 ' verileri indirmek için kullanabileceğiniz araçlar listesi aşağıda verilmiştir.

|Tool | Rehber |
|---|--|
|Azure Data Factory | [Azure Data Factory’de Kopyalama Etkinliği](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |
|Apache DistCop | [Azure depolama BLOB'ları ile Azure Data Lake depolama Gen2 arasında veri kopyalamak için DistCp kullanma](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
