---
title: Azure HDInsight HBase veri - yazma ve okuma için Spark kullanma
description: HBase Spark Bağlayıcısı'nı okuyup bir HBase kümesi için Spark kümesinden veri yazmak için kullanın.
services: hdinsight
author: maxluk
ms.author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: 5123a95852fae58adf0b4a4684b012d3b9c71e3b
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144780"
---
# <a name="use-spark-to-read-and-write-hbase-data"></a>HBase verilerini okuyup yazmak için Spark kullanma

Apache HBase, genellikle düşük düzey API'si (taramaları, alır ve puts) veya Phoenix kullanarak bir SQL söz dizimi sorgulanır. Apache, uygun bir Spark HBase Bağlayıcısı ve yüksek performanslı alternatif HBase tarafından depolanan verileri sorgulamak ve değiştirmek için de sağlar.

## <a name="prerequisites"></a>Önkoşullar

* İki seçenek, HDInsight kümeleri, bir HBase ve bir Spark Spark yüklü 2.1 (HDInsight 3.6) ile ayırın.
* Önerilen yapılandırma, aynı sanal ağdaki her iki küme dağıtımı için en düşük gecikme süresiyle HBase kümesi ile doğrudan iletişim kurmak Spark kümesi gerekir. Daha fazla bilgi için [Azure portalını kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-portal.md).
* Her bir kümeye SSH erişimi.
* Her kümenin varsayılan depolama erişim.

## <a name="overall-process"></a>Genel işlem

HDInsight kümenizi sorgulamak Spark kümenizin etkinleştirmek için üst düzey işlem aşağıdaki gibidir:

1. Bazı örnek veriler HBase hazırlayın.
2. Hbase-site.xml dosyasının HBase kümesi yapılandırma klasörünüzden (/ etc/hbase/conf) alın.
3. Hbase-site.xml kopyasını Spark 2 yapılandırma klasörünüzde (/ etc/spark2/conf) yerleştirin.
4. Çalıştırma `spark-shell` Spark HBase bağlayıcı, Maven tarafından başvuran koordinatları içinde `packages` seçeneği.
5. HBase için Spark şemadan eşleyen bir katalog tanımlayın.
6. RDD ya da veri çerçevesi API'lerini kullanarak HBase verilerle etkileşim kurun.

## <a name="prepare-sample-data-in-hbase"></a>HBase örnek verileri hazırlama

Bu adımda, oluşturun ve ardından Spark kullanarak sorgulayabilirsiniz HBase basit bir tablodaki doldurun.

1. SSH kullanarak HBase kümenizin baş düğümüne bağlanın. Daha fazla bilgi için [SSH kullanarak HDInsight Bağlan](hdinsight-hadoop-linux-use-ssh-unix.md).
2. HBase kabuğunu çalıştırın:

        hbase shell

3. Oluşturma bir `Contacts` tablo sütun ailesi ile `Personal` ve `Office`:

        create 'Contacts', 'Personal', 'Office'

4. Birkaç örnek veri satırı yükle:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
        put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'

## <a name="acquire-hbase-sitexml-from-your-hbase-cluster"></a>Hbase-site.xml HBase kümenizden Al

1. SSH kullanarak HBase kümenizin baş düğümüne bağlanın.
2. Hbase-site.xml yerel depolama alanından HBase kümenin varsayılan depolama kök dizinine kopyalayın:

        hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml /

