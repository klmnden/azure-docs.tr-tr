---
title: "Apache Storm örnek Java topolojisi - Azure Hdınsight | Microsoft Docs"
description: "Bir örnek word count topolojisi oluşturarak Java'da Apache Storm topolojilerini oluşturmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "Apache storm, apache storm örnek, storm java, storm topoloji örneği"
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: ca566aed706d4598c6067d42bdbec08d16dc3841
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a>Apache Storm topolojisini Java oluşturma

Apache Storm için Java tabanlı bir topoloji oluşturmayı öğrenin. Word-count uygulama uygulayan bir Storm topolojisinin oluşturun. Maven oluşturun ve projeyi paketini kullanın. Ardından, nasıl Flux framework kullanarak topolojisi tanımlayacağınızı öğrenin.

> [!NOTE]
> Flux Storm 0.10.0 veya sonraki sürümlerinde çerçevedir. Storm 0.10.0 Hdınsight 3.3 ve 3.4 ile kullanılabilir.

Bu belgedeki adımları tamamladıktan sonra Hdınsight üzerinde Apache Storm topolojisini dağıtabilirsiniz.

> [!NOTE]
> Bu belgede oluşturulan Storm topolojisini örnekler tamamlanmış bir sürümünü şu adresten edinilebilir [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

## <a name="prerequisites"></a>Ön koşullar

* [Java Geliştirme Seti (JDK) sürüm 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

* [Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven Java projeleri için bir proje derleme sistemidir.

* Bir metin düzenleyicisi veya IDE.

## <a name="configure-environment-variables"></a>Ortam değişkenleri yapılandırın

Java ve JDK yüklediğinizde aşağıdaki ortam değişkenleri ayarlayabilirsiniz. Ancak, bunlar mevcut olduğundan ve sisteminiz için doğru değerleri içerdikleri denetlemeniz gerekir.

* **JAVA_HOME** -Java Çalışma zamanı ortamı (JRE) yüklü olduğu dizine işaret etmelidir. Örneğin, bir UNIX veya Linux dağıtımlarında benzeri bir değer olması gereken `/usr/lib/jvm/java-8-oracle`. Windows'da benzeri bir değer gerekir`c:\Program Files (x86)\Java\jre1.8`

* **YOL** -aşağıdaki yolları içermelidir:

  * **JAVA_HOME** (veya eşdeğer yolu)

  * **JAVA_HOME\bin** (veya eşdeğer yolu)

  * Maven'ın yüklendiği dizin

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturun

Komut satırından adlı bir Maven projesi oluşturmak için aşağıdaki komutu kullanın. **WordCount**:

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> PowerShell kullanıyorsanız, çevreleyen gerekir`-D` çift tırnak işareti parametrelerle.
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

Bu komut adlı bir dizin oluşturur `WordCount` geçerli konumda içeren temel bir Maven projesi. `WordCount` Dizini aşağıdaki öğeleri içerir:

* `pom.xml`: Bir Maven projesi için ayarları içerir.
* `src\main\java\com\microsoft\example`: Uygulama kodunuz içerir.
* `src\test\java\com\microsoft\example`: Uygulamanız için testleri içerir. 

### <a name="remove-the-generated-example-code"></a>Oluşturulan örnek kodu Kaldır

Oluşturulan test ve uygulama dosyalarını silin:

* **src\test\java\com\microsoft\example\AppTest.Java**
* **src\main\java\com\microsoft\example\App.Java**

## <a name="add-maven-repositories"></a>Maven depoları ekleme

Hdınsight Hortonworks veri Platformu (HDP) üzerinde dayalı, bu yüzden Apache Storm projelerinizi bağımlılıklarını karşıdan yüklemek için Hortonworks depo kullanmanızı öneririz. İçinde __pom.xml__ dosya, aşağıdaki XML'i ekleyin sonra `<url>http://maven.apache.org</url>` satır:

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Özellikler ekleme

Maven özellikleri olarak adlandırılan proje düzeyi değerleri tanımlamanızı sağlar. İçinde __pom.xml__, aşağıdaki metinden sonra Ekle `</repositories>` satır:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from the Hortonworks repository that is compatible with HDInsight 3.6.
    -->
    <storm.version>1.1.0.2.6.1.9-1</storm.version>
</properties>
```

Bu değer diğer bölümlerinde artık kullanabilirsiniz `pom.xml`. Örneğin, Storm bileşenleri belirtirken kullanabileceğiniz `${storm.version}` sabit bir değer kodlama yerine.

## <a name="add-dependencies"></a>Bağımlılıkları ekleyin.

Storm bileşenleri için bağımlılık ekleyin. Açık `pom.xml` dosya ve aşağıdaki kodu ekleyin `<dependencies>` bölümü:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of the jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

Derleme zamanında Maven aramak için bu bilgileri kullanır `storm-core` Maven deposunda. Yerel bilgisayarınızda depodaki İlk bakar. Dosya yoksa, Maven ortak Maven depodan yükler ve yerel depoda depolar.

> [!NOTE]
> Bildirim `<scope>provided</scope>` bu bölümdeki satır. Bu ayar dışlamak için Maven söyler **storm çekirdek** sistem tarafından sağlanan bulunduğundan, oluşturulan JAR dosyalarını.

## <a name="build-configuration"></a>Derleme yapılandırması

Maven eklentileri projeyi derleme aşamaları özelleştirmenizi sağlar. Örneğin, projenin nasıl derlenmiş veya JAR dosyasına paketlemek nasıl. Açık `pom.xml` dosya ve doğrudan yukarıdaki aşağıdaki kodu ekleyin `</project>` satır.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Bu bölümde, eklentiler, kaynakları ve diğer yapı yapılandırma seçeneklerini eklemek için kullanılır. Bir tam başvuru için **pom.xml** dosya için bkz: [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

### <a name="add-plug-ins"></a>Eklentiler

Apache Storm topolojilerini Java'da, uygulanan için [Exec Maven eklentisi](http://www.mojohaus.org/exec-maven-plugin/) kolayca topolojisi geliştirme ortamınızı yerel olarak çalıştırmanızı izin verdiği için kullanışlıdır. Aşağıdakileri ekleyin `<plugins>` bölümünü `pom.xml` Exec Maven eklentisi eklenecek dosyası:

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.5.0</version>
    <executions>
        <execution>
        <goals>
            <goal>exec</goal>
        </goals>
        </execution>
    </executions>
    <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
        <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

Başka bir eklenti yararlı [Apache Maven derleyici eklentisi](http://maven.apache.org/plugins/maven-compiler-plugin/), derleme seçeneklerini değiştirmek için kullanılır. Java değişiklikleri kaynak ve hedef uygulamanız için Maven kullanımları sürümü.

* Hdınsight için __3.4 veya önceki__, kaynak ayarlayabilir ve Java sürümü için hedef __1.7__.

* Hdınsight için __3.5__, kaynak ayarlayabilir ve Java sürümü için hedef __1.8__.

Aşağıdaki metni eklemek `<plugins>` bölümünü `pom.xml` Apache Maven derleyici eklentisi eklenecek dosyası. Hedef Hdınsight sürüm 3.5 olması için bu örnek 1.8, belirtir.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a>Kaynaklarını yapılandırma

Kaynakları bölümü kod olmayan kaynakları topolojideki bileşenleri tarafından gereken yapılandırma dosyaları gibi eklemenizi sağlar. Bu örnek için aşağıdaki metni eklemek `<resources>` bölümünü ' pom.xml dosyasını.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

Bu örnek kaynaklar directory proje kök dizininde ekler (`${basedir}`) kaynaklar içeriyor ve adlı dosyayı içeren bir konum olarak `log4j2.xml`. Bu dosya, hangi bilgilerin topolojisi tarafından kaydedilir yapılandırmak için kullanılır.

## <a name="create-the-topology"></a>Topoloji oluşturma

Java tabanlı Apache Storm topolojisini bağımlılık olarak yazmanız gerekir üç bileşeni (veya başvuru) oluşur.

* **Spout'lar**: dış veri kaynakları ve veri akışları topoloji yayar okur.

* **Cıvatalar**: spout'lar veya diğer Cıvatalar tarafından gösterilen akışları üzerinde işlemeyi gerçekleştirir ve bir veya daha fazla akışları yayar.

* **Topoloji**: nasıl spout'lar Cıvatalar düzenlenir ve giriş noktası için topoloji sunar tanımlar.

### <a name="create-the-spout"></a>Spout oluşturma

Dış veri kaynakları için gereksinimler azaltmak için aşağıdaki spout yalnızca rastgele cümleleri yayar. İle sağlanan bir spout değiştirilmiş bir sürümünü olan [Storm Starter örnekleri](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [!NOTE]
> Bir dış veri kaynağından okur spout bir örnek için aşağıdaki örneklerde birine bakın:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): Twitter'dan okuyan bir örnek spout
> * [Storm Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): Kafka okur spout

Spout için adlı bir dosya oluşturun `RandomSentenceSpout.java` içinde `src\main\java\com\microsoft\example` dizin ve kullanım aşağıdaki Java kod içeriği:

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used to emit output
  SpoutOutputCollector _collector;
  //Used to generate a random number
  Random _rand;

  //Open is called when an instance of the class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set the instance collector to the one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data to the stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //The sentences that are randomly emitted
    String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
        "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit the sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare the output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> Bu topoloji yalnızca bir spout kullansa da, diğerlerinin birkaç topoloji farklı kaynaklardan veri akışı olabilir.

### <a name="create-the-bolts"></a>Cıvatalar oluşturma

Cıvatalar veri işleme işleyin. Bu topoloji iki Cıvatalar kullanır:

* **SplitSentence**: tarafından gösterilen cümleleri böler **RandomSentenceSpout** ayrı sözcükleri içine.

* **WordCount**: her sözcüğün oluştu kaç kez sayar.

> [!NOTE]
> Cıvatalar hiçbir şey, örneğin, hesaplama, sürdürme veya dış bileşenlere Konuşmayı yapabilirsiniz.

İki yeni dosyalar oluşturma `SplitSentence.java` ve `WordCount.java` içinde `src\main\java\com\microsoft\example` dizin. Aşağıdaki metin dosyalarını içeriği kullanın:

#### <a name="splitsentence"></a>SplitSentence

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get the sentence content from the tuple
    String sentence = tuple.getString(0);
    //An iterator to get each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give the iterator the sentence
    boundary.setText(sentence);
    //Find the beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it to the output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get the word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a>WordCount

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often to emit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default to 60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used to trigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get the word contents from the tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment the count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-the-topology"></a>Topolojisi tanımlayın

Spout'lar bağlar ve bileşenler arasında veri akışını tanımlayan bir grafik içine birlikte Cıvatalar topoloji. Ayrıca, Storm bileşenleri kümedeki örneklerini oluştururken kullandığı paralellik ipuçlarını sağlar.

Aşağıdaki resimde, grafik bileşenlerinin bu topoloji için temel bir diyagramıdır.

![spout'lar ve Cıvatalar düzenlemesini gösteren diyagram](./media/apache-storm-develop-java-topology/wordcount-topology.png)

Topoloji uygulamak için adlı bir dosya oluşturun `WordCountTopology.java` içinde `src\main\java\com\microsoft\example` dizin. Aşağıdaki Java kod dosyasının içeriği kullanın:

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for the topology
  public static void main(String[] args) throws Exception {
  //Used to build the topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add the spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add the SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes to the spout, and equally distributes
    //tuples (sentences) across instances of the SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add the counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes to the split bolt, and
    //ensures that the same word is sent to the same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set to false to disable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint to set the number of workers
      conf.setNumWorkers(3);
      //submit the topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap the maximum number of executors that can be spawned
      //for a component to 3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used to run locally
      LocalCluster cluster = new LocalCluster();
      //submit the topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down the cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a>Günlük tutmayı yapılandırma

Storm bilgileri günlüğe kaydetmek için Apache Log4j kullanır. Günlük kaydını yapılandırmazsanız topoloji tanılama bilgisi yayar. Günlüğe kaydedilenler denetlemek için adlı bir dosya oluşturun `log4j2.xml` içinde `resources` dizin. Aşağıdaki XML dosyasının içeriği kullanın.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

Yeni bir Günlükçü için bu XML yapılandırır `com.microsoft.example` Bu örnek topolojide bileşenlerini içeren sınıf. Düzeyi izleme bu topolojide bileşenleri tarafından gösterilen tüm günlük bilgilerini yakalar bu Günlükçü için ayarlanır.

`<Root level="error">` Bölümü kök düzeyini yapılandırır (içinde değil her şeyi `com.microsoft.example`) yalnızca hata bilgileri günlüğe kaydetmek için.

Log4j için günlüğe kaydetmeyi yapılandırma hakkında daha fazla bilgi için bkz: [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [!NOTE]
> Storm sürüm 0.10.0 ve daha yüksek kullanım Log4j 2.x. Storm eski sürümlerinde kullanılan Log4j günlük yapılandırması için farklı bir biçim kullanılan 1.x. Eski yapılandırma hakkında daha fazla bilgi için bkz: [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

## <a name="test-the-topology-locally"></a>Topoloji yerel olarak test etme

Dosyaları kaydettikten sonra topoloji yerel olarak test etmek için aşağıdaki komutu kullanın.

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

Çalışırken, topoloji başlangıç bilgileri görüntüler. Aşağıdaki metni word sayısı çıkış örneğidir:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Bu örnek günlük belirten word 've' 113 kez yayılan. Sayı spout sürekli olarak aynı cümleleri yayar çünkü topoloji çalıştığı sürece gidebilir devam eder.

Sözcükler yayımlanmasını ve sayılar arasında 5 saniye aralığını yoktur. **WordCount** bileşen değer çizgisi tanımlama grubu geldiğinde bilgileri yalnızca yaymak üzere yapılandırılır. Diziler yalnızca beş saniyede teslim edilir, onay ister.

## <a name="convert-the-topology-to-flux"></a>Topoloji Flux için Dönüştür

Flux, uygulama yapılandırmasından ayırmanıza olanak sağlayan bir yeni kullanılabilir Storm 0.10.0 veya üzeri bir çerçevedir. Bileşenlerinizi Java'da tanımlanmış olan ancak topoloji YAML dosyası kullanılarak tanımlanır. Projenizi ile varsayılan topoloji tanımı paketini veya tek başına dosya topoloji gönderirken kullanın. Storm için topoloji gönderirken YAML topoloji tanımı değerleri doldurmak için ortam değişkenleri veya yapılandırma dosyalarını kullanabilirsiniz.

Topoloji ve verileri için kullanılacak bileşenleri YAML dosyası tanımlar aralarında akış. Bir YAML dosyası jar dosyasını bir parçası olarak ekleyebilirsiniz veya dış YAML dosyası kullanabilirsiniz.

Flux hakkında daha fazla bilgi için bkz: [Flux framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

> [!WARNING]
> Verilecek bir [hata (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) Storm 1.0.1 yüklemeniz gerekebilir bir [Storm geliştirme ortamı](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) Flux topolojileri yerel olarak çalıştırmak için.

1. Taşıma `WordCountTopology.java` dosya proje dışında. Daha önce bu dosyayı topoloji tanımlı, ancak Flux ile gerekli değildir.

2. İçinde `resources` dizin adlı bir dosya oluşturun `topology.yaml`. Aşağıdaki metni bu dosyanın içeriğini kullanın.

    ```yaml
    name: "wordcount"       # friendly name for the topology

    config:                 # Topology configuration
      topology.workers: 1     # Hint for the number of workers to create

    spouts:                 # Spout definitions
    - id: "sentence-spout"
      className: "com.microsoft.example.RandomSentenceSpout"
      parallelism: 1      # parallelism hint

    bolts:                  # Bolt definitions
    - id: "splitter-bolt"
      className: "com.microsoft.example.SplitSentence"
      parallelism: 1
        
    - id: "counter-bolt"
      className: "com.microsoft.example.WordCount"
      constructorArgs:
        - 10
      parallelism: 1

    streams:                # Stream definitions
    - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
      from: "sentence-spout"       # The stream emitter
      to: "splitter-bolt"          # The stream consumer
      grouping:                    # Grouping type
        type: SHUFFLE
    
    - name: "Splitter -> Counter"
      from: "splitter-bolt"
      to: "counter-bolt"
      grouping:
        type: FIELDS
        args: ["word"]           # field(s) to group on
    ```

3. Aşağıdaki değişiklikleri yapın `pom.xml` dosya.
   
   * Aşağıdaki yeni bağımlılık olarak ekleme `<dependencies>` bölümü:
     
        ```xml
        <!-- Add a dependency on the Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * Aşağıdaki eklenti ekleme `<plugins>` bölümü. Bu eklenti projesi için bir paket (jar dosyasını) oluşturulmasını işler ve belirli bazı dönüşümleri paket oluştururken Flux için geçerlidir.
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer to it as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
                </transformers>
                <!-- Keep us from getting a bad signature error -->
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```

   * İçinde **exec maven eklentisi** `<configuration>` bölümünde, değerini değiştirin `<mainClass>` için `org.apache.storm.flux.Flux`. Bu ayar topolojisi geliştirme yerel olarak çalışan işlemeye Flux sağlar.

   * İçinde `<resources>` bölümünde, aşağıdakileri ekleyin `<includes>`. Bu XML topoloji projenin bir parçası tanımlayan YAML dosyası içerir.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-the-flux-topology-locally"></a>Flux topoloji yerel olarak test etme

1. Maven kullanarak Flux topolojisi derleyip için aşağıdakileri kullanın:

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    PowerShell kullanıyorsanız, aşağıdaki komutu kullanın:

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > Topolojiniz Storm 1.0.1 BITS kullanıyorsa, bu komut başarısız olur. Bu hatanın nedeni [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055). Bunun yerine, [geliştirme ortamınızda Storm yüklemek](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) ve aşağıdaki adımları kullanın:
    >
    > Varsa [Storm geliştirme ortamınızda yüklü](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), bunun yerine aşağıdaki komutları kullanın:
    >
    > ```bash
    > mvn compile package
    > storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    > ```

    `--local` Parametre yerel modda topolojisi geliştirme ortamınızı çalıştırır. `-R /topology.yaml` Parametresini kullanır `topology.yaml` topoloji tanımlamak için jar dosyasından kaynak dosya.

    Çalışırken, topoloji başlangıç bilgileri görüntüler. Aşağıdaki metni çıkış örneğidir:

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    Günlüğe kaydedilen bilgileri toplu işlemleri arasında 10 saniye gecikme olur.

2. Bir kopyasını `topology.yaml` proje dosyasından. Yeni dosya adı `newtopology.yaml`. İçinde `newtopology.yaml` dosya, aşağıdaki bölümü bulun ve değerini değiştirme `10` için `5`. Bu değişikliği 5 için 10 saniye gelen sözcük sayıları verme toplu arasındaki aralığı değiştirir.

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. To run the topology, use the following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    Veya, geliştirme ortamınızı Storm varsa:

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    Değişiklik `/path/to/newtopology.yaml` önceki adımda oluşturduğunuz newtopology.yaml dosyasının yolu. Bu komut newtopology.yaml topoloji tanımı olarak kullanır. Biz eklemediniz beri `compile` parametresi, Maven, önceki adımda yapılandırdığınız projenin sürümü kullanır.

    Topoloji başladıktan sonra verilmiş toplu işlemleri arasındaki süre newtopology.yaml değerinde yansıtmak üzere değiştirilmiştir dikkat etmelidir. Bu nedenle, yapılandırmanızı YAML dosyası aracılığıyla topoloji yeniden derlemenize gerek kalmadan değiştirebileceğiniz olduğunu görebilirsiniz.

Bunlar ve diğer özellikler Flux framework'ün hakkında daha fazla bilgi için bkz: [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

## <a name="trident"></a>Trident

Trident Storm tarafından sağlanan üst düzey bir soyutlamadır. Durum bilgisi olan işlemeyi destekler. Trident birincil avantajı, topoloji girer her ileti yalnızca bir kez işlenir garanti edebilir ' dir. Trident kullanmadan topolojinizi yalnızca iletileri en az bir kez işlenir garanti edebilir. Cıvatalar oluşturmak yerine kullanılabilir yerleşik bileşenleri gibi diğer farklar vardır. Aslında, Cıvatalar filtreleri, tahminleri ve işlevleri gibi daha az genel bileşenler tarafından değiştirilir.

Trident uygulamaları Maven projelerini kullanarak oluşturulabilir. Aynı temel adımlar bu makalenin önceki bölümlerinde sunulan gibi kullandığınız — yalnızca kodu farklı. Trident de (şu anda) Flux framework ile kullanılamaz.

Trident hakkında daha fazla bilgi için bkz: [Trident API genel bakış](http://storm.apache.org/documentation/Trident-API-Overview.html).

## <a name="next-steps"></a>Sonraki Adımlar

Java kullanarak bir Storm topolojisinin oluşturma öğrendiniz. Daha fazla bilgi nasıl yapılır:

* [Dağıtma ve Hdınsight üzerinde Apache Storm topolojilerini yönetme](apache-storm-deploy-monitor-topology.md)

* [Visual Studio kullanarak Hdınsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md)

Daha fazla örnek Storm topolojileri ziyaret ederek bulabileceğiniz [Hdınsight üzerinde Storm için örnek topolojiler](apache-storm-example-topology.md).

