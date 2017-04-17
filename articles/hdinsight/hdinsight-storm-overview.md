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
ms.date: 03/31/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 73e524bf9e7d51cee841a3c5599ab33aa31cbc37
ms.lasthandoff: 04/06/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>HDInsight üzerinde Apache Storm’a giriş: Hadoop için gerçek zamanlı analiz

Azure HDInsight üzerinde Apache Storm kullanarak dağıtılmış, gerçek zamanlı analiz çözümleri oluşturabilirsiniz.

Storm; dağıtılmış, hataya dayanıklı, açık kaynaklı bir hesaplama sistemidir. Verileri Hadoop ile gerçek zamanlı olarak işlemek için Storm’u kullanabilirsiniz. Storm çözümleri ayrıca ilk seferde başarılı bir şekilde işlenmemiş verileri yeniden yürütme özelliğiyle birlikte verilerin garantili işlenmesini sağlayabilir.

## <a name="how-does-storm-work"></a>Storm nasıl çalışır?

Storm, HDInsight veya Hadoop’ta alışkın olabileceğiniz MapReduce işlerinin yerine topolojiler çalıştırır. Topolojiler döngüsel olmayan yönlü grafikte (DAG) düzenlenmiş birden fazla bileşenden oluşur. Aşağıdaki diyagram, temel bir sözcük sayısı topolojisindeki bileşenler arasında verilerin nasıl aktığını gösterir:

![Bileşenlerin bir Storm topolojisinde nasıl düzenlendiğini gösteren örnek](./media/hdinsight-storm-overview/wordcount-topology.png)

* Spout bileşenleri, verileri bir topolojiye getirir. Bu bileşenler topolojiye bir veya daha fazla akış yayar.

* Bolt bileşenleri, spout veya diğer boltlardan yayılan akışları kullanır. Boltlar topolojiye isteğe bağlı olarak yeni akışlar yayabilir. Boltlar ayrıca HDFS veya HBase gibi kalıcı depolama alanlarına veri yazmaktan sorumludur.

## <a name="why-use-storm-on-hdinsight"></a>HDInsight üzerinde Storm neden kullanılmalıdır?

HDInsight üzerinde Storm aşağıdaki önemli avantajları sunar:

* Yüzde 99,9 çalışma zamanı SLA'sı ile yönetilen bir hizmet olarak çalışır.

* Oluşturma sırasında veya sonrasında kümede betik çalıştırarak kolay özelleştirmeyi destekler. Daha fazla bilgi için bkz. [HDInsight kümelerini betik eylemi kullanarak özelleştirme](hdinsight-hadoop-customize-cluster-linux.md).

* Çeşitli diller kullanır. Storm bileşenlerini Java, C# ve Python gibi dilediğiniz bir dilde yazabilirsiniz.

    * C# topolojisi geliştirme, yönetme ve izleme için HDInsight ile Visual Studio’yu tümleştirir. Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Trident Java arabirimini destekler. İletilerin tam olarak bir kez işlenmesini, işlemsel veri deposu kalıcılığını ve sık kullanılan Stream Analytics işlemlerini destekleyen Storm topolojileri oluşturabilirsiniz.

*  Kümelerin ölçeğini kolayca artırır ve azaltır. Çalışan Storm topolojilerini etkilemeden çalışan düğümleri ekleyebilir veya kaldırabilirsiniz.

* Aşağıdaki Azure Hizmetleri ile tümleştirilir:

    * Azure Event Hubs

    * Azure Sanal Ağ

    * Azure SQL Database

    * Azure Storage

    * Azure DocumentDB

* Sanal Ağ kullanarak birden fazla HDInsight kümesinin özelliklerini güvenli bir şekilde birleştirir. HDInsight, HBase veya Hadoop kümeleri kullanan analitik işlem hatları oluşturabilirsiniz.

