---
title: "HDInsight Üzerinde Apache Storm’a Giriş | Microsoft Belgeleri"
description: "HDInsight Üzerinde Apache Storm hakkında bilgi alın."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 5e6ffbb8f1373f7170f87ad0e345a63cc20f08dd
ms.openlocfilehash: 0aa2a7075f64b353f6b052ab6b973a06622a9339
ms.lasthandoff: 03/24/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>HDInsight üzerinde Apache Storm’a giriş: Hadoop için gerçek zamanlı analiz

HDInsight üzerinde Apache Storm, Azure üzerinde dağıtılmış, gerçek zamanlı analiz çözümleri oluşturmanıza imkan tanır.

Apache Storm verileri Hadoop ile gerçek zamanlı olarak işlemenize imkan tanıyan dağıtılmış, hataya dayanıklı ve açık kaynaklı bir hesaplama sistemidir. Storm çözümleri ayrıca ilk seferde başarılı bir şekilde işlenmemiş verileri yeniden yürütme özelliğiyle birlikte verilerin garantili işlenmesini sağlayabilir.

## <a name="why-use-storm-on-hdinsight"></a>HDInsight üzerinde Storm neden kullanılmalıdır

HDInsight üzerinde Apache Storm, Azure ortamına tümleştirilmiş yönetilen bir kümedir. HDInsight üzerindeki Storm ve diğer Hadoop bileşenleri, Hortonworks Data Platform (HDP) temellidir ancak kümenin işletim sistemi Ubuntu'dur (bir Linux dağıtımı). Bu yapılandırma, Hadoop ekosistemindeki popüler araçlar ve hizmetlerle uyumlu bir platform sunar.

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın Kullanım Dışı Bırakılması](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

HDInsight üzerinde Apache Storm aşağıdaki önemli avantajları sunar:

* %99,9 çalışma zamanı SLA'sı ile yönetilen bir hizmet olarak çalışır.

* Oluşturma sırasında veya sonrasında kümede betik çalıştırarak kolay özelleştirme. Daha fazla bilgi için bkz. [HDInsight kümelerini betik eylemi kullanarak özelleştirme](hdinsight-hadoop-customize-cluster-linux.md).

* Seçtiğiniz dili kullanın: Storm bileşenleri **Java**, **C#** ve **Python** gibi çeşitli dillerde yazılabilir.

  * C# topolojisi geliştirme, yönetme ve izleme için HDInsight ile Visual Studio tümleştirmesi. Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

  * **Trident** Java arabirimini destekler. Bu arabirim iletilerin "tam bir kez" işlenmesini, "işlemsel" veri deposu kalıcılığını ve bir dizi ortak akış analizi işlemlerini destekleyen Storm topolojileri oluşturmanızı sağlar.

* Kümenin ölçeğini kolayca büyütün veya küçültün: Çalışan Storm topolojilerini etkilemeden çalışan düğümleri ekleyin veya kaldırın.

* Aşağıdaki Azure Hizmetleri ile tümleştirin:

    * Event Hubs
    * Sanal Ağ
    * SQL Database
    * Azure Storage
    * DocumentDB.

  * Azure Sanal Ağ kullanarak birden fazla HDInsight kümesinin özelliklerini güvenli bir şekilde birleştirin: HDInsight, HBase veya Hadoop kümeleri kullanan analitik işlem hatları oluşturun.

Gerçek zamanlı analiz çözümleri için Apache Storm kullanan şirketlerin listesi için bkz. [Apache Storm Kullanan Şirketler](https://storm.apache.org/documentation/Powered-By.html).

Storm kullanmaya başlamak için bkz. [HDInsight Üzerinde Storm ile çalışmaya başlama][gettingstarted].

### <a name="ease-of-creation"></a>Oluşturma kolaylığı

HDInsight kümesinde dakikalar için yeni bir Storm sağlayabilirsiniz. Küme adı, boyutu, yönetici hesabı ve depolama hesabı belirtin. Azure, örnek topolojileri ve web yönetimi panosu ile birlikte kümeyi oluşturur.

> [!NOTE]
> Storm kümelerini ayrıca [Azure CLI](../cli-install-nodejs.md) veya [Azure PowerShell](/powershell/azureps-cmdlets-docs) kullanarak da sağlayabilirsiniz.

İsteği gönderdikten sonraki 15 dakika içinde ilk gerçek zamanlı analiz işlem hattınız için çalışır ve hazır durumda olan yeni bir Storm kümesine sahip olursunuz.

### <a name="ease-of-use"></a>Kullanım kolaylığı

* __Secure Shell bağlantısı__: HDInsight kümenizin baş düğümlerine internet üzerinden SSH kullanarak erişebilirsiniz. SSH sayesinde doğrudan küme üzerinde komut çalıştırabilirsiniz.

  Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Web bağlantısı__: HDInsight kümeleri Ambari web kullanıcı arabirimini sunar. Ambari web kullanıcı arabirimini kullanarak kümenizdeki hizmetleri kolayca izleyebilir, yapılandırabilir ve yönetebilirsiniz. HDInsight üzerinde Storm'un sunduğu Storm kullanıcı arabirimi de çalışan Storm topolojilerini tarayıcınızdan izlemenizi ve yönetmenizi sağlar.

  Daha fazla bilgi için bkz. [HDInsight'ı Ambari Web Kullanıcı Arabirimini kullanarak yönetme](hdinsight-hadoop-manage-ambari.md) ve [Storm kullanıcı arabirimini kullanarak izleme ve yönetme](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui).

* __Azure PowerShell ve CLI__: Hem Azure PowerShell hem de Azure CLI, HDInsight ve diğer Azure hizmetleriyle çalışmak için istemci sisteminizde kullanabileceğiniz komut satırı yardımcı programları sunar.

* __Visual Studio tümleştirmesi__: Visual Studio için Data Lake Araçları, C# Storm topolojileri oluşturmak için kullanabileceğiniz proje şablonlarının yanı sıra HDInsight üzerinde Storm izlemeye yönelik araçları içerir. C# topolojilerinizi Visual Studio'dan oluşturabilir, dağıtabilir, izleyebilir ve yönetebilirsiniz.

  Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

* __Diğer Azure hizmetleriyle tümleştirme__

  * Microsoft, __Java__ geliştirmesi için, mümkün olduğu durumlarda diğer Azure hizmetleriyle tümleştirme amacıyla mevcut Storm bileşenlerini kullanır. Bazı durumlarda hizmete özgü bileşene veya çözüme ihtiyaç duyulabilir.

    * __Azure Data Lake Store__: Java tabanlı teknolojiler Data Lake Store'a Storm-HDFS bağlantısını `adl://` URI şemasıyla kullanarak erişebilir. Storm-HDFS bağlantısını kullanan bir örnek için bkz. [HDInsight'ta Azure Data Lake Store ile Apache Storm kullanma](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-write-data-lake-store).

    * __Azure Depolama__ (HDInsight için depolama çözümü olarak kullanıldığında): Java tabanlı topolojiler, Storm-HDFS bolt ile birlikte `wasb://` URI şeması kullanarak Azure Depolama’ya erişebilir.

    * __Azure Event Hubs__: Microsoft tarafından sağlanan EventHubSpout ve EventHubBolt bileşenleri kullanılarak erişilebilir. Java dilinde yazılmış olan bu bileşenler ayrı .jar dosyaları olarak sunulur.

    Java çözümü geliştirme hakkında daha fazla bilgi için bkz. [HDInsight'ta Storm için Java tabanlı bir topoloji geliştirme](hdinsight-storm-develop-java-topology.md).

  * __C#__ ile geliştirme yapmak istiyorsanız Azure hizmeti için .NET SDK'sını kullanabilirsiniz. Bazı durumlarda SDK, Linux (HDInsight 3.4 ve üzeri için ana işletim sistemi) üzerinde bulunmayan çerçevelere ihtiyaç duyabilir. Bu durumda C# çözümünüzün içindeki Java bileşenlerini kullanabilirsiniz.

    * __SQL DB__, __DocumentDB__, __EventHub__ ve __HBase__ ile çalışma örnekleri Visual Studio için Azure Data Lake Araçları'na şablon olarak eklenmiştir. Daha fazla bilgi için bkz. [HDInsight'ta Storm için C# topolojisi geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * __Azure Event Hubs__: Bir C# çözümündeki Java bileşenlerini kullanma örneği için bkz. [HDInsight'ta Strom ile Azure Event Hubs'tan gelen olayları işleme (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md).

    C# çözümü geliştirme hakkında daha fazla bilgi için bkz. [HDInsight'ta Storm için C# topolojisi geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="reliability"></a>Güvenilirlik

Apache Storm, veri analizi yüzlerce düğüme dağıldığında bile tüm gelen iletilerin her zaman tamamen işleneceğini garanti eder.

**Nimbus düğümü**, Hadoop JobTracker’a benzer bir işlevsellik sağlar ve **Zookeeper** aracılığıyla kümedeki diğer düğümlere görevler atar. Zookeeper düğümleri küme için eşgüdüm sağlar ve çalışan düğümleri üzerinde Nimbus ile **Supervisor** işlemi arasındaki iletişimi kolaylaştırır. Bir işleme düğümü devre dışı kalırsa Nimbus düğümü bilgilendirilir ve görevi ve ilişkili verileri başka bir düğüme atar.

Apache Storm için varsayılan yapılandırma yalnızca bir Nimbus düğümü içerir. HDInsight üzerindeki Storm iki Nimbus düğümü çalıştırır. Birincil düğüm başarısız olursa birincil düğüm kurtarılırken HDInsight kümesi ikincil düğüme geçiş yapar.

![Nimbus, zookeeper ve supervisor diyagramı](./media/hdinsight-storm-overview/nimbus.png)

### <a name="scale"></a>Ölçek

Oluşturma sırasında kümedeki düğüm sayısını belirtebilseniz de, iş yüküyle eşleşmesi için kümeyi büyütmek ya da küçültmek isteyebilirsiniz. Tüm HDInsight kümeleri, veriler işlenirken bile kümedeki düğüm sayısını değiştirmenize izin verir.

> [!NOTE]
> Ölçeklendirme aracılığıyla eklenen yeni düğümlerden yararlanmak için küme boyutu artırılmadan önce topolojileri yeniden dengelemeniz gerekir.

### <a name="support"></a>Destek

HDInsight üzerinde Storm 7 gün 24 saat kurumsal düzeyde tam destek ile birlikte gelir. HDInsight üzerinde Storm ayrıca %99,9 SLA’ya sahiptir. Diğer bir deyişle kümenin, sürenin en az %99,9’unda dış bağlantıya sahip olacağı garanti edilir.

## <a name="common-use-cases-for-real-time-analytics"></a>Gerçek zamanlı analiz için ortak kullanım durumları

HDInsight üzerinde Apache storm kullanabileceğiniz bazı yaygın senaryolar aşağıda verilmiştir. Gerçek senaryolar hakkında daha fazla bilgi için bkz. [Şirketler Storm’u nasıl kullanıyor?](https://storm.apache.org/documentation/Powered-By.html).

* Nesnelerin İnterneti (IoT)
* Sahtekarlık algılama
* Sosyal analiz
* Ayıklama, Dönüştürme, Yükleme (ETL)
* Ağ izleme
* Arama
* Mobil katılım

## <a name="how-is-data-in-hdinsight-storm-processed"></a>HDInsight Storm içindeki veriler nasıl işlenir?

Apache Storm, HDInsight veya Hadoop’ta alışkın olabileceğiniz MapReduce işlerinin yerine **topolojiler** çalıştırır. HDInsight kümesi üzerinde Storm iki tür düğüm içerir: **Nimbus** çalıştıran baş düğümler ve **Supervisor** çalıştıran çalışan düğümleri.

* **Nimbus**: Hadoop’taki JobTracker’a benzer şekilde kodu küme boyunca dağıtmaktan, görevleri sanal makinelere atamaktan ve hatalara karşı izlemekten sorumludur. HDInsight iki Nimbus düğümüne sahiptir, bu nedenle HDInsight üzerinde Storm için tek bir hata noktası yoktur
* **Supervisor**: Her bir çalışan düğümünün gözetmeni, düğüm üzerindeki **çalışan işlemlerini** başlatmaktan ve durdurmaktan sorumludur.
* **Çalışan işlemi**: Bir **topoloji** alt kümesi çalıştırır. Çalışan bir topoloji, küme içindeki çok sayıda çalışan işlemine dağıtılır.
* **Topoloji**: Veri **akışlarını** işleyen bir hesaplama grafiği tanımlar. MapReduce işlerinin aksine topolojiler siz durdurana kadar çalışır.
* **Akış**: Bağlantısız bir **tanımlama grubu** koleksiyonu. Akışlar **spout** ve **cıvatalar** ile oluşturulur ve **cıvatalar** tarafından kullanılır.
* **Tanımlama grubu**: Dinamik olarak yazılan değerlerin adlandırılmış listesi.
* **Spout**: Bir veri kaynağındaki verileri kullanır ve bir veya daha fazla **akış** yayar.

  > [!NOTE]
  > Çoğu durumda veriler Kafka veya Azure Event Hubs gibi bir kuyruktan okunur. Kuyruk, bir kesinti oluşursa verilerin kalıcı olmasını sağlar.

* **Cıvata**: **Akışları** kullanır, **tanımlama grupları** üzerinde işlemeyi gerçekleştirir ve **akışlar** yayabilir. Cıvatalar ayrıca kuyruk, HDInsight, HBase, blob veya diğer veri depoları gibi dış depolama alanlarına veri yazmaktan sorumludur.
* **Apache Thrift**: Ölçeklenebilir çapraz dil hizmet dağıtımına yönelik bir yazılım çerçevesi. C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk ve diğer diller arasında çalışan hizmetler oluşturmanıza imkan tanır.

Storm bileşenleri hakkında daha fazla bilgi için apache.org adresindeki [Storm öğreticisi][apachetutorial] bölümüne bakın.

## <a name="what-programming-languages-can-i-use"></a>Hangi programlama dillerini kullanabilirim?

### <a name="c35"></a>C&#35;

Visual Studio için Data Lake Araçları, .NET geliştiricilerinin C# dilinde topoloji tasarlayıp uygulamasına imkan tanır. Ayrıca Java ve C# bileşenlerini kullanan karma topolojiler de oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="java"></a>Java

Karşılaştığınız çoğu Java örneği düz Java veya Trident’tir. Trident; birleştirme, toplama, gruplandırma ve filtreleme gibi işleri kolaylaştıran üst düzey bir soyutlamadır. Ancak, Trident toplu tanımlama grupları üzerinde hareket ederken, ham Java çözümü bir akışı her seferinde bir tanımlama grubu ile işler.

Trident hakkında daha fazla bilgi için apache.org sayfasındaki [Trident öğreticisi](https://storm.apache.org/documentation/Trident-tutorial.html) bölümüne bakın.

Java ve Trident topolojilerinin örnekleri için [örnek Storm topolojileri listesine](hdinsight-storm-example-topology.md) veya HDInsight kümenizdeki storm başlangıç örneklerine bakın.

Storm başlangıç örnekleri, HDInsight kümenizin **/usr/hdp/current/storm-client/contrib/storm-starter** dizininde bulunur.

### <a name="python"></a>Python

Python bileşenlerini kullanma örneği için bkz. [HDInsight üzerinde Python kullanarak Storm topolojileri geliştirme](hdinsight-storm-develop-python-topology.md).

## <a name="what-are-some-common-development-patterns"></a>Bazı ortak geliştirme desenleri nelerdir?

### <a name="guaranteed-message-processing"></a>Garantili ileti işleme

Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir Storm uygulaması en az bir kez işlemeyi, Trident ise tam bir kez işlemeyi garanti edebilir.

Daha fazla bilgi için apache.org sayfasındaki [Veri işleme garantileri](https://storm.apache.org/about/guarantees-data-processing.html) bölümüne bakın.

### <a name="ibasicbolt"></a>IBasicBolt

Bir girdi tanımlama grubunu okuma, sıfır veya daha fazla tanımlama grubu yayma ve yürütme yönteminin hemen sonrasında girdi tanımlama grubunu onaylama deseni yaygındır. Storm bu deseni otomatik hale getirmek için [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) arabirimini kullanır.

### <a name="joins"></a>Birleştirme

Uygulamalar arasına veri akışları nasıl katılır? Örneğin, yeni birden fazla akıştaki her bir tanımlama grubunu yeni bir akışta birleştirebilir veya yalnızca toplu tanımlama gruplarını belirli bir pencere için birleştirebilirsiniz. Her iki işlem de tanımlama gruplarının cıvatalara nasıl yönlendirileceğini tanımlayan [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) yöntemiyle gerçekleştirilebilir.

Aşağıdaki Java örneğinde fieldsGrouping, "1", "2" ve "3" bileşenlerinden kaynaklanan tanımlama gruplarını **MyJoiner** cıvatasına yönlendirmek için kullanılır.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batching"></a>Toplu İşleme

Storm her X saniyede bir toplu iş yaymak üzere kullanılabilecek, "değer çizgisi tanımlama grubu" olarak bilinen bir iç zamanlama mekanizmasını sağlar.

Bir C# bileşeninden değer çizgisi tanımlama grubu kullanımı örneği için bkz. [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

Trident kullanıyorsanız bu durum tanımlama grubu toplu işlerini işlemeye bağlıdır.

### <a name="caching"></a>Önbelleğe alma

Bellek içi önbelleğe alma, sık kullanılan varlıkları bellekte tuttuğu için çoğunlukla işlemeyi hızlandırmaya yönelik bir mekanizma olarak kullanılır. Bir topoloji birden fazla düğüme ve her bir düğüm içindeki birden fazla işleme dağıtıldığı için, [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) kullanmayı düşünmeniz gerekir. `fieldsGrouping`, önbellek araması için kullanılan alanları içeren tanımlama gruplarının her zaman aynı işleme yönlendirilmesini sağlar. Bu gruplandırma işlevi, süreçler arasında önbellek girişlerinin yinelenmesini önler.

### <a name="streaming-top-n"></a>İlk N akışı

Topolojiniz "ilk N" değeri hesaplamaya bağlıysa ilk N değeri paralel olarak hesaplamanız ve ardından bu hesaplamaların çıktısını genel bir değerde birleştirmeniz gerekir. Paralel işleme için bu işlem, alana göre yönlendirmek ve ardından ilk N değeri genel olarak belirleyen bir bolt’a yönlendirmek üzere [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) kullanılarak yapılabilir.

"İlk N" değeri hesaplama örneği için [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java) örneğine bakın.

## <a name="what-type-of-logging-does-storm-use"></a>Storm ne tür bir günlük kullanır?

Storm bilgileri günlüğe kaydetmek için Apache Log4j kullanır. Varsayılan olarak, büyük miktarlarda veriler günlüğe kaydedilir ve bilgilerin sıralanması zor olabilir. Günlüğe kaydetme davranışını denetlemek üzere Storm topolojinizin bir parçası olarak günlük yapılandırma dosyası ekleyebilirsiniz.

Günlüğün nasıl yapılandırılacağını gösteren örnek bir topoloji için HDInsight üzerinde Storm için [Java tabanlı WordCount](hdinsight-storm-develop-java-topology.md) örneğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Storm ile gerçek zamanlı analiz çözümleri hakkında daha fazla bilgi edinin:

* [HDInsight'ta Storm'u kullanmaya başlama][gettingstarted]
* [HDInsight üzerinde Storm için örnek topolojiler](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

