---
title: -Azure HDInsight kümesinde SSH ile Apache Pig kullanma
description: Bilgi nasıl bir Apache Hadoop, Linux tabanlı küme ile SSH ve Pig Latin açıklamaları etkileşimli olarak çalışacak şekilde Pig komutunu kullanın veya toplu iş olarak bağlanın.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 503449e36b2b11e5663449dd732fdaf785417570
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62110761"
---
# <a name="run-apache-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Pig komut (SSH) ile bir Linux tabanlı kümesinde Apache Pig işleri çalıştırma

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Apache Pig işleri SSH bağlantısından HDInsight kümenize etkileşimli olarak çalıştırmayı öğrenin. Pig Latin'i programlama dili, istenen çıkış üretmek üzere giriş verilerini için uygulanan dönüşümler açıklamak sağlar.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar, Linux tabanlı HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>SSH ile bağlanma

HDInsight kümenize bağlanmak için SSH kullanın. Aşağıdaki örnekte adlı bir kümeye bağlanır **myhdinsight** adlı hesap olarak **sshuser**:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Apache Pig komutunu kullanın

1. Bağlandıktan sonra aşağıdaki komutu kullanarak Pig komut satırı arabirimi (CLI) başlatın:

    ```bash
    pig
    ```

    Kısa bir süre sonra istem değişikliklerini`grunt>`.

2. Aşağıdaki ifadeyi girin:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    ```

    Bu komut, oturum AÇTIĞI sample.log dosyasına içeriğini yükler. Dosyanın içeriğini aşağıdaki deyimi kullanarak görüntüleyebilirsiniz:

    ```piglatin
    DUMP LOGS;
    ```

3. Ardından, verileri aşağıdaki deyimi kullanarak her bir kayıttan yalnızca günlüğe kaydetme düzeyini ayıklamak için normal bir ifade uygulayarak dönüştürün:

    ```piglatin
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    ```

    Kullanabileceğiniz **dökümü** dönüştürme işleminin ardından verileri görüntülemek için. Bu durumda, `DUMP LEVELS;`.

4. Aşağıdaki tabloda ifadeleri kullanarak dönüşümleri uyguladıktan devam edin:

    | Pig Latin'i deyimi | Deyim yapar |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Günlük düzeyi için bir null değer içeren satırları kaldırır ve sonuçları içine depolar `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Günlük düzeyi tarafından satırları gruplandırır ve sonuçlara depolar `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Bir küme oluşturur her bir benzersiz günlüğe içeren veri düzeyi değeri ve kaç kez gerçekleşir. Veri kümesi içinde depolanan `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Günlük düzeyleri (Azalan) sayısına göre sıralar ve içine depolar `RESULT`. |

    > [!TIP]  
    > Kullanım `DUMP` her adımdan sonra dönüşümünün sonucu görüntülemek için.

5. Kullanarak bir dönüştürme sonuçlarını da kaydedebilirsiniz `STORE` deyimi. Örneğin, aşağıdaki deyim kaydeder `RESULT` için `/example/data/pigout` kümeniz için varsayılan depolama dizinine:

    ```piglatin
    STORE RESULT into '/example/data/pigout';
    ```

   > [!NOTE]  
   > Veriler, dosya adında belirtilen dizinde depolanır `part-nnnnn`. Dizin zaten varsa, bir hata alırsınız.

6. Grunt istemi çıkmak için aşağıdaki ifadeyi girin:

    ```piglatin
    QUIT;
    ```

### <a name="pig-latin-batch-files"></a>Pig Latin'i toplu iş dosyaları

Pig komutu, Pig Latin bir dosyada yer alan çalıştırmak için de kullanabilirsiniz.

1. Grunt istemi çıktıktan sonra adlı dosyası oluşturmak için aşağıdaki komutu kullanın `pigbatch.pig`:

    ```bash
    nano ~/pigbatch.pig
    ```

2. Aşağıdaki satırları yapıştırın veya yazın:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;
    ```

    İşiniz bittiğinde kullanın __Ctrl__ + __X__, __Y__, ardından __Enter__ dosyayı kaydetmek için.

3. Çalıştırmak için aşağıdaki komutu kullanın `pigbatch.pig` Pig komutu kullanarak dosya.

    ```bash
    pig ~/pigbatch.pig
    ```

    Batch işi tamamlandıktan sonra aşağıdaki çıktıyı görürsünüz:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Sonraki adımlar

Aşağıdaki belge, HDInsight Pig hakkında genel bilgi için bkz:

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)

HDInsight üzerinde Hadoop ile çalışmak için diğer yollar hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
