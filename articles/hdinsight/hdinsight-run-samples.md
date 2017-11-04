---
title: "Hdınsight - Azure Hadoop örneklerini çalıştırma | Microsoft Docs"
description: "Azure Hdınsight hizmeti sağlanan örneklerle kullanmaya başlayın. MapReduce programları veri kümelerinde çalışan bir PowerShell komut dosyası kullanın."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bf76d452-abb4-4210-87bd-a2067778c6ed
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6e4dbf1fa32f916a206e4f8d796218ab809835ad
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Hadoop MapReduce Windows tabanlı Hdınsight'ta örneklerini çalıştırma
[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Örnekler kümesi kullanarak Azure Hdınsight Hadoop kümeleri üzerinde başlatılan çalışan MapReduce işleri sağlamanıza yardımcı olması için sağlanır. Bu örneklerin her oluşturduğunuz yönetilen Hdınsight kümeleri kullanılabilir hale getirilir. Bu örnekleri çalıştırma hakkında bilgi edinin, Hadoop kümelerine işlerini çalıştırmak için Azure PowerShell cmdlet'lerini kullanarak ile.

* [**Word sayısı**][hdinsight-sample-wordcount]: bir metin dosyasındaki word oluşum sayar.
* [**C# sözcük sayımı akış**][hdinsight-sample-csharp-streaming]: Hadoop akış arabirimini kullanarak bir metin dosyasındaki word oluşum sayar.
* [**Pi tahmin**][hdinsight-sample-pi-estimator]: bir istatistik kullanır (yarı Monte Carlo) pi değerini tahmin etmek için yöntem.
* [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: Hdınsight kullanarak genel amaçlı GraySort bir 10 GB dosyasını çalıştırın. Çalıştırmak için üç iş vardır: verileri sıralamak için Terasort ve verilerin düzgün bir şekilde sıralanmış olduğunu onaylamak için Teravalidate verileri oluşturmak için Teragen.

> [!NOTE]
> Kaynak kodu ekinde bulunabilir.

Java tabanlı MapReduce programlama ve akış ve Windows PowerShell içinde kullanılan cmdlet'ler hakkında belgeler gibi Hadoop ilgili teknolojileri için Web'de çok ek belgeler mevcut betik oluşturma. Bu kaynaklar hakkında daha fazla bilgi için bkz:

* [Hdınsight'ta Hadoop için Java MapReduce programlar geliştirmek](hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md)
* [HDInsight'ta Hadoop işlerini gönderme](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Azure Hdınsight giriş][hdinsight-introduction]

Günümüzde, çoğu kişi Hive veya Pig MapReduce seçin.  Daha fazla bilgi için bkz.

* [Hdınsight'ta Hive kullanma](hadoop/hdinsight-use-hive.md)
* [Hdınsight'ta pig kullanma](hadoop/hdinsight-use-pig.md)

**Önkoşullar**:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Hdınsight kümesi**. İçinde bu tür kümeler oluşturulabilir çeşitli yollar hakkında yönergeler için bkz: [Hdınsight'ta oluşturmak Hadoop kümeleri](hdinsight-hadoop-provision-linux-clusters.md).
* **Azure PowerShell içeren bir iş istasyonu**.

    > [!IMPORTANT]
    > Azure Service Manager kullanılarak HDInsight kaynaklarının yönetilmesi için Azure PowerShell desteği **kullanım dışıdır** ve 1 Ocak 2017 tarihine kadar kaldırılacaktır. Bu belgede yer alan adımlar, Azure Resource Manager ile çalışan yeni HDInsight cmdlet'lerini kullanır.
    >
    > Adımları [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs) Azure PowerShell'in en son sürümünü yüklemek için. Azure Resource Manager ile çalışan yeni cmdlet'lerle kullanmak için değiştirilmesi gereken komut dosyalarınız varsa, bkz: [için Hdınsight kümeleri için Azure Resource Manager tabanlı geliştirme araçlarına geçme](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="hdinsight-sample-wordcount"></a>Sayısı - Java word
MapReduce projesini gönderme için önce bir MapReduce işi tanımı oluşturun. İş tanımında MapReduce program jar dosyasını ve jar dosyasını konumunu belirtin **wasb:///example/jars/hadoop-mapreduce-examples.jar**, sınıf adını ve bağımsız değişkenler.  Wordcount MapReduce program iki bağımsız değişkeni alır: sözcükler ve çıkış konumunu hesaplamak için kullanılan kaynak dosyası.

Kaynak kodu bulunabilir [ek A](#apendix-a---the-word-count-MapReduce-program-in-java).

Java MapReduce geliştirme yordamı için program için bkz: - [hdınsight'ta Hadoop için Java MapReduce geliştirmek programlar](hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md)

**Word sayısı MapReduce işi göndermek için**

1. Açık **Windows PowerShell ISE**. Yönergeler için bkz: [yükleyin ve Azure PowerShell yapılandırma][powershell-install-configure].
2. Aşağıdaki PowerShell betiğini yapıştırın:

    ```powershell
    $subscriptionName = "<Azure Subscription Name>"
    $resourceGroupName = "<Resource Group Name>"
    $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name

    Select-AzureRmSubscription -SubscriptionName $subscriptionName

    # Define the MapReduce job
    $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "wordcount" `
                                -Arguments "wasb:///example/data/gutenberg/davinci.txt", "wasb:///example/data/WordCountOutput"

    # Submit the job and wait for job completion
    $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:"
    $mrJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $clusterName `
                        -HttpCredential $cred `
                        -JobDefinition $mrJobDefinition

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -JobId $mrJob.JobId

    # Get the job output
    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -DefaultStorageAccountName $defaultStorageAccount `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultStorageContainer  `
        -JobId $mrJob.JobId `
        -DisplayOutputType StandardError

    # Download the job output to the workstation
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force

    # Display the output file
    cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"
    ```

    MapReduce işi adlı bir dosya oluşturur *bölümü r 00000*, sözcükleri ve sayılarını içerir. Komut dosyası kullanan **findstr** içeren tüm sözcükleri listelemek için komut *"yok"*.
3. İlk üç değişkenleri ayarlayın ve betiği çalıştırın.

## <a name="hdinsight-sample-csharp-streaming"></a>Sayısı - C# akış word
Hadoop harita yazma ve Java dışındaki dillerde işlevleri azaltmak sağlar MapReduce akış bir API sağlar.

> [!NOTE]
> Bu öğreticideki adımlar, yalnızca Windows tabanlı Hdınsight kümeleri için geçerlidir. Linux tabanlı Hdınsight kümeleri için akış örneği için bkz: [programları Hdınsight için akış Python geliştirme](hadoop/apache-hadoop-streaming-python.md).

Örnekte, Eşleyici ve reducer girdiden okunan yürütülebilir dosyalar [stdin] [ stdin-stdout-stderr] (--satır) ve çıktıyı yayma [stdout] [ stdin-stdout-stderr]. Program metindeki tüm sözcükleri sayar.

İçin yürütülebilir bir dosya belirtildiğinde **mappers**, Eşleyici başlatıldığında her Eşleyici görev yürütülebilir ayrı bir işlem olarak başlatır. Eşleyici görev çalışırken, kendi giriş satırlarına dönüştürür ve satırlarına akışları [stdin] [ stdin-stdout-stderr] işleminin.

Bu arada, Eşleyici satır yönelimli çıkış işlemi stdout'tan toplar. Eşleyici çıkış olarak toplanan bir anahtar/değer çifti her satırın dönüştürür. Varsayılan olarak, ilk sekme karakteri kadar bir satır önek anahtar ve (sekme karakteri hariç) satırın geri kalan değerdir. Satırda sekme karakteri varsa, tüm satır anahtarı olarak kabul edilir ve değeri null.

İçin yürütülebilir bir dosya belirtildiğinde **reducers**, reducer başlatıldığında her reducer görev yürütülebilir ayrı bir işlem olarak başlatır. Reducer görev çalışır, kendi giriş anahtar/değer çiftleri satırlarına dönüştürür ve satırlarına akışları [stdin] [ stdin-stdout-stderr] işleminin.

Bu arada, satır yönelimli çıktısını reducer toplar [stdout] [ stdin-stdout-stderr] işleminin. Reducer çıkış olarak toplanan bir anahtar/değer çifti her satırın dönüştürür. Varsayılan olarak, ilk sekme karakteri kadar bir satır önek anahtar ve (sekme karakteri hariç) satırın geri kalan değerdir.

**Bir C word sayısı iş akış # göndermek için**

* Yordamı izleyin [sayısı - Java Word](#word-count-java)ve iş tanımı aşağıdaki satırla değiştirin:

    ```powershell
    $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                            -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                            -Mapper "cat.exe" `
                            -Reducer "wc.exe" `
                            -InputPath "/example/data/gutenberg/davinci.txt" `
                            -OutputPath "/example/data/StreamingOutput/wc.txt"
    ```

    Çıktı dosyası olacaktır:

        example/data/StreamingOutput/wc.txt/part-00000

## <a name="hdinsight-sample-pi-estimator"></a>PI tahmin
Pi tahmin bir istatistik kullanır (yarı Monte Carlo) pi değerini tahmin etmek için yöntem. Rastgele bir birim içinde yerleştirilen noktaları kare ayrıca kalan daire alana eşit bir olasılık ile bu kare içinde içinde bir daire içinde pi/4. Pi değerini R kare içinde noktalarını toplam sayısı için bir daire içinde olduğunda puan sayısının oranını olduğu 4R değerinden tahmin edilebilir. Büyük kullanılan noktaları, daha iyi tahmin örneğidir.

Bu örnek için sağlanan komut dosyasını Hadoop jar işi gönderir ve en fazla 16 haritalar, her biri 10 milyon örnek noktaları işlem için parametre değerleri ile gerekli bir değerle çalıştırdığınızda ayarlanır. Bu parametre değerlerini tahmini pi değerini artırmak için değiştirilebilir. Başvuru için ilk 10 ondalık pi'nin 3.1415926535 yerlerdir.

**Pi tahmin işi göndermek için**

* Yordamı izleyin [sayısı - Java Word](#word-count-java)ve iş tanımı aşağıdaki satırla değiştirin:

    ```powershell
    $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "pi" `
                                -Arguments "16", "10000000"
    ```

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB Graysort
Bu örnek uygun bir 10 GB veri kullanır, böylece oldukça hızlı bir şekilde çalıştırılabilir. Owen O'Malley ve 2009 0.578 TB/dak (100 TB 173 dakika cinsinden) oranı ile yıllık genel amaçlı ("daytona") terabayt sıralama Kıyaslama won Arun Murthy tarafından geliştirilen MapReduce uygulamaları kullanır. Bu ve diğer sıralama değerlendirmeleri hakkında daha fazla bilgi için bkz: [Sortbenchmark](http://sortbenchmark.org/) site.

Bu örnek, MapReduce programlar üç kümesi kullanır:

1. **TeraGen** sıralamak için veri satırı oluşturmak için kullanabileceğiniz bir MapReduce programdır.
2. **TeraSort** giriş verilerini örnekleri ve MapReduce toplam sıralamaya verileri sıralamak için kullanır. TeraSort standart sıralama MapReduce işlevlerin her azaltma için anahtar aralığı tanımlayın örneklenen N-1 anahtarları sıralı listesini kullanan özel bir bölümleyici dışında ' dir. Özellikle, [i-1] örnek tüm anahtarları böyle < = anahtar < örnek [i] i azaltmak için gönderilir. Bu çıkışı azaltmak i + 1 değerinden çıkışları i azaltmak garanti tüm.
3. **TeraValidate** çıkış genel olarak kullanılan doğrulayan bir MapReduce programdır. Belirtilen çıkış dizinine dosya başına bir harita oluşturur ve her eşleme her anahtar öncekinin küçük veya buna eşit olmasını sağlar. Harita işlevi de her dosyanın ilk ve son anahtarların kayıtlar oluşturur ve dosya ilk anahtarı dosyası i-1 son anahtarı büyüktür azaltma işlevi sağlar. Herhangi bir sorun bozuk anahtarlarla azaltın, çıkış olarak raporlanır.

Üç uygulama tarafından kullanılan giriş ve çıkış biçimi okur ve metin dosyaları doğru biçimde yazar. Kıyaslama bağlamı bağlamı çıktı verilerini birden çok düğüm açın çoğaltılması gerektirmediğinden azaltın çıktısını çoğaltma 1, 3, varsayılan yerine kümesine sahiptir.

Üç görevleri tarafından her birine girişte açıklanan MapReduce programları karşılık gelen örnek gereklidir:

1. Çalıştırarak sıralamak için verileri oluşturmak **TeraGen** MapReduce işi.
2. Çalıştırarak verileri sıralamak **TeraSort** MapReduce işi.
3. Verileri doğru çalıştırarak sıralandıktan olduğunu onaylayın **TeraValidate** MapReduce işi.

**İşlerini göndermek için**

* Yordamı izleyin [sayısı - Java Word](#word-count-java)ve aşağıdaki iş tanımları kullanın:

    ```powershell
    $teragen = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teragen" `
                                -Arguments "-Dmapred.map.tasks=50", "100000000", "/example/data/10GB-sort-input"

    $terasort = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "terasort" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-input", "/example/data/10GB-sort-output"

    $teravalidate = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teravalidate" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-output", "/example/data/10GB-sort-validate"
    ```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede ve her örnekleri makaleler, Azure PowerShell kullanarak Hdınsight kümeleri ile dahil örnekleri çalıştırmak nasıl öğrendiniz. Hdınsight ile Pig, Hive ve MapReduce kullanma hakkında daha fazla öğreticileri için aşağıdaki konulara bakın:

* [Hadoop ile hdınsight'ta Hive mobil ahize kullanımını çözümleme için kullanmaya başlama][hdinsight-get-started]
* [Hdınsight'ta Hadoop ile pig kullanma][hdinsight-use-pig]
* [Hdınsight'ta Hadoop ile Hive kullanma][hdinsight-use-hive]
* [Hdınsight'ta Hadoop işleri gönderme][hdinsight-submit-jobs]
* [Azure Hdınsight SDK Belgeleri][hdinsight-sdk-documentation]

## <a name="appendix-a---the-word-count-source-code"></a>Ek A - Word sayısı kaynak kodu

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

## <a name="appendix-b---the-word-count-streaming-source-code"></a>Ek B - kaynak kodu akış sözcük sayımı
MapReduce program cat.exe uygulama konsolu ve bir belgeden akışı sözcük sayısını azaltın arabirimi olarak wc.exe uygulama metin akışını sağlamak için bir eşleme arabirimi olarak kullanır. Eşleyici ve reducer standart giriş akışı (stdin) karakterleri, satır satır, okuma ve standart çıktı akışı (stdout) yazma.

```csharp
// The source code for the cat.exe (Mapper).

using System;
using System.IO;

namespace cat
{
    class cat
    {
        static void Main(string[] args)
        {
            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            string line;
            char[] separators = { ' ', '\n'};
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split(separators);
                foreach (var word in words)
                {
                    Console.WriteLine("{0}\t1", word);
                }
            }
        }
    }
}
```

Cat.cs dosyasındaki Eşleyici kodu kullanan bir [StreamReader] [ streamreader] statikilestandartçıktıakışınaakışaYazarkonsolgelenakışınkarakterleriokumakiçinnesnesi[ Console.Writeline] [ console-writeline] yöntemi.

```csharp
// The source code for wc.exe (Reducer) is:

using System;
using System.IO;
using System.Linq;
using System.Collections;

namespace wc
{
    class wc
    {
        static void Main(string[] args)
        {
            string line;

            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            Hashtable wordCount = new Hashtable();
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split('\t');

                string key = words[0];

                if (wordCount.ContainsKey(key) == true)
                {
                    int n = Convert.ToInt32(wordCount[key]);
                    wordCount[key] = Convert.ToString(n + 1);
                }
                else
                {
                    wordCount[key] = words[1];
                }
            }
            foreach (var key in wordCount.Keys)
            {
                Console.WriteLine("{0} {1}", key, wordCount[key]);
            }
        }
    }
}
```

Wc.cs dosyasında reducer kodu kullanan bir [StreamReader] [ streamreader] cat.exe Eşleyicisi tarafından çıktı silinmiş standart giriş akışı karakter okunacak nesne. Karakterlerle okuduğu [Console.Writeline] [ console-writeline] yöntemi, bu sayıları sözcükleri alanları ve her sözcüğün sonuna satır sonu karakterleri sayma. Standart çıktı akışına ile toplam sonra Yazar [Console.Writeline] [ console-writeline] yöntemi.

## <a name="appendix-c---the-pi-estimator-source-code"></a>Ek C - Pi tahmin kaynak kodu
Pi tahmin Eşleyici ve reducer işlevleri içeren Java kod incelemesi aşağıdaki kullanılabilir. Eşleyici program rastgele bir birim kare içinde yerleştirilen noktaları belirtilen sayıda oluşturur ve dairenin içine bu noktalarını sayısını sayar. Reducer program tarafından mappers sayılan noktaları toplanır ve R kare içinde noktalarını toplam sayısı için bir daire içinde sayılan puan sayısının oranını olduğu formül 4R gelen pi değerini tahmin eder.

```java
/**
* Licensed to the Apache Software Foundation (ASF) under one
* or more contributor license agreements. See the NOTICE file
* distributed with this work for additional information
* regarding copyright ownership. The ASF licenses this file
* to you under the Apache License, Version 2.0 (the
* "License"); you may not use this file except in compliance
* with the License. You may obtain a copy of the License at
*
* http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed to in writing, software
* distributed under the License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or     implied.
* See the License for the specific language governing permissions and
* limitations under the License.
*/

package org.apache.hadoop.examples;

import java.io.IOException;
import java.math.BigDecimal;
import java.util.Iterator;

import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.BooleanWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Writable;
import org.apache.hadoop.io.WritableComparable;
import org.apache.hadoop.io.SequenceFile.CompressionType;
import org.apache.hadoop.mapred.FileInputFormat;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.MapReduceBase;
import org.apache.hadoop.mapred.Mapper;
import org.apache.hadoop.mapred.OutputCollector;
import org.apache.hadoop.mapred.Reducer;
import org.apache.hadoop.mapred.Reporter;
import org.apache.hadoop.mapred.SequenceFileInputFormat;
import org.apache.hadoop.mapred.SequenceFileOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

//A Map-reduce program to estimate the value of Pi
//using quasi-Monte Carlo method.
//
//Mapper:
//Generate points in a unit square
//and then count points inside/outside of the inscribed circle of the square.
//
//Reducer:
//Accumulate points inside/outside results from the mappers.
//Let numTotal = numInside + numOutside.
//The fraction numInside/numTotal is a rational approximation of
//the value (Area of the circle)/(Area of the square),
//where the area of the inscribed circle is Pi/4
//and the area of unit square is 1.
//Then, Pi is estimated value to be 4(numInside/numTotal).
//

public class PiEstimator extends Configured implements Tool {
//tmp directory for input/output
static private final Path TMP_DIR = new Path(
PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

//2-dimensional Halton sequence {H(i)},
//where H(i) is a 2-dimensional point and i >= 1 is the index.
//Halton sequence is used to generate sample points for Pi estimation.
private static class HaltonSequence {
// Bases
static final int[] P = {2, 3};
//Maximum number of digits allowed
static final int[] K = {63, 40};

private long index;
private double[] x;
private double[][] q;
private int[][] d;

//Initialize to H(startindex),
//so the sequence begins with H(startindex+1).
HaltonSequence(long startindex) {
index = startindex;
x = new double[K.length];
q = new double[K.length][];
d = new int[K.length][];
for(int i = 0; i < K.length; i++) {
q[i] = new double[K[i]];
d[i] = new int[K[i]];
}

for(int i = 0; i < K.length; i++) {
long k = index;
x[i] = 0;

for(int j = 0; j < K[i]; j++) {
q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
d[i][j] = (int)(k % P[i]);
k = (k - d[i][j])/P[i];
x[i] += d[i][j] * q[i][j];
}
}
}

//Compute next point.
//Assume the current point is H(index).
//Compute H(index+1).
//@return a 2-dimensional point with coordinates in [0,1)^2
double[] nextPoint() {
index++;
for(int i = 0; i < K.length; i++) {
for(int j = 0; j < K[i]; j++) {
d[i][j]++;
x[i] += q[i][j];
if (d[i][j] < P[i]) {
break;
}
d[i][j] = 0;
x[i] -= (j == 0? 1.0: q[i][j-1]);
}
}
return x;
}
}

//Mapper class for Pi estimation.
//Generate points in a unit square and then
//count points inside/outside of the inscribed circle of the square.
public static class PiMapper extends MapReduceBase
implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

//Map method.
//@param offset samples starting from the (offset+1)th sample.
//@param size the number of samples for this map
//@param out output {ture->numInside, false->numOutside}
//@param reporter
public void map(LongWritable offset,
LongWritable size,
OutputCollector<BooleanWritable, LongWritable> out,
Reporter reporter) throws IOException {

final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
long numInside = 0L;
long numOutside = 0L;

for(long i = 0; i < size.get(); ) {
//generate points in a unit square
final double[] point = haltonsequence.nextPoint();

//count points inside/outside of the inscribed circle of the square
final double x = point[0] - 0.5;
final double y = point[1] - 0.5;
if (x*x + y*y > 0.25) {
numOutside++;
} else {
numInside++;
}

//report status
i++;
if (i % 1000 == 0) {
reporter.setStatus("Generated " + i + " samples.");
}
}

//output map results
out.collect(new BooleanWritable(true), new LongWritable(numInside));
out.collect(new BooleanWritable(false), new LongWritable(numOutside));
}
}

//Reducer class for Pi estimation.
//Accumulate points inside/outside results from the mappers.
public static class PiReducer extends MapReduceBase
implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

private long numInside = 0;
private long numOutside = 0;
private JobConf conf; //configuration for accessing the file system

//Store job configuration.
@Override
public void configure(JobConf job) {
conf = job;
}

// Accumulate number of points inside/outside results from the mappers.
// @param isInside Is the points inside?
// @param values An iterator to a list of point counts
// @param output dummy, not used here.
// @param reporter

public void reduce(BooleanWritable isInside,
Iterator<LongWritable> values,
OutputCollector<WritableComparable<?>, Writable> output,
Reporter reporter) throws IOException {
if (isInside.get()) {
for(; values.hasNext(); numInside += values.next().get());
} else {
for(; values.hasNext(); numOutside += values.next().get());
}
}

//Reduce task done, write output to a file.
@Override
public void close() throws IOException {
//write output to a file
Path outDir = new Path(TMP_DIR, "out");
Path outFile = new Path(outDir, "reduce-out");
FileSystem fileSys = FileSystem.get(conf);
SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
outFile, LongWritable.class, LongWritable.class,
CompressionType.NONE);
writer.append(new LongWritable(numInside), new LongWritable(numOutside));
writer.close();
}
}

//Run a map/reduce job for estimating Pi.
//@return the estimated value of Pi.
public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
)
throws IOException {
//setup job conf
jobConf.setJobName(PiEstimator.class.getSimpleName());

jobConf.setInputFormat(SequenceFileInputFormat.class);

jobConf.setOutputKeyClass(BooleanWritable.class);
jobConf.setOutputValueClass(LongWritable.class);
jobConf.setOutputFormat(SequenceFileOutputFormat.class);

jobConf.setMapperClass(PiMapper.class);
jobConf.setNumMapTasks(numMaps);

jobConf.setReducerClass(PiReducer.class);
jobConf.setNumReduceTasks(1);

// turn off speculative execution, because DFS doesn't handle
// multiple writers to the same file.
jobConf.setSpeculativeExecution(false);

//setup input/output directories
final Path inDir = new Path(TMP_DIR, "in");
final Path outDir = new Path(TMP_DIR, "out");
FileInputFormat.setInputPaths(jobConf, inDir);
FileOutputFormat.setOutputPath(jobConf, outDir);

final FileSystem fs = FileSystem.get(jobConf);
if (fs.exists(TMP_DIR)) {
throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
+ " already exists. Remove it first.");
}
if (!fs.mkdirs(inDir)) {
throw new IOException("Cannot create input directory " + inDir);
}

//generate an input file for each map task
try {
for(int i=0; i < numMaps; ++i) {
final Path file = new Path(inDir, "part"+i);
final LongWritable offset = new LongWritable(i * numPoints);
final LongWritable size = new LongWritable(numPoints);
final SequenceFile.Writer writer = SequenceFile.createWriter(
fs, jobConf, file,
LongWritable.class, LongWritable.class, CompressionType.NONE);
try {
writer.append(offset, size);
} finally {
writer.close();
}
System.out.println("Wrote input for Map #"+i);
}

//start a map/reduce job
System.out.println("Starting Job");
final long startTime = System.currentTimeMillis();
JobClient.runJob(jobConf);
final double duration = (System.currentTimeMillis() - startTime)/1000.0;
System.out.println("Job Finished in " + duration + " seconds");

//read outputs
Path inFile = new Path(outDir, "reduce-out");
LongWritable numInside = new LongWritable();
LongWritable numOutside = new LongWritable();
SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
try {
reader.next(numInside, numOutside);
} finally {
reader.close();
}

//compute estimated value
return BigDecimal.valueOf(4).setScale(20)
.multiply(BigDecimal.valueOf(numInside.get()))
.divide(BigDecimal.valueOf(numMaps))
.divide(BigDecimal.valueOf(numPoints));
} finally {
fs.delete(TMP_DIR, true);
}
}

//Parse arguments and then runs a map/reduce job.
//Print output in standard out.
//@return a non-zero if there is an error. Otherwise, return 0.
public int run(String[] args) throws Exception {
if (args.length != 2) {
System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
ToolRunner.printGenericCommandUsage(System.err);
return -1;
}

final int nMaps = Integer.parseInt(args[0]);
final long nSamples = Long.parseLong(args[1]);

System.out.println("Number of Maps = " + nMaps);
System.out.println("Samples per Map = " + nSamples);

final JobConf jobConf = new JobConf(getConf(), getClass());
System.out.println("Estimated value of Pi is "
+ estimate(nMaps, nSamples, jobConf));
return 0;
}

//main method for running it as a stand alone command.
public static void main(String[] argv) throws Exception {
System.exit(ToolRunner.run(null, new PiEstimator(), argv));
}
}
```

## <a name="appendix-d---the-10gb-graysort-source-code"></a>Ek D - 10gb graysort kaynak kodu
Bu bölümdeki İnceleme için TeraSort MapReduce programın kodunu sunulur.

```java
/**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements.  See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership.  The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License.  You may obtain a copy of the License at
    *
    *     http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

package org.apache.hadoop.examples.terasort;

import java.io.IOException;
import java.io.PrintStream;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.filecache.DistributedCache;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.Partitioner;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

/**
    * Generates the sampled split points, launches the job,
    * and waits for it to finish.
    * <p>
    * To run the program:
    * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
    */

public class TeraSort extends Configured implements Tool {
    private static final Log LOG = LogFactory.getLog(TeraSort.class);

    /**
    * A partitioner that splits text keys into roughly equal
    * partitions in a global sorted order.
    */

    static class TotalOrderPartitioner implements Partitioner<Text,Text>{
    private TrieNode trie;
    private Text[] splitPoints;

    /**
        * A generic trie node
        */
    static abstract class TrieNode {
        private int level;
        TrieNode(int level) {
        this.level = level;
        }
        abstract int findPartition(Text key);
        abstract void print(PrintStream strm) throws IOException;
        int getLevel() {
        return level;
        }
    }

    /**
        * An inner trie node that contains 256 children based on the next
        * character.
        */
    static class InnerTrieNode extends TrieNode {
        private TrieNode[] child = new TrieNode[256];

        InnerTrieNode(int level) {
        super(level);
        }
        int findPartition(Text key) {
        int level = getLevel();
        if (key.getLength() <= level) {
            return child[0].findPartition(key);
        }
        return child[key.getBytes()[level]].findPartition(key);
        }
        void setChild(int idx, TrieNode child) {
        this.child[idx] = child;
        }
        void print(PrintStream strm) throws IOException {
        for(int ch=0; ch < 255; ++ch) {
            for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
            }
            strm.print(ch);
            strm.println(" ->");
            if (child[ch] != null) {
            child[ch].print(strm);
            }
        }
        }
    }

    /**
        * A leaf trie node that does string compares to figure out where the given
        * key belongs between lower..upper.
        */
    static class LeafTrieNode extends TrieNode {
        int lower;
        int upper;
        Text[] splitPoints;
        LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
        super(level);
        this.splitPoints = splitPoints;
        this.lower = lower;
        this.upper = upper;
        }
        int findPartition(Text key) {
        for(int i=lower; i<upper; ++i) {
            if (splitPoints[i].compareTo(key) >= 0) {
            return i;
            }
        }
        return upper;
        }
        void print(PrintStream strm) throws IOException {
        for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
        }
        strm.print(lower);
        strm.print(", ");
        strm.println(upper);
        }
    }

    /**
        * Read the cut points from the given sequence file.
        * @param fs the file system
        * @param p the path to read
        * @param job the job config
        * @return the strings to split the partitions on
        * @throws IOException
        */
    private static Text[] readPartitions(FileSystem fs, Path p,
                                            JobConf job) throws IOException {
        SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
        List<Text> parts = new ArrayList<Text>();
        Text key = new Text();
        NullWritable value = NullWritable.get();
        while (reader.next(key, value)) {
        parts.add(key);
        key = new Text();
        }
        reader.close();
        return parts.toArray(new Text[parts.size()]);
    }

    /**
        * Given a sorted set of cut points, build a trie that will find the correct
        * partition quickly.
        * @param splits the list of cut points
        * @param lower the lower bound of partitions 0..numPartitions-1
        * @param upper the upper bound of partitions 0..numPartitions-1
        * @param prefix the prefix that we have already checked against
        * @param maxDepth the maximum depth we will build a trie for
        * @return the trie node that will divide the splits correctly
        */
    private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                        Text prefix, int maxDepth) {
        int depth = prefix.getLength();
        if (depth >= maxDepth || lower == upper) {
        return new LeafTrieNode(depth, splits, lower, upper);
        }
        InnerTrieNode result = new InnerTrieNode(depth);
        Text trial = new Text(prefix);
        // append an extra byte on to the prefix
        trial.append(new byte[1], 0, 1);
        int currentBound = lower;
        for(int ch = 0; ch < 255; ++ch) {
        trial.getBytes()[depth] = (byte) (ch + 1);
        lower = currentBound;
        while (currentBound < upper) {
            if (splits[currentBound].compareTo(trial) >= 0) {
            break;
            }
            currentBound += 1;
        }
        trial.getBytes()[depth] = (byte) ch;
        result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                        maxDepth);
        }
        // pick up the rest
        trial.getBytes()[depth] = 127;
        result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                    maxDepth);
        return result;
    }

    public void configure(JobConf job) {
        try {
        FileSystem fs = FileSystem.getLocal(job);
        Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
        splitPoints = readPartitions(fs, partFile, job);
        trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
        } catch (IOException ie) {
        throw new IllegalArgumentException("can't read paritions file", ie);
        }
    }

    public TotalOrderPartitioner() {
    }

    public int getPartition(Text key, Text value, int numPartitions) {
        return trie.findPartition(key);
    }

    }

    public int run(String[] args) throws Exception {
    LOG.info("starting");
    JobConf job = (JobConf) getConf();
    Path inputDir = new Path(args[0]);
    inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
    Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
    URI partitionUri = new URI(partitionFile.toString() +
                                "#" + TeraInputFormat.PARTITION_FILENAME);
    TeraInputFormat.setInputPaths(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    job.setJobName("TeraSort");
    job.setJarByClass(TeraSort.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(Text.class);
    job.setInputFormat(TeraInputFormat.class);
    job.setOutputFormat(TeraOutputFormat.class);
    job.setPartitionerClass(TotalOrderPartitioner.class);
    TeraInputFormat.writePartitionFile(job, partitionFile);
    DistributedCache.addCacheFile(partitionUri, job);
    DistributedCache.createSymlink(job);
    job.setInt("dfs.replication", 1);
    TeraOutputFormat.setFinalSync(job, true);
    JobClient.runJob(job);
    LOG.info("done");
    return 0;
    }

    /**
    * @param args
    */

    public static void main(String[] args) throws Exception {
    int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
    System.exit(res);
    }
}
```

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hadoop/submit-apache-hadoop-jobs-programmatically.md
[hdinsight-introduction]:hadoop/apache-hadoop-introduction.md

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]: hadoop/hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
