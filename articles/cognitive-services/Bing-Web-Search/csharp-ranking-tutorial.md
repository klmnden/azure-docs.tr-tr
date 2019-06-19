---
title: Arama sonuçlarını görüntülemek için derece kullanma
titleSuffix: Azure Cognitive Services
description: Bing RankingResponse yanıt derece sırada arama sonuçlarını görüntülemek için nasıl kullanılacağını gösterir.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 2575A80C-FC74-4631-AE5D-8101CF2591D3
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: cfe198352cae95227a76535544d55a6be0c66abc
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203247"
---
# <a name="build-a-console-app-search-client-in-c"></a>C# konsol uygulaması arama istemci derleme

Bu öğreticide, Bing Web araması API'si sorgulamak ve dereceli sonuçları görüntülemek kullanıcılara basit bir .NET Core konsol uygulaması oluşturma işlemi gösterilmektedir.

Bu öğreticide gösterilmiştir nasıl yapılır:

- Bing Web araması API'si için basit bir sorgu olun
- Sorgu sonuçları kişilerinin sıralı bir düzende görüntüler.

## <a name="prerequisites"></a>Önkoşullar

Eğitimle birlikte ilerlemek için ihtiyacınız vardır:

- Visual Studio. Yoksa [ücretsiz Visual Studio 2017 Community Edition'ı yükleyip](https://www.visualstudio.com/downloads/).
- Bing Web araması API'si için bir abonelik anahtarı. Bir aboneliğiniz yoksa [ücretsiz deneme sürümüne kaydolun](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

## <a name="create-a-new-console-app-project"></a>Yeni bir konsol uygulaması projesi oluşturma

Visual Studio’da `Ctrl`+`Shift`+`N` ile yeni bir proje oluşturun.

İçinde **yeni proje** iletişim kutusunda, tıklayın **Visual C# > Windows Klasik Masaüstü > konsol uygulaması (.NET Framework)** .

Uygulama adı **MyConsoleSearchApp**ve ardından **Tamam**.

## <a name="add-the-jsonnet-nuget-package-to-the-project"></a>JSON.net Nuget paketini projeye ekleyin.

JSON.net API'si tarafından döndürülen JSON yanıtlarıyla çalışmanıza olanak sağlar. Kendi NuGet paketini projenize ekleyin:

- İçinde **Çözüm Gezgini** sağ tıklatın ve proje **NuGet paketlerini Yönet...** .
- Üzerinde **Gözat** sekmesinde, arama `Newtonsoft.Json`. En son sürümü seçin ve ardından **yükleme**.
- Tıklayın **Tamam** düğmesini **değişiklikleri gözden geçir** penceresi.
- Başlıklı Visual Studio sekmeyi kapatırsanız **NuGet: MyConsoleSearchApp**.

## <a name="add-a-reference-to-systemweb"></a>System.Web bir başvuru ekleyin

Bu öğreticide dayanan `System.Web` derleme. Bu derlemeye olan başvuruyu projenize ekleyin:

- İçinde **Çözüm Gezgini**, sağ **başvuruları** seçip **Başvuru Ekle...**
- Seçin **derlemeleri > Framework**, ardından aşağı kaydırın ve denetleme **System.Web**
- **Tamam**’ı seçin

## <a name="add-some-necessary-using-statements"></a>Bazı gerekli using deyimleri ekleme

Bu öğreticideki kod üç ek gerektirir using deyimleri. Varolan aşağıda bu deyimleri ekleme `using` deyimleri en üstündeki **Program.cs**:

```csharp
using System.Web;
using System.Net.Http;
```

## <a name="ask-the-user-for-a-query"></a>Bir sorgu için kullanıcıya sor

İçinde **Çözüm Gezgini**açın **Program.cs**. Güncelleştirme `Main()` yöntemi:

```csharp
static void Main()
{
    // Get the user's query
    Console.Write("Enter Bing query: ");
    string userQuery = Console.ReadLine();
    Console.WriteLine();

    // Run the query and display the results
    RunQueryAndDisplayResults(userQuery);

    // Prevent the console window from closing immediately
    Console.WriteLine("\nHit ENTER to exit...");
    Console.ReadLine();
}
```

Bu yöntem:

- Bir sorgu için kullanıcıya sorar
- Çağrıları `RunQueryAndDisplayResults(userQuery)` sorgu yürütmek ve sonuçları görüntülemek için
- Konsol penceresi hemen kapatarak önlemek için kullanıcı girişi için bekler.

## <a name="search-for-query-results-using-the-bing-web-search-api"></a>Bing Web araması API'si kullanarak sorgu sonuçlarını arayın

Ardından, API sorgular ve sonuçları görüntüleyen bir yöntem ekleyin:

```csharp
static void RunQueryAndDisplayResults(string userQuery)
{
    try
    {
        // Create a query
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<YOUR_SUBSCRIPTION_KEY_GOES_HERE>");
        var queryString = HttpUtility.ParseQueryString(string.Empty);
        queryString["q"] = userQuery;
        var query = "https://api.cognitive.microsoft.com/bing/v7.0/search?" + queryString;

        // Run the query
        HttpResponseMessage httpResponseMessage = client.GetAsync(query).Result;

        // Deserialize the response content
        var responseContentString = httpResponseMessage.Content.ReadAsStringAsync().Result;
        Newtonsoft.Json.Linq.JObject responseObjects = Newtonsoft.Json.Linq.JObject.Parse(responseContentString);

        // Handle success and error codes
        if (httpResponseMessage.IsSuccessStatusCode)
        {
            DisplayAllRankedResults(responseObjects);
        }
        else
        {
            Console.WriteLine($"HTTP error status code: {httpResponseMessage.StatusCode.ToString()}");
        }
    }
    catch (Exception e)
    {
        Console.WriteLine(e.Message);
    }
}
```

Bu yöntem:

- Oluşturur bir `HttpClient` Web araması API'si sorgulamak için
- Kümeleri `Ocp-Apim-Subscription-Key` Bing isteğin kimliğini kullanan HTTP üstbilgisi
- İsteği yürütür ve sonuçları seri durumdan çıkarılacak JSON.net kullanır
- Çağrıları `DisplayAllRankedResults(responseObjects)` dereceli sırada tüm sonuçları görüntülemek için

Değerini ayarladığınızdan emin olun `Ocp-Apim-Subscription-Key` abonelik anahtarınız için.

## <a name="display-ranked-results"></a>Dereceli sonuçları görüntüle

Kişilerinin sıralı bir düzende sonuçları görüntülemek nasıl göstermeden önce bir örnek web arama yanıt göz atın:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346...",
        "totalEstimatedMatches" : 982000,
        "value" : [{
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
            "name" : "Contoso Sailing Club - Seattle",
            "url" : "https:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/contososailingsea...",
            "snippet" : "Come sail with Contoso in Seattle...",
            "dateLastCrawled" : "2017-04-07T02:25:00"
        },
        {
            "id" : "https:\/\/api.cognitive.microsoft.com\/api\/7\/#WebPages.6",
            "name" : "Contoso Sailing Lessons - Official Site",
            "url" : "http:\/\/www.bing.com\/cr?IG=70BE289346ED4594874FE...",
            "displayUrl" : "https:\/\/www.constososailinglessonsseat...",
            "snippet" : "Contoso sailing lessons in Seattle...",
            "dateLastCrawled" : "2017-04-09T14:30:00"
        },

        ...

        ],
        "someResultsRemoved" : true
    },
    "relatedSearches" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/7\/#RelatedSearches",
        "value" : [{
            "text" : "sailing lessons",
            "displayText" : "sailing lessons",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=70BE289346E..."
        }

        ...

        ]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "WebPages",
                "resultIndex" : 0,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0"
                }
            },
            {
                "answerType" : "WebPages",
                "resultIndex" : 1,
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.1"
                }
            }

            ...

            ]
        },
        "sidebar" : {
            "items" : [{
                "answerType" : "RelatedSearches",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#RelatedSearches"
                }
            }]
        }
    }
}
```

`rankingResponse` JSON nesnesi ([belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#rankingresponse)) arama sonuçları için uygun görüntülenme sırasını tanımlar. Bu, bir veya daha fazla aşağıdaki, Önceliklendirilmiş grupları içerir:

- `pole`: En çok görünen işleme almak için arama sonuçları (örneğin, ana hat görüntülenen ve kenar çubuğunuzu).
- `mainline`: Ana hatta da görüntülemek için arama sonuçları.
- `sidebar`: Kenar çubuğunda görüntülemek için arama sonuçları. Hiçbir kenar varsa, aşağıda ana hattı sonuçları görüntüler.

JSON derecelendirme yanıt, bir veya daha fazla gruplar içerebilir.

İçinde **Program.cs**, düzgün bir şekilde kişilerinin sıralı bir düzende sonuçları görüntülemek için aşağıdaki yöntemi ekleyin:

```csharp
static void DisplayAllRankedResults(Newtonsoft.Json.Linq.JObject responseObjects)
{
    string[] rankingGroups = new string[] { "pole", "mainline", "sidebar" };

    // Loop through the ranking groups in priority order
    foreach (string rankingName in rankingGroups)
    {
        Newtonsoft.Json.Linq.JToken rankingResponseItems = responseObjects.SelectToken($"rankingResponse.{rankingName}.items");
        if (rankingResponseItems != null)
        {
            foreach (Newtonsoft.Json.Linq.JObject rankingResponseItem in rankingResponseItems)
            {
                Newtonsoft.Json.Linq.JToken resultIndex;
                rankingResponseItem.TryGetValue("resultIndex", out resultIndex);
                var answerType = rankingResponseItem.Value<string>("answerType");
                switch (answerType)
                {
                    case "WebPages":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("webPages.value"), "WebPage", "name", "url", "displayUrl", "snippet");
                        break;
                    case "News":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("news.value"), "News", "name", "url", "description");
                        break;
                    case "Images":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("images.value"), "Image", "thumbnailUrl");
                        break;
                    case "Videos":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("videos.value"), "Video", "embedHtml");
                        break;
                    case "RelatedSearches":
                        DisplaySpecificResults(resultIndex, responseObjects.SelectToken("relatedSearches.value"), "RelatedSearch", "displayText", "webSearchUrl");
                        break;
                }
            }
        }
    }
}
```

Bu yöntem:

- Üzerinde döngü `rankingResponse` yanıtını içeren gruplar
- Öğeleri çağırarak her grupta görüntüler. `DisplaySpecificResults(...)`

İçinde **Program.cs**, aşağıdaki iki yöntemi ekleyin:

```csharp
static void DisplaySpecificResults(Newtonsoft.Json.Linq.JToken resultIndex, Newtonsoft.Json.Linq.JToken items, string title, params string[] fields)
{
    if (resultIndex == null)
    {
        foreach (Newtonsoft.Json.Linq.JToken item in items)
        {
            DisplayItem(item, title, fields);
        }
    }
    else
    {
        DisplayItem(items.ElementAt((int)resultIndex), title, fields);
    }
}

static void DisplayItem(Newtonsoft.Json.Linq.JToken item, string title, string[] fields)
{
    Console.WriteLine($"{title}: ");
    foreach( string field in fields )
    {
        Console.WriteLine($"- {field}: {item[field]}");
    }
    Console.WriteLine();
}
```

Bu yöntemler arama sonuçlarını konsola çıkarmak için birlikte çalışır.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın. Çıktı aşağıdakine benzer görünmelidir:

```
Enter Bing query: sailing lessons seattle

WebPage:
- name: Contoso Sailing Club - Seattle
- url: https://www.bing.com/cr?IG=70BE289346ED4594874FE...
- displayUrl: https://contososailingsea....
- snippet: Come sail with Contoso in Seattle...

WebPage:
- name: Contoso Sailing Lessons Seattle - Official Site
- url: http://www.bing.com/cr?IG=70BE289346ED4594874FE...
- displayUrl: https://www.constososailinglessonsseat...
- snippet: Contoso sailing lessons in Seattle...

...
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [sonuçları görüntülemek için sıralama kullanarak](rank-results.md).
