---
title: Anomali algılama Javascript uygulaması - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel hizmetler Anomali algılama API'sini kullanan bir Javascript Web apps'i keşfedin. Özgün veri noktaları API'ye gönderin ve beklenen değerini ve anomali noktalarını alın.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 5bb123648a683454597b0561f9f82dffb70eab04
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248372"
---
# <a name="anomaly-detection-javascript-application"></a>Anomali algılama Javascript uygulama

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Bir anomali algılama için Anomali algılama REST API kullanan bir Web uygulaması keşfedin. Örnek abonelik anahtarınız ile zaman serisi verilerini Anomali algılama API'sine gönderir, ardından tüm anomali noktaları ve beklenen değer, her veri noktası için API'den alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu öğreticide, bir basit metin düzenleyicisi kullanarak geliştirilmiştir.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali algılama için abone ve bir abonelik anahtarı edinirler 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Örneği kullanın ve alma

Bu öğretici, zaman serisi verileri anomali algılamaya yönelik iki senaryo sunar. Haydi başlayalım.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Öğretici projesinin indirin

Kopya [Bilişsel hizmetler JavaScript Anomali algılama öğretici](https://github.com/MicrosoftAnomalyDetection/javascript-sample), .zip dosyasını indirin ve boş bir dizine ayıklayın.

<a name="Step2"></a>
### <a name="run-the-example"></a>Örneği çalıştırın

Örnek yapabileceğiniz iki senaryo vardır.
1. Yerleştirme, **abonelik anahtarı** üzerinde bir abonelik anahtarı alanına anomalydetection.html işlevi algılayın.
2. Anomali algılama API'si uç nokta koyun ve abonelik bölgede doğru bölgeyi kullandığınızdan emin olun.
3. Açık **anomalydetection.html** bir Web tarayıcısında dosya.

**Senaryo 1 Algıla haftalık zaman serisi verileri**
1. Dönem Dönem alanında giriş **7**. 
2. Örnek verileri, haftalık zaman serisi veri noktaları (Json) noktaları alanında değiştirin veya doğrudan örnek verileri kullanabilirsiniz.
3. Anomali algılama düğmesine tıklayın ve sağ yanıt metin kutusuna sonucu doğrulayın.

**Senaryo 2 Algıla bir nokta olmadan zaman serisi verileri**
1. Dönem Dönem içinde boş Dosyalanan, bırakın varsayılır zaman serisi süre bilinmiyor.
2. Senaryo 1 aynı zaman serisi verileri kullanıyor.
3. Anomali algılama düğmesine tıklayın ve sağ yanıt metin kutusuna dönem alanı doğrulayın.

<a name="Step3"></a>
### <a name="read-the-result"></a>Sonuç okuyun

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Gözden geçirin ve bilgi edinin

Artık çalışan bir uygulamayı alın. Örnek uygulamayı Bilişsel hizmetler teknolojisi ile nasıl tümleştirildiğini gözden geçirelim. Bu adım bu uygulama üzerinde oluşturmaya devam edin veya Microsoft Anomali algılama kullanarak kendi uygulamanızı geliştirmek kolaylaştırır.
Bu örnek uygulama Anomali algılama Restful API'si yararlanır uç noktası.
Restful API'yi nasıl örnek uygulamada kullanılan gözden geçirme, bir kod parçacığı anomalydetection.html göz atalım.
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
