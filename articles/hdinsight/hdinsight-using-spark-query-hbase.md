---
title: Azure Hdınsight HBase data - okumasına ve yazmasına Spark kullanın | Microsoft Docs
description: Spark HBase Bağlayıcısı'nı okuyun ve Spark kümeden bir HBase kümesi veri yazmak için kullanın.
services: hdinsight
documentationcenter: ''
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: 7cfc7f586e8a92c29736a7c4cff0b12796be430a
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34161184"
---
# <a name="use-spark-to-read-and-write-hbase-data"></a>HBase verilerini okuyup yazmak için Spark kullanma

Apache HBase, genellikle düşük düzey API'si (taramaları, alır ve yerleştirmelerin) veya Phoenix kullanarak bir SQL söz dizimi sorgulanır. Apache, uygun bir Spark HBase bağlayıcı ve kullanıcı alternatif sorgulamak ve HBase tarafından depolanan verileri değiştirmek için de sağlar.

## <a name="prerequisites"></a>Önkoşullar

* İki seçenek, Hdınsight kümeleri, bir HBase ve bir Spark Spark yüklenmiş 2.1 (Hdınsight 3.6) ile ayırın.
* Önerilen yapılandırma aynı sanal ağda hem kümeler dağıtmak için en düşük gecikme süresi ile doğrudan HBase kümesi ile iletişim kurması Spark kümesi gerekiyor. Daha fazla bilgi için bkz: [Azure portalını kullanarak Hdınsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-portal.md).
* Her küme SSH erişim.
* Her kümenin varsayılan depolama erişim.

## <a name="overall-process"></a>Genel işlem

Hdınsight kümenize sorgulamak Spark kümenizin etkinleştirmek için üst düzey işlem aşağıdaki gibidir:

1. Bazı örnek veriler HBase hazırlayın.
2. Hbase-site.xml dosyasının HBase kümesi yapılandırma klasörünüzdeki (/ hbase/etc/conf) alın.
3. Hbase-site.xml bir kopyasını Spark 2 yapılandırma klasörünüzde (/ spark2/etc/conf) yerleştirin.
4. Çalıştırma `spark-shell` Spark HBase bağlayıcı kendi Maven tarafından başvuran koordinatları `packages` seçeneği.
5. Spark şemadan HBase için eşleşen bir katalog tanımlayın.
6. RDD veya DataFrame API'lerini kullanarak HBase verilerle etkileşim.

## <a name="prepare-sample-data-in-hbase"></a>HBase örnek verileri hazırlama

Bu adımda, oluşturma ve Spark kullanarak ardından sorgu yürütebilir HBase basit bir tabloda doldurun.

1. SSH kullanarak HBase kümenize baş düğüme bağlanma. Daha fazla bilgi için bkz: [SSH kullanarak Hdınsight Bağlan](hdinsight-hadoop-linux-use-ssh-unix.md).
2. HBase kabuğunu çalıştırın:

        hbase shell

3. Oluşturma bir `Contacts` tablo sütun ailesi ile `Personal` ve `Office`:

        create 'Contacts', 'Personal', 'Office'

4. Veri birkaç örnek satırı yük:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
        put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'

## <a name="acquire-hbase-sitexml-from-your-hbase-cluster"></a>Hbase-site.xml, HBase kümeden Al

1. SSH kullanarak HBase kümenize baş düğüme bağlanma.
2. Hbase-site.xml yerel depolama biriminden HBase kümenin varsayılan depolama kök dizinine kopyalayın:

        hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml /

3. HBase kümesi kullanarak için gidin [Azure portal](https://portal.azure.com).
4. Depolama hesabı seçin. 

    ![Depolama hesapları](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

5. Depolama hesabı varsayılan sütunu altında bir onay işareti içeren listeyi seçin.

    ![Varsayılan depolama hesabı](./media/hdinsight-using-spark-query-hbase/default-storage.png)

6. Depolama hesabı bölmesi, BLOB'ları kutucuk seçin.

    ![BLOB'ları döşeme](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

7. Kapsayıcıları listesinde, HBase kümesi tarafından kullanılan kapsayıcısı seçin.
8. Dosya listesinde seçin `hbase-site.xml`.

    ![HBase-site.xml](./media/hdinsight-using-spark-query-hbase/hbase-site-xml.png)

9. Blob özellikleri paneli seçin karşıdan yükle ve Kaydet `hbase-site.xml` yerel makinenizde bir konuma.

    ![İndirme](./media/hdinsight-using-spark-query-hbase/download.png)

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Hbase-site.xml Spark kümesinde yerleştirme

1. Spark kümesi kullanmaya gidin [Azure portal](https://portal.azure.com).
2. Depolama hesabı seçin.

    ![Depolama hesapları](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

3. Depolama hesabı varsayılan sütunu altında bir onay işareti içeren listeyi seçin.

    ![Varsayılan depolama hesabı](./media/hdinsight-using-spark-query-hbase/default-storage.png)

4. Depolama hesabı bölmesi, BLOB'ları kutucuk seçin.

    ![BLOB'ları döşeme](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

5. Kapsayıcıları listesinde, Spark küme tarafından kullanılan kapsayıcısı seçin.
6. Yükleme'yi seçin.

    ![Karşıya Yükle](./media/hdinsight-using-spark-query-hbase/upload.png)

7. Seçin `hbase-site.xml` yerel makinenize daha önce indirilen dosya.

    ![Hbase-site.xml karşıya yükle](./media/hdinsight-using-spark-query-hbase/upload-selection.png)

8. Yükleme'yi seçin.
9. SSH kullanarak Spark kümenizin baş düğümüne bağlanmak.
10. Kopya `hbase-site.xml` Spark kümenizin varsayılan depolama Spark 2 yapılandırma klasörüne kümenin yerel depolama biriminden:

        sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Spark Spark HBase bağlayıcı başvuran Shell çalıştırma

1. SSH kullanarak Spark kümenizin baş düğümüne bağlanmak.
2. Spark HBase bağlayıcı paketini belirtme spark Kabuk başlatın:

        spark-shell --packages com.hortonworks:shc-core:1.1.0-2.1-s_2.11 --repositories http://repo.hortonworks.com/coroups/public/

3. Bu Spark Kabuk örneğini açık tutmak ve sonraki adıma devam edin.

## <a name="define-a-catalog-and-query"></a>Bir katalog ve sorguyu tanımlayın

Bu adımda, Spark şemadan HBase için eşleşen bir katalog nesnesi tanımlayın. 

1. Açık, Spark kabuğunda, aşağıdaki komutu çalıştırarak `import` deyimleri:

        import org.apache.spark.sql.{SQLContext, _}
        import org.apache.spark.sql.execution.datasources.hbase._
        import org.apache.spark.{SparkConf, SparkContext}
        import spark.sqlContext.implicits._

2. HBase oluşturduğunuz kişiler tablo için bir katalog tanımlayın:
    1. Adlı HBase tablo için bir katalog şema tanımlamak `Contacts`.
    2. Rowkey olarak tanımlamak `key`ve Spark sütun ailesi, sütun adı ve HBase kullanılan sütun türü için kullanılan sütun adları eşleyin.
    3. Rowkey da ayrıntılı bir adlandırılmış sütun olarak tanımlanması gerekir (`rowkey`), belirli sütun ailesi olan `cf` , `rowkey`.

            def catalog = s"""{
                |"table":{"namespace":"default", "name":"Contacts"},
                |"rowkey":"key",
                |"columns":{
                |"rowkey":{"cf":"rowkey", "col":"key", "type":"string"},
                |"officeAddress":{"cf":"Office", "col":"Address", "type":"string"},
                |"officePhone":{"cf":"Office", "col":"Phone", "type":"string"},
                |"personalName":{"cf":"Personal", "col":"Name", "type":"string"},
                |"personalPhone":{"cf":"Personal", "col":"Phone", "type":"string"}
                |}
            |}""".stripMargin

3. Geçici bir DataFrame sağlayan bir yöntemi tanımlamak, `Contacts` HBase tablosunda:

            def withCatalog(cat: String): DataFrame = {
                spark.sqlContext
                .read
                .options(Map(HBaseTableCatalog.tableCatalog->cat))
                .format("org.apache.spark.sql.execution.datasources.hbase")
                .load()
            }

4. DataFrame örneği oluşturun:

        val df = withCatalog(catalog)

5. Sorgu DataFrame:

        df.show()

6. İki veri satırını görmeniz gerekir:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Spark SQL kullanarak HBase tablo sorgulamak için geçici bir tablo kaydedin:

        df.registerTempTable("contacts")

8. Bir SQL sorgusunu sorun `contacts` tablosu:

        val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
        query.show()

9. Bu gibi sonuçları görmeniz gerekir:

        +-------------+--------------------+
        | personalName|       officeAddress|
        +-------------+--------------------+
        |    John Dole|1111 San Gabriel Dr.|
        |  Calvin Raji|5415 San Gabriel Dr.|
        +-------------+--------------------+

## <a name="insert-new-data"></a>Yeni veri Ekle

1. Yeni bir kişi kaydı eklemek için tanımlayan bir `ContactRecord` sınıfı:

        case class ContactRecord(
            rowkey: String,
            officeAddress: String,
            officePhone: String,
            personalName: String,
            personalPhone: String
            )

2. Bir örneğini oluşturmak `ContactRecord` ve bir dizi yerleştirin:

        val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

        var newData = new Array[ContactRecord](1)
        newData(0) = newContact

3. Yeni veri dizisi HBase için Kaydet:

        sc.parallelize(newData).toDF.write
        .options(Map(HBaseTableCatalog.tableCatalog -> catalog))
        .format("org.apache.spark.sql.execution.datasources.hbase").save()

4. Sonuçları inceleyin:
    
        df.show()

5. Şunun gibi bir çıktı görmeniz gerekir:

        +------+--------------------+--------------+------------+--------------+
        |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
        +------+--------------------+--------------+------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
        | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
        |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+------------+--------------+

## <a name="next-steps"></a>Sonraki adımlar

* [Spark HBase Bağlayıcısı](https://github.com/hortonworks-spark/shc)
