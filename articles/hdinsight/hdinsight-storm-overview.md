---
title: Apache Storm nedir - Azure HDInsight | Microsoft Docs
description: "Apache Storm, veri akışlarını gerçek zamanlı olarak işlemenize olanak tanır. Azure HDInsight, Azure bulutu üzerinde Storm kümelerini kolayca oluşturmanıza olanak tanır. Visual Studio ile C# kullanarak Storm çözümleri oluşturabilir ve sonra HDInsight Storm kümelerinize dağıtabilirsiniz."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "apache storm kullanım örnekleri,storm kümesi,apache storm nedir"
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 073672f1223313938baedee027072cb96062294b
ms.contentlocale: tr-tr
ms.lasthandoff: 08/04/2017

---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Azure HDInsight’ta Apache Storm nedir?

[Apache Storm](http://storm.apache.org/); dağıtılmış, hataya dayanıklı, açık kaynaklı bir hesaplama sistemidir. Veri akışlarını Hadoop ile gerçek zamanlı olarak işlemek için Storm’u kullanabilirsiniz. Storm çözümleri ayrıca ilk seferde başarılı bir şekilde işlenmemiş verileri yeniden yürütme özelliğiyle birlikte verilerin garantili işlenmesini sağlayabilir.

HDInsight üzerinde Storm aşağıdaki önemli avantajları sunar:

* Yüzde 99,9 çalışma zamanı SLA'sı ile yönetilen bir hizmet olarak çalışır.

* Oluşturma sırasında veya sonrasında Storm kümesinde betik çalıştırarak kolay özelleştirmeyi destekler. Daha fazla bilgi için bkz. [HDInsight kümelerini betik eylemi kullanarak özelleştirme](hdinsight-hadoop-customize-cluster-linux.md).

* Çeşitli diller kullanır. Storm bileşenlerini Java, C# ve Python gibi dilediğiniz bir dilde yazabilirsiniz.

    * C# topolojisi geliştirme, yönetme ve izleme için HDInsight ile Visual Studio’yu tümleştirir. Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Trident Java arabirimini destekler. İletilerin tam olarak bir kez işlenmesini, işlemsel veri deposu kalıcılığını ve sık kullanılan Stream Analytics işlemlerini destekleyen Storm topolojileri oluşturabilirsiniz.

*  Storm kümelerinin ölçeğini kolayca artırın veya azaltın. Çalışan Storm topolojilerini etkilemeden çalışan düğümleri ekleyebilir veya kaldırabilirsiniz.

* Aşağıdaki Azure Hizmetleri ile tümleştirilir:

    * Azure Event Hubs

    * Azure Sanal Ağ

    * Azure SQL Database

    * Azure Storage

    * Azure Cosmos DB

* Sanal Ağ kullanarak birden fazla HDInsight kümesinin özelliklerini güvenli bir şekilde birleştirir. Storm, Kafka, Spark, HBase veya Hadoop kümeleri kullanan analitik işlem hatları oluşturabilirsiniz.

Gerçek zamanlı analiz çözümleri için Apache Storm kullanan şirketlerin listesi için bkz. [Apache Storm Kullanan Şirketler](https://storm.apache.org/documentation/Powered-By.html).

Storm kullanmaya başlamak için bkz. [HDInsight Üzerinde Storm ile çalışmaya başlama][gettingstarted].

## <a name="how-does-storm-work"></a>Storm nasıl çalışır?

Storm, alışkın olabileceğiniz MapReduce işlerinin yerine topolojiler çalıştırır. Storm topolojileri döngüsel olmayan yönlü grafikte (DAG) düzenlenmiş birden fazla bileşenden oluşur. Veriler grafikteki bileşenler arasında akar. Her bileşen bir veya daha fazla veri akışı kullanır ve isteğe bağlı olarak bir veya daha fazla akış yayar. Aşağıdaki diyagram, temel bir sözcük sayısı topolojisindeki bileşenler arasında verilerin nasıl aktığını gösterir:

![Bileşenlerin bir Storm topolojisinde nasıl düzenlendiğini gösteren örnek](./media/hdinsight-storm-overview/example-apache-storm-topology-diagram.png)

* Spout bileşenleri, verileri bir topolojiye getirir. Bu bileşenler topolojiye bir veya daha fazla akış yayar.

* Bolt bileşenleri, spout veya diğer boltlardan yayılan akışları kullanır. Boltlar topolojiye isteğe bağlı olarak akışlar yayabilir. Boltlar ayrıca HDFS, Kafka veya HBase gibi dış hizmetlere veya depolama alanlarına veri yazmaktan sorumludur.

## <a name="ease-of-creation"></a>Oluşturma kolaylığı

HDInsight üzerinde dakikalar için yeni bir Storm kümesi sağlayabilirsiniz. Storm kümesi oluşturma hakkında daha fazla bilgi için bkz. [HDInsight’ta Storm kullanmaya başlama](hdinsight-apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Kullanım kolaylığı

* __Secure Shell (SSH) bağlantısı__: Storm kümenizin baş düğümlerine İnternet üzerinden SSH kullanarak erişebilirsiniz. SSH kullanarak komutları doğrudan kümeniz üzerinde çalıştırabilirsiniz.

  Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Web bağlantısı__: Tüm HDInsight kümeleri Ambari web kullanıcı arabirimini sunar. Ambari web kullanıcı arabirimini kullanarak kümenizdeki hizmetleri kolayca izleyebilir, yapılandırabilir ve yönetebilirsiniz. Storm kümeleri ayrıca Storm Kullanıcı Arabirimini sağlar. Storm kullanıcı arabirimini kullanarak, çalışan Storm topolojilerini tarayıcınızdan izleyip yönetebilirsiniz.

  Daha fazla bilgi için [HDInsight'ı Ambari Web Kullanıcı Arabirimini kullanarak yönetme](hdinsight-hadoop-manage-ambari.md) ve [Storm kullanıcı arabirimini kullanarak izleme ve yönetme](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui) belgelerine bakın.

* __Azure PowerShell ve Azure CLI__: Hem Azure PowerShell hem de CLI, HDInsight ve diğer Azure hizmetleriyle çalışmak için istemci sisteminizde kullanabileceğiniz komut satırı yardımcı programları sunar.

* __Visual Studio tümleştirmesi__: Visual Studio için Azure Data Lake Araçları, SCP.Net çerçevesi kullanarak C# Storm topolojileri oluşturmaya yönelik proje şablonları içerir. Data Lake Araçları ayrıca HDInsight üzerinde Storm ile çözümleri dağıtma, izleme ve yönetmeye yönelik araçlar sağlar.

  Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Diğer Azure hizmetleriyle tümleştirme

* __Azure Data Lake Store__: Data Lake Store’u bir Storm kümesi ile kullanmaya ilişkin örnek için bkz. [HDInsight’ta Azure Data Lake Store ile Apache Storm kullanma](hdinsight-storm-write-data-lake-store.md).

* __Event Hubs__: Event Hubs’ı bir Storm kümesi ile kullanma örneği için aşağıdaki belgelere bakın:

    * [HDInsight üzerinde Storm için Java tabanlı topoloji geliştirme](hdinsight-storm-develop-java-topology.md)

    * [HDInsight üzerinde Storm ile Azure Event Hubs’tan olay işleme (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md)

* __SQL Veritabanı__, __Cosmos DB__, __Event Hubs__ ve __HBase__: Visual Studio için Data Lake Araçlarına şablon örnekleri eklenmiştir. Daha fazla bilgi için bkz. [HDInsight'ta Storm için C# topolojisi geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="reliability"></a>Güvenilirlik

Apache Storm, veri analizi yüzlerce düğüme dağıldığında bile tüm gelen iletilerin her zaman tamamen işleneceğini garanti eder.

Nimbus düğümü, Hadoop JobTracker’a benzer bir işlevsellik sağlar ve Zookeeper aracılığıyla kümedeki diğer düğümlere görevler atar. Zookeeper düğümleri küme için eşgüdüm sağlar ve çalışan düğümleri üzerinde Nimbus ile Supervisor işlemi arasındaki iletişimi kolaylaştırır. Bir işleme düğümü devre dışı kalırsa Nimbus düğümü bilgilendirilir ve görevi ve ilişkili verileri başka bir düğüme atar.

Apache Storm kümeleri için varsayılan yapılandırma yalnızca bir Nimbus düğümü içerir. HDInsight üzerindeki Storm iki Nimbus düğümü sağlar. Birincil düğüm başarısız olursa birincil düğüm kurtarılırken Storm kümesi ikincil düğüme geçiş yapar. Aşağıdaki diyagramda HDInsight üzerinde Storm için görev akışı yapılandırması gösterilmektedir:

![Nimbus, zookeeper ve supervisor diyagramı](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Ölçek

Çalışan düğümleri ekleyerek veya kaldırarak HDInsight kümeleri ölçeklendirilebilir. Bu işlem, veriler işlenirken gerçekleştirilebilir.

> [!IMPORTANT]
> Ölçeklendirme aracılığıyla eklenen yeni düğümlerden yararlanmak için küme boyutu artırılmadan önce Storm topolojilerini yeniden dengelemeniz gerekir.

## <a name="support"></a>Destek

HDInsight üzerinde Storm kurumsal düzeyde kesintisiz tam destek ile birlikte gelir. HDInsight üzerinde Storm ayrıca yüzde 99,9 SLA’ya sahiptir. Diğer bir deyişle Storm kümesinin, sürenin en az yüzde 99,9’unda dış bağlantıya sahip olacağı garanti edilir.

Daha fazla bilgi için bkz. [Azure desteği](https://azure.microsoft.com/support/options/).

## <a name="apache-storm-use-cases"></a>Apache Storm kullanım örnekleri

HDInsight üzerinde Storm kullanabileceğiniz bazı yaygın senaryolar aşağıda verilmiştir:

* Nesnelerin İnterneti (IoT)
* Sahtekarlık algılama
* Sosyal analiz
* Ayıklama, dönüşüm ve yükleme (ETL)
* Ağ izleme
* Arama
* Mobil katılım

Gerçek senaryolar hakkında daha fazla bilgi için [Şirketler Storm’u nasıl kullanıyor?](https://storm.apache.org/documentation/Powered-By.html) belgesine bakın.

## <a name="development"></a>Geliştirme

.NET geliştiricileri, Visual Studio için Data Lake Araçları kullanarak C# dilinde topolojiler tasarlayıp uygulayabilir. Ayrıca Java ve C# bileşenlerini kullanan karma topolojiler de oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Storm için C# topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Ayrıca seçtiğiniz IDE’yi kullanarak Java çözümleri geliştirebilirsiniz. Daha fazla bilgi için bkz. [HDInsight'ta Storm için Java topolojileri geliştirme](hdinsight-storm-develop-java-topology.md).

Storm bileşenleri geliştirmek için Python da kullanılabilir. Daha fazla bilgi için bkz. [HDInsight’ta Python kullanarak Storm topolojileri geliştirme](hdinsight-storm-develop-python-topology.md).

## <a name="common-development-patterns"></a>Yaygın geliştirme desenleri

### <a name="guaranteed-message-processing"></a>Garantili ileti işleme

Apache Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir Storm uygulaması en az bir kez işlemeyi, Trident ise tam bir kez işlemeyi garanti edebilir.

Daha fazla bilgi için apache.org sayfasındaki [Veri işleme garantileri](https://storm.apache.org/about/guarantees-data-processing.html) bölümüne bakın.

### <a name="ibasicbolt"></a>IBasicBolt

Bir girdi tanımlama grubunu okuma, sıfır veya daha fazla tanımlama grubu yayma ve yürütme yönteminin hemen sonrasında girdi tanımlama grubunu onaylama deseni yaygındır. Storm bu deseni otomatik hale getirmek için [IBasicBolt](https://storm.apache.org/releases/1.0.3/javadocs/org/apache/storm/topology/IBasicBolt.html) arabirimini kullanır.

### <a name="joins"></a>Birleştirme

Uygulamalar arasına veri akışları nasıl katılır? Örneğin, yeni birden fazla akıştaki her bir tanımlama grubunu yeni bir akışta birleştirebilir veya yalnızca toplu tanımlama gruplarını belirli bir pencere için birleştirebilirsiniz. Her iki durumda da katılım, [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanılarak gerçekleştirilebilir. Alan gruplandırma, tanımlama gruplarının boltlara nasıl yönlendirileceğini tanımlamanın bir yoludur.

Aşağıdaki Java örneğinde fieldsGrouping, "1", "2" ve "3" bileşenlerinden kaynaklanan tanımlama gruplarını MyJoiner boltuna yönlendirmek için kullanılır:

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batches"></a>Toplu işler

Apache Storm "değer çizgisi tanımlama grubu" olarak bilinen bir iç zamanlama mekanizmasını sağlar. Değer çizgisi tanımlama grubunun topolojiniz içinde ne sıklıkta yayıldığını ayarlayabilirsiniz.

Bir C# bileşeninden değer çizgisi tanımlama grubu kullanımı örneği için bkz. [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

### <a name="caches"></a>Caches

Bellek içi önbelleğe alma, sık kullanılan varlıkları bellekte tuttuğu için çoğunlukla işlemeyi hızlandırmaya yönelik bir mekanizma olarak kullanılır. Bir topoloji birden fazla düğüme ve her bir düğüm içindeki birden fazla işleme dağıtıldığı için, [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanmayı düşünmeniz gerekir. Önbellek araması için kullanılan alanları içeren tanımlama gruplarının her zaman aynı işleme yönlendirildiğinden emin olmak için `fieldsGrouping` kullanın. Bu gruplandırma işlevi, süreçler arasında önbellek girişlerinin yinelenmesini önler.

### <a name="stream-top-n"></a>"İlk N" akış

Topolojiniz bir "ilk N" değeri hesaplamaya bağlıysa, ilk N değeri paralel olarak hesaplayın. Ardından bu hesaplamaların çıktısını genel bir değerde birleştirmeniz gerekir. Bu işlem, alanı paralel işleme için yönlendirmek üzere [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanılarak yapılabilir. Ardından, ilk n değerini genel olarak belirleyen bir bolta yönlendirebilirsiniz.

İlk N değeri hesaplama örneği için [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java) örneğine bakın.

## <a name="logging"></a>Günlüğe kaydetme

Storm bilgileri günlüğe kaydetmek için Apache Log4j kullanır. Varsayılan olarak, büyük miktarlarda veriler günlüğe kaydedilir ve bilgilerin sıralanması zor olabilir. Günlüğe kaydetme davranışını denetlemek üzere Storm topolojinizin bir parçası olarak günlük yapılandırma dosyası ekleyebilirsiniz.

Günlüğün nasıl yapılandırılacağını gösteren örnek bir topoloji için HDInsight üzerinde Storm için [Java tabanlı WordCount](hdinsight-storm-develop-java-topology.md) örneğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight üzerinde Storm ile gerçek zamanlı analiz çözümleri hakkında daha fazla bilgi edinin:

* [HDInsight üzerinde Apache Storm kullanmaya başlama][gettingstarted]
* [HDInsight üzerinde Apache Storm için örnek topolojiler](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

