---
title: Hadoop MapReduce Hdınsight'ta - Azure örneklerini çalıştırmak | Microsoft Docs
description: MapReduce örnekleri Hdınsight'ta dahil jar dosyalarında kullanmaya başlayın. Kümeye bağlanmak için SSH kullanın ve ardından örnek işlerini çalıştırmak için Hadoop komutunu kullanın.
keywords: hadoop örnek jar, hadoop örnekler jar, hadoop mapreduce örnekler, mapreduce örnekleri
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1d2a0b9-1659-4fab-921e-4a8990cbb30a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: larryfr
ms.openlocfilehash: 14f860d64c482ac7ef74512aea4850821d30132c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a>Hdınsight'ta dahil MapReduce örnekler çalıştırın

[!INCLUDE [samples-selector](../../../includes/hdinsight-run-samples-selector.md)]

Hdınsight'ta Hadoop ile birlikte gelen MapReduce örneklerini çalıştırmak öğrenin.

## <a name="prerequisites"></a>Önkoşullar

* **Hdınsight kümesi**: bkz [Hadoop ile Linux'ta Hdınsight Hive kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md)

    > [!IMPORTANT]
    > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Bir SSH istemcisi**: daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="the-mapreduce-examples"></a>MapReduce örnekleri

**Konum**: örnekler Hdınsight kümesinde bulunan `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.

**İçeriği**: Aşağıdaki örnekleri bu arşivde yer alır:

* `aggregatewordcount`: Bir toplama girdi dosyaları sözcükleri sayar mapreduce program temel.
* `aggregatewordhist`: Bir toplama girdi dosyaları sözcükleri histogram hesaplar mapreduce program temel.
* `bbp`Pi sayısının tam basamak hesaplamak için Bailey Borwein Plouffe kullanır mapreduce program.
* `dbcount`: Bir veritabanında depolanan sayfa görünümü günlükleri sayar bir örnek işi.
* `distbbp`Pi sayısının tam BITS hesaplamak için BBP türü formülü kullanır mapreduce program.
* `grep`Regex girişte eşleşme sayısı mapreduce program.
* `join`: Üzerinde birleştirme gerçekleştirir bir iş sıralanmış, veri kümeleri eşit olarak bölümlenmiş.
* `multifilewc`: Birkaç dosyalarından sözcükleri sayar bir iş.
* `pentomino`: Bir mapreduce pentomino sorunlara çözüm bulmak için program yerleştirmede döşeme.
* `pi`Yarı Monte kullanarak Pi tahminleri mapreduce program Carlo yöntemi.
* `randomtextwriter`Düğüm başına rastgele metinsel veri 10 GB Yazar mapreduce program.
* `randomwriter`Düğüm başına rastgele veri 10 GB Yazar mapreduce program.
* `secondarysort`: İkincil bir sıralama azaltın aşamasına tanımlama bir örnek.
* `sort`Rastgele yazıcı tarafından yazılan veriler sıralar mapreduce program.
* `sudoku`: Bir sudoku Çözücü.
* `teragen`: Veri terasort için oluşturun.
* `terasort`: Terasort çalıştırın.
* `teravalidate`: Terasort sonuçlarını denetleniyor.
* `wordcount`Giriş dosyaları sözcükleri sayar mapreduce program.
* `wordmean`Ortalama uzunluğu girdi dosyaları sözcükleri sayar mapreduce program.
* `wordmedian`ORTANCA uzunluğu girdi dosyaları sözcükleri sayar mapreduce program.
* `wordstandarddeviation`Giriş dosyaları sözcükleri uzunluğu standart sapmasını sayar mapreduce program.

**Kaynak kodu**: Bu örnekleri için kaynak kodunu dahil Hdınsight kümesinde `/usr/hdp/current/hadoop-client/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.

## <a name="run-the-wordcount-example"></a>Wordcount örneği çalıştırın

1. SSH kullanarak Hdınsight bağlayın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Gelen `username@#######:~$` isteminde, örneklerini listelemek için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Bu komut, bu belgenin önceki bölümden örnek listesi oluşturur.

3. Belirli bir örneği temel Yardım almak için aşağıdaki komutu kullanın. Bu durumda, **wordcount** örnek:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Aşağıdaki iletiyi alırsınız:

        Usage: wordcount <in> [<in>...] <out>

    Bu ileti, kaynak belgeler için birkaç giriş yollarının sağlayabilir gösterir. (Kaynak belgeleri sözcükleri sayısı) çıkış depolandığı son yoludur.

4. Dizüstü bilgisayarlar, Leonardo Da örnek veri kümenizle sağlanan Vinci, tüm sözcükleri saymak için aşağıdakileri kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Bu işi okuma girdisi `/example/data/gutenberg/davinci.txt`. Bu örnek depolanan çıktısı `/example/data/davinciwordcount`. Her iki yolları kümesi, yerel dosya sistemi için varsayılan depolama bulunur.

   > [!NOTE]
   > Wordcount örneği için Yardım'a belirtildiği gibi birden fazla girdi dosyası da belirtebilirsiniz. Örneğin, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` davinci.txt ve ulysses.txt sözcükleri sayar.

5. İş tamamlandığında, çıkışı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Bu komut iş tarafından üretilen tüm çıktı dosyaları art arda ekler. Konsola çıktı görüntüler. Çıktı aşağıdaki metne benzer:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Her satır bir sözcük ve isteğe bağlı olarak kaç kez giriş verilerini oluştu temsil eder.

## <a name="the-sudoku-example"></a>Sudoku örneği

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) olan dokuz 3 x 3 ızgaralar oluşan bir mantık Bulmacanın. Izgara bazı hücreler, başkalarının boş ve hedeftir boş hücrelerin çözmeye çalışırken numaraları içeriyor. Önceki bağlantı Bulmacanın hakkında daha fazla bilgi gerekiyor, ancak bu örnek amacı için boş hücreler çözmek için. Bu nedenle bizim giriş aşağıdaki biçimde bir dosya olmalıdır:

