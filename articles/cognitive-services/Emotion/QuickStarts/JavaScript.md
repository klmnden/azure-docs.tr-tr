---
title: "Hızlı başlangıç: Bir görüntüdeki yüzlerin duygularını tanıma - Duygu Tanıma API'si, JavaScript"
titlesuffix: Azure Cognitive Services
description: JavaScript ile Duygu Tanıma API'sini kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri edinin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: eeaf2ea080d8c0b604b9831532028e31b8306169
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239498"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Hızlı başlangıç: Bir görüntüdeki yüzlerin duygularını tanımak için bir uygulama oluşturma.

> [!IMPORTANT]
> Duygu Tanıma API'si 15 Şubat 2019 tarihinde kullanım dışı bırakılacaktır. Duygu tanıma özelliği [Yüz Tanıma API'sinin](https://docs.microsoft.com/azure/cognitive-services/face/) bir parçası olarak genel kullanıma sunulmuştur. 

Bu makalede bir görüntüdeki bir veya daha fazla kişinin duygularını tanımak için JavaScript ile [Duygu Tanıma API'si Recognize metodunu](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri bulunmaktadır.

## <a name="prerequisite"></a>Önkoşul
* [Buradan](https://azure.microsoft.com/try/cognitive-services/) ücretsiz Abonelik Anahtarınızı alın veya Azure Aboneliğiniz varsa bir Duygu Tanıma API'si kaynağı oluşturun ve oradaki Abonelik Anahtarı ile Uç Nokta değerlerini alın.

![Duygu Tanıma API'si Kaynağı Oluşturma](../Images/create-resource.png)

## <a name="recognize-emotions-javascript-example-request"></a>Duygu Tanıma JavaScript Örnek İsteği

Aşağıdakini kopyalayın ve `test.html` gibi bir dosyaya kaydedin. `url` isteğini abonelik anahtarlarınızı aldığınız konumu kullanacak şekilde değiştirin ve "Ocp-Apim-Subscription-Key" değerinin yerine geçerli abonelik anahtarınızı yazın. Bu anahtarlara Azure portalda Duygu Tanıma API'si kaynağınızın Genel Bakış ve Anahtarlar bölümlerinden ulaşabilirsiniz.

![API Uç Noktası](../Images/api-url.png)

![API Abonelik Anahtarı](../Images/keys.png)

İstek gövdesini kullanmak istediğiniz görüntünün konumuyla değiştirin. Örneği çalıştırmak için dosyayı tarayıcınıza sürükleyin ve bırakın.

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

## <a name="recognize-emotions-sample-response"></a>Duygu Tanıma Örneği Yanıtı
Başarılı bir çağrı, yüz dikdörtgenine göre büyükten küçüğe doğru sıralanmış şekilde yüz girişlerinden ve ilgili duygu puanlarından oluşan bir dizi döndürür. Yanıtın boş olması hiç yüz algılanmadığını gösterir. Duygu girişi aşağıdaki alanları içerir:
* faceRectangle - Dikdörtgen şeklinde yüzün görüntü içindeki konumu.
* scores - Görüntüdeki her bir yüzün duygu puanı.

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
