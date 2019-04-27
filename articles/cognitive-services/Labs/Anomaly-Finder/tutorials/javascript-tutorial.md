---
title: 'Öğretici: Javascript ile anomali algılama'
titlesuffix: Azure Cognitive Services
description: Anomali Algılama API'sini kullanan bir Javascript Web uygulamasını keşfedin. Özgün veri noktalarını API'ye gönderin ve beklenen değerle anomali noktalarını alın.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 9e66b24987b2318f3022404d951fbb911e7b592d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60709883"
---
# <a name="tutorial-anomaly-detection-with-javascript-application"></a>Öğretici: Javascript uygulaması ile anomali algılama

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Anomali algılamak için Anomali Algılama REST API'sini kullanan bir Web uygulamasını keşfedin. Örnek, abonelik anahtarınızı kullanarak Anomali Algılama API'sine zaman serisi verileri gönderir ve API'den her bir veri noktasıyla ilgili anomali noktalarını ve beklenen değerleri alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu öğretici, basit bir metin düzenleyicisi kullanılarak geliştirilmiştir.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali Algılama için abone olun ve abonelik anahtarını alın 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Örneği alma ve kullanma

Bu öğretici zaman serisi verilerinde anomali algılama için iki senaryo sunmaktadır. Başlayalım.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Öğretici projesini indirme

[Bilişsel Hizmetler JavaScript Anomali Algılama Öğreticisi](https://github.com/MicrosoftAnomalyDetection/javascript-sample)’ni kopyalayın veya .zip dosyasını indirip boş bir dizine ayıklayın.

<a name="Step2"></a>
### <a name="run-the-example"></a>Örneği çalıştırma

Örneği denemek için kullanabileceğiniz iki senaryo vardır.
1. **Abonelik anahtarınızı** anomalydetection.html dosyasındaki detect işlevinin Subscription Key alanına yerleştirin.
2. Anomali algılama API'si uç noktasını yerleştirin ve Subscription Region (Abonelik Bölgesi) için doğru bölgeyi kullandığınızdan emin olun.
3. **anomalydetection.html** dosyasını bir Web tarayıcısında açın.

**Senaryo 1: Haftalık zaman serisi verilerini algılama**
1. Period (Dönem) alanına **7** yazın. 
2. Points (Noktalar) alanındaki örnek verileri haftalık zaman serisi veri noktalarıyla (Json) değiştirin veya doğrudan örnek verileri kullanın.
3. Anomaly Detection (Anomali Algılama) düğmesine tıklayın ve sağ taraftaki Response (Yanıt) metin kutusunda görüntülenen sonuçları doğrulayın.

**Senaryo 2: Dönem belirtmeden zaman serisi verilerini algılama**
1. Period (Dönem) alanını boş bırakın ve zaman serisinin süresi hakkında bilgi sahibi olmadığınızı düşünün.
2. Senaryo 1 ile aynı zaman serisi verilerini kullanın.
3. Anomaly Detection (Anomali Algılama) düğmesine tıklayın ve sağ taraftaki Response (Yanıt) metin kutusunda görüntülenen Period (Dönem) alanını doğrulayın.

<a name="Step3"></a>
### <a name="read-the-result"></a>Sonucu okuyun

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Gözden geçirme ve öğrenme

Artık çalışan bir uygulamanız var. Şimdi örnek uygulamanın Bilişsel Hizmetler teknolojisiyle nasıl tümleştirildiğine bakalım. Bu adım, Microsoft Anomali Algılama özelliğini kullanarak bu uygulamayı derlemeye devam etmenizi veya kendi uygulamanızı geliştirmenizi kolaylaştıracaktır.
Bu örnekte Anomali Algılama Restful API'si uç noktası kullanılmıştır.
Örnek uygulamadaki Restful API'si kullanımını inceledikten sonra anomalydetection.html dosyasındaki bir kod parçacığına bakalım.
```JavaScript
function anomalyDetection(url, subscriptionKey, points, period) {
    var obj = new Object();
    obj.Points = JSON.parse(points); // this points are read from text box.
    obj.Period = parseInt(period);//period=7 weekly period
    var tsData = JSON.stringify(obj);
    // Perform the REST API call.
    $.ajax({
        url: url, //Anomaly Detection API endpoint
        // Request headers.
        beforeSend: function (xhrObj) {
            xhrObj.setRequestHeader("Content-Type", "application/json");
            xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey); // Replace your subscription key
        },
        type: "POST",
        // Request body.
        data: tsData, // json format
        })
        .done(function (data) {
            // Show formatted JSON on webpage.
            $("#responseTextArea").val(JSON.stringify(data, null, 2));
        })
        .fail(function (jqXHR, textStatus, errorThrown) {
            // Display error message.
            var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
            $("#responseTextArea").val(errorString);           
        });
}

```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
