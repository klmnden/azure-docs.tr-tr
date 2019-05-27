---
title: Apache (Apache Hadoop) Data Lake araçları ile Visual Studio - Azure HDInsight Hive
description: Azure HDInsight üzerinde Apache Hadoop ile Apache Hive sorguları çalıştırmayı Visual Studio için Data Lake Araçları'nı kullanmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: hrasheed
ms.openlocfilehash: 7480dafe435e555bfba81ebd9242bb5724c0bf3f
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861590"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Visual Studio için Data Lake araçları kullanarak Apache Hive sorguları çalıştırma

Sorgu Apache Hive Visual Studio için Data Lake araçları kullanmayı öğrenin. Data Lake araçları, kolayca oluşturun, gönderin ve Hive sorgularını Azure HDInsight üzerinde Apache Hadoop için izlemenize olanak tanır.

## <a id="prereq"></a>Önkoşullar

* HDInsight üzerinde Apache Hadoop kümesi. Bkz: [Linux'ta HDInsight kullanmaya başlama](./apache-hadoop-linux-tutorial-get-started.md).

* Visual Studio (aşağıdaki sürümlerinden biri):

    * Visual Studio 2015, 2017 (herhangi bir sürümü)
    * Visual Studio 2013 Community/Professional/Premium/Ultimate güncelleştirme 4 ile

* Visual Studio için HDInsight araçları veya Visual Studio için Azure Data Lake araçları. Bkz: [HDInsight için Visual Studio Hadoop araçlarını kullanmaya başlama](apache-hadoop-visual-studio-tools-get-started.md) yükleme ve yapılandırma araçları hakkında bilgi.

## <a id="run"></a> Visual Studio kullanarak Apache Hive sorguları çalıştırma

Hive sorguları oluşturmak ve çalıştırmak için iki seçeneğiniz vardır:

* Geçici sorgular oluşturma
* Hive uygulaması oluşturma

### <a name="ad-hoc"></a>Geçici

Geçici sorgular ya da çalıştırılabilir **Batch** veya **etkileşimli** modu.

1. Açık **Visual Studio**.

2. Gelen **Sunucu Gezgini**, gitmek **Azure** > **HDInsight**.

3. Genişletin **HDInsight**ve sorguyu çalıştırın ve ardından istediğiniz kümeye sağ **Hive sorgusu yaz**.

4. Aşağıdaki hive sorgusunu girin:

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. **Yürüt**’ü seçin. Yürütme modu için varsayılan olarak Not **etkileşimli**.

    ![Etkileşimli Hive sorguları yürütme ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. Aynı sorgu çalıştırmak için **Batch** modu, iki durumlu aşağı açılan listeden **etkileşimli** için **Batch**. Yürütme düğmesini değişiklikleri Not **yürütme** için **Gönder**.

    ![Hive sorgusu gönderme ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/vs-batch-query.png)

    Hive düzenleyicisi IntelliSense’i destekler. Visual Studio için Data Lake Araçları, Hive betiğinizi düzenlerken uzak meta verilerin yüklenmesini destekler. Örneğin, `SELECT * FROM`, IntelliSense önerilen tablo adlarını listeler. Bir tablo adı belirtildiğinde, IntelliSense sütun adlarını listeler. Araçlar çoğu Hive DML deyimlerini, alt sorguları ve yerleşik UDF'leri destekler. IntelliSense yalnızca HDInsight araç çubuğunda seçilen kümelerin meta verilerini önerir.

    ![HDInsight Visual Studio Araçları IntelliSense örnek 1’in ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-table-name.png "U-SQL IntelliSense")
   
    ![HDInsight Visual Studio Araçları IntelliSense örnek 2’nin ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-column-name.png "U-SQL IntelliSense")

7. **Gönder** veya **Gönder (Gelişmiş)** öğesini seçin.

   Gelişmiş gönderme seçeneğini belirlerseniz, betik için **İş Adı**, **Bağımsız Değişkenler**, **Ek Yapılandırmalar** ve **Durum Dizini**’ni yapılandırın:

    ![HDInsight Hadoop Hive sorgusunun ekran görüntüsü](./media/apache-hadoop-use-hive-visual-studio/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Sorgu gönderme")

### <a name="hive-application"></a>Hive uygulaması

1. Açık **Visual Studio**.

2. Menü çubuğundan gidin **dosya** > **yeni** > **proje**.

3. Gelen **yeni proje** penceresinde gidin **şablonları** > **Azure Data Lake** > **HIVE (HDInsight)**  >  **Hive uygulaması**. 

4. Bu proje için bir ad belirtin ve ardından **Tamam**.

5. Açık **Script.hql** bu proje ve aşağıdaki HiveQL ifadelerini yapıştırma seçeneğiyle oluşturulan dosya:

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

   * `ROW FORMAT`: Hive verileri nasıl biçimlendirildiğini söyler. Bu durumda, her günlük alanlar boşlukla ayrılır.

   * `STORED AS TEXTFILE LOCATION`: Hive veri örnek/veri dizininde depolanır ve metin olarak depolandığını belirtir.

   * `SELECT`: Tüm satırların sayımını seçin burada sütun `t4` değeri içeren `[ERROR]`. Bu bildirimi bir değeri döndürür `3` olmadığı için bu değeri içeren üç satır.

   * `INPUT__FILE__NAME LIKE '%.log'` -Hive biz yalnızca veri sonu dosyalarından dönmesi gerektiğini söyler. günlük. Bu yan tümce arama verileri içeren sample.log dosyasına kısıtlar.

6. Araç çubuğundan seçin **HDInsight küme** bu sorgu için kullanmak istediğiniz. Seçin **Gönder** deyimleri bir Hive işi olarak çalıştırmak için.

   ![Gönderme çubuğu](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

7. **Hive işi özeti** görünür ve çalışan işle ilgili bilgileri görüntüler. Kullanım **Yenile** kadar iş bilgilerini yenilemek için bağlantı **iş durumu** değişikliklerini **tamamlandı**.

   ![İş özeti tamamlanan iş görüntüleme](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

8. Kullanım **iş çıktısı** bu işin çıkışı görüntülemek için bağlantı. Bu görüntüler `[ERROR] 3`, bu sorgu tarafından döndürülen değer olduğu.

### <a name="additional-example"></a>Ek örnek

Bu örnekte dayanan `log4jLogs` önceki adımda oluşturulan tablo.

1. Gelen **Sunucu Gezgini**, kümenizi sağ tıklayıp **Hive sorgusu yaz**.

2. Aşağıdaki hive sorgusunu girin:

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

    * `CREATE TABLE IF NOT EXISTS`: Zaten yoksa, bir tablo oluşturur. Çünkü `EXTERNAL` anahtar sözcüğü kullanılmazsa, bu deyimi iç tablo oluşturur. İç tablolar Hive veri ambarı'nda depolanır ve Hive ile yönetilir.
    
    > [!NOTE]  
    > Farklı `EXTERNAL` Ayrıca iç tablo bırakılırken tablolar, temel alınan verileri siler.

    * `STORED AS ORC`: En iyi duruma getirilmiş satır irdelemenizde (ORC) verileri depolar. ORC Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.
    
    * `INSERT OVERWRITE ... SELECT`: Satırları seçer `log4jLogs` içeren tablo `[ERROR]`, ardından verileri ekler `errorLogs` tablo.

3. Sorgu yürütme **Batch** modu.

4. İş tablo oluşturulan doğrulamak için **Sunucu Gezgini** genişletin **Azure** > **HDInsight** > HDInsight kümenizi >  **Hive veritabanları** > **varsayılan**. **Günlüklerini** tablo ve **log4jLogs** tabloda listelenmiştir.

## <a id="nextsteps"></a>Sonraki adımlar

Gördüğünüz gibi Visual Studio için HDInsight araçları Hive sorguları ile HDInsight üzerinde çalışmak için kolay bir yol sağlar.

HDInsight Hive hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)

Diğer yollar hakkında daha fazla bilgi için HDInsight üzerinde Hadoop ile çalışabilirsiniz:

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)

* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Visual Studio için HDInsight araçları hakkında daha fazla bilgi için:

* [Visual Studio için HDInsight araçları ile Başlarken](apache-hadoop-visual-studio-tools-get-started.md)