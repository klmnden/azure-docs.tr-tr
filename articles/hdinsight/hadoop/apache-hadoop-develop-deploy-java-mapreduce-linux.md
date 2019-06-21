---
title: Apache Hadoop - Azure HDInsight için Java MapReduce oluşturma
description: Apache Maven MapReduce Java tabanlı bir uygulama oluşturun, sonra Azure HDInsight üzerinde Hadoop ile çalıştırmak için kullanmayı öğrenin.
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: hrasheed
ms.openlocfilehash: 9eb331e303ff64f4316aab0a0af553975bb72fc1
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137495"
---
# <a name="develop-java-mapreduce-programs-for-apache-hadoop-on-hdinsight"></a>HDInsight üzerinde Apache Hadoop için Java MapReduce programları geliştirme

Apache Maven MapReduce Java tabanlı bir uygulama oluşturun, sonra Azure HDInsight üzerinde Apache Hadoop ile çalıştırmak için kullanmayı öğrenin.

> [!NOTE]
> Bu örnekte, HDInsight 3.6 üzerinde en son test edilmiştir.

## <a name="prerequisites"></a>Önkoşullar

* [Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/) 8 veya üzeri (veya OpenJDK gibi eşdeğeri).
    
    > [!NOTE]
    > HDInsight sürüm 3.4 ve eski Java 7 kullanın. HDInsight 3.5 ve üzeri Java 8 kullanır.

