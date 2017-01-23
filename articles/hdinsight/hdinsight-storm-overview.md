---
title: "HDInsight Üzerinde Apache Storm’a Giriş | Microsoft Belgeleri"
description: "Apache Storm hakkında giriş bilgileri alın ve bulutta gerçek zamanlı veri analizi çözümleri oluşturmak üzere Storm’u HDInsight üzerinde nasıl kullanacağınızı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/11/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: bccec1e4078c38e1cc9205a36d3a5df579df35b6
ms.openlocfilehash: d5ff397e947a7edc8310da59ff9fe8896829e35d


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>HDInsight üzerinde Apache Storm’a giriş: Hadoop için gerçek zamanlı analiz
HDInsight üzerinde Apache Storm, [Apache Hadoop](http://hadoop.apache.org) kullanarak Azure ortamında dağıtılmış, gerçek zamanlı analiz çözümleri oluşturmanıza imkan tanır.

## <a name="what-is-apache-storm"></a>Apache Storm nedir?
Apache Storm verileri Hadoop ile gerçek zamanlı olarak işlemenize imkan tanıyan dağıtılmış, hataya dayanıklı ve açık kaynaklı bir hesaplama sistemidir. Storm çözümleri ayrıca ilk seferde başarılı bir şekilde işlenmemiş verileri yeniden yürütme özelliğiyle birlikte verilerin garantili işlenmesini sağlayabilir.

## <a name="why-use-storm-on-hdinsight"></a>HDInsight üzerinde Storm neden kullanılmalıdır?
HDInsight üzerinde Apache Storm, Azure ortamına tümleştirilmiş yönetilen bir kümedir. Aşağıdaki faydaları sağlar:

* %99,9 çalışma zamanı SLA’sı ile yönetilen bir hizmet olarak çalışır
* Seçtiğiniz dili kullanma: **Java**, **C#** ve **Python** dilinde yazılan Storm bileşenleri için destek sağlar
  
  * Programlama dillerinin bir karışımını destekler: Java kullanarak verileri okuyun, daha sonra C kullanarak işleyin#
    
    > [!NOTE]
    > Linux tabanlı kümeyle C# topolojisini kullanmak için projenizin kullandığı Microsoft.SCP.Net.SDK NuGet paketini 0.10.0.6 veya üzeri sürüme güncelleştirmeniz gerekir. Paketin sürümünün ayrıca HDInsight üzerinde yüklü olan Storm ana sürümüyle eşleşmesi gerekir. Örneğin, HDInsight sürüm 3.3 ve 3.4 üzerindeki Strom bileşenleri, Storm 0.10.x sürümünü kullanırken HDInsight 3.5 ile Storm 1.0.x kullanılır.
    > 
    > Linux tabanlı kümelerdeki C# topolojilerinin .NET 4.5 kullanması ve HDInsight kümesi üzerinde çalışması için Mono kullanması gerekir. Çoğu işlev çalışacaktır ancak olası uyumsuzluklar için [Mono Uyumluluğu](http://www.mono-project.com/docs/about-mono/compatibility/) belgesini incelemeniz gerekir.
    > 
  * İletilerin "tam bir kez" işlenmesini, "işlemsel" veri deposu kalıcılığını ve bir dizi ortak akış analizi işlemlerini destekleyen Storm topolojileri oluşturmak için **Trident** Java arabirimini kullanın
* Yerleşik ölçek artırma ve ölçek azaltma özellikleri içerir: Çalışan Storm topolojilerini etkilemeden bir HDInsight kümesini ölçeklendirin
* Event Hubs, Azure Virtual Network, SQL Database, Blob Storage ve DocumentDB gibi diğer Azure hizmetleriyle tümleştirme
  
  * Azure Virtual Network kullanarak birden fazla HDInsight kümesinin özelliklerini birleştirme: HDInsight, HBase veya Hadoop kümeleri kullanan analitik komut zincirleri oluşturun

Gerçek zamanlı analiz çözümleri için Apache Storm kullanan şirketlerin listesi için bkz. [Apache Storm Kullanan Şirketler](https://storm.apache.org/documentation/Powered-By.html).

Storm kullanmaya başlamak için bkz. [HDInsight Üzerinde Storm ile çalışmaya başlama][gettingstarted].

### <a name="ease-of-provisioning"></a>Sağlama kolaylığı
HDInsight kümesinde dakikalar için yeni bir Storm sağlayabilirsiniz. Küme adı, boyutu, yönetici hesabı ve depolama hesabı belirtin. Azure, örnek topolojileri ve web yönetimi panosu ile birlikte kümeyi oluşturur.

> [!NOTE]
> Storm kümelerini ayrıca [Azure CLI](../xplat-cli-install.md) veya [Azure PowerShell](/powershell/azureps-cmdlets-docs) kullanarak da sağlayabilirsiniz.
> 
> 

İsteği gönderdikten sonraki 15 dakika içinde ilk gerçek zamanlı analiz komut zinciriniz için çalışır ve hazır durumda olan yeni bir Storm kümesine sahip olacaksınız.

### <a name="ease-of-use"></a>Kullanım kolaylığı
**HDInsight kümelerinde Linux tabanlı Storm için** SSH kullanarak kümeye bağlanabilir ve `storm` komutunu kullanarak topolojileri başlatıp yönetebilirsiniz. Ayrıca, Storm hizmetini izlemek için Ambari ve çalışan topolojileri izleyip yönetmek için Storm kullanıcı arabirimi kullanabilirsiniz.

Linux tabanlı Storm kümeleri ile çalışma hakkında daha fazla bilgi için bkz. [Linux tabanlı HDInsight üzerinde Apache Storm ile çalışmaya başlama](hdinsight-apache-storm-tutorial-get-started-linux.md).

**HDInsight kümelerinde Windows tabanlı Storm için**, Visual Studio HDInsight Araçları C# ve karma C#/Java topolojileri oluşturmanıza ve ardından bunları HDInsight kümesi üzerinde Storm’a göndermenize imkan tanır.  

![Storm Projesi oluşturma](./media/hdinsight-storm-overview/createproject.png)

Visual Studio HDInsight Araçları ayrıca bir küme üzerindeki Storm topolojilerini izleyip yönetmenize imkan tanır.

![Storm yönetimi](./media/hdinsight-storm-overview/stormview.png)

Bir Storm uygulaması oluşturmak üzere HDInsight Araçları kullanma örneği için bkz. [Visual Studio HDInsight Araçları ile C# Storm topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Visual Studio HDInsight Araçları hakkında daha fazla bilgi için bkz. [Visual Studio HDInsight Araçlarını kullanmaya başlama](hdinsight-hadoop-visual-studio-tools-get-started.md).

HDInsight kümesi üzerindeki her bir Storm ayrıca küme üzerinde çalışan Storm topolojilerini göndermenize, izlemenize ve yönetmenize imkan tanıyan web tabanlı bir Storm Panosu sunar.

![Storm panosu](./media/hdinsight-storm-overview/dashboard.png)

Storm Panosunu kullanma hakkında daha fazla bilgi için bkz. [HDInsight üzerinde Apache Storm topolojilerini dağıtma ve yönetme](hdinsight-storm-deploy-monitor-topology.md).

HDInsight üzerinde Storm ayrıca **Event Hubs Spout** aracılığıyla Azure Event Hubs ile kolay tümleştirme sağlar. Bu bileşenin en son sürümü [https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs) sayfasında mevcuttur. Bu bileşeni kullanma hakkında daha fazla bilgi için aşağıdaki belgelere bakın.

* [Azure Event Hubs kullanan bir C# topolojisi geliştirme](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [Azure Event Hubs kullanan bir Java topolojisi geliştirme](hdinsight-storm-develop-java-event-hub-topology.md)

### <a name="reliability"></a>Güvenilirlik
Apache Storm, veri analizi yüzlerce düğüme dağıldığında bile gelen her bir iletinin her zaman tamamen işleneceğini garanti eder.

**Nimbus düğümü**, Hadoop JobTracker’a benzer bir işlevsellik sağlar ve **Zookeeper** aracılığıyla kümedeki diğer düğümlere görevler atar. Zookeeper düğümleri küme için eşgüdüm sağlar ve çalışan düğümleri üzerinde Nimbus ile **Supervisor** işlemi arasındaki iletişimi kolaylaştırır. Bir işleme düğümü devre dışı kalırsa Nimbus düğümü bilgilendirilir ve görevi ve ilişkili verileri başka bir düğüme atar.

Apache Storm için varsayılan yapılandırma yalnızca bir Nimbus düğümü içerir. HDInsight üzerindeki Storm iki Nimbus düğümü çalıştırır. Birincil düğüm başarısız olursa birincil düğüm kurtarılırken HDInsight kümesi ikincil düğüme geçiş yapar.

![Nimbus, zookeeper ve supervisor diyagramı](./media/hdinsight-storm-overview/nimbus.png)

### <a name="scale"></a>Ölçek
Oluşturma sırasında kümedeki düğüm sayısını belirtebilseniz de, iş yüküyle eşleşmesi için kümeyi büyütmek ya da küçültmek isteyebilirsiniz. Tüm HDInsight kümeleri, veriler işlenirken bile kümedeki düğüm sayısını değiştirmenize izin verir.

> [!NOTE]
> Ölçeklendirme aracılığıyla eklenen yeni düğümlerden yararlanmak için küme boyutu artırılmadan önce topolojileri yeniden dengelemeniz gerekir.
> 
> 

### <a name="support"></a>Destek
HDInsight üzerinde Storm 7 gün 24 saat kurumsal düzeyde tam destek ile birlikte gelir. HDInsight üzerinde Storm ayrıca %99,9 SLA’ya sahiptir. Diğer bir deyişle kümenin, sürenin en az %99,9’unda dış bağlantıya sahip olacağı garanti edilir.

## <a name="common-use-cases-for-real-time-analytics"></a>Gerçek zamanlı analiz için ortak kullanım durumları
HDInsight üzerinde Apache storm kullanabileceğiniz bazı yaygın senaryolar aşağıda verilmiştir. Gerçek senaryolar hakkında daha fazla bilgi için [Şirketler Storm’u nasıl kullanıyor?](https://storm.apache.org/documentation/Powered-By.html) sayfasını okuyun.

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
  > Çoğu durumda veriler Kafka, Azure Service Bus kuyrukları veya Event Hubs gibi bir kuyruktan okunur. Kuyruk, bir kesinti oluşursa verilerin kalıcı olmasını sağlar.
  > 
  > 
* **Cıvata**: **Akışları** kullanır, **tanımlama grupları** üzerinde işlemeyi gerçekleştirir ve **akışlar** yayabilir. Cıvatalar ayrıca kuyruk, HDInsight, HBase, blob veya diğer veri depoları gibi dış depolama alanlarına veri yazmaktan sorumludur.
* **Apache Thrift**: Ölçeklenebilir çapraz dil hizmet dağıtımına yönelik bir yazılım çerçevesi. C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk ve diğer diller arasında çalışan hizmetler oluşturmanıza imkan tanır.
  
  * **Nimbus** bir Thrift hizmeti, **topoloji** ise bir Thrift tanımıdır; bu nedenle çeşitli programlama dillerini kullanarak topolojiler geliştirmek mümkündür.

Storm bileşenleri hakkında daha fazla bilgi için apache.org adresindeki [Storm öğreticisi][apachetutorial] bölümüne bakın.

## <a name="what-programming-languages-can-i-use"></a>Hangi programlama dillerini kullanabilirim?
HDInsight kümesindeki Storm C#, Java ve Python desteği sağlar.

### <a name="c35"></a>C&#35;
Visual Studio HDInsight Araçları, .NET geliştiricilerinin C# dilinde topoloji tasarlayıp uygulamasına imkan tanır. Ayrıca Java ve C# bileşenlerini kullanan karma topolojiler de oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="java"></a>Java
Karşılaştığınız çoğu Java örneği düz Java veya Trident olacaktır. Trident; birleştirme, toplama, gruplandırma ve filtreleme gibi işleri kolaylaştıran üst düzey bir soyutlamadır. Ancak, Trident toplu tanımlama grupları üzerinde hareket ederken, ham Java çözümü bir akışı her seferinde bir tanımlama grubu ile işler.

Trident hakkında daha fazla bilgi için apache.org sayfasındaki [Trident öğreticisi](https://storm.apache.org/documentation/Trident-tutorial.html) bölümüne bakın.

Java ve Trident topolojilerinin örnekleri için [örnek Storm topolojileri listesine](hdinsight-storm-example-topology.md) veya HDInsight kümenizdeki storm başlangıç örneklerine bakın.

Storm başlangıç örnekleri Linux tabanlı kümelerde ** /usr/hdp/current/storm-client/contrib/storm-starter** dizininde ve Windows tabanlı kümelerde **%storm_home%\contrib\storm-starter** dizininde bulunur.

## <a name="what-are-some-common-development-patterns"></a>Bazı ortak geliştirme desenleri nelerdir?
### <a name="guaranteed-message-processing"></a>Garantili ileti işleme
Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir Storm uygulaması en az bir kez işlemeyi, Trident ise tam bir kez işlemeyi garanti edebilir.

Daha fazla bilgi için apache.org sayfasındaki [Veri işleme garantileri](https://storm.apache.org/about/guarantees-data-processing.html) bölümüne bakın.

### <a name="ibasicbolt"></a>IBasicBolt
Bir girdi tanımlama grubunu okuma, sıfır veya daha fazla tanımlama grubu yayma ve ardından girdi tanımlama grubuna yürütme yönteminin hemen sonrasında ack uygulama deseni oldukça yaygındır ve Storm bu deseni otomatik hale getirmek için [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) arabirimini sağlar.

### <a name="joins"></a>Birleştirme
İki veri akışının birleştirilmesi uygulamalar arasında farklılık gösterir. Örneğin, yeni birden fazla akıştaki her bir tanımlama grubunu yeni bir akışta birleştirebilir veya yalnızca toplu tanımlama gruplarını belirli bir pencere için birleştirebilirsiniz. Her iki işlem de tanımlama gruplarının cıvatalara nasıl yönlendirileceğini tanımlayan [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) yöntemiyle gerçekleştirilebilir.

Aşağıdaki Java örneğinde fieldsGrouping, "1", "2" ve "3" bileşenlerinden kaynaklanan tanımlama gruplarını **MyJoiner** cıvatasına yönlendirmek için kullanılır.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batching"></a>Toplu İşleme
Toplu işleme çeşitli yollardan gerçekleştirilebilir. Temel bir Storm Java topolojisi ile tanımlama gruplarını yaymadan önce X sayıda tanımlama grubunu toplu işlemek üzere basit bir sayaç kullanabilir veya "değer çizgisi tanımlama grubu" olarak bilinen iç zamanlama mekanizmasını kullanarak X saniyede bir toplu iş yayabilirsiniz.

Değer çizgisi tanımlama gruplarını kullanma örneği için bkz. [HDInsight üzerinde Storm ve HBase ile sensör verilerini analiz etme](hdinsight-storm-sensor-data-analysis.md).

Trident kullanıyorsanız bu durum tanımlama grubu toplu işlerini işlemeye bağlıdır.

### <a name="caching"></a>Önbelleğe alma
Bellek içi önbelleğe alma, sık kullanılan varlıkları bellekte tuttuğu için çoğunlukla işlemeyi hızlandırmaya yönelik bir mekanizma olarak kullanılır. Bir topoloji birden fazla düğüme ve her bir düğümdeki birden fazla işleme dağıtıldığı için önbellek araması için kullanılan alanları içeren tanımlama gruplarının her zaman aynı işleme yönlendirildiğinden emin olmak için [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) kullanmayı düşünmeniz gerekir. Bunun yapılması süreçler arasında önbellek girişlerinin yinelenmesini önler.

### <a name="streaming-top-n"></a>İlk N akışı
Topolojiniz Twitter’daki ilk 5 trend gibi "ilk N" değeri hesaplamaya bağlıysa ilk N değeri paralel olarak hesaplamanız ve ardından bu hesaplamaların çıktısını genel bir değerde birleştirmeniz gerekir. Bu işlem alanın paralel cıvatalara (verileri alan değerine göre böler) ve ardından ilk N değeri genel olarak belirleyen bir cıvataya yönlendirilmesi için [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) kullanılarak yapılabilir.

Bunun bir örneği için [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java) örneği.

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



<!--HONumber=Jan17_HO1-->


