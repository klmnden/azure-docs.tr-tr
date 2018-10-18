---
title: 'Hızlı Başlangıç: Uzak görüntüyü analiz etme - REST, Java - Görüntü İşleme'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java ile Görüntü İşleme API’si kullanarak bir görüntüyü analiz edeceksiniz.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.openlocfilehash: 6dae10262c40a127cb24dd40f0d5389c0cdd7c20
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340027"
---
# <a name="quickstart-analyze-a-remote-image-using-the-rest-api-and-java-in-computer-vision"></a>Hızlı Başlangıç: Görüntü İşleme’de REST API ve Java kullanarak uzak görüntüyü analiz etme

Bu hızlı başlangıçta, Görüntü İşleme’nin REST API’sini kullanarak görsel özellikleri ayıklamak için uzakta depolanan bir görüntüyü analiz edeceksiniz. [Görüntü Analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yöntemi ile, görüntü içeriğini temel alarak görsel özellikleri ayıklayabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- [Java&trade; Platform, Standard Edition Geliştirme Seti 7 veya 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (JDK 7 veya 8) yüklü olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Bir abonelik anahtarı almak için bkz. [Abonelik Anahtarları Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample-application"></a>Örnek uygulamayı oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Tercih ettiğiniz IDE veya düzenleyicide bir Java projesi oluşturun. Seçenek yoksa, Java projesini bir komut satırı uygulaması şablondan oluşturun.
1. Aşağıdaki kitaplıkları Java projenize içeri aktarın. Maven kullanıyorsanız, her bir kitaplık için Maven koordinatları sağlanır.
   - [Apache HTTP istemcisi](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.5)
   - [Apache HTTP çekirdek](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.9)
   - [JSON kitaplığı](https://github.com/stleary/JSON-java) (org.json:json:20180130)
1. Aşağıdaki `import` deyimlerini projeniz için `Main` ortak sınıfını içeren dosyaya ekleyin.  

   ```java
   import java.net.URI;
   import org.apache.http.HttpEntity;
   import org.apache.http.HttpResponse;
   import org.apache.http.client.methods.HttpPost;
   import org.apache.http.entity.StringEntity;
   import org.apache.http.client.utils.URIBuilder;
   import org.apache.http.impl.client.CloseableHttpClient;
   import org.apache.http.impl.client.HttpClientBuilder;
   import org.apache.http.util.EntityUtils;
   import org.json.JSONObject;
   ```

1. `Main` ortak sınıfını aşağıdaki kodla değiştirin, ardından kodda gereken yerlerde aşağıdaki değişiklikleri yapın:
   1. `subscriptionKey` değerini abonelik anahtarınızla değiştirin.
   1. Gerekirse `uriBase` değerini [Görüntü Analizi](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) yönteminin abonelik anahtarlarınızı aldığınız Azure bölgesinden uç nokta URL'si ile değiştirin.
   1. İsteğe bağlı olarak `imageToAnalyze` değerini, analiz etmek istediğiniz başka bir görüntünün URL’si ile değiştirin.
1. Kaydedin, ardından Java projesini derleyin.
1. Bir IDE kullanıyorsanız, `Main` yöntemini çalıştırın. Kullanmıyorsanız, bir komut istemi penceresi açın ve ardından derlenmiş sınıfı çalıştırmak için `java` komutunu kullanın. Örneğin, `java Main`.

```java
public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    // You must use the same Azure region in your REST API method as you used to
    // get your subscription keys. For example, if you got your subscription keys
    // from the West US region, replace "westcentralus" in the URL
    // below with "westus".
    //
    // Free trial subscription keys are generated in the West Central US region.
    // If you use a free trial subscription key, you shouldn't need to change
    // this region.
    private static final String uriBase =
            "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze";

    private static final String imageToAnalyze =
            "https://upload.wikimedia.org/wikipedia/commons/" +
                    "1/12/Broadway_and_Times_Square_by_night.jpg";

    public static void main(String[] args) {
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();

        try {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color");
            builder.setParameter("language", "en");

            // Prepare the URI for the REST API method.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Call the REST API method and get the response entity.
            HttpResponse response = httpClient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null) {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("REST Response:\n");
                System.out.println(json.toString(2));
            }
        } catch (Exception e) {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek uygulama aşağıdaki örneğe benzer şekilde başarılı bir yanıtı ayrıştırıp konsol penceresinde görüntüler:

```json
REST Response:

{
  "metadata": {
    "width": 1826,
    "format": "Jpeg",
    "height": 2436
  },
  "color": {
    "dominantColorForeground": "Brown",
    "isBWImg": false,
    "accentColor": "B74314",
    "dominantColorBackground": "Brown",
    "dominantColors": ["Brown"]
  },
  "requestId": "bbffe1a1-4fa3-4a6b-a4d5-a4964c58a811",
  "description": {
    "captions": [{
      "confidence": 0.8241405091548035,
      "text": "a group of people on a city street filled with traffic at night"
    }],
    "tags": [
      "outdoor",
      "building",
      "street",
      "city",
      "busy",
      "people",
      "filled",
      "traffic",
      "many",
      "table",
      "car",
      "group",
      "walking",
      "bunch",
      "crowded",
      "large",
      "night",
      "light",
      "standing",
      "man",
      "tall",
      "umbrella",
      "riding",
      "sign",
      "crowd"
    ]
  },
  "categories": [{
    "score": 0.625,
    "name": "outdoor_street"
  }]
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse Java projesini, derlenen sınıf ve içeri aktarılan kitaplıklarla birlikte silin.

## <a name="next-steps"></a>Sonraki adımlar

Optik karakter tanıma (OCR) gerçekleştirmek için Görüntü İşleme kullanan bir Java Swing uygulaması keşfedin. Akıllı kırpılmış küçük resimler oluşturun. Buna ek olarak, bir görüntüdeki yüzler gibi görsel özellikleri algılayın, kategorilere ayırın, etiketleyin ve açıklayın. Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna bakın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'si Java Öğreticisi](../Tutorials/java-tutorial.md)
