<properties
    pageTitle="HDInsight Üzerinde Apache Storm’a Giriş | Microsoft Azure"
    description="Apache Storm hakkında giriş bilgileri alın ve bulutta gerçek zamanlı veri analizi çözümleri oluşturmak üzere Storm’u HDInsight üzerinde nasıl kullanacağınızı öğrenin."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="03/18/2016"
   ms.author="larryfr"/>

#HDInsight üzerinde Apache Storm’a giriş: Hadoop için gerçek zamanlı analiz

HDInsight üzerinde Apache Storm, [Apache Hadoop](http://hadoop.apache.org) kullanarak Azure ortamında dağıtılmış, gerçek zamanlı analiz çözümleri oluşturmanıza imkan tanır.

##Apache Storm nedir?

Apache Storm verileri Hadoop ile gerçek zamanlı olarak işlemenize imkan tanıyan dağıtılmış, hataya dayanıklı ve açık kaynaklı bir hesaplama sistemidir. Storm çözümleri ayrıca ilk seferde başarılı bir şekilde işlenmemiş verileri yeniden yürütme özelliğiyle birlikte verilerin garantili işlenmesini sağlayabilir.

##HDInsight üzerinde Storm neden kullanılmalıdır?

HDInsight üzerinde Apache Storm, Azure ortamına tümleştirilmiş yönetilen bir kümedir. Aşağıdaki faydaları sağlar:

* %99,9 çalışma zamanı SLA’sı ile yönetilen bir hizmet olarak çalışır

* Seçtiğiniz dili kullanma: **Java**, **C#** ve **Python** dilinde yazılan Storm bileşenleri için destek sağlar

    * Programlama dillerinin bir karışımını destekler: Java kullanarak verileri okuyun, daha sonra C kullanarak işleyin#
    
        > [AZURE.NOTE] C# topolojileri yalnızca Windows tabanlı HDInsight kümelerinde desteklenir.

    * İletilerin "tam bir kez" işlenmesini, "işlemsel" veri deposu kalıcılığını ve bir dizi ortak akış analizi işlemlerini destekleyen Storm topolojileri oluşturmak için **Trident** Java arabirimini kullanın

* Yerleşik ölçek artırma ve ölçek azaltma özellikleri içerir: Çalışan Storm topolojilerini etkilemeden bir HDInsight kümesini ölçeklendirin

* Event Hubs, Azure Virtual Network, SQL Database, Blob Storage ve DocumentDB gibi diğer Azure hizmetleriyle tümleştirme

    * Azure Virtual Network kullanarak birden fazla HDInsight kümesinin özelliklerini birleştirme: HDInsight, HBase veya Hadoop kümeleri kullanan analitik komut zincirleri oluşturun

Gerçek zamanlı analiz çözümleri için Apache Storm kullanan şirketlerin listesi için bkz. [Apache Storm Kullanan Şirketler](https://storm.apache.org/documentation/Powered-By.html).

Storm kullanmaya başlamak için bkz. [HDInsight Üzerinde Storm ile çalışmaya başlama][gettingstarted].

###Sağlama kolaylığı

HDInsight kümesinde dakikalar için yeni bir Storm sağlayabilirsiniz. Küme adı, boyutu, yönetici hesabı ve depolama hesabı belirtin. Azure, örnek topolojileri ve web yönetimi panosu ile birlikte kümeyi oluşturur.

> [AZURE.NOTE] Storm kümelerini ayrıca [Azure CLI](../xplat-cli-install.md) veya [Azure PowerShell](../powershell-install-configure.md) kullanarak da sağlayabilirsiniz.

İsteği gönderdikten sonraki 15 dakika içinde ilk gerçek zamanlı analiz komut zinciriniz için çalışır ve hazır durumda olan yeni bir Storm kümesine sahip olacaksınız.

###Kullanım kolaylığı

__HDInsight kümelerinde Linux tabanlı Storm için__ SSH kullanarak kümeye bağlanabilir ve `storm` komutunu kullanarak topolojileri başlatıp yönetebilirsiniz. Ayrıca, Storm hizmetini izlemek için Ambari ve çalışan topolojileri izleyip yönetmek için Storm kullanıcı arabirimi kullanabilirsiniz.

Linux tabanlı Storm kümeleri ile çalışma hakkında daha fazla bilgi için bkz. [Linux tabanlı HDInsight üzerinde Apache Storm ile çalışmaya başlama](hdinsight-apache-storm-tutorial-get-started-linux.md).

__HDInsight kümelerinde Windows tabanlı Storm için__, Visual Studio HDInsight Araçları C# ve karma C#/Java topolojileri oluşturmanıza ve ardından bunları HDInsight kümesi üzerinde Storm’a göndermenize imkan tanır.  

![Storm Projesi oluşturma](./media/hdinsight-storm-overview/createproject.png)

Visual Studio HDInsight Araçları ayrıca bir küme üzerindeki Storm topolojilerini izleyip yönetmenize imkan tanır.

![Storm yönetimi](./media/hdinsight-storm-overview/stormview.png)

Bir Storm uygulaması oluşturmak üzere HDInsight Araçları kullanma örneği için bkz. [Visual Studio HDInsight Araçları ile C# Storm topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

Visual Studio HDInsight Araçları hakkında daha fazla bilgi için bkz. [Visual Studio HDInsight Araçlarını kullanmaya başlama](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md).

HDInsight kümesi üzerindeki her bir Storm ayrıca küme üzerinde çalışan Storm topolojilerini göndermenize, izlemenize ve yönetmenize imkan tanıyan web tabanlı bir Storm Panosu sunar.

![Storm panosu](./media/hdinsight-storm-overview/dashboard.png)

Storm Panosunu kullanma hakkında daha fazla bilgi için bkz. [HDInsight üzerinde Apache Storm topolojilerini dağıtma ve yönetme](hdinsight-storm-deploy-monitor-topology.md).

HDInsight üzerinde Storm ayrıca **Event Hubs Spout** aracılığıyla Azure Event Hubs ile kolay tümleştirme sağlar. Bu bileşenin en son sürümü [https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs) sayfasında mevcuttur. Bu bileşeni kullanma hakkında daha fazla bilgi için aşağıdaki belgelere bakın.

* [Azure Event Hubs kullanan bir C# topolojisi geliştirme](hdinsight-storm-develop-csharp-event-hub-topology.md)

* [Azure Event Hubs kullanan bir Java topolojisi geliştirme](hdinsight-storm-develop-java-event-hub-topology.md)

###Güvenilirlik

Apache Storm, veri analizi yüzlerce düğüme dağıldığında bile gelen her bir iletinin her zaman tamamen işleneceğini garanti eder.

**Nimbus düğümü**, Hadoop JobTracker’a benzer bir işlevsellik sağlar ve **Zookeeper** aracılığıyla kümedeki diğer düğümlere görevler atar. Zookeeper düğümleri küme için eşgüdüm sağlar ve çalışan düğümleri üzerinde Nimbus ile **Supervisor** işlemi arasındaki iletişimi kolaylaştırır. Bir işleme düğümü devre dışı kalırsa Nimbus düğümü bilgilendirilir ve görevi ve ilişkili verileri başka bir düğüme atar.

Apache Storm için varsayılan yapılandırma yalnızca bir Nimbus düğümü içerir. HDInsight üzerindeki Storm iki Nimbus düğümü çalıştırır. Birincil düğüm başarısız olursa birincil düğüm kurtarılırken HDInsight kümesi ikincil düğüme geçiş yapar.

![Nimbus, zookeeper ve supervisor diyagramı](./media/hdinsight-storm-overview/nimbus.png)

###Ölçek

Oluşturma sırasında kümedeki düğüm sayısını belirtebilseniz de, iş yüküyle eşleşmesi için kümeyi büyütmek ya da küçültmek isteyebilirsiniz. Tüm HDInsight kümeleri, veriler işlenirken bile kümedeki düğüm sayısını değiştirmenize izin verir.

> [AZURE.NOTE] Ölçeklendirme aracılığıyla eklenen yeni düğümlerden yararlanmak için küme boyutu artırılmadan önce topolojileri yeniden dengelemeniz gerekir.

###Destek

HDInsight üzerinde Storm 7 gün 24 saat kurumsal düzeyde tam destek ile birlikte gelir. HDInsight üzerinde Storm ayrıca %99,9 SLA’ya sahiptir. Diğer bir deyişle kümenin, sürenin en az %99,9’unda dış bağlantıya sahip olacağı garanti edilir.

##Gerçek zamanlı analiz için ortak kullanım durumları

HDInsight üzerinde Apache storm kullanabileceğiniz bazı yaygın senaryolar aşağıda verilmiştir. Gerçek senaryolar hakkında daha fazla bilgi için [Şirketler Storm’u nasıl kullanır](https://storm.incubator.apache.org/documentation/Powered-By.html) bölümünü okuyun.

* Nesnelerin İnterneti (IoT)
* Sahtekarlık algılama
* Sosyal analiz
* Ayıklama, Dönüştürme, Yükleme (ETL)
* Ağ izleme
* Arama
* Mobil katılım

##HDInsight Storm içindeki veriler nasıl işlenir?

Apache Storm, HDInsight veya Hadoop’ta alışkın olabileceğiniz MapReduce işlerinin yerine **topolojiler** çalıştırır. HDInsight kümesi üzerinde Storm iki tür düğüm içerir: **Nimbus** çalıştıran baş düğümler ve **Supervisor** çalıştıran çalışan düğümleri.

* **Nimbus**: Hadoop’taki JobTracker’a benzer şekilde kodu küme boyunca dağıtmaktan, görevleri sanal makinelere atamaktan ve hatalara karşı izlemekten sorumludur. HDInsight iki Nimbus düğümüne sahiptir, bu nedenle HDInsight üzerinde Storm için tek bir hata noktası yoktur

* **Supervisor**: Her bir çalışan düğümünün gözetmeni, düğüm üzerindeki **çalışan işlemlerini** başlatmaktan ve durdurmaktan sorumludur.

* **Çalışan işlemi**: Bir **topoloji** alt kümesi çalıştırır. Çalışan bir topoloji, küme içindeki çok sayıda çalışan işlemine dağıtılır.

* **Topoloji**: Veri **akışlarını** işleyen bir hesaplama grafiği tanımlar. MapReduce işlerinin aksine topolojiler siz durdurana kadar çalışır.

* **Akış**: Bağlantısız bir **tanımlama grubu** koleksiyonu. Akışlar **spout** ve **cıvatalar** ile oluşturulur ve **cıvatalar** tarafından kullanılır.

* **Tanımlama grubu**: Dinamik olarak yazılan değerlerin adlandırılmış listesi.

* **Spout**: Bir veri kaynağındaki verileri kullanır ve bir veya daha fazla **akış** yayar.

    > [AZURE.NOTE] Çoğu durumda veriler Kafka, Azure Service Bus kuyrukları veya Event Hubs gibi bir kuyruktan okunur. Kuyruk, bir kesinti oluşursa verilerin kalıcı olmasını sağlar.

* **Cıvata**: **Akışları** kullanır, **tanımlama grupları** üzerinde işlemeyi gerçekleştirir ve **akışlar** yayabilir. Cıvatalar ayrıca kuyruk, HDInsight, HBase, blob veya diğer veri depoları gibi dış depolama alanlarına veri yazmaktan sorumludur.

* **Apache Thrift**: Ölçeklenebilir çapraz dil hizmet dağıtımına yönelik bir yazılım çerçevesi. C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk ve diğer diller arasında çalışan hizmetler oluşturmanıza imkan tanır.

    * **Nimbus** bir Thrift hizmeti, **topoloji** ise bir Thrift tanımıdır; bu nedenle çeşitli programlama dillerini kullanarak topolojiler geliştirmek mümkündür.

Storm bileşenleri hakkında daha fazla bilgi için apache.org adresindeki [Storm öğreticisi][apachetutorial] bölümüne bakın.


##Hangi programlama dillerini kullanabilirim?

HDInsight kümesindeki Storm C#, Java ve Python desteği sağlar.

### C&#35;

Visual Studio HDInsight Araçları, .NET geliştiricilerinin C# dilinde topoloji tasarlayıp uygulamasına imkan tanır. Ayrıca Java ve C# bileşenlerini kullanan karma topolojiler de oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](hdinsight-storm-develop-csharp-visual-studio-topology.md).

###Java

Karşılaştığınız çoğu Java örneği düz Java veya Trident olacaktır. Trident; birleştirme, toplama, gruplandırma ve filtreleme gibi işleri kolaylaştıran üst düzey bir soyutlamadır. Ancak, Trident toplu tanımlama grupları üzerinde hareket ederken, ham Java çözümü bir akışı her seferinde bir tanımlama grubu ile işler.

Trident hakkında daha fazla bilgi için apache.org sayfasındaki [Trident öğreticisi](https://storm.incubator.apache.org/documentation/Trident-tutorial.html) bölümüne bakın.

Java ve Trident topolojilerinin örnekleri için [örnek Storm topolojileri listesine](hdinsight-storm-example-topology.md) veya HDInsight kümenizdeki storm başlangıç örneklerine bakın.

Storm başlangıç örnekleri Linux tabanlı kümelerde __ /usr/hdp/current/storm-client/contrib/storm-starter__ dizininde ve Windows tabanlı kümelerde **%storm_home%\contrib\storm-starter** dizininde bulunur.

##Bazı ortak geliştirme desenleri nelerdir?

###Garantili ileti işleme

Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir Storm uygulaması en az bir kez işlemeyi, Trident ise tam bir kez işlemeyi garanti edebilir.

Daha fazla bilgi için apache.org sayfasındaki [Veri işleme garantileri](https://storm.apache.org/about/guarantees-data-processing.html) bölümüne bakın.

###IBasicBolt

Bir girdi tanımlama grubunu okuma, sıfır veya daha fazla tanımlama grubu yayma ve ardından girdi tanımlama grubuna yürütme yönteminin hemen sonrasında ack uygulama deseni oldukça yaygındır ve Storm bu deseni otomatik hale getirmek için [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) arabirimini sağlar.

###Birleştirme

İki veri akışının birleştirilmesi uygulamalar arasında farklılık gösterir. Örneğin, yeni birden fazla akıştaki her bir tanımlama grubunu yeni bir akışta birleştirebilir veya yalnızca toplu tanımlama gruplarını belirli bir pencere için birleştirebilirsiniz. Her iki işlem de tanımlama gruplarının cıvatalara nasıl yönlendirileceğini tanımlayan [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) yöntemiyle gerçekleştirilebilir.

Aşağıdaki Java örneğinde fieldsGrouping, "1", "2" ve "3" bileşenlerinden kaynaklanan tanımlama gruplarını **MyJoiner** cıvatasına yönlendirmek için kullanılır.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

###Toplu İşleme

Toplu işleme çeşitli yollardan gerçekleştirilebilir. Temel bir Storm Java topolojisi ile tanımlama gruplarını yaymadan önce X sayıda tanımlama grubunu toplu işlemek üzere basit bir sayaç kullanabilir veya "değer çizgisi tanımlama grubu" olarak bilinen iç zamanlama mekanizmasını kullanarak X saniyede bir toplu iş yayabilirsiniz.

Değer çizgisi tanımlama gruplarını kullanma örneği için bkz. [HDInsight üzerinde Storm ve HBase ile sensör verilerini analiz etme](hdinsight-storm-sensor-data-analysis.md).

Trident kullanıyorsanız bu durum tanımlama grubu toplu işlerini işlemeye bağlıdır.

###Önbelleğe alma

Bellek içi önbelleğe alma, sık kullanılan varlıkları bellekte tuttuğu için çoğunlukla işlemeyi hızlandırmaya yönelik bir mekanizma olarak kullanılır. Bir topoloji birden fazla düğüme ve her bir düğümdeki birden fazla işleme dağıtıldığı için önbellek araması için kullanılan alanları içeren tanımlama gruplarının her zaman aynı işleme yönlendirildiğinden emin olmak için [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) kullanmayı düşünmeniz gerekir. Bunun yapılması süreçler arasında önbellek girişlerinin yinelenmesini önler.

###İlk N akışı

Topolojiniz Twitter’daki ilk 5 trend gibi "ilk N" değeri hesaplamaya bağlıysa ilk N değeri paralel olarak hesaplamanız ve ardından bu hesaplamaların çıktısını genel bir değerde birleştirmeniz gerekir. Bu işlem alanın paralel cıvatalara (verileri alan değerine göre böler) ve ardından ilk N değeri genel olarak belirleyen bir cıvataya yönlendirilmesi için [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) kullanılarak yapılabilir.

Bunun bir örneği için [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java) örneği.

##Sonraki adımlar

HDInsight’ta Apache Storm ile gerçek zamanlı analiz çözümleri hakkında daha fazla bilgi edinin:

* [HDInsight üzerinde Storm Kullanmaya Başlama][gettingstarted]

* [HDInsight üzerinde Storm için örnek topolojiler](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.incubator.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.incubator.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md



<!----HONumber=Jun16_HO2-->


