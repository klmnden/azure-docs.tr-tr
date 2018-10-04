---
title: Anomali algılama C# uygulaması - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel hizmetler Anomali algılama API'sini kullanan bir C# uygulaması keşfedin. Özgün veri noktaları API'ye gönderin ve beklenen değerini ve anomali noktalarını alın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: fb434bd668b065fbdbaac39f2926676bcc90e794
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247833"
---
# <a name="anomaly-detection-c-application"></a>Anomali algılama C# uygulaması

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Girdisinden anormallikleri için Anomali algılama API'sini kullanan temel bir Windows uygulaması keşfedin. Örnek abonelik anahtarınız ile zaman serisi verilerini Anomali algılama API'sine gönderir, ardından tüm anomali noktaları ve beklenen değer, her veri noktası için API'den alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Örneğin, .NET Framework için geliştirilmiştir kullanarak [Visual Studio 2017, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali algılama için abone ve bir abonelik anahtarı edinirler 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Örneği kullanın ve alma

Anomali algılama örnek uygulama bilgisayarınıza kopyalayabilirsiniz [Github](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Bir örnek yükleyin

GitHub masaüstünüzü Sample\AnomalyDetectionSample.sln açın.

<a name="Step2"></a>
### <a name="build-the-example"></a>Örnek oluşturma

Ctrl + Shift + B tuşlarına basın veya derleme Şerit menüsünde'nı tıklatın, ardından yapı çözümü seçin.

<a name="Step3"></a>
### <a name="run-the-example"></a>Örneği çalıştırın

1. Derleme tamamlandıktan sonra basın **F5** veya **Başlat** örneği çalıştırmak için Şerit menüsünde.
2. Anomali algılama kullanıcı arabirimi penceresi, metin düzenleme kutusu "{your_subscription_key}" okunuyor bulun.
3. Kendi verilerinizle örnek veriler içeren request.json dosyasını değiştirin ve ardından "Gönder" düğmesine tıklayın. Microsoft, karşıya yükleme ve bunları daha sonra arasında herhangi bir anomali noktası algılamak için kullanmak verileri alır. Yüklediğiniz veri Microsoft'un Server'da kalıcı olmaz. Anomali noktası algılamak için yeniden verileri yeniden yüklemeniz.
4. Veri iyi ise, anomali algılama sonucu "Yanıt" alanında bulabilirsiniz. Herhangi bir hata oluşursa hata bilgilerini de yanıt alanında gösterilir.

<a name="Review"></a>
### <a name="read-the-result"></a>Sonuç okuyun

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Gözden geçirin ve bilgi edinin

Çalışan bir uygulamanız olduğuna göre örnek uygulamayı Bilişsel hizmetler teknolojisi ile nasıl tümleştirildiğini gözden geçirelim. Bu adım bu uygulamanın üzerine derlemeye devam edebilirsiniz ya da Microsoft Anomali algılama kullanarak kendi uygulamanızı geliştirmeyi kolaylaştırır.

Bu örnek uygulama Anomali algılama Restful API'si yararlanır uç noktası.

Restful API'yi nasıl örnek uygulamada kullanılan gözden geçirme, bir kod parçacığı göz atalım **AnomalyDetectionClient.cs**. Dosya "Anahtar örnek kod başlar burada" ve "Anahtar örnek kod sona ERER burada" kodu aşağıdaki kod parçacıkları çoğaltılamaz bulmanıza yardımcı olmak için gösteren kod açıklamaları içerir.

```csharp
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

```
### <a name="request"></a>**İstek**
Aşağıdaki kod parçacığında HttpClient abonelik anahtarını ve veri noktalarınızı uç noktasına Anomali algılama API'si göndermek için nasıl kullanılacağını gösterir.

```csharp
    public async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData)
    {
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) })
        {
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Build the request content
            // ---------------------------------------------------------------------- 
            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Send the request content with POST method.
            // ---------------------------------------------------------------------- 
            var res = await client.PostAsync(endpoint, content);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------
            if (res.IsSuccessStatusCode)
            {
                return await res.Content.ReadAsStringAsync();
            }
            else
            {
                return $"ErrorCode: {res.StatusCode}";
            }
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
