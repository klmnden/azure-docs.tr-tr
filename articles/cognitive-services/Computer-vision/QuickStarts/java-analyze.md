---
title: Görüntü İşleme API'si Java hızlı başlangıcı | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Bu hızlı başlangıçta, Bilişsel Hizmetler’de Java ile Görüntü İşleme kullanarak bir görüntü analiz edeceksiniz.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 9faa8d05a747855ceb0a468845c3d3a82aa71337
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772611"
---
# <a name="quickstart-analyze-a-remote-image---rest-java"></a>Hızlı Başlangıç: Uzak görüntü analiz etme - REST, Java

Bu hızlı başlangıçta, Görüntü İşleme kullanarak görsel özellikleri ayıklamak için bir görüntü analiz edeceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Görüntü İşleme’yi kullanmak için, bir abonelik anahtarınızın olması gerekir; bkz. [Abonelik Anahtarlarını Alma](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-image-request"></a>Görüntü Analizi isteği

[Görüntü Analizi yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) ile, görüntü içeriğini temel alarak görsel özellikleri ayıklayabilirsiniz. Bir görüntüyü karşıya yükleyebilir veya bir görüntü URL’si belirleyebilir ve aşağıdaki özelliklerden hangilerinin döndürüleceğini seçebilirsiniz:

* Görüntü içeriğiyle ilgili etiketlerin ayrıntılı listesi.
* Görüntü içeriğinin tam bir cümlelik açıklaması.
* Görüntünün içerdiği yüzlerin koordinatları, cinsiyeti ve yaşı.
* ImageType (küçük resim veya çizgi çizim).
* Baskın renk, vurgu rengi veya bir görüntünün siyah beyaz olup olmadığı.
* Bu [sınıflandırmada](../Category-Taxonomy.md) tanımlanan kategori.
* Görüntü, yetişkinlere yönelik veya müstehcen cinsel içerik içeriyor mu?

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Yeni bir komut satırı uygulaması oluşturun.
1. Main sınıfını aşağıdaki kod ile değiştirin (tüm `package` deyimlerini tutun).
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse `uriBase` değerini abonelik anahtarlarınızı aldığınız konumla değiştirin.
1. İsterseniz `imageToAnalyze` değerini başka bir görüntüyle değiştirin.
1. Bu kitaplıkları Maven Deposu’ndan projenizdeki `lib` dizinine indirin:
   * `org.apache.httpcomponents:httpclient:4.5.5`
   * `org.apache.httpcomponents:httpcore:4.4.9`
   * `org.json:json:20180130`
1. 'Main' komutunu çalıştırın.

```java
// This sample uses the following libraries:
//  - Apache HTTP client (org.apache.httpcomponents:httpclient:4.5.5)
//  - Apache HTTP core (org.apache.httpcomponents:httpccore:4.4.9)
//  - JSON library (org.json:json:20180130).

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

public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    // You must use the same region in your REST call as you used to get your
    // subscription keys. For example, if you got your subscription keys from
    // westus, replace "westcentralus" in the URI below with "westus".
    //
    // Free trial subscription keys are generated in the westcentralus region. If you
    // use a free trial subscription key, you shouldn't need to change this region.
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

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Make the REST API call and get the response entity.
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

## <a name="analyze-image-response"></a>Görüntü Analizi yanıtı

JSON’da başarılı bir yanıt döndürülür, örneğin:

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

## <a name="next-steps"></a>Sonraki adımlar

Optik karakter tanıma (OCR) gerçekleştirmek için Görüntü İşleme kullanan bir Java Swing uygulaması keşfedin. Akıllı kırpılmış küçük resimler oluşturun. Buna ek olarak, bir görüntüdeki yüzler gibi görsel özellikleri algılayın, kategorilere ayırın, etiketleyin ve açıklayın. Görüntü İşleme API'lerini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna göz atın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'si Java Öğreticisi](../Tutorials/java-tutorial.md)
