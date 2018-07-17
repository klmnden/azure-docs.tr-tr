---
title: Duygu tanıma API'si JavaScript hızlı başlangıç | Microsoft Docs
description: Hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get, Bilişsel hizmetler JavaScript'te duygu tanıma API'sini kullanmaya başlayın.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: fb9cc2335582c4ec75ec45635e519346d65d7e08
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072101"
---
# <a name="emotion-api-javascript-quick-start"></a>Duygu tanıma API'si JavaScript hızlı başlangıç

> [!IMPORTANT]
> Video API'si önizlemesi 30 Ekim 2017 tarihinde sona erecek. Yeni deneyin [Video Indexer API önizlemesi](https://azure.microsoft.com/services/cognitive-services/video-indexer/) kolayca videolardan içgörüleri ayıklayın ve konuşulan sözcükleri, yüzleri, karakterleri ve duyguları algılayarak arama sonuçları gibi içerik keşif deneyimlerini geliştirin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Bu makalede bilgiler ve kod örnekleri, hızlı bir şekilde yardımcı olması için kullanmaya başlama [duygu tanıma API'si yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) görüntüyü bir veya daha fazla insana ile ifade edilen duyguları tanıma için JavaScript ile.

## <a name="prerequisite"></a>Önkoşul
* Ücretsiz abonelik anahtarınızı alın [burada](https://azure.microsoft.com/try/cognitive-services/), veya bir Azure aboneliğiniz varsa bir duygu tanıma API'si kaynağı oluşturun ve abonelik anahtarını ve uç nokta var. alın.

![Duygu tanıma API kaynağı oluşturun](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>JavaScript örnek istek duyguları tanıma

Gibi bir dosyaya kaydedin ve aşağıdakini kopyalayın `test.html`. Değişiklik isteği `url` burada elde ettiğiniz abonelik anahtarlarınızın konumunu ve "Ocp-Apim-Subscription-Key" değeri geçerli bir abonelik anahtarınız ile değiştirin. Bunlar Azure portalındaki kaynağınızın duygu tanıma API'si genel bakış ve anahtarları bölümlerindeki sırasıyla bulunabilir. 

![API uç noktası](../Images/api-url.png)

![API abonelik anahtarı](../Images/keys.png)

İstek gövdesi, kullanmak istediğiniz görüntüyü konumuyla değiştirin. Sürükle ve bırak örnek tarayıcınıza dosyayı çalıştırmak için.

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

## <a name="recognize-emotions-sample-response"></a>Örnek yanıt duyguları tanıma
Başarılı bir çağrı, yüz tanıma girişleri dizisi ve yüz dikdörtgeni boyutu azalan düzende sıralanmış ilişkili duygu tanıma puanlarını döndürür. Boş bir yanıt yok yüzleri algılandığını gösterir. Duygu tanıma girdiyi aşağıdaki alanları içerir:
* faceRectangle - resimdeki yüz dikdörtgeni konumu.
* puanları - duygu tanıma puanları resimdeki her yüz için. 

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
