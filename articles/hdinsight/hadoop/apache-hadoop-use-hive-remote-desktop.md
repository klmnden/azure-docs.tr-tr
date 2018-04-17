---
title: Hdınsight - Azure Hadoop Hive ve Uzak Masaüstü'nü kullanma | Microsoft Docs
description: Uzak Masaüstü'nü kullanarak, hdınsight'ta Hadoop kümesine bağlanma hakkında bilgi edinin ve Hive sorguları Hive komut satırı arabirimi kullanarak çalıştırın.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 70eab088ce87d8a62d3f258b70aaec5e2e147d0e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Uzak Masaüstü kullanarak hdınsight'ta Hadoop ile Hive kullanma
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Bu makalede, Uzak Masaüstü'nü kullanarak bir Hdınsight kümesine bağlanma öğrenin ve Hive sorguları Hive komut satırı arabirimi (CLI) kullanarak çalıştırın.

> [!IMPORTANT]
> Uzak Masaüstü, Windows işletim sistemi olarak kullanma Hdınsight kümelerinde yalnızca kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Hdınsight 3.4 veya büyük, bkz [Hdınsight ve Beeline ile Hive kullanma](apache-hadoop-use-hive-beeline.md) Hive sorguları doğrudan kümede bir komut satırından çalıştırma hakkında bilgi.

## <a id="prereq"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir:

* Bir Windows tabanlı Hdınsight (Hadoop hdınsight) kümesi
* Windows 10, Windows 8 veya Windows 7 çalıştıran bir istemci bilgisayar

## <a id="connect"></a>İle Uzak Masaüstü Bağlantısı
Hdınsight kümesi için Uzak Masaüstü'nü etkinleştirin ve ardından yönergeleri izleyerek bağlanmak [RDP kullanarak Hdınsight kümelerini Bağlan](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Hive komutunu kullanın
Hdınsight kümesi için Masaüstü bağlı olduğunda Hive ile çalışmak için aşağıdaki adımları kullanın:

1. Hdınsight masaüstünden başlatmak **Hadoop komut satırı**.
2. Hive CLI başlatmak için aşağıdaki komutu girin:

        %hive_home%\bin\hive

    CLI başladığında, Hive CLI istemi görürsünüz: `hive>`.
3. CLI kullanarak girin adlı yeni bir tablo oluşturmak için aşağıdaki ifadeleri **log4jLogs** örnek verileri kullanarak:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * **DROP TABLE**: Tablo zaten varsa, tablo ve veri dosyasını siler.
   * **Dış tablo oluştur**: kovanında yeni bir 'external' tablo oluşturur. Dış tablolara yalnızca tablo tanımı (verileri özgün konumda bırakılır) Hive depolayın.

     > [!NOTE]
     > Bir dış kaynağa (örneğin, bir otomatik veri karşıya yükleme işlemi) veya başka bir MapReduce işlemi tarafından güncelleştirilecek temel alınan veri beklediğiniz dış tablolara kullanılması gereken, ancak her zaman en son verileri kullanmak üzere Hive sorguları istiyor.
     >
     > Bir dış tablo bırakma mu **değil** verileri, yalnızca tablo tanımını silin.
     >
     >
   * **SATIR biçimi**: verileri nasıl biçimlendirildiğini Hive söyler. Bu durumda, her günlüğün içinde alanlar boşlukla ayrılır.
   * **AS TEXTFILE konumu DEPOLANAN**: (örneğin/veri dizini) depolanan verileri nerede Hive bildirir ve metin olarak depolanır.
   * **SEÇİN**: tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]**. Bu değeri döndürmelidir **3** çünkü bu değer içeren üç satır vardır.
   * **INPUT__FILE__NAME gibi '%.log'** -biz yalnızca veri biten dosyalarından döndürmelidir Hive söyler. günlük. Bu arama verileri içeren ve verileri diğer örnekten tanımladığımız şema eşleşmiyor veri dosyalarını döndürmesini tutar sample.log dosyasına kısıtlar.
4. Adlı yeni bir 'iç' tablo oluşturmak için aşağıdaki deyimleri kullanın **günlüklerini**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * **Tablo IF değil var oluşturmak**: zaten yoksa, bir tablo oluşturur. Çünkü **dış** anahtar sözcüğü kullanılmaz, Hive veri ambarında depolanır ve Hive tamamen yönetilen bir iç tablosu budur.

     > [!NOTE]
     > Farklı **dış** tablolar, bir iç tablosu da bırakarak temel alınan verileri siler.
     >
     >
   * **AS ORC DEPOLANAN**: en iyi duruma getirilmiş satır sütun (ORC) biçiminde verileri depolar. Kovan verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimde budur.
   * **INSERT ÜZERİNE... SEÇİN**: satırları seçer **log4jLogs** içeren tablo **[Hata]**, verileri ekler **günlüklerini** tablo.

     Yalnızca satırları içeren doğrulamak için **[Hata]** sütununda t4 saklı **günlüklerini** tablo, tüm satırların döndürmek için şu deyimi kullanın **günlüklerini**:

       SEÇİN * günlüklerini; gelen

     Üç veri satırı döndürülmesi, tüm içeren **[Hata]** sütun t4 içinde.

## <a id="summary"></a>Özet
Gördüğünüz gibi Hive komutu etkileşimli olarak bir Hdınsight kümesine Hive sorgularını çalıştırma, iş durumunu izleyebilir ve çıkış almak için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar
Hdınsight'ta Hive hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Tez Hive ile kullanıyorsanız, hata ayıklama bilgileri için aşağıdaki belgelere bakın:

* [Windows tabanlı Hdınsight üzerinde Tez kullanıcı arabirimini kullanma](../hdinsight-debug-tez-ui.md)
* [Linux tabanlı Hdınsight üzerinde Ambari Tez görünümünü kullanın](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
