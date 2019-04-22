---
title: Apache Storm - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede Apache Storm kullanarak olayları Azure Event Hubs'dan olay alma konusunda bilgi sağlanır.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: java
ms.devlang: multiple
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 75a96127c48186befc48b2240f78e49cd5914239
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59679270"
---
# <a name="receive-events-from-event-hubs-using-apache-storm"></a>Apache Storm kullanarak Event Hubs'dan olayları alma

[Apache Storm](https://storm.incubator.apache.org) güvenilir işlenmesini sınırsız veri akışlarını basitleştiren bir dağıtılmış gerçek zamanlı bir hesaplama sistemidir. Bu bölümde, olayları Event Hubs'dan olay almak için bir Azure olay hub'ları Storm spout kullanmayı gösterir. Apache Storm kullanan, farklı düğümlerde barındırılan birden çok işlem arasında olayları bölebilirsiniz. Storm ile Event Hubs tümleştirme olay tüketiminin şeffaf bir şekilde denetim noktası tarafından Storm'ın Zookeeper yükleme, kalıcı denetim noktalarını yönetme, ilerleme durumunu basitleştirir ve paralel Event Hubs'dan alır.

Event Hubs hakkında daha fazla bilgi için desenler almak için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

## <a name="prerequisites"></a>Önkoşullar
Hızlı Başlangıç ile başlamadan önce **bir Event Hubs ad alanı ve bir olay hub'ı oluşturma**. Kullanım [Azure portalında](https://portal.azure.com) Event Hubs türünde bir ad alanı oluşturma, ardından uygulamanızın olay hub'ı ile iletişim kurmak için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md). 

## <a name="create-project-and-add-code"></a>Proje oluşturma ve kod ekleme

Bu öğreticide bir [HDInsight Storm] [ HDInsight Storm] zaten kullanılabilir Event Hubs spout ile birlikte gelen yükleme.

1. İzleyin [HDInsight Storm - Get Started](../hdinsight/storm/apache-storm-overview.md) yeni bir HDInsight kümesi oluşturma ve Uzak Masaüstü aracılığıyla bağlanmak için yordamı.
2. Kopyalama `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` yerel geliştirme ortamınıza dosya. Bu olayları storm spout içerir.
3. Paketi yerel Maven deposuna yüklemek için aşağıdaki komutu kullanın. Bu, sonraki bir adımda Storm projedeki bir başvuruyu olarak eklemenize olanak sağlar.

    ```shell
    mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar
    ```
4. Eclipse'te, yeni bir Maven projesi oluşturun (tıklayın **dosya**, ardından **yeni**, ardından **proje**).
   
    ![Dosya -> Yeni Proje ->][12]
5. Seçin **varsayılan çalışma alanı konumu kullanın**, ardından **İleri**
6. Seçin **maven archetype hızlı** archetype, ardından **İleri**
7. INSERT bir **GroupID** ve **Artifactıd**, ardından **son**
8. İçinde **pom.xml**, aşağıdaki bağımlılıkları ekleyin `<dependency>` düğümü.

    ```xml  
    <dependency>
        <groupId>org.apache.storm</groupId>
        <artifactId>storm-core</artifactId>
        <version>0.9.2-incubating</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>com.microsoft.eventhubs</groupId>
        <artifactId>eventhubs-storm-spout</artifactId>
        <version>0.9</version>
    </dependency>
    <dependency>
        <groupId>com.netflix.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>1.3.3</version>
        <exclusions>
            <exclusion>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
        </exclusions>
        <scope>provided</scope>
    </dependency>
    ```

9. İçinde **src** klasöründe adlı bir dosya oluşturun **Config.properties** ve aşağıdakini kopyalayın değiştirerek içerik `receive rule key` ve `event hub name` değerleri:

    ```java
    eventhubspout.username = ReceiveRule
    eventhubspout.password = {receive rule key}
    eventhubspout.namespace = ioteventhub-ns
    eventhubspout.entitypath = {event hub name}
    eventhubspout.partitions.count = 16
       
    # if not provided, will use storm's zookeeper settings
    # zookeeper.connectionstring=localhost:2181
       
    eventhubspout.checkpoint.interval = 10
    eventhub.receiver.credits = 10
    ```
    Değeri **eventhub.receiver.credits** Storm işlem hattına bırakmadan önce kaç tane olay toplu belirler. Basitleştirmek amacıyla, bu örnekte bu değer 10'a ayarlar. Üretim ortamında, bunu genellikle yüksek değerler için ayarlanmalıdır; Örneğin, 1024.
