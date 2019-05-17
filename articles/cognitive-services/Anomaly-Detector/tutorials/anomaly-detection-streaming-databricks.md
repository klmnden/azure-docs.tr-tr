---
title: 'Öğretici: Azure Databricks kullanarak akış verileri anomali algılama'
description: Verilerinizdeki anormallikleri izlemek için Azure Databricks ve Anomali algılayıcısı API'sini kullanın.
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 05/08/2019
ms.author: aahi
ms.openlocfilehash: a00ad2523c215fa54d7d19d8c9e923b621f3081a
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65791761"
---
# <a name="tutorial-anomaly-detection-on-streaming-data-using-azure-databricks"></a>Öğretici: Azure Databricks kullanarak akış verileri anomali algılama

Microsoft Power BI Desktop, verilerinize bağlanmanıza, verilerinizi dönüştürmenize ve görselleştirmenize olanak sağlayan ücretsiz bir uygulamadır. Anomali algılayıcısı APİ'si, Azure Bilişsel Hizmetler'in bir parçası, zaman serisi verilerinizle izleme bir yol sağlar. Anomali algılama neredeyse gerçek zamanlı veri akışı çalıştırmak için bu öğreticiyi kullanın. Azure Databricks kullanarak. Azure Event Hubs kullanarak twitter verilerini alma ve Spark Event Hubs bağlayıcısını kullanarak Azure Databricks'e aldıktan. Ardından, akış verileri üzerinde anomalileri algılamak için API kullanacaksınız. 

Aşağıdaki şekilde uygulama akışı gösterilmektedir:

![Event Hubs ve Bilişsel Hizmetler ile Azure Databricks](../media/tutorials/databricks-cognitive-services-tutorial.png "Event Hubs ve Bilişsel Hizmetler ile Azure Databricks")

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Databricks çalışma alanı oluşturma
> * Azure Databricks’te Spark kümesi oluşturma
> * Akış verilerine erişmek için bir Twitter uygulaması oluşturma
> * Azure Databricks’te not defterleri oluşturma
> * Event Hubs ve Twitter API’si için kitaplıklar ekleme
> * Bir Anomali algılayıcısı kaynak oluşturmak ve erişim anahtarını alma
> * Event Hubs’a tweet’ler gönderme
> * Event Hubs’tan tweet’leri okuma
> * Anomali algılama tweet'lerde çalıştırın

