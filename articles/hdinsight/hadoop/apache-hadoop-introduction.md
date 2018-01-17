---
title: "HDInsight ile Hadoop ve Spark teknoloji yığını nedir? - Azure | Microsoft Docs"
description: "HDInsight ile Hadoop ve Spark teknoloji yığını ve bileşenlerine (büyük veri analizi için Kafka, Hive, Storm ve HBase dahil) giriş."
keywords: "azure hadoop, hadoop azure, hadoop giriş, hadoop’a giriş, hadoop teknoloji yığını, yeni başlayanlar için hadoop, temel hadoop bilgileri, hadoop kümesi nedir, hadoop kümeleri nedir, hadoop ne için kullanılır"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/13/2017
ms.author: cgronlun
ms.openlocfilehash: c53c4eba6d46c03bbfc6bb316ae4e505abb7b781
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2018
---
# <a name="introduction-to-azure-hdinsight-and-the-hadoop-and-spark-technology-stack"></a>Azure HDInsight ile Hadoop ve Spark teknoloji yığınına giriş
Bu makalede, Azure HDInsight için bir tanıtım sunulmaktadır. Azure HDInsight kuruluşlara yönelik tam yönetilen, tam spektrumlu ve açık kaynaklı bir analiz hizmetidir. Hadoop, Spark, Hive, LLAP, Kafka, Storm, R ve daha fazla açık kaynak çerçeveyi kullanabilirsiniz. 

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Kümelerde büyük veri kümelerinin dağıtılmış işlenmesi ve analizine yönelik ilk açık kaynak çerçeve [Apache Hadoop](http://hadoop.apache.org/)’tu. Hadoop teknoloji yığını; Apache Hive, HBase, Spark, Kafka ve diğer birçok bileşen dahil olmak üzere ilgili yazılım ve yardımcı programları içerir. 

HDInsight üzerindeki kullanılabilir Hadoop teknolojisi yığını bileşenlerini görmek için, bkz. [HDInsight ile sağlanan bileşenler ve sürümler][component-versioning]. HDInsight'ta Hadoop hakkında daha fazla bilgi edinmek için bkz. [HDInsight için Azure özellikleri sayfası](https://azure.microsoft.com/services/hdinsight/).

[Apache Spark](http://spark.apache.org), büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen açık kaynaklı bir paralel işleme altyapısıdır. HDInsight’ta Spark hakkında daha fazla bilgi için bkz. [Azure HDInsight’ta Spark’a giriş](../spark/apache-spark-overview.md). 

<a href="https://ms.portal.azure.com/#create/Microsoft.HDInsightCluster" target="_blank"><img src="./media/apache-hadoop-introduction/deploy-to-azure.png" alt="Deploy an Azure HDInsight cluster"></a>

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>HDInsight ve Hadoop teknoloji yığını nedir? 
Azure HDInsight, [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/) tarafından sağlanan Hadoop bileşenlerinin bulut dağıtımıdır. Azure HDInsight, devasa miktarlardaki verileri işlemeyi kolay, hızlı ve uygun maliyetli hale getirir. Hadoop, Spark, Hive, LLAP, Kafka, Storm ve R gibi en popüler açık kaynak çerçeveleri kullanabilirsiniz. Bu çerçeveler sayesinde ayıklama, dönüştürme ve yükleme (ETL), veri ambarı, makine öğrenimi ve IoT gibi diğer birçok senaryoyu mümkün kılabilirsiniz.

## <a name="what-is-big-data"></a>Büyük veri nedir?

Hacmi gittikçe artan büyük veriler hiç olmadığı kadar yüksek hızlarda ve yüksek çeşitlilikteki biçimlerde toplanmaktadır. Geçmiş tarihli (depolanmış anlamına gelir) veya gerçek zamanlı (kaynaktan akışlı anlamına gelir) olabilir. Büyük veriler için en yaygın kullanım örnekleri hakkında bilgi edinmek için bkz. [HDInsight kullanma senaryoları](#scenarios-for-using-hdinsight).

## <a name="why-should-i-use-hdinsight"></a>Neden HDInsight kullanmalıyım?

Bu bölümde Azure HDInsight özellikleri listelenmektedir.


|Özellik  |Açıklama  |
|---------|---------|
|Yerel bulut     |     Azure HDInsight, Azure üzerinde [Hadoop](apache-hadoop-linux-tutorial-get-started.md),  [Spark](../spark/apache-spark-jupyter-spark-sql.md),  [Etkileşimli sorgu (LLAP)](../interactive-query/apache-interactive-query-get-started.md),  [Kafka](../kafka/apache-kafka-get-started.md),  [Storm](../storm/apache-storm-tutorial-get-started-linux.md),  [HBase](../hbase/apache-hbase-tutorial-get-started-linux.md) ve  [R Server](../r-server/r-server-get-started.md) için en iyi duruma getirilmiş kümeler oluşturmanıza olanak sağlar. HDInsight ayrıca tüm üretim iş yüklerinizde uçtan uca SLA sağlar.  |
|Düşük maliyetli ve ölçeklendirilebilir     | HDInsight iş yüklerinin [ölçeğini](../hdinsight-administer-use-portal-linux.md)  artırmanıza veya azaltmanıza olanak sağlar.  [İsteğe bağlı olarak kümeler oluşturup](../hdinsight-hadoop-create-linux-clusters-adf.md)  yalnızca kullandığınız kadarı için ödeme yaparak maliyetleri azaltabilirsiniz. İşlerinizi kullanıma hazır hale getirmek için veri işlem hatları da oluşturabilirsiniz. Ayrılmış işlem ve depolama daha iyi performans ve esneklik sağlar. |
|Güvenli ve uyumlu    | HDInsight; [Azure Sanal Ağ](../hdinsight-extend-hadoop-virtual-network.md), [şifreleme](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md) ve [Azure Active Directory](../domain-joined/apache-domain-joined-introduction.md) tümleştirmesi ile kurumsal veri varlıklarınızı korumanıza olanak sağlar. HDInsight ayrıca en popüler sektör ve kamu [uyumluluk standartlarını](https://azure.microsoft.com/overview/trusted-cloud) karşılar.        |
|İzleme    | Azure HDInsight, tüm kümelerinizi izlemek için kullanabileceğiniz tek bir arabirim sağlamak üzere [Azure Log Analytics](../hdinsight-hadoop-oms-log-analytics-tutorial.md) ile tümleştirilir.        |
|Genel kullanılabilirlik | HDInsight diğer büyük veri analizi tekliflerinden daha fazla  [bölgede](https://azure.microsoft.com/regions/services/)  kullanılabilir. Azure HDInsight ayrıca temel bağımsız bölgelerde kurumsal ihtiyaçlarınızı karşılamanıza olanak sağlayan Azure Kamu, Çin ve Almanya’da da kullanılabilir. |  
|Üretkenlik     |  Azure HDInsight, tercih ettiğiniz geliştirme ortamlarıyla Hadoop ve Spark için zengin üretkenlik araçları kullanmanıza imkan tanır. Bu geliştirme ortamlarına Scala, Python, R, Java ve .NET için [Visual Studio](apache-hadoop-visual-studio-tools-get-started.md), [Eclipse](../spark/apache-spark-eclipse-tool-plugin.md) ve [IntelliJ](../spark/apache-spark-intellij-tool-plugin.md) dahildir. Ayrıca, veri bilimcileri [Jupyter](../spark/apache-spark-jupyter-notebook-kernels.md) ve [Zeppelin](../spark/apache-spark-zeppelin-notebook.md) gibi popüler not defterlerini kullanarak işbirliği yapabilir.    |
|Genişletilebilirlik     |  [Betik eylemlerini](../hdinsight-hadoop-customize-cluster-linux.md) kullanarak, [kenar düğümleri ekleyerek](../hdinsight-apps-use-edge-node.md) veya [diğer büyük veri sertifikalı uygulamalarla tümleştirerek](../hdinsight-apps-install-applications.md) yüklü bileşenlerle (Hue, Presto, vb.) HDInsight kümelerini genişletebilirsiniz. HDInsight [tek tıklamayla](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) dağıtım ile en popüler büyük veri çözümleriyle sorunsuz tümleştirme sağlar.|

## <a name="scenarios-for-using-hdinsight"></a>HDInsight kullanma senaryoları

Azure HDInsight, büyük veri işlemeye yönelik çeşitli senaryolar için kullanılabilir. Geçmiş verileri (zaten toplanmış ve depolanmış veriler) veya gerçek zamanlı veriler (doğrudan kaynaktan akışı yapılan veriler) olabilir. Bu tür verileri işlemeye yönelik senaryolar aşağıdaki kategorilerde özetlenebilir: 

### <a name="batch-processing-etl"></a>Toplu işleme (ETL)

Ayıklama, dönüştürme ve yükleme (ETL), heterojen veri kaynaklarından yapılandırılmış veya yapılandırılmamış verilerin ayıklandığı bir süreçtir. Bunlar daha sonra yapılandırılmış bir biçime dönüştürülür ve bir veri deposuna yüklenir. Dönüştürülen verileri veri bilimi veya veri ambarlama için kullanabilirsiniz.

### <a name="internet-of-things-iot"></a>Nesnelerin İnterneti (IoT)

HDInsight kullanarak çeşitli cihazlardan gerçek zamanlı olarak alınan akış verilerini işleyebilirsiniz. Daha fazla bilgi edinmek için [Azure tarafından hazırlanan ve Azure Yönetilen disklerle HDInsight’ta Apache Kafka önizlemesinin genel önizlemeye sunulduğunu duyuran bu blog gönderisini okuyun](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/).

![HDInsight mimarisi: Nesnelerin İnterneti](./media/apache-hadoop-introduction/hdinsight-architecture-iot.png) 

### <a name="data-science"></a>Veri bilimi

Verilerden kritik öngörüleri ayıklayan uygulamalar oluşturmak için HDInsight kullanabilirsiniz. İşletmeniz için gelecekteki eğilimleri tahmin etmek için buna ek olarak Azure Machine Learning de kullanabilirsiniz. Daha fazla bilgi için, [bu müşteri başarı öyküsünü okuyun](https://customers.microsoft.com/story/pros).

![HDInsight mimarisi: Veri bilimi](./media/apache-hadoop-introduction/hdinsight-architecture-data-science.png)

### <a name="data-warehousing"></a>Veri ambarlama

Herhangi bir biçimdeki yapılandırılmış veya yapılandırılmamış veriler üzerinde petabayt ölçeğinde etkileşimli sorgular gerçekleştirmek için HDInsight kullanabilirsiniz. Ayrıca bunları BI araçlarına bağlayan modeller de oluşturabilirsiniz. Daha fazla bilgi için, [bu müşteri başarı öyküsünü okuyun](https://customers.microsoft.com/story/milliman). 

![HDInsight mimarisi: Veri ambarlama](./media/apache-hadoop-introduction/hdinsight-architecture-data-warehouse.png)

### <a name="hybrid"></a>Karma

Bulutun gelişmiş analiz özelliklerinden yararlanmak üzere mevcut şirket içi büyük veri altyapınızı Azure’a genişletmek için HDInsight kullanabilirsiniz.

![HDInsight mimarisi: Karma](./media/apache-hadoop-introduction/hdinsight-architecture-hybrid.png)

## <a name="overview"></a>HDInsight’taki küme türleri
HDInsight belirli küme türlerinin yanı sıra bileşen, yardımcı program ve dil ekleme olanağı gibi küme özelleştirme özelliklerini de içerir.

### <a name="spark-kafka-interactive-query-hbase-customized-and-other-cluster-types"></a>Spark, Kafka, Interactive Query, HBase, özelleştirilmiş ve diğer küme türleri
HDInsight şu küme türlerini sunar:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: Toplu verileri paralel işlemek ve analiz etmek için [HDFS](#hdfs), [YARN](#yarn) kaynak yönetimini ve basit bir [MapReduce](#mapreduce) programlama modelini kullanan bir çerçeve.

* **[Apache Spark](http://spark.apache.org/)**: Büyük veri analizi uygulamalarının performansını artırmak üzere bellek içi işlemeyi destekleyen paralel işleme altyapısı. Spark; SQL, veri akışı ve makine öğrenimi için çalışır. Bkz. [HDInsight’ta Apache Spark nedir?](../spark/apache-spark-overview.md)

* **[Apache HBase](http://hbase.apache.org/)**: Büyük miktarlarda yapılandırmamış ve yarı yapılandırılmış veriler (potansiyel olarak milyarlarca satır çarpı milyonlarca sütun) için rastgele erişim ve güçlü tutarlılık özellikleri sağlayan Hadoop'u temel alan bir NoSQL veritabanı. Bkz. [HDInsight'ta HBase nedir?](../hbase/apache-hbase-overview.md)

* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: Paralel ve dağıtılmış R işlemlerini barındırmak ve yönetmek için bir sunucu. Veri uzmanlarının, istatistikçilerin ve R programcılarının HDInsight üzerindeki ölçeklenebilir ve dağıtılmış analitik yöntemlerine istedikleri an erişmesini sağlar. Bkz. [HDInsight'ta R Server'a genel bakış](../r-server/r-server-overview.md).

* **[Apache Storm](https://storm.incubator.apache.org/)**: Büyük veri akışlarını hızlı şekilde işlemek için dağıtılmış, gerçek zamanlı bir işlem sistemi. Storm HDInsight’ta yönetilen küme olarak sunulur. Bkz. [Storm ve Hadoop kullanarak gerçek zamanlı algılayıcı verilerini çözümleme](../storm/apache-storm-sensor-data-analysis.md).

* **[Apache Interactive Query önizleme (diğer adı: Live Long and Process)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: Etkileşimli ve daha hızlı Hive sorguları için bellek içi önbelleğe alma. Bkz. [HDInsight'ta Interactive Query kullanımı](../interactive-query/apache-interactive-query-get-started.md).

* **[Apache Kafka](https://kafka.apache.org/)**: Akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılan açık kaynak platform. Kafka ayrıca veri akışları yayımlamanızı ve abone olmanızı sağlayan ileti-kuyruk işlevi de sunar. Bkz. [HDInsight'ta Apache Kafka'ya giriş](../kafka/apache-kafka-introduction.md).

## <a name="open-source-components-in-hdinsight"></a>HDInsight’ta açık kaynak bileşenler

Azure HDInsight Hadoop,  Spark,  Hive,  LLAP,  Kafka,  Storm,  HBase ve  R gibi açık kaynak çerçeveler ile kümeler oluşturmanıza olanak sağlar. Bu kümeler, varsayılan olarak, kümeye dahil edilen [Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md), [Avro](http://avro.apache.org/docs/current/spec.html), [Hive](http://hive.apache.org), [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog/), [Mahout](https://mahout.apache.org/), [MapReduce](http://wiki.apache.org/hadoop/MapReduce), [YARN](http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Phoenix](http://phoenix.apache.org/), [Pig](http://pig.apache.org/), [Sqoop](http://sqoop.apache.org/), [Tez](http://tez.apache.org/), [Oozie](http://oozie.apache.org/), [ZooKeeper](http://zookeeper.apache.org/) gibi diğer açık kaynak bileşenler ile birlikte gelir.  


## <a name="programming-languages-in-hdinsight"></a>HDInsight’taki programlama dilleri
Spark, HBase, Kafka ve Hadoop gibi HDInsight kümeleri birçok programlama dilini destekler. Bazı programlama dilleri varsayılan olarak yüklü değildir. Varsayılan olarak yüklü olmayan kitaplıklar, modüller veya paketler için [bileşeni betik eylemi kullanarak yükleyin](../hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Varsayılan programlama dili desteği 
Varsayılan olarak, HDInsight kümeleri aşağıdakileri destekler:

* Java
* Python

[Betik eylemleri](../hdinsight-hadoop-script-actions-linux.md) kullanarak ek diller yükleyebilirsiniz.

### <a name="java-virtual-machine-jvm-languages"></a>Java sanal makine (JVM) dilleri
Java sanal makinelerinde (JVM) Java dışındaki birçok dil çalışabilir. Bununla birlikte, bu dillerden bazılarını çalıştırırsanız kümeye ek bileşenler yüklemeniz gerekebilir.

Aşağıdaki JVM tabanlı diller HDInsight kümelerinde desteklenir:

* Clojure
* Jython (Java için Python)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop’a özgü diller
HDInsight kümeleri, Hadoop teknoloji yığınına özgü aşağıdaki dilleri destekler:

* Pig işleri için Pig Latin
* Hive işleri için HiveQL ve SparkSQL

## <a name="business-intelligence-on-hdinsight"></a>HDInsight’ta İş Zekası
Bilinen iş zekası (BI) araçları, Power Query eklentisini veya Microsoft Hive ODBC sürücüsünü kullanarak HDInsight ile tümleştirilmiş verileri alır, çözümler ve raporlar:

* [Azure HDInsight ile veri görselleştirme araçları kullanarak Apache Spark BI](../spark/apache-spark-use-bi-tools.md)

* [Azure HDInsight’ta Microsoft Power BI ile Hive verileri görselleştirme](apache-hadoop-connect-hive-power-bi.md) 

* [Azure HDInsight'ta Power BI ile Etkileşimli Sorgu Hive verilerini görselleştirme](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)

* [Excel’i Power Query ile Hadoop’a Bağlama](apache-hadoop-connect-excel-power-query.md): Excel için Microsoft Power Query kullanarak Excel’i HDInsight kümenizden gelen verileri depolayan Azure Storage hesabına bağlamayı öğrenin. Windows İş İstasyonu gereklidir. 

* [Excel’i Microsoft Hive ODBC Sürücüsü ile Hadoop’a Bağlama](apache-hadoop-connect-excel-hive-odbc-driver.md): Microsoft Hive ODBC Sürücüsü ile verileri HDInsight’tan içeri aktarmayı öğrenin. Windows İş İstasyonu gereklidir. 

* [Microsoft Bulut Platformu](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Office 365 için Power BI hakkında bilgi edinin, SQL Server deneme sürümünü indirin ve SharePoint Server 2013 ve SQL Server BI’ı ayarlayın.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight’ta Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight’ta Spark kullanmaya başlama](../spark/apache-spark-jupyter-spark-sql.md)
* [HDInsight'ta Kafka kullanmaya başlama](../kafka/apache-kafka-get-started.md)
* [HDInsight üzerinde Storm kullanmaya başlama](../storm/apache-storm-tutorial-get-started-linux.md)
* [HDInsight üzerinde HBase kullanmaya başlama](../hbase/apache-hbase-tutorial-get-started-linux.md)
* [HDInsight üzerinde Etkileşimli Sorgu (LLAP) kullanmaya başlama](../interactive-query/apache-interactive-query-get-started.md)
* [HDInsight üzerinde R Server kullanmaya başlama](../r-server/r-server-get-started.md)
* [HDInsight kümelerini yönetme](../hdinsight-administer-use-portal-linux.md)
* [HDInsight kümelerinin güvenliğini sağlama](../domain-joined/apache-domain-joined-introduction.md)
* [HDInsight kümelerini İzleme](../hdinsight-hadoop-oms-log-analytics-tutorial.md)


[component-versioning]: ../hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
