 <properties
    pageTitle="Bulutta Hadoop nedir? HDInsight’a Giriş | Microsoft Azure"
    description="Bulutta Hadoop nedir ve Microsoft Azure HDInsight’ta nasıl yönetilir? Hadoop bileşenleri ve büyük verilerin çözümlemesine giriş."
    keywords="büyük veri analizi, hadoop’a giriş, hadoop nedir, bulutta hadoop, hadoop teknoloji yığını, hadoop ekosistemi"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/21/2016"
   ms.author="cgronlun"/>


# Bulutta Hadoop nedir? HDInsight’ta Hadoop ekosistemine giriş

Azure HDInsight’ta Hadoop, ekosistemi ve büyük verilere girişi: HDInsight’ta Hadoop nedir ve büyük veri analizi için Hadoop bileşenleri, ortak terminoloji ve senaryolar nelerdir? Ayrıca, HDInsight’ta bulutta Hadoop kullanmaya ilişkin Hadoop öğreticileri, belgeleri ve kaynakları hakkında bilgi edinin.

## HDInsight’ta Hadoop nedir?

Azure HDInsight **Hortonworks Veri Platformu (HDP)** Hadoop dağıtımı kullanır. HDInsight, yüksek güvenilirlik ve kullanılabilirlik ile büyük verileri işlemek, çözümlemek ve raporlamak için tasarlanmış bir yazılım altyapısı sağlayarak, yönetilen Apache Hadoop kümelerini bulutta dağıtır ve hazırlar.  

