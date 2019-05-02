---
title: Hive ambarı Bağlayıcısı ile Apache Hive ve Apache Spark'ı tümleştirme
description: Apache Spark ve Apache Hive, Azure HDInsight Hive ambarı bağlayıcı ile tümleştirmeyi öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 04/18/2019
ms.openlocfilehash: b450fe763104adbbd08e4b5f362bd51ffbf82c81
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64729051"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Hive ambarı Bağlayıcısı ile Apache Hive ve Apache Spark'ı tümleştirme

Apache Hive ambarı Bağlayıcısı (HWC), Apache Hive ve Apache Spark ile DataFrames Spark ve Hive tabloları arasında veri taşıma ve ayrıca Hive tablolarına akış verileri Spark yönlendiren gibi görevleri destekleyerek daha kolayca çalışmanıza olanak sağlayan bir kitaplıktır. Spark ve Hive arasında bir köprü gibi Hive ambarı Bağlayıcısı çalışır. Scala, Java ve Python için geliştirmeyi destekler.

Hive ambarı Bağlayıcısı güçlü büyük veri uygulamaları oluşturmak için Hive'ı ve Spark'ın benzersiz özelliklerinden yararlanmanıza olanak sağlar. Apache Hive veritabanı işlemler atomik, Consistent, Isolated ve dayanıklı (ACID) için destek sunar. ACID hakkında daha fazla bilgi ve Hive işlemleri için bkz: [Hive işlemleri](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions). Hive, Apache Ranger ve düşük gecikme süresi analitik işleme Apache Spark, kullanılabilir değil aracılığıyla ayrıntılı güvenlik denetimleri de sunar.

Apache Spark bir yapılandırılmış Akış akış özellikleri kullanılabilir değil, Apache Hive veren API vardır. Hortonworks Data Platform (HDP) 3.0 ile başlayarak, Apache Spark ve Apache Hive, birlikte çalışabilirlik zorlaştırabilir ayrı meta depolar vardır. Hive ambarı Bağlayıcısı, Spark ve Hive birlikte kullanmayı kolaylaştırır. HWC kitaplığı paralel olarak daha etkili ve ölçeklenebilir bir standart Hive JDBC bağlantı spark'tan kullanmaktan yapmadan Spark Yürütücü için LLAP Daemon'ları verileri yükler.

