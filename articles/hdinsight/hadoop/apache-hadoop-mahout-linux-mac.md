---
title: Mahout ve Hdınsight (SSH) - Azure kullanarak öneri oluşturmak | Microsoft Docs
description: Apache Mahout machine learning kitaplığı Hdınsight (Hadoop) ile film önerileri oluşturma için nasıl kullanılacağını öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c78ec37c-9a8c-4bb6-9e38-0bdb9e89fbd7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: larryfr
ms.openlocfilehash: ea9706d30797385718db5cb89bd5399b251f3253
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight-ssh"></a>Hdınsight (SSH) Linux tabanlı Hadoop ile Apache Mahout kullanarak film önerileri oluşturma

[!INCLUDE [mahout-selector](../../../includes/hdinsight-selector-mahout.md)]

Nasıl kullanacağınızı öğrenin [Apache Mahout](http://mahout.apache.org) machine learning kitaplığı Azure Hdınsight'ın Film önerileri oluşturma ile.

Mahout olan bir [makine öğrenme] [ ml] Apache Hadoop için kitaplığı. Mahout, Sınıflandırma, filtreleme ve kümeleme gibi veri işlemeye algoritmalarını içerir. Bu makalede, arkadaşlarınızın gördünüz filmler dayalı film önerileri oluşturma için bir öneri altyapısı kullanın.

## <a name="prerequisites"></a>Önkoşullar

* Linux tabanlı Hdınsight kümesi. Bir oluşturma hakkında daha fazla bilgi için bkz: [Hdınsight'ta Linux tabanlı Hadoop ile çalışmaya başlamak][getstarted].

> [!IMPORTANT]
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir SSH istemcisi. Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="mahout-versioning"></a>Mahout sürüm oluşturma

Hdınsight'ta Mahout sürümü hakkında daha fazla bilgi için bkz: [Hdınsight sürümleri ve Hadoop bileşenleri](../hdinsight-component-versioning.md).

## <a name="recommendations"></a>Anlama önerileri

Mahout tarafından sağlanan işlevleri bir öneri altyapısı biridir. Bu altyapı biçiminde verilerini kabul eden `userID`, `itemId`, ve `prefValue` (tercih öğesi için). Mahout sonra belirlemek için ortak oluşumu analiz gerçekleştirin: *bir öğe için bir öncelik olan kullanıcıların diğer öğeler bir tercih bunlar için de*. Mahout sonra öneriler yapmak için kullanılan benzer öğe Tercihler kullanıcılarla belirler.

Aşağıdaki iş akışı film verilerini kullanan basitleştirilmiş bir örnek verilmiştir:

* **Ortak oluşumu**: Can, Alice ve Bob tüm beğendiğinizi *yıldız çatışmaları*, *geri Empire düşer*, ve *Jedi dönüşünü*. Bu filmler herhangi biri de gibi kullanıcıların diğer iki ister mahout belirler.

* **Ortak oluşumu**: Bob ve Alice de beğendiğinizi *hayali İstilası*, *klonlar saldırı*, ve *Sith Revenge*. Önceki üç filmler de ilişkilendirilmiş kullanıcılar bu üç filmler ister mahout belirler.

* **Benzerlik öneri**: çünkü Joe beğendiğinizi ilk üç filmler, Mahout o beğendiğinizi benzer Tercihler başkalarıyla filmler görünür, ancak Joe olmayan izlenen (beğendiğinizi/derecelendirilmiş). Bu durumda, Mahout önerir *hayali İstilası*, *klonlar saldırı*, ve *Sith Revenge*.

### <a name="understanding-the-data"></a>Veri anlama

Uygun şekilde, [GroupLens araştırma] [ movielens] Mahout ile uyumlu bir biçimde film derecelendirme veri sağlar. Bu veriler, kümenin varsayılan depolama kullanılabilir `/HdiSamples/HdiSamples/MahoutMovieData`.

İki dosya vardır `moviedb.txt` ve `user-ratings.txt`. `user-ratings.txt` Dosyası Çözümleme sırasında kullanılır. `moviedb.txt` Sonuçları görüntülerken kullanıcı dostu metin bilgileri sağlamak için kullanılır.

Kullanıcı-ratings.txt bulunan verileri yapısını sahip `userID`, `movieID`, `userRating`, ve `timestamp`, her kullanıcı bir filmi nasıl yüksek oranda derecelendirilmiş gösterir. Verileri bir örneği burada verilmiştir:

    196    242    3    881250949
    186    302    3    891717742
    22    377    1    878887116
    244    51    2    880606923
    166    346    1    886397596

## <a name="run-the-analysis"></a>Analiz çalıştırma

Küme için bir SSH bağlantısından öneri işi çalıştırmak için aşağıdaki komutu kullanın:

```bash
mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp
```

> [!NOTE]
> İşin tamamlanması birkaç dakika sürebilir ve birden çok MapReduce işleri çalışabilir.

## <a name="view-the-output"></a>Çıktısını görüntüleyin

1. İş tamamlandığında, oluşturulan çıktı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /example/data/mahoutout/part-r-00000
    ```

    Çıktı aşağıdaki gibidir:

        1    [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2    [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3    [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4    [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    İlk sütun `userID`. İçinde yer alan değerler ' [' ve ']' olan `movieId`:`recommendationScore`.

2. Moviedb.txt birlikte çıkış öneriler hakkında daha fazla bilgi sağlamak için kullanabilirsiniz. İlk olarak, yerel olarak aşağıdaki komutları kullanarak dosyaları kopyalayın:

    ```bash
    hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
    hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .
    ```

    Bu komut çıktı verilerini adlı bir dosyaya kopyalar **recommendations.txt** film veri dosyalarının yanı sıra geçerli dizin.

3. Film adları önerileri çıktı verileri için arayan bir Python betiği oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano show_recommendations.py
    ```

    Düzenleyici açıldığında aşağıdaki metni dosyasının içeriği kullanın:

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

4. Python komut dosyasını çalıştırın. Aşağıdaki komut, tüm dosyaları indirdiğiniz dizininde olduğunu varsayar:

    ```bash
    python show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt
    ```

    Bu komut, kullanıcı kimliği 4 için oluşturulan önerileri bakar.

    * **Kullanıcı ratings.txt** dosya derecelendirilmiş filmler almak için kullanılır.

    * **Moviedb.txt** dosya filmler adlarını almak için kullanılır.

    * **Recommendations.txt** bu kullanıcı için film önerileri almak için kullanılır.

     Bu komutun çıktısı aşağıdakine benzer:

        Tibet (1997) yedi yıllarda puan 5.0 = Indiana Can ve son Crusade (1989), puan 5.0 = Jaws (1975'ten), puan 5.0 = algılama ve Sensibility (1995), puan 5.0 = bağımsızlığı gün (ID4) (1996) puanı 5.0 My en iyi arkadaşınızın = (1997) Düğün puan 5.0 = Jerry Maguire (1996) puan 5.0 = Scream 2 (1997) puanı 5.0 = zaman KILL, bir (1996), puan 5.0 =

## <a name="delete-temporary-data"></a>Geçici verileri Sil

Mahout işleri iş işlenirken oluşturulan geçici verileri kaldırmayın. `--tempDir` Parametresi kolay silme işlemi için belirli bir yolu içine geçici dosyalar yalıtmak için örnek proje belirtilen. Geçici dosyaları kaldırmak için aşağıdaki komutu kullanın:

```bash
hdfs dfs -rm -f -r /temp/mahouttemp
```

> [!WARNING]
> Komut yeniden çalıştırmak istiyorsanız, çıktı dizinine silmeniz gerekir. Bu dizin silmek için aşağıdakileri kullanın:
>
> `hdfs dfs -rm -f -r /example/data/mahoutout`


## <a name="next-steps"></a>Sonraki adımlar

Mahout kullanmayı öğrendiniz, Hdınsight'ta veri ile çalışmanın diğer yolları Bul:

* [Hdınsight ile hive](hdinsight-use-hive.md)
* [Hdınsight ile pig](hdinsight-use-pig.md)
* [Hdınsight ile MapReduce](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]:apache-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
