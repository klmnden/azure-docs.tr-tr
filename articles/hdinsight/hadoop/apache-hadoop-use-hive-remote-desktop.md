---
title: -Azure HDInsight Apache Hive ve Uzak Masaüstü kullanma
description: Uzak Masaüstü'nü kullanarak, HDInsight Hadoop kümesine bağlanma hakkında bilgi almak ve ardından Hive komut satırı arabirimi kullanarak Hive sorguları çalıştırın.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 6e0641f2d9427133f951ef63720b4efdac4defe5
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409063"
---
# <a name="use-apache-hive-with-apache-hadoop-on-hdinsight-with-remote-desktop"></a>Uzak Masaüstü kullanarak HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Bu makalede, Uzak Masaüstü kullanarak bir HDInsight kümesine bağlanma hakkında bilgi almak ve ardından Hive komut satırı arabirimi (CLI) kullanarak Apache Hive sorguları çalıştırın.

> [!IMPORTANT]  
> Uzak Masaüstü, yalnızca Windows işletim sistemi olarak kullandığınız HDInsight kümelerinde kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4 veya büyük bkz [Apache Hive ile HDInsight ve Beeline kullanma](apache-hadoop-use-hive-beeline.md) doğrudan küme üzerinde bir komut satırından Hive sorguları çalıştırma hakkında bilgi.

## <a id="prereq"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir:

* Bir Windows tabanlı HDInsight (Hadoop HDInsight üzerinde) kümesi
* Windows 10, Windows 8 veya Windows 7 çalıştıran bir istemci bilgisayar

## <a id="connect"></a>Uzak Masaüstü ile bağlanın
HDInsight kümesi için Uzak masaüstünü etkinleştirme, sonra yönergeleri izleyerek bağlanmamız [RDP kullanarak HDInsight kümelerini Bağlan](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Hive komutunu kullanın
HDInsight kümesi için masaüstüne bağladığınızda, Hive ile çalışmak için aşağıdaki adımları kullanın:

1. HDInsight masaüstünden başlatmak **Hadoop komut satırı**.
2. Hive CLI başlatmak için aşağıdaki komutu girin:

        %hive_home%\bin\hive

    CLI'yı başladığında, Hive CLI istemi görürsünüz: `hive>`.
3. CLI kullanarak girin adlı yeni bir tablo oluşturmak için aşağıdaki deyimleri **log4jLogs** örnek verileri kullanarak:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

   * **TABLO BIRAKMA**: Tablo zaten varsa ve tablodan veri dosyası siler.
   * **CREATE EXTERNAL TABLE**: Yeni bir 'dış' tablosu, Hive oluşturur. Dış tablolar yalnızca tablo tanımı Hive (verileri özgün konumunda bırakılır) depolar.

     > [!NOTE]  
     > Dış tablolar, temel alınan verileri (örneğin, bir otomatik veri karşıya yükleme işlemi) bir dış kaynaktan veya başka bir MapReduce işlem güncelleştirilecek beklediğiniz kullanılmalıdır, ancak her zaman en son verileri kullanmak için Hive sorguları istiyor.
     >
     > Bir dış tablo bırakılırken mu **değil** verileri, yalnızca tablo tanımını silin.
     >
     >
   * **SATIR BİÇİMİ**: Hive verileri nasıl biçimlendirildiğini söyler. Bu durumda, her günlük alanlar boşlukla ayrılır.
   * **TEXTFILE KONUMU OLARAK DEPOLANAN**: Verilerin depolandığı Hive bildirir (örnek/veri dizini) ve metin olarak depolanır.
   * **SEÇİN**: Tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]**. Bu değeri döndürmelidir **3** olmadığı için bu değeri içeren üç satır.
   * **INPUT__FILE__NAME gibi '%.log'** -Hive biz yalnızca veri sonu dosyalarından döndürmesi söyler. günlük. Bu, arama verilerini içeren ve verileri diğer örneklerden tanımladığımız şemayla eşleşmiyor veri dosyalarını döndürmesini tutar sample.log dosyasına kısıtlar.
4. Adlı yeni bir 'internal' tablo oluşturmak için aşağıdaki deyimleri kullanın **günlüklerini**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

   * **MEVCUT DEĞİLSE, TABLO OLUŞTURMA**: Zaten yoksa, bir tablo oluşturur. Çünkü **dış** anahtar sözcüğü kullanılmazsa, Hive veri ambarı'nda depolanır ve Hive ile tamamen yönetilen bir iç tablo budur.

     > [!NOTE]  
     > Farklı **dış** Ayrıca iç tablo bırakılırken tablolar, temel alınan verileri siler.
     >
     >
   * **ORC DEPOLANAN**: En iyi duruma getirilmiş satır irdelemenizde (ORC) verileri depolar. Hive veri depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimde budur.
   * **INSERT ÜZERİNE... SEÇİN**: Satırları seçer **log4jLogs** içeren tablo **[Hata]**, ardından verileri ekler **günlüklerini** tablo.

     Yalnızca satırlar içeren doğrulamak için **[Hata]** sütununda t4 saklı **günlüklerini** tablo, bulunan tüm satırlar döndürülecek aşağıdaki deyimini **günlüklerini**:

       SEÇİN * günlüklerini; öğesinden

     Üç veri satırı döndürülmesi, tüm içeren **[Hata]** sütun t4 içinde.

## <a id="summary"></a>Özet
Gördüğünüz gibi Hive komut etkileşimli bir HDInsight kümesinde Hive sorguları çalıştırma, iş durumunu izlemek ve çıktısını almak için kolay bir yol sağlar.

## <a id="nextsteps"></a>Sonraki adımlar
HDInsight Hive hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Tez Hive'ı kullanıyorsanız, hata ayıklama bilgileri için aşağıdaki belgelere bakın:

* [Windows tabanlı HDInsight üzerinde Apache Tez kullanıcı Arabirimi kullanma](../hdinsight-debug-tez-ui.md)
* [Linux tabanlı HDInsight üzerinde Apache Ambari Tez görünümünü kullanın](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx
