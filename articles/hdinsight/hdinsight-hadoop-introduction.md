---
title: "HDInsight, Hadoop teknoloji yığını ve kümeler nedir? - Azure | Microsoft Docs"
description: "HDInsight, Hadoop teknoloji yığını ve bileşenlerine (büyük veri analizi için Spark, Kafka, Hive, HBase dahil) giriş."
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
ms.date: 07/20/2017
ms.author: cgronlun
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: e5ed09ddb1556e6c76813e71bcb31cf4f792b616
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---
# <a name="introduction-to-azure-hdinsight-the-hadoop-technology-stack-and-hadoop-clusters"></a>Azure HDInsight, Hadoop teknoloji yığını ve Hadoop kümelerine giriş
 Bu makalede, Hadoop teknoloji yığınının bir bulut dağıtımı olan Azure HDInsight tanıtılmaktadır. Hadoop kümelerinin ne olduğu ve ne zaman kullanılacağı da ele alınmaktadır. 

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>HDInsight ve Hadoop teknoloji yığını nedir? 
Azure HDInsight, [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/) tarafından sağlanan Hadoop bileşenlerinin bulut dağıtımıdır. Bilgisayar kümelerinde büyük veri kümelerinin dağıtılmış işlenmesi, depolanması ve analizine yönelik ilk açık kaynak çerçeve [Apache Hadoop](http://hadoop.apache.org/)’tu. 


Hadoop teknoloji yığını; Apache Hive, HBase, Spark, Kafka ve diğer birçok bileşen dahil olmak üzere ilgili yazılım ve yardımcı programları içerir. HDInsight üzerindeki kullanılabilir Hadoop teknolojisi yığını bileşenlerini görmek için, bkz. [HDInsight ile sağlanan bileşenler ve sürümler][component-versioning]. HDInsight'ta Hadoop hakkında daha fazla bilgi edinmek için bkz. [HDInsight için Azure özellikleri sayfası](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-a-hadoop-cluster-and-when-do-you-use-it"></a>Hadoop kümesi nedir ve ne zaman kullanılır?
Ayrıca *Hadoop* bir küme türü olarak şunlara da sahiptir:

* İş zamanlama ve kaynak yönetimi için YARN
* Paralel işleme için MapReduce
* Hadoop dağıtılmış dosya sistemi (HDFS)
  
Hadoop kümelerinin en sık kullanıldığı senaryo, depolanan verilerin toplu olarak işlenmesidir. HDInsight’taki diğer küme türlerinin ek özellikleri vardır: Hızlı bellek içi işleme özelliği sayesinde Spark'ın popülerliği artmıştır. Ayrıntılı bilgi edinmek için bkz. [HDInsight’taki küme türleri](#overview).

## <a name="what-is-big-data"></a>Büyük veri nedir?
Büyük veri, aşağıdakiler gibi herhangi bir büyük dijital bilgi kütlesini ifade eder:

* Endüstriyel ekipmanlardan toplanan algılayıcı verileri
* Web sitesinden toplanan müşteri etkinliği verileri
* Twitter haber akışı

Büyük veri, artan miktarlarda, yükselen hızlarda ve artan çeşitlilikteki biçimlerde toplanır. Geçmiş tarihli (depolanmış anlamına gelir) veya gerçek zamanlı (kaynaktan akışlı anlamına gelir) olabilir. 

## <a name="overview"></a>HDInsight’taki küme türleri
HDInsight belirli küme türlerinin yanı sıra bileşen, yardımcı program ve dil ekleme gibi küme özelleştirme özelliklerini de içerir.

### <a name="spark-kafka-interactive-query-hbase-customized-and-other-cluster-types"></a>Spark, Kafka, Interactive Query, HBase, özelleştirilmiş ve diğer küme türleri
HDInsight şu küme türlerini sunar:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: Toplu verileri paralel işlemek ve analiz etmek için [HDFS](#hdfs), [YARN](#yarn) kaynak yönetimini ve basit bir [MapReduce](#mapreduce) programlama modelini kullanır.
* **[Apache Spark](http://spark.apache.org/)**: Büyük veri analizi uygulamalarının, SQL akış verileri için Spark işlerinin ve Machine Learning’in performansını artırmak üzere bellek içi işlemeyi destekleyen paralel bir işleme altyapısı. Bkz. [HDInsight’ta Apache Spark nedir?](hdinsight-apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: Büyük miktarlarda yapılandırmamış ve yarı yapılandırılmış veriler (potansiyel olarak milyarlarca satır çarpı milyonlarca sütun) için rastgele erişim ve güçlü tutarlılık özellikleri sağlayan Hadoop'u temel alan bir NoSQL veritabanı. Bkz. [HDInsight'ta HBase nedir?](hdinsight-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: Paralel ve dağıtılmış R işlemlerini barındırmak ve yönetmek için bir sunucu. Veri uzmanlarının, istatistikçilerin ve R programcılarının HDInsight üzerindeki ölçeklenebilir ve dağıtılmış analitik yöntemlerine istedikleri an erişmesini sağlar. Bkz. [HDInsight'ta R Server'a genel bakış](hdinsight-hadoop-r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: Büyük veri akışlarını hızlı şekilde işlemek için dağıtılmış, gerçek zamanlı bir işlem sistemi. Storm HDInsight’ta yönetilen küme olarak sunulur. Bkz. [Storm ve Hadoop kullanarak gerçek zamanlı algılayıcı verilerini çözümleme](hdinsight-storm-sensor-data-analysis.md).
* **[Apache Interactive Query önizleme (diğer adı: Live Long and Process)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: Etkileşimli ve daha hızlı Hive sorguları için bellek içi önbelleğe alma. Bkz. [HDInsight'ta Interactive Query kullanımı](hdinsight-hadoop-use-interactive-hive.md).
* **[Apache Kafka](https://kafka.apache.org/)**: Akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılan açık kaynak platform. Kafka ayrıca veri akışları yayımlamanızı ve abone olmanızı sağlayan ileti-kuyruk işlevi de sunar. Bkz. [HDInsight'ta Apache Kafka'ya giriş](hdinsight-apache-kafka-introduction.md).

Kümeleri yapılandırmak için aşağıdaki yöntemleri de kullanabilirsiniz:
* **[Etki alanına katılmış kümeler önizleme](hdinsight-domain-joined-introduction.md)**: Veri erişimini denetlemek ve yönetim sağlamak için Active Directory etki alanına katılmış bir küme.
* **[Betik eylemlerine sahip özel kümeler](hdinsight-hadoop-customize-cluster-linux.md)**: Ek bileşenlerin sağlanması ve yüklenmesi sırasında çalıştırılan betiklere sahip kümeler.

### <a name="example-cluster-customization-scripts"></a>Örnek küme özelleştirme betikleri
Betik eylemleri, küme hazırlama sırasında Linux üzerinde çalışan ve kümede ek bileşenleri yüklemek için kullanılabilen Bash betikleridir. 

Aşağıdaki örnek betikler HDInsight ekibi tarafından sağlanmıştır:

* **[Hue](hdinsight-hadoop-hue-linux.md)**: Kümeyle etkileşim kurmak için kullanılan bir dizi web uygulaması. Yalnızca Linux kümeleri.
* **[Giraph](hdinsight-hadoop-giraph-install-linux.md)**: Öğeler veya kişiler arasındaki ilişkileri modellemek için grafik işleme.
* **[Solr](hdinsight-hadoop-solr-install-linux.md)**: Verilerde tam metin arama yapma olanağı sağlayan kurumsal ölçekte bir arama platformu.

Kendi Betik Eylemlerinizi geliştirme hakkında daha fazla bilgi için bkz. [HDInsight ile Betik Eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).

## <a name="components-and-utilities-on-hdinsight-clusters"></a>HDInsight kümelerindeki bileşenler ve yardımcı programlar
Aşağıdaki bileşenler ve yardımcı programlar HDInsight kümelerine dahil edilmiştir:

* **[Ambari](#ambari)**: Küme hazırlama, yönetim, izleme ve yardımcı programları.
* **[Avro](#avro)** (Avro için Microsoft .NET kitaplığı): Microsoft .NET ortamı için veri seri hale getirme. 
* **[Hive & HCatalog](#hive)**: Yapılandırılmış Sorgu Dili (SQL) benzeri sorgulama ile bir tablo ve depolama yönetimi katmanı.
* **[Mahout](#mahout)**: Ölçeklenebilir makine öğrenme uygulamaları için.
* **[MapReduce](#mapreduce)**: Hadoop dağıtılan işleme ve kaynak yönetimi için eski altyapı. Bkz. [YARN](#yarn).
* **[Oozie](#oozie)**: İş akışı yönetimi.
* **[Phoenix](#phoenix)**: HBase üzerinde ilişkisel veritabanı katmanı.
* **[Pig](#pig)**: MapReduce dönüştürmeleri için daha basit betik oluşturma.
* **[Sqoop](#sqoop)**: Verileri içeri ve dışarı aktarma.
* **[Tez](#tez)**: Veri kullanımı yoğun işlemlerin ölçekli olarak verimli çalışmasını sağlar.
* **[YARN](#yarn)**: Hadoop çekirdek kitaplığının parçası olan kaynak yönetimi.
* **[ZooKeeper](#zookeeper)**: Dağıtılmış sistemlerdeki süreçlerin koordinasyonu.

> [!NOTE]
> Belirli bileşenler hakkında bilgi edinmek ve sürüm bilgilerini öğrenmek için bkz. [HDInsight’taki Hadoop bileşenleri ve sürümleri][component-versioning]
>
>

### <a name="ambari"></a>Ambari
Apache Ambari, Apache Hadoop kümelerini hazırlamak, yönetmek ve izlemek içindir. Bu, kümelerin çalışmasını kolaylaştırarak, işleç araçlarının sezgisel bir koleksiyonunu ve Hadoop’un karmaşıklığı gizleyen sağlam bir API kümesini içerir. Linux’taki HDInsight kümeleri hem Ambari web kullanıcı arabirimini hem de Ambari REST API’sini sağlar. HDInsight kümelerinde Ambari Görünümleri eklenti kullanıcı arabirimi özelliklerine imkan tanır.
Bkz. [Ambari kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md) ve <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API başvurusu</a>.

### <a name="avro"></a>Avro (Avro için Microsoft .NET kitaplığı)
Avro için Microsoft .NET kitaplığı, Microsoft .NET ortamının seri hale getirilmesi amacıyla Apache Avro sıkıştırılmış ikili veri değişim biçimini uygular. Bir dilde seri hale getirilmiş verilerin başka bir dilde okunabilmesi için dilden bağımsız bir şema tanımlar. Biçim hakkında ayrıntılı bilgi <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Özellikler</a> adresinde bulunabilir. Avro dosyalarının biçimi, dağıtılmış MapReduce programlama modelini destekler: Dosyalar “bölünebilir”, yani bir dosyadaki herhangi bir noktayı bulabilir ve bu bloktan itibaren okumaya başlayabilirsiniz. Nasıl yapacağınızı öğrenmek için bkz. [Avro için Microsoft .NET kitaplığı ile verileri seri hale getirme](hdinsight-dotnet-avro-serialization.md). Linux tabanlı küme desteği yakında sunulacaktır.

### <a name="hdfs"></a>HDFS
Hadoop Dağıtılmış Dosya Sistemi (HDFS), YARN ve MapReduce ile birlikte Hadoop teknolojisinin çekirdeğini oluşturan bir dosya sistemidir. HDInsight’taki Hadoop kümeleri için standart dosya sistemidir. Bkz. [HDFS ile uyumlu depolama alanından veri sorgulama](hdinsight-hadoop-use-blob-storage.md).

### <a name="hive"></a>Hive ve HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a>, HiveQL adında SQL benzeri bir dil kullanarak dağıtılmış depolamada büyük veri kümelerini sorgulamanızı ve yönetmenizi sağlayan, Hadoop üzerinde kurulu bir veri ambarı yazılımıdır. Pig gibi Hive da MapReduce üzerindeki bir soyutlamadır ve sorguları bir dizi MapReduce işine çevirir. Hive, Pig ile karşılaştırıldığında bir ilişkisel veritabanı yönetim sistemine daha yakındır ve daha fazla yapılandırılmış veriyle kullanılır. Yapılandırılmamış veriler için, Pig daha iyi bir seçimdir. Bkz: [HDInsight’ta Hadoop ile Hive kullanma](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a>, Hadoop için ilişkisel veri görünümü sunan bir tablo ve depolama yönetimi katmanıdır. HCatalog’da, dosyaları bir Hive SerDe (seri hale getirici-seri kaldırıcı) için uygun olan her biçimde okuyabilir ve yazabilirsiniz.

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a>, Hadoop üzerinde çalışan bir makine öğrenimleri algoritmaları kitaplığıdır. İstatistik ilkelerini kullanarak, Machine Learning uygulamaları sistemlere verilerden öğrenmeyi ve gelecekteki davranışı belirlemek için geçmiş çıktıları kullanmayı öğretir. Bkz. [Hadoop’ta Mahout kullanarak film önerileri oluşturma](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce, paralel büyük veri kümelerini toplu olarak işlemek için uygulamalara yazmaya yönelik eski yazılım altyapısıdır. Bir MapReduce işi, büyük veri kümelerini böler ve verileri işleme için anahtar-değer çiftleri halinde düzenler. MapReduce işleri [YARN](#yarn) üzerinde çalışır. Hadoop Wiki’deki <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> makalesine bakın.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a>, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir. Bu Hadoop yığını ile tümleştirilir ve MapReduce, Pig, Hive ve Sqoop için Hadoop işlerini destekler. Ayrıca, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanılabilir. Bkz. [Hadoop ile Oozie kullanma](hdinsight-use-oozie-linux-mac.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a>, HBase üzerinde ilişkisel veritabanı katmanıdır. Phoenix, SQL tablolarını doğrudan sorgulamanıza ve yönetmenize olanak tanıyan bir JDBC sürücüsü içerir. Phoenix, MapReduce kullanmak yerine, sorguları ve diğer ifadeleri yerel NoSQL API çağrılarına çevirir, böylece NoSQL depoları üstünde daha hızlı uygulamalar sağlar. Bkz. [HBase kümeleriyle Apache ve SQuirreL kullanma](hdinsight-hbase-phoenix-squirrel.md).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a>, Pig Latin adlı basit bir betik dilini kullanarak büyük veri kümelerinde karmaşık MapReduce dönüşümleri gerçekleştirmenize olanak tanıyan yüksek düzeyli bir platformdur. Pig, Hadoop’ta çalışacakları şekilde, Pig Latin betiklerini çevirir. Pig Latin’i genişletmek için Kullanıcı Tanımlı İşlevler (UDF) oluşturabilirsiniz. Bkz. [Hadoop ile Pig kullanma](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a>, aktarımları Hadoop ile SQL gibi ilişkisel veritabanları veya diğer yapılandırılmış veri depoları arasında mümkün olduğunca verimli şekilde toplu veri aktaran bir araçtır. Bkz: [Hadoop ile Sqoop kullanma](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a>, Hadoop YARN'ı temel alan ve karmaşık, döngüsel olmayan genel veri işleme grafikleri çalıştıran bir uygulama altyapısıdır. Bu, ölçekte daha verimli çalışmasıyla, Hive gibi veri yoğun işlemlere izin veren MapReduce altyapısının daha esnek ve güçlü bir ardılıdır. Bkz. [Hive ve HiveQL Kullanımında "Gelişmiş performans için Apache Tez kullanma"](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN, yeni nesil MapReduce’dür (MapReduce 2.0 veya MRv2) ve daha yüksek ölçeklenebilirlik ve gerçek zamanlı işleme ile MapReduce toplu işlemler ötesinde veri işleme senaryolarını destekler. YARN kaynak yönetimi ve dağıtılmış uygulama altyapısı sağlar. MapReduce işleri YARN üzerinde çalışır. Bkz. <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a>, paylaşılan bir hiyerarşik veri kayıtları ad alanını (znode) kullanarak büyük çaplı dağıtılmış sistemlerde işlemleri koordine eder. Znode’lar işlemleri koordine etmek için gerekli az miktarda meta bilgiler içerir: durum, konum, yapılandırma vb. Örnek olarak bkz. [Bir HBase kümesi ve Apache Phoenix içeren ZooKeeper](hdinsight-hbase-phoenix-squirrel-linux.md). 

## <a name="programming-languages-on-hdinsight"></a>HDInsight’ta programlama dilleri
Spark, HBase, Kafka ve Hadoop gibi HDInsight kümeleri birçok programlama dilini destekler, ancak bunların bazı varsayılan olarak yüklü değildir. Varsayılan olarak yüklü olmayan kitaplıklar, modüller veya paketler için [bileşeni yüklemek üzere betik eylemi kullanın](hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Varsayılan programlama dili desteği 
Varsayılan olarak, HDInsight kümeleri aşağıdakileri destekler:

* Java
* Python

[Betik eylemleri](hdinsight-hadoop-script-actions-linux.md) kullanılarak ek diller yüklenebilir.

### <a name="java-virtual-machine-jvm-languages"></a>Java sanal makine (JVM) dilleri
Java dışındaki birçok dil Java sanal makinesi (JVM) üzerinde çalıştırılabilir; ancak, bu dillerden bazılarını çalıştırmak kümeye yüklü ek bileşenler gerektirebilir.

Bu JVM tabanlı diller HDInsight kümelerinde desteklenir:

* Clojure
* Jython (Java için Python)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop’a özgü diller
HDInsight kümeleri, Hadoop teknoloji yığınına özgü aşağıdaki dilleri destekler:

* Pig işleri için Pig Latin
* Hive işleri için HiveQL ve SparkSQL

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standart ve HDInsight Premium
HDInsight, Standart ve Premium olmak üzere, iki kategoride büyük veri bulutu teklifleri sunar. HDInsight Standart, kuruluşların kendi büyük veri iş yüklerini çalıştırmak için kullanabileceği kurumsal ölçekte küme sağlar. HDInsight Premium, Standart özellikleri temel alır ve HDInsight kümesi için gelişmiş bir çözümsel ve güvenlik özellikleri sağlar. Daha fazla bilgi için bkz. [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="microsoft-business-intelligence-and-hdinsight"></a>Microsoft iş zekası ve HDInsight
Bilinen iş zekası (BI) araçları, Power Query eklentisini veya Microsoft Hive ODBC sürücüsünü kullanarak HDInsight ile tümleştirilmiş verileri alır, çözümler ve raporlar:

* [Excel’i Power Query ile Hadoop’a Bağlama](hdinsight-connect-excel-power-query.md): Excel için Microsoft Power Query kullanarak HDInsight kümenizden gelen verileri depolayan Azure Storage hesabına Excel’i bağlamayı öğrenin. Windows iş istasyonu gereklidir. 
* [Excel’i Microsoft Hive ODBC Sürücüsü ile Hadoop’a Bağlama](hdinsight-connect-excel-hive-odbc-driver.md): Microsoft Hive ODBC Sürücüsü ile verileri HDInsight’tan içeri aktarmayı öğrenin. Windows iş istasyonu gereklidir. 
* [Microsoft Bulut Platformu](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Office 365 için Power BI hakkında bilgi edinin, SQL Server deneme sürümünü indirin ve SharePoint Server 2013 ve SQL Server BI’ı ayarlayın.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight’ta Hadoop kullanmaya başlama](hdinsight-hadoop-linux-tutorial-get-started.md): HDInsight Hadoop kümeleri hazırlamak ve örnek Hive sorguları çalıştırmak için hızlı başlangıç öğreticisi.
* [HDInsight’ta Spark kullanmaya başlama](hdinsight-apache-spark-jupyter-spark-sql.md): Spark kümesi oluşturmak ve etkileşimli Spark SQL sorguları çalıştırmak için hızlı başlangıç öğreticisi.
* [HDInsight’ta R Sunucusu Kullanma](hdinsight-hadoop-r-server-get-started.md): HDInsight Premium’da R Sunucusu kullanmaya başlayın.
* [HDInsight kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md): Azure Portal, Azure CLI veya Azure PowerShell aracılığıyla bir HDInsight Hadoop kümesi hazırlamayı öğrenin.


[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
