---
title: 'Öğretici: Azure HDInsight, Apache Spark ile Azure Event Hubs verilerini işleme '
description: Azure HDInsight, Apache Spark Azure Event Hubs'a bağlanın ve akış verilerini işleme.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.custom: hdinsightactive,mvc
ms.topic: conceptual
ms.date: 06/14/2018
ms.openlocfilehash: 27c8a51ee3f0274489041f4dafbbf73d906e2fa4
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617658"
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-spark-in-hdinsight"></a>Öğretici: Azure Event Hubs ve Spark HDInsight'ı kullanarak bir işlem tweet

Bu öğreticide, Azure olay hub'ına tweetler gönderin ve tweetleri event hub'ından okumak için başka bir uygulama oluşturmak için bir Apache akış uygulaması Spark oluşturma konusunda bilgi edinin. Spark akış ayrıntılı bir açıklaması için bkz. [Apache Spark'ın genel bakış akış](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight, Azure üzerinde bir Spark kümesi için aynı akış özellikleri sunar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure olay Hub'ına ileti gönderme
> * Azure olay Hub'ından iletiler okuma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* **[Öğretici: Azure HDInsight içindeki bir Apache Spark kümesinde veri yükleme ve sorgu çalıştırma](./apache-spark-load-data-run-query.md)** makalesini tamamlayın.

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Tweet’lerin akışını almak için Twitter’da bir uygulama oluşturursunuz. İzleme yönergeleri bir Twitter uygulaması oluşturun ve bu öğreticiyi tamamlamak gereken değerleri not alın.

1. Gözat [Twitter Uygulama Yönetimi](https://apps.twitter.com/).
2. Seçin **yeni uygulama oluştur**.
3. Aşağıdaki değerleri sağlayın:

    - Ad: uygulama adı sağlayın. Bu öğreticide kullanılan değer **HDISparkStreamApp0423**. Bu ad benzersiz bir adı olması gerekir.
    - Açıklama: uygulama kısa bir açıklamasını sağlayın. Bu öğreticide kullanılan değer **basit bir HDInsight Spark akış uygulaması**.
    - Web sitesi: uygulamanın Web sitesini belirtin. Geçerli Web sitesi yok.  Bu öğreticide kullanılan değer **http://www.contoso.com**.
    - Geri çağırma URL'si: boş bırakılabilir.

4. Seçin **Evet, okuma ve Twitter Geliştirici sözleşmesini kabul**ve ardından **kendi Twitter uygulamanızı oluşturun**.
5. Seçin **anahtarlar ve erişim belirteçleri** sekmesi.
6. Seçin **erişim belirtecimi Oluştur** sayfanın sonundaki.
7. Sayfasından aşağıdaki değerleri yazın.  Öğreticinin ilerleyen bölümlerinde bu değerlere ihtiyacınız olur:

    - **Tüketici anahtarı (API anahtarı)**    
    - **Tüketici gizli anahtarı (API gizli)**  
    - **Erişim belirteci**
    - **Erişim belirteci gizli anahtarı**   

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub’ı oluşturma

Tweetleri depolamak için bu olay hub'ı kullanın.

1. [Azure Portal](https://ms.portal.azure.com)’da oturum açın.
2. Seçin **kaynak Oluştur** , ekranın sol üst köşesindeki.
3. Seçin **nesnelerin interneti**, ardından **Event Hubs**.

    ![Spark akış örneği için oluşturma olay hub'ı](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Spark akış örneği için oluşturma olay hub'ı")
4. Yeni olay hub'ı ad alanı için aşağıdaki değerleri girin:

    - **Ad**: olay hub'ı için bir ad girin.  Bu öğreticide kullanılan değer **myeventhubns20180403**.
    - **Fiyat katmanı**: seçin **standart**.
    - **Kaynak grubu**: Spark kümesi için kaynak grubunu seçin veya yeni bir seçeneğiniz vardır. 
    - **Konum**: aynı seçin **konumu** gecikme süresini ve maliyetleri azaltmak için HDInsight, Apache Spark kümenizin olarak.

    ![Belirtin Spark akış örneği için bir olay hub'ı adı](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "belirtin Spark akış örneği için bir olay hub'ı adı")
5. Seçin **Oluştur** ad alanı oluşturmak için.

6. Aşağıdaki yönergeleri kullanarak olay hub'ı ad alanını açın:

    1. Portaldan seçin **tüm hizmetleri**.
    2. Filtre kutusuna **olay hub'ları**.
    3. Oluşturduğunuz ad alanına çift tıklayın.
    4. Seçin **+ olay hub'ı**.

6. Event Hubs ad alanı listesinde yeni oluşturulan ad alanı seçin.      
5. Seçin **Event Hubs**ve ardından **+ olay hub'ı** yeni bir olay hub'ı oluşturmak için.
  

6. Aşağıdaki değerleri girin:

    - Ad: olay Hub'ınız için bir ad verin.
    - Bölüm sayısı: 10
    - İleti bekletme: 1. 
   
    ![Olay hub'ı ayrıntılarını belirtin Spark akış örneği için](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Spark akış örneği için olay hub'ı ayrıntılarını sağlayın")

7. **Oluştur**’u seçin.
8. Seçin **paylaşılan erişim ilkeleri** ad alanı (olay hub'ı paylaşılan erişim ilkeleri değil. Not) ve ardından **RootManageSharedAccessKey**.
    
     ![Spark akış örneği için olay hub'ı ilkeler ayarlama](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "örnek akış Spark için Event Hub'ı ayarlama ilkeleri")

9. Değerlerini kaydetmek **birincil anahtar** ve **bağlantı dizesi-birincil anahtar** öğreticinin ilerleyen bölümlerinde kullanmak için.

     ![Olay hub'ı ilke anahtarları görüntülemek için örnek akış Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "görünümü olay hub'ı ilke anahtarları için Spark akış örneği")


## <a name="send-tweets-to-the-event-hub"></a>Olay hub'ına tweet gönderin

Jupyter not defteri oluşturun ve adlandırın için ihtiyaç duyduğunuz **SendTweetsToEventHub**. 

1. Dış Maven kitaplıkları eklemek için aşağıdaki kodu çalıştırın:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. Olay hub'ınıza tweetleri göndermek için aşağıdaki kodu çalıştırın:

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

3. Olay hub'ı, Azure portalında açın.  Üzerinde **genel bakış**, olay hub'ına gönderilen iletilerin gösteren bazı grafikler göreceksiniz.

## <a name="read-tweets-from-the-event-hub"></a>Olay hub'ından tweet'leri okuma

Başka bir Jupyter not defteri oluşturun ve adlandırın için ihtiyaç duyduğunuz **ReadTweetsFromEventHub**. 

1. Bir dış Maven Kitaplığı eklemek için aşağıdaki kodu çalıştırın:

    ```
    %%configure -f
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0"}}
    ```
2. Olay hub'ından tweetleri okumak için aşağıdaki kodu çalıştırın:

    ```
    import org.apache.spark.eventhubs._
    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    val customEventhubParameters = EventHubsConf(connStr).setMaxEventsPerTrigger(5)
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

HDInsight ile verileriniz, Azure Depolama’da veya Azure Data Lake Store’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Sonraki öğretici üzerinde hemen çalışmayı planlıyorsanız, kümeyi tutmak isteyebilirsiniz.

Azure portalında kümeyi açıp **Sil**’i seçin.

![HDInsight kümesini silme](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

* Bir olay hub'ından ileti okuyun.
Bir machine learning uygulama oluşturacağınızı öğrenmek için sonraki makaleye ilerleyin. 

> [!div class="nextstepaction"]
> [Bir makine öğrenimi uygulaması oluşturma](./apache-spark-ipython-notebook-machine-learning.md)


