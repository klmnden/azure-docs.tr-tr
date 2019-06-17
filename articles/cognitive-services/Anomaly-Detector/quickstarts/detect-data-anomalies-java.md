---
title: 'Hızlı Başlangıç: Java ve Anomali algılayıcısı REST API kullanarak, zaman serisi verilerinde görülen anormallikleri | Microsoft Docs'
description: Toplu olarak ya da akış verileri, veri serisinde prosesler algılamak için Anomali algılayıcısı API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 04ace16559a6f5b747bc735aa89265d2962a32b3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67073219"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-java"></a>Hızlı Başlangıç: Java ve Anomali algılayıcısı REST API kullanarak, zaman serisi verilerinde görülen anomalileri algılayın

Zaman serisi verilerinizdeki anormallikleri algılamak için Anomali algılayıcısı API'SİNİN iki algılama modunu kullanmaya başlamak için bu Hızlı Başlangıç'ı kullanın. Bu Java uygulaması içeren JSON biçimli zaman serisi verilerini iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıktısı                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Toplu iş olarak anomalileri algılayın                        | Zaman serisi verileri ve algılanan anomalileri konumlarını her veri noktası için anomali durumu (ve diğer verileri) içeren bir JSON yanıtı. |
| En son veri noktası durumunu anomali algılama | Zaman serisi verilerinde en son veri noktası için anomali durumu (ve diğer verileri) içeren bir JSON yanıtı.                                                                                                                                         |

 Bu uygulama, Java dilinde yazılır, ancak çoğu programlama dilleri ile uyumlu bir RESTful web hizmeti API'dir.

## <a name="prerequisites"></a>Önkoşullar

