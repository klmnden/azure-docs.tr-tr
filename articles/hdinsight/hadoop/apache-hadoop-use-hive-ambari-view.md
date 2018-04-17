---
title: Hive Hdınsight (Hadoop) - Azure üzerinde çalışmak için Ambari görünümleri kullanma | Microsoft Docs
description: Web tarayıcınız Hive görünümünden Hive sorguları göndermek için nasıl kullanılacağını öğrenin. Hive görünümü Ambari Web kullanıcı Arabirimi ile Linux tabanlı Hdınsight kümenizi sağlanan bir parçasıdır.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/13/2018
ms.author: larryfr
ms.openlocfilehash: 78fee8e3b3e4c0e0c02fa5e1c85bdef58c9cd543
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-ambari-hive-view-with-hadoop-in-hdinsight"></a>Hdınsight'ta Hadoop ile Ambari Hive görünümünü kullanın

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Ambari Hive görünümünü kullanarak Hive sorguları çalıştırmayı öğrenin. Hive görünümü yazar, en iyi duruma getirme ve web tarayıcınızdan Hive sorguları çalıştırmanıza olanak sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Bir Linux tabanlı Hadoop Hdınsight kümesi sürüm 3.4 veya daha büyük.

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir web tarayıcısı

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

1. [Azure portalı](https://portal.azure.com) açın.

2. Hdınsight kümenize seçin ve ardından **Ambari görünümleri** gelen **hızlı bağlantılar** bölümü.

    ![portal hızlı bağlantılar bölümü](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

    Küme oturum açma kimlik doğrulaması yapmak isteyip istemediğiniz sorulduğunda kullanın (varsayılan `admin`) hesap adı ve küme oluştururken verdiğiniz parolayı.

3. Görünüm listesinden __Hive görünümü__.

    ![Seçilen Hive görünümü](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

    Hive görünümü sayfasında, aşağıdaki görüntüye benzer:

    ![Hive görünümü için sorgu çalışma sayfası görüntüsü](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

4. Gelen __sorgu__ sekmesinde, aşağıdaki HiveQL ifadelerini çalışma sayfasına yapıştırın:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS loglevel, COUNT(*) AS count FROM log4jLogs 
        WHERE t4 = '[ERROR]' 
        GROUP BY t4;
    ```

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

   * `DROP TABLE`: Tablo ve veri dosyası siler durumda tablo zaten mevcut.

   * `CREATE EXTERNAL TABLE`: Yeni bir "dış" Tablo kovanında oluşturur.
   Dış tablolara yalnızca tablo tanımı kovanında depolar. Veriler özgün konumda kalır.

   * `ROW FORMAT`: Veri nasıl biçimlendirilmiş gösterir. Bu durumda, her günlüğün içinde alanlar boşlukla ayrılır.

   * `STORED AS TEXTFILE LOCATION`: Verilerin depolandığı ve metin olarak depolanır gösterir.

   * `SELECT`: Sütun t4 [Hata] değeri, içerdiği tüm satırların sayımını seçer.

    > [!IMPORTANT]
    > Bırakın __veritabanı__ seçimi daha __varsayılan__. Bu belgedeki örneklerde, Hdınsight ile dahil varsayılan veritabanı kullanın.

5. Sorguyu başlatmak için kullanmak **yürütme** çalışma aşağıdaki düğmesine. Düğme turuncu kapatır ve metin değişir **durdurmak**.

6. Sorgu tamamlandıktan sonra **sonuçları** sekmesi işlemin sonuçlarını görüntüler. Sorgunun sonucu metindir:

        loglevel       count
        [ERROR]        3

    Kullanabileceğiniz **günlükleri** iş oluşturulan günlük bilgilerini görüntülemek için.

   > [!TIP]
   > İndirme veya sonuçlarından Kaydet **Sonuçları Kaydet** üst açılan iletişim kutusunda sol üst **sorgu işleminin sonuçları** bölümü.

### <a name="visual-explain"></a>Visual açıklanmaktadır

Görsel sorgu planı olarak görüntülenecek seçin **Visual açıklayan** sekmenin çalışma altında.

**Visual açıklayan** sorgu görünümünü karmaşık sorgular akışını anlamak yararlı olabilir. Bu görünüm metinsel denk kullanarak gördüğünüz **açıklama** düğmesi sorgu Düzenleyicisi'nde.

### <a name="tez-ui"></a>Tez kullanıcı Arabirimi

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

> [!TIP]
> Kayıtlı sorgu varsayılan küme depolama alanında depolanır. Kayıtlı sorgu yolunda bulabilirsiniz `/user/<username>/hive/scripts`. Bunlar düz metin depolanır `.hql` dosyaları.
>
> Küme silme, ancak depolama korumak, bir yardımcı programı gibi kullanabilirsiniz [Azure Storage Gezgini](https://azure.microsoft.com/features/storage-explorer/) veya Data Lake Storage Gezgini (gelen [Azure Portal](https://portal.azure.com)) sorguları almak için.

## <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Kullanıcı tanımlı işlevler (UDF) Hive genişletebilirsiniz. Bir UDF işlev veya kolayca modellenir değil mantığı içinde HiveQL uygulamak için kullanın.

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
