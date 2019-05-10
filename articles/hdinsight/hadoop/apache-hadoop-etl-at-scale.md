---
title: Ayıklama, dönüştürme ve yükleme (ETL) uygun ölçekte - Azure HDInsight
description: ETL HDInsight Apache Hadoop ile nasıl kullanıldığını öğrenin.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: a343caaa998505a1772096b058ec7ad300eec03c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64725705"
---
# <a name="extract-transform-and-load-etl-at-scale"></a>Ayıklama, dönüştürme ve yükleme (ETL) uygun ölçekte

Ayıklama, dönüştürme ve yükleme (ETL), veriler çeşitli kaynaklardan alınan, standart bir konumda toplanır, temizlenen ve işlenen ve sonuçta da sorgulanabilir bir veri deposu yüklenen işlem olduğu. Eski ETL işlemleri verileri içeri aktarmak, yerinde temizlemek ve ardından bir ilişkisel veri altyapısı saklayın. HDInsight ile çok çeşitli Apache Hadoop ekosistemi bileşenlerini destekleyen uygun ölçekte ETL gerçekleştirme. 

Bu işlem hattı tarafından HDInsight ETL işlemi kullanımını özetlenebilir:

![HDInsight ETL genel bakış](./media/apache-hadoop-etl-at-scale/hdinsight-etl-at-scale-overview.png)

Aşağıdaki bölümlerde her bir ETL aşamaları ve ilişkili bileşenleri keşfedin.

## <a name="orchestration"></a>Düzenleme

Orchestration ETL işlem hattının tüm aşamalarda yayılır. HDInsight ETL işleri genellikle birbirleri ile birlikte çalışan birkaç farklı ürünleri içerir.  Hive, Pig, başka bir bölümü temizler sırada veri kısmı temizlemek için kullanabilirsiniz.  Azure Data Factory, verileri Azure SQL veritabanı'ndan Azure Data Lake Store yüklemek için kullanabilirsiniz.

Orchestration uygun zamanda uygun işi çalıştırmak için gereklidir.

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir. Oozie bir HDInsight kümesi içinde çalışan ve Hadoop yığını ile tümleştirilir. Oozie, Apache Hadoop MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işlerini destekler. Oozie Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanılabilir.

Daha fazla bilgi için [tanımlamak ve bir iş akışını HDInsight üzerinde çalıştırmak için Apache Hadoop ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md) Oozie bir uçtan uca işlem hattı sürücü için nasıl kullanılacağını gösteren derinlemesine bir bakış için bkz [veri işlem hattıkullanımahazırhalegetirme](../hdinsight-operationalize-data-pipeline.md). 

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory, hizmet olarak platform şeklinde düzenleme özellikleri sağlar. Bu veri taşıma ve veri dönüştürmeyi düzenleyip otomatikleştirmek için veri odaklı iş akışları oluşturmanıza olanak tanıyan bir bulut tabanlı veri tümleştirme hizmetidir. 

Azure Data Factory kullanarak şunları yapabilirsiniz:

1. Oluşturabilir ve farklı veri depolarından veri alabilen (işlem hatları olarak adlandırılır) veri odaklı iş akışları zamanlayabilirsiniz.
2. İşlem ve kullanarak verileri Dönüştürme Hizmetleri Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics, Azure Batch ve Azure Machine Learning gibi işlem.
3. Çıktı verilerini iş zekası (BI) uygulamalarının kullanması için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilirsiniz.

Azure Data Factory hakkında daha fazla bilgi için bkz. [belgeleri](../../data-factory/introduction.md).

## <a name="ingest-file-storage-and-result-storage"></a>Dosya depolama ve sonuç deposu alma

Kaynak veri dosyaları, genellikle Azure depolama veya Azure Data Lake Storage bir konuma yüklenir. Dosyaları herhangi bir biçimde olabilir, ancak genellikle bunlar Csv'leri gibi düz dosyalar. 

### <a name="azure-storage"></a>Azure Storage 

