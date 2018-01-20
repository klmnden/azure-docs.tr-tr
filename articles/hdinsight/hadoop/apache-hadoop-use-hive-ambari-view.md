---
title: "Hive Hdınsight (Hadoop) - Azure üzerinde çalışmak için Ambari görünümleri kullanma | Microsoft Docs"
description: "Web tarayıcınız Hive görünümünden Hive sorguları göndermek için nasıl kullanılacağını öğrenin. Hive görünümü Ambari Web kullanıcı Arabirimi ile Linux tabanlı Hdınsight kümenizi sağlanan bir parçasıdır."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2018
ms.author: larryfr
ms.openlocfilehash: 5f66e60249af489e695029cbb072f3cc881bb039
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="use-ambari-hive-view-with-hadoop-in-hdinsight"></a>Hdınsight'ta Hadoop ile Ambari Hive görünümünü kullanın

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Ambari Hive görünümünü kullanarak Hive sorguları çalıştırmayı öğrenin. Ambari, yönetim ve izleme yardımcı programı Linux tabanlı Hdınsight kümeleri ile sağlanan ' dir. Ambari sağlanan özelliklerden birini Hive sorguları çalıştırmak için kullanılan bir Web UI'dır.

> [!NOTE]
> Ambari, bu belgede ele alınan değil birçok özellikleri vardır. Daha fazla bilgi için bkz: [Hdınsight kümelerini yönetme Ambari Web kullanıcı arabirimini kullanarak](../hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Önkoşullar

* Linux tabanlı Hdınsight kümesi. Kümeleri oluşturma hakkında daha fazla bilgi için bkz: [Hdınsight'ta Hadoop kullanmaya başlamanıza](apache-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan Azure Hdınsight kümesi gerektirir. Linux üzerinde Hdınsight sürüm 3.4 veya üstü kullanılan yalnızca işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="open-the-hive-view"></a>Hive görünümü Aç

Azure portalından Ambari görünümleri açabilirsiniz. Hdınsight kümenize seçin ve ardından **Ambari görünümleri** gelen **hızlı bağlantılar** bölümü.

![portal hızlı bağlantılar bölümü](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

Görünüm listesinden __Hive görünümü__.

![Seçilen Hive görünümü](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> Ambari erişirken siteye kimlik doğrulaması yapmak istenir. Yönetici girin (varsayılan `admin`) hesap adı ve küme oluştururken kullandığınız parola.

Aşağıdaki görüntüye benzer bir sayfa görmeniz gerekir:

![Hive görünümü için sorgu çalışma sayfası görüntüsü](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="run-a-query"></a>Sorgu çalıştırma

Bir Hive sorgusu çalıştırmak için Hive görünümünden aşağıdaki adımları kullanın.

1. Gelen __sorgu__ sekmesinde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * `DROP TABLE`: Tablo ve veri dosyası siler durumda tablo zaten mevcut.

   * `CREATE EXTERNAL TABLE`: Yeni bir "dış" Tablo kovanında oluşturur.
   Dış tablolara yalnızca tablo tanımı kovanında depolar. Veriler özgün konumda kalır.

   * `ROW FORMAT`: Veri nasıl biçimlendirilmiş gösterir. Bu durumda, her günlüğün içinde alanlar boşlukla ayrılır.

   * `STORED AS TEXTFILE LOCATION`: Verilerin depolandığı ve metin olarak depolanır gösterir.

   * `SELECT`: Sütun t4 [Hata] değeri, içerdiği tüm satırların sayımını seçer.

     > [!NOTE]
     > Bir dış kaynak tarafından güncelleştirilecek temel alınan veri beklediğiniz dış tabloları kullanır gibi bir otomatik veri işlem veya başka bir MapReduce işlem karşıya yükleyin. Bir dış tablo bırakma mu *değil* verileri, yalnızca tablo tanımını silin.

    > [!IMPORTANT]
    > Bırakın __veritabanı__ seçimi daha __varsayılan__. Bu belgedeki örneklerde, Hdınsight ile dahil varsayılan veritabanı kullanın.

2. Sorguyu başlatmak için kullanmak **yürütme** çalışma aşağıdaki düğmesine. Düğme turuncu kapatır ve metin değişir **durdurmak**.

3. Sorgu tamamlandıktan sonra **sonuçları** sekmesi işlemin sonuçlarını görüntüler. Sorgunun sonucu metindir:

        sev       cnt
        [ERROR]   3

    Kullanabileceğiniz **günlükleri** iş oluşturulan günlük bilgilerini görüntülemek için.

   > [!TIP]
   > İndirme veya sonuçlarından Kaydet **Sonuçları Kaydet** üst açılan iletişim kutusunda sol üst **sorgu işleminin sonuçları** bölümü.

4. Bu sorgunun ilk dört satırları seçin ve ardından **yürütme**. İş tamamlandığında, hiçbir sonuç olduğuna dikkat edin. Kullanarak **yürütme** sorgu parçası seçildiğinde düğmesi yalnızca seçilen ifadeleri çalıştırır. Bu durumda, seçim tablosundan satır alır son deyim eklemediniz. Yalnızca bu satırı seçin ve kullanırsanız **yürütme**, beklenen sonuçları görmeniz gerekir.

5. Bir çalışma sayfası eklemek için kullanın **yeni çalışma sayfası** alt kısmındaki düğmesi **sorgu Düzenleyicisi'ni**. Yeni çalışma sayfasında, aşağıdaki HiveQL ifadelerini girin:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * **Tablo IF değil var oluşturmak**: bir zaten yoksa, bir tablo oluşturur. Çünkü **dış** anahtar sözcüğü kullanılmaz, bir iç tablosu oluşturulur. Bir iç tablosu Hive veri ambarında depolanır ve tamamen Hive tarafından yönetilir. Aksine ile dış tablolar, bir iç tablosu bırakarak temel alınan veri de siler.

   * **AS ORC DEPOLANAN**: en iyi duruma getirilmiş satır sütunlu (ORC) biçiminde verileri depolar. ORC Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.

   * **INSERT ÜZERİNE... SEÇİN**: satırları seçer **log4jLogs** içeren tablo `[ERROR]`ve ardından verileri ekler **günlüklerini** tablo.

Kullanım **yürütme** bu sorguyu çalıştırmak için düğmesi. **Sonuçları** sekmesinde sorgu sıfır satır geri döndüğünde herhangi bir bilgi içermiyor. Durum olarak göstermelidir **başarılı** sorgu bittikten sonra.

### <a name="visual-explain"></a>Visual açıklanmaktadır

Görsel sorgu planı olarak görüntülenecek seçin **Visual açıklayan** sekmenin çalışma altında.

**Visual açıklayan** sorgu görünümünü karmaşık sorgular akışını anlamak yararlı olabilir. Bu görünüm metinsel denk kullanarak gördüğünüz **açıklama** düğmesi sorgu Düzenleyicisi'nde.

### <a name="tez-ui"></a>Tez UI

Sorgu için Tez UI görüntülemek için seçin **Tez** sekmenin çalışma altında.

> [!IMPORTANT]
> Tez tüm sorguları çözümlemek için kullanılmaz. Çok sayıda sorgu Tez kullanılmadan çözümleyebilirsiniz. 

Tez sorgusunu çözümlemek için kullanıldıysa, yönlendirilmiş Çevrimsiz grafik (DAG) görüntülenir. Geçmişte çalıştırmak sorgular için DAG görüntülemek istiyorsanız veya Tez işlemde hata ayıklamak istiyorsanız kullanın [Tez Görünüm](../hdinsight-debug-ambari-tez-view.md) yerine.

## <a name="view-job-history"></a>İş geçmişini görüntüleme

__İşleri__ sekmesi Hive sorguları geçmişini görüntüler.

![İş Geçmişi görüntüsü](./media/apache-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Veritabanı tabloları

Kullanabileceğiniz __tabloları__ tabloları Hive veritabanı içinde çalışmak üzere sekmesi.

![Tablolar sekmesini görüntüsü](./media/apache-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Kaydedilmiş sorgular

Gelen **sorgu** sekmesinde sorguları isteğe bağlı olarak kaydedebilir. Bir sorgu kaydettikten sonra ondan yeniden kullanabilir __kaydedilen sorguları__ sekmesi.

![Kaydedilen sorgular sekmesini görüntüsü](./media/apache-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Ayrıca, kullanıcı tanımlı işlevler (UDF) Hive genişletebilirsiniz. Bir UDF işlev veya kolayca modellenir değil mantığı içinde HiveQL uygulamak için kullanın.

Bildirme ve UDF'ler kümesini kullanarak kaydetme **UDF** Hive görünümü üstündeki sekmesi. Bu UDF'ler kullanılabilir **sorgu Düzenleyicisi'ni**.

![UDF sekmesini görüntüsü](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

Hive görünümü için bir UDF ekledikten sonra bir **Ekle UDF'ler** düğmesinin alt kısmındaki **sorgu Düzenleyicisi'ni**. Bu girişi seçerseniz Hive görünümünde tanımlanan UDF'ler açılan listesini görüntüler. Bir UDF seçerek HiveQL ifadelerini UDF etkinleştirmek için sorgunuza ekler.

Örneğin, aşağıdaki özelliklere sahip bir UDF tanımlanmışsa:

* Kaynak adı: myudfs

* Kaynak yol: /myudfs.jar

* UDF ad: myawesomeudf

* UDF sınıf adı: com.myudfs.Awesome

Kullanarak **Ekle UDF'ler** düğmesi görüntüler adlı bir girdi **myudfs**, bu kaynak için tanımlanan her UDF için başka bir açılır liste. Bu durumda olan **myawesomeudf**. Bu girdi seçerek aşağıdaki sorguyu başlangıcına ekler:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Ardından sorgunuza UDF kullanabilirsiniz. Örneğin, `SELECT myawesomeudf(name) FROM people;`.

UDF'ler hdınsight'ta Hive kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hive veya Pig hdınsight'ta Python kullanarak](python-udf-hdinsight.md)
* [Hdınsight için özel bir Hive UDF ekleme](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive ayarları

Tez (varsayılan) kovana MapReduce için yürütme altyapısı değiştirme gibi çeşitli Hive ayarlarını değiştirebilirsiniz.

## <a id="nextsteps"></a>Sonraki adımlar

Hdınsight'ta Hive hakkında genel bilgi için:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)

Diğer yöntemler hakkında bilgi için hdınsight'ta Hadoop ile çalışabilirsiniz:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)
