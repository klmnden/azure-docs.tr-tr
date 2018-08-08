---
title: Hadoop MapReduce örnekleri - Azure HDInsight üzerinde çalıştırın.
description: Jar dosyalarını dahil HDInsight MapReduce örneklerini kullanarak başlayın. Kümeye bağlanmak için SSH kullanın ve ardından örnek işlerini çalıştırmak için Hadoop komutunu kullanın.
keywords: Örnek jar hadoop, hadoop örnekler jar, hadoop mapreduce örneklerini, mapreduce örnekleri
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: b29fb56f6ce244811aef924bb947a8b8ee8e4da4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597312"
---
# <a name="run-the-mapreduce-examples-included-in-hdinsight"></a>Dahil HDInsight MapReduce örneklerini çalıştırma

[!INCLUDE [samples-selector](../../../includes/hdinsight-run-samples-selector.md)]

HDInsight üzerinde Hadoop kullanmaya dahil MapReduce örneklerini çalıştırma hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

* **Bir HDInsight kümesi**: bkz [Linux'ta HDInsight Hive ile Hadoop kullanmaya başlama](apache-hadoop-linux-tutorial-get-started.md)

    > [!IMPORTANT]
    > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **Bir SSH istemcisi**: daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="the-mapreduce-examples"></a>MapReduce örnekleri

**Konum**: örnekleri HDInsight kümesinde bulunan `/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar`.

**İçeriği**: Aşağıdaki örnekleri bu Arşiv'de yer alır:

* `aggregatewordcount`: Bir toplama giriş dosyaları sözcükleri sayar mapreduce programını temel.
* `aggregatewordhist`: Bir toplama giriş dosyaları bir kelimelerin histogram hesaplayan bir mapreduce programını temel.
* `bbp`: Pi sayısının tam basamak sayısını hesaplamak için Bailey Borwein Plouffe kullanır bir mapreduce programını.
* `dbcount`Bir veritabanında depolanan sayfa görüntülemesi günlükleri sayar örnek proje.
* `distbbp`: Pi sayısının tam BITS hesaplamak için BBP türü formül kullanır bir mapreduce programını.
* `grep`: Girişteki bir regex eşleşmeleri sayar bir mapreduce programını.
* `join`: Üzerinde birleştirme gerçekleştirir bir işin sıralanmış, eşit veri kümeleri bölümlenmiş.
* `multifilewc`: Birden fazla dosyalardan sözcükleri sayar bir proje.
* `pentomino`: Bir mapreduce pentomino sorunlara çözümler bulmak için program yerleştirme Döşe.
* `pi`: Pi benzeri bir Monte kullanarak tahminleri bir mapreduce programını Carlo yöntemi.
* `randomtextwriter`: Rastgele metin verileri düğüm başına 10 GB Yazar bir mapreduce programını.
* `randomwriter`: Düğüm başına rastgele veri 10 GB Yazar bir mapreduce programını.
* `secondarysort`: İkincil bir sıralama azaltın aşamasına tanımlama bir örnek.
* `sort`: Rastgele yazıcı tarafından yazılan veriler sıralar bir mapreduce programını.
* `sudoku`: Bir sudoku Çözücü.
* `teragen`: Terasort için veriler oluşturur.
* `terasort`: Terasort çalıştırın.
* `teravalidate`: Terasort sonuçlarını denetleniyor.
* `wordcount`: Giriş dosyaları sözcükleri sayar bir mapreduce programını.
* `wordmean`: Giriş dosyaları bir kelimelerin ortalama süresi sayar bir mapreduce programını.
* `wordmedian`: Giriş dosyaları bir kelimelerin ORTANCA uzunluğu sayar bir mapreduce programını.
* `wordstandarddeviation`: Giriş dosyaları bir kelimelerin uzunluğu standart sapmasını sayar bir mapreduce programını.

**Kaynak kodu**: Bu örnekler için kaynak kodu dahil HDInsight kümesinde `/usr/hdp/current/hadoop-client/src/hadoop-mapreduce-project/hadoop-mapreduce-examples`.

## <a name="run-the-wordcount-example"></a>Wordcount örneği çalıştırma

1. SSH kullanarak HDInsight için bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Gelen `username@#######:~$` isteminde, örneklerini listelemek için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar
    ```

    Bu komut, bu belgenin önceki bölümdeki örnek listesi oluşturur.

3. Belirli bir örneği temel Yardım almak için aşağıdaki komutu kullanın. Bu durumda, **wordcount** örnek:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount
    ```

    Şu iletiyi alırsınız:

        Usage: wordcount <in> [<in>...] <out>

    Bu ileti, çeşitli giriş yollarından kaynak belgeler sağlayabilirsiniz gösterir. ' % S'çıkış (sayısı kaynak belgelerde bir kelimelerin) depolandığı son yoludur.

4. Not defterleri, Leonardo Da kümenizle örnek veri olarak sağlanan Vinci, tüm sözcükleri saymak için aşağıdakileri kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount
    ```

    Bu iş öğesinden okumak için giriş `/example/data/gutenberg/davinci.txt`. Bu örnekte depolanan çıktısı `/example/data/davinciwordcount`. Yerel dosya sistemine değil küme için varsayılan depolama alanı her iki yol bulunur.

   > [!NOTE]
   > Wordcount örneği için Yardım'a belirtildiği gibi birden fazla giriş dosyası da belirtebilirsiniz. Örneğin, `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` davinci.txt hem ulysses.txt sözcükleri sayar.

5. İş tamamlandığında, çıkışı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -cat /example/data/davinciwordcount/*
    ```

    Bu komut iş tarafından üretilen tüm çıktı dosyaları art arda ekler. Bu, konsola çıkışı görüntüler. Çıktı aşağıdaki metne benzer:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Satır sonundaki bir sözcük ve isteğe bağlı olarak kaç kez giriş verilerini oluştu temsil eder.

## <a name="the-sudoku-example"></a>Sudoku örneği

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) dokuz 3 x 3 ızgaralar oluşan bir Bulmaca olduğu. Başkalarının boştur ve çözmek için boş hücreler olmaktır kılavuzdaki bazı hücreler sayı bulunur. Önceki bağlantı Bulmacanın hakkında daha fazla bilgi var, ancak bu örnek amacı boş hücreler için çözmek için. Bu nedenle bizim giriş şu biçimde bir dosya olmalıdır:

