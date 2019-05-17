---
title: 'Öğretici: Anomali algılamaC#'
titlesuffix: Azure Cognitive Services
description: Anomali Algılama API'sini kullanan bir C# uygulamasını keşfedin. Özgün veri noktalarını API'ye gönderin ve beklenen değerle anomali noktalarını alın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 42b7b6fac8f99d78e4a7460ad8753f48b0a3c0ba
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65823433"
---
# <a name="tutorial-anomaly-detection-with-c-application"></a>Öğretici: Anomali algılama ile C# uygulama

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Girişteki anomalileri algılamak için Anomali Algılama API'sini kullanan basit bir Windows uygulamasını keşfedin. Örnek, abonelik anahtarınızı kullanarak Anomali Algılama API'sine zaman serisi verileri gönderir ve API'den her bir veri noktasıyla ilgili anomali noktalarını ve beklenen değerleri alır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Örneğin, .NET Framework için geliştirilmiştir kullanarak [Visual Studio 2019, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali Algılama için abone olun ve abonelik anahtarını alın 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Örneği alma ve kullanma

Anomali algılama örnek uygulama bilgisayarınıza kopyalayabilirsiniz [GitHub](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Örneği yükleme

GitHub Desktop uygulamasında Sample\AnomalyDetectionSample.sln dizinini açın.

<a name="Step2"></a>
### <a name="build-the-example"></a>Örneği derleme

Ctrl+Shift+B tuşlarına basın veya şerit menüsünde Derle'ye tıklayıp Çözümü Derle'yi seçin.

<a name="Step3"></a>
### <a name="run-the-example"></a>Örneği çalıştırma

1. Derleme tamamlandıktan sonra örneği çalıştırmak için **F5** tuşuna basın veya şerit menüsündeki **Başlat** düğmesine tıklayın.
2. Anomali Algılama kullanıcı arabirimi penceresinde "{your_subscription_key}" yazan metin düzenleme kutusunu bulun.
3. Örnek veriler içeren request.json dosyasını kendi verilerinizle değiştirip "Submit" (Gönder) düğmesine tıklayın. Microsoft, yüklediğiniz verileri alır ve anomali noktası algılamak için kullanır. Yüklediğiniz veriler Microsoft sunucularında kalıcı olmaz. Anomali noktasını yeniden algılamak için verileri tekrar yüklemeniz gerekir.
4. Veriler iyi durumdaysa anomali algılama sonucu "Response" (Yanıt) alanında görüntülenir. Hata oluşursa Response (Yanıt) alanında hata bilgileri de gösterilir.

<a name="Review"></a>
### <a name="read-the-result"></a>Sonucu okuyun

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Gözden geçirme ve öğrenme

Artık çalışan bir uygulamanız olduğuna göre örnek uygulamanın Bilişsel Hizmetler teknolojiyle nasıl tümleştirildiğini inceleyebilirsiniz. Bu adım, Microsoft Anomali Algılama özelliğini kullanarak bu uygulamayı derlemeye devam etmenizi veya kendi uygulamanızı geliştirmenizi kolaylaştıracaktır.

Bu örnekte Anomali Algılama Restful API'si uç noktası kullanılmıştır.

Örnek uygulamadaki Restful API'si kullanımını inceledikten sonra **AnomalyDetectionClient.cs** dosyasındaki bir kod parçacığına bakalım. Aşağıda gösterilen kod parçacıklarını bulmanıza yardımcı olmak için dosyaya “KEY SAMPLE CODE STARTS HERE” (Anahtar kodu örneği başlangıcı) ve “KEY SAMPLE CODE ENDS HERE” (Anahtar kodu örneği sonu) şeklinde kod açıklamaları eklenmiştir.

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
Aşağıdaki kod parçacığı, abonelik anahtarınızı ve veri noktalarınızı Anomali Algılama API'si uç noktasına göndermek için HttpClient kullanma adımlarını göstermektedir.

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
