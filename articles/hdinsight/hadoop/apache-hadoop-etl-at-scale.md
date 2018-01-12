---
title: "Ayıklama, dönüştürme ve ölçekte - Azure Hdınsight (ETL) yükleme | Microsoft Docs"
description: "ETL hdınsight'ta Hadoop ile nasıl kullanıldığı hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 47c2d129cb296f6387142e03b14356bcd83ad698
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="extract-transform-and-load-etl-at-scale"></a>Ayıklama, dönüştürme ve yükleme (ETL) ölçekte

Ayıklama, dönüştürme ve yükleme (ETL) işlemidir olarak veriler çeşitli kaynaklardan edinilen, standart bir konumda toplanır, temizlenen ve işlenen ve sonuçta, sorgulanabilir bir veri deposu yüklenir. Eski ETL işlemleri veri içeri aktarın, yerinde temizleyin ve bir ilişkisel veri altyapısında depolar. Hdınsight ile çok çeşitli Hadoop ekosistemi bileşenlerini destekleyen ETL ölçekte gerçekleştirme. 

Bu ardışık düzen tarafından ETL işlemi Hdınsight kullanımda özetlenebilir:

![Hdınsight ETL genel bakış](./media/apache-hadoop-etl-at-scale/hdinsight-etl-at-scale-overview.png)

Aşağıdaki bölümlerde her ETL aşamaları ve ilişkili bileşenleri keşfedin.

## <a name="orchestration"></a>Düzenleme

Orchestration tüm ETL ardışık düzen aşamaları yayar. Hdınsight'ta ETL işleri genellikle birbirleri ile birlikte çalışma birkaç farklı ürünlere içerir.  Hive, Pig başka bir bölümü temizler sırasında veri kısmı temizlemek için kullanabilirsiniz.  Azure Data Factory, Azure Data Lake Store Azure SQL veritabanından veri yüklemek için kullanabilirsiniz.

Orchestration, doğru zamanda uygun işi çalıştırmak için gereklidir.

### <a name="oozie"></a>Oozie

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir. Oozie Hdınsight küme içinde çalışır ve Hadoop yığını ile tümleştirilir. Oozie Apache MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işlerini destekler. Oozie, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanılabilir.

