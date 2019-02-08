---
title: 'Hızlı Başlangıç: JavaScript ve REST API ile bir resimdeki yüz algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Bilişsel Hizmetler’de JavaScript ile Yüz Tanıma API’sini kullanarak bir görüntüdeki yüzleri algılayacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 05/10/2018
ms.author: pafarley
ms.openlocfilehash: 07868fd70c1b2601fa676f7069f2508468e2be0e
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866959"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-javascript"></a>Hızlı Başlangıç: JavaScript ve REST API ile bir resimdeki yüz algılama

Bu hızlı başlangıçta, Yüz Tanıma API’sini kullanarak bir görüntüdeki yüzleri algılayacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Örneği çalıştırmanız için bir abonelik anahtarınız olmalıdır. [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=face-api)'den ücretsiz deneme abonelik anahtarları alabilirsiniz.

## <a name="detect-faces-in-an-image"></a>Bir görüntüdeki yüzleri algılama

[Yüz - Algılama](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) yöntemini kullanarak bir görüntüdeki yüzleri algılayın ve aşağıdaki yüz özniteliklerini döndürün:

* Face ID: Birkaç yüz tanıma API'si senaryoda kullanılan benzersiz kimliği.
* Yüz dikdörtgeni: Sol, üst, genişlik ve yükseklik resimdeki yüz konumunu belirten.
* Yer işareti: Yüz tanıma bileşenlerinin önemli konumlarına işaret eden 27 noktası yüz yer işareti dizisi.
* Yaş, cinsiyet, gülümseme yoğunluğu, kafanın duruşu ve sakal ve bıyık gibi yüzdeki öznitelikler.

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdakini kopyalayın ve `detectFaces.html` gibi bir dosyaya kaydedin.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. Gerekirse, değiştirme `uriBase` abonelik anahtarlarınızın aldığınız yere konumu kullanmak üzere değeri (bkz [yüz tanıma API'si belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) tüm bölge uç noktalar listesi için).
1. Dosyayı tarayıcınıza sürükleyin ve bırakın.
1. `Analyze faces` düğmesine tıklayın.

### <a name="face---detect-request"></a>Yüz - Algılama isteği

```html
<!DOCTYPE html>
<html>
<head>
    <title>Detect Faces Sample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>
<body>

<script type="text/javascript">
    function processImage() {
        // Replace <Subscription Key> with your valid subscription key.
        var subscriptionKey = "<Subscription Key>";

        // NOTE: You must use the same region in your REST call as you used to
        // obtain your subscription keys. For example, if you obtained your
        // subscription keys from westus, replace "westcentralus" in the URL
        // below with "westus".
        //
        // Free trial subscription keys are generated in the "westus" region.
        // If you use a free trial subscription key, you shouldn't need to change 
        // this region.
        var uriBase =
            "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect";

        // Request parameters.
        var params = {
            "returnFaceId": "true",
            "returnFaceLandmarks": "false",
            "returnFaceAttributes":
                "age,gender,headPose,smile,facialHair,glasses,emotion," +
                "hair,makeup,occlusion,accessories,blur,exposure,noise"
        };

        // Display the image.
        var sourceImageUrl = document.getElementById("inputImage").value;
        document.querySelector("#sourceImage").src = sourceImageUrl;

        // Perform the REST API call.
        $.ajax({
            url: uriBase + "?" + $.param(params),

            // Request headers.
            beforeSend: function(xhrObj){
                xhrObj.setRequestHeader("Content-Type","application/json");
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            },

            type: "POST",

            // Request body.
            data: '{"url": ' + '"' + sourceImageUrl + '"}',
        })

        .done(function(data) {
            // Show formatted JSON on webpage.
            $("#responseTextArea").val(JSON.stringify(data, null, 2));
        })

        .fail(function(jqXHR, textStatus, errorThrown) {
            // Display error message.
            var errorString = (errorThrown === "") ?
                "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ?
                "" : (jQuery.parseJSON(jqXHR.responseText).message) ?
                    jQuery.parseJSON(jqXHR.responseText).message :
                        jQuery.parseJSON(jqXHR.responseText).error.message;
            alert(errorString);
        });
    };
</script>

<h1>Detect Faces:</h1>
Enter the URL to an image that includes a face or faces, then click
the <strong>Analyze face</strong> button.<br><br>

Image to analyze: <input type="text" name="inputImage" id="inputImage"
value="https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg" />

<button onclick="processImage()">Analyze face</button><br><br>

<div id="wrapper" style="width:1020px; display:table;">
    <div id="jsonOutput" style="width:600px; display:table-cell;">
        Response:<br><br>

        <textarea id="responseTextArea" class="UIInput"
                  style="width:580px; height:400px;"></textarea>
    </div>
    <div id="imageDiv" style="width:420px; display:table-cell;">
        Source image:<br><br>

        <img id="sourceImage" width="400" />
    </div>
</div>
</body>
</html>
```

### <a name="face---detect-response"></a>Yüz - Algılama yanıtı

Başarılı bir yanıt JSON biçiminde döndürülür.

![GettingStartCSharpScreenshot](../Images/face-detect-javascript.png)

Aşağıda, başarılı bir yanıt örneği verilmiştir:

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
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
      }
    }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Bir görüntüdeki insan yüzlerini algılamak, yüzleri dikdörtgenlerle ayırmak ve yaş ve cinsiyet gibi öznitelikleri döndürmek için kullanılan Yüz Tanıma API'sini keşfedin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API’leri](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