![Mimari](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Hive ambarı Bağlayıcısı tarafından desteklenen operations bazıları şunlardır:

* Açıklayan bir tablo
* ORC biçimlendirilmiş veriler için tablo oluşturma
* Hive veri seçme ve bir DataFrame alınıyor
* Bir veri çerçevesi için Hive toplu işlemde yazma
* Bir Hive update deyimi yürütülüyor
* Tablo verilerini kovanından okuma, Spark, dönüştürme ve yeni bir Hive tablosuna yazma
* Bir DataFrame veya Spark akışı HiveStreaming kullanarak Hive için yazma

## <a name="hive-warehouse-connector-setup"></a>Hive ambarı bağlayıcı Kurulumu

Bir Azure HDInsight Spark ve etkileşimli sorgu kümesi arasında Hive ambarı bağlayıcısını ayarlamak için aşağıdaki adımları izleyin:

1. Bir depolama hesabı ve özel bir Azure sanal ağ ile Azure portalını kullanarak bir HDInsight Spark 4.0 kümesi oluşturun. Bir Azure sanal ağında bir küme oluşturma hakkında daha fazla bilgi için bkz: [var olan bir sanal ağa ekleme HDInsight](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md#existingvnet).
1. Spark kümesi olarak aynı depolama hesabını ve Azure sanal ağ ile Azure portalını kullanarak bir HDInsight etkileşimli sorgu (LLAP) 4.0 kümesi oluşturun.
1. İçeriğini kopyalayın `/etc/hosts` için etkileşimli sorgu kümenizin headnode0 dosyada `/etc/hosts` Spark kümenizin headnode0 dosyası. Bu adım, etkileşimli sorgu kümesi düğümlerin IP adreslerini çözümlemeye Spark kümenizin izin verir. Güncelleştirilmiş dosyanın içeriğini görüntülemek `cat /etc/hosts`. Çıktı aşağıdaki ekran görüntüsünde gösterilen gibi görünmelidir.

    ![hosts dosyasını görüntüleme](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

1. Aşağıdaki adımları uygulayarak Spark küme ayarları yapılandırın: 
    1. Azure portalına gidin, HDInsight kümeleri seçin ve ardından, küme adına tıklayın.
    1. Sağ taraftaki altında **küme panoları**seçin **Ambari giriş**.
    1. Ambari web kullanıcı Arabirimi, tıklayın **SPARK2** > **YAPILANDIRMALARI** > **özel spark2-varsayılanları**.

        ![Spark2 Ambari yapılandırma](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

    1. Ayarlama `spark.hadoop.hive.llap.daemon.service.hosts` özelliği aynı değere **LLAP uygulama adı** altında **hive etkileşimli-env Gelişmiş**. Örneğin, `@llap0`

    1. Ayarlama `spark.sql.hive.hiveserver2.jdbc.url` üzerinde etkileşimli sorgu kümesi Hiveserver2 ile bağlanan JDBC bağlantı dizesi. Kümeniz için bağlantı dizesi, URI aşağıdaki gibi görünür. `CLUSTERNAME` Spark kümenizin adıdır ve `user` ve `password` parametreleri, kümeniz için doğru değerlere ayarlanır.

        ```
        jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2
        ```

        >[!Note] 
        > JDBC URL Hiveserver2 ile bağlanmak için kimlik bilgilerini içermesi gereken bir kullanıcı adı ve parola da dahil olmak üzere.

    1. Ayarlama `spark.datasource.hive.warehouse.load.staging.dir` uygun HDFS uyumlu hazırlama dizinine. İki farklı küme varsa HiveServer2 erişimi olduğunu hazırlama dizini LLAP kümenin depolama hesabının hazırlama dizinindeki bir klasör olmalıdır. Örneğin, `wasb://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp` burada `STORAGE_ACCOUNT_NAME` küme tarafından kullanılan depolama hesabı adı ve `STORAGE_CONTAINER_NAME` depolama kapsayıcısının adı.

    1. Ayarlama `spark.datasource.hive.warehouse.metastoreUri` meta veri deposu URI'sini etkileşimli sorgu kümesi değerine sahip. LLAP kümeniz için metastoreUri bulmak için Ara **hive.metastore.uris** Ambari UI, LLAP için bir özellik kümesi altında **Hive** > **Gelişmiş**  >  **Genel**. Değer, aşağıdaki URI aşağıdakine benzer:

        ```
        thrift://hn0-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083,
        thrift://hn1-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083
        ```

    1. Ayarlama `spark.security.credentials.hiveserver2.enabled` için `false` modu için YARN istemci dağıtma.
    1. Ayarlama `spark.hadoop.hive.zookeeper.quorum` LLAP kümenizin Zookeeper çekirdek için. Zookeeper çekirdek LLAP kümeniz için bulmak için Ara **hive.zookeeper.quorum** Ambari UI, LLAP için bir özellik kümesi altında **Hive** > **Gelişmiş**  >  **Hive site Gelişmiş**. Değer, aşağıdaki dize aşağıdakine benzer:

        ```
        zk1-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk4-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk6-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181
        ```

Hive ambarı Bağlayıcınızı yapılandırmasını test etmek için bu bölümdeki adımları [bağlanan ve çalışan sorguların](#connecting-and-running-queries).

## <a name="using-the-hive-warehouse-connector"></a>Hive ambarı Bağlayıcısı'nı kullanma

### <a name="connecting-and-running-queries"></a>Bağlanma ve sorgu çalıştırma

Etkileşimli sorgu kümenize bağlanmak ve Hive ambarı Bağlayıcısı'nı kullanarak sorguları yürütmek için birkaç farklı yöntemleri arasında seçim yapabilirsiniz. Desteklenen yöntemler aşağıdaki araçlar şunlardır:

* [Spark-shell](../spark/apache-spark-shell.md)
* PySpark
* Spark-submit
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Bu makaledeki tüm örnekleri spark-shell üzerinden yürütülür.

Spark-shell oturumu başlatmak için aşağıdaki adımları uygulayın:

1. SSH kümenizin baş düğümüne uygulayın. Kümenize SSH ile bağlanma hakkında daha fazla bilgi için bkz. [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
1. Doğru dizinde oturum yazarak değiştirme `cd /usr/hdp/current/hive_warehouse_connector` veya spark-shell komutu parametre olarak kullanılan tüm jar dosyalarının tam yolunu sağlayın.
1. Spark Kabuğu'nu başlatmak için aşağıdaki komutu girin:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

1. Bir karşılama iletisi görürsünüz ve `scala>` komutları girebildiğiniz istemi.

1. Spark-shell başlattıktan sonra bir Hive ambarı bağlayıcı örneği aşağıdaki komutları kullanarak başlatılabilir:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Bağlanarak ve sorgular kümelerine Kurumsal güvenlik paketi (ESP)

Kurumsal güvenlik paketi (ESP), Active Directory tabanlı kimlik doğrulaması, birden çok kullanıcı desteği ve Azure HDInsight, Apache Hadoop kümeleri için rol tabanlı erişim denetimi gibi kurumsal sınıf özellikler sağlar. ESP hakkında daha fazla bilgi için bkz. [Kurumsal güvenlik paketi ile Apache Hadoop güvenliğine giriş](../domain-joined/apache-domain-joined-introduction.md).

1. İlk 1 ve 2 altında adımları [bağlanan ve çalışan sorguların](#connecting-and-running-queries).
1. Tür `kinit` ve bir etki alanı kullanıcısı ile oturum açın.
1. Başlangıç spark-shell ile tam listesini aşağıda gösterildiği gibi yapılandırma parametreleri. Tüm değerleri açılı ayraçlar arasındaki tüm büyük harfleri, kümenizde tabanlı belirtilmelidir. Herhangi bir parametre aşağıdaki giriş değerleri bulmanız gerekiyorsa, bölümüne bakın [Hive ambarı bağlayıcı Kurulumu](#hive-warehouse-connector-setup).:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    --conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
    --conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
    --conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
    --conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
    --conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
   ```

### <a name="creating-spark-dataframes-from-hive-queries"></a>Spark veri çerçevelerini Hive sorguları oluşturma

HWC kitaplığını kullanarak tüm sorguların sonuçlarını bir veri çerçevesi döndürülür. Aşağıdaki örnekler temel sorgu oluşturma işlemini göstermektedir.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

Sorgu sonuçlarını Spark kitaplıklar MLIB ve SparkSQL gibi kullanılabilir Spark DataFrames ' dir.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Hive tablolarını Spark veri çerçevelerini çıkış yazma

Spark, Hive'nın yönetilen ACID tablolarını yazmak yerel olarak desteklemiyor. HWC kullanarak, ancak herhangi bir veri çerçevesi bir Hive tablosuna dışarı yazabileceğiniz. Bu işlev aşağıdaki örnekte iş görebilirsiniz:

1. Adlı bir tablo oluşturmak `sampletable_colorado` ve aşağıdaki komutu kullanarak sütunlarını belirtin:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

2. Tabloyu filtreleyip `hivesampletable` nerede sütunu `state` eşittir `Colorado`. Hive tablo bu sorgu, bir Spark DataFrame döndürülür. Hive tablosunda veri çerçevesi kaydedildikten sonra `sampletable_colorado` kullanarak `write` işlevi.
    
    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

Sonuçta elde edilen tabloda aşağıdaki ekran görüntüsünde görebilirsiniz.

![Sonuçta elde edilen Tablo Göster](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Yapılandırılmış akış yazma

Hive ambarı Bağlayıcısı'nı kullanarak, Spark, Hive tablolarına veri yazmak için akış kullanabilirsiniz.

Verilerini bir Hive tablosuna 9999 localhost bağlantı noktası üzerinde bir Spark akışı'ndan alır bir Hive ambarı bağlayıcısı örneği oluşturmak için aşağıdaki adımları izleyin.

1. Spark kümenizde bir terminal açın.
1. Aşağıdaki komutla spark akışı başlayın:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9988).load()
    ```

1. Veriler için aşağıdaki adımları uygulayarak, oluşturduğunuz Spark akışı oluştur:
    1. Başka aynı Spark kümesinde terminal açın.
    1. Komut isteminde `nc -lk 9999`. Bu komut, komut satırından belirtilen bağlantı noktasına veri göndermesini netcat yardımcı programını kullanır.
    1. Başı tarafından izlenen Spark akış alımı için istediğiniz sözcükleri yazın.
        ![Giriş verileri için spark akışı](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark-stream-data-input.png)
1. Akış verileri tutmak için yeni bir Hive tablosu oluşturur. Spark-shell aşağıdaki komutları yazın:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. Veri akışı aşağıdaki komutu kullanarak yeni oluşturduğunuz tabloya yazma:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > `metastoreUri` Ve `database` seçenekleri şu anda ayarlanmalıdır el ile Apache Spark için bilinen bir sorun nedeniyle. Bu sorun hakkında daha fazla bilgi için bkz. [SPARK 25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. Aşağıdaki komutla tabloya eklenen verilere görüntüleyebilirsiniz:

    ```scala
    hive.table("stream_table").show()
    ```

### <a name="securing-data-on-spark-esp-clusters"></a>ESP Spark kümelerinde verilerin güvenliğini sağlama

1. Tablo oluşturma `demo` aşağıdaki komutları girerek bazı örnek verilerle:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Aşağıdaki komutla tablonun içeriğini görüntüleyin. İlke, uygulamadan önce `demo` tablo tam sütun gösterir.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![ranger İlkesi uygulanmadan önce tanıtım tablo](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Bir sütuna maskeleme yalnızca son dört karakterini sütunun gösteren ilke uygulayın.  
    1. Ranger yönetici Arabirimine Git `https://CLUSTERNAME.azurehdinsight.net/ranger/`.
    1. Hive hizmet kümenizin tıklayın **Hive**.
        ![ranger İlkesi uygulanmadan önce tanıtım tablo](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. Tıklayarak **maskeleme** sekmesini ve ardından **Add New Policy** ![ilke listesi](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)
    1. İstenen ilke adı belirtin. Veritabanı seçin: **Varsayılan**, Hive tablosu: **tanıtım**, Hive sütun: **adı**, kullanıcı: **rsadmin2**, erişim türleri: **seçin**ve **Kısmi maskesi: son 4 Göster** gelen **maskeleme seçenek** menüsü. **Ekle**'ye tıklayın.
                ![ilke listesi](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. Tablonun içeriğini yeniden görüntüleyin. Ranger İlkesi uygulandıktan sonra yalnızca son dört karakterini sütunun görebiliriz.

    ![ranger İlkesi uygulandıktan sonra tanıtım tablo](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight ile etkileşimli Sorgu'yu kullanma](https://docs.microsoft.com/azure/hdinsight/interactive-query/apache-interactive-query-get-started)
* [Zeppelin, Livy, kullanarak Hive ambarı bağlayıcısıyla etkileşim örnekleri spark-submit ve pyspark](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