* [Apache Maven](https://maven.apache.org/)

## <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

Java ve JDK yüklediğinizde aşağıdaki ortam değişkenlerini ayarlanabilir. Ancak, bunların mevcut olup olmadığını ve sisteminiz için doğru değerleri içerip içermediğini denetlemeniz gerekir.

* `JAVA_HOME` -Java Çalışma zamanı ortamı (JRE) yüklü olduğu dizine işaret etmelidir. Örneğin, bir OS X, Unix veya Linux sisteminde, benzer bir değer olmalıdır `/usr/lib/jvm/java-7-oracle`. Windows benzer bir değere sahip `c:\Program Files (x86)\Java\jre1.7`

* `PATH` - aşağıdaki yolları içermelidir:
  
  * `JAVA_HOME` (veya eşdeğer yolu)

  * `JAVA_HOME\bin` (veya eşdeğer yolu)

  * Maven'ın yüklendiği dizinin

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturun

1. Bir terminal oturumu, veya geliştirme ortamınızdaki komut satırından, dizinleri bu projeyi depolamak istediğiniz konuma değiştirin.

2. Kullanım `mvn` proje için yapı iskelesini oluşturmak için Maven ile yüklenir, komutu.

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > PowerShell kullanıyorsanız, almalısınız `-D` çift tırnak parametreleri.
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Bu komut, belirtilen ada sahip bir dizin oluşturur. `artifactID` parametre (**wordcountjava** Bu örnekte.) Bu dizin, aşağıdaki öğeleri içerir:

   * `pom.xml` - [Proje nesne modeli (POM)](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html) Projeyi derlemek için kullanılan bilgiler ve yapılandırma ayrıntılarını içerir.

   * `src` -Uygulamayı içeren dizin.

3. Silme `src/test/java/org/apache/hadoop/examples/apptest.java` dosya. Bu örnekte kullanılmaz.

## <a name="add-dependencies"></a>Bağımlılıkları ekleyin

1. Düzen `pom.xml` dosya ve içine aşağıdaki metni ekleyin `<dependencies>` bölümü:
   
   ```xml
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-examples</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-client-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
   ```

    Bu, gerekli kitaplıkları tanımlar (içinde listelenen &lt;Artifactıd\>) belirli bir sürümle (içinde listelenen &lt;sürüm\>). Bu bağımlılıklar, derleme zamanında varsayılan Maven deposundan yüklenir. Kullanabileceğiniz [Maven deposu arama](https://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) daha fazla bilgi görüntülemek için.
   
    `<scope>provided</scope>` HDInsight kümesi çalışma zamanında tarafından sağlanan Bu bağımlılıklar uygulama ile birlikte paketlenmesi gereken değil, Maven söyler.

    > [!IMPORTANT]
    > Kullanılan sürümü, kümenizde mevcut Hadoop sürümüyle eşleşmelidir. Sürümleri hakkında daha fazla bilgi için bkz. [HDInsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md) belge.

2. Aşağıdakileri `pom.xml` dosyasına ekleyin. Bu metin içinde olmalıdır `<project>...</project>` etiketleri dosya; Örneğin, arasında `</dependencies>` ve `</project>`.

   ```xml
    <build>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
            <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                </transformer>
            </transformers>
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
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
            <source>1.8</source>
            <target>1.8</target>
            </configuration>
        </plugin>
        </plugins>
    </build>
   ```

    İlk eklentisi yapılandırır [Maven gölge eklentisi](https://maven.apache.org/plugins/maven-shade-plugin/), uygulamanın gerektirdiği bağımlılıkları içeren (bazen bir fatjar olarak adlandırılır) bir uberjar oluşturmak için kullanılır. Ayrıca, bazı sistemlerde sorunlara neden olabilir bir jar paketi içinde lisanslarının çoğaltma önler.

    İkinci eklentisi hedef Java sürümü yapılandırır.

    > [!NOTE]
    > HDInsight 3.4 ve Java 7 önceki kullanın. HDInsight 3.5 ve üzeri Java 8 kullanır.

3. `pom.xml` dosyasını kaydedin.

## <a name="create-the-mapreduce-application"></a>MapReduce uygulaması oluşturma

1. Git `wordcountjava/src/main/java/org/apache/hadoop/examples` dizin ve yeniden adlandırma `App.java` dosyasını `WordCount.java`.

2. Açık `WordCount.java` dosyasını bir metin düzenleyicisinde ve içeriğini aşağıdaki metinle değiştirin:
   
    ```java
    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

        public static class TokenizerMapper
            extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
            StringTokenizer itr = new StringTokenizer(value.toString());
            while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
            }
        }
    }

    public static class IntSumReducer
            extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                            Context context
                            ) throws IOException, InterruptedException {
            int sum = 0;
            for (IntWritable val : values) {
            sum += val.get();
            }
            result.set(sum);
            context.write(key, result);
        }
    }

    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
            System.err.println("Usage: wordcount <in> <out>");
            System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
        }
    }
    ```
   
    Paket adı olduğuna dikkat edin `org.apache.hadoop.examples` ve sınıf adını `WordCount`. MapReduce işi gönderdiğinizde bu adlar kullanın.

3. Dosyayı kaydedin.

## <a name="build-the-application"></a>Uygulama oluşturma

1. Değiştirin `wordcountjava` , zaten orada değilseniz dizin.

2. Uygulamayı içeren bir JAR dosyasını oluşturmak için aşağıdaki komutu kullanın:

   ```
   mvn clean package
   ```

    Bu komut, önceki tüm derleme yapıtları temizler, paket uygulama ve sonra yapılar ve zaten yüklü, bağımlılıkları yükler.

3. Komut bittikten sonra `wordcountjava/target` dizin adlı dosyayı içeren `wordcountjava-1.0-SNAPSHOT.jar`.
   
   > [!NOTE]
   > `wordcountjava-1.0-SNAPSHOT.jar` Yalnızca WordCount işi, aynı zamanda işin çalışma zamanında gereken bağımlılıklar içeren bir uberjar dosyasıdır.

## <a id="upload"></a>Jar dosyasını karşıya yükle

HDInsight baş düğüme jar dosyasını karşıya yüklemek için aşağıdaki komutu kullanın:

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

Değiştirin __kullanıcıadı__ ile küme SSH kullanıcı adı. Değiştirin __CLUSTERNAME__ ile HDInsight kümesi adı.

Bu komut dosyaları, yerel sistemden baş düğüme kopyalar. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run"></a>Hadoop üzerinde bir MapReduce işi çalıştırın

1. SSH kullanarak HDInsight için bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. SSH oturumundan, MapReduce uygulamayı çalıştırmak için aşağıdaki komutu kullanın:
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    Bu komut WordCount MapReduce uygulamayı başlatır. Giriş dosyası `/example/data/gutenberg/davinci.txt`, ve çıkış dizini `/example/data/wordcountout`. Hem giriş dosyası hem de çıkış kümenin varsayılan depolama alanına depolanır.

3. İş tamamlandıktan sonra sonuçları görüntülemek için aşağıdaki komutu kullanın:
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    Sözcükleri ve sayıları, aşağıdaki metne benzer değerlerle listesini almanız gerekir:
   
        zeal    1
        zelus   1
        zenith  2

## <a id="nextsteps"></a>Sonraki adımlar

Bu belgede, bir Java MapReduce işi geliştirme gerçekleştirmeyi öğrendiniz. HDInsight ile çalışmanın diğer yolları için aşağıdaki belgelere bakın.

* [Apache Hive, HDInsight ile kullanma](hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Daha fazla bilgi için Ayrıca bkz: [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]:apache-hadoop-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-power-query]:apache-hadoop-connect-excel-power-query.md

[powershell-PSCredential]: https://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

