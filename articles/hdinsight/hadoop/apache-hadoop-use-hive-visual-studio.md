---
title: Visual Studio - Azure HDInsight (Hadoop) Data Lake araçları ile hive
description: Azure HDInsight üzerinde Apache Hadoop ile Apache Hive sorguları çalıştırmayı Visual Studio için Data Lake Araçları'nı kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.openlocfilehash: fd57e713a24fb83e42d46b4a1978530b706bf583
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049905"
---
# <a name="run-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları kullanarak Hive sorguları çalıştırma

Sorgu Apache Hive Visual Studio için Data Lake araçları kullanmayı öğrenin. Data Lake araçları, kolayca oluşturun, gönderin ve Hive sorgularını Azure HDInsight üzerinde Hadoop için izleme sağlar.

## <a id="prereq"></a>Önkoşullar

* Bir Azure HDInsight (Hadoop HDInsight üzerinde) kümesi

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio (aşağıdaki sürümlerinden biri):

    * Visual Studio 2013 Community/Professional/Premium/Ultimate güncelleştirme 4 ile

    * Visual Studio 2015 (herhangi bir sürümü)

    * Visual Studio 2017 (herhangi bir sürümü)

* Visual Studio için HDInsight araçları veya Visual Studio için Azure Data Lake araçları. Bkz: [HDInsight için Visual Studio Hadoop araçlarını kullanmaya başlama](apache-hadoop-visual-studio-tools-get-started.md) yükleme ve yapılandırma araçları hakkında bilgi.

## <a id="run"></a> Visual Studio kullanarak Hive sorguları çalıştırma

1. Açık **Visual Studio** seçip **yeni** > **proje** > **Azure Data Lake**  >   **HIVE** > **Hive uygulaması**. Bu proje için bir ad sağlayın.

2. Açık **Script.hql** bu proje ve aşağıdaki HiveQL ifadelerini yapıştırma seçeneğiyle oluşturulan dosya:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

   * `DROP TABLE`: Tablo varsa, bu deyimi siler.

   * `CREATE EXTERNAL TABLE`: Yeni bir 'dış' tablosu, Hive oluşturur. Dış tablolar yalnızca tablo tanımı Hive (verileri özgün konumunda bırakılır) depolayın.

     > [!NOTE]
     > Dış tablolar, temel alınan veriler dış bir kaynak tarafından güncelleştirilmesi beklediğiniz kullanılmalıdır. Örneğin, bir MapReduce işi veya Azure hizmeti.
     >
     > Bir dış tablo bırakılırken mu **değil** verileri, yalnızca tablo tanımını silin.

   * `ROW FORMAT`: Veri nasıl biçimlendirildiğini Hive söyler. Bu durumda, her günlük alanlar boşlukla ayrılır.

   * `STORED AS TEXTFILE LOCATION`: Hive veriler örnek/veri dizininde depolanır ve metin olarak depolandığını belirtir.

   * `SELECT`: Tüm satırların sayımını seçme Burada sütun `t4` değeri içeren `[ERROR]`. Bu bildirimi bir değeri döndürür `3` olmadığı için bu değeri içeren üç satır.

   * `INPUT__FILE__NAME LIKE '%.log'` -Hive biz yalnızca veri sonu dosyalarından dönmesi gerektiğini söyler. günlük. Bu yan tümce arama verileri içeren sample.log dosyasına kısıtlar.

3. Araç çubuğundan seçin **HDInsight küme** bu sorgu için kullanmak istediğiniz. Seçin **Gönder** deyimleri bir Hive işi olarak çalıştırmak için.

   ![Gönderme çubuğu](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

4. **Hive işi özeti** görünür ve çalışan işle ilgili bilgileri görüntüler. Kullanım **Yenile** kadar iş bilgilerini yenilemek için bağlantı **iş durumu** değişikliklerini **tamamlandı**.

   ![İş özeti tamamlanan iş görüntüleme](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

5. Kullanım **iş çıktısı** bu işin çıkışı görüntülemek için bağlantı. Bu görüntüler `[ERROR] 3`, bu sorgu tarafından döndürülen değer olduğu.

6. Bir proje oluşturmadan Hive sorguları da çalıştırabilirsiniz. Kullanarak **Sunucu Gezgini**, genişletme **Azure** > **HDInsight**HDInsight sunucunuza sağ tıklayın ve ardından **Hive sorgusu Yaz** .

7. İçinde **temp.hql** görünen belge aşağıdaki HiveQL ifadelerini ekleyin:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

   * `CREATE TABLE IF NOT EXISTS`: Zaten yoksa, bir tablo oluşturur. Çünkü `EXTERNAL` anahtar sözcüğü kullanılmazsa, bu deyimi iç tablo oluşturur. İç tablolar Hive veri ambarı'nda depolanır ve Hive ile yönetilir.

     > [!NOTE]
     > Farklı `EXTERNAL` Ayrıca iç tablo bırakılırken tablolar, temel alınan verileri siler.

   * `STORED AS ORC`: Veri en iyi duruma getirilmiş satır irdelemenizde (ORC) depolar. ORC Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.

   * `INSERT OVERWRITE ... SELECT`: Satırları seçer `log4jLogs` içeren tablo `[ERROR]`, ardından verileri ekler `errorLogs` tablo.

8. Araç çubuğundan seçin **Gönder** işi çalıştırmak için. Kullanım **iş durumu** işin başarıyla tamamlandığını belirlemek için.

9. İş tablo oluşturulan doğrulamak için **Sunucu Gezgini** genişletin **Azure** > **HDInsight** > HDInsight kümenizi >  **Hive veritabanları** > **varsayılan**. **Günlüklerini** tablo ve **log4jLogs** tabloda listelenmiştir.

## <a id="nextsteps"></a>Sonraki adımlar

Gördüğünüz gibi Visual Studio için HDInsight araçları Hive sorguları ile HDInsight üzerinde çalışmak için kolay bir yol sağlar.

HDInsight Hive hakkında genel bilgi için:

* [HDInsight üzerinde Hadoop ile Hive kullanma](hdinsight-use-hive.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Hadoop ile Pig kullanma](hdinsight-use-pig.md)

* [HDInsight üzerinde Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Visual Studio için HDInsight araçları hakkında daha fazla bilgi için:

* [Visual Studio için HDInsight araçları ile Başlarken](apache-hadoop-visual-studio-tools-get-started.md)

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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
