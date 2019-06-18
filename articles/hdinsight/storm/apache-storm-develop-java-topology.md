---
title: Apache Storm örnek Java topolojisi - Azure HDInsight
description: Apache Storm topolojilerini Java'da bir örnek sözcük sayımı topolojisinin oluşturarak oluşturmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
keywords: Apache storm, apache storm örneği, storm, java, storm topolojisi örneği
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 89cee70c9d7c5dffdb3078756cf4fa94d7cd1a9a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078216"
---
# <a name="create-an-apache-storm-topology-in-java"></a>Java'da bir Apache Storm topolojisi oluşturma

Bir Java tabanlı topoloji için oluşturmayı [Apache Storm](https://storm.apache.org/). Burada, bir word-count uygulama uygulayan bir Storm topolojisi oluşturma. Kullandığınız [Apache Maven](https://maven.apache.org/) derlemek ve projeyi paketlemek için. Topolojisi kullanan tanımlamayı öğrenin ardından [Apache Storm Flux](https://storm.apache.org/releases/2.0.0/flux.html) framework.

Bu belgedeki adımları tamamladıktan sonra HDInsight üzerinde Apache Storm topolojisi dağıtabilirsiniz.

> [!NOTE]  
> Bu belgede oluşturulan Storm topolojisi örnekleri tamamlanmış bir sürümünü şu adresten edinilebilir [ https://github.com/Azure-Samples/hdinsight-java-storm-wordcount ](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

## <a name="prerequisites"></a>Önkoşullar

* [Java Developer Kit (JDK) 8 sürümü](https://aka.ms/azure-jdks)

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün [yüklü](https://maven.apache.org/install.html) Apache göre.  Maven derleme sistemi Java projeleri için proje olur.

## <a name="test-environment"></a>Test ortamı
Windows 10 çalıştıran bir bilgisayar için bu makalede kullanılan ortamı oluştu.  Komutları komut isteminde yürütüldü ve çeşitli dosyaların Notepad ile düzenlendi.

Bir komut isteminden çalışma ortamı oluşturmak için aşağıdaki komutları girin:

```cmd
mkdir C:\HDI
cd C:\HDI
```

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturun

Adlı bir Maven projesi oluşturmak için aşağıdaki komutu girin **WordCount**:

```cmd
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

cd WordCount
mkdir resources
```

Bu komut, adlı bir dizin oluşturur. `WordCount` geçerli konumunda, temel bir Maven projesi içerir. İkinci komut, mevcut çalışma dizinini değiştirir `WordCount`. Üçüncü komut yeni bir dizin oluşturur `resources`, doğrulayacak daha sonra.  `WordCount` Dizini aşağıdaki öğeleri içerir:

* `pom.xml`: Maven projesi ayarlarını içerir.
* `src\main\java\com\microsoft\example`: Uygulama kodunuza içerir.
* `src\test\java\com\microsoft\example`: Uygulamanız için testleri içerir.  

### <a name="remove-the-generated-example-code"></a>Oluşturulan örnek kodu kaldırın

Oluşturulan test ve uygulama dosyalarını silin `AppTest.java`, ve `App.java` aşağıdaki komutları girerek:

```cmd
DEL src\main\java\com\microsoft\example\App.java
DEL src\test\java\com\microsoft\example\AppTest.java
```

## <a name="add-maven-repositories"></a>Maven deposu ekleme

HDInsight, Apache Storm projeleriniz için bağımlılıklar indirmek için Hortonworks depo kullanmanızı öneririz, böylece Hortonworks Data Platform (HDP) üzerinde temel alır.  

Açık `pom.xml` aşağıdaki komutu girerek:

```cmd
notepad pom.xml
```

Sonra aşağıdaki XML eklersiniz `<url> https://maven.apache.org</url>` satırı:

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
        <url>https://repo.hortonworks.com/content/repositories/releases/</url>
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
        <url>https://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Özellikler ekleme

Maven, proje düzeyi değerleri özellikler olarak adlandırılan tanımlamanızı sağlar. İçinde `pom.xml`, sonra aşağıdaki metni ekleyin `</repositories>` satırı:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from the Hortonworks repository that is compatible with HDInsight 3.6.
    -->
    <storm.version>1.1.0.2.6.1.9-1</storm.version>
</properties>
```

Bu değer artık diğer bölümlerinde kullanabilirsiniz `pom.xml`. Örneğin, Storm bileşenleri belirtirken kullanabileceğiniz `${storm.version}` değeri sabit kodlama yerine.

## <a name="add-dependencies"></a>Bağımlılıkları ekleyin

Storm bileşenleri için bağımlılık ekleme. İçinde `pom.xml`, aşağıdaki metni ekleyin `<dependencies>` bölümü:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of the jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

Derleme zamanında Maven aramak için bu bilgileri kullanır `storm-core` Maven deposunda. Yerel bilgisayarınızda depodaki önce arar. Dosya yoksa, Maven bunları genel Maven deposundan indirir ve bunları yerel depoda depolar.

> [!NOTE]  
> Bildirim `<scope>provided</scope>` bu bölümdeki satır. Bu ayar hariç tutmak için Maven söyler **storm çekirdek** gelen sistem tarafından sağlandığından, oluşturulan JAR dosyaları.

## <a name="build-configuration"></a>Derleme yapılandırması

Maven eklentileri proje derleme aşamalarında özelleştirmenizi sağlar. Örneğin, nasıl proje derlendiğinde veya nasıl bir JAR dosyasına paketleyin. İçinde `pom.xml`, doğrudan yukarıdaki aşağıdaki metni ekleyin `</project>` satır.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Bu bölümde, eklentiler, kaynaklar ve diğer yapı yapılandırma seçenekleri eklemek için kullanılır. Tam bir başvuru için `pom.xml` bkz [ https://maven.apache.org/pom.html ](https://maven.apache.org/pom.html).

### <a name="add-plug-ins"></a>Eklentiler

* **Exec Maven eklentisi**

    Apache Storm topolojilerini Java dilinde uygulanan için [Exec Maven Plugin](https://www.mojohaus.org/exec-maven-plugin/) topoloji geliştirme ortamınızda yerel olarak kolayca çalıştırmanıza izin verdiği için yararlıdır. Ekleyin `<plugins>` bölümünü `pom.xml` Exec Maven plugin dahil edilecek dosyası:
    
    ```xml
    <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.6.0</version>
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

* **Apache Maven derleme eklentisini**

    Başka bir eklentidir yararlı [Apache Maven derleme eklentisini](https://maven.apache.org/plugins/maven-compiler-plugin/), derleme seçeneklerini değiştirmek için kullanılır. Kaynak ve hedef uygulamanız için Maven'ı kullanan Java sürümünü değiştirin.
    
  * HDInsight için __3.4 veya önceki__, kaynağı ve hedef için Java sürümü __1.7__.
    
  * HDInsight için __3.5__, kaynağı ve hedef için Java sürümü __1.8__.
    
    Aşağıdaki metni ekleyin `<plugins>` bölümünü `pom.xml` Apache Maven derleme eklentisini eklemek üzere dosyası. Bu örnekte, hedef HDInsight sürüm 3.5, bu nedenle 1.8, belirtir.
    
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

Kaynak bölümü topolojisindeki bileşenler tarafından gereken yapılandırma dosyaları gibi kod olmayan kaynaklar eklemenize imkan sağlar. Bu örnek için aşağıdaki metni ekleyin `<resources>` bölümünü `pom.xml` dosya.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

Bu örnek kaynaklar dizini, proje kök dizininde ekler (`${basedir}`) kaynakları içeren ve adlı dosyayı içeren bir konum olarak `log4j2.xml`. Bu dosya, hangi bilgilerin topolojiye göre kaydedilir yapılandırmak için kullanılır.

## <a name="create-the-topology"></a>Topoloji oluşturma

Apache Storm, Java tabanlı topoloji, bir bağımlılık olarak yazmanız gerekir üç bileşeni (veya başvuru) oluşur.

* **Spout'lar**: Dış kaynaklardan alınan verileri okur ve veri akışlarını topolojiye yayar.

* **Cıvatalar**: Spout veya diğer boltlardan yayılan akışları üzerinde işlemeyi gerçekleştirir ve bir veya daha fazla akış yayar.

* **Topoloji**: Nasıl spout ve bolt düzenlenir ve giriş noktası için topoloji sağlar tanımlar.

### <a name="create-the-spout"></a>Spout oluşturma

Dış veri kaynağı kurma için gereksinimlerini azaltmak için aşağıdaki spout yalnızca rastgele tümceler yayar. İle sağlanan bir spout değiştirilmiş bir sürümü olan [Storm-Starter örneklerini](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).  Bu topolojinin tek spout kullansa da, diğer birkaç topolojiye farklı kaynaklardan veri akışı olabilir.

Oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `RandomSentenceSpout.java`:

```cmd
notepad src\main\java\com\microsoft\example\RandomSentenceSpout.java
```

Ardından kopyalayıp aşağıdaki java kod yeni dosyasına yapıştırın.  Dosyayı kaydedip kapatın.

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
> Bir dış veri kaynağından okur bir spout örneği için aşağıdaki örneklerde birine bakın:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): Twitter'dan okuyan bir örnek spout.
> * [Storm Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): Kafka'dan okur spout.


### <a name="create-the-bolts"></a>Boltlar oluşturma

Boltlar veri işleme işleyin. Boltlar hiçbir şey, örneğin, hesaplama, Kalıcılık veya dış bileşenleri Konuşmayı yapabilirsiniz. Bu topoloji, iki Cıvatalar kullanır:

* **SplitSentence**: Tarafından yayılan cümleler böler **RandomSentenceSpout** kelimeler içine.

* **WordCount**: Her sözcüğün oluştu kaç kez sayılır.


#### <a name="splitsentence"></a>SplitSentence

Oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `SplitSentence.java`:

```cmd
notepad src\main\java\com\microsoft\example\SplitSentence.java
```

Ardından kopyalayıp aşağıdaki java kod yeni dosyasına yapıştırın.  Dosyayı kaydedip kapatın.

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

Oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `WordCount.java`:

```cmd
notepad src\main\java\com\microsoft\example\WordCount.java
```

Ardından kopyalayıp aşağıdaki java kod yeni dosyasına yapıştırın.  Dosyayı kaydedip kapatın.

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

Spout bölümlere ve bileşenler arasında verilerin nasıl aktığını tanımlayan bir grafik uygulamasına birlikte Cıvatalar topoloji. Ayrıca, Storm bileşenleri kümedeki örneklerini oluştururken kullandığı paralellik ipuçları sağlar.

Aşağıdaki görüntüde bu topoloji bileşenleri grafiğin temel bir diyagramıdır.

![spout ve bolt düzenlemesini gösteren diyagram](./media/apache-storm-develop-java-topology/wordcount-topology.png)

Topolojiyi uygulamak için oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `WordCountTopology.java`:

```cmd
notepad src\main\java\com\microsoft\example\WordCountTopology.java
```

Ardından kopyalayıp aşağıdaki java kod yeni dosyasına yapıştırın.  Dosyayı kaydedip kapatın.

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

Storm kullanan [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) bilgileri günlüğe kaydetmek için. Günlük kaydını yapılandırmazsanız topoloji tanılama bilgilerini gösterir. Günlüğe kaydedilenler denetlemek için adlı bir dosya oluşturun. `log4j2.xml` içinde `resources` dizininden aşağıdaki komutu girerek:

```cmd
notepad resources\log4j2.xml
```

Daha sonra kopyalayın ve yeni dosyaya aşağıdaki XML metni yapıştırın.  Dosyayı kaydedip kapatın.

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

Bu XML için yeni bir Günlükçü yapılandırır `com.microsoft.example` Bu örnek topolojide bileşenleri içeren sınıf. Düzeyi izleme, bu topolojinin bileşenleri tarafından yayılan herhangi bir günlük kaydı bilgilerini yakalar bu Günlükçü için ayarlanır.

`<Root level="error">` Bölüm kök düzeyini yapılandırır (içinde olmayan her şeyi `com.microsoft.example`) yalnızca hata bilgileri günlüğe kaydetmek için.

Log4j 2 için günlüğe kaydetmeyi yapılandırma ile ilgili daha fazla bilgi için bkz: [ https://logging.apache.org/log4j/2.x/manual/configuration.html ](https://logging.apache.org/log4j/2.x/manual/configuration.html).

> [!NOTE]  
> Storm sürümü: 0.10.0 ve daha yüksek kullanım Log4j 2.x. Storm'ın eski sürümlerinde kullanılan Log4j günlük yapılandırması için farklı bir biçim kullanılan 1.x. Eski yapılandırma hakkında daha fazla bilgi için bkz: [ https://wiki.apache.org/logging-log4j/Log4jXmlFormat ](https://wiki.apache.org/logging-log4j/Log4jXmlFormat).

## <a name="test-the-topology-locally"></a>Topoloji yerel olarak test etme

Dosyalar kaydedildikten sonra topoloji yerel olarak test etmek için aşağıdaki komutu kullanın.

```cmd
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

Çalıştığı topoloji başlangıç bilgileri görüntüler. Sözcük sayısı çıktının bir örneği aşağıda gösterilmiştir:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Bu örnek günlük gösterir sözcük 've' 113 kez yayılır. Sayı, Yukarı Git spout aynı cümle sürekli olarak yayar çünkü topoloji çalıştırdığı sürece devam eder.

Arabellek sözcükleri ve sayıları arasında 5 saniyelik aralığına yoktur. **WordCount** değer çizgisi tanımlama grubu geldiğinde yalnızca bilgileri yaymak üzere yapılandırılır. Bu değer çizgisi tanımlama grubu yalnızca beş saniyede teslim açıklamayın.

## <a name="convert-the-topology-to-flux"></a>Topoloji için Flux Dönüştür

[Flux](https://storm.apache.org/releases/2.0.0/flux.html) uygulama yapılandırmasından ayrı olanak tanıyan bir yeni kullanılabilir olan Storm 0.10.0 veya üzeri bir çerçevedir. Bileşenlerinizi yine de Java'da tanımlanır, ancak topoloji, bir YAML dosyası kullanılarak tanımlanır. Varsayılan bir topoloji tanım paketini projenizle veya topoloji gönderirken bir tek başına dosya kullanın. Storm topoloji gönderirken YAML topoloji tanımı değerleri doldurmak için ortam değişkenlerini veya yapılandırma dosyalarını kullanabilirsiniz.

YAML dosyası tanımlar topoloji ve veriler için kullanılacak bileşenler arasındaki akış. Bir YAML dosyası jar dosyasını bir parçası olarak ekleyebilirsiniz veya dış bir YAML dosyası kullanabilirsiniz.

Flux hakkında daha fazla bilgi için bkz. [Flux framework (https://storm.apache.org/releases/1.0.6/flux.html)](https://storm.apache.org/releases/1.0.6/flux.html).

> [!WARNING]  
> Nedeniyle bir [hata (https://issues.apache.org/jira/browse/STORM-2055) ](https://issues.apache.org/jira/browse/STORM-2055) Storm 1.0.1 yüklemeniz gerekebilir bir [Storm geliştirme ortamı](https://storm.apache.org/releases/current/Setting-up-development-environment.html) Flux topolojileri yerel olarak çalıştırın.

1. Daha önce `WordCountTopology.java` topoloji tanımlı ancak Flux ile gerekli değildir. Aşağıdaki komutla dosyayı silin:

    ```cmd
    DEL src\main\java\com\microsoft\example\WordCountTopology.java
    ```

2. Oluşturmak ve yeni bir dosya açmak için aşağıdaki komutu girin `topology.yaml`:

    ```cmd
    notepad resources\topology.yaml
    ```

    Daha sonra kopyalayın ve yeni dosyaya aşağıdaki metni yapıştırın.  Dosyayı kaydedip kapatın.

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

3. Açmak için aşağıdaki komutu girin `pom.xml` aşağıda açıklandığı gibi düzeltmeler yapmak için:

    ```cmd
    notepad pom.xml
    ```

   * Aşağıdaki yeni bağımlılık olarak ekleme `<dependencies>` bölümü:

        ```xml
        <!-- Add a dependency on the Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```

   * Eklemek için aşağıdaki eklenti `<plugins>` bölümü. Bu eklenti projesi için bir paket (jar dosyası) oluşturulmasını işler ve belirli bazı dönüştürmeleri paket oluştururken Flux için geçerlidir.

        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.2.1</version>
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

   * İçinde **exec maven plugin** `<configuration>` bölümünde, değerini `<mainClass>` gelen `${storm.topology}` için `org.apache.storm.flux.Flux`. Bu ayar, topoloji geliştirme yerel olarak çalışan işlemek Flux sağlar.

   * İçinde `<resources>` bölümünde, ekleyin `<includes>`. Bu XML topoloji projenin bir parçası tanımlayan bir YAML dosyası içerir.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-the-flux-topology-locally"></a>Flux topolojisi yerel olarak test etme

1. Derleme ve Maven kullanarak Flux topolojisi yürütmek için aşağıdaki komutu girin:

    ```cmd
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    > [!WARNING]  
    > Topolojiniz Storm 1.0.1 BITS kullanıyorsa, bu komut başarısız olur. Bu hatanın nedeni [ https://issues.apache.org/jira/browse/STORM-2055 ](https://issues.apache.org/jira/browse/STORM-2055). Bunun yerine, [Storm geliştirme ortamınıza yükleyin](https://storm.apache.org/releases/current/Setting-up-development-environment.html) ve aşağıdaki adımları kullanın:
    >
    > Varsa [Storm geliştirme ortamınızda yüklü](https://storm.apache.org/releases/current/Setting-up-development-environment.html), bunun yerine aşağıdaki komutları kullanın:
    >
    > ```cmd
    > mvn compile package
    > storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    > ```

    `--local` Parametresi yerel modunda topoloji üzerinde geliştirme ortamınızı çalışır. `-R /topology.yaml` Parametresini kullanır `topology.yaml` topoloji tanımlamak için jar dosyasından kaynak dosyası.

    Çalıştığı topoloji başlangıç bilgileri görüntüler. Çıktının bir örneği aşağıda gösterilmiştir:

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    Günlüğe kaydedilen bilgileri toplu işlemler arasında 10 saniyelik gecikme yoktur.

2. Yeni bir topoloji yaml projeden oluşturun.
 
    a. Açmak için aşağıdaki komutu girin `topology.xml`:

    ```cmd
    notepad resources\topology.yaml
    ```

    b. Aşağıdaki bölüm ve değerini değiştirme `10` için `5`. Bu değişiklik ile 5 sözcük sayıları 10 saniyelerden yayan toplu aralığını değiştirir.  

    ```yaml
    - id: "counter-bolt"
      className: "com.microsoft.example.WordCount"
      constructorArgs:
        - 5
      parallelism: 1  
    ```  

    c. Dosyayı Farklı Kaydet `newtopology.yaml`.

3. Topoloji çalıştırmak için aşağıdaki komutu girin:

    ```cmd
    mvn exec:java -Dexec.args="--local resources/newtopology.yaml"
    ```

    Veya Storm üzerinde geliştirme ortamınızı varsa:

    ```cmd
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local resources/newtopology.yaml
    ```

     Bu komut `newtopology.yaml` topoloji tanımı. Biz eklemediğiniz beri `compile` parametresi, Maven, önceki adımlarda oluşturulan proje sürümünü kullanır.

    Topoloji başladıktan sonra yayılan toplu işler arasındaki zaman değerini yansıtmak üzere değiştiğini fark etmişsinizdir `newtopology.yaml`. Bu nedenle, yapılandırmanızın bir YAML dosyası aracılığıyla topoloji yeniden derlemenize gerek kalmadan değiştirebilirsiniz olduğunu görebilirsiniz.

Bu ve diğer özellikleri Flux framework'ün hakkında daha fazla bilgi için bkz. [Flux (https://storm.apache.org/releases/current/flux.html)](https://storm.apache.org/releases/current/flux.html).

## <a name="trident"></a>Trident

[Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html) Storm tarafından sağlanan üst düzey bir soyutlamadır. Bu durum bilgisi olan işleme destekler. Trident birincil avantajı, topoloji girdiği her ileti yalnızca bir kez işlenir garanti edebilir ' dir. Trident kullanmadan topolojinizi yalnızca iletileri en az bir kez işlenir garanti edebilir. Boltlar oluşturmak yerine kullanılabilen yerleşik bileşenleri gibi başka farklılıklar da vardır. Aslında, Cıvatalar filtreleri, tahminler ve işlevler gibi daha az genel bileşenleri tarafından değiştirilir.

Trident uygulamalarını Maven projelerini kullanarak oluşturulabilir. Bu makalenin önceki bölümlerinde sunulan temel adımları tekrarlayın — yalnızca kod farklıdır. Trident ayrıca (şu anda) Flux framework ile kullanılamaz.

Trident hakkında daha fazla bilgi için bkz: [Trident API'sine genel bakış](https://storm.apache.org/releases/current/Trident-API-Overview.html).

## <a name="next-steps"></a>Sonraki Adımlar

Java kullanarak bir Apache Storm topolojisi oluşturulacağını öğrendiniz. Daha fazla bilgi için nasıl:

* [HDInsight üzerinde Apache Storm topolojilerini dağıtma ve yönetme](apache-storm-deploy-monitor-topology-linux.md)

* [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md)

Apache Storm topolojilerini ziyaret ederek daha fazla örnek bulabilirsiniz [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md).