* Dokuz sütunların dokuz satırları
* Her sütun bir sayı içerebilir veya `?` (boş bir hücreye gösterir)
* Hücre bir boşluk ile ayrılır.

Sudoku yapbozlar CAN oluşturmak için belirli bir yolu yoktur; bir satır veya sütun bir sayı yinelenemez. Doğru şekilde oluşturulmuş olan HDInsight kümesinde bir örnek verilmiştir. Şu konumdadır: `/usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta` ve aşağıdaki metni içerir:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

Bu örnekte sorun Sudoku örneği çalıştırmak için aşağıdaki komutu kullanın:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/*/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta
```

Sonuçlar aşağıdaki metne benzer şekilde görünür:

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

Pi örnek bir istatistik kullanır (yarı-Monte Carlo) pi değerini tahmin etmek için yöntemi. Noktaları rastgele bir birim karede yerleştirilir. Kare bir daire de içerir. Noktaları daire içinde kalan olasılık daire alanına eşit PI/4. Pi değerini 4R değerinden tahmin edilebilir. R kare içinde noktalarını toplam sayısına daire içinde noktalarını sayısını oranıdır. Daha büyük kullanılan noktaları, daha iyi tahmin örneğidir.

Bu örneği çalıştırmak için aşağıdaki komutu kullanın. Bu komut, pi değerini tahmin etmek için 10.000.000 örnekleri ile her 16 haritalar kullanır:

```bash
yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000
```

Bu komut tarafından döndürülen değer benzer **3.14159155000000000000**. Başvurular için ilk 10 ondalık pi'nin 3.1415926535 yerlerdir.

## <a name="10-gb-greysort-example"></a>10 GB Greysort örneği

GraySort Kıyaslama sıralama ' dir. Ölçüm, büyük miktarlarda veri, genellikle en az bir 100 TB sıralama sırasında elde sıralama (TB/dakika) hızıdır.

Bu örnek, oldukça hızlı bir şekilde çalıştırılabilir böylece büyüklükteki bir 10 GB veri kullanmaktadır. Arun Murthy Owen O'Malley ile geliştirilen MapReduce uygulamalar kullanır. Bu uygulamaların yıllık genel amaçlı ("daytona") terabayt sıralama Kıyaslama 0.578 TB/dak (100 TB 173 dakika cinsinden) fiyatı, 2009 kazandı. Bu ve diğer sıralama değerlendirmeleri hakkında daha fazla bilgi için bkz. [Sortbenchmark](http://sortbenchmark.org/) site.

Bu örnek, üç adet MapReduce programlarını kullanır:

* **TeraGen**: sıralamak için veri satırlarını oluşturan bir MapReduce programını

* **TeraSort**: giriş verileri örnekler ve MapReduce toplam sıralamaya verileri sıralamak için kullanılır

    Özel bir bölümleyici dışında standart MapReduce sıralama TeraSort olur. Her azaltma için anahtar aralığını tanımlamak örneklenen N-1 anahtarların sıralanmış bir bölümleyici kullanır. Özellikle, örneği [i-1] tüm anahtarlar gibi < key = < örnek [i] i azaltmak için gönderilir. Bu bölümleyici çıktısını azaltmak i + 1 değerinden çıkışlarına i azaltmak garanti tüm.

* **TeraValidate**: çıkış genel olarak sıralanmış doğrulayan bir MapReduce programını

    Çıktı dizininde dosya başına bir harita oluşturur ve her eşleme her anahtar Öncekine küçük veya eşit olmasını sağlar. Harita işlevi, her bir dosyanın ilk ve son anahtarların kayıtları oluşturur. Reduce işlevi dosya ilk anahtarını dosyası i-1 son anahtardan daha büyük olmasını sağlar. Herhangi bir sorunu sıralamaya anahtarları ile azaltma aşaması çıkış olarak raporlanır.

Aşağıdaki adımlar, verileri oluşturmak için sıralama ve ardından çıktısını doğrulamak:

1. HDInsight kümenin varsayılan depolama alanı için depolanan veriler, 10 GB oluşturmak `/example/data/10GB-sort-input`:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input
    ```

    `-Dmapred.map.tasks` Hadoop kullanmak için bu iş için kaç harita görevleri bildirir. İş 10 GB veri oluşturmak ve adresinden depolamak için son iki parametre isteyin `/example/data/10GB-sort-input`.

2. Verileri sıralamak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output
    ```

    `-Dmapred.reduce.tasks` Hadoop işi için kullanılacak görevler kaç azaltır söyler. Son iki parametre yalnızca giriş ve çıkış veri konumlardır.

3. Sıralama tarafından oluşturulan verileri doğrulamak için aşağıdakileri kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Linux tabanlı HDInsight kümeleriyle dahil örneklerini çalıştırma öğrendiniz. HDInsight ile Pig, Hive ve MapReduce kullanma hakkında daha fazla öğreticiler için aşağıdaki konulara bakın:

* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-introduction]:apache-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

