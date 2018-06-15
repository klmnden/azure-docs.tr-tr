---
title: Hdınsight'ta - Azure Hadoop ile MapReduce ve SSH bağlantısı | Microsoft Docs
description: MapReduce işleri Hdınsight'ta Hadoop kullanarak çalıştırmak için SSH kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlunb
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/10/2018
ms.author: larryfr
ms.openlocfilehash: 67e1bf6cee04eda51f5dbfc51a95614347fc2b7f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31399020"
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Hdınsight ile SSH Hadoop ile MapReduce kullanma

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Güvenli Kabuk (SSH) bağlantısı MapReduce işleri hdınsight'a gönderme öğrenin.

> [!NOTE]
> Linux tabanlı Hadoop sunucuları kullanarak bilginiz, ancak yeni Hdınsight için, bkz: [Linux tabanlı Hdınsight ipuçları](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Önkoşullar

* Linux tabanlı Hdınsight (Hadoop hdınsight) küme

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir SSH istemcisi. Daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>SSH ile bağlanma

SSH kullanarak kümeye bağlanın. Örneğin, aşağıdaki komutu adlı bir kümeye bağlanır **myhdinsight** olarak **sshuser** hesabı:

```bash
ssh sshuser@myhdinsight-ssh.azurehdinsight.net
```

**SSH kimlik doğrulaması için bir sertifika anahtarı kullanırsanız**, örneğin, istemci sisteminizde özel anahtarı konumunu belirtmeniz gerekebilir:

```bash
ssh -i ~/mykey.key sshuser@myhdinsight-ssh.azurehdinsight.net
```

**SSH kimlik doğrulaması için bir parola kullanıyorsanız**, istendiğinde parolayı sağlamanız gerekir.

Hdınsight ile SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Hadoop komutları kullanın

1. Hdınsight kümesine bağlandıktan sonra bir MapReduce işi başlatmak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Bu komut başlatır `wordcount` bulunan sınıfı `hadoop-mapreduce-examples.jar` dosya. Kullandığı `/example/data/gutenberg/davinci.txt` belge giriş ve çıkış olarak depolandı `/example/data/WordCountOutput`.

    > [!NOTE]
    > Bu MapReduce işi ve örnek veriler hakkında daha fazla bilgi için bkz: [hdınsight'ta Hadoop içinde kullanım MapReduce](hdinsight-use-mapreduce.md).

2. İş, işler ve iş tamamlandığında, bilgileri aşağıdaki metni benzer döndürür gibi ayrıntıları gösterir:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. İş tamamlandığında, çıkış dosyaları listelemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Bu komut, iki dosya görüntülemek `_SUCCESS` ve `part-r-00000`. `part-r-00000` Dosyası bu iş için çıktıyı içerir.

    > [!NOTE]
    > Bazı MapReduce işleri sonuçları birden çok bölme **bölümü r ###** dosyaları. Bu durumda, kullanın ### dosyaların sırasını belirtmek için soneki.

4. Çıktı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Bu komut bulunan sözcüklerin listesini görüntüler **wasb://example/data/gutenberg/davinci.txt** dosya ve her sözcüğün yapma sayısı. Aşağıdaki metin dosyasında bulunan verileri örneğidir:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Özet

Gördüğünüz gibi Hadoop komutları bir Hdınsight küme MapReduce işleri çalıştırma ve iş çıktısı görüntülemek için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight'ta MapReduce işleri hakkında genel bilgi için:

* [Hdınsight Hadoop MapReduce kullanın](hdinsight-use-mapreduce.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