3. HBase kümesi kullanarak için gidin [Azure portalında](https://portal.azure.com).
4. Depolama hesabı seçin. 

    ![Depolama hesapları](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

5. Listedeki varsayılan sütunu altında bir onay işareti olan depolama hesabını seçin.

    ![Varsayılan depolama hesabı](./media/hdinsight-using-spark-query-hbase/default-storage.png)

6. Depolama hesabı bölmesinde, BLOB'ları kutucuğu seçin.

    ![Blobları kutucuğu](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

7. Kapsayıcılar listesinde, HBase kümesi tarafından kullanılan kapsayıcıyı seçin.
8. Dosya listesinde `hbase-site.xml`.

    ![HBase-site.xml](./media/hdinsight-using-spark-query-hbase/hbase-site-xml.png)

9. Blob özellikleri paneli seçin indirin ve kaydedin `hbase-site.xml` yerel makinenizde bir konuma.

    ![İndirme](./media/hdinsight-using-spark-query-hbase/download.png)

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Spark kümenizde hbase-site.xml yerleştirin

1. Spark kümesi kullanarak için gidin [Azure portalında](https://portal.azure.com).
2. Depolama hesabı seçin.

    ![Depolama hesapları](./media/hdinsight-using-spark-query-hbase/storage-accounts.png)

3. Listedeki varsayılan sütunu altında bir onay işareti olan depolama hesabını seçin.

    ![Varsayılan depolama hesabı](./media/hdinsight-using-spark-query-hbase/default-storage.png)

4. Depolama hesabı bölmesinde, BLOB'ları kutucuğu seçin.

    ![Blobları kutucuğu](./media/hdinsight-using-spark-query-hbase/blobs-tile.png)

5. Kapsayıcılar listesinde, Spark kümeniz tarafından kullanılan kapsayıcıyı seçin.
6. Yükleme'yi seçin.

    ![Karşıya Yükle](./media/hdinsight-using-spark-query-hbase/upload.png)

7. Seçin `hbase-site.xml` yerel makinenize daha önce indirilen dosya.

    ![Hbase-site.xml karşıya yükleme](./media/hdinsight-using-spark-query-hbase/upload-selection.png)

8. Yükleme'yi seçin.
9. SSH kullanarak Spark kümenizin baş düğümüne bağlanın.
10. Kopyalama `hbase-site.xml` Spark 2 yapılandırma klasörü yerel kümenin depolama üzerinde Spark kümenizin varsayılan depolama biriminden:

        sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Spark Shell başvuran HBase Spark Bağlayıcısı'nı çalıştırın

1. SSH kullanarak Spark kümenizin baş düğümüne bağlanın.
2. Spark, HBase Bağlayıcısı paket belirtme spark shell başlatın:

        spark-shell --packages com.hortonworks:shc-core:1.1.0-2.1-s_2.11 --repositories http://repo.hortonworks.com/content/groups/public/

3. Bu bir Spark Shell örneği açık tutun ve sonraki adıma devam edin.

## <a name="define-a-catalog-and-query"></a>Katalog ve sorgu tanımlama

Bu adımda, Spark şemadan HBase için eşleşen bir katalog nesnesi tanımlayın. 

1. Açık, Spark Shell'de aşağıdaki komutu çalıştırın. `import` ifadeleri:

        import org.apache.spark.sql.{SQLContext, _}
        import org.apache.spark.sql.execution.datasources.hbase._
        import org.apache.spark.{SparkConf, SparkContext}
        import spark.sqlContext.implicits._

2. Hbase'de oluşturduğunuz kişiler tablo için bir katalog tanımlayın:
    1. Adlı HBase tablo için bir katalog şema tanımlayabilir `Contacts`.
    2. Rowkey olarak tanımlamak `key`ve Spark, sütun ailesi, sütun adı ve Hbase'de kullanılan sütun türü için kullanılan sütun adlarını eşleyin.
    3. Rowkey da ayrıntılı bir adlandırılmış sütun olarak tanımlanması gerekir (`rowkey`), belirli bir sütun ailesi olduğu `cf` , `rowkey`.

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

3. Geçici bir DataFrame sağlayan bir yöntem tanımlayın, `Contacts` HBase Tablo:

            def withCatalog(cat: String): DataFrame = {
                spark.sqlContext
                .read
                .options(Map(HBaseTableCatalog.tableCatalog->cat))
                .format("org.apache.spark.sql.execution.datasources.hbase")
                .load()
            }

4. Bir örneği bir DataFrame oluşturun:

        val df = withCatalog(catalog)

5. Sorgu veri çerçevesi:

        df.show()

6. Veri iki satırı görmeniz gerekir:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Spark SQL kullanarak HBase tablosuyla sorgulayabilmesi geçici bir tabloya kaydedin:

        df.registerTempTable("contacts")

8. Bir SQL sorgusunu sorun `contacts` tablosu:

        val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
        query.show()

9. Buna benzer sonuçları görmeniz gerekir:

        +-------------+--------------------+
        | personalName|       officeAddress|
        +-------------+--------------------+
        |    John Dole|1111 San Gabriel Dr.|
        |  Calvin Raji|5415 San Gabriel Dr.|
        +-------------+--------------------+

## <a name="insert-new-data"></a>Yeni veri Ekle

1. Yeni bir kişi kaydı eklemek için tanımladığınız bir `ContactRecord` sınıfı:

        case class ContactRecord(
            rowkey: String,
            officeAddress: String,
            officePhone: String,
            personalName: String,
            personalPhone: String
            )

2. Bir örneğini oluşturmak `ContactRecord` ve bir dizi içinde yerleştirin:

        val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

        var newData = new Array[ContactRecord](1)
        newData(0) = newContact

3. Yeni veri dizisi için HBase kaydedin:

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

* [Spark, HBase Bağlayıcısı](https://github.com/hortonworks-spark/shc)