Daha fazla bilgi için bkz: [tanımlamak ve Hdınsight'ta bir iş akışını çalıştırmak için Hadoop ile kullanım Oozie](../hdinsight-use-oozie-linux-mac.md)

<!-- For a deep dive showing how to use Oozie to drive an end-to-end pipeline, see [Operationalize the Data Pipeline](hdinsight-operationalize-data-pipeline.md). -->

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory hizmet olarak platform biçiminde orchestration yetenekleri sağlar. Veri temelli iş akışlarını yönetme ve veri taşıma ve veri dönüştürme otomatikleştirmek için bulutta oluşturmanıza olanak tanıyan bir bulut tabanlı veri tümleştirme hizmetidir. 

Azure Data Factory kullanarak şunları yapabilirsiniz:

1. Oluşturma ve farklı veri depolarına veri alma (ardışık olarak adlandırılır) veri temelli iş akışlarını zamanlayabilirsiniz.
2. İşlem ve kullanarak veri dönüştürme hizmetleri Azure Hdınsight Hadoop, Spark, Azure Data Lake Analytics, Azure Batch ve Azure Machine Learning gibi işlem.
3. Çıktı verilerini iş zekası (BI) uygulamalarının kullanması için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilirsiniz.

Azure Data Factory hakkında daha fazla bilgi için bkz: [belgelerine](../../data-factory/introduction.md).

## <a name="ingest-file-storage-and-result-storage"></a>Dosya depolama ve sonuç deposu alma

Kaynak veri dosyalarını, genellikle Azure Storage veya Azure Data Lake Store içinde bir konuma yüklenir. Dosyaları herhangi bir biçimde olabilir ancak genellikle csv gibi düz dosyalar etmektedir. 

### <a name="azure-storage"></a>Azure Storage 

[Azure depolama](https://azure.microsoft.com/services/storage/blobs/) sahip [belirli ölçeklenebilirlik hedefleri](../../storage/common/storage-scalability-targets.md).  En analitik düğümleri için Azure Storage ile çok daha küçük dosyalar ilgilenirken en iyi şekilde ölçeklendirir.  Azure depolama kaç dosyaları veya (sınırlar içinde olduğunuz sürece) ne kadar büyük dosyalar olsun aynı performansı güvence altına alır.  Bir alt kümesini verileri veya tüm verileri kullanıp kullanmadığınızı bu terabayt veri depolamak ve tutarlı bir performans almaya devam anlamına gelir.

Azure Storage birkaç farklı türde blob var.  Bir *blob append* web günlükleri veya algılayıcı verilerini depolamak için kullanışlı bir seçenektir.  

Birden çok BLOB'ları onlara erişimi genişletmek için çok sayıda sunucuya dağıtılmış olabilir, ancak tek bir blob yalnızca tek bir sunucu tarafından sunulan. BLOB'ları blob kapsayıcı mantıksal olarak gruplandırılabilir olsa da, bu gruplandırma bölümleme hiçbir etkileri vardır.

Azure depolama blob storage için WebHDFS API katmanı da vardır.  Hdınsight'ta tüm hizmetleri Azure Blob Depolama veri temizleme ve bu hizmetleri Hadoop dağıtılmış dosya sistemi (HDFS) nasıl kullanacağınız benzer şekilde, veri işleme, dosyalara erişebilir.

Veri genellikle Azure PowerShell, Azure depolama SDK'sını ya da AZCopy kullanarak depolama alanına alınan.

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

Azure Data Lake deposu (ADLS) ile HDFS uyumlu analiz verileri için yönetilen, ölçekte deposudur.  ADLS HDFS için benzer ve sınırsız ölçeklenebilirlik açısından toplam kapasite ve tek tek dosyaların boyutu sunan bir tasarım standardı kullanır. ADLS çok büyük bir dosya birden çok düğümüne depolanabilir beri büyük dosyaları ile çalışırken uygundur.  ADLS veri bölümlendirme arka planda gerçekleştirilir.  Yüzlerce terabayt boyutunda veriyi verimli bir şekilde okuyan ve yazan eşzamanlı binlerce yürütücü sayesinde, analiz işlerini çok yüksek aktarım hızlarıyla çalıştırabilirsiniz.

Veri genellikle Azure Data Factory, ADLS SDK'ları, AdlCopy hizmeti, Apache Distcp'yi ya da Apache Sqoop kullanma ADLS alınan.  Büyük ölçüde kullanmak için bu hizmetleri hangisinin verileri nerede olduğuna bağlıdır.  Verileri şu anda varolan bir Hadoop kümede ise, Apache Distcp'yi, AdlCopy hizmeti veya Azure Data Factory kullanabilir.  Azure Blob depolama alanına ise, Azure Data Lake Store .NET SDK, Azure PowerShell veya Azure Data Factory kullanabilir.

ADLS ayrıca Azure olay hub'ı ya da Apache Storm kullanan olay alımı için optimize edilmiştir.

#### <a name="considerations-for-both-storage-options"></a>Her iki depolama seçenekleri için ilgili önemli noktalar

Veri kümeleri terabayt aralığında yüklemek için özellikle verileri bir şirket içi konumdan geliyorsa ağ gecikmesi büyük bir sorun olabilir.  Böyle durumlarda, aşağıdaki seçenekleri kullanabilirsiniz:

* Azure ExpressRoute: Azure ExpressRoute, Azure veri merkezleri ve şirket içi altyapınızı arasında özel bağlantılar oluşturmanızı sağlar. Bu bağlantılar büyük miktarlarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için bkz: [Azure ExpressRoute belgeleri](../../expressroute/expressroute-introduction.md).

* "Çevrimdışı" verilerin karşıya yükleyin. Kullanabileceğiniz [Azure içeri/dışarı aktarma hizmeti](../../storage/common/storage-import-export-service.md) verilerinizi bir Azure veri merkezi için sabit disk sürücülerini dağıtmayı. Verilerinizi ilk Azure Storage Bloblarında yüklenir. Daha sonra [Azure Data Factory](../../data-factory/v1/data-factory-azure-datalake-connector.md) veya [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md) Azure Storage bloblarından Data Lake Store'a veri kopyalamak için aracı.

### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

Azure SQL DW depolamak için bir harika temizlendi ve sonuçları gelecekteki analizi için hazırlanan seçimdir.  Azure Hdınsight, Azure SQL DW için bu hizmetlerin gerçekleştirmek için kullanılabilir.

Azure SQL veri ambarı (SQL DW) analitik iş yükleri için en iyi hale getirilmiş bir ilişkisel veritabanı deposudur.  Azure SQL DW bölümlenmiş tabloları göre ölçeklendirir.  Tablolar arasında birden çok düğüm bölümlenebilir.  Azure SQL DW düğümleri oluşturma sırasında seçilir.  Olaydan sonra ölçeklendirme yapabilir, ancak veri taşıma gerektirebilir etkin bir işlem. Bkz: [SQL Data Warehouse - yönetmek işlem](../../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md) daha fazla bilgi için.

### <a name="hbase"></a>HBase

Apache HBase Azure Hdınsight'ta kullanılabilir bir anahtar-değer deposudur.  Apache HBase, Hadoop’ta oluşturulan ve Google BigTable’a göre modellenen açık kaynaklı bir NoSQL veritabanıdır. HBase kullanıcı rasgele erişim ve büyük miktarlarda sütun aileleri tarafından veritabanında yapılandırılmamış ve yarı yapılandırılmış veriler için güçlü tutarlılık sağlar.

Veriler bir tablonun satırlarında depolanır ve satır içindeki veriler sütun ailesi tarafından gruplandırılır. HBase, kullanılmadan önce sütunların ya da bunlarda depolanan veri türünün tanımlanmasına gerek duyulmayan, şemasız bir veritabanıdır. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. HBase veri artıklığı, toplu işleme ve Hadoop ekosistemindeki dağıtılmış uygulamalar tarafından sağlanan diğer özelliklere güvenebilirsiniz.   

HBase algılayıcı ve günlük verilerini gelecekteki analiz için mükemmel bir hedefi değil.

HBase ölçeklenebilirlik, Hdınsight kümedeki düğüm sayısını bağımlıdır.

### <a name="azure-sql-database-and-azure-database"></a>Azure SQL Database ve Azure veritabanı

Azure platformu-bir hizmet olarak (PAAS) üç farklı ilişkisel veritabanları sunar.

* [Azure SQL veritabanı](../../sql-database/sql-database-technical-overview.md) Microsoft SQL Server'ın uygulamasıdır. Performans hakkında daha fazla bilgi için bkz: [Azure SQL veritabanında performans ayarlama](../../sql-database/sql-database-performance-guidance.md).
* [Azure veritabanı için MySQL](../../mysql/overview.md) Oracle MySQL uygulamasıdır.
* [Azure veritabanı PostgreSQL için](../../postgresql/quickstart-create-server-database-portal.md) PostgreSQL uygulamasıdır.

Bunlar daha fazla CPU ve bellek ekleyerek ölçeklenir anlamına gelir, bu ürünler ölçeklendirin.  Premium diskleri ürünleri ile daha iyi g/ç performans kullanmayı seçebilirsiniz.

## <a name="azure-analysis-services"></a>Azure Analysis Services 

Karar destek ve İş analizi, Raporlama Hizmetleri raporları, iş raporları ve Power BI, Excel gibi istemci uygulamalarını analitik verileri sağlayan ve diğer verileri kullanılan bir analitik veri altyapısı Azure Analysis Services (AAS). görsel araçlar.

Analiz küpleri, tek tek her küp için katmanları değiştirerek ölçeklendirebilirsiniz.  Daha fazla bilgi için bkz: [Azure Analysis Services fiyatlandırması](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="extract-and-load"></a>Ayıklama ve yükleme

Verileri Azure'da var olduğunda, birçok Hizmetleri ayıklamak ve diğer ürünler yüklemek için kullanabilirsiniz.  Hdınsight Sqoop ve Flume destekler. 

### <a name="sqoop"></a>Sqoop

Apache Sqoop verimli bir şekilde yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış veri kaynakları arasında veri aktarmak için tasarlanmış bir araçtır. 

Sqoop MapReduce içeri ve dışarı aktarma paralel işlem ve hataya dayanıklılık sağlamak için verileri için kullanır.

### <a name="flume"></a>Flume

Apache Flume, verimli bir şekilde toplanması, toplama ve büyük miktarlarda günlük veri taşıma için dağıtılmış, güvenilir ve kullanılabilir bir hizmettir. Flume veri akışları akış dayalı basit ve esnek bir mimari vardır. Flume, güçlü ve hataya dayanıklı ince ayarlanabilir güvenilirlik mekanizmaları ve birçok yük devretme ve kurtarma mekanizmaları ile. Flume için çevrimiçi analitik uygulama sağlayan bir basit genişletilebilir veri modeli kullanır.

Apache Flume Azure Hdınsight ile kullanılamaz.  Bir şirket içi Hadoop yükleme, Flume, Azure Storage Bloblarında veya Azure Data Lake Store için veri göndermek için kullanabilirsiniz.  Daha fazla bilgi için bkz: [kullanarak Apache Flume Hdınsight ile](https://blogs.msdn.microsoft.com/bigdatasupport/2014/03/18/using-apache-flume-with-hdinsight/).

## <a name="transform"></a>Dönüştürme

Veriler Seçilen konumda bulunduğunu sonra temizlemesini, onu birleştirmek veya belirli kullanım düzeni için hazırlanması gerekir.  Hive, Pig ve Spark SQL, bu tür bir iş için tüm iyi seçimlerdir.  Hdınsight üzerinde tüm desteklenir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
<!-- * [Using Apache Hive as an ETL Tool](hdinsight-using-apache-hive-as-an-etl-tool.md) -->
