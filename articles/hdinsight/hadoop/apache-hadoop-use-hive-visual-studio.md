---
title: Data Lake (Hadoop) araçları ile Visual Studio - Azure Hdınsight Hive | Microsoft Docs
description: Azure Hdınsight'ta Apache Hadoop ile Apache Hive sorguları çalıştırmak için Visual Studio için Data Lake araçları kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: larryfr
ms.openlocfilehash: 862a2aae2e9d417ccf9daf336177b23842dd3db7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201797"
---
# <a name="run-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları kullanarak Hive sorguları çalıştırma

Sorgu için Apache Hive Visual Studio için Data Lake araçları kullanmayı öğrenin. Data Lake araçları, kolayca oluşturmanıza, gönderme ve Azure hdınsight'ta Hadoop Hive sorguları izlemenize olanak tanır.

## <a id="prereq"></a>Önkoşullar

* Azure Hdınsight (Hadoop hdınsight) kümesi

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio (aşağıdaki sürümlerinden biri):

    * Visual Studio 2013 Community/Professional/Premium/Ultimate güncelleştirme 4 ile

    * Visual Studio 2015 (herhangi bir sürümünü)

    * Visual Studio 2017 (herhangi bir sürümünü)

* Visual Studio ya da Azure Data Lake araçları Visual Studio için Hdınsight araçları. Bkz: [Hdınsight için Visual Studio Hadoop araçlarını kullanmaya başlamanıza](apache-hadoop-visual-studio-tools-get-started.md) yükleme ve yapılandırma araçları hakkında bilgi için.

## <a id="run"></a> Visual Studio kullanarak Hive sorguları çalıştırma

1. Açık **Visual Studio** seçip **yeni** > **proje** > **Azure Data Lake**  >   **HIVE** > **Hive uygulaması**. Bu proje için bir ad sağlayın.

2. Açık **Script.hql** bu proje ve aşağıdaki HiveQL ifadelerini Yapıştır ile oluşturulan dosyası:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * `DROP TABLE`: Tablo zaten varsa, bu deyimi bu siler.

   * `CREATE EXTERNAL TABLE`: Yeni bir 'external' tablo kovanında oluşturur. Dış tablolara (verileri özgün konumda bırakılır) Hive tablo tanımı yalnızca depolayın.

     > [!NOTE]
     > Dış kaynak tarafından güncelleştirilecek temel alınan veri beklediğiniz dış tablolara kullanılmalıdır. Örneğin, bir MapReduce işi veya Azure hizmeti.
     >
     > Bir dış tablo bırakma mu **değil** verileri, yalnızca tablo tanımını silin.

   * `ROW FORMAT`: Veri nasıl biçimlendirilmiş Hive söyler. Bu durumda, her günlüğün içinde alanlar boşlukla ayrılır.

   * `STORED AS TEXTFILE LOCATION`: Veri örnek/veri dizininde depolanır ve metin olarak depolanır Hive söyler.

   * `SELECT`: Tüm satırların sayımını seçme Burada sütun `t4` değeri içeren `[ERROR]`. Bu ifade değerini döndürür `3` çünkü bu değer içeren üç satır vardır.

   * `INPUT__FILE__NAME LIKE '%.log'` -Hive biz yalnızca veri biten dosyalarından döndürmesi gerektiğini bildirir. günlük. Bu yan tümcesi arama verileri içeren sample.log dosyası kısıtlar.

3. Araç çubuğundan seçin **Hdınsight kümesi** bu sorgu için kullanmak istediğiniz. Seçin **gönderme** deyimleri bir Hive işi olarak çalıştırmak için.

   ![Gönderme çubuğu](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

4. **Hive işi Özet** görünür ve çalışan iş hakkındaki bilgileri görüntüler. Kullanım **yenileme** kadar iş bilgilerini yenilemek için bağlantı **iş durumu** değişikliklerini **tamamlandı**.

   ![İş özeti tamamlanmış bir iş görüntüleme](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

5. Kullanım **iş çıktısı** bu işin çıktısını görüntülemek için bağlantı. Görüntülediği `[ERROR] 3`, bu sorgu tarafından döndürülen değer olduğu.

6. Ayrıca, bir proje oluşturmadan Hive sorguları çalıştırabilirsiniz. Kullanarak **Sunucu Gezgini**, genişletin **Azure** > **Hdınsight**Hdınsight sunucunuzun sağ tıklayın ve ardından **Hive sorgusu Yaz** .

7. İçinde **temp.hql** görünür, belge aşağıdaki HiveQL ifadelerini ekleyin:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * `CREATE TABLE IF NOT EXISTS`: Zaten yoksa, bir tablo oluşturur. Çünkü `EXTERNAL` anahtar sözcüğü kullanılmaz, bu deyim bir iç tablosu oluşturur. İç tabloları Hive veri ambarında depolanır ve Hive tarafından yönetilir.

     > [!NOTE]
     > Farklı `EXTERNAL` tablolar, bir iç tablosu da bırakarak temel alınan verileri siler.

   * `STORED AS ORC`: En iyi duruma getirilmiş satır sütun (ORC) biçiminde verileri depolar. ORC Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.

   * `INSERT OVERWRITE ... SELECT`: Satırları seçer `log4jLogs` içeren tablo `[ERROR]`, verileri ekler `errorLogs` tablo.

8. Araç çubuğundan seçin **gönderme** işi çalıştırmak için. Kullanım **iş durumu** işi başarıyla tamamlandığını belirlemek için.

9. İş tablo oluştuğunu doğrulamak için kullanmak **Sunucu Gezgini** ve genişletin **Azure** > **Hdınsight** > Hdınsight kümenize >  **Veritabanları hive** > **varsayılan**. **Günlüklerini** tablo ve **log4jLogs** tablo listelenir.

## <a id="nextsteps"></a>Sonraki adımlar

Gördüğünüz gibi Visual Studio için Hdınsight araçları Hdınsight'ta Hive sorguları ile çalışmak için kolay bir yol sağlar.

Hdınsight'ta Hive hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)

Diğer yolları hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)

* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Visual Studio için Hdınsight araçları hakkında daha fazla bilgi için:

* [Visual Studio için Hdınsight araçlarını kullanmaya başlama](apache-hadoop-visual-studio-tools-get-started.md)

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
