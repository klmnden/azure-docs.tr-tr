---
title: 'Hızlı Başlangıç: REST API ve Java kullanarak bir görüntüdeki yüzleri algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Java ile Yüz Tanıma API’sini kullanarak bir görüntüdeki yüzleri algılayacaksınız.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 05/10/2018
ms.author: pafarley
ms.openlocfilehash: df9490a3ee2af115b48dafd323e1afdec24b392d
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956230"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Hızlı Başlangıç: REST API ve Java kullanarak bir görüntüdeki yüzleri algılama

Bu hızlı başlangıçta, Yüz Tanıma API'sini kullanarak bir görüntüdeki insan yüzlerini algılayacaksınız.

## <a name="prerequisites"></a>Ön koşullar

Örneği çalıştırmanız için bir abonelik anahtarınız olmalıdır. [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=face-api)'den ücretsiz deneme abonelik anahtarları alabilirsiniz.

## <a name="detect-faces-in-an-image"></a>Bir görüntüdeki yüzleri algılama

[Yüz - Algılama](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) yöntemini kullanarak bir görüntüdeki yüzleri algılayın ve aşağıdaki yüz özniteliklerini döndürün:

* Face ID: Birkaç Yüz Tanıma API'si senaryosunda kullanılan benzersiz kimlik.
* Yüz Dikdörtgeni: Görüntüdeki yüzün konumunu gösteren sol kısım, üst kısım, genişlik ve yükseklik.
* Yer İşaretleri: Yüz bileşenlerinin önemli konumlarına işaret eden 27 noktalık yüz yer işareti dizisi.
* Yaş, cinsiyet, gülümseme yoğunluğu, kafanın duruşu ve sakal ve bıyık gibi yüzdeki öznitelikler.

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Sık kullandığınız Java IDE’de yeni bir komut satırı uygulaması oluşturun.
2. Main sınıfını aşağıdaki kod ile değiştirin (tüm `package` deyimlerini tutun).
3. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
4. Gerekirse abonelik anahtarlarınızı aldığınız konumu kullanmak için `uriBase` değerini değiştirin.
5. Bu genel kitaplıkları Maven Deposu’ndan projenizdeki `lib` dizinine indirin:
   * `org.apache.httpcomponents:httpclient:4.2.4`
   * `org.json:json:20170516`
6. 'Main' komutunu çalıştırın.

### <a name="face---detect-request"></a>Yüz - Algılama isteği

```java
// This sample uses Apache HttpComponents:
// http://hc.apache.org/httpcomponents-core-ga/httpcore/apidocs/
// https://hc.apache.org/httpcomponents-client-ga/httpclient/apidocs/

import java.net.URI;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.client.utils.URIBuilder;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;

public class Main
{
    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    // NOTE: You must use the same region in your REST call as you used to
    // obtain your subscription keys. For example, if you obtained your
    // subscription keys from westus, replace "westcentralus" in the URL
    // below with "westus".
    //
    // Free trial subscription keys are generated in the westcentralus region. If you
    // use a free trial subscription key, you shouldn't need to change this region.
    private static final String uriBase =
        "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect";

    private static final String imageWithFaces =
        "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}";

    private static final String faceAttributes =
        "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";

    public static void main(String[] args)
    {
        HttpClient httpclient = new DefaultHttpClient();

        try
        {
            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameters. All of them are optional.
            builder.setParameter("returnFaceId", "true");
            builder.setParameter("returnFaceLandmarks", "false");
            builder.setParameter("returnFaceAttributes", faceAttributes);

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity reqEntity = new StringEntity(imageWithFaces);
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            if (entity != null)
            {
                // Format and display the JSON response.
                System.out.println("REST Response:\n");

                String jsonString = EntityUtils.toString(entity).trim();
                if (jsonString.charAt(0) == '[') {
                    JSONArray jsonArray = new JSONArray(jsonString);
                    System.out.println(jsonArray.toString(2));
                }
                else if (jsonString.charAt(0) == '{') {
                    JSONObject jsonObject = new JSONObject(jsonString);
                    System.out.println(jsonObject.toString(2));
                } else {
                    System.out.println(jsonString);
                }
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
        }
    }
}
```

### <a name="face---detect-response"></a>Yüz - Algılama yanıtı

Başarılı bir yanıt JSON biçiminde döndürülür.

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
    "hair": {
      "bald": 0,
      "invisible": false,
      "hairColor": [
        {
          "color": "brown",
          "confidence": 1
        },
        {
          "color": "black",
          "confidence": 0.87
        },
        {
          "color": "other",
          "confidence": 0.51
        },
        {
          "color": "blond",
          "confidence": 0.08
        },
        {
          "color": "red",
          "confidence": 0.08
        },
        {
          "color": "gray",
          "confidence": 0.02
        }
      ]
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]
```

## <a name="next-steps"></a>Sonraki adımlar

Bir görüntüde yüzleri algılamak için Yüz Tanıma hizmetini kullanan bir Android uygulamasının nasıl oluşturulacağını öğreneceksiniz. Uygulama, her yüzün çevresine bir çerçeve çizilmiş şekilde görüntüyü görüntüler.

> [!div class="nextstepaction"]
> [Öğretici: Android’de Yüz Tanıma API’sini Kullanmaya Başlama](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)