- [Java&trade; geliştirme Kit(JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya üzeri.

- Bu kitaplıklar Maven deposundan içeri aktarma
    - [Java'da JSON](https://mvnrepository.com/artifact/org.json/json) paket
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paket

- Bir JSON dosyası içeren zaman serisi verilerini işaret eder. Bu Hızlı Başlangıç için örnek veri çubuğunda bulunabilir [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

    ```java
    import org.apache.http.HttpEntity;
    import org.apache.http.client.methods.CloseableHttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;
    import org.json.JSONArray;
    import org.json.JSONObject;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    ```

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Anomali algılama için kullanabileceğiniz bir URI'leri aşağıdadır. Bunlar daha sonra API oluşturmak için hizmet uç noktanıza eklenir istek URL'lerini.

    |Algılama yöntemi  |URI  |
    |---------|---------|
    |Batch algılama    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |En son veri noktasına algılama     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    ```java
    // Replace the subscriptionKey string value with your valid subscription key.
    static final String subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";
    //replace the endpoint URL with the correct one for your subscription. Your endpoint can be found in the Azure portal. 
    //For example: https://westus2.api.cognitive.microsoft.com
    static final String endpoint = "[YOUR_ENDPOINT_URL]";
    // Replace the dataPath string with a path to the JSON formatted time series data.
    static final String dataPath = "[PATH_TO_TIME_SERIES_DATA]";
    static final String latestPointDetectionUrl = "/anomalydetector/v1.0/timeseries/last/detect";
    static final String batchDetectionUrl = "/anomalydetector/v1.0/timeseries/entire/detect";
    ```

3. JSON veri dosyasını oku

    ```java
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "utf-8");
    ```

## <a name="create-a-function-to-send-requests"></a>İstekleri göndermek için bir işlev oluşturma

1. Adlı yeni bir işlev oluşturma `sendRequest()` yukarıda oluşturulan değişkenleri alır. Ardından aşağıdaki adımları gerçekleştirin.

2. Oluşturma bir `CloseableHttpClient` nesnesini API için istek gönderebilirsiniz. İstek gönderin bir `HttpPost` uç noktanızı ve bir Anomali algılayıcısı URL birleştirerek istek nesnesi.

3. İsteğin kullanın `setHeader()` ayarlamak için işlevi `Content-Type` başlığına `application/json`, abonelik anahtarınızı ekleyin `Ocp-Apim-Subscription-Key` başlığı.

4. İsteğin kullanın `setEntity()` işleve gönderilecek veri.

5. İstemcinin kullanmak `execute()` isteği göndermek için işlev ve kaydetmesi bir `CloseableHttpResponse` nesne.

6. Oluşturma bir `HttpEntity` yanıt içeriği depolamak için nesne. İçeriği ile `getEntity()`. Yanıt boş değilse, onu döndürür.

```java
static String sendRequest(String apiAddress, String endpoint, String subscriptionKey, String requestData) {
    try (CloseableHttpClient client = HttpClients.createDefault()) {
        HttpPost request = new HttpPost(endpoint + apiAddress);
        // Request headers.
        request.setHeader("Content-Type", "application/json");
        request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
        request.setEntity(new StringEntity(requestData));
        try (CloseableHttpResponse response = client.execute(request)) {
            HttpEntity respEntity = response.getEntity();
            if (respEntity != null) {
                return EntityUtils.toString(respEntity, "utf-8");
            }
        } catch (Exception respEx) {
            respEx.printStackTrace();
        }
    } catch (IOException ex) {
        System.err.println("Exception on Anomaly Detector: " + ex.getMessage());
        ex.printStackTrace();
    }
    return null;
}
```

## <a name="detect-anomalies-as-a-batch"></a>Toplu iş olarak anomalileri algılayın

1. Adlı bir yöntem oluşturma `detectAnomaliesBatch()` toplu olarak veri boyunca anomalileri algılamak için. Çağrı `sendRequest()` , uç noktası, url, abonelik anahtarı ve json verilerini yukarıda oluşturulan yöntemi. Sonucu alın ve konsola yazdırır.

2. Yanıt içeriyorsa `code` alan, hata kodu ve hata iletisi yazdırın.

3. Aksi takdirde, anomalileri konumları, veri kümesinde bulun. Yanıtın `isAnomaly` alan belirli veri noktasına bir anomali olup ilgili bir Boole değeri içerir. JSON dizisi alın ve herhangi bir dizin yazdırma, yineleme `true` değerleri. Karşılanmadığı, bu değerler, anormal veri noktası dizini karşılık gelir.

```java
static void detectAnomaliesBatch(String requestData) {
    System.out.println("Detecting anomalies as a batch");
    String result = sendRequest(batchDetectionUrl, endpoint, subscriptionKey, requestData);
    if (result != null) {
        System.out.println(result);
        JSONObject jsonObj = new JSONObject(result);
        if (jsonObj.has("code")) {
            System.out.println(String.format("Detection failed. ErrorCode:%s, ErrorMessage:%s", jsonObj.getString("code"), jsonObj.getString("message")));
        } else {
            JSONArray jsonArray = jsonObj.getJSONArray("isAnomaly");
            System.out.println("Anomalies found in the following data positions:");
            for (int i = 0; i < jsonArray.length(); ++i) {
                if (jsonArray.getBoolean(i))
                    System.out.print(i + ", ");
            }
            System.out.println();
        }
    }
}
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktası durumunu anomali algılama

* Adlı bir yöntem oluşturma `detectAnomaliesLatest()` veri kümesinde son veri noktası anomali durumunu algılamak için. Çağrı `sendRequest()` , uç noktası, url, abonelik anahtarı ve json verilerini yukarıda oluşturulan yöntemi. Sonucu alın ve konsola yazdırır.

```java
static void detectAnomaliesLatest(String requestData) {
    System.out.println("Determining if latest data point is an anomaly");
    String result = sendRequest(latestPointDetectionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Zaman serisi verilerinizi yüklemek ve isteği gönder

1. Uygulamanızın ana yöntemde isteklerine eklenen veriler içeren JSON dosyasındaki okuyun.

2. Yukarıda oluşturulan iki anomali algılama işlevlerini çağırın.

```java
public static void main(String[] args) throws Exception {
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "utf-8");
    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);
}
```

### <a name="example-response"></a>Örnek yanıt

Başarılı bir yanıt JSON biçiminde döndürülür. GitHub üzerinde JSON yanıtı görüntülemek için aşağıdaki bağlantılara tıklayın:
* [Örnek batch algılama yanıt](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [En son noktası algılama yanıt örneği](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)
