---
title: MapReduce ve Uzak Masaüstü ile HDInsight - Azure Hadoop
description: HDInsight üzerinde Hadoop bağlanmak ve MapReduce işlerini çalıştırmak için Uzak Masaüstü'ı kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: cf791fbada590109a485394964b9d99bdd1f9a3d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599240"
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Uzak Masaüstü kullanarak HDInsight üzerinde Hadoop MapReduce kullanma
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Bu makalede, HDInsight kümesinde bir Hadoop için Uzak Masaüstü'nü kullanarak bağlanın ve ardından MapReduce işleri Hadoop komutunu kullanarak öğreneceksiniz.

> [!IMPORTANT]
> Uzak Masaüstü, yalnızca Windows tabanlı HDInsight kümelerinde kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4 veya büyük bkz [SSH ile MapReduce kullanma](apache-hadoop-use-mapreduce-ssh.md) HDInsight kümesine bağlanma ve MapReduce işleri çalıştırma hakkında bilgi.

## <a id="prereq"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir:

* Bir Windows tabanlı HDInsight (Hadoop HDInsight üzerinde) kümesi
* Windows 10, Windows 8 veya Windows 7 çalıştıran bir istemci bilgisayar

## <a id="connect"></a>Uzak Masaüstü ile bağlanın
HDInsight kümesi için Uzak masaüstünü etkinleştirme, sonra yönergeleri izleyerek bağlanmamız [RDP kullanarak HDInsight kümelerini Bağlan](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Hadoop komutunu kullanın
HDInsight kümesi için Masaüstü bağlandığınızda Hadoop komutunu kullanarak bir MapReduce işi çalıştırmak için aşağıdaki adımları kullanın:

1. HDInsight masaüstünden başlatmak **Hadoop komut satırı**. Bu yeni bir komut istemi'nde açılır **c:\apps\dist\hadoop-&lt;sürüm numarası >** dizin.

   > [!NOTE]
   > Hadoop güncelleştirildikçe sürüm numarasını değiştirir. **HADOOP_HOME** ortam değişkeni yolunu bulmak için kullanılabilir. Örneğin, `cd %HADOOP_HOME%` sürüm numarasını bilmeniz gerek kalmadan Hadoop dizinine dizinleri değiştirir.
   >
   >
2. Kullanılacak **Hadoop** komut bir örnek MapReduce işi çalıştırmak için aşağıdaki komutu kullanın:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Bu başlatır **wordcount** bulunan sınıfı **hadoop mapreduce examples.jar** geçerli dizin dosyası. Giriş olarak kullandığı **wasb://example/data/gutenberg/davinci.txt** belge ve çıkış konumunda depolanır: **wasb: / / / örnek/data/WordCountOutput**.

   > [!NOTE]
   > Bu bir MapReduce işi ve örnek veriler hakkında daha fazla bilgi için bkz. <a href="hdinsight-use-mapreduce.md">, HDInsight Hadoop MapReduce kullanma</a>.
   >
   >
3. İş, işlenir ve iş tamamlandığında bilgi aşağıdakine benzer döndürür gibi ayrıntıları gösterir:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. İş tamamlandığında, çıkış dosyalarının depolandığı listelemek için aşağıdaki komutu kullanın **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Bu iki dosya görüntülenmelidir **_SUCCESS** ve **bölümü r 00000**. **Bölümü r 00000** bu iş için çıktı dosyası içerir.

   > [!NOTE]
   > Bazı MapReduce işleri sonuçları arasında birden fazla bölme **bölümü r ###** dosyaları. Bu durumda, kullanın ### dosyaların sırasını göstermek için soneki.
   >
   >
5. Çıkışı görüntülemek için aşağıdaki komutu kullanın:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Bu bulunan bir kelimelerin listesini görüntüler **wasb://example/data/gutenberg/davinci.txt** oluştu her bir sözcüğün kaç kez dosyası. Dosyada bulunan verilerin bir örnek verilmiştir:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Özet
Gördüğünüz gibi Hadoop komut bir HDInsight kümesinde MapReduce işleri çalıştırma ve ardından iş çıktısını görüntülemek için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar
HDInsight MapReduce işleri hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)
