---
title: 'Öğretici: Yapılandırılmış akış ile Apache Kafka - Azure HDInsight, Apache Spark'
description: Apache Spark akışını kullanarak Apache Kafka içine veya dışına veri almayı öğrenin. Bu öğreticide, HDInsight üzerinde Spark’tan bir Jupyter not defterini kullanarak verilerinizi akışla aktaracaksınız.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: tutorial
ms.date: 05/22/2019
ms.author: hrasheed
ms.openlocfilehash: 51f84234ac35be5f60d1aaa5dac661ad9ce5e0c2
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257895"
---
# <a name="tutorial-use-apache-spark-structured-streaming-with-apache-kafka-on-hdinsight"></a>Öğretici: Apache Spark yapılandırılmış akışını HDInsight üzerinde Apache Kafka ile kullanma

Bu öğreticide nasıl kullanılacağını gösterir [Apache Spark yapılandırılmış akış](https://spark.apache.org/docs/latest/structured-streaming-programming-guide) ile veri okuma ve yazma için [Apache Kafka](https://kafka.apache.org/) Azure HDInsight üzerinde.

Spark yapılandırılmış akış Spark SQL üzerinde yerleşik bir akış işleme motorudur. Bu altyapıyı kullanarak, statik veriler üzerinde toplu hesaplamayla aynı şekilde akış hesaplamalarını ifade edebilirsiniz.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kümeleri oluşturmak için bir Azure Resource Manager şablonu kullanma
> * Spark yapılandırılmış akışı ile Kafka kullanın

Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümeleri silmeyi unutmayın.

## <a name="prerequisites"></a>Önkoşullar

* jq, bir komut satırı JSON işlemcisi'ni tıklatın.  Bkz: [ https://stedolan.github.io/jq/ ](https://stedolan.github.io/jq/).

* Kullanarak ile aşinalık [Jupyter not defterleri](https://jupyter.org/) HDInsight üzerinde Spark ile. Daha fazla bilgi için [veri yükleme ve HDInsight üzerinde Apache Spark ile sorguları çalıştırma](spark/apache-spark-load-data-run-query.md) belge.

* [Scala](https://www.scala-lang.org/) programlama dilini bilme. Bu öğreticide kullanılan kod, Scala dilinde yazılmıştır.

* Kafka konuları oluşturmayı bilme. Daha fazla bilgi için [hızlı HDInsight üzerinde Apache Kafka](kafka/apache-kafka-get-started.md) belge.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar hem HDInsight üzerinde Spark hem de HDInsight kümesi üzerinde Kafka içeren bir Azure kaynak grubu gerektirir. Bu kümelerin her ikisi de Spark kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanıyan bir Azure Sanal Ağı içinde bulunur.
> 
> Size kolaylık sağlamak için bu belgede, tüm gerekli Azure kaynaklarını oluşturabilecek bir şablonun bağlantıları sağlanır. 
>
> Sanal ağ üzerinde HDInsight kullanma hakkında daha fazla bilgi için [Sanal ağ kullanarak HDInsight’ı genişletme](hdinsight-extend-hadoop-virtual-network.md) belgesine bakın.

## <a name="structured-streaming-with-apache-kafka"></a>Apache Kafka ile structured Streaming

Spark Yapılandırılmış Akışı, Spark SQL altyapısı üzerinde derlenen bir akış işleme altyapısıdır. Yapılandırılmış Akış kullanırken, toplu sorgular yazdığınız şekilde akış sorguları yazabilirsiniz.

Aşağıdaki kod parçacıkları, Kafka’dan okuma ve dosyaya depolama işlemlerini gösterir. Birincisi bir toplu iş işlemi, ikincisiyse bir akış işlemidir:

```scala
// Read a batch from Kafka
val kafkaDF = spark.read.format("kafka")
                .option("kafka.bootstrap.servers", kafkaBrokers)
                .option("subscribe", kafkaTopic)
                .option("startingOffsets", "earliest")
                .load()

// Select data and write to file
kafkaDF.select(from_json(col("value").cast("string"), schema) as "trip")
                .write
                .format("parquet")
                .option("path","/example/batchtripdata")
                .option("checkpointLocation", "/batchcheckpoint")
                .save()
```

```scala
// Stream from Kafka
val kafkaStreamDF = spark.readStream.format("kafka")
                .option("kafka.bootstrap.servers", kafkaBrokers)
                .option("subscribe", kafkaTopic)
                .option("startingOffsets", "earliest")
                .load()

// Select data from the stream and write to file
kafkaStreamDF.select(from_json(col("value").cast("string"), schema) as "trip")
                .writeStream
                .format("parquet")
                .option("path","/example/streamingtripdata")
                .option("checkpointLocation", "/streamcheckpoint")
                .start.awaitTermination(30000)
```

Her iki kod parçacığında Kafka’dan veriler okunur ve dosyaya yazılır. Örnekler arasındaki farklar şunlardır:

| Yığın | Akış |
| --- | --- |
| `read` | `readStream` |
| `write` | `writeStream` |
| `save` | `start` |

Akış işlemi de kullanır `awaitTermination(30000)`, 30.000 ms sonra akış durdurur. 

Kafka ile Yapılandırılmış Akışı kullanmak için projenizin `org.apache.spark : spark-sql-kafka-0-10_2.11` paketinde bir bağımlılığı olmalıdır. Bu paketin sürümü, HDInsight üzerinde Spark sürümüyle eşleşmelidir. Spark 2.2.0 (HDInsight 3.6’da kullanılabilir) için, [https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar](https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar) adresinden farklı proje türleri için bağımlılık bilgilerini bulabilirsiniz.

Bu öğreticide kullanılan Jupyter Notebook için aşağıdaki hücre bu paket bağımlılığı yükler:

```
%%configure -f
{
    "conf": {
        "spark.jars.packages": "org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0",
        "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.11"
    }
}
```

## <a name="create-the-clusters"></a>Kümeleri oluşturma

HDInsight üzerinde Apache Kafka, genel internet üzerinden Kafka aracılarına erişim sağlamaz. Kafka kullanan her özellik aynı Azure sanal ağı içinde olmalıdır. Bu öğreticide hem Kafka hem de Spark kümeleri aynı Azure sanal ağı içinde yer alır. 

Aşağıdaki diyagramda Spark ile Kafka arasındaki iletişimin nasıl aktığı gösterilmektedir:

![Bir Azure sanal ağında Spark ve Kafka kümeleri diyagramı](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]  
> Kafka hizmeti, sanal ağ içindeki iletişimle sınırlıdır. SSH ve Ambari gibi küme üzerindeki diğer hizmetlere internet üzerinden erişilebilir. HDInsight üzerinde kullanılabilir olan genel bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight Tarafından Kullanılan Bağlantı Noktaları ve URI’ler](hdinsight-hadoop-port-settings-for-services.md).

Bir Azure Sanal Ağı oluşturmak ve sonra bunun içinde Kafka ve Spark kümeleri oluşturmak için aşağıdaki adımları kullanın:

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Azure Resource Manager şablonu **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json** sayfasında bulunur.

    Bu şablon aşağıdaki kaynakları oluşturur:

   * HDInsight 3.6 kümesi üzerinde bir Kafka.
   * HDInsight 3.6 kümesi üzerinde bir Spark 2.2.0.
   * HDInsight kümeleri içeren bir Azure Sanal Ağı.

     > [!IMPORTANT]  
     > Bu öğreticide kullanılan yapılandırılmış akış not defteri için HDInsight 3.6 üzerinde Spark 2.2.0 gerekir. HDInsight üzerinde Spark’ın daha önceki bir sürümünü kullanıyorsanız, not defterini kullanırken hatalarla karşılaşırsınız.

2. **Özelleştirilmiş şablon** bölümündeki girişleri doldurmak için aşağıdaki bilgileri kullanın:

    | Ayar | Değer |
    | --- | --- |
    | Abonelik | Azure aboneliğiniz |
    | Kaynak grubu | Kaynakları içeren kaynak grubu. |
    | Location | İçinde kaynakların oluşturulduğu Azure bölgesi. |
    | Spark Kümesi Adı | Spark kümesinin adı. İlk altı karakter Kafka küme adından farklı olmalıdır. |
    | Kafka Kümesi Adı | Kafka kümesinin adı. İlk altı karakter Spark küme adından farklı olmalıdır. |
    | Küme Oturum Açma Kullanıcı Adı | Kümeler için yönetici kullanıcı adı. |
    | Küme Oturum Açma Parolası | Kümeler için yönetici kullanıcı parolası. |
    | SSH Kullanıcı Adı | Kümeler için oluşturulacak SSH kullanıcısı. |
    | SSH Parolası | SSH kullanıcısı için parola. |
   
    ![Özelleştirilmiş şablonun ekran görüntüsü](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

3. **Hüküm ve Koşullar**’ı okuyun ve ardından **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**’u seçin

4. Son olarak, **Panoya sabitle**’yi işaretleyin ve **Satın Al**’ı seçin. 

> [!NOTE]  
> Kümelerin oluşturulması 20 dakikaya kadar sürebilir.

## <a name="use-spark-structured-streaming"></a>Spark yapılandırılmış akışını kullanma

Bu örnekte, HDInsight üzerinde Kafka ile Spark yapılandırılmış akışını kullanmayı gösterilmiştir. Veri taksi gelişlerin üzerinde New York City tarafından sağlanan kullanır.  Bu not defteri tarafından kullanılan veri kümesini dandır [2016 yeşil taksi seyahat verilerini](https://data.cityofnewyork.us/Transportation/2016-Green-Taxi-Trip-Data/hvrh-b6nb).

1. Konak bilgilerini toplayın. Curl kullanma ve [jq](https://stedolan.github.io/jq/) edinmek, Kafka ZooKeeper ve aracı konakların bilgilerini için aşağıdaki komutları. Komutları için Windows komut istemi tasarlanmıştır, küçük farklılıklar diğer ortamları için gereklidir. Değiştirin `KafkaCluster` ile Kafka kümenizin adını ve `KafkaPassword` ile küme oturum açma parolası. Ayrıca, değiştirin `C:\HDI\jq-win64.exe` jq yüklemenizi gerçek yoluyla. Bir Windows komut isteminde komutların girin ve sonraki adımlarda kullanmak için çıkış kaydedin.

    ```cmd
    set CLUSTERNAME=KafkaCluster
    set PASSWORD=KafkaPassword
    
    curl -u admin:%PASSWORD% -G "https://%CLUSTERNAME%.azurehdinsight.net/api/v1/clusters/%CLUSTERNAME%/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | C:\HDI\jq-win64.exe -r "["""\(.host_components[].HostRoles.host_name):2181"""] | join(""",""")"
    
    curl -u admin:%PASSWORD% -G "https://%CLUSTERNAME%.azurehdinsight.net/api/v1/clusters/%CLUSTERNAME%/services/KAFKA/components/KAFKA_BROKER" | C:\HDI\jq-win64.exe -r "["""\(.host_components[].HostRoles.host_name):9092"""] | join(""",""")"
    ```

2. Web tarayıcınızdan Spark kümeniz üzerindeki Jupyter not defterine bağlanın. Aşağıdaki URL’de `CLUSTERNAME` değerini __Spark__ kümenizin adıyla değiştirin:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Sorulduğunda, kümeyi oluştururken kullanılan küme kullanıcı adı (yönetici) ve parolasını girin.

3. Seçin **yeni > Spark** bir not defteri oluşturmak için.

4. Not Not Defteri hücreye aşağıdaki bilgileri girerek kullanılan paketleri yükleyin. Komutunu kullanarak çalıştırın **CTRL + ENTER**.

    ```
    %%configure -f
    {
        "conf": {
            "spark.jars.packages": "org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0",
            "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.11"
        }
    }
    ```

5. Kafka konu oluşturun. Aşağıdaki komutta değiştirerek Düzenle `YOUR_ZOOKEEPER_HOSTS` içeren Zookeeper konak ilk adımda ayıklanan bilgileri. Düzenlenen komutu oluşturmak için Jupyter not defterinde girin `tripdata` konu.

    ```scala
    %%bash
    export KafkaZookeepers="YOUR_ZOOKEEPER_HOSTS"

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic tripdata --zookeeper $KafkaZookeepers
    ```

6. Taksi dönüş verileri alın. Sonraki hücrenin da oturan gelişlerin taksi verileri yüklemek için komutu girin. Veriler bir dataframe'e yüklendikten ve ardından veri çerçevesi hücre çıktı olarak görüntülenir.

    ```scala
    import spark.implicits._

    // Load the data from the New York City Taxi data REST API for 2016 Green Taxi Trip Data
    val url="https://data.cityofnewyork.us/resource/pqfs-mqru.json"
    val result = scala.io.Source.fromURL(url).mkString
    
    // Create a dataframe from the JSON data
    val taxiDF = spark.read.json(Seq(result).toDS)
    
    // Display the dataframe containing trip data
    taxiDF.show()
    ```

7. Kafka aracı konak bilgilerini ayarlayın. Değiştirin `YOUR_KAFKA_BROKER_HOSTS` 1. adımda ayıkladığınız aracı konakların bilgilerini ile.  Jupyter Not Defteri içinde sonraki hücreye düzenlenen komutu girin.

    ```scala
    // The Kafka broker hosts and topic used to write to Kafka
    val kafkaBrokers="YOUR_KAFKA_BROKER_HOSTS"
    val kafkaTopic="tripdata"
    
    println("Finished setting Kafka broker and topic configuration.")
    ```

8. Kafka için gereken verileri gönderebilirsiniz. Aşağıdaki komutta `vendorid` alan Kafka ileti için anahtar değeri kullanılır. Anahtar, veriler bölümlenirken Kafka tarafından kullanılır. Tüm alanları Kafka iletisinde JSON dize değeri olarak depolanır. Jupyter kullanarak batch sorgu Kafka için verileri kaydetmek için aşağıdaki komutu girin.

    ```scala
    // Select the vendorid as the key and save the JSON string as the value.
    val query = taxiDF.selectExpr("CAST(vendorid AS STRING) as key", "to_JSON(struct(*)) AS value").write.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("topic", kafkaTopic).save()

    println("Data sent to Kafka")
    ```

9. Bir şema bildirin. Aşağıdaki komut, JSON verileri kafka'dan okunurken bir şema nasıl yapılacağı açıklanır. Sonraki Jupyter hücreye komutu girin.

    ```scala
    // Import bits useed for declaring schemas and working with JSON data
    import org.apache.spark.sql._
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._
    
    // Define a schema for the data
    val schema = (new StructType).add("dropoff_latitude", StringType).add("dropoff_longitude", StringType).add("extra", StringType).add("fare_amount", StringType).add("improvement_surcharge", StringType).add("lpep_dropoff_datetime", StringType).add("lpep_pickup_datetime", StringType).add("mta_tax", StringType).add("passenger_count", StringType).add("payment_type", StringType).add("pickup_latitude", StringType).add("pickup_longitude", StringType).add("ratecodeid", StringType).add("store_and_fwd_flag", StringType).add("tip_amount", StringType).add("tolls_amount", StringType).add("total_amount", StringType).add("trip_distance", StringType).add("trip_type", StringType).add("vendorid", StringType)
    // Reproduced here for readability
    //val schema = (new StructType)
    //   .add("dropoff_latitude", StringType)
    //   .add("dropoff_longitude", StringType)
    //   .add("extra", StringType)
    //   .add("fare_amount", StringType)
    //   .add("improvement_surcharge", StringType)
    //   .add("lpep_dropoff_datetime", StringType)
    //   .add("lpep_pickup_datetime", StringType)
    //   .add("mta_tax", StringType)
    //   .add("passenger_count", StringType)
    //   .add("payment_type", StringType)
    //   .add("pickup_latitude", StringType)
    //   .add("pickup_longitude", StringType)
    //   .add("ratecodeid", StringType)
    //   .add("store_and_fwd_flag", StringType)
    //   .add("tip_amount", StringType)
    //   .add("tolls_amount", StringType)
    //   .add("total_amount", StringType)
    //   .add("trip_distance", StringType)
    //   .add("trip_type", StringType)
    //   .add("vendorid", StringType)
    
    println("Schema declared")
    ```

10. Verileri seçin ve akış'ı başlatın. Aşağıdaki komutu kullanarak bir batch sorgu kafka'dan veri almayı gösterir ve sonra sonuçları HDFS'ye Spark kümesinde yazamadı. Bu örnekte, `select` Kafka'dan (değer alanı) iletiyi alır ve şema uygular. Veriler, ardından HDFS'ye (WASB veya ADL) parquet biçiminde yazılır. Sonraki Jupyter hücreye komutu girin.

    ```scala
    // Read a batch from Kafka
    val kafkaDF = spark.read.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("subscribe", kafkaTopic).option("startingOffsets", "earliest").load()
    
    // Select data and write to file
    val query = kafkaDF.select(from_json(col("value").cast("string"), schema) as "trip").write.format("parquet").option("path","/example/batchtripdata").option("checkpointLocation", "/batchcheckpoint").save()
    
    println("Wrote data to file")
    ```

11. Dosyaları sonraki Jupyter hücreye komutunu girerek oluşturulduğunu doğrulayabilirsiniz. Dosyaları listeler `/example/batchtripdata` dizin.

    ```scala
    %%bash
    hdfs dfs -ls /example/batchtripdata
    ```

12. Önceki örnekte kullanılan bir toplu işlem sorguları, ancak aşağıdaki komutu bir akış sorgu kullanarak aynı şeyi yapmak nasıl gösterir. Sonraki Jupyter hücreye komutu girin.

    ```scala
    // Stream from Kafka
    val kafkaStreamDF = spark.readStream.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("subscribe", kafkaTopic).option("startingOffsets", "earliest").load()
    
    // Select data from the stream and write to file
    kafkaStreamDF.select(from_json(col("value").cast("string"), schema) as "trip").writeStream.format("parquet").option("path","/example/streamingtripdata").option("checkpointLocation", "/streamcheckpoint").start.awaitTermination(30000)
    println("Wrote data to file")
    ```

13. Dosyaları akış sorgu tarafından yazılmış doğrulamak için aşağıdaki hücre çalıştırın.

    ```scala
    %%bash
    hdfs dfs -ls /example/streamingtripdata
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve sonra __Kaynak Grupları__'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

> [!WARNING]  
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz.
> 
> HDInsight üzerinde Kafka kümesinin silinmesi Kafka’da depolanmış tüm verileri siler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici sayesinde nasıl kullanacağınızı öğrendiniz [Apache Spark yapılandırılmış akış](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) verileri okuyup yazmak için [Apache Kafka](https://kafka.apache.org/) HDInsight üzerinde. Nasıl kullanılacağını öğrenmek için aşağıdaki bağlantıyı kullanın [Apache Storm](https://storm.apache.org/) Kafka ile.

> [!div class="nextstepaction"]
> [Apache Storm ile Apache kafka'yı kullanma](hdinsight-apache-storm-with-kafka.md)
