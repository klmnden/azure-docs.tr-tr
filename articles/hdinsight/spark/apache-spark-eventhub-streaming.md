---
title: "Öğretici: Azure hdınsight'ta Apache Spark Azure Event Hubs verilerini işlemek | Microsoft Docs"
description: Azure hdınsight'ta Apache Spark Azure Event Hubs'a bağlanmak ve veri akışı işlem.
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: jgao
ms.openlocfilehash: 9b59f5d58234aaf8f8385f722d6659548e066933
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-spark-in-hdinsight"></a>Öğretici: İşlem Hdınsight'ta Azure olay hub'ları ve Spark kullanarak tweet'leri

Bu öğreticide, Azure olay hub'ına tweet'leri göndermek ve olay hub'ından tweet'leri okumak için başka bir uygulama oluşturmak için bir Apache uygulama yayınlamayı Spark oluşturmayı öğrenin. Spark akış ayrıntılı bir açıklaması için bkz: [Apache Spark genel bakış akış](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). Hdınsight, Azure üzerinde bir Spark kümesi için aynı akış özellikleri getirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure Event Hub'ına iletileri gönder
> * Azure Event Hub'ından iletilerini okuyun

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* **Makaleyi tamamlamak [Öğreticisi: Azure hdınsight'ta Apache Spark kümesinde sorguları çalıştırmak ve veri yükleme](./apache-spark-load-data-run-query.md)**.

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Tweet’lerin akışını almak için Twitter’da bir uygulama oluşturursunuz. Bir Twitter uygulaması oluşturun ve bu öğreticiyi tamamlamak gereken değerleri yazın yönergeleri izleyin.

1. Gözat [Twitter Uygulama Yönetimi](https://apps.twitter.com/).
2. Seçin **yeni uygulama oluştur**.
3. Aşağıdaki değerleri sağlayın:

    - Ad: uygulama adı sağlayın. Bu öğretici için kullanılan değer **HDISparkStreamApp0423**. Bu ad benzersiz bir adı olması gerekir.
    - Açıklama: uygulamanın kısa bir açıklama sağlayın. Bu öğretici için kullanılan değer **uygulama yayınlamayı basit bir Hdınsight Spark**.
    - Web sitesi: uygulamanın Web sitesi sağlar. Geçerli bir Web sitesi olması gerekmez.  Bu öğretici için kullanılan değer **http://www.contoso.com**.
    - Geri çağırma URL'si: boş bırakılabilir.

4. Seçin **Evet, okuma ve Twitter Geliştirici sözleşmesini kabul**ve ardından **Twitter uygulamanızı oluşturma**.
5. Seçin **anahtarları ve erişim belirteçleri** sekmesi.
6. Seçin **my erişim belirteci oluşturma** sayfa sonunda.
7. Aşağıdaki değerleri sayfasından yazın.  Öğreticide daha sonra bu değerler gerekir:

    - **Tüketici anahtarı (API anahtarı)**    
    - **Tüketici gizli anahtarı (API gizli)**  
    - **Erişim belirteci**
    - **Erişim belirteci gizli anahtarı**   

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub’ı oluşturma

Tweet'leri depolamak için bu olay hub'ı kullanın.

1. [Azure Portal](https://ms.portal.azure.com)’da oturum açın.
2. Seçin **kaynak oluşturma** en üst ekranın sol.
3. Seçin **nesnelerin interneti**seçeneğini belirleyip **olay hub'ları**.

    ![Örnek Spark akış Create event hub](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "örnek Spark akış Create event hub")
4. Yeni olay hub'ad alanı için aşağıdaki değerleri girin:

    - **Ad**: olay hub'ı için bir ad girin.  Bu öğretici için kullanılan değer **myeventhubns20180403**.
    - **Fiyat katmanı**: seçin **standart**.
    - **Kaynak grubu**: Spark küme kaynak grubu seçin veya yeni bir seçeneği vardır. 
    - **Konum**: aynı seçin **konumu** gecikme süresini ve maliyetleri azaltmak hdınsight'ta Apache Spark kümeniz.

    ![Bir örnek Spark akış için olay hub'ı ad](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "örnek Spark akış için bir olay hub'ı adı belirtin")
5. Seçin **oluşturma** ad alanı oluşturmak için.

6. Aşağıdaki yönergeleri kullanarak olay hub'ı ad açın:

    1. Portaldan seçin **tüm hizmetleri**.
    2. Filtre kutusuna **olay hub'ları**.
    3. Oluşturduğunuz ad alanı çift tıklatın.
    4. Seçin **+ olay hub'ı**.

6. Olay hub'ları ad listesinde yeni oluşturulan ad alanı seçin.      
5. Seçin **Event Hubs**ve ardından **+ olay hub'ı** yeni bir olay hub'ı oluşturmak için.
  

6. Aşağıdaki değerleri girin:

    - Ad: olay Hub'ınız için bir ad verin.
    - Bölüm sayısı: 10
    - İleti saklama: 1. 
   
    ![Olay hub'ı ayrıntılarını sağlamak için örnek akış Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "için Spark akış örnek olay hub'ı ayrıntılarını sağlayın")

7. **Oluştur**’u seçin.
8. Seçin **paylaşılan erişim ilkeleri** ad alanı (Not olay hub'ı paylaşılan erişim ilkeleri değil) ve ardından **RootManageSharedAccessKey**.
    
     ![Olay hub'ı ilkeler için örnek akış Spark ayarlama](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "örnek akış Spark için Event Hub'ı ayarlamak ilkeleri")

9. Değerlerini kaydetmek **birincil anahtar** ve **bağlantı dize birincil anahtarı** daha sonra öğreticide kullanmak üzere.

     ![Olay hub'ı İlkesi anahtarları için örnek akış Spark görüntülemek](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "görünüm olay hub'ı ilke anahtarları için Spark akış örneği")


## <a name="send-tweets-to-the-event-hub"></a>Olay hub'ına tweet'leri Gönder

Jupyter not defteri oluşturun ve adlandırın gerek **SendTweetsToEventHub**. 

1. Dış Maven kitaplıkları eklemek için aşağıdaki kodu çalıştırın:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. Olay hub'ınıza tweet'leri göndermek için aşağıdaki kodu çalıştırın:

    ```
    import java.util._
    import scala.collection.JavaConverters._
    import java.util.concurrent._
    
    import org.apache.spark._
    import org.apache.spark.streaming._
    import org.apache.spark.eventhubs.ConnectionStringBuilder

    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    import com.microsoft.azure.eventhubs._
    val pool = Executors.newFixedThreadPool(1)
    val eventHubClient = EventHubClient.create(connStr.toString(), pool)
    
    def sendEvent(message: String) = {
          val messageData = EventData.create(message.getBytes("UTF-8"))
          eventHubClient.get().send(messageData)
          println("Sent event: " + message + "\n")
    }
    
    import twitter4j._
    import twitter4j.TwitterFactory
    import twitter4j.Twitter
    import twitter4j.conf.ConfigurationBuilder

    // Twitter application configurations
    // Replace values below with yours   
    val twitterConsumerKey = "<CONSUMER KEY>"
    val twitterConsumerSecret = "<CONSUMER SECRET>"
    val twitterOauthAccessToken = "<ACCESS TOKEN>"
    val twitterOauthTokenSecret = "<TOKEN SECRET>"
    
    val cb = new ConfigurationBuilder()
    cb.setDebugEnabled(true).setOAuthConsumerKey(twitterConsumerKey).setOAuthConsumerSecret(twitterConsumerSecret).setOAuthAccessToken(twitterOauthAccessToken).setOAuthAccessTokenSecret(twitterOauthTokenSecret)
    
    val twitterFactory = new TwitterFactory(cb.build())
    val twitter = twitterFactory.getInstance()

    // Getting tweets with keyword "Azure" and sending them to the Event Hub in realtime!
    
    val query = new Query(" #Azure ")
    query.setCount(100)
    query.lang("en")
    var finished = false
    while (!finished) {
      val result = twitter.search(query)
      val statuses = result.getTweets()
      var lowestStatusId = Long.MaxValue
      for (status <- statuses.asScala) {
        if(!status.isRetweet()){
          sendEvent(status.getText())
        }
        lowestStatusId = Math.min(status.getId(), lowestStatusId)
        Thread.sleep(2000)
      }
      query.setMaxId(lowestStatusId - 1)
    }
    
    // Closing connection to the Event Hub
    eventHubClient.get().close()
    ```

3. Azure Portalı'nda olay hub'ı açın.  Üzerinde **genel bakış**, olay hub'ına gönderilen iletileri gösteren bazı grafiklerde göreceksiniz.

## <a name="read-tweets-from-the-event-hub"></a>Olay hub'ı gelen okuma tweet'leri

Başka bir Jupyter not defteri oluşturun ve adlandırın gerek **ReadTweetsFromEventHub**. 

1. Dış Maven kitaplık eklemek için aşağıdaki kodu çalıştırın:

    ```
    %%configure -f
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0"}}
    ```
2. Olay hub'ından tweet'leri okumak için aşağıdaki kodu çalıştırın:

    ```
    import org.apache.spark.eventhubs._
    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    val customEventhubParameters = EventHubsConf(connectionString).setMaxEventsPerTrigger(5)
    val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()
    //incomingStream.printSchema    
    
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._
    
    // Event Hub message format is JSON and contains "body" field
    // Body is binary, so you cast it to string to see the actual content of the message
    val messages = incomingStream.withColumn("Offset", $"offset".cast(LongType)).withColumn("Time (readable)", $"enqueuedTime".cast(TimestampType)).withColumn("Timestamp", $"enqueuedTime".cast(LongType)).withColumn("Body", $"body".cast(StringType)).select("Offset", "Time (readable)", "Timestamp", "Body")
    
    messages.printSchema
    
    messages.writeStream.outputMode("append").format("console").option("truncate", false).start().awaitTermination()
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kullanımda olmadığında bir kümeyi güvenle silebilirsiniz Hdınsight ile verileriniz Azure Storage veya Azure Data Lake Store, depolanır, böylece. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Sonraki öğretici hemen çalışmayı planlıyorsanız, küme tutmak isteyebilirsiniz.

Azure portalında bir küme açıp seçin **silmek**.

![Hdınsight kümesini silmek](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "silmek Hdınsight kümesi")

Ayrıca kaynak grubu sayfasını açmak için kaynak grubu adı seçin ve ardından **kaynak grubu Sil**. Kaynak grubunu silerek Hdınsight Spark kümesinde hem varsayılan depolama hesabını silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Bir olay hub'ından iletisini okuyun.
Machine learning uygulama oluşturabileceğiniz görmek için sonraki makalede ilerleyin. 

> [!div class="nextstepaction"]
> [Machine learning uygulama oluşturma](./apache-spark-ipython-notebook-machine-learning.md)