[Azure depolama](https://azure.microsoft.com/services/storage/blobs/) sahip [belirli ölçeklenebilirlik hedefleri](../../storage/common/storage-scalability-targets.md).  En analitik düğümleri için Azure depolama ile çok daha küçük dosyalar ilgilenirken en iyi şekilde ölçeklendirir.  Azure depolama, kaç dosyanın ya da (sınırlarınızı olduğunuz sürece) ne kadar büyük dosyalar ne olursa olsun aynı performans garanti eder.  Veri ya da tüm verilerin bir alt kullanıp kullanmadığınızı bu terabaytlarca veri depolayabilir ve tutarlı bir performans almaya anlamına gelir.

Azure depolama blobları birkaç farklı türde sahiptir.  Bir *ekleme blobu* web günlükleri veya sensör verilerini depolamak için harika bir seçenektir.  

Birden çok BLOB'ları, onlara yönelik erişimi ölçeklendirmek için birçok sunucuya dağıtılabilir, ancak tek bir blob yalnızca tek bir sunucu tarafından sunulabilen. Blob kapsayıcıları blobları mantıksal gruplandırılabilir olsa da bu gruplandırma bölümleme hiçbir etkileri vardır.

Azure depolama, blob depolama için WebHDFS API'sini katman de vardır.  HDInsight tüm hizmetler, veri temizleme ve Hadoop dağıtılmış dosya sistemi (HDFS) bu hizmetleri nasıl kullanacağınız benzer şekilde, veri işleme, Azure Blob storage'daki dosyalara erişebilirsiniz.

Veriler, PowerShell, Azure depolama SDK'sı veya AZCopy kullanarak Azure Depolama'ya genellikle alınır.

### <a name="azure-data-lake-storage"></a>Azure Data Lake Storage

Azure Data Lake Storage (ADLS) ile HDFS uyumlu analiz verileri için bir yönetilen, çok büyük ölçekte depo ' dir.  ADLS HDFS'ye benzer ve sınırsız ölçeklenebilirlik açısından toplam kapasite ve tek tek dosyaların boyutu sunan bir tasarım paradigma kullanır. ADLS birden fazla düğümde büyük bir dosya depolanabilir beri büyük dosyalarla çalışırken çok iyidir.  ADLS verileri bölümleme arka planda gerçekleştirilir.  Yüzlerce terabayt boyutunda veriyi verimli bir şekilde okuyan ve yazan eşzamanlı binlerce yürütücü sayesinde, analiz işlerini çok yüksek aktarım hızlarıyla çalıştırabilirsiniz.

Veriler, Azure Data Factory, ADLS SDK'ları, AdlCopy hizmeti, Apache DistCp veya Apache Sqoop kullanarak ADSL'ye genellikle alınır.  Hangi büyük ölçüde kullanmak için bu hizmetlerin, veri nerede olduğuna bağlıdır.  Veriler şu anda var olan bir Hadoop kümesi ise, Apache DistCp, AdlCopy Service veya Azure Data Factory kullanabilirsiniz.  Azure Blob Depolama alanında ise, Azure Data Lake depolama .NET SDK, Azure PowerShell veya Azure Data Factory kullanabilirsiniz.

ADLS, ayrıca Azure olay hub'ı veya Apache Storm kullanarak olay alma işlemi için optimize edilmiştir.

#### <a name="considerations-for-both-storage-options"></a>Her iki depolama seçenekleri için dikkat edilmesi gerekenler

Özellikle, verileri bir şirket içi konumundan geliyorsa terabayt aralıktaki veri kümelerini karşıya yükleme için ağ gecikmesi büyük bir sorun olabilir.  Bu gibi durumlarda, aşağıdaki seçenekleri kullanabilirsiniz:

* Azure ExpressRoute:  Azure ExpressRoute, Azure veri merkezleri ile şirket içi altyapınız arasında özel bağlantılar oluşturmanızı sağlar. Bu bağlantılar, büyük miktarlarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için [Azure ExpressRoute belgeleri](../../expressroute/expressroute-introduction.md).

* "Çevrimdışı" verilerini karşıya yükleyin. Kullanabileceğiniz [Azure içeri/dışarı aktarma hizmeti](../../storage/common/storage-import-export-service.md) verilerinizi bir Azure veri merkezine sabit disk sürücüleri göndermeye. Verilerinizi Azure depolama BLOB'ları için önce yüklenir. Ardından [Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md) veya [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md) Data Lake Storage için Azure depolama bloblarından veri kopyalamak için aracı.

### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

Azure SQL DW depolamak için harika bir seçim temizlenir ve sonuçları gelecekteki analizler için hazır olduğunu.  Azure HDInsight, bu hizmetler için Azure SQL DW gerçekleştirmek için kullanılabilir.

Azure SQL veri ambarı (SQL DW) analitik iş yükleri için iyileştirilmiş bir ilişkisel veritabanı deposudur.  Azure SQL DW bölümlenmiş tabloları göre ölçeklendirir.  Tablolar, birden fazla düğümde bölümlenebilir.  Azure SQL DW düğümleri oluşturma sırasında seçilir.  Bunlar olaydan sonra ölçeklendirebilirsiniz, ancak, veri taşıma gerektirebilecek, etkin bir işlemdir. Bkz: [SQL veri ambarı - yönetme işlem](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md) daha fazla bilgi için.

### <a name="apache-hbase"></a>Apache HBase

Apache HBase Azure HDInsight kullanılabilir bir anahtar-değer deposudur.  Apache HBase, Hadoop’ta oluşturulan ve Google BigTable’a göre modellenen açık kaynaklı bir NoSQL veritabanıdır. HBase, yüksek performanslı rastgele erişim ve güçlü tutarlılık için büyük miktarda yapılandırılmamış ve yarı yapılandırılmış verileri sütun aileleri tarafından veritabanında sağlar.

Veriler bir tablonun satırlarında depolanır ve satır içindeki veriler sütun ailesi tarafından gruplandırılır. HBase, kullanılmadan önce sütunların ya da bunlarda depolanan veri türünün tanımlanmasına gerek duyulmayan, şemasız bir veritabanıdır. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. HBase veri yedekliği, toplu işleme ve Hadoop ekosistemindeki dağıtılmış uygulamalar tarafından sağlanan diğer özelliklere dayanabilir.   

HBase gelecekteki analizler için sensör ve günlük verileri için mükemmel bir hedef olur.

HBase ölçeklenebilirlik, HDInsight kümesindeki düğümlere sayısına bağlıdır.

### <a name="azure-sql-database-and-azure-database"></a>Azure SQL veritabanı ve Azure veritabanı

Azure platformu-bir hizmet olarak (PAAS) üç farklı ilişkisel veritabanları sunar.

* [Azure SQL veritabanı](../../sql-database/sql-database-technical-overview.md) Microsoft SQL Server'ın uygulamasıdır. Performans hakkında daha fazla bilgi için bkz. [performans ayarlama Azure SQL veritabanı'nda](../../sql-database/sql-database-performance-guidance.md).
* [MySQL için Azure veritabanı](../../mysql/overview.md) Oracle MySQL uygulamasıdır.
* [PostgreSQL için Azure veritabanı](../../postgresql/quickstart-create-server-database-portal.md) PostgreSQL uygulamasıdır.