Gerçek zamanlı analiz çözümleri için Storm kullanan şirketlerin listesi için bkz. [Apache Storm kullanan şirketler](https://storm.apache.org/documentation/Powered-By.html).

Storm kullanmaya başlamak için bkz. [HDInsight Üzerinde Storm ile çalışmaya başlama][gettingstarted].

## <a name="ease-of-creation"></a>Oluşturma kolaylığı

HDInsight üzerinde dakikalar için yeni bir Storm kümesi sağlayabilirsiniz. Storm kümesi oluşturma hakkında daha fazla bilgi için bkz. [HDInsight’ta Storm kullanmaya başlama](hdinsight-apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Kullanım kolaylığı

* __Secure Shell (SSH) bağlantısı__: HDInsight kümenizin baş düğümlerine İnternet üzerinden SSH kullanarak erişebilirsiniz. SSH kullanarak komutları doğrudan kümeniz üzerinde çalıştırabilirsiniz.

  Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Web bağlantısı__: HDInsight kümeleri Ambari web kullanıcı arabirimini sunar. Ambari web kullanıcı arabirimini kullanarak kümenizdeki hizmetleri kolayca izleyebilir, yapılandırabilir ve yönetebilirsiniz. HDInsight üzerinde Storm ayrıca, Storm kullanıcı arabirimini de sağlar. Storm kullanıcı arabirimini kullanarak, çalışan Storm topolojilerini tarayıcınızdan izleyip yönetebilirsiniz.

  Daha fazla bilgi için bkz. [HDInsight'ı Ambari Web Kullanıcı Arabirimini kullanarak yönetme](hdinsight-hadoop-manage-ambari.md) ve [Storm kullanıcı arabirimini kullanarak izleme ve yönetme](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui).

* __Azure PowerShell ve Azure CLI__: Hem Azure PowerShell hem de CLI, HDInsight ve diğer Azure hizmetleriyle çalışmak için istemci sisteminizde kullanabileceğiniz komut satırı yardımcı programları sunar.

* __Visual Studio tümleştirmesi__: Visual Studio için Azure Data Lake Araçları, SCP.Net çerçevesi kullanarak C# Storm topolojileri oluşturmaya yönelik proje şablonları içerir. Data Lake Araçları ayrıca HDInsight üzerinde Storm ile çözümleri dağıtma, izleme ve yönetmeye yönelik araçlar sağlar.

  Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Diğer Azure hizmetleriyle tümleştirme

* __Azure Data Lake Store__: Data Lake Store’u Storm ile kullanmaya ilişkin bir örnek için bkz. [HDInsight’ta Azure Data Lake Store ile Apache Storm kullanma](hdinsight-storm-write-data-lake-store.md).

* __Event Hubs__: Event Hubs’ı Storm ile kullanma örneği için aşağıdaki belgelere bakın:

    * [HDInsight üzerinde Storm için Java tabanlı topoloji geliştirme](hdinsight-storm-develop-java-topology.md)

    * [HDInsight üzerinde Storm ile Azure Event Hubs’tan olay işleme (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md)

* __SQL Veritabanı__, __DocumentDB__, __Event Hubs__ ve __HBase__: Visual Studio için Data Lake Araçlarına şablon örnekleri eklenmiştir. Daha fazla bilgi için bkz. [HDInsight'ta Storm için C# topolojisi geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="reliability"></a>Güvenilirlik

Storm, veri analizi yüzlerce düğüme dağıldığında bile tüm gelen iletilerin her zaman tamamen işleneceğini garanti eder.

Nimbus düğümü, Hadoop JobTracker’a benzer bir işlevsellik sağlar ve Zookeeper aracılığıyla kümedeki diğer düğümlere görevler atar. Zookeeper düğümleri küme için eşgüdüm sağlar ve çalışan düğümleri üzerinde Nimbus ile Supervisor işlemi arasındaki iletişimi kolaylaştırır. Bir işleme düğümü devre dışı kalırsa Nimbus düğümü bilgilendirilir ve görevi ve ilişkili verileri başka bir düğüme atar.

Storm için varsayılan yapılandırma yalnızca bir Nimbus düğümü içerir. HDInsight üzerindeki Storm iki Nimbus düğümü çalıştırır. Birincil düğüm başarısız olursa birincil düğüm kurtarılırken HDInsight kümesi ikincil düğüme geçiş yapar. Aşağıdaki diyagramda HDInsight üzerinde Storm için görev akışı yapılandırması gösterilmektedir:

![Nimbus, zookeeper ve supervisor diyagramı](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Ölçek

Oluşturma sırasında kümedeki düğüm sayısını belirtebilseniz de, iş yüküyle eşleşmesi için kümeyi büyütmek ya da küçültmek isteyebilirsiniz. Veriler işlenirken bile tüm HDInsight kümelerindeki düğüm sayısını değiştirebilirsiniz.

> [!NOTE]
> Ölçeklendirme aracılığıyla eklenen yeni düğümlerden yararlanmak için küme boyutu artırılmadan önce topolojileri yeniden dengelemeniz gerekir.

## <a name="support"></a>Destek

HDInsight üzerinde Storm kurumsal düzeyde kesintisiz tam destek ile birlikte gelir. HDInsight üzerinde Storm ayrıca yüzde 99,9 SLA’ya sahiptir. Diğer bir deyişle kümenin, sürenin en az yüzde 99,9’unda dış bağlantıya sahip olacağı garanti edilir.

Daha fazla bilgi için bkz. [Azure desteği](https://azure.microsoft.com/support/options/).

## <a name="common-use-cases"></a>Genel kullanım örnekleri

HDInsight üzerinde Storm kullanabileceğiniz bazı yaygın senaryolar aşağıda verilmiştir. 

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

Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir Storm uygulaması en az bir kez işlemeyi, Trident ise tam bir kez işlemeyi garanti edebilir.

Daha fazla bilgi için apache.org sayfasındaki [Veri işleme garantileri](https://storm.apache.org/about/guarantees-data-processing.html) bölümüne bakın.

### <a name="ibasicbolt"></a>IBasicBolt

Bir girdi tanımlama grubunu okuma, sıfır veya daha fazla tanımlama grubu yayma ve yürütme yönteminin hemen sonrasında girdi tanımlama grubunu onaylama deseni yaygındır. Storm bu deseni otomatik hale getirmek için [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) arabirimini kullanır.

### <a name="joins"></a>Birleştirme

Uygulamalar arasına veri akışları nasıl katılır? Örneğin, yeni birden fazla akıştaki her bir tanımlama grubunu yeni bir akışta birleştirebilir veya yalnızca toplu tanımlama gruplarını belirli bir pencere için birleştirebilirsiniz. Her iki işlem de tanımlama gruplarının cıvatalara nasıl yönlendirileceğini tanımlayan [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) yöntemiyle gerçekleştirilebilir.

Aşağıdaki Java örneğinde fieldsGrouping, "1", "2" ve "3" bileşenlerinden kaynaklanan tanımlama gruplarını MyJoiner boltuna yönlendirmek için kullanılır:

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batches"></a>Toplu işler

Storm her X saniyede bir toplu iş yaymak üzere kullanılabilecek, "değer çizgisi tanımlama grubu" olarak bilinen bir iç zamanlama mekanizmasını sağlar.

Bir C# bileşeninden değer çizgisi tanımlama grubu kullanımı örneği için bkz. [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

Trident, tanımlama grubu toplu işlerini işlemeye bağlıdır.

### <a name="caches"></a>Caches

Bellek içi önbelleğe alma, sık kullanılan varlıkları bellekte tuttuğu için çoğunlukla işlemeyi hızlandırmaya yönelik bir mekanizma olarak kullanılır. Bir topoloji birden fazla düğüme ve her bir düğüm içindeki birden fazla işleme dağıtıldığı için, [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) kullanmayı düşünmeniz gerekir. Önbellek araması için kullanılan alanları içeren tanımlama gruplarının her zaman aynı işleme yönlendirildiğinden emin olmak için `fieldsGrouping` kullanın. Bu gruplandırma işlevi, süreçler arasında önbellek girişlerinin yinelenmesini önler.

### <a name="stream-top-n"></a>"İlk N" akış

Topolojiniz bir "ilk N" değeri hesaplamaya bağlıysa, ilk N değeri paralel olarak hesaplayın. Ardından bu hesaplamaların çıktısını genel bir değerde birleştirmeniz gerekir. Paralel işleme için bu işlem, alana göre yönlendirmek ve ardından ilk N değeri genel olarak belirleyen bir bolt’a yönlendirmek üzere [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) kullanılarak yapılabilir.

İlk N değeri hesaplama örneği için [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java) örneğine bakın.

## <a name="logging"></a>Günlüğe kaydetme

Storm bilgileri günlüğe kaydetmek için Apache Log4j kullanır. Varsayılan olarak, büyük miktarlarda veriler günlüğe kaydedilir ve bilgilerin sıralanması zor olabilir. Günlüğe kaydetme davranışını denetlemek üzere Storm topolojinizin bir parçası olarak günlük yapılandırma dosyası ekleyebilirsiniz.

Günlüğün nasıl yapılandırılacağını gösteren örnek bir topoloji için HDInsight üzerinde Storm için [Java tabanlı WordCount](hdinsight-storm-develop-java-topology.md) örneğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight üzerinde Storm ile gerçek zamanlı analiz çözümleri hakkında daha fazla bilgi edinin:

* [HDInsight üzerinde Storm kullanmaya başlama][gettingstarted]
* [HDInsight üzerinde Storm için örnek topolojiler](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