10. Adlı yeni bir sınıf oluşturma **LoggerBolt** aşağıdaki kod ile:
    
    ```java
    import java.util.Map;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import backtype.storm.task.OutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichBolt;
    import backtype.storm.tuple.Tuple;
    
    public class LoggerBolt extends BaseRichBolt {
        private OutputCollector collector;
        private static final Logger logger = LoggerFactory
                  .getLogger(LoggerBolt.class);
    
        @Override
        public void execute(Tuple tuple) {
            String value = tuple.getString(0);
            logger.info("Tuple value: " + value);
   
            collector.ack(tuple);
        }
   
        @Override
        public void prepare(Map map, TopologyContext context, OutputCollector collector) {
            this.collector = collector;
            this.count = 0;
        }
        
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            // no output fields
        }
    
    }
    ```
    
    Bu Storm bolt, alınan olayların içeriğini günlüğe kaydeder. Bu kolayca bir depolama hizmetine tanımlama grupları depolamak için genişletilebilir. [Olay hub'ı örneği ile HDInsight Storm] Azure depolama ve Power BI ile verileri depolamak için bu yaklaşımı kullanır.
11. Adlı bir sınıf oluşturmak **LogTopology** aşağıdaki kod ile:
    
    ```java
    import java.io.FileReader;
    import java.util.Properties;
    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.generated.StormTopology;
    import backtype.storm.topology.TopologyBuilder;
    import com.microsoft.eventhubs.samples.EventCount;
    import com.microsoft.eventhubs.spout.EventHubSpout;
    import com.microsoft.eventhubs.spout.EventHubSpoutConfig;
        
    public class LogTopology {
        protected EventHubSpoutConfig spoutConfig;
        protected int numWorkers;
        
        protected void readEHConfig(String[] args) throws Exception {
            Properties properties = new Properties();
            if (args.length > 1) {
                properties.load(new FileReader(args[1]));
            } else {
                properties.load(EventCount.class.getClassLoader()
                        .getResourceAsStream("Config.properties"));
            }
        
            String username = properties.getProperty("eventhubspout.username");
            String password = properties.getProperty("eventhubspout.password");
            String namespaceName = properties
                    .getProperty("eventhubspout.namespace");
            String entityPath = properties.getProperty("eventhubspout.entitypath");
            String zkEndpointAddress = properties
                    .getProperty("zookeeper.connectionstring"); // opt
            int partitionCount = Integer.parseInt(properties
                    .getProperty("eventhubspout.partitions.count"));
            int checkpointIntervalInSeconds = Integer.parseInt(properties
                    .getProperty("eventhubspout.checkpoint.interval"));
            int receiverCredits = Integer.parseInt(properties
                    .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                // (opt)
            System.out.println("Eventhub spout config: ");
            System.out.println("  partition count: " + partitionCount);
            System.out.println("  checkpoint interval: "
                    + checkpointIntervalInSeconds);
            System.out.println("  receiver credits: " + receiverCredits);
     
            spoutConfig = new EventHubSpoutConfig(username, password,
                    namespaceName, entityPath, partitionCount, zkEndpointAddress,
                    checkpointIntervalInSeconds, receiverCredits);
        
            // set the number of workers to be the same as partition number.
            // the idea is to have a spout and a logger bolt co-exist in one
            // worker to avoid shuffling messages across workers in storm cluster.
            numWorkers = spoutConfig.getPartitionCount();
        
            if (args.length > 0) {
                // set topology name so that sample Trident topology can use it as
                // stream name.
                spoutConfig.setTopologyName(args[0]);
            }
        }
        
        protected StormTopology buildTopology() {
            TopologyBuilder topologyBuilder = new TopologyBuilder();
       
            EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
            topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                    spoutConfig.getPartitionCount()).setNumTasks(
                    spoutConfig.getPartitionCount());
            topologyBuilder
                    .setBolt("LoggerBolt", new LoggerBolt(),
                            spoutConfig.getPartitionCount())
                    .localOrShuffleGrouping("EventHubsSpout")
                    .setNumTasks(spoutConfig.getPartitionCount());
            return topologyBuilder.createTopology();
        }
        
        protected void runScenario(String[] args) throws Exception {
            boolean runLocal = true;
            readEHConfig(args);
            StormTopology topology = buildTopology();
            Config config = new Config();
            config.setDebug(false);
        
            if (runLocal) {
                config.setMaxTaskParallelism(2);
                LocalCluster localCluster = new LocalCluster();
                localCluster.submitTopology("test", config, topology);
                Thread.sleep(5000000);
                localCluster.shutdown();
            } else {
                config.setNumWorkers(numWorkers);
                StormSubmitter.submitTopology(args[0], config, topology);
            }
        }
        
        public static void main(String[] args) throws Exception {
            LogTopology topology = new LogTopology();
            topology.runScenario(args);
        }
    }
    ```

    Bu sınıfın örneğini başlatmak için yapılandırma dosyasında özelliklerini kullanarak yeni bir Event Hubs spout oluşturur. Bu örnek çok spout görevleri olarak olay hub'ındaki bölüm sayısı, olay hub'ı tarafından izin verilen en büyük paralellik kullanmak için oluşturduğu dikkat edin önemlidir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs'a genel bakış][Event Hubs overview]
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[HDInsight Storm]: ../hdinsight/storm/apache-storm-overview.md
[Olay hub'ı örneği ile HDInsight Storm]: https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/

<!-- Images -->

[12]: ./media/event-hubs-get-started-receive-storm/create-storm1.png
