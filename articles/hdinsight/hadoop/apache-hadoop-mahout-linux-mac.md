---
title: Apache Mahout HDInsight (SSH) - Azure ile öneriler oluşturma
description: Apache Mahout machine learning kitaplığı HDInsight (Hadoop) ile film önerileri oluşturma için kullanmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: 63f1cfbf697f9cb1211e2c4671f64b19f933bc94
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129363"
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-apache-hadoop-in-hdinsight-ssh"></a>Apache Hadoop Linux tabanlı HDInsight (SSH) ile Apache Mahout kullanarak film önerileri oluşturma

[!INCLUDE [mahout-selector](../../../includes/hdinsight-selector-mahout.md)]

Nasıl kullanacağınızı öğrenin [Apache Mahout](https://mahout.apache.org) makine öğrenimi kitaplığı olan Azure HDInsight'ın Film önerileri oluşturma.

Mahout olduğu bir [makine öğrenimi] [ ml] Apache Hadoop için kitaplığı. Mahout, filtreleme, Sınıflandırma ve kümelendirme gibi verileri işlemek için algoritmalar içerir. Bu makalede, arkadaşlarınızın gördünüz filmler tabanlı film önerileri oluşturma için bir öneri altyapısını kullanın.

## <a name="prerequisites"></a>Önkoşullar

* Bir Linux tabanlı HDInsight kümesi. Bir oluşturma hakkında daha fazla bilgi için bkz: [HDInsight içinde Linux tabanlı Hadoop kullanmaya başlama][getstarted].

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir SSH istemcisi. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="apache-mahout-versioning"></a>Apache Mahout sürüm oluşturma

HDInsight içinde Mahout sürümü hakkında daha fazla bilgi için bkz. [HDInsight sürümleri ve Apache Hadoop bileşenleri](../hdinsight-component-versioning.md).

## <a name="recommendations"></a>Önerilerini anlama

Mahout tarafından sağlanan işlevlerden birini bir öneri altyapısıdır. Bu altyapı, veri biçimi kabul `userID`, `itemId`, ve `prefValue` (tercih öğesi için). Mahout ardından belirlemek için ortak oluşum analiz gerçekleştirin: *mutfağından hiçbir öğeyi sahip kullanıcılar diğer öğeleri bu mutfağından de*. Mahout sonra öneriler yapmak için kullanılan benzer öğe tercihleri, kullanıcılarla belirler.

Aşağıdaki iş akışı film verileri kullanan basitleştirilmiş bir örnektir:

* **Ortak oluşum**: ALi, Alice ve Bob tüm bağlanan *Star Wars*, *geri Empire durumda*, ve *Jedi dönüşü*. Ayrıca bu filmler herhangi biri gibi kullanıcılar diğer iki ister mahout belirler.

* **Ortak oluşum**: Bob ve Gamze ayrıca beğenmediğinizi *hayali İstilası*, *kopyaları saldırısını*, ve *Sith, Revenge*. Önceki üç filmler ayrıca beğenmediğinizi kullanıcılar bu üç filmler ister mahout belirler.

* **Benzerlik öneri**: Ali'nin ilk üç filmler beğenmediğinizi çünkü Mahout filmleri beğenmediğinizi benzer tercihleri, başkalarıyla görünür, ancak Joe (beğenmediğinizi/derecelendirilmiş) izlenen değil. Bu durumda, Mahout önerir *hayali İstilası*, *kopyaları saldırısını*, ve *Sith, Revenge*.

### <a name="understanding-the-data"></a>Verileri anlama

Rahat, [GroupLens araştırma] [ movielens] Mahout ile uyumlu bir biçimde film derecelendirmesi veri sağlar. Bu veriler, kümenin varsayılan depolama alanı üzerinde kullanılabilir `/HdiSamples/HdiSamples/MahoutMovieData`.

İki dosya vardır `moviedb.txt` ve `user-ratings.txt`. `user-ratings.txt` Analiz sırasında kullanılır. `moviedb.txt` Sonuçlarını görüntülerken kullanıcı dostu metin bilgi sağlamak için kullanılır.

Kullanıcı-ratings.txt içerdiği veri yapısını sahip `userID`, `movieID`, `userRating`, ve `timestamp`, her kullanıcı bir filmi nasıl yüksek dereceli gösterir. Verilerin bir örnek aşağıda verilmiştir:

    196    242    3    881250949
    186    302    3    891717742
    22     377    1    878887116
    244    51     2    880606923
    166    346    1    886397596

## <a name="run-the-analysis"></a>Analizini Çalıştır

Kümeye SSH bağlantısından, öneri işi çalıştırmak için aşağıdaki komutu kullanın:

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]  
> İşin tamamlanması birkaç dakika sürebilir ve birden çok MapReduce işleri çalıştırabilirsiniz.

