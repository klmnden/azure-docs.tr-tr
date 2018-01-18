---
title: "Hdınsight - Azure ile MapReduce işleri akış Python geliştirme | Microsoft Docs"
description: "Akış MapReduce işlerinin Python kullanmayı öğrenin. Hadoop, Java dışındaki dillerde yazmak için MapReduce için akış bir API sağlar."
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7631d8d9-98ae-42ec-b9ec-ee3cf7e57fb3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2018
ms.author: larryfr
ms.openlocfilehash: 2563927684720a0be1144fa51aea7b9dae4abe7e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a>MapReduce programları Hdınsight için akış Python geliştirme

Python MapReduce işlemleri akışında kullanmayı öğrenin. Hadoop harita yazma ve Java dışındaki dillerde işlevleri azaltmak sağlar MapReduce akış bir API sağlar. Bu belgede yer alan adımlar eşleme uygulamak ve Python bileşenlerinde azaltın.

## <a name="prerequisites"></a>Önkoşullar

* Bir Hdınsight kümesinde Linux tabanlı Hadoop

  > [!IMPORTANT]
  > Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir metin düzenleyicisi

  > [!IMPORTANT]
  > Metin Düzenleyicisi LF olarak satırı bitiş kullanmanız gerekir. CRLF satır sonu kullanarak hataları MapReduce işi Linux tabanlı Hdınsight kümelerinde çalıştırırken neden olur.

* `ssh` Ve `scp` komutları veya [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-3.8.0)

## <a name="word-count"></a>Sözcük Sayımı

Bu temel sözcük sayımı bir python Eşleyici ve reducer uygulanan bir örnektir. Eşleyici cümleleri tek tek sözcüklere böler ve reducer sözcükleri toplar ve bir çıktı oluşturmak için sayar.

Aşağıdaki akış çizelgesi ne sırasında harita olur ve aşamaları azaltmak gösterilmektedir.

![mapreduce işlem çizimi](./media/apache-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a>Akış MapReduce

Hadoop, eşleme içeren bir dosya belirtin ve bir iş tarafından kullanılan mantığı azaltmak sağlar. Eşleme için özel gereksinimleri ve mantığı azaltmak şunlardır:

* **Giriş**: eşleme ve azaltma bileşenleri STDIN giriş verilerinin okuma gerekir.
* **Çıktı**: eşleme ve azaltma bileşenleri STDOUT çıkış veri yazma gerekir.
* **Veri biçimi**: tüketilen ve üretilen veriler bir sekme karakteriyle ayrılmış bir anahtar/değer çifti olmalıdır.

Python kolayca işleyebilir bu gereksinimleri kullanarak `sys` STDIN ve kullanarak okumak için Modülü `print` STDOUT yazdırmak için. Kalan görev yalnızca bir sekme verilerle biçimlendirme (`\t`) anahtar ve değer arasında karakter.

## <a name="create-the-mapper-and-reducer"></a>Eşleyici ve reducer oluşturma

1. Adlı bir dosya oluşturun `mapper.py` ve içeriği olarak aşağıdaki kodu kullanabilirsiniz:

   ```python
   #!/usr/bin/env python

   # Use the sys module
   import sys

   # 'file' in this case is STDIN
   def read_input(file):
       # Split each line into words
       for line in file:
           yield line.split()

   def main(separator='\t'):
       # Read the data using read_input
       data = read_input(sys.stdin)
       # Process each word returned from read_input
       for words in data:
           # Process each word
           for word in words:
               # Write to STDOUT
               print '%s%s%d' % (word, separator, 1)

   if __name__ == "__main__":
       main()
   ```

2. Adlı bir dosya oluşturun **reducer.py** ve içeriği olarak aşağıdaki kodu kullanabilirsiniz:

   ```python
   #!/usr/bin/env python

   # import modules
   from itertools import groupby
   from operator import itemgetter
   import sys

   # 'file' in this case is STDIN
   def read_mapper_output(file, separator='\t'):
       # Go through each line
       for line in file:
           # Strip out the separator character
           yield line.rstrip().split(separator, 1)

   def main(separator='\t'):
       # Read the data using read_mapper_output
       data = read_mapper_output(sys.stdin, separator=separator)
       # Group words and counts into 'group'
       #   Since MapReduce is a distributed process, each word
       #   may have multiple counts. 'group' will have all counts
       #   which can be retrieved using the word as the key.
       for current_word, group in groupby(data, itemgetter(0)):
           try:
               # For each word, pull the count(s) for the word
               #   from 'group' and create a total count
               total_count = sum(int(count) for current_word, count in group)
               # Write to stdout
               print "%s%s%d" % (current_word, separator, total_count)
           except ValueError:
               # Count was not a number, so do nothing
               pass

   if __name__ == "__main__":
       main()
   ```

## <a name="run-using-powershell"></a>PowerShell kullanarak çalıştırma

Dosyalarınızın doğru satır sonları olmasını sağlamak için aşağıdaki PowerShell betiğini kullanın:

[!code-powershell[main](../../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

Dosyaları karşıya yükleme, işini çalıştırın ve çıktıyı görüntülemek için aşağıdaki PowerShell betiğini kullanın:

[!code-powershell[main](../../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a>Bir SSH oturumunda

1. Aynı dizinde, geliştirme ortamınızdan `mapper.py` ve `reducer.py` dosyaları, aşağıdaki komutu kullanın:

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    Değiştir `username` , kümeniz için SSH kullanıcı adı ve `clustername` , küme adı.

    Bu komut dosyaları, yerel sistemden baş düğüme kopyalar.

    > [!NOTE]
    > SSH hesabınızı korumak için parola kullandıysanız, parola istenir. Bir SSH anahtarı kullandıysanız, kullanmak zorunda kalabilirsiniz `-i` parametre ve özel anahtar yolu. Örneğin, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

2. SSH kullanarak kümeye bağlanın:

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    Daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

3. Mapper.py ve reducer.py doğru satır sonları sahip emin olmak için aşağıdaki komutları kullanın:

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. MapReduce işi başlatmak için aşağıdaki komutu kullanın.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    Bu komut, aşağıdaki bölümleri içerir:

   * **hadoop streaming.jar**: akış MapReduce işlemleri yaparken kullanılır. Bu Hadoop MapReduce harici kod sağladığınız arabirimleri.

   * **-dosyaları**: MapReduce işi belirtilen dosyaları ekler.

   * **-Eşleyici**: Hadoop Eşleyici kullanmak için hangi dosya söyler.

   * **-reducer**: Hadoop reducer kullanmak için hangi dosya söyler.

   * **-Giriş**: gelen güvenmemeniz gerekir giriş dosyası sözcükleri.

   * **-Çıktı**: çıktı yazılır dizin.

    MapReduce işi çalıştığından, işlem yüzde olarak görüntülenir.

        02/15/05 19:01:04 bilgisi mapreduce. İş: eşleme %0 azaltmak %0 02/15/05 19:01:16 bilgisi mapreduce. İş: % 0 harita % 100 azaltmak 02/15/05 19:01:27 bilgisi mapreduce. İş: eşleme % %100 100 azaltın.


5. Çıktı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Bu komut oluştu sözcükleri ve word kaç kez listesini görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight ile akış MapRedcue işi kullanmayı öğrendiniz, Azure Hdınsight ile çalışmak için diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [HDInsight ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight ile Pig kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)
