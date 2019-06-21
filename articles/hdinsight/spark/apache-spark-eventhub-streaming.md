---
title: 'Öğretici: Apache Spark, Azure HDInsight ile Azure Event hubs verilerini işleme '
description: Öğretici - Azure HDInsight, Apache Spark Azure Event Hubs'a bağlanma ve akış verilerini işleme.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/24/2019
ms.openlocfilehash: c8c99d976f416d0c1d07fb3a266d37ecd6235fdb
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295364"
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-apache-spark-in-hdinsight"></a>Öğretici: HDInsight Azure Event Hubs ve Apache Spark'ı kullanarak tweetleri işleyin

Bu öğreticide, şunların nasıl oluşturulacağı bir [Apache Spark](https://spark.apache.org/) tweetleri bir Azure olay hub'ına gönderme ve tweetleri event hub'ından okumak için başka bir uygulama oluşturmak için uygulama akış. Spark akış ayrıntılı bir açıklaması için bkz. [Apache Spark'ın genel bakış akış](https://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight, Azure üzerinde bir Spark kümesi için aynı akış özellikleri sunar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure olay Hub'ına ileti gönderme
> * Azure olay Hub'ından iletiler okuma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Apache Spark kümesi. Bkz: [bir Apache Spark kümesi oluşturma](./apache-spark-jupyter-spark-sql-use-portal.md).

* HDInsight üzerinde Spark ile Jupyter Notebook kullanma bilgisi. Daha fazla bilgi için [veri yükleme ve HDInsight üzerinde Apache Spark ile sorguları çalıştırma](./apache-spark-load-data-run-query.md).

* A [Twitter hesabımızı](https://twitter.com/i/flow/signup).

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Tweet’lerin akışını almak için Twitter’da bir uygulama oluşturursunuz. İzleme yönergeleri bir Twitter uygulaması oluşturun ve bu öğreticiyi tamamlamak gereken değerleri not alın.

1. Gözat [Twitter Uygulama Yönetimi](https://apps.twitter.com/).

1. Seçin **yeni uygulama oluştur**.

1. Aşağıdaki değerleri sağlayın:

    |Özellik |Değer |
    |---|---|
    |Ad|Uygulama adı sağlayın. Bu öğreticide kullanılan değer **HDISparkStreamApp0423**. Bu ad benzersiz bir adı olması gerekir.|
    |Açıklama|Uygulamanın kısa bir açıklama sağlayın. Bu öğreticide kullanılan değer **basit bir HDInsight Spark akış uygulaması**.|
    |Web sitesi|Uygulamanın Web sitesini belirtin. Geçerli Web sitesi yok.  Bu öğreticide kullanılan değer **http://www.contoso.com** .|
    |Geri çağırma URL'si|Boş bırakabilirsiniz.|

1. Seçin **Evet, okuma ve Twitter Geliştirici sözleşmesini kabul**ve ardından **kendi Twitter uygulamanızı oluşturun**.

1. Seçin **anahtarlar ve erişim belirteçleri** sekmesi.

1. Seçin **erişim belirtecimi Oluştur** sayfanın sonundaki.

1. Sayfasından aşağıdaki değerleri yazın.  Öğreticinin ilerleyen bölümlerinde bu değerlere ihtiyacınız olur:

    - **Tüketici anahtarı (API anahtarı)**    
    - **Tüketici gizli anahtarı (API gizli)**  
    - **Erişim belirteci**
    - **Erişim belirteci gizli anahtarı**   

## <a name="create-an-azure-event-hubs-namespace"></a>Bir Azure Event Hubs ad alanı oluşturma

Tweetleri depolamak için bu olay hub'ı kullanın.

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Sol menüden **tüm hizmetleri**.  

3. Altında **NESNELERİN İNTERNETİ**seçin **Event Hubs**. 

    ![Spark akış örneği için oluşturma olay hub'ı](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Spark akış örneği için oluşturma olay hub'ı")

4. **+ Ekle** öğesini seçin.

5. Yeni Event Hubs ad alanı için aşağıdaki değerleri girin:

    |Özellik |Değer |
    |---|---|
    |Ad|Olay hub'ı için bir ad girin.  Bu öğreticide kullanılan değer **myeventhubns20180403**.|
    |Fiyatlandırma katmanı|Seçin **standart**.|
    |Abonelik|Uygun aboneliğinizi seçin.|
    |Kaynak grubu|Aşağı açılan listeden mevcut bir kaynak grubunu seçin ya da seçin **Yeni Oluştur** yeni bir kaynak grubu oluşturmak için.|
    |Location|Aynı seçin **konumu** gecikme süresini ve maliyetleri azaltmak için HDInsight, Apache Spark kümenizin olarak.|
    |Otomatik Şişme (isteğe bağlı) etkinleştir |Otomatik şişme trafiğiniz atanmış üretilen iş Birimi'ne kapasiteyi aştığında, olay hub'ları Namespace için atanan işleme birimleri sayısını otomatik olarak ölçeklendirir.  |
    |Otomatik Şişme en fazla işleme birimi (isteğe bağlı)|Bu kaydırıcı, iade ederseniz yalnızca görünür **etkinleştirmek otomatik Şişme**.  |

    ![Belirtin Spark akış örneği için bir olay hub'ı adı](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "belirtin Spark akış örneği için bir olay hub'ı adı")

6. Seçin **Oluştur** ad alanı oluşturmak için.  Dağıtım birkaç dakika içinde tamamlanır.

## <a name="create-an-azure-event-hub"></a>Bir Azure olay hub'ı oluşturma
Event Hubs ad alanı dağıtıldıktan sonra bir olay hub'ı oluşturun.  Portaldan:

1. Sol menüden **tüm hizmetleri**.  

1. Altında **NESNELERİN İNTERNETİ**seçin **Event Hubs**.  

1. Event Hubs ad alanınız listeden seçin.  

1. Gelen **olay hub'ları Namespace** sayfasında **+ olay hub'ı**.  
1. Aşağıdaki değerleri girin **olay hub'ı Oluştur** sayfası:

    - **Ad**: Olay Hub'ınız için bir ad verin. 
 
    - **Bölüm sayısı**: 10.  

    - **İleti bekletme**: 1.   
   
      ![Olay hub'ı ayrıntılarını belirtin Spark akış örneği için](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Spark akış örneği için olay hub'ı ayrıntılarını sağlayın")

1. **Oluştur**’u seçin.  Dağıtım birkaç saniye içinde tamamlanır ve olay hub'ları Namespace sayfaya döner.

1. Altında **ayarları**seçin **paylaşılan erişim ilkeleri**.

1. Seçin **RootManageSharedAccessKey**.
    
     ![Spark akış örneği için olay hub'ı ilkeler ayarlama](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "örnek akış Spark için Event Hub'ı ayarlama ilkeleri")

1. Değerlerini kaydetmek **birincil anahtar** ve **bağlantı dizesi-birincil anahtar** öğreticinin ilerleyen bölümlerinde kullanmak için.

     ![Olay hub'ı ilke anahtarları görüntülemek için örnek akış Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "görünümü olay hub'ı ilke anahtarları için Spark akış örneği")


## <a name="send-tweets-to-the-event-hub"></a>Olay hub'ına tweet gönderin

Jupyter not defteri oluşturun ve adlandırın **SendTweetsToEventHub**. 

1. Dış Apache Maven kitaplıkları eklemek için aşağıdaki kodu çalıştırın:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. Aşağıdaki kod değiştirerek Düzenle `<Event hub name>`, `<Event hub namespace connection string>`, `<CONSUMER KEY>`, `<CONSUMER SECRET>`, `<ACCESS TOKEN>`, ve `<TOKEN SECRET>` uygun değerlerle. Olay hub'ınıza tweet'ler göndermek için düzenlenen kodu çalıştırın:

    ```scala
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

Başka bir Jupyter not defteri oluşturun ve adlandırın **ReadTweetsFromEventHub**. 

1. Bir dış Apache Maven Kitaplığı eklemek için aşağıdaki kodu çalıştırın:

    ```
    %%configure -f
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0"}}
    ```

2. Aşağıdaki kod değiştirerek Düzenle `<Event hub name>`, ve `<Event hub namespace connection string>` uygun değerlerle. Olay hub'ından tweetleri okumak için düzenlenen kodu çalıştırın:

    ```scala
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

Kullanımda olmadığında bir kümeyi güvenle silebilirsiniz HDInsight ile Azure depolama veya Azure Data Lake depolama, verilerinizi depolanır. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Sonraki öğretici hemen işe planlıyorsanız, küme tutar, aksi takdirde devam edin ve kümeyi silmek isteyebilirsiniz.

Azure portalında kümeyi açıp **Sil**’i seçin.

![HDInsight kümesini silme](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "HDInsight kümesini silme")

Kaynak grubu adını seçerek de kaynak grubu sayfasını açabilir ve sonra **Kaynak grubunu sil**’i seçebilirsiniz. Kaynak grubunu silerek hem HDInsight Spark kümesini hem de varsayılan depolama hesabını silersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Apache Spark tweetleri bir Azure olay hub'ına göndermek için uygulama akışı oluşturma hakkında bilgi edindiniz ve tweetleri event hub'ından okumak için başka bir uygulama oluşturdunuz.  Bir machine learning uygulama oluşturacağınızı öğrenmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Bir makine öğrenimi uygulaması oluşturma](./apache-spark-ipython-notebook-machine-learning.md)