Bunlar daha fazla CPU ve bellek ekleyerek ölçeklenir anlamına gelir, bu ürünleri ölçeklendirin.  Ürünlerle iyi g/ç performansı için premium diskleri kullanmayı seçebilirsiniz.

## <a name="azure-analysis-services"></a>Azure Analysis Services 

Azure Analysis Services (AAS) karar destek ve İş analizi, Reporting Services raporları, iş raporlar ve Power BI, Excel gibi istemci uygulamaları için analitik veriler sağlayan ve diğer verileri kullanılan bir analitik veri altyapısıdır Görselleştirme araçları.

Analiz küpler katmanları ayrı ayrı her küp için değiştirerek ölçeklendirebilirsiniz.  Daha fazla bilgi için [Azure Analysis Services fiyatlandırması](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="extract-and-load"></a>Ayıklama ve yükleme

Azure'da verileri mevcut olduğunda, ayıklayın ve diğer ürünlere kaymasını yüklemek için birçok hizmet kullanabilirsiniz.  HDInsight, Sqoop ve Flume destekler. 

### <a name="apache-sqoop"></a>Apache Sqoop

Apache Sqoop verimli bir şekilde yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış veri kaynakları arasında veri aktarmak için tasarlanmış bir araçtır. 

Sqoop alma ve paralel işlem ve hata toleransı sağlamak için verileri dışarı aktarmak için MapReduce kullanır.

### <a name="apache-flume"></a>Apache Flume

Apache Flume, verimli bir şekilde toplanması, toplama ve büyük miktarlarda günlük veri taşıma için dağıtılmış, güvenilir ve kullanılabilir bir hizmettir. Flume veri akışları akış dayalı basit ve esnek bir mimariye sahiptir. Flume, güçlü ve hataya dayanıklı ayarlanabilir güvenilirlik mekanizmalar ve birçok yük devretme ve kurtarma mekanizmaları ile. Flume çevrimiçi analitik uygulama için izin veren basit genişletilebilir veri modeli kullanır.

Azure HDInsight ile Apache Flume kullanılamaz.  Bir şirket içi Hadoop yükleme, Flume, Azure depolama Blobları veya Azure Data Lake Storage veri göndermek için kullanabilirsiniz.  Daha fazla bilgi için [kullanarak, HDInsight ile Apache Flume](https://web.archive.org/web/20190217104751/https://blogs.msdn.microsoft.com/bigdatasupport/2014/03/18/using-apache-flume-with-hdinsight/).

## <a name="transform"></a>Dönüşüm

Veriler Seçilen konumda mevcut olduğunda, şekillendirip temizleyerek, onu birleştirin veya belirli kullanım deseni için hazırlama gerekir.  Hive, Pig ve Spark SQL, bu tür bir iş için tüm iyi seçimlerdir.  HDInsight üzerinde tümü desteklenir. 

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
* [Apache Hive bir ETL aracı olarak kullanma](apache-hadoop-using-apache-hive-as-an-etl-tool.md) 
* [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