* Dokuz sütunların dokuz satırları
* Her sütunun bir sayı içerebilir veya `?` (boş bir hücre gösterir)
* Hücreleri boşlukla ayrılmış

Sudoku yapbozlar oluşturmak için belirli bir yolu yoktur; bir sütun veya satır numarasında yinelenemez. Doğru şekilde oluşturulduğundan Hdınsight kümesinde bir örnek verilmiştir. Şu konumdadır: `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` ve aşağıdaki metni içerir:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

Bu örnek sorunu Sudoku örneği çalıştırmak için aşağıdaki komutu kullanın:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

Sonuçlar aşağıdakine benzer şekilde görünür:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi--example"></a>Pi (π) örneği

Pi örnek bir istatistik kullanır (yarı Monte Carlo) pi değerini tahmin etmek için yöntem. Noktaları rastgele bir birim kare yerleştirilir. Kare bir daire de içerir. Noktaları daire içinde kalan olasılık daire alana eşit pi/4. Pi değerini 4R değerinden tahmin edilebilir. R kare içinde noktalarını toplam sayısı için bir daire içinde olduğunda puan sayısının orandır. Büyük kullanılan noktaları, daha iyi tahmin örneğidir.

Bu örneği çalıştırmak için aşağıdaki komutu kullanın. Bu komut, pi değerini tahmin etmek için 10,000,000 örnekleri ile her 16 eşlemeleri kullanır:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

Bu komut tarafından döndürülen değer benzer **3.14159155000000000000**. Başvuru için ilk 10 ondalık pi'nin 3.1415926535 yerlerdir.

## <a name="10-gb-greysort-example"></a>10 GB Greysort örneği

GraySort Kıyaslama sıralama ' dir. Büyük miktarlarda verinin, genellikle en az bir 100 TB sıralama sırasında elde edilen sıralama oranı (TB dakikada) ölçümüdür.

Bu örnek uygun bir 10 GB veri kullanır, böylece oldukça hızlı bir şekilde çalıştırılabilir. Owen O'Malley ve Arun Murthy tarafından geliştirilen MapReduce uygulamaları kullanır. Bu uygulamaların yıllık genel amaçlı ("daytona") terabayt sıralama Kıyaslama 0.578 TB/dak (100 TB 173 dakika cinsinden) oranı ile 2009 kazanmıştır. Bu ve diğer sıralama değerlendirmeleri hakkında daha fazla bilgi için bkz: [Sortbenchmark](http://sortbenchmark.org/) site.

Bu örnek, MapReduce programlar üç kümesi kullanır:

* **TeraGen**: sıralamak için veri satırı oluşturur A MapReduce programı

* **TeraSort**: giriş verilerini örnekleri ve toplam sıralamaya verileri sıralamak için MapReduce kullanır

    TeraSort özel bölümleyici dışında bir standart MapReduce sıralama ' dir. Bölümleyici her azaltma için anahtar aralığı tanımlayın örneklenen N-1 anahtarları sıralı listesini kullanır. Özellikle, [i-1] örnek tüm anahtarları böyle < = anahtar < örnek [i] i azaltmak için gönderilir. Bu bölümleyici çıktısını azaltmak i + 1 değerinden çıkışları i azaltmak garanti tüm.

* **TeraValidate**: çıktı genel olarak kullanılan doğrular A MapReduce programı

    Belirtilen çıkış dizinine dosya başına bir harita oluşturur ve her eşleme her anahtar öncekinin küçük veya buna eşit olmasını sağlar. Map işlevi her dosyanın ilk ve son anahtarların kayıtları oluşturur. Reduce işlevi dosya ilk anahtarı dosyası i-1 son anahtarı büyük olmasını sağlar. Herhangi bir sorun bozuk anahtarlarla azaltın aşamasının çıkış olarak raporlanır.

Aşağıdaki kullanın verileri oluşturmaya yönelik adımlar sıralamak ve çıktı doğrulayın:

1. Hdınsight kümenin varsayılan depolama depolanan veri 10 GB oluşturmak `/example/data/10GB-sort-input`:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    `-Dmapred.map.tasks` Hadoop bu proje için kullanmak için kaç tane harita görevleri bildirir. Son iki parametre 10 GB veri oluşturmak ve yerinde depolamak için iş istemeniz `/example/data/10GB-sort-input`.

2. Verileri sıralamak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    `-Dmapred.reduce.tasks` Kaç iş için kullanmayı görevleri azaltmak Hadoop söyler. Son iki parametre yalnızca giriş ve çıkış veri konumlarının.

3. Sıralama ölçütü oluşturulan verileri doğrulamak için aşağıdakileri kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Linux tabanlı Hdınsight kümeleri ile dahil örneklerini çalıştırma öğrendiniz. Hdınsight ile Pig, Hive ve MapReduce kullanma hakkında daha fazla öğreticileri için aşağıdaki konulara bakın:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-introduction]:apache-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

