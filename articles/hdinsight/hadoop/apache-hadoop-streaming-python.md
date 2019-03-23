---
title: Python akış HDInsight - Azure ile MapReduce işlerini geliştirme
description: Python MapReduce işleri akışında kullanmayı öğrenin. Bir akış API'sini, Apache Hadoop MapReduce için Java dışındaki dillerde yazmak için sağlar.
services: hdinsight
keyword: mapreduce python,python map reduce,python mapreduce
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: hrasheed
ms.openlocfilehash: 95a78fafcd83e6f56f26ef6670beaa179085de62
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58359589"
---
# <a name="develop-python-streaming-mapreduce-programs-for-hdinsight"></a>Python için HDInsight akış MapReduce programları geliştirme

Python MapReduce operations akışında kullanmayı öğrenin. Apache Hadoop, harita yazma ve Java dışındaki dillerde işlevleri azaltmak sağlayan bir MapReduce için bir akış API'sini sağlar. Bu belgedeki adımlarda, harita uygulamak ve python'da altına düşürün.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* Bir HDInsight kümesinde Linux tabanlı Apache Hadoop

  > [!IMPORTANT]
  > Bu belgedeki adımlar, Linux kullanan bir HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir metin düzenleyicisi

  > [!IMPORTANT]
  > Metin düzenleyici, satır sonu olarak LF kullanmanız gerekir. CRLF satır sonu kullanarak Linux tabanlı HDInsight kümelerinde MapReduce işi çalıştırılırken hataya neden.

* `ssh` Ve `scp` komutları veya [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)

## <a name="word-count"></a>Sözcük Sayımı

Bu örnek, bir temel bir sözcük sayımı bir python Eşleyici ve azaltıcı uygulanan içindir. Eşleyici cümleleri tek tek sözcüklere böler ve azaltıcı sözcükleri toplar ve çıkış üretmesi sayar.

Aşağıdaki akış çizelgesi ne eşleme sırasında gerçekleşir ve aşamaları azaltmak gösterilmektedir.

![mapreduce işleminin çizimi](./media/apache-hadoop-streaming-python/HDI.WordCountDiagram.png)

## <a name="streaming-mapreduce"></a>Akış MapReduce

Hadoop, eşleme içeren bir dosya belirtin ve bir iş tarafından kullanılan mantıksal azaltılmasına olanak tanır. Eşleme için belirli gereksinimler ve mantıksal azaltmak şunlardır:

* **Giriş**: Eşleme ve azaltma bileşenleri STDIN giriş verilerinin okuma gerekir.
* **Çıkış**: Eşleme ve azaltma bileşenleri STDOUT için çıktı verilerini yazmalıdır.
* **Veri biçimi**: Tüketilen ve üretilen veriler, bir sekme karakteri tarafından ayrılmış bir anahtar/değer çifti olmalıdır.

Python kolayca işleyebilir bu gereksinimleri kullanarak `sys` STDIN ve kullanarak Modülü `print` STDOUT yazdırmak için. Kalan görev yalnızca bir sekme ile verileri biçimlendirme (`\t`) karakteri anahtar ve değer arasında.

## <a name="create-the-mapper-and-reducer"></a>Eşleyici ve azaltıcı oluşturma

1. Adlı bir dosya oluşturun `mapper.py` ve içeriği olarak aşağıdaki kodu kullanın:

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

2. Adlı bir dosya oluşturun **reducer.py** ve içeriği olarak aşağıdaki kodu kullanın:

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

Dosyalarınızın doğru satır sonlarını olmasını sağlamak için aşağıdaki PowerShell betiğini kullanın:

[!code-powershell[main](../../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=138-140)]

Dosyaları karşıya yükleme, işini çalıştırın ve çıktıyı görüntülemek için aşağıdaki PowerShell betiğini kullanın:

[!code-powershell[main](../../../powershell_scripts/hdinsight/streaming-python/streaming-python.ps1?range=5-134)]

## <a name="run-from-an-ssh-session"></a>Bir SSH oturumundan çalıştırın

1. Geliştirme ortamınızdan aynı dizinde `mapper.py` ve `reducer.py` dosyaları, aşağıdaki komutu kullanın:

    ```bash
    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:
    ```

    Değiştirin `username` , kümenizin SSH kullanıcı adı ve `clustername` değerini kümenizin adıyla.

    Bu komut dosyaları, yerel sistemden baş düğüme kopyalar.

    > [!NOTE]  
    > SSH hesabınızın güvenliğini sağlamak için parola kullandıysanız, parola istenir. Bir SSH anahtarı kullandıysanız, kullanmak zorunda kalabilirsiniz `-i` parametresi ve özel anahtarın yolunu. Örneğin, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

2. SSH kullanarak kümeye bağlanın:

    ```bash
    ssh username@clustername-ssh.azurehdinsight.net`
    ```

    Hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

3. Doğru satır sonlarını mapper.py ve reducer.py sahip olmak için aşağıdaki komutları kullanın:

    ```bash
    perl -pi -e 's/\r\n/\n/g' mapper.py
    perl -pi -e 's/\r\n/\n/g' reducer.py
    ```

4. MapReduce işi başlatmak için aşağıdaki komutu kullanın.

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
    ```

    Bu komut, aşağıdaki bölümleri içerir:

   * **hadoop streaming.jar**: Akış MapReduce işlemleri gerçekleştirirken kullanılır. Bu Hadoop MapReduce dış kod sağladığınız arabirimleri.

   * **-dosyaları**: Belirtilen dosyalar, MapReduce işine ekler.

   * **-Eşleyici**: Hadoop Eşleyici kullanmak için hangi dosya söyler.

   * **-Azaltıcı**: Hadoop Azaltıcı kullanmak için hangi dosya söyler.

   * **-Giriş**: Biz sözcük sayısı giriş dosyası.

   * **-Çıkış**: Çıkış yazılan dizin.

     MapReduce işi çalıştığı işlemin yüzde olarak görüntülenir.

        02/15/05 19:01:04 bilgisi mapreduce. İş: eşleme %0 azaltmak %0 02/15/05 19:01:16 bilgi mapreduce. İş: % 0 harita %100 azaltmak 02/15/05 19:01:27 bilgisi mapreduce. İş: eşleme %100 %100 azaltın.


5. Çıkışı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Bu komut oluştu sözcükler ve sözcüğün kaç kez listesini görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight ile akış MapRedcue işi kullanmayı öğrendiniz, Azure HDInsight ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [Apache Hive, HDInsight ile kullanma](hdinsight-use-hive.md)
* [Apache Pig, HDInsight ile kullanma](hdinsight-use-pig.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)
