---
title: Hadoop Pig, Hdınsight kümesi üzerinde - Azure ile SSH kullanma | Microsoft Docs
description: Bilgi nasıl SSH ve ardından Pig Latin deyimleri etkileşimli olarak çalıştırmak için Pig komutunu kullanın veya toplu iş olarak Linux tabanlı Hadoop kümeye bağlanın.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: c296e01096480b85aea52ace69f25aff39e3bd2d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Pig komutu (SSH) ile Linux tabanlı kümesi pig işleri çalıştırma

[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Pig işleri bir SSH bağlantısı Hdınsight kümenize etkileşimli olarak çalışmak üzere öğrenin. Pig Latin programlama dili, istenen çıkış üretmek için giriş verileri uygulanan Dönüşümlerin açıklamak sağlar.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux tabanlı Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>SSH ile bağlanma

SSH Hdınsight kümenize bağlanmak için kullanın. Aşağıdaki örnek adlı bir kümeye bağlanır **myhdinsight** adlı hesap olarak **sshuser**:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Pig komutunu kullanın

1. Bağlantı kurulduktan sonra Pig komut satırı arabirimi (CLI) aşağıdaki komutu kullanarak başlatın:

    ```bash
    pig
    ```

    Bir süre sonra istemi değişikliklerini`grunt>`.

2. Aşağıdaki deyimi girin:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    ```

    Bu komut sample.log dosyasının içeriğini oturum AÇTIĞI yükler. Şu deyimi kullanarak dosyanın içeriğini görüntüleyebilirsiniz:

    ```piglatin
    DUMP LOGS;
    ```

3. Ardından, verileri şu deyimi kullanarak her kayıttan yalnızca günlüğe kaydetme düzeyi ayıklamak için normal bir ifade uygulayarak dönüştürün:

    ```piglatin
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    ```

    Kullanabileceğiniz **dökümü** dönüştürme işleminin ardından verileri görüntülemek için. Bu durumda, kullanarak `DUMP LEVELS;`.

4. Aşağıdaki tabloda deyimleri kullanarak dönüşümleri uygulama devam edin:

    | Pig Latin deyimi | Deyim yaptığı |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Günlük düzeyini null değerini içeren satırları kaldırır ve sonuçları içine depolar `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Günlük düzeyi tarafından satırları gruplandırır ve sonuçları içine depolar `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Bir küme oluşturur her benzersiz günlük içeren verilerin düzeyi değeri ve kaç kez oluşur. Veri kümesi içinde depolanan `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Günlük düzeyleri (Azalan) sayısına göre sıralar ve içine depolar `RESULT`. |

    > [!TIP]
    > Kullanım `DUMP` her adımından sonra dönüşümünün sonucu görüntülemek için.

5. Kullanarak bir dönüşüm sonuçları kaydedebilirsiniz `STORE` deyimi. Örneğin aşağıdaki deyim kaydeder `RESULT` için `/example/data/pigout` kümeniz için varsayılan depolama dizinine:

    ```piglatin
    STORE RESULT into '/example/data/pigout';
    ```

   > [!NOTE]
   > Veri adlı dosyaları belirtilen dizinde depolanır `part-nnnnn`. Dizini zaten varsa, bir hata alırsınız.

6. Grunt istemi çıkmak için şu deyimi girin:

    ```piglatin
    QUIT;
    ```

### <a name="pig-latin-batch-files"></a>Pig Latin toplu iş dosyaları

Pig komutu, bir dosyada yer alan Pig Latin çalıştırmak için de kullanabilirsiniz.

1. Grunt istemi çıktıktan sonra adlı dosyayı oluşturmak için aşağıdaki komutu kullanın `pigbatch.pig`:

    ```bash
    nano ~/pigbatch.pig
    ```

2. Yazın veya aşağıdaki satırları yapıştırın:

    ```piglatin
    LOGS = LOAD '/example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;
    ```

    Bunu yaptıktan sonra kullanmak __Ctrl__ + __X__, __Y__ve ardından __Enter__ dosyayı kaydetmek için.

3. Çalıştırmak için aşağıdaki komutu kullanın `pigbatch.pig` Pig komutunu kullanarak dosya.

    ```bash
    pig ~/pigbatch.pig
    ```

    Toplu işi tamamlandıktan sonra aşağıdaki çıkış bakın:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight'ta Pig hakkında genel bilgi için aşağıdaki belgesine bakın:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)

Hdınsight'ta Hadoop ile çalışmak için diğer yöntemler hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
