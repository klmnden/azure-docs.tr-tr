---
title: Hdınsight'ta Hadoop ile MapReduce | Microsoft Docs
description: Hdınsight kümelerinde Hadoop MapReduce işleri çalıştırmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f321501-d62c-4ffc-b5d6-102ecba6dd76
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: larryfr
ms.openlocfilehash: 131e3065da4ccfb20d63856844a302a94806fd19
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34202637"
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Hdınsight'ta Hadoop MapReduce kullanın

Hdınsight kümelerinde MapReduce işleri çalıştırmayı öğrenin. MapReduce kullanılabilir çeşitli şekillerde bulmak için aşağıdaki tabloyu kullanın Hdınsight ile:

| **Bu**... | **...bilgisayarınızın bunu** | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |Hadoop komutu aracılığıyla kullanmak **SSH** |Linux |Linux, UNIX, Mac OS X veya Windows |
| [REST](apache-hadoop-use-mapreduce-curl.md) |İşi uzaktan kullanarak göndermek **REST** (örnekler cURL kullanın) |Linux veya Windows |Linux, UNIX, Mac OS X veya Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |İşi uzaktan kullanarak göndermek **Windows PowerShell** |Linux veya Windows |Windows |

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>

## <a id="whatis"></a>MapReduce nedir

Hadoop MapReduce, çok büyük miktarda veri işleyen işlerini yazmak için bir yazılım çerçevedir. Giriş verisi bağımsız parçalara bölünür. Her bir öbeğin, kümenizdeki düğümlerin arasında paralel olarak işlenir. Bir MapReduce işi, iki işlevden oluşur:

* **Eşleyici**: giriş verileri kullanır (genellikle filtre ve sıralama işlemleri ile) çözümler ve diziler (anahtar-değer çiftleri) yayar

* **Reducer**: Eşleyicisi tarafından gösterilen diziler kullanır ve Eşleyici verilerden daha küçük, birleşik bir sonuç oluşturur Özet bir işlemi gerçekleştirir

Aşağıdaki şemada temel word sayısı MapReduce işi örneği gösterilmiştir:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

Bu iş çıktısı, her sözcüğün metinde oluştu kaç kez sayısıdır.

* Eşleyici her bir girdi olarak giriş metin satırından alır ve sözcüklere böler. Bir anahtar/değer yayar çifti bir sözcük sözcüğün her oluştuğunda bir 1 tarafından izlendiği. Çıktı için reducer göndermeden önce sıralanır.
* Reducer her sözcüğün için bu tek tek sayıları toplar ve onun oluşum toplamı tarafından izlenen sözcüğünü içeren bir tek anahtar/değer çifti yayar.

MapReduce, çeşitli dillerde uygulanabilir. Java en yaygın uygulamasıdır ve bu belgedeki tanıtım amacıyla kullanılır.

## <a name="development-languages"></a>Geliştirme dilleri

Diller veya Java ve Java sanal makinesi bağlı çerçeveler olması başlattıysanız doğrudan bir MapReduce işi. Bu belgede kullanılan örnekte, bir Java MapReduce uygulamasıdır. C#, Python veya tek başına yürütülebilir dosyaları, gibi olmayan Java dilleri kullanmalıdır **Hadoop akış**.

Hadoop akış Eşleyici ve reducer STDIN ve STDOUT üzerinden iletişim kurar. Eşleyici ve reducer STDIN aynı anda bir satır veri okuma ve çıktı STDOUT yazma. Okuma veya Eşleyici ve reducer tarafından gösterilen her satır bir sekme karakteriyle ayrılmış bir anahtar/değer çifti biçiminde olması gerekir:

    [key]/t[value]

Daha fazla bilgi için bkz: [Hadoop akış](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Hdınsight ile akış Hadoop kullanım örnekleri için aşağıdaki belgelere bakın:

* [C# MapReduce işleri geliştirin](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Python MapReduce işleri geliştirin](apache-hadoop-streaming-python.md)

## <a id="data"></a>Örnek veri

Hdınsight içinde depolanan çeşitli örnek veri kümeleri, sağlar `/example/data` ve `/HdiSamples` dizin. Bu dizinleri, kümeniz için varsayılan depolama arasındadır. Bu belgede, kullandığımız `/example/data/gutenberg/davinci.txt` dosya. Bu dosya Leonardo Da Vinci not defterlerini içerir.

## <a id="job"></a>Örnek MapReduce

Bir örnek MapReduce word sayısı uygulaması Hdınsight kümenize dahil edilir. Bu örnek adresindedir `/example/jars/hadoop-mapreduce-examples.jar` kümeniz için varsayılan depolama.

Aşağıdaki Java kod içinde yer alan MapReduce uygulama kaynağıdır `hadoop-mapreduce-examples.jar` dosyası:

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

* [Hdınsight için Java MapReduce uygulamalar geliştirin](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [Hdınsight için Python MapReduce uygulamalar geliştirin](apache-hadoop-streaming-python.md)

## <a id="run"></a>MapReduce Çalıştır

Hdınsight HiveQL işleri çeşitli yöntemleri kullanarak çalıştırabilirsiniz. Hangi yöntemi sizin için uygun olduğuna karar vermek için aşağıdaki tabloyu kullanın, sonra bir anlatım için bağlantıyı izleyin.

| **Bu**... | **...bilgisayarınızın bunu** | ...hemen bu **küme işletim sistemi** | ...from bu **istemci işletim sistemi** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |Hadoop komutu aracılığıyla kullanmak **SSH** |Linux |Linux, UNIX, Mac OS X veya Windows |
| [Curl](apache-hadoop-use-mapreduce-curl.md) |İşi uzaktan kullanarak göndermek **REST** |Linux veya Windows |Linux, UNIX, Mac OS X veya Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |İşi uzaktan kullanarak göndermek **Windows PowerShell** |Linux veya Windows |Windows |

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight'ta verilerle çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Hdınsight için Java MapReduce programlar geliştirmek](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [MapReduce programları Hdınsight için akış Python geliştirme](apache-hadoop-streaming-python.md)

* [HDInsight ile Hive kullanma][hdinsight-use-hive]

* [HDInsight ile Pig kullanma][hdinsight-use-pig]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-pig]:hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
