---
title: Anomali Bulucu API C# ile - Microsoft Bilişsel hizmetler kullanma | Microsoft Docs
description: C# ve Anomali Bulucu API Bilişsel Hizmetleri'ni kullanarak hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get başlayın.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 867ce4d0262c94de8da0dadeb8de71c28a8295d5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353333"
---
# <a name="use-the-anomaly-finder-api-with-c"></a>Anomali Bulucu API C# ile kullanma

Bu makalede bilgiler sağlanmaktadır ve hızlı bir şekilde yardımcı olmak için kod örnekleri Anomali Bulucu API C# ile anomali sonucu zaman serisi veri alma görevi gerçekleştirmek için kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-c"></a>C# kullanarak Anomali Bulucu API'si ile anomali noktalarını alma

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data-points"></a>Zaman serisi veri noktaları örneği

Zaman serisi veri noktaları örneği aşağıdaki gibidir.
[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-c-example"></a>Verileri çözümlemek ve anomali noktaları C# örnek alma

Örneği kullanarak adımlar aşağıdaki gibidir.

1. Visual Studio'da yeni bir konsol çözümü oluşturun.
2. Program.cs aşağıdaki kodla değiştirin ve System.Net.Http başvuru ekleyin.
3. Değiştir `[YOUR_SUBSCRIPTION_KEY]` değeri geçerli bir abonelik anahtarınızı ile.
4. Değiştir `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` veri noktaları.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Text.RegularExpressions;
using System.Threading.Tasks;

namespace Console
{
    class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the subscriptionKey string value with your valid subscription key.
        const string subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";

        const string endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

        // Replace the request data with your real data.
        const string requestData = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]";
        static void Main(string[] args)
        {
            var match = Regex.Match(endpoint, "(?<BaseAddress>https://[^/]+)(?<Url>/*.+)");
            if (match.Success)
            {
                var res = Request(
                    match.Groups["BaseAddress"].Value,
                    match.Groups["Url"].Value,
                    subscriptionKey,
                    requestData).Result;
                System.Console.Write(res);
            }
            else
            {
                System.Console.Write("Incorrect endpoint.");
            }
        }

        /// <summary>
        /// Call API to detect the anomaly points
        /// </summary>
        /// <param name="baseAddress">Base address of the API endpoint.</param>
        /// <param name="endpoint">The endpoint of the API</param>
        /// <param name="subscriptionKey">The subscription key applied  </param>
        /// <param name="requestData">The JSON string for requet data points</param>
        /// <returns>The JSON string for anomaly points and expected values.</returns>
        static async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData)
        {
            using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) })
            {
                client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

                var content = new StringContent(requestData, Encoding.UTF8, "application/json");
                var res = await client.PostAsync(endpoint, content);
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
    }
}

```

### <a name="example-response"></a>Örnek yanıt

Başarılı yanıt JSON döndürülür. Örnek yanıt aşağıdaki gibidir.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [C# uygulama](../tutorials/csharp-tutorial.md)
