---
title: Anomali algılama Javascript uygulaması - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde Anomali algılama API'sini kullanan bir Javascript Web uygulaması keşfedin. API için özgün veri noktaları göndermek ve beklenen değer ve anomali noktaları alabilirsiniz.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 42c3941a05efe8b74f818cd99f3606b3073892a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353338"
---
# <a name="anomaly-detection-javascript-application"></a>Anomali algılama Javascript uygulama

Bir anomali algılama için Anomali algılama REST API'sini kullanan bir Web uygulaması keşfedin. Örnek abonelik anahtarınızla Anomali algılama API için zaman serisi veri gönderir, ardından tüm anomali noktaları ve beklenen değer her veri noktası için API alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu öğreticide, basit bir metin düzenleyicisi kullanarak geliştirilmiştir.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali algılama abone olma ve aboneliği anahtarı alma 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Alın ve örnek kullanın

Bu öğretici için zaman serisi veri anomali algılama iki senaryo sunar. Haydi başlayalım.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Eğitmen projenizi indirin

Kopya [Bilişsel hizmetler JavaScript Anomali algılama öğretici](https://github.com/MicrosoftAnomalyDetection/javascript-sample), .zip dosyasını indirin ve boş bir dizine ayıklayın.

<a name="Step2"></a>
### <a name="run-the-example"></a>Örneği çalıştırın

Örnek deneyebilirsiniz iki senaryo vardır.
1. PUT, **abonelik anahtarı** üzerinde abonelik anahtarı alanına anomalydetection.html işlevini algıla.
2. Anomali algılama API uç noktası koyabilir ve abonelik bölgede doğru bölgeyi kullandığınızdan emin olun.
3. Açık **anomalydetection.html** dosyasını bir Web tarayıcısında.

**Senaryo 1 Algıla haftalık zaman serisi veri**
1. Dönem alanında dönemi giriş **7**. 
2. Örnek verileri, haftalık zaman serisi veri noktaları (Json) noktaları alanında değiştirin veya örnek verileri doğrudan kullanın.
3. Anomali algılama düğmesine tıklayın ve sağ yanıt metin kutusuna sonucu doğrulayın.

**Senaryo 2 Algıla bir dönem olmadan zaman serisi veri**
1. Dönemde boş süre Dosyalanan, bırakın zaman serisi süre tanımadığınız varsayalım.
2. Aynı zaman serisi veri Senaryo 1 kullanma.
3. Anomali algılama düğmesine tıklayın ve sağ yanıt metin kutusunda dönem alanı doğrulayın.

<a name="Step3"></a>
### <a name="read-the-result"></a>Sonucunu oku

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Gözden geçirin ve öğrenin

Artık çalışan bir uygulama alın. Şimdi örnek uygulama Bilişsel hizmetler teknolojisi ile nasıl tümleşik çalıştığını gözden geçirin. Bu adım, üzerinde bu uygulamayı oluşturmaya devam etmek veya Microsoft Anomali algılama kullanarak kendi uygulamanızı geliştirin daha kolay hale getirir.
Bu örnek uygulama Anomali algılama Restful API'si yararlanır uç noktası.
Örnek uygulamasında Restful API'si kullanılma gözden geçirme, kod parçacığında anomalydetection.html bakalım.
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
