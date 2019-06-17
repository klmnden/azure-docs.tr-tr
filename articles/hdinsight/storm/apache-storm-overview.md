---
title: Apache Storm nedir? - Azure HDInsight
description: Apache Storm, veri akışlarını gerçek zamanlı olarak işlemenize olanak tanır. Azure HDInsight, Azure bulutu üzerinde Storm kümelerini kolayca oluşturmanıza olanak tanır. Visual Studio ile C# kullanarak Storm çözümleri oluşturabilir ve sonra HDInsight Storm kümelerinize dağıtabilirsiniz.
author: hrasheed-msft
ms.reviewer: jasonh
keywords: apache storm kullanım örnekleri,storm kümesi,apache storm nedir
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: overview
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: 97083142066e59acbefe60181743e5aa32541bac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67115835"
---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Azure HDInsight’ta Apache Storm nedir?

[Apache Storm](https://storm.apache.org/); dağıtılmış, hataya dayanıklı, açık kaynaklı bir hesaplama sistemidir. Storm ile gerçek zamanlı veri akışlarını için kullanabileceğiniz [Apache Hadoop](https://hadoop.apache.org/). Storm çözümleri ayrıca ilk seferde başarılı bir şekilde işlenmemiş verileri yeniden yürütme özelliğiyle birlikte verilerin garantili işlenmesini sağlayabilir.

## <a name="why-use-apache-storm-on-hdinsight"></a>HDInsight üzerinde Apache Storm neden kullanılmalıdır?

HDInsight üzerinde Storm aşağıdaki özellikleri sunar:

* __% 99 'un üzerinde Storm çalışma süresi hizmet düzeyi sözleşmesi (SLA)__ : Daha fazla bilgi için [HDInsight için SLA bilgileri](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/) belgesine bakın.

* Oluşturma sırasında veya sonrasında Storm kümesinde betik çalıştırarak kolay özelleştirmeyi destekler. Daha fazla bilgi için bkz. [HDInsight kümelerini betik eylemi kullanarak özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md).

* **Birden fazla dilde çözümler oluşturma**: Storm bileşenlerini Java, C# ve Python gibi dilediğiniz bir dilde yazabilirsiniz.

    * C# topolojisi geliştirme, yönetme ve izleme için HDInsight ile Visual Studio’yu tümleştirir. Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

    * Trident Java arabirimini destekler. İletilerin tam olarak bir kez işlenmesini, işlemsel veri deposu kalıcılığını ve sık kullanılan Stream Analytics işlemlerini destekleyen Storm topolojileri oluşturabilirsiniz.

* **Dinamik ölçeklendirme**: Çalışan Storm topolojilerini etkilemeden çalışan düğümleri ekleyebilir veya kaldırabilirsiniz.

    * Ölçeklendirme işlemleriyle eklenen birkaç yeni düğümden yararlanmak için, çalışan topolojileri devre dışı bırakıp yeniden etkinleştirebilirsiniz.

* **Birden çok Azure hizmetlerini kullanarak akış işlem hatları oluşturma**: HDInsight üzerinde Storm, Event Hubs, SQL veritabanı, Azure depolama ve Azure Data Lake depolama gibi diğer Azure hizmetleriyle tümleşir.

    Azure hizmetleriyle tümleşen örnek bir çözüm için bkz: [HDInsight üzerinde Apache Storm ile Event hubs'tan olay işleme](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/).

Gerçek zamanlı analiz çözümleri için Apache Storm kullanan şirketlerin listesi için bkz. [Apache Storm Kullanan Şirketler](https://storm.apache.org/documentation/Powered-By.html).

Storm kullanmaya başlamak için bkz: [HDInsight üzerinde Apache Storm ile çalışmaya başlama](apache-storm-tutorial-get-started-linux.md).

## <a name="how-does-apache-storm-work"></a>Apache Storm nasıl çalışır

Storm yerine topolojiler çalıştırır [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) alışkın olabileceğiniz işler. Storm topolojileri döngüsel olmayan yönlü grafikte (DAG) düzenlenmiş birden fazla bileşenden oluşur. Veriler grafikteki bileşenler arasında akar. Her bileşen bir veya daha fazla veri akışı kullanır ve isteğe bağlı olarak bir veya daha fazla akış yayar. Aşağıdaki diyagram, temel bir sözcük sayısı topolojisindeki bileşenler arasında verilerin nasıl aktığını gösterir:

![Bileşenlerin bir Storm topolojisinde nasıl düzenlendiğini gösteren örnek](./media/apache-storm-overview/example-apache-storm-topology-diagram.png)

* Spout bileşenleri, verileri bir topolojiye getirir. Bu bileşenler topolojiye bir veya daha fazla akış yayar.

* Bolt bileşenleri, spout veya diğer boltlardan yayılan akışları kullanır. Boltlar topolojiye isteğe bağlı olarak akışlar yayabilir. Boltlar ayrıca HDFS, Kafka veya HBase gibi dış hizmetlere veya depolama alanlarına veri yazmaktan sorumludur.

## <a name="reliability"></a>Güvenilirlik

Apache Storm, veri analizi yüzlerce düğüme dağıldığında bile tüm gelen iletilerin her zaman tamamen işleneceğini garanti eder.

Nimbus düğümü Apache Hadoop Jobtracker'a benzer bir işlevsellik sağlar ve aracılığıyla kümedeki diğer düğümlere görevler atar [Apache ZooKeeper](https://zookeeper.apache.org/). Zookeeper düğümleri küme için eşgüdüm sağlar ve çalışan düğümleri üzerinde Nimbus ile Supervisor işlemi arasındaki iletişimi kolaylaştırır. Bir işleme düğümü devre dışı kalırsa Nimbus düğümü bilgilendirilir ve görevi ve ilişkili verileri başka bir düğüme atar.

Apache Storm kümeleri için varsayılan yapılandırma yalnızca bir Nimbus düğümü içerir. HDInsight üzerindeki Storm iki Nimbus düğümü sağlar. Birincil düğüm başarısız olursa birincil düğüm kurtarılırken Storm kümesi ikincil düğüme geçiş yapar. Aşağıdaki diyagramda HDInsight üzerinde Storm için görev akışı yapılandırması gösterilmektedir:

![Nimbus, zookeeper ve supervisor diyagramı](./media/apache-storm-overview/nimbus.png)

## <a name="ease-of-creation"></a>Oluşturma kolaylığı

HDInsight üzerinde dakikalar için yeni bir Storm kümesi oluşturabilirsiniz. Storm kümesi oluşturma hakkında daha fazla bilgi için bkz. [HDInsight’ta Storm kullanmaya başlama](apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Kullanım kolaylığı

* __Secure Shell (SSH) bağlantısı__: SSH kullanarak Storm kümenizin baş düğümlerine Internet üzerinden erişebilirsiniz. SSH kullanarak komutları doğrudan kümeniz üzerinde çalıştırabilirsiniz.

  Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

* __Web Bağlantısı__: Tüm HDInsight kümeleri Ambari web kullanıcı Arabirimi sağlar. Ambari web kullanıcı arabirimini kullanarak kümenizdeki hizmetleri kolayca izleyebilir, yapılandırabilir ve yönetebilirsiniz. Storm kümeleri ayrıca Storm Kullanıcı Arabirimini sağlar. Storm kullanıcı arabirimini kullanarak, çalışan Storm topolojilerini tarayıcınızdan izleyip yönetebilirsiniz.

  Daha fazla bilgi için [Apache Ambari Web kullanıcı arabirimini kullanarak HDInsight yönetme](../hdinsight-hadoop-manage-ambari.md) ve [İzleyici ve Apache Storm kullanıcı arabirimini kullanarak yönetme](apache-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui) belgeleri.

* __Azure PowerShell ve Azure Klasik CLI__: PowerShell ve klasik CLI hem de HDInsight ve diğer Azure hizmetleriyle çalışmak için istemci sisteminizde kullanabileceğiniz komut satırı yardımcı programları sağlar.

* __Visual Studio tümleştirmesi__: Visual Studio için Azure Data Lake araçları oluşturmaya yönelik proje şablonları içerir C# Storm topolojileri SCP.NET çerçevesi kullanarak. Data Lake Araçları ayrıca HDInsight üzerinde Storm ile çözümleri dağıtma, izleme ve yönetmeye yönelik araçlar sağlar.

  Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Diğer Azure hizmetleriyle tümleştirme

* __Azure Data Lake depolama__: Data Lake Storage'ı bir Storm kümesi ile kullanma örneği için bkz: [kullanım Azure Data Lake Store ile HDInsight üzerinde Apache Storm](apache-storm-write-data-lake-store.md).

* __Olay hub'ları__: Event Hubs'ı bir Storm kümesi ile kullanma örneği için aşağıdaki örneklere bakın:

    * [(Java) HDInsight üzerinde Apache Storm ile Azure Event hubs'dan olayları işleme](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/)

    * [HDInsight üzerinde Apache Storm ile Azure Event hubs'tan olay işleme (C#)](apache-storm-develop-csharp-event-hub-topology.md)

* __SQL veritabanı__, __Cosmos DB__, __olay hub'ları__, ve __HBase__: Şablon örnekleri, Visual Studio için Data Lake araçlarına dahil edilir. Daha fazla bilgi için [geliştirme bir C# HDInsight üzerinde Apache Storm topolojisini](apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="support"></a>Destek

HDInsight üzerinde Storm kurumsal düzeyde kesintisiz tam destek ile birlikte gelir. HDInsight üzerinde Storm ayrıca yüzde 99,9 SLA’ya sahiptir. Diğer bir deyişle Microsoft, Storm kümesinin, sürenin en az yüzde 99,9’unda dış bağlantıya sahip olacağını garanti eder.

Daha fazla bilgi için bkz. [Azure desteği](https://azure.microsoft.com/support/options/).

## <a name="apache-storm-use-cases"></a>Apache Storm kullanım örnekleri

HDInsight üzerinde Storm kullanabileceğiniz bazı yaygın senaryolar aşağıda verilmiştir:

* Nesnelerin İnterneti (IoT)
* Sahtekarlık algılama
* Sosyal analiz
* Ayıklama, dönüşüm ve yükleme (ETL)
* Ağ izleme
* Ara
* Mobil katılım

Gerçek senaryolar hakkında daha fazla bilgi için bkz: [şirketler Apache Storm'u nasıl kullanıyor](https://storm.apache.org/documentation/Powered-By.html) belge.

## <a name="development"></a>Geliştirme

.NET geliştiricileri, Visual Studio için Data Lake Araçları kullanarak C# dilinde topolojiler tasarlayıp uygulayabilir. Ayrıca Java ve C# bileşenlerini kullanan karma topolojiler de oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

Ayrıca seçtiğiniz IDE’yi kullanarak Java çözümleri geliştirebilirsiniz. Daha fazla bilgi için [HDInsight üzerinde Apache Storm için Java topolojileri geliştirme](apache-storm-develop-java-topology.md).

Storm bileşenleri geliştirmek için Python da kullanılabilir. Daha fazla bilgi için [Python kullanarak HDInsight üzerinde Apache Storm topolojileri geliştirme](apache-storm-develop-python-topology.md).

## <a name="common-development-patterns"></a>Yaygın geliştirme desenleri

### <a name="guaranteed-message-processing"></a>Garantili ileti işleme

Apache Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir Storm uygulaması en az bir kez işlemeyi garanti edebilir ve [Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html) tam bir kez işlemeyi garanti edebilir.

Daha fazla bilgi için apache.org sayfasındaki [Veri işleme garantileri](https://storm.apache.org/about/guarantees-data-processing.html) bölümüne bakın.

### <a name="ibasicbolt"></a>IBasicBolt

Bir girdi tanımlama okuma, sıfır veya daha fazla tanımlama grubu yayma ve yürütme yönteminin hemen sonrasında girdi tanımlama olumsuz bildirimde bulunmaya deseni yaygındır. Storm bu deseni otomatik hale getirmek için [IBasicBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/IBasicBolt.html) arabirimini kullanır.

### <a name="joins"></a>Birleştirme

Uygulamalar arasına veri akışları nasıl katılır? Örneğin, yeni birden fazla akıştaki her bir tanımlama grubunu yeni bir akışta birleştirebilir veya yalnızca toplu tanımlama gruplarını belirli bir pencere için birleştirebilirsiniz. Her iki durumda da katılım, [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanılarak gerçekleştirilebilir. Alan gruplandırma, tanımlama gruplarının boltlara nasıl yönlendirileceğini tanımlamanın bir yoludur.

Aşağıdaki Java örneğinde fieldsGrouping, "1", "2" ve "3" bileşenlerinden kaynaklanan tanımlama gruplarını MyJoiner boltuna yönlendirmek için kullanılır:

```java
builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));
```

### <a name="batches"></a>Toplu işler

Apache Storm "değer çizgisi tanımlama grubu" olarak bilinen bir iç zamanlama mekanizmasını sağlar. Değer çizgisi tanımlama grubunun topolojiniz içinde ne sıklıkta yayıldığını ayarlayabilirsiniz.

Bir C# bileşeninden değer çizgisi tanımlama grubu kullanımı örneği için bkz. [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

### <a name="caches"></a>Caches

Bellek içi önbelleğe alma, sık kullanılan varlıkları bellekte tuttuğu için çoğunlukla işlemeyi hızlandırmaya yönelik bir mekanizma olarak kullanılır. Bir topoloji birden fazla düğüme ve her bir düğüm içindeki birden fazla işleme dağıtıldığı için, [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanmayı düşünmeniz gerekir. Önbellek araması için kullanılan alanları içeren tanımlama gruplarının her zaman aynı işleme yönlendirildiğinden emin olmak için `fieldsGrouping` kullanın. Bu gruplandırma işlevi, süreçler arasında önbellek girişlerinin yinelenmesini önler.

### <a name="stream-top-n"></a>"İlk N" akış

Topolojiniz bir "ilk N" değeri hesaplamaya bağlıysa, ilk N değeri paralel olarak hesaplayın. Ardından bu hesaplamaların çıktısını genel bir değerde birleştirmeniz gerekir. Bu işlem, alanı paralel işleme için yönlendirmek üzere [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanılarak yapılabilir. Ardından, ilk n değerini genel olarak belirleyen bir bolta yönlendirebilirsiniz.

İlk N değeri hesaplama örneği için [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java) örneğine bakın.

## <a name="logging"></a>Günlüğe kaydetme

Storm kullanan [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) bilgileri günlüğe kaydetmek için. Varsayılan olarak, büyük miktarlarda veriler günlüğe kaydedilir ve bilgilerin sıralanması zor olabilir. Günlüğe kaydetme davranışını denetlemek üzere Storm topolojinizin bir parçası olarak günlük yapılandırma dosyası ekleyebilirsiniz.

Günlüğün nasıl yapılandırılacağını gösteren örnek bir topoloji için HDInsight üzerinde Storm için [Java tabanlı WordCount](apache-storm-develop-java-topology.md) örneğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight üzerinde Apache Storm ile gerçek zamanlı analiz çözümleri hakkında daha fazla bilgi edinin:

* [HDInsight üzerinde Apache Storm ile çalışmaya başlama](apache-storm-tutorial-get-started-linux.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md)
