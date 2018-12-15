---
title: Apache Hive HDInsight - Azure sorgu konsolunda kullanın
description: Apache Hive sorguları bir HDInsight Hadoop kümesinde tarayıcınızdan çalıştırmak için web tabanlı sorgu Konsolu kullanma konusunda bilgi edinin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 1e638bd348b7a5272dd8bfbe25aa841f38a51b9a
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409709"
---
# <a name="run-apache-hive-queries-using-the-query-console"></a>Sorgu Konsolu kullanarak Apache Hive sorguları çalıştırma
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Bu makalede, Apache Hive sorguları bir HDInsight Hadoop kümesinde tarayıcınızdan çalıştırmak için HDInsight sorgu Konsolu kullanma öğreneceksiniz.

> [!IMPORTANT]  
> HDInsight sorgu konsolu yalnızca Windows tabanlı HDInsight kümelerinde kullanılabilir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4 veya büyük bkz [Apache Hive sorgularını çalıştırma Ambari Hive Görünümü'nde](apache-hadoop-use-hive-ambari-view.md) bir web tarayıcısından Hive sorguları çalıştırma hakkında bilgi.

## <a id="prereq"></a>Önkoşullar
Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir.

* Bir Windows tabanlı HDInsight Hadoop kümesi
* Modern bir web tarayıcısı

## <a id="run"></a> Sorgu Konsolu kullanarak Apache Hive sorguları çalıştırma
1. Bir web tarayıcısı açın ve gidin **https://CLUSTERNAME.azurehdinsight.net**burada **CLUSTERNAME** HDInsight kümenizin adıdır. İstenirse, kullanıcı adını ve kümeyi oluştururken kullandığınız parolayı girin.
2. Sayfanın üstündeki bağlantılardan birini seçin **Hive Düzenleyicisi**. Bu, HDInsight kümesinde çalıştırmak istediğiniz HiveQL ifadelerini girmek için kullanılan bir form görüntüler.

    ![hive Düzenleyicisi](./media/apache-hadoop-use-hive-query-console/queryconsole.png)

    Metni Değiştir `Select * from hivesampletable` aşağıdaki HiveQL ifadelerini ile:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

   * **TABLO BIRAKMA**: Tablo zaten varsa ve tablodan veri dosyası siler.
   * **CREATE EXTERNAL TABLE**: Yeni bir 'dış' tablosu, Hive oluşturur. Dış tablolar yalnızca tablo tanımı Hive içinde depolamak; verileri özgün konumunda bırakılır.

     > [!NOTE]  
     > Dış tablolar, temel alınan verileri (örneğin, bir otomatik veri karşıya yükleme işlemi) bir dış kaynaktan veya başka bir MapReduce işlem güncelleştirilecek beklediğiniz kullanılmalıdır, ancak her zaman en son verileri kullanmak için Hive sorguları istiyor.
     >
     > Bir dış tablo bırakılırken mu **değil** verileri, yalnızca tablo tanımını silin.
     >
     >
   * **SATIR BİÇİMİ**: Hive verileri nasıl biçimlendirildiğini söyler. Bu durumda, her günlük alanlar boşlukla ayrılır.
   * **TEXTFILE KONUMU OLARAK DEPOLANAN**: Verilerin depolandığı Hive bildirir (örnek/veri dizini) ve metin olarak depolanır
   * **SEÇİN**: Tüm satırların sayımını seçin burada sütun **t4** değerini içeren **[Hata]**. Bu değeri döndürmelidir **3** olmadığı için bu değeri içeren üç satır.
   * **INPUT__FILE__NAME gibi '%.log'** -Hive biz yalnızca veri sonu dosyalarından döndürmesi söyler. günlük. Bu, arama verilerini içeren ve verileri diğer örneklerden tanımladığımız şemayla eşleşmiyor veri dosyalarını döndürmesini tutar sample.log dosyasına kısıtlar.
3. **Gönder**'e tıklayın. **Proje oturumunu** sayfanın sonunda iş ayrıntılarını görüntülenmelidir.
4. Zaman **durumu** alan değişikliklerini **tamamlandı**seçin **ayrıntıları** iş için. Ayrıntıları sayfasındaki **iş çıktısı** içeren `[ERROR]    3`. Kullanabileceğiniz **indirme** düğmesi tanımlı işlemin çıktısını içeren bir dosyayı indirmek için bu alanı altında.

## <a id="summary"></a>Özet
Gördüğünüz gibi sorgu konsolunda bir HDInsight kümesinde Hive sorguları çalıştırma, iş durumunu izlemek ve çıktısını almak için kolay bir yol sağlar.

Hive işlerini çalıştırmak için Hive sorgu Konsolu kullanma hakkında daha fazla bilgi edinmek için seçin **Başlarken** sorgu Konsolu üstünde, ardından sağlanan örnekleri kullanın. Her örnek örnekte kullanılan HiveQL ifadelerini açıklamalar dahil olmak üzere verileri analiz etmek üzere Hive kullanma işleminde size yol gösterir.

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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
