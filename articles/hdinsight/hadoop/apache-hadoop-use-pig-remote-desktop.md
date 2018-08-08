---
title: HDInsight - Azure Uzak Masaüstü ile Hadoop Pig kullanma
description: Bir Uzak Masaüstü Bağlantısı'ndan bir Windows tabanlı bir Hadoop kümesinde HDInsight Pig Latin açıklamaları çalıştırmak için Pig komut kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 5aec07a5ebbbb56abcbaebbddc5579cf4d076b4d
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590675"
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Bir Uzak Masaüstü Bağlantısı'ndan pig işleri çalıştırma
[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Bu belge, bir Uzak Masaüstü Bağlantısı'ndan bir Windows tabanlı HDInsight kümesi için Pig Latin açıklamaları çalıştırılacak Pig komutunu kullanarak bir kılavuz sağlar. Pig Latin'i veri dönüşümleri açıklayarak MapReduce uygulamaları oluşturmak, İşlevler azaltmak ve yerine eşleme sağlar.

> [!IMPORTANT]
> Uzak Masaüstü, yalnızca Windows işletim sistemi olarak kullandığınız HDInsight kümelerinde kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4 veya büyük bkz [HDInsight ve SSH ile Pig kullanma](apache-hadoop-use-pig-ssh.md) Pig işleri doğrudan küme üzerinde bir komut satırından çalıştırma etkileşimli olarak hakkında bilgi.

## <a id="prereq"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir.

* Bir Windows tabanlı HDInsight (Hadoop HDInsight üzerinde) kümesi
* Windows 10, Windows 8 veya Windows 7 çalıştıran bir istemci bilgisayar

## <a id="connect"></a>Uzak Masaüstü ile bağlanın
HDInsight kümesi için Uzak masaüstünü etkinleştirme, sonra yönergeleri izleyerek bağlanmamız [RDP kullanarak HDInsight kümelerini Bağlan](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Pig komutunu kullanın
1. Bir Uzak Masaüstü bağlantı kurduktan sonra Başlat **Hadoop komut satırı** masaüstünde simgesini kullanarak.
2. Pig komutu başlatmak için aşağıdakileri kullanın:

        %pig_home%\bin\pig

    İle sunulacak bir `grunt>` istemi.
3. Aşağıdaki ifadeyi girin:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Bu komut, GÜNLÜKLERİ dosyasına sample.log dosyasına içeriğini yükler. Aşağıdaki komutu kullanarak dosya içeriğini görüntüleyebilirsiniz:

        DUMP LOGS;
4. Her bir kayıttan yalnızca günlüğe kaydetme düzeyini ayıklamak için normal bir ifade uygulayarak verileri dönüştürün:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Kullanabileceğiniz **dökümü** dönüştürme işleminin ardından verileri görüntülemek için. Bu durumda, `DUMP LEVELS;`.
5. Aşağıdaki deyimleri kullanarak dönüşümleri uyguladıktan devam edin. Kullanım `DUMP` her adımdan sonra dönüşümünün sonucu görüntülemek için.

    <table>
    <tr>
    <th>Deyim</th><th>Ne yapar?</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = LOGLEVEL FİLTRE DÜZEYLERİNİN null; değil</td><td>Günlük düzeyi için bir null değer içeren satırları kaldırır ve sonuçları FILTEREDLEVELS depolar.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS LOGLEVEL tarafından; Grup FILTEREDLEVELS =</td><td>Günlük düzeyi tarafından satırları gruplandırır ve sonuçları GROUPEDLEVELS depolar.</td>
    </tr>
    <tr>
    <td>Sıklık foreach = GROUPEDLEVELS LOGLEVEL sayısı (FILTEREDLEVELS Grup Oluştur. Sayım olarak LOGLEVEL);</td><td>Yeni bir küme oluşturur her bir benzersiz günlüğe içeren veri düzeyi değeri ve kaç kez gerçekleşir. Bu FREKANSLARI depolanır.</td>
    </tr>
    <tr>
    <td>Sonuç sipariş sayısı desc tarafından; sıklık =</td><td>Günlük düzeyleri (Azalan) sayısına göre sıralar ve sonucu depolar</td>
    </tr>
</table>

6. Kullanarak bir dönüştürme sonuçlarını da kaydedebilirsiniz `STORE` deyimi. Örneğin, aşağıdaki kaydeder komut `RESULT` için **/example/data/pigout** kümeniz için varsayılan depolama kapsayıcısındaki dizin:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > Veriler, dosya adında belirtilen dizinde depolanır **bölümü nnnnn**. Dizin zaten varsa, bir hata iletisi alırsınız.
   >
   >
   
7. Grunt istemi çıkmak için aşağıdaki deyimi girin.

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin'i toplu iş dosyaları
Pig komutu, bir dosyada yer alan bir Pig Latin çalıştırmak için de kullanabilirsiniz.

1. Grunt istemi çıktıktan sonra açmak **not defteri** adlı yeni bir dosya oluşturun **pigbatch.pig** içinde **PIG_HOME %** dizin.
2. Aşağıdaki satırları içine yapıştırın veya yazın **pigbatch.pig** dosya ve kaydedin:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Aşağıdakileri çalıştırmak için kullandığınız **pigbatch.pig** pig komutunu kullanarak dosya.

        pig %PIG_HOME%\pigbatch.pig

    Batch işi tamamlandığında, kendisini kullanıldığında, aynı olmalıdır aşağıdaki bir çıktı görmeniz gerekir `DUMP RESULT;` , önceki adımlarda:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Özet
Gördüğünüz gibi Pig komut etkileşimli olarak MapReduce işlemlerini çalıştırmak veya bir toplu iş dosyasında depolanır, Pig Latin işleri çalıştırmanızı sağlar.

## <a id="nextsteps"></a>Sonraki adımlar
HDInsight, Pig hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
