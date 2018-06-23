---
title: Anomali algılama C# app - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde Anomali algılama API'sini kullanan bir C# uygulaması keşfedin. API için özgün veri noktaları göndermek ve beklenen değer ve anomali noktaları alabilirsiniz.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 2e4100fd7d8e85a6b103c31000176aaaeb3d7151
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353362"
---
# <a name="anomaly-detection-c-application"></a>Anomali algılama C# uygulaması

Girdisinden anormallikleri algılamak için Anomali algılama API'sini kullanan temel bir Windows uygulaması keşfedin. Örnek abonelik anahtarınızla Anomali algılama API için zaman serisi veri gönderir, ardından tüm anomali noktaları ve beklenen değer her veri noktası için API alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Örnek .NET Framework için geliştirilen kullanarak [Visual Studio 2017, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali algılama abone olma ve aboneliği anahtarı alma 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Alın ve örnek kullanın

Bilgisayarınızdan Anomali algılama örnek uygulamaya kopyalayabilirsiniz [Github](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Örnek yükleyin

GitHub masaüstünüzü Sample\AnomalyDetectionSample.sln açın.

<a name="Step2"></a>
### <a name="build-the-example"></a>Örnek oluşturma

Ctrl + Shift + B tuşuna basın veya yapı Şerit menüsünde'nı tıklatın ve sonra yapı çözümü seçin.

<a name="Step3"></a>
### <a name="run-the-example"></a>Örneği çalıştırın

1. Yapı tamamlandıktan sonra basın **F5** veya **Başlat** örneği çalıştırmak için Şerit menüsünde.
2. Anomali algılama kullanıcı arabirimi penceresi metin düzenleme kutusu "{your_subscription_key}" okuma bulun.
3. Örnek verileri içeren request.json dosyayı kendi verilerinizle değiştirin, ardından "Gönderme" düğmesini tıklatın. Microsoft, karşıya yükleme ve bunları daha sonra arasında herhangi bir anomali noktası algılamak üzere kullanmak veri alır. Güncelleştirilen verileri, Microsoft'un Server'da kalıcı değildir. Anomali noktası algılamak için yeniden, verileri yeniden yüklemeniz.
4. Veri iyi durumda, "Yanıt" alanında anomali algılama sonucu bulur. Herhangi bir hata oluşursa hata bilgilerini de yanıt alanında gösterilir.

<a name="Review"></a>
### <a name="read-the-result"></a>Sonucunu oku

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Gözden geçirin ve öğrenin

Çalışan bir uygulama sahip olduğunuza göre şimdi örnek uygulama Bilişsel hizmetler teknolojisi ile nasıl tümleşik çalıştığını gözden geçirin. Bu adım, bu uygulamanın üzerine yapı devam ya da Microsoft Anomali algılama kullanarak kendi uygulama geliştirmek kolaylaştırır.

Bu örnek uygulama Anomali algılama Restful API'si yararlanır uç noktası.

Örnek uygulamayı Restful API'si kullanılma gözden geçirme, bir kod parçacığı bakalım **AnomalyDetectionClient.cs**. Dosya "Anahtar örnek kodu BAŞLATIR burada" ve "Anahtar örnek kodu sona ERER burada" aşağıda parçacıkları çoğaltılamaz kod bulmanıza yardımcı olmak üzere gösteren kod açıklamaları içerir.

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
**Request(...)**  Kod parçacığını HttlClient kullanmayı gösterir, abonelik anahtarı ve veri noktalarının uç Anomali algılama API gönderin.

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
