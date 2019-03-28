---
title: HDInsight üzerinde Apache Hadoop ile MapReduce
description: HDInsight kümelerinde Apache Hadoop MapReduce işleri çalıştırmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: f2baaf598be8ede69fd6e1fa49a5f5a6b64c24ff
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521209"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight"></a>HDInsight üzerinde Apache Hadoop MapReduce kullanma

HDInsight kümelerinde MapReduce işleri çalıştırmayı öğrenin. 

## <a id="whatis"></a>MapReduce nedir

Apache Hadoop MapReduce, çok büyük miktarda veri işleyen işlerini yazmak için bir yazılım çerçevedir. Girdi verilerini bağımsız parçalara bölünür. Her bir öbeği kümenizdeki düğümler arasında paralel olarak işlenir. Bir MapReduce işi iki işlevlerini oluşur:

* **Eşleyici**: Giriş verilerini kullanır (genellikle filtre ve sıralama işlemleri ile) analiz eder ve diziler (anahtar-değer çiftleri) yayar

* **Azaltıcı**: Eşleyicisi tarafından yayılan tanımlama grubu tüketir ve Eşleyici verilerden daha küçük, birleşik bir sonuç oluşturan bir Özet işlemi gerçekleştirir

Temel bir sözcük sayımı MapReduce işi örnek, aşağıdaki diyagramda gösterilmiştir:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Bu işin çıkışı, kaç kez her sözcüğün metinde oluştuğu sayısıdır.

* Eşleyici, her satır girdi olarak giriş metninde alır ve sözcüklere böler. Kendisini çıkaran derlemeninkinden bir anahtar/değer çifti bir word sözcük her gerçekleştiğinde 1 tarafından izlendiği. Çıkış için Azaltıcı göndermeden önce sıralanır.
* Azaltıcı bu tek tek her sözcük sayısını toplar ve kendi oluşum toplamı tarafından izlenen sözcüğünü içeren bir tek anahtar/değer çifti yayar.

MapReduce, çeşitli dillerde uygulanabilir. Java en yaygın bir uygulamadır ve bu belgedeki tanıtım amacıyla kullanılır.

## <a name="development-languages"></a>Geliştirme dilleri

Dil veya Java ve Java sanal makine tabanlı çerçeve olması çalıştı doğrudan bir MapReduce işi. Bu belgede kullanılan örnek bir Java MapReduce uygulamasıdır. C#, Python veya tek başına yürütülebilir dosyaları, gibi Java dışındaki dilleri kullanmaları gereken **Hadoop akış**.

Hadoop akış Eşleyici ve azaltıcı STDIN ve STDOUT üzerinden iletişim kurar. Eşleyici Azaltıcı STDIN aynı anda bir satır veri okuma ve çıktısını STDOUT'a yazma. Okuma veya Eşleyici ve azaltıcı tarafından yayılan her satır bir sekme karakteri tarafından ayrılmış bir anahtar/değer çifti biçimi olmalıdır:

    [key]/t[value]

Daha fazla bilgi için [Hadoop akış](https://hadoop.apache.org/docs/r1.2.1/streaming.html).

Hadoop ile HDInsight akış kullanma örnekleri için aşağıdaki belgelere bakın:

* [C# MapReduce işlerini geliştirme](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Python MapReduce işlerini geliştirme](apache-hadoop-streaming-python.md)

## <a id="data"></a>Örnek veri

HDInsight sağlar içinde depolanan çeşitli örnek veri kümeleri, `/example/data` ve `/HdiSamples` dizin. Bu dizinler, kümenin varsayılan depolama içindedir. Bu belgede, kullandığımız `/example/data/gutenberg/davinci.txt` dosya. Bu dosya Leonardo Da Vinci, not defterlerini içerir.

## <a id="job"></a>Örnek MapReduce

MapReduce sözcük sayısı uygulama örneği, HDInsight kümenizle dahildir. Bu örnekte şu konumdadır `/example/jars/hadoop-mapreduce-examples.jar` kümeniz için varsayılan depolama.

Aşağıdaki Java kod içindeki MapReduce uygulama kaynağıdır `hadoop-mapreduce-examples.jar` dosyası:

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

Kendi MapReduce uygulamaları yazmak yönergeler için aşağıdaki belgelere bakın:

* [HDInsight için Java MapReduce uygulamalar geliştirin](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [HDInsight için Python MapReduce uygulamalar geliştirin](apache-hadoop-streaming-python.md)

## <a id="run"></a>MapReduce çalıştırma

HDInsight, çeşitli yöntemlerle HiveQL işleri çalıştırabilirsiniz. Hangi yöntemin size uygun olduğuna karar vermek için aşağıdaki tabloyu kullanın ve ardından bir kılavuz için bağlantıyı izleyin.

| **Bunu kullanın**... | **...bilgisayarınızın bunu** | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |Hadoop komutu aracılığıyla **SSH** |Linux |Linux, UNIX, Mac OS X veya Windows |
| [Curl](apache-hadoop-use-mapreduce-curl.md) |Kullanarak uzaktan işi göndermek **REST** |Linux veya Windows |Linux, UNIX, Mac OS X veya Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |Kullanarak uzaktan işi göndermek **Windows PowerShell** |Linux veya Windows |Windows |

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight verilerle çalışma hakkında daha fazla bilgi edinmek için aşağıdaki belgelere bakın:

* [HDInsight için Java MapReduce programları geliştirme](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [Python için HDInsight akış MapReduce programları geliştirme](apache-hadoop-streaming-python.md)

* [Apache Hive, HDInsight ile kullanma][hdinsight-use-hive]

* [Apache Pig, HDInsight ile kullanma][hdinsight-use-pig]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-pig]:hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
