---
title: "MapReduce ve hdınsight'ta - Azure Hadoop ile Uzak Masaüstü | Microsoft Docs"
description: "Hdınsight'ta Hadoop bağlanmak ve MapReduce işleri çalıştırmak için Uzak Masaüstü kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9d3a7b34-7def-4c2e-bb6c-52682d30dee8
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: e0dd2c0c0eeeedda00d73f697897582a48d0fc04
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Uzak Masaüstü kullanarak hdınsight'ta Hadoop MapReduce kullanın
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Bu makalede, Hdınsight kümesinde bir Hadoop için Uzak Masaüstü kullanarak bağlanmak ve ardından MapReduce işleri Hadoop komutunu kullanarak öğreneceksiniz.

> [!IMPORTANT]
> Uzak Masaüstü yalnızca Windows tabanlı Hdınsight kümelerinde kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Hdınsight 3.4 veya büyük, bkz [SSH ile kullanım MapReduce](apache-hadoop-use-mapreduce-ssh.md) Hdınsight kümesine bağlanma ve MapReduce işleri çalıştırma hakkında bilgi.

## <a id="prereq"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir:

* Bir Windows tabanlı Hdınsight (Hadoop hdınsight) kümesi
* Windows 10, Windows 8 veya Windows 7 çalıştıran bir istemci bilgisayar

## <a id="connect"></a>İle Uzak Masaüstü Bağlantısı
Hdınsight kümesi için Uzak Masaüstü'nü etkinleştirin ve ardından yönergeleri izleyerek bağlanmak [RDP kullanarak Hdınsight kümelerini Bağlan](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Hadoop komutunu kullanın
Hdınsight kümesi için Masaüstü bağlandığında, Hadoop komutunu kullanarak bir MapReduce işi çalıştırmak için aşağıdaki adımları kullanın:

1. Hdınsight masaüstünden başlatmak **Hadoop komut satırı**. Bu yeni bir komut istemi'nde açar **c:\apps\dist\hadoop-&lt;sürüm numarası >** dizin.

   > [!NOTE]
   > Hadoop güncelleştirildikçe sürüm numarasını değiştirir. **HADOOP_HOME** ortam değişkeni yolunu bulmak için kullanılabilir. Örneğin, `cd %HADOOP_HOME%` sürüm numarasını bilmeniz gerek kalmadan Hadoop dizinine dizinleri değiştirir.
   >
   >
2. Kullanılacak **Hadoop** komutu bir örnek MapReduce işi çalıştırmak için aşağıdaki komutu kullanın:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Bu başlatır **wordcount** bulunan sınıfı **hadoop mapreduce examples.jar** dosyası geçerli dizinde. Giriş olarak kullandığı **wasb://example/data/gutenberg/davinci.txt** belge ve çıktı konumunda depolanır: **wasb: / / / örnek/data/WordCountOutput**.

   > [!NOTE]
   > Bu MapReduce işi ve örnek veriler hakkında daha fazla bilgi için bkz: <a href="hdinsight-use-mapreduce.md">Hdınsight Hadoop içinde kullanım MapReduce</a>.
   >
   >
3. İş ayrıntıları bu işlenir ve iş tamamlandıktan sonra bu bilgileri aşağıdakine benzer döndürür gösterir:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. İş tamamlandığında depolandığı çıktı dosyaları listelemek için aşağıdaki komutu kullanın **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Bu iki dosya görüntülenmelidir **_SUCCESS** ve **bölümü r 00000**. **Bölümü r 00000** dosyası bu iş için çıktıyı içerir.

   > [!NOTE]
   > Bazı MapReduce işleri sonuçları birden çok bölme **bölümü r ###** dosyaları. Bu durumda, kullanın ### dosyaların sırasını belirtmek için soneki.
   >
   >
5. Çıktı görüntülemek için aşağıdaki komutu kullanın:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Bu bulunan sözcüklerin listesini görüntüler **wasb://example/data/gutenberg/davinci.txt** dosyası her sözcüğün yapma sayısı. Dosyalarda bulunan verilerin bir örnek verilmiştir:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Özet
Gördüğünüz gibi Hadoop komutu bir Hdınsight kümesine MapReduce işleri çalıştırma ve iş çıktısı görüntülemek için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar
Hdınsight'ta MapReduce işleri hakkında genel bilgi için:

* [Hdınsight Hadoop MapReduce kullanın](hdinsight-use-mapreduce.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