## <a name="view-the-output"></a>Çıkışı görüntülemek

1. İş tamamlandıktan sonra oluşturulan çıktı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    Çıktı aşağıdaki gibi görünür:

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    İlk sütun `userID`. Bulunan değerler ' [' ve ']' olan `movieId`:`recommendationScore`.

2. Öneriler hakkında daha fazla bilgi sağlamak için çıktıyı moviedb.txt birlikte kullanabilirsiniz. İlk olarak, aşağıdaki komutları kullanarak yerel olarak kopyalayın:

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    Bu komut, çıktı verilerini adlı bir dosyaya kopyalar **recommendations.txt** film veri dosyalarının yanı sıra geçerli dizin.

3. Öneriler çıktı verileri için film adlarını arar bir Python betiği oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano show_recommendations.py
    ```

    Düzenleyici açıldığında, dosyanın içeriğini aşağıdaki metni kullanın:

   ```python
   #!/usr/bin/env python

   import sys

   if len(sys.argv) != 5:
        print "Arguments: userId userDataFilename movieFilename recommendationFilename"
        sys.exit(1)

   userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

   print "Reading Movies Descriptions"
   movieFile = open(movieFilename)
   movieById = {}
   for line in movieFile:
       tokens = line.split("|")
       movieById[tokens[0]] = tokens[1:]
   movieFile.close()

   print "Reading Rated Movies"
   userDataFile = open(userDataFilename)
   ratedMovieIds = []
   for line in userDataFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           ratedMovieIds.append((tokens[1],tokens[2]))
   userDataFile.close()

   print "Reading Recommendations"
   recommendationFile = open(recommendationFilename)
   recommendations = []
   for line in recommendationFile:
       tokens = line.split("\t")
       if tokens[0] == userId:
           movieIdAndScores = tokens[1].strip("[]\n").split(",")
           recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
           break
   recommendationFile.close()

   print "Rated Movies"
   print "------------------------"
   for movieId, rating in ratedMovieIds:
       print "%s, rating=%s" % (movieById[movieId][0], rating)
   print "------------------------"

   print "Recommended Movies"
   print "------------------------"
   for movieId, score in recommendations:
       print "%s, score=%s" % (movieById[movieId][0], score)
   print "------------------------"
   ```

    Tuşuna **Ctrl-X**, **Y**ve son olarak **Enter** verileri kaydetmek için.

4. Python betiğini çalıştırın. Aşağıdaki komut, tüm dosyaları indirdiğiniz dizine olduğunu varsayar:

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    Bu komut, kullanıcı kimliği 4 için oluşturulan öneriler bakar.

   * **Kullanıcı ratings.txt** derecelendirilmiş olmasa filmler almak için kullanılır.

   * **Moviedb.txt** filmler adlarını almak için kullanılır.

   * **Recommendations.txt** bu kullanıcı için film önerileri almak için kullanılır.

     Bu komutun çıktısı şu metne benzer:

       Yedi yıl içinde (1997) Tibet puan 5.0 = Indiana Jones ve son Crusade (1989), puan 5.0 = Jaws (1975) puan 5.0 = algılama ve Sensibility (1995), puan 5.0 = bağımsızlığı gün (ıd4) (1996) puanı My en iyi arkadaşınızın 5.0 = (1997'den) Düğün puan 5.0 = Jerry Maguire (1996) puan 5.0 = Scream 2 (1997), puan = 5.0, sonlandırma süresi bir (1996), puan 5.0 =

## <a name="delete-temporary-data"></a>Geçici verileri silme

Mahout işleri iş işlenirken oluşan geçici veri kaldırmayın. `--tempDir` Kolay silinmek belirli bir yolu içine geçici dosyalar yalıtmak için örnek işteki belirtilen parametre. Geçici dosyaları kaldırmak için aşağıdaki komutu kullanın:

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]  
> Komut yeniden çalıştırmak istiyorsanız, çıkış dizinine da silmeniz gerekir. Bu dizini silmek için aşağıdakileri kullanın:
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a>Sonraki adımlar

Mahout kullanmayı öğrendiniz, HDInsight üzerinde verilerle çalışma için diğer yöntemler keşfedin:

* [Apache Hive ile HDInsight](hdinsight-use-hive.md)
* [HDInsight ile Apache Pig](hdinsight-use-pig.md)
* [HDInsight ile MapReduce](hdinsight-use-mapreduce.md)

[build]: https://mahout.apache.org/developers/buildingmahout.html
[movielens]: https://grouplens.org/datasets/movielens/
[100k]: https://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]:apache-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: https://en.wikipedia.org/wiki/Machine_learning
[forest]: https://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