Hadoop genellikle Apache HBase, Apache Spark ve Apache Storm’un yanı sıra Hadoop şemsiyesi altındaki diğer teknolojileri içeren, tüm Hadoop ekosistemi bileşenlerini ifade eder. Ayrıntılar için aşağıdaki [HDInsight’ta Hadoop ekosistemine genel bakış](#overview) bölümüne bakın.

## Büyük veri nedir?

Büyük veri, Twitter akışındaki bir metinden endüstriyel ekipmandaki algılayıcı bilgilerine bir web sitesinde gezinen ve satın alma yapan müşteri hakkındaki bilgilere kadar tüm büyük hacimli dijital verileri açıklar. Büyük veri geçmiş tarihli (depolanmış veriler anlamına gelir) veya gerçek zamanlı (doğrudan kaynaktan akışlı anlamına gelir) olabilir. Büyük veri, giderek daha yüksek miktarlarda, yükselen hızlarda ve artan çeşitlilikteki biçimlerde toplanır.

Büyük verilere ilişkin işlem yapılabilir bilgi veya görüş sağlamak için, ilgili verileri toplamanız ve doğru soruları sormanız yetmez, bunun yanı sıra veriler erişilebilir, temiz, çözümlenmiş olmalı ve sonra kullanışlı şekilde sunulmalıdır. Bu, HDInsight’ta Hadoop’ta büyük veri çözümlemenin yardımcı olabileceği konudur.

## <a name="overview"></a>HDInsight’ta Hadoop ekosistemine genel bakış

HDInsight, büyük veri çözümleme için başvurulacak çözüm olan hızla genişleyen Apache Hadoop teknoloji yığınının Microsoft Azure üzerinde bir bulut uygulamasıdır. Apache Spark, HBase, Storm, Pig, Hive, Sqoop, Oozie, Ambari ve benzeri uygulamaları içerir. HDInsight, ayrıca Power BI, Excel, SQL Server Analysis Services ve SQL Server Reporting Services gibi iş zekası (BI) araçları ile entegre olur.

### Hadoop, HBase, Spark, Storm ve özelleştirilmiş kümeler

HDInsight Apache Hadoop, Spark, HBase veya Storm için küme yapılandırmaları sağlar. Veya [betik eylemleri ile kümeleri özelleştirebilirsiniz](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop** ("Sorgu" iş yükü): [HDFS](#hdfs) ile güvenilir veri depolama ve paralel verileri işlemek ve çözümlemek için basit bir [MapReduce](#mapreduce) programlama modeli sağlar.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: Büyük veri analizi uygulamalarının, SQL akış verileri için Spark işlerinin ve Machine Learning’in performansını artırmak üzere bellek içi işlemeyi destekleyen paralel bir işleme altyapısı. Bkz: [Genel Bakış: HDInsight’ta Apache Spark nedir?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>** ("NoSQL" iş yükü): Büyük miktarlarda yapılandırmamış ve yarı yapılandırılmış veriler, potansiyel olarak milyarlarca satır çarpı milyonlarca sütun, için rasgele erişim ve güçlü tutarlılık sağlayan Hadoop’ta oluşturulan bir NoSQL veritabanı. Bkz. [HDInsight’ta HBase’e genel bakış](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>** ("Akış" iş yükü): Büyük veri akışlarını hızlı şekilde işlemek için dağıtılmış, gerçek zamanlı bir işlem sistemi. Storm HDInsight’ta yönetilen küme olarak sunulur. Bkz. [Storm ve Hadoop kullanarak gerçek zamanlı algılayıcı verilerini çözümleme](hdinsight-storm-sensor-data-analysis.md).

### Örnek özelleştirme betikleri

Betik Eylemleri, küme hazırlama sırasında çalışan betikleridir ve bunlar kümede ek bileşenleri yüklemek için kullanılabilir. Linux tabanlı kümeler için, bunlar Bash betikleridir.

Aşağıdaki örnek betikler HDInsight ekibi tarafından sağlanmıştır:

* [Hue](hdinsight-hadoop-hue-linux.md): Kümeyle etkileşim kurmak için kullanılan bir dizi web uygulaması. Yalnızca Linux kümeleri.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): Öğeler veya kişiler arasındaki ilişkileri modellemek için grafik işleme.

* [R](hdinsight-hadoop-r-scripts-linux.md): Machine learning’de kullanılan istatistiksel bilgi işlem için açık kaynaklı dil ve ortam.

* [Solr](hdinsight-hadoop-solr-install-linux.md): Verilerde tam metin arama yapma olanağı sağlayan kurumsal ölçekte bir arama platformu.

Kendi Betik Eylemlerinizi geliştirme hakkında daha fazla bilgi için bkz. [HDInsight ile Betik Eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).


## Hadoop bileşenleri ve hizmet programları nelerdir?

Aşağıdaki bileşenler ve yardımcı programları HDInsight kümelerine dahil edilmiştir.

* **[Ambari](#ambari)**: Küme hazırlama, yönetim, izleme ve yardımcı programları.

* **[Avro](#avro)** (Avro için Microsoft .NET kitaplığı): Microsoft .NET ortamı için veri seri hale getirme.

* **[Hive & HCatalog](#hive)**: Sorgulama gibi bir Yapılandırılmış Sorgu Dili (SQL) ve bir tablo ve depolama yönetimi katmanı.

* **[Mahout](#mahout)**: Machine Learning.

* **[MapReduce](#mapreduce)**: Hadoop dağıtılan işleme ve kaynak yönetimi için eski altyapı. Bkz: [YARN](#yarn), gelecek nesil kaynak altyapısı.

* **[Oozie](#oozie)**: İş akışı yönetimi.

* **[Phoenix](#phoenix)**: HBase üzerinde ilişkisel veritabanı katmanı.

* **[Pig](#pig)**: MapReduce dönüştürmeleri için daha basit betik oluşturma.

* **[Sqoop](#sqoop)**: Verileri içeri ve dışarı aktarma.

* **[Tez](#tez)**: Veri kullanımı yoğun işlemlerin ölçekli olarak verimli çalışmasını sağlar.

* **[YARN](#yarn)**: Hadoop çekirdek kitaplığı ve gelecek nesil MapReduce yazılım altyapısının parçası.

* **[ZooKeeper](#zookeeper)**: Dağıtılmış sistemlerdeki süreçlerin koordinasyonu.

> [AZURE.NOTE] Belirli bileşenler ve sürüm bilgileri hakkında bilgi için bkz. [HDInsight’ta Hadoop bileşenleri, sürümleri ve hizmet teklifleri][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari, Apache Hadoop kümelerini hazırlamak, yönetmek ve izlemek içindir. Bu, kümelerin çalışmasını kolaylaştırarak, işleç araçlarının sezgisel bir koleksiyonunu ve Hadoop’un karmaşıklığı gizleyen sağlam bir API kümesini içerir. Linux tabanlı HDInsight kümeleri hem Ambari web kullanıcı arabirimi hem de Ambari REST API sağlarken, Windows tabanlı kümeler REST API’nin bir alt kümesini sağlar. HDInsight kümelerinde Ambari Görünümleri eklenti kullanıcı arabirimi özelliklerine imkan tanır.

Bkz. [Ambari kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md) (yalnızca Linux) [ Ambari API kullanarak HDInsight’ta Hadoop kümelerini izleme](hdinsight-monitor-use-ambari-api.md) ve <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API başvurusu</a>.

### <a name="avro"></a>Avro (Avro için Microsoft .NET kitaplığı)

Avro için Microsoft .NET kitaplığı, Microsoft .NET ortamının seri hale getirilmesi amacıyla Apache Avro sıkıştırılmış ikili veri değişim biçimini uygular. Bir dilde seri hale getirilmiş olan verilerin başka bir dilde okunabileceği anlamına gelen, dil birlikte çalışabilirliğini sağlayan bir dilden bağımsız şemasını tanımlamak amacıyla <a target="_blank" href="http://www.json.org/">JavaScript Nesne Gösterimi (JSON)</a> kullanır. Biçim hakkında ayrıntılı bilgi <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Özellikler</a> adresinde bulunabilir.
Avro dosyalarının biçimi, dağıtılmış MapReduce programlama modelini destekler. Dosyaların “bölünebilir” olması, bir dosyada herhangi bir noktayı arayabileceğiniz ve belirli bir bloktan itibaren okumaya başlayabileceğiniz anlamına gelir Nasıl yapacağınızı öğrenmek için bkz. [Avro için Microsoft .NET kitaplığı ile verileri seri hale getirme](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Hadoop Dağıtılmış Dosya Sistemi (HDFS), MapReduce ve YARN ile, Hadoop ekosisteminin çekirdeği olan bir dağıtılmış dosya sistemidir. HDFS, HDInsight’taki Hadoop kümeleri için standart dosya sistemidir.

### <a name="hive"></a>Hive & HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache Hive</a>, HiveQL adında SQL benzeri bir dil kullanarak dağıtılmış depolamada büyük veri kümelerini sorgulamanızı ve yönetmenizi sağlayan, Hadoop üzerinde kurulu bir veri ambarı yazılımıdır. Hive, Pig gibi, MapReduce üzerinde bir soyutlamadır. Çalıştırdığınızda, Hive sorguları bir dizi MapReduce işine çevirir. Hive, Pig’e göre, bir ilişkisel veritabanı yönetim sistemine kavramsal olarak daha yakındır ve bu nedenle daha fazla yapılandırılmış veriyle kullanıma uygundur. Yapılandırılmamış veriler için, Pig daha iyi bir seçimdir. Bkz: [HDInsight’ta Hadoop ile Hive kullanma](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a>, kullanıcılara ilişkisel bir veri görünümü sunan bir tablo ve Hadoop için veri yönetimi katmanıdır. HCatalog’da, dosyalara bir Hive SerDe’ni (seri hale getirici-seri halden çıkarıcı) yazılabileceği her biçimde okuyabilir ve yazabilirsiniz.

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a>, Hadoop üzerinde çalışan Machine Learning algoritmalarının ölçeklenebilir bir kitaplığıdır. İstatistik ilkelerini kullanarak, Machine Learning uygulamaları sistemlere verilerden öğrenmeyi ve gelecekteki davranışı belirlemek için geçmiş çıktıları kullanmayı öğretir. Bkz. [Hadoop’ta Mahout kullanarak film önerileri oluşturma](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce, paralel büyük veri kümelerini toplu olarak işlemek için uygulamalara yazmaya yönelik eski yazılım altyapısıdır. Bir MapReduce işi, büyük veri kümelerini böler ve verileri işleme için anahtar-değer çiftleri halinde düzenler.

[YARN](#yarn), Hadoop yeni nesil kaynak yöneticisi ve uygulama altyapısıdır ve MapReduce 2.0 olarak adlandırılır. MapReduce işleri YARN üzerinde çalışır.

MapReduce hakkında daha fazla bilgi için bkz. Hadoop Wiki'de <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a>.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a>, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir. Bu Hadoop yığını ile tümleştirilir ve MapReduce, Pig, Hive ve Sqoop için Hadoop işlerini destekler. Ayrıca, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanılabilir. Bkz. [Hadoop ile Oozie kullanma](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a>, HBase üzerinde ilişkisel veritabanı katmanıdır. Phoenix, kullanıcıların SQL tablolarını doğrudan sorgulamasına ve yönetmesine olanak tanıyan bir JDBC sürücüsü içerir. Phoenix, MapReduce kullanmak yerine, sorguları ve diğer ifadeleri yerel NoSQL API çağrılarına çevirir, böylece NoSQL depoları üstünde daha hızlı uygulamalar sağlar. Bkz. [HBase kümeleriyle Apache ve SQuirreL kullanma](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a>, Pig Latin adlı basit bir betik dilini kullanarak çok büyük veri kümelerinde karmaşık MapReduce dönüşümleri gerçekleştirmenize olanak tanıyan yüksek düzeyli bir platformdur. Pig, Hadoop’ta çalışacakları şekilde, Pig Latin betiklerini çevirir. Pig Latin’i genişletmek için Kullanıcı Tanımlı İşlevler (UDF) oluşturabilirsiniz. Bkz. [Hadoop ile Pig kullanma](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a>, aktarımları Hadoop ile SQL gibi ilişkisel veritabanları veya diğer yapılandırılmış veri depoları arasında mümkün olduğunca verimli şekilde toplu veri aktaran araçtır. Bkz: [Hadoop ile Sqoop kullanma](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a>, Hadoop YARN üzerinde, gene ver işlemenin karmaşık, döngüsel olmayan grafiklerini yürüten bir uygulama altyapısıdır. Bu, ölçekte daha verimli çalışmasıyla, Hive gibi veri yoğun işlemlere izin veren MapReduce altyapısının daha esnek ve güçlü bir ardılıdır. Bkz. [Hive ve HiveQL Kullanımında "Gelişmiş performans için Apache Tez kullanma"](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN, yeni nesil MapReduce’dür (MapReduce 2.0 veya MRv2) ve daha yüksek ölçeklenebilirlik ve gerçek zamanlı işleme ile MapReduce toplu işlemler ötesinde veri işleme senaryolarını destekler. YARN kaynak yönetimi ve dağıtılmış uygulama altyapısı sağlar. MapReduce işleri YARN üzerinde çalışır.

YARN hakkında bilgi edinmek için bkz. <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a>, paylaşılan hiyerarşik veri kayıtları ad alanı koordinatları (znode) yoluyla büyük dağıtılan sistemlerde işlemleri koordine eder. Znode’lar işlemleri koordine etmek için gerekli az miktarda meta bilgiler içerir: durum, konum, yapılandırma vb.

## HDInsight’ta programlama dilleri

HDInsight kümeleri--Hadoop, HBase, Storm ve Spark kümeleri--çeşitli programlama dillerini destekler ancak bunların bazı varsayılan olarak yüklü değildir. Varsayılan olarak yüklü olmayan kitaplıklar, modüller veya paketler için, bileşeni yüklemek üzere betik eylemi kullanın. Bkz: [HDInsight ile Betik Eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).

### Varsayılan programlama dili desteği 

Varsayılan olarak, HDInsight kümeleri aşağıdakileri destekler:

* Java

* Python

Betik eylemleri kullanılarak ek diller yüklenebilir: [HDInsight ile betik eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).

### Java sanal makine (JVM) dilleri

Java dışındaki birçok dil Java sanal makinesi (JVM) kullanılarak çalıştırılabilir; ancak, bu dillerden bazılarını çalıştırmak kümeye yüklü ek bileşenler gerektirebilir.

Bu JVM tabanlı diller HDInsight kümelerinde desteklenir:

* Clojure

* Jython (Java için Python)

* Scala

### Hadoop’a özgü diller

HDInsight kümeleri için Hadoop ekosistemine özgü aşağıdaki diller için destek sağlar:

* Pig işleri için Pig Latin

* Hive işleri için HiveQL ve SparkSQL


## <a name="advantage"></a>Bulutta Hadoop avantajları

Azure bulut ekosisteminin bir parçası olarak, HDInsight’ta Hadoop birçok avantaj sunar, bunlar:

* Otomatik Hadoop kümeleri hazırlama. HDInsight kümelerini oluşturmak Hadoop kümelerinin el ile yapılandırmaktan çok daha kolaydır. Ayrıntılar için bkz. [HDInsight Hadoop kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md).

* Resim durumu Hadoop bileşenleri. Ayrıntılar için bkz. [HDInsight’ta Hadoop bileşenleri, sürümleri ve hizmet teklifleri][component-versioning].

* Yüksek küme kullanılabilirliği ve güvenilirliği. Bkz. [HDInsight’ta Hadoop kümelerinin kullanılabilirliği ve güvenilirliğin](hdinsight-high-availability-linux.md).

* Hadoop uyumlu bir seçenek olan Azure Blob Storage ile verimli ve ekonomik veri depolama. Ayrıntılar için bkz. [HDInsight’ta Hadoop ile Azure Blob Storage kullanma](hdinsight-hadoop-use-blob-storage.md).

* [Web apps](https://azure.microsoft.com/documentation/services/app-service/web/) ve [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/) dahil diğer Azure hizmetleriyle tümleştirme.

* HDInsight kümelerini çalıştırmak için ek sanal makine boyutları ve türleri. Ayrıntılar için bkz. [HDInsight’ta Hadoop bileşenleri, sürümleri ve hizmet teklifleri][component-versioning].

* Küme ölçeklendirme. Küme ölçeklendirme, silmenize ya da yeniden oluşturmanıza gerek kalmadan, çalışan bir HDInsight kümesine ait düğümlerin sayısını değiştirmenizi sağlar.

* Virtual Network desteği. HDInsight kümeleri, bulut kaynaklarının yalıtımını ya da bulut kaynaklarını veri merkezinizdekilere bağlayan hibrit senaryoları desteklemek için Azure Virtual Network ile birlikte kullanılabilir.

* Düşük giriş maliyeti. [Ücretsiz deneme sürümü](https://azure.microsoft.com/free/) başlatın ya da [HDInsight fiyatlandırma ayrıntılarını](https://azure.microsoft.com/pricing/details/hdinsight/) inceleyin.

HDInsight’ta Hadoop’un avantajları hakkında daha fazla bilgi için bkz. [HDInsight için Azure özellikleri sayfası ][marketing-page]

## HDInsight Standart ve HDInsight Premium

HDInsight, Standart ve Premium olmak üzere, iki kategoride büyük veri bulutu teklifleri sunar. HDInsight Standart, kuruluşların kendi büyük veri iş yüklerini çalıştırmak için kullanabileceği kurumsal ölçekte küme sağlar. HDInsight Premium, bunun üzerine kurulur ve HDInsight kümesi için gelişmiş bir çözümsel ve güvenlik özellikleri sağlar. Daha fazla bilgi için bkz. [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Büyük veri analizi, Hadoop ve HDInsight hakkında daha fazla bilgi için kaynaklar.

Bu, bulutta Hadoop ve büyük verileri çözümlemeye ilişkin girişi aşağıdaki kaynaklarla geliştirin.


### HDInsight için Hadoop belgeleri

* [HDInsight belgeleri](https://azure.microsoft.com/documentation/services/hdinsight/): Makaleler, videolar ve diğer kaynaklara bağlantılarla Azure HDInsight’ta Hadoop için belgeler sayfası.

* [HDInsight’ta Hadoop kullanmaya başlama](hdinsight-hadoop-linux-tutorial-get-started.md): HDInsight Hadoop kümeleri hazırlamak ve örnek Hive sorguları çalıştırmak için hızlı başlangıç öğreticisi.

* [HDInsight’ta Spark kullanmaya başlama](hdinsight-apache-spark-jupyter-spark-sql.md): Spark kümesi oluşturmak ve etkileşimli Spark SQL sorguları çalıştırmak için hızlı başlangıç öğreticisi.

* [HDInsight’ta R Sunucusu Kullanma](hdinsight-hadoop-r-server-get-started.md): HDInsight Premium’da R Sunucusu kullanmaya başlayın.

* [HDInsight kümeleri hazırlama](hdinsight-hadoop-provision-linux-clusters.md): Azure Portal, Azure CLI veya Azure PowerShell aracılığıyla bir HDInsight Hadoop kümesi hazırlamayı öğrenin.


### Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: Büyük veri kümelerinin bilgisayar kümelerinde dağıtılmış işlemesine olanak tanıyan bir altyapı olan Apache Hadoop yazılım kitaplığı hakkında daha fazla bilgi edinin.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: Hadoop uygulamaları tarafından kullanılan birincil depolama sistemi olan Hadoop Dağıtılmış Dosya Sistemi yapısı ve tasarımı hakkında daha fazla bilgi edinin.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce Öğreticisi</a>: İşlem düğümlerinde büyük miktarlarda verileri paralel olarak hızlı şekilde işleyen Hadoop uygulamalarını yazmaya yönelik programlama altyapısı hakkında daha fazla bilgi edinin.


### Microsoft iş zekası

Excel, PowerPivot, SQL Server Analysis Services ve SQL Server Reporting Services - gibi bilinen iş zekası (BI) araçları - Power Query eklentisini veya Microsoft Hive ODBC sürücüsünü kullanarak HDInsight ile tümleştirilmiş verileri alır, çözümler ve raporlar.

Bu BI araçları, büyük veri çözümlemede size yardımcı olabilir:

* [Excel’i Power Query ile Hadoop’a Bağlama](hdinsight-connect-excel-power-query.md): Excel için Microsoft Power Query kullanarak HDInsight kümenizle ilişkili verileri depolayan Azure Storage hesabına Excel’i bağlamayı öğrenin. Windows iş istasyonu gereklidir. Windows veya Linux tabanlı küme ile çalışır.

* [Excel’i Microsoft Hive ODBC Sürücüsü ile Hadoop’a Bağlama](hdinsight-connect-excel-hive-ODBC-driver.md): Microsoft Hive ODBC Sürücüsü ile verileri HDInsight’tan içeri aktarmayı öğrenin. Windows iş istasyonu gereklidir. Windows veya Linux tabanlı küme ile çalışır.

* [Microsoft Bulut Platformu](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Office 365 için Power BI hakkında bilgi edinin, SQL Server deneme sürümünü indirin ve SharePoint Server 2013 ve SQL Server BI’ı ayarlayın.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/



<!--HONumber=sep16_HO2-->


