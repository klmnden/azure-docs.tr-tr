---
title: 'Öğretici: Yapılandırılmış akış ile Apache Kafka - Azure HDInsight, Apache Spark'
description: Apache Spark akışını kullanarak Apache Kafka içine veya dışına veri almayı öğrenin. Bu öğreticide, HDInsight üzerinde Spark’tan bir Jupyter not defterini kullanarak verilerinizi akışla aktaracaksınız.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: tutorial
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 388ce607cf75a12705c9a32fe19086dbf9f15e71
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62096156"
---
# <a name="tutorial-use-apache-spark-structured-streaming-with-apache-kafka-on-hdinsight"></a>Öğretici: Apache Spark yapılandırılmış akışını HDInsight üzerinde Apache Kafka ile kullanma

Bu öğreticide nasıl kullanılacağını gösterir [Apache Spark yapılandırılmış akış](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) ile veri okuma ve yazma için [Apache Kafka](https://kafka.apache.org/) Azure HDInsight üzerinde.

Spark yapılandırılmış akışı, Spark SQL üzerinde yerleşik bir akış işleme altyapısıdır. Bu altyapıyı kullanarak, statik veriler üzerinde toplu hesaplamayla aynı şekilde akış hesaplamalarını ifade edebilirsiniz.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kafka ile Yapılandırılmış Akış
> * Kafka ve Spark kümeleri oluşturma
> * Not defterini Spark’a karşıya yükleme
> * Not defterini kullanma
> * Kaynakları temizleme

Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümeleri silmeyi unutmayın.

## <a name="prerequisites"></a>Önkoşullar

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

| Batch | Akış |
| --- | --- |
| `read` | `readStream` |
| `write` | `writeStream` |
| `save` | `start` |

Akış işlemi, 30000 ms sonra akışı durduran `awaitTermination(30000)` komutunu da kullanır. 

Kafka ile Yapılandırılmış Akışı kullanmak için projenizin `org.apache.spark : spark-sql-kafka-0-10_2.11` paketinde bir bağımlılığı olmalıdır. Bu paketin sürümü, HDInsight üzerinde Spark sürümüyle eşleşmelidir. Spark 2.2.0 (HDInsight 3.6’da kullanılabilir) için, [https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar](https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar) adresinden farklı proje türleri için bağımlılık bilgilerini bulabilirsiniz.

Bu öğretici ile sağlanan Jupyter Notebook için aşağıdaki hücre, bu paket bağımlılığını yükler:

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

## <a name="upload-the-notebook"></a>Not defterini karşıya yükleme

Not defterini projeden HDInsight üzerinde Spark kümenize yüklemek için aşağıdaki adımları kullanın:

1. [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming) adresinden projeyi indirin.

1. Web tarayıcınızdan Spark kümeniz üzerindeki Jupyter not defterine bağlanın. Aşağıdaki URL’de `CLUSTERNAME` değerini __Spark__ kümenizin adıyla değiştirin:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Sorulduğunda, kümeyi oluştururken kullanılan küme kullanıcı adı (yönetici) ve parolasını girin.

2. Sayfanın sağ üst kısmındaki __Karşıya Yükle__ düğmesini kullanarak __spark-structured-streaming-kafka.ipynb__ dosyasını kümeye yükleyin. Karşıya yüklemeyi başlatmak için __Aç__’ı seçin.

    ![Karşıya yükleme düğmesini kullanarak not defterini seçme ve karşıya yükleme](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![KafkaStreaming.ipynb dosyasını seçme](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Not defterleri listesinde __spark-structured-streaming-kafka.ipynb__ girişini bulun ve yanındaki __Karşıya Yükle__ düğmesini seçin.

    ![Not defterini karşıya yüklemek için KafkaStreaming.ipynb girişinin karşıya yükleme düğmesini kullanın](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)


## <a name="use-the-notebook"></a>Not defterini kullanma

Dosyalar karşıya yüklendikten sonra __spark-structured-streaming-kafka.ipynb__ girişini seçerek not defterini açın. Spark yapılandırılmış akışını HDInsight üzerinde Kafka ile birlikte kullanma hakkında bilgi almak için not defterindeki yönergeleri izleyin.

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
