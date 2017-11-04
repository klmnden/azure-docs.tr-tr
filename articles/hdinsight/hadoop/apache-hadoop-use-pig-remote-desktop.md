---
title: "Hadoop Pig hdınsight'ta - Azure Uzak Masaüstü ile kullanma | Microsoft Docs"
description: "Hdınsight'ta bir Windows tabanlı Hadoop kümesine bir Uzak Masaüstü bağlantısı üzerinden Pig Latin deyimleri çalıştırmak için Pig komutu kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e034a286-de0f-465f-8bf1-3d085ca6abed
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 8b5e8e7f400a4494549c997e969a46ca90eb0ba5
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Uzak Masaüstü bağlantısı üzerinden pig işleri çalıştırma
[!INCLUDE [pig-selector](../../../includes/hdinsight-selector-use-pig.md)]

Bu belge, Pig Latin deyimleri Windows tabanlı Hdınsight kümesi için bir Uzak Masaüstü bağlantısı üzerinden çalıştırmak için Pig komutunu kullanarak bir kılavuz sağlar. Pig Latin veri dönüşümleri açıklayarak MapReduce uygulamalar oluşturmak yerine eşleme ve İşlevler azaltmak sağlar.

> [!IMPORTANT]
> Uzak Masaüstü, Windows işletim sistemi olarak kullanma Hdınsight kümelerinde yalnızca kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Hdınsight 3.4 veya büyük, bkz [Hdınsight ve SSH ile Pig kullanma](apache-hadoop-use-pig-ssh.md) Pig işleri doğrudan kümede bir komut satırından çalıştırma etkileşimli olarak hakkında bilgi.

## <a id="prereq"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir.

* Bir Windows tabanlı Hdınsight (Hadoop hdınsight) kümesi
* Windows 10, Windows 8 veya Windows 7 çalıştıran bir istemci bilgisayar

## <a id="connect"></a>İle Uzak Masaüstü Bağlantısı
Hdınsight kümesi için Uzak Masaüstü'nü etkinleştirin ve ardından yönergeleri izleyerek bağlanmak [RDP kullanarak Hdınsight kümelerini Bağlan](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Pig komutunu kullanın
1. Uzak Masaüstü bağlantı kurduktan sonra Başlangıç **Hadoop komut satırı** masaüstünde simgesini kullanarak.
2. Pig komutu başlatmak için aşağıdakileri kullanın:

        %pig_home%\bin\pig

    İle sunulur bir `grunt>` istemi.
3. Aşağıdaki deyimi girin:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Bu komut sample.log dosyasının içeriğini GÜNLÜKLERİ dosyasına yükler. Aşağıdaki komutu kullanarak dosyanın içeriğini görüntüleyebilirsiniz:

        DUMP LOGS;
4. Verileri yalnızca günlüğe kaydetme düzeyi her kayıttan ayıklamak için normal bir ifade uygulayarak dönüştürün:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Kullanabileceğiniz **dökümü** dönüştürme işleminin ardından verileri görüntülemek için. Bu durumda, `DUMP LEVELS;`.
5. Aşağıdaki deyim kullanarak dönüşümleri uygulama devam edin. Kullanım `DUMP` her adımından sonra dönüşümünün sonucu görüntülemek için.

    <table>
    <tr>
    <th>Deyimi</th><th>Neler yapar?</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = LOGLEVEL FİLTRE DÜZEYLERİYLE null; değil</td><td>Günlük düzeyini null değerini içeren satırları kaldırır ve sonuçları FILTEREDLEVELS depolar.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS Grup FILTEREDLEVELS LOGLEVEL tarafından; =</td><td>Günlük düzeyi satır grupları ve sonuçları GROUPEDLEVELS depolar.</td>
    </tr>
    <tr>
    <td>Sıklık foreach = GROUPEDLEVELS sayısı (FILTEREDLEVELS LOGLEVEL Grup Oluştur. LOGLEVEL) sayısı gibi;</td><td>Yeni bir küme oluşturur her benzersiz günlük içeren verilerin düzeyi değeri ve kaç kez oluşur. Bu SIKLIKLARINI depolanır</td>
    </tr>
    <tr>
    <td>Sonuç order SIKLIĞI sayısı desc tarafından; =</td><td>Günlük düzeyleri (Azalan) sayısına göre sıralar ve sonucu depolar</td>
    </tr>
    </table>
6.Kullanarak bir dönüşüm sonuçları kaydedebilirsiniz `STORE` deyimi. Örneğin, aşağıdaki kaydeder komut `RESULT` için **/example/data/pigout** kümeniz için varsayılan depolama kapsayıcısında dizin:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > Veri adlı dosyaları belirtilen dizinde depolanır **bölümü nnnnn**. Dizini zaten varsa, bir hata iletisi alırsınız.
   >
   >
7. Grunt istemi çıkmak için şu deyimi girin.

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin toplu iş dosyaları
Pig komutu, bir dosyada yer alan Pig Latin çalıştırmak için de kullanabilirsiniz.

1. Grunt istemi çıktıktan sonra açmak **not defteri** ve adlı yeni bir dosya oluşturun **pigbatch.pig** içinde **PIG_HOME %** dizin.
2. Yazın veya yapıştırın içine aşağıdaki satırları **pigbatch.pig** dosya ve dosyayı kaydedin:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Çalıştırmak için aşağıdakileri kullanın **pigbatch.pig** pig komutunu kullanarak dosya.

        pig %PIG_HOME%\pigbatch.pig

    Toplu işlem tamamlandığında kullanıldığında, aynı olmalıdır aşağıdaki çıktı görmeniz gerekir `DUMP RESULT;` , önceki adımlarda:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Özet
Gördüğünüz gibi Pig komutu etkileşimli olarak MapReduce işlemleri çalıştırın veya bir toplu iş dosyasında depolanan Pig Latin işleri çalıştırma olanak sağlar.

## <a id="nextsteps"></a>Sonraki adımlar
Hdınsight'ta Pig hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