> [!Note]
> Bu öğreticide önerilen bir yaklaşım sunar [çözüm mimarisi](https://azure.microsoft.com/solutions/architecture/anomaly-detector-process/) Anomali algılayıcısı API'si için.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

> [!Note]
> Bu öğreticide Anomali algılayıcısı API'si için ücretsiz bir deneme anahtarıyla tamamlanamıyor. Azure Databricks kümesini oluşturmak için ücretsiz hesap oluşturmak istiyorsanız kümeyi oluşturmadan önce profilinize gidin ve aboneliğini **kullandıkça öde** modeline geçirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- Bir [Azure Event Hubs ad alanı](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) ve olay hub'ı.

- [Bağlantı dizesi](../../../event-hubs/event-hubs-get-connection-string.md) Event Hubs ad alanına erişmek için. Bağlantı dizesi, benzer bir biçime sahip olmalıdır:

    `Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<key name>;SharedAccessKey=<key value>`. 

- Event Hubs için paylaşılan erişim ilkesi adı ve ilke anahtarı.

Azure Event Hubs bkz [hızlı](../../../event-hubs/event-hubs-create.md) bir ad alanı ve olay hub'ı oluşturma hakkında daha fazla bilgi için.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde, kullanarak bir Azure Databricks çalışma alanı oluşturma [Azure portalında](https://portal.azure.com/).

1. Azure portalında **Kaynak oluşturun** > **Analiz** > **Azure Databricks**'i seçin.

    ![Azure portalında Databricks](../media/tutorials/azure-databricks-on-portal.png "Databricks on Azure portal")

3. Altında **Azure Databricks hizmeti**, Databricks çalışma alanı oluşturmak için aşağıdaki değerleri sağlayın:


    |Özellik  |Açıklama  |
    |---------|---------|
    |**Çalışma alanı adı**     | Databricks çalışma alanınız için bir ad sağlayın        |
    |**Abonelik**     | Açılan listeden Azure aboneliğinizi seçin.        |
    |**Kaynak grubu**     | Yeni bir kaynak grubu oluşturmayı veya mevcut bir kaynak grubunu kullanmayı seçin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları bir arada tutan kapsayıcıdır. Daha fazla bilgi için bkz. [Azure Kaynak Grubuna genel bakış](../../../azure-resource-manager/resource-group-overview.md). |
    |**Konum**     | Seçin **Doğu ABD 2** veya kullanılabilir diğer bölgeler. Bkz: [bölgeye göre kullanılabilir Azure Hizmetleri](https://azure.microsoft.com/regions/services/) için bölge kullanılabilirliği.        |
    |**Fiyatlandırma Katmanı**     |  **Standart** veya **Premium** arasında seçim yapın. Seçmediğiniz **deneme**. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

    **Oluştur**’u seçin.

4. Hesabın oluşturulması birkaç dakika sürer. 

## <a name="create-a-spark-cluster-in-databricks"></a>Databricks’te Spark kümesi oluşturma

1. Azure portalında, oluşturduğunuz Databricks çalışma alanına gidin ve sonra **Çalışma Alanını Başlat**’ı seçin.

2. Azure Databricks portalına yönlendirilirsiniz. Portaldan seçin **yeni kümeye**.

    ![Azure’da Databricks](../media/tutorials/databricks-on-azure.png "Databricks on Azure")

3. İçinde **yeni kümeye** sayfasında, bir küme oluşturmak için değerleri sağlayın.

    ![Azure’da Databricks Spark kümesi oluşturma](../media/tutorials/create-databricks-spark-cluster.png "Create Databricks Spark cluster on Azure")

    Aşağıdakiler dışında diğer tüm varsayılan değerleri kabul edin:

   * Küme için bir ad girin.
   * Bu makale için bir küme oluşturun **5.2** çalışma zamanı. Seçmeyin **5.3** çalışma zamanı.
   * Emin **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** onay kutusunun seçili. Küme kullanılmıyor, kümenin sonlandırılması için biz süre (dakika cinsinden) belirtin.

     **Küme oluştur**’u seçin. Küme çalışmaya başladıktan sonra kümeye not defterleri ekleyebilir ve Spark işleri çalıştırabilirsiniz.

## <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma

Tweet’lerin akışını almak için Twitter’da bir uygulama oluşturmanız gerekir. Adımları izleyerek bir Twitter uygulaması oluşturun ve bu öğreticiyi tamamlamak için ihtiyaç duyduğunuz değerleri kaydedin.

1. Bir web tarayıcısından [Twitter Uygulama Yönetimi](https://apps.twitter.com/)’ne gidip **Yeni Uygulama Oluştur**’u seçin.

    ![Twitter uygulaması oluşturma](../media/tutorials/databricks-create-twitter-app.png "Twitter uygulaması oluşturma")

2. **Uygulama oluşturun** sayfasında yeni uygulamaya ilişkin ayrıntıları sağlayın ve **Kendi Twitter uygulamanızı oluşturun**’u seçin.

    ![Twitter uygulaması ayrıntıları](../media/tutorials/databricks-provide-twitter-app-details.png "Twitter uygulaması ayrıntıları")

3. Uygulama sayfasında **Anahtarlar ve Erişim Belirteçleri** sekmesini seçin ve **Tüketici Anahtarı** ve **Tüketici Parolası** değerlerini kopyalayın. Ayrıca **Erişim belirtecimi oluştur**’u seçip erişim belirteçleri oluşturun. **Erişim Belirteci** ve**Erişim Belirteci Parolası** değerlerini kopyalayın.

    ![Twitter uygulaması ayrıntıları](../media/tutorials/twitter-app-key-secret.png "Twitter uygulaması ayrıntıları")

Twitter uygulaması için aldığınız değerleri kaydedin. Öğreticinin sonraki bölümlerinde bu değerlere ihtiyacınız olacaktır.

## <a name="attach-libraries-to-spark-cluster"></a>Spark kümesine kitaplıklar ekleme

Bu öğreticide, Event Hubs’a tweet’ler göndermek için Twitter API’lerini kullanırsınız. Azure Event Hubs’a verileri okuyup yazmak için de [Apache Spark Event Hubs bağlayıcısını](https://github.com/Azure/azure-event-hubs-spark) kullanırsınız. Kümenizin parçası olarak bu API’leri kullanmak için, Azure Databricks’e bunları kitaplıklar olarak ekler ve sonra Spark kümenizle ilişkilendirirsiniz. Aşağıdaki yönergeler için kitaplıklar ekleme Göster **paylaşılan** çalışma alanınızda bir klasör.

1. Azure Databricks çalışma alanında **Çalışma Alanı**’nı seçin ve sonra **Paylaşılan**’a sağ tıklayın. Bağlam menüsünden **Oluştur** > **Kitaplık** seçeneklerini belirleyin.

   ![Kitaplık ekle iletişim kutusu](../media/tutorials/databricks-add-library-option.png "Kitaplık ekle iletişim kutusu")

2. Yeni Kitaplık sayfasında **Kaynak** için **Maven Koordinatı**’nı seçin. **Koordinat** için, eklemek istediğiniz paketin koordinatını girin. Bu öğreticide kullanılan kitaplıklar için Maven koordinatları aşağıdaki gibidir:

   * Spark Event Hubs bağlayıcısı - `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`
   * Twitter API’si - `org.twitter4j:twitter4j-core:4.0.7`

     ![Maven koordinatları sağlama](../media/tutorials/databricks-eventhub-specify-maven-coordinate.png "Maven koordinatları sağlama")

3. **Oluştur**’u seçin.

4. Kitaplığı eklediğiniz klasörü seçin ve sonra kitaplık adını seçin.

    ![Eklenecek kitaplığı seçme](../media/tutorials/select-library.png "Eklenecek kitaplığı seçme")

5. Kitaplık sayfasında hiç küme yoksa seçin **kümeleri** ve oluşturduğunuz kümeyi çalıştırın. Durum 'Çalışıyor' gösterilene kadar bekleyin ve kitaplık sayfasına geri dönün.
Kitaplık sayfasında, kitaplığı kullanmak ve ardından istediğiniz kümeyi seçin **yükleme**. Kitaplık başarıyla kümeyle olduktan sonra durum hemen değişir **yüklü**.

    ![Kümeye kitaplık yükleme](../media/tutorials/databricks-library-attached.png "kümeye kitaplık yükleme")

6. Twitter paketi için şu adımları yineleyin, `twitter4j-core:4.0.7`.

## <a name="get-a-cognitive-services-access-key"></a>Bilişsel Hizmetler erişim anahtarını alma

Bu öğreticide kullandığınız [Azure Bilişsel hizmetler Anomali algılayıcısı API'leri](../overview.md) anomali algılama neredeyse gerçek zamanlı tweet akışı çalıştırmak için. API'leri kullanmadan önce Azure üzerinde bir Anomali algılayıcısı kaynağı oluşturun ve Anomali algılayıcısı API'leri kullanmak için bir erişim anahtarı almanız gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **+ Kaynak oluştur**’u seçin.

3. Azure Marketi bölümünde seçin **yapay ZEKA + makine öğrenimi** > **tümünü gör** > **Bilişsel Hizmetleri - fazla**  >  **Anomali algılayıcısı**. Ya da kullanabileceğinizi [bu bağlantıyı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) gitmek için **Oluştur** doğrudan iletişim kutusu.

    ![Anomali algılayıcısı kaynak oluşturma](../media/tutorials/databricks-cognitive-services-anomaly-detector.png "oluşturma Anomali algılayıcısı kaynak")

4. **Oluştur** iletişim kutusunda aşağıdaki değerleri sağlayın:

    |Değer |Açıklama  |
    |---------|---------|
    |Ad     | Anomali algılayıcısı kaynak için bir ad.        |
    |Abonelik     | Azure kaynak aboneliği ile ilişkilendirilecektir.        |
    |Location     | Bir Azure konumu.        |
    |Fiyatlandırma katmanı     | Hizmet için bir fiyatlandırma katmanı. Anomali algılayıcısı fiyatlandırması hakkında daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cognitive-services/anomaly-detector/).        |
    |Kaynak grubu     | Yeni bir kaynak grubu oluşturun veya mevcut bir isteyip istemediğinizi belirtin.        |


     **Oluştur**’u seçin.

5. Kaynak oluşturulduktan sonra gelen **genel bakış** sekmesinde **erişim anahtarlarını gösterme**.

    ![Erişim anahtarlarını gösterme](../media/tutorials/cognitive-services-get-access-keys.png "Erişim anahtarlarını gösterme")

    Ayrıca ekran görüntüsünde gösterildiği gibi uç nokta URL’sinin bir kısmını kopyalayın. Öğreticide bu URL’ye ihtiyacınız olacaktır.

6. Altında **anahtarları**, kullanmak istediğiniz anahtarın karşısındaki Kopyala simgesini seçin.

    ![Erişim anahtarlarını kopyalama](../media/tutorials/cognitive-services-copy-access-keys.png "Erişim anahtarlarını kopyalama")

7. Uç nokta URL’si ve erişim anahtarı için bu adımda aldığınız değerleri kaydedin. Daha sonra bu öğreticide gerekli olacak.

## <a name="create-notebooks-in-databricks"></a>Databricks’te not defterleri oluşturma

Bu bölümde, Databricks çalışma alanında aşağıdaki adlarla iki not defteri oluşturacaksınız

- **SendTweetsToEventHub** - Bu, Twitter’dan tweet’ler almak ve bunları Event Hubs’ta akışa almak için kullandığınız bir üretici not defteridir.
- **AnalyzeTweetsFromEventHub** -bir tüketici not defteri kullanırsınız Event Hubs'tan tweetleri okumak ve anomali algılama çalıştırın.

1. Sol bölmede **Çalışma Alanı**’nı seçin. **Çalışma Alanı** açılır listesinden **Oluştur**’u ve sonra **Not Defteri**’ni seçin.

    ![Databricks’te not defteri oluşturma](../media/tutorials/databricks-create-notebook.png "Create notebook in Databricks")

2. İçinde **Not Defteri Oluştur** iletişim kutusuna **SendTweetsToEventHub** belirleyin adı olarak **Scala** dil ve daha önce oluşturduğunuz Spark kümesini seçin.

    ![Databricks’te not defteri oluşturma](../media/tutorials/databricks-notebook-details.png "Create notebook in Databricks")

    **Oluştur**’u seçin.

3. **AnalyzeTweetsFromEventHub** not defterini oluşturma adımlarını yineleyin.

## <a name="send-tweets-to-event-hubs"></a>Event Hubs’a tweet’ler gönderme

İçinde **SendTweetsToEventHub** not defterine aşağıdaki kodu yapıştırın ve yer tutucuyu, daha önce oluşturduğunuz Twitter uygulamasının ve Event Hubs ad alanı için değerlerle değiştirin. Bu not defteri, gerçek zamanlı olarak "Azure" anahtar sözcüğünü içeren tweet’leri Event Hubs’ta akışa alır.

```scala
//
// Send Data to Eventhub
//

import scala.collection.JavaConverters._
import com.microsoft.azure.eventhubs._
import java.util.concurrent._
import com.google.gson.{Gson, GsonBuilder, JsonParser}
import java.util.Date
import scala.util.control._
import twitter4j._
import twitter4j.TwitterFactory
import twitter4j.Twitter
import twitter4j.conf.ConfigurationBuilder

// Event Hub Config
val namespaceName = "[Placeholder: EventHub namespace]"
val eventHubName = "[Placeholder: EventHub name]"
val sasKeyName = "[Placeholder: EventHub access key name]"
val sasKey = "[Placeholder: EventHub access key key]"
val connStr = new ConnectionStringBuilder()
  .setNamespaceName(namespaceName)
  .setEventHubName(eventHubName)
  .setSasKeyName(sasKeyName)
  .setSasKey(sasKey)

// Connect to the Event Hub
val pool = Executors.newScheduledThreadPool(1)
val eventHubClient = EventHubClient.create(connStr.toString(), pool)

def sendEvent(message: String) = {
  val messageData = EventData.create(message.getBytes("UTF-8"))
  eventHubClient.get().send(messageData)
  System.out.println("Sent event: " + message + "\n")
}

case class MessageBody(var timestamp: Date, var favorite: Int)
val gson: Gson = new GsonBuilder().setDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'").create()

val twitterConsumerKey = "[Placeholder: Twitter consumer key]"
val twitterConsumerSecret = "[Placeholder: Twitter consumer seceret]"
val twitterOauthAccessToken = "[Placeholder: Twitter oauth access token]"
val twitterOauthTokenSecret = "[Placeholder: Twitter oauth token secret]"

val cb = new ConfigurationBuilder()
cb.setDebugEnabled(true)
  .setOAuthConsumerKey(twitterConsumerKey)
  .setOAuthConsumerSecret(twitterConsumerSecret)
  .setOAuthAccessToken(twitterOauthAccessToken)
  .setOAuthAccessTokenSecret(twitterOauthTokenSecret)

val twitterFactory = new TwitterFactory(cb.build())
val twitter = twitterFactory.getInstance()

// Getting tweets with keyword "Azure" and sending them to the Event Hub in realtime!

val query = new Query(" #Azure ")
query.setCount(100)
query.lang("en")

var finished = false
var maxStatusId = Long.MinValue
var preMaxStatusId = Long.MinValue
val innerLoop = new Breaks
while (!finished) {
  val result = twitter.search(query)
  val statuses = result.getTweets()
  var lowestStatusId = Long.MaxValue
  innerLoop.breakable {
    for (status <- statuses.asScala) {
      if (status.getId() <= preMaxStatusId) {
        preMaxStatusId = maxStatusId
        innerLoop.break
      }
      if(!status.isRetweet()) {
        sendEvent(gson.toJson(new MessageBody(status.getCreatedAt(), status.getFavoriteCount())))
      }
      lowestStatusId = Math.min(status.getId(), lowestStatusId)
      maxStatusId = Math.max(status.getId(), maxStatusId)
    }
  }
  
  if (lowestStatusId == Long.MaxValue) {
    preMaxStatusId = maxStatusId
  }
  Thread.sleep(10000)
  query.setMaxId(lowestStatusId - 1)
}

// Close connection to the Event Hub
eventHubClient.get().close()
pool.shutdown()
```

Not defterlerini çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Aşağıdaki kod parçacığında gösterildiği gibi bir çıktı görürsünüz. Çıktıdaki her olay, Event Hubs’a giren bir tweet’tir.

    Sent event: {"timestamp":"2019-04-24T09:39:40.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:38:48.000Z","favorite":1}

    Sent event: {"timestamp":"2019-04-24T09:38:36.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:37:27.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:37:00.000Z","favorite":2}

    Sent event: {"timestamp":"2019-04-24T09:31:11.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:30:15.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:30:02.000Z","favorite":1}

    ...
    ...

## <a name="read-tweets-from-event-hubs"></a>Event Hubs’tan tweet’leri okuma

**AnalyzeTweetsFromEventHub** not defterine aşağıdaki kodu yapıştırın ve yer tutucuyu, daha önce oluşturduğunuz Azure Event Hubs değerleriyle değiştirin. Bu not defteri, **SendTweetsToEventHub** not defterini kullanarak önceden Event Hubs’ta akışa alınan tweet’leri okur.

İlk olarak, Anomali algılayıcısı çağırmak için bir istemci yazma. 
```scala

//
// Anomaly Detection Client
//

import java.io.{BufferedReader, DataOutputStream, InputStreamReader}
import java.net.URL
import java.sql.Timestamp

import com.google.gson.{Gson, GsonBuilder, JsonParser}
import javax.net.ssl.HttpsURLConnection

case class Point(var timestamp: Timestamp, var value: Double)
case class Series(var series: Array[Point], var maxAnomalyRatio: Double, var sensitivity: Int, var granularity: String)
case class AnomalySingleResponse(var isAnomaly: Boolean, var isPositiveAnomaly: Boolean, var isNegativeAnomaly: Boolean, var period: Int, var expectedValue: Double, var upperMargin: Double, var lowerMargin: Double, var suggestedWindow: Int)
case class AnomalyBatchResponse(var expectedValues: Array[Double], var upperMargins: Array[Double], var lowerMargins: Array[Double], var isAnomaly: Array[Boolean], var isPositiveAnomaly: Array[Boolean], var isNegativeAnomaly: Array[Boolean], var period: Int)

object AnomalyDetector extends Serializable {

  // Cognitive Services API connection settings
  val subscriptionKey = "[Placeholder: Your Anomaly Detector resource access key]"
  val endpoint = "[Placeholder: Your Anomaly Detector resource endpoint]"
  val latestPointDetectionPath = "/anomalydetector/v1.0/timeseries/last/detect"
  val batchDetectionPath = "/anomalydetector/v1.0/timeseries/entire/detect";
  val latestPointDetectionUrl = new URL(endpoint + latestPointDetectionPath)
  val batchDetectionUrl = new URL(endpoint + batchDetectionPath)
  val gson: Gson = new GsonBuilder().setDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'").setPrettyPrinting().create()

  def getConnection(path: URL): HttpsURLConnection = {
    val connection = path.openConnection().asInstanceOf[HttpsURLConnection]
    connection.setRequestMethod("POST")
    connection.setRequestProperty("Content-Type", "text/json")
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey)
    connection.setDoOutput(true)
    return connection
  }

  // Handles the call to Cognitive Services API.
  def processUsingApi(request: String, path: URL): String = {
    println(request)
    val encoded_text = request.getBytes("UTF-8")
    val connection = getConnection(path)
    val wr = new DataOutputStream(connection.getOutputStream())
    wr.write(encoded_text, 0, encoded_text.length)
    wr.flush()
    wr.close()

    val response = new StringBuilder()
    val in = new BufferedReader(new InputStreamReader(connection.getInputStream()))
    var line = in.readLine()
    while (line != null) {
      response.append(line)
      line = in.readLine()
    }
    in.close()
    return response.toString()
  }

  // Calls the Latest Point Detection API for timeserie.
  def detectLatestPoint(series: Series): Option[AnomalySingleResponse] = {
    try {
      println("Process Timestamp: " + series.series.apply(series.series.length-1).timestamp.toString + ", size: " + series.series.length)
      val response = processUsingApi(gson.toJson(series), latestPointDetectionUrl)
      println(response)
      // Deserializing the JSON response from the API into Scala types
      val anomaly = gson.fromJson(response, classOf[AnomalySingleResponse])
      Thread.sleep(5000)
      return Some(anomaly)
    } catch {
      case e: Exception => {
        println(e)
        e.printStackTrace()
        return None
      }
    }
  }

  // Calls the Batch Detection API for timeserie.
  def detectBatch(series: Series): Option[AnomalyBatchResponse] = {
    try {
      val response = processUsingApi(gson.toJson(series), batchDetectionUrl)
      println(response)
      // Deserializing the JSON response from the API into Scala types
      val anomaly = gson.fromJson(response, classOf[AnomalyBatchResponse])
      Thread.sleep(5000)
      return Some(anomaly)
    } catch {
      case e: Exception => {
        println(e)
        return None
      }
    }
  }
}
```

Not defterlerini çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Aşağıdaki kod parçacığında gösterildiği gibi bir çıktı görürsünüz. : 

    import java.io.{BufferedReader, DataOutputStream, InputStreamReader}
    import java.net.URL
    import java.sql.Timestamp
    import com.google.gson.{Gson, GsonBuilder, JsonParser}
    import javax.net.ssl.HttpsURLConnection
    defined class Point
    defined class Series
    defined class AnomalySingleResponse
    defined class AnomalyBatchResponse
    defined object AnomalyDetector

Ardından bir toplama işlevi, gelecekteki kullanımlarınız için hazırlayın.
```scala
//
// User Defined Aggregation Function for Anomaly Detection
//

import org.apache.spark.sql.Row
import org.apache.spark.sql.expressions.{MutableAggregationBuffer, UserDefinedAggregateFunction}
import org.apache.spark.sql.types.{StructType, TimestampType, FloatType, MapType, BooleanType, DataType}
//import org.apache.spark.sql.functions._
import scala.collection.immutable.ListMap

class AnomalyDetectorAggregationFunction_Hourly extends UserDefinedAggregateFunction {
  override def inputSchema: StructType = new StructType().add("timestamp", TimestampType).add("value", FloatType)
  
  override def bufferSchema: StructType = new StructType().add("point", MapType(TimestampType, FloatType))
  
  override def dataType: DataType = BooleanType
  
  override def deterministic: Boolean = false
  
  override def initialize(buffer: MutableAggregationBuffer): Unit = {
    buffer(0) = Map()
  }
  
  override def update(buffer: MutableAggregationBuffer, input: Row): Unit = {
    buffer(0) = buffer.getAs[Map[java.sql.Timestamp, Float]](0) + (input.getTimestamp(0) -> input.getFloat(1))
  }
  
  override def merge(buffer1: MutableAggregationBuffer, buffer2: Row): Unit = {
    buffer1(0) = buffer1.getAs[Map[java.sql.Timestamp, Float]](0) ++ buffer2.getAs[Map[java.sql.Timestamp, Float]](0)
  }
  
  override def evaluate(buffer: Row): Any = {
    val points = buffer.getAs[Map[java.sql.Timestamp, Float]](0)
    if (points.size > 12) {
      val sorted_points = ListMap(points.toSeq.sortBy(_._1.getTime):_*)
      var detect_points: List[Point] = List()
      sorted_points.keys.foreach {
        key => detect_points = detect_points :+ new Point(key, sorted_points(key))
      }
      
      
      // 0.25 is maxAnomalyRatio. It represents 25%, max anomaly ratio in a time series.
      // 95 is the sensitivity of the algorithms. 
      // Check Anomaly detector API reference (https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-last-detect)
      
      val series: Series = new Series(detect_points.toArray, 0.25, 95, "hourly")
      val response: Option[AnomalySingleResponse] = AnomalyDetector.detectLatestPoint(series)
      if (!response.isEmpty) {
        return response.get.isAnomaly
      }
    }
    
    return None
  }
}

```

Not defterlerini çalıştırmak için **SHIFT + ENTER** tuşlarına basın. Aşağıdaki kod parçacığında gösterildiği gibi bir çıktı görürsünüz. 

    import org.apache.spark.sql.Row
    import org.apache.spark.sql.expressions.{MutableAggregationBuffer, UserDefinedAggregateFunction}
    import org.apache.spark.sql.types.{StructType, TimestampType, FloatType, MapType, BooleanType, DataType}
    import scala.collection.immutable.ListMap
    defined class AnomalyDetectorAggregationFunction

Ardından, anomali algılama için olay hub'ından veri yükleyebilirsiniz.

```scala
//
// Load Data from Eventhub
//

import org.apache.spark.eventhubs._
import org.apache.spark.sql.types._
import org.apache.spark.sql.functions._

val connectionString = ConnectionStringBuilder("[Placeholder: EventHub namespace connection string]")
  .setEventHubName("[Placeholder: EventHub name]")
  .build

val customEventhubParameters =
  EventHubsConf(connectionString)
  .setConsumerGroup("$Default")
  .setMaxEventsPerTrigger(100)

val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()

val messages =
  incomingStream
  .withColumn("enqueuedTime", $"enqueuedTime".cast(TimestampType))
  .withColumn("body", $"body".cast(StringType))
  .select("enqueuedTime", "body")

val bodySchema = new StructType().add("timestamp", TimestampType).add("favorite", IntegerType)

val msgStream = messages.select(from_json('body, bodySchema) as 'fields).select("fields.*")

msgStream.printSchema

display(msgStream)

```

Çıkış şimdi şu resme benzer şekilde görünür. Gerçek zamanlı verileri olduğu gibi ödeme dikkat tarihinizden tabloda tarihten itibaren bu öğreticideki farklı olabilir.
![Yük gelen verileri olay hub'ı](../media/tutorials/load-data-from-eventhub.png "yük veri gelen olay hub'ı")

Artık, neredeyse gerçek zamanlı olarak Apache Spark için Event Hubs bağlayıcısını kullanarak Azure Databricks'e Azure Event Hubs verilerini yaptınız. Spark için Event Hubs bağlayıcısını kullanma hakkında daha fazla bilgi için [bağlayıcı belgesi](https://github.com/Azure/azure-event-hubs-spark/tree/master/docs)ne başvurun.



## <a name="run-anomaly-detection-on-tweets"></a>Anomali algılama tweet'lerde çalıştırın

Bu bölümde, anomali algılama API'si Anomali algılayıcısı kullanılarak alınan tweet'lerde çalıştırın. Bu bölüm için, aynı **AnalyzeTweetsFromEventHub** not defterine kod parçacıklarını eklersiniz.

Anomali algılama yapmak için ilk olarak, ölçüm sayınız saatlik olarak gerekir.
```scala
//
// Aggregate Metric Count by Hour
//

// If you want to change granularity, change the groupBy window. 
val groupStream = msgStream.groupBy(window($"timestamp", "1 hour"))
  .agg(avg("favorite").alias("average"))
  .withColumn("groupTime", $"window.start")
  .select("groupTime", "average")

groupStream.printSchema

display(groupStream)
```
Çıkış şimdi aşağıdaki kod parçacıklarının benzer.
```
groupTime                       average
2019-04-23T04:00:00.000+0000    24
2019-04-26T19:00:00.000+0000    47.888888888888886
2019-04-25T12:00:00.000+0000    32.25
2019-04-26T09:00:00.000+0000    63.4
...
...

```

Ardından, Delta toplanmış çıkış sonucu alın. Anomali algılama daha uzun bir geçmiş penceresinde gerektirdiğinden, Delta algılamak için istediğiniz noktasının geçmiş verileri tutmak için kullanıyoruz. 


```scala
//
// Output Aggregation Result to Delta
//

groupStream.writeStream
  .format("delta")
  .outputMode("complete")
  .option("checkpointLocation", "/delta/[Placeholder: table name]/_checkpoints/[Placeholder: folder name for checkpoints]")
  .table("[Placeholder: table name]")

```

```scala
//
// Show Aggregate Result
//

val twitterCount = spark.sql("SELECT COUNT(*) FROM [Placeholder: table name]")
twitterCount.show()

val twitterData = spark.sql("SELECT * FROM [Placeholder: table name] ORDER BY groupTime")
twitterData.show(200, false)

display(twitterData)
```
Aşağıda gösterildiği gibi çıkış: 
```
groupTime                       average
2019-04-08T01:00:00.000+0000    25.6
2019-04-08T02:00:00.000+0000    6857
2019-04-08T03:00:00.000+0000    71
2019-04-08T04:00:00.000+0000    55.111111111111114
2019-04-08T05:00:00.000+0000    2203.8
...
...

```

Şimdi toplu zaman serisi verilerinin sürekli olarak Delta alınır. Ardından en son noktasının anomali algılama için saatte bir iş zamanlayabilirsiniz. 

```scala
//
// Anomaly Detection with Batch query
//

import java.time.Instant
import java.time.temporal.ChronoUnit

val detectData = spark.read.format("delta").table("[Placeholder: table name]")

// How long history you want to use in anomaly detection. It is hourly time series in this tutorial, so 72 means 72 hours. 
val batchSize = 72

// Change the endTime to where you want to detect. You could use Databricks to schedule a job and change it to the latest hour. 
val endTime = Instant.parse("2019-04-16T00:00:00Z")
val startTime = endTime.minus(batchSize, ChronoUnit.HOURS)

val series = detectData.filter($"groupTime" < endTime.toString && $"groupTime" >= startTime.toString).sort($"groupTime")

series.createOrReplaceTempView("series")

//series.show()

// Register the function to access it
spark.udf.register("anomalydetect", new AnomalyDetectorAggregationFunction)

val adResult = spark.sql("SELECT '" + endTime.toString + "' as timestamp, anomalydetect(groupTime, average) as anomaly FROM series")
adResult.show()
```
Aşağıdaki gibi neden: 

```
+--------------------+-------+
|           timestamp|anomaly|
+--------------------+-------+
|2019-04-16T00:00:00Z|  false|
+--------------------+-------+

```
Delta geri dön anomali algılama sonucu çıktı. 
```scala
//
// Output Batch AD Result to delta
//

adResult.writeStream
  .format("delta")
  .outputMode("complete")
  .option("checkpointLocation", "/delta/[Placeholder: table name]/_checkpoints/[Placeholder: folder name for checkpoints]")
  .table("[Placeholder: table name]")
  
```


İşte bu kadar! Azure Databricks kullanarak verileri başarıyla yaptınız Azure Event Hubs'a, Event Hubs bağlayıcısını kullanarak akış verilerini kullandınız ve sonra akış verileriyle neredeyse gerçek zamanlı anomali algılama çalıştırdınız.
Bu öğreticideki olsa da, ayrıntı düzeyi saatlik, her zaman ayrıntı düzeyi, gereksinime uygun şekilde değiştirebilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi çalıştırdıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’i seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesini seçin.

![Databricks kümesini durdurma](../media/tutorials/terminate-databricks-cluster.png "Databricks kümesini durdurma")

El ile otomatik olarak durdurur küme sonlandırmaz, seçtiğiniz sağlanan **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** küme oluşturulurken onay kutusu. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, verileri Azure Event Hubs’ta akışa almak ve sonra gerçek zamanlı olarak Event Hubs’tan akış verilerini okumak için Azure Databricks’i kullanmayı öğrendiniz. Anomali algılayıcısı API çağrısı ve anomalileri Power BI desktop'ı kullanarak görselleştirme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
>[Power BI desktop ile batch anomali algılama](batch-anomaly-detection-powerbi.md)
