---
title: Azure HDInsight HBase veri - yazma ve okuma için Spark kullanma
description: HBase Spark Bağlayıcısı'nı okuyup bir HBase kümesi için Spark kümesinden veri yazmak için kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: e3f5cb726dddbdbfbd1b1f48c800ac681e7a174c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696542"
---
# <a name="use-apache-spark-to-read-and-write-apache-hbase-data"></a>Apache HBase verilerini okuyup yazmak için Apache Spark kullanma

Apache HBase, genellikle düşük düzey API'si (taramaları, alır ve puts) veya Apache Phoenix kullanarak bir SQL söz dizimi sorgulanır. Apache, uygun bir Apache Spark HBase Bağlayıcısı ve yüksek performanslı alternatif HBase tarafından depolanan verileri sorgulamak ve değiştirmek için de sağlar.

## <a name="prerequisites"></a>Önkoşullar

* İki ayrı HDInsight kümeleri, bir HBase ve bir Spark ile en az Spark 2.1 yüklü (HDInsight 3.6).
* Önerilen yapılandırma, aynı sanal ağdaki her iki küme dağıtımı için en düşük gecikme süresiyle HBase kümesi ile doğrudan iletişim kurmak Spark kümesi gerekir. Daha fazla bilgi için [Azure portalını kullanarak HDInsight oluşturma Linux tabanlı kümelerde](hdinsight-hadoop-create-linux-clusters-portal.md).
* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).
* [URI şeması](hdinsight-hadoop-linux-information.md#URI-and-scheme) kümeleri birincil depolama alanı için. Bu wasb olacaktır: / / abfs olan Azure Blob Depolama için: / / Azure Data Lake depolama Gen2'ye veya adl: / / Azure Data Lake depolama Gen1. Güvenli aktarım için Blob Depolama veya Data Lake depolama Gen2 etkinse, URI wasbs olacaktır: / / ya da abfss: / /, sırasıyla ayrıca bakın [güvenli aktarım](../storage/common/storage-require-secure-transfer.md).


## <a name="overall-process"></a>Genel işlem

HDInsight kümenizi sorgulamak Spark kümenizin etkinleştirmek için üst düzey işlem aşağıdaki gibidir:

1. Bazı örnek veriler HBase hazırlayın.
2. Hbase-site.xml dosyasının HBase kümesi yapılandırma klasörünüzden (/ etc/hbase/conf) alın.
3. Hbase-site.xml kopyasını Spark 2 yapılandırma klasörünüzde (/ etc/spark2/conf) yerleştirin.
4. Çalıştırma `spark-shell` Spark HBase bağlayıcı, Maven tarafından başvuran koordinatları içinde `packages` seçeneği.
5. HBase için Spark şemadan eşleyen bir katalog tanımlayın.
6. RDD ya da veri çerçevesi API'lerini kullanarak HBase verilerle etkileşim kurun.

## <a name="prepare-sample-data-in-apache-hbase"></a>Apache HBase, örnek verileri hazırlama

Bu adımda, oluşturun ve Apache HBase, Spark'ı kullanarak ardından sorgulayabilirsiniz basit bir tablodaki doldurun.

1. SSH kullanarak HBase kümenizin baş düğümüne bağlanın. Daha fazla bilgi için [SSH kullanarak HDInsight Bağlan](hdinsight-hadoop-linux-use-ssh-unix.md).  Aşağıdaki komutta değiştirerek Düzenle `HBASECLUSTER` HBase kümenizin adıyla `sshuser` ile ssh kullanıcı hesabı adı ve ardından komutu girin.

    ```
    ssh sshuser@HBASECLUSTER-ssh.azurehdinsight.net
    ```

2. HBase Kabuğu'nu başlatmak için aşağıdaki komutu girin:

        hbase shell

3. Oluşturmak için aşağıdaki komutu girin bir `Contacts` tablo sütun ailesi ile `Personal` ve `Office`:

        create 'Contacts', 'Personal', 'Office'

4. Birkaç örnek satırlar veri yüklemek için aşağıdaki komutları girin:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
        put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
        put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'

5. HBase Kabuğu'ndan çıkmak için aşağıdaki komutu girin:

        exit 

## <a name="copy-hbase-sitexml-to-spark-cluster"></a>Hbase-site.xml Spark kümesine kopyalama
Hbase-site.xml yerel depolama alanından Spark kümenizin varsayılan depolama kök dizinine kopyalayın.  Yapılandırmanızı yansıtacak şekilde aşağıdaki komutu düzenleyin.  Ardından, açık SSH oturumundan HBase kümesi için aşağıdaki komutu girin:

| Söz dizimi değeri | Yeni değer|
|---|---|
|[URI şeması](hdinsight-hadoop-linux-information.md#URI-and-scheme) | Depolama alanınızı yansıtacak şekilde değiştirin.  Aşağıdaki blob depolama alanı için güvenli aktarım özellikli sözdizimidir.|
|`SPARK_STORAGE_CONTAINER`|Spark kümesi için kullanılan varsayılan depolama kapsayıcısı adı ile değiştirin.|
|`SPARK_STORAGE_ACCOUNT`|Spark kümesi için kullanılan varsayılan depolama hesabı adı ile değiştirin.|

```
hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml wasbs://SPARK_STORAGE_CONTAINER@SPARK_STORAGE_ACCOUNT.blob.core.windows.net/
```

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Spark kümenizde hbase-site.xml yerleştirin

1. SSH kullanarak Spark kümenizin baş düğümüne bağlanın.

2. Kopyalamak için aşağıdaki komutu girin `hbase-site.xml` Spark 2 yapılandırma klasörü yerel kümenin depolama üzerinde Spark kümenizin varsayılan depolama biriminden:

        sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Spark Shell başvuran HBase Spark Bağlayıcısı'nı çalıştırın

1. Açık SSH oturumundan Spark kümesi için bir spark shell başlatmak için aşağıdaki komutu girin:

    ```
    spark-shell --packages com.hortonworks:shc-core:1.1.1-2.1-s_2.11 --repositories https://repo.hortonworks.com/content/groups/public/
    ```  

2. Bu bir Spark Shell örneği açık tutun ve sonraki adıma devam edin.

## <a name="define-a-catalog-and-query"></a>Katalog ve sorgu tanımlama

Bu adımda, Apache HBase için Apache Spark şemadan eşleyen bir katalog nesnesi tanımlayın.  

1. Açık Spark Kabuğunuzda aşağıdakileri girin `import` ifadeleri:

    ```scala
    import org.apache.spark.sql.{SQLContext, _}
    import org.apache.spark.sql.execution.datasources.hbase._
    import org.apache.spark.{SparkConf, SparkContext}
    import spark.sqlContext.implicits._
    ```  

2. Hbase'de oluşturduğunuz kişiler tablo için bir katalog tanımlamak için aşağıdaki komutu girin:

    ```scala
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
    ```

    Kod aşağıdakileri gerçekleştirir:  

     a. Adlı HBase tablo için bir katalog şema tanımlayabilir `Contacts`.  
     b. Rowkey olarak tanımlamak `key`ve Spark, sütun ailesi, sütun adı ve Hbase'de kullanılan sütun türü için kullanılan sütun adlarını eşleyin.  
     c. Rowkey da ayrıntılı bir adlandırılmış sütun olarak tanımlanması gerekir (`rowkey`), belirli bir sütun ailesi olduğu `cf` , `rowkey`.  

3. Geçici bir DataFrame sağlayan bir yöntemi tanımlamak için aşağıdaki komutu girin, `Contacts` HBase Tablo:

    ```scala
    def withCatalog(cat: String): DataFrame = {
        spark.sqlContext
        .read
        .options(Map(HBaseTableCatalog.tableCatalog->cat))
        .format("org.apache.spark.sql.execution.datasources.hbase")
        .load()
     }
    ```

4. Bir örneği bir DataFrame oluşturun:

    ```scala
    val df = withCatalog(catalog)
    ```  

5. Sorgu veri çerçevesi:

    ```scala
    df.show()
    ```

6. Veri iki satırı görmeniz gerekir:

        +------+--------------------+--------------+-------------+--------------+
        |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
        +------+--------------------+--------------+-------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
        |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+-------------+--------------+

7. Spark SQL kullanarak HBase tablosuyla sorgulayabilmesi geçici bir tabloya kaydedin:

    ```scala
    df.createTempView("contacts")
    ```

8. Bir SQL sorgusunu sorun `contacts` tablosu:

    ```scala
    val query = spark.sqlContext.sql("select personalName, officeAddress from contacts")
    query.show()
    ```

9. Buna benzer sonuçları görmeniz gerekir:

        +-------------+--------------------+
        | personalName|       officeAddress|
        +-------------+--------------------+
        |    John Dole|1111 San Gabriel Dr.|
        |  Calvin Raji|5415 San Gabriel Dr.|
        +-------------+--------------------+

## <a name="insert-new-data"></a>Yeni veri Ekle

1. Yeni bir kişi kaydı eklemek için tanımladığınız bir `ContactRecord` sınıfı:

    ```scala
    case class ContactRecord(
        rowkey: String,
        officeAddress: String,
        officePhone: String,
        personalName: String,
        personalPhone: String
        )
    ```

2. Bir örneğini oluşturmak `ContactRecord` ve bir dizi içinde yerleştirin:

    ```scala
    val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

    var newData = new Array[ContactRecord](1)
    newData(0) = newContact
    ```

3. Yeni veri dizisi için HBase kaydedin:

    ```scala
    sc.parallelize(newData).toDF.write.options(Map(HBaseTableCatalog.tableCatalog -> catalog, HBaseTableCatalog.newTable -> "5")).format("org.apache.spark.sql.execution.datasources.hbase").save()
    ```

4. Sonuçları inceleyin:

    ```scala  
    df.show()
    ```

5. Şunun gibi bir çıktı görmeniz gerekir:

        +------+--------------------+--------------+------------+--------------+
        |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
        +------+--------------------+--------------+------------+--------------+
        |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
        | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
        |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
        +------+--------------------+--------------+------------+--------------+

## <a name="next-steps"></a>Sonraki adımlar

* [HBase bağlayıcı Apache Spark](https://github.com/hortonworks-spark/shc)
