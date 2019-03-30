---
title: "Hızlı Başlangıç: Java ve Azure REST API'si ile bir görüntüdeki yüzleri algılayın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir resimdeki yüz algılama için Java ile Azure yüz REST API'sini kullanır.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 03/27/2019
ms.author: pafarley
ms.openlocfilehash: c0c1b9c1e9afc84e9702f6c1897d372a017be868
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629889"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Hızlı Başlangıç: Java ve REST API ile bir resimdeki yüz algılama

Bu hızlı başlangıçta, bir resimdeki İnsan yüzlerini algılamak için Java ile Azure yüz REST API'sini kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

- Yüz tanıma API'si abonelik anahtarı. Ücretsiz deneme aboneliği anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yüz tanıma API'si hizmete abone ve anahtarınızı alın.
- Herhangi bir Java IDE, tercih ettiğiniz.

## <a name="create-the-java-project"></a>Java projesi oluşturma

1. IDE'nizi içinde yeni bir komut satırı Java uygulaması oluşturma ve ekleme bir **ana** sınıfıyla birlikte bir **ana** yöntemi.
1. Aşağıdaki kitaplıkları Java projenize içeri aktarın. Maven kullanıyorsanız, her bir kitaplık için Maven koordinatları sağlanır.
   - [Apache HTTP istemcisini](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.6)
   - [Apache HTTP çekirdek](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.10)
   - [JSON kitaplığı](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Apache Commons günlük](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (commons-günlük kaydı: commons-günlük kaydı: 1.1.2)

## <a name="add-face-detection-code"></a>Yüz algılama kodu ekleyin

Projenizin ana sınıfı açın. Burada, görüntülerini yükle ve yüz algılama için gereken kodu ekleyeceksiniz.

### <a name="import-packages"></a>Paketleri içeri aktarma

Aşağıdaki `import` deyimlerini dosyanın üstüne.

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
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONArray;
import org.json.JSONObject;
```

### <a name="add-essential-fields"></a>Gerekli alanları ekleyin

Değiştirin **ana** aşağıdaki kodla sınıfı. Bu veriler yüz tanıma Hizmeti'ne bağlanmayı ve giriş verilerinin alınacağı belirtir. Güncellemeniz gerekecektir `subscriptionKey` alan abonelik anahtarınız ve değeri ile değiştirmek gerekebilir `uriBase` doğru bölge tanımlayıcısı içeren dize (bkz [yüz tanıma API'si belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) tüm bölge listesi uç noktaları). Ayarlamak isteyebilirsiniz `imageWithFaces` değeri farklı bir resim dosyasına işaret eden bir yolu.

`faceAttributes` Alanı yalnızca belirli tür öznitelik listesi verilmiştir. Algılanan yüzeylere hakkında almak için hangi bilgilerin, belirtin.

```Java
public class Main {
    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    // NOTE: You must use the same region in your REST call as you used to
    // obtain your subscription keys. For example, if you obtained your
    // subscription keys from westus, replace "westcentralus" in the URL
    // below with "westus".
    //
    // Free trial subscription keys are generated in the "westus" region. If you
    // use a free trial subscription key, you shouldn't need to change this region.
    private static final String uriBase =
        "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect";

    private static final String imageWithFaces =
        "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}";

    private static final String faceAttributes =
        "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";
```

### <a name="call-the-face-detection-rest-api"></a>Yüz algılama REST API çağrısı

Ekleme **ana** yöntemini aşağıdaki kod ile. Uzak görüntüde yüz bilgilerini algılamak için yüz tanıma API'si için REST çağrısı oluşturur ( `faceAttributes` dizesini almak için hangi yüz öznitelikleri belirtir). Ardından bir JSON dizesine çıktı verilerini yazar.

```Java
    public static void main(String[] args) {
        HttpClient httpclient = HttpClientBuilder.create().build();

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
```

### <a name="parse-the-json-response"></a>JSON yanıtı ayrıştırılamadı

Doğrudan konsola yazdırmadan önce döndürülen JSON verilerini daha kolay okunabilir bir biçimine dönüştürür aşağıdaki bloğu önceki kodun altına ekleyin. Son olarak, try-catch bloğu kapatmak **ana** yöntemi ve **ana** sınıfı.

```Java
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

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Kodu derleyin ve çalıştırın. Başarılı bir yanıt yüz verileri kolay okunabilir JSON biçiminde konsol penceresinde görüntüler. Örneğin:

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

Bu hızlı başlangıçta, bir resimdeki yüz algılama ve onların öznitelikleri döndürmek için Azure yüz tanıma API'SİYLE REST çağrılarını kullanan basit bir Java konsol uygulaması oluşturdunuz. Ardından, daha fazlasını yapma hakkında bilgi bu işleviyle bir Android uygulaması.

> [!div class="nextstepaction"]
> [Öğretici: Algılayıp yüzleri çerçeve Android uygulaması oluşturma](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)
