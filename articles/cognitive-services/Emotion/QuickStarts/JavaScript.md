---
title: Duygu tanıma API'si JavaScript hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get duygu tanıma API'si Bilişsel hizmetler JavaScript ile kullanmaya başlayın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 2578b0212f9b4a6483402074d7c9eff73e51ca6b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352402"
---
# <a name="emotion-api-javascript-quick-start"></a>Duygu tanıma API'si JavaScript hızlı başlangıç

> [!IMPORTANT]
> Video API Önizleme 30 Ekim 2017 sona erer. Yeni deneyin [Video dizin oluşturucu API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videoların öngörüleri ayıklamak ve konuşulan sözcüklerin, yüzler, karakterler ve duygular algılayarak arama sonuçları gibi içerik bulma deneyimlerini geliştirmek üzere. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Bu makalede bilgiler sağlar ve hızlı bir şekilde yardımcı olmak için kod örnekleri, kullanımına başlamanıza [duygu tanıma API'si tanıması yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) görüntünün bir veya daha fazla kişiler tarafından ifade duygular tanımak için JavaScript ile.

## <a name="prerequisite"></a>Önkoşul
* Ücretsiz abonelik anahtarınızı alın [burada](https://azure.microsoft.com/try/cognitive-services/), veya bir Azure aboneliğiniz varsa bir duygu tanıma API'si kaynak oluşturmak ve abonelik anahtarı ve uç nokta var. alın.

![Duygu tanıma API'si kaynak oluştur](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Tanı duygular JavaScript örnek istek

Aşağıdaki kopyalayın ve aşağıdaki gibi bir dosyaya Kaydet `test.html`. Değişiklik isteği `url` abonelik anahtarlarınızı aldığınız burada konum kullanın ve "Ocp-Apim-Subscription-Key" değeri geçerli bir abonelik anahtarınızla değiştirin. Bunlar duygu tanıma API'si kaynağınız genel bakış ve anahtarları bölümlerini Azure portalında sırasıyla bulunabilir. 

![API uç noktası](../Images/api-url.png)

![API abonelik anahtarı](../Images/keys.png)

ve kullanmak istediğiniz görüntü konumu için istek gövdesini değiştirin. Örnek, sürükle ve bırak tarayıcınıza dosyayı çalıştırmak için.

```html
<!DOCTYPE html>
<html>
<head>
    <title>JSSample</title>
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
</head>

<h2>Face Rectangle</h2>
<ul id="faceRectangle">
<!-- Will populate list with response content -->
</ul>

<h2>Emotions</h2>
<ul id="scores">
<!-- Will populate list with response content -->
</ul>

<body>

<script type="text/javascript">
    $(function() {
        // No query string parameters for this API call.
        var params = { };
      
        $.ajax({
            // NOTE: You must use the same location in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
            //   URL below with "westcentralus".
            url: "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?" + $.param(params),
            beforeSend: function(xhrObj){
                // Request headers, also supports "application/octet-stream"
                xhrObj.setRequestHeader("Content-Type","application/json");

                // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
                xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key","<your subscription key>");
            },
            type: "POST",
            // Request body
            data: '{"url": "<your image url>"}',
        }).done(function(data) {
            // Get face rectangle dimensions
            var faceRectangle = data[0].faceRectangle;
            var faceRectangleList = $('#faceRectangle');

            // Append to DOM
            for (var prop in faceRectangle) {
                faceRectangleList.append("<li> " + prop + ": " + faceRectangle[prop] + "</li>");
            }
            
            // Get emotion confidence scores
            var scores = data[0].scores;
            var scoresList = $('#scores');

            // Append to DOM
            for(var prop in scores) {
                scoresList.append("<li> " + prop + ": " + scores[prop] + "</li>")
            }
        }).fail(function(err) {
            alert("Error: " + JSON.stringify(err));
        });
    });
</script>
</body>
</html>
```

## <a name="recognize-emotions-sample-response"></a>Tanı duygular örnek yanıt
Başarılı bir çağrı yüz girişleri dizisi ve azalan düzende yüz dikdörtgen boyutuna göre derece ilişkili duygu puanlarını döndürür. Boş bir yanıt hiçbir yüzeyleri algıladığını belirtir. Bir duygu giriş aşağıdaki alanları içerir:
* faceRectangle - yüz görüntüdeki dikdörtgen konumu.
* puanları - görüntüdeki her yüz duygu puanlarını. 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
