---
title: "Hızlı Başlangıç: C#SDK'sı sorgu tahmin uç noktası"
titleSuffix: Azure Cognitive Services
description: Kullanım C# SDK'sını bir kullanıcı utterance LUIS için gönderin ve bir tahmin alırsınız.
author: diberry
manager: nitinme
ms.service: cognitive-services
services: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: diberry
ms.openlocfilehash: 086f55094474d4c06e52001d77630932cd04213c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60557435"
---
# <a name="quickstart-query-prediction-endpoint-with-c-net-sdk"></a>Hızlı Başlangıç: Sorgu tahmin noktayla C# .NET SDK'sı

.NET SDK'yı, bulunan [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/), Language Understanding (LUIS) kullanıcı utterance gönderin ve kullanıcının engellemekse bir tahminini almak için. 

Bu hızlı başlangıçta bir kullanıcı utterance gibi gönderir `turn on the bedroom light`, ortak bir dil anlama uygulamaya tahmin sonra alır ve üst Puanlama amacıyla görüntüler `HomeAutomation.TurnOn` ve varlık `HomeAutomation.Room` utterance içinde bulunamadı. 

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio Community 2017 sürümü](https://visualstudio.microsoft.com/vs/community/)
* C# programlama dili (VS Community 2017 sürümünde bulunur)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2

> [!Note]
> Eksiksiz bir çözüm kullanılabilir [bilişsel hizmetler-dil-anlama](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/sdk-quickstarts/c%23/UsePredictionRuntime) GitHub deposu.

Daha fazla belgelerini mi arıyorsunuz?

 * [SDK başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet)


## <a name="get-cognitive-services-or-language-understanding-key"></a>Bilişsel hizmetler veya Language Understanding anahtarını alma

Genel ev otomasyonu için kullanılabilmesi için uç nokta tahminler elde etmek için geçerli bir anahtar gerekir. Birçok bilişsel hizmetler için geçerli olan (aşağıdaki Azure CLI ile oluşturulan), ya da bir Bilişsel hizmetler anahtar kullanabilirsiniz veya `Language Understanding` anahtarı. 

Aşağıdaki [bir Bilişsel hizmet anahtarı oluşturmak için Azure CLI komutunu](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create):

```azurecli-interactive
az cognitiveservices account create \
    -n my-cog-serv-resource \
    -g my-cog-serv-resource-group \
    --kind CognitiveServices \
    --sku S0 \
    -l WestEurope \ 
    --yes
```

## <a name="create-net-core-project"></a>.NET Core projesi oluşturma

Visual Studio Community 2017'de .NET Core konsol projesi oluşturun.

1. Visual Studio Community 2017'yi açın.
1. Yeni bir proje oluşturma **Visual C#**  bölümünde, seçin **konsol uygulaması (.NET Core)**.
1. Proje adını girin `QueryPrediction`, kalan varsayılan değerleri değiştirmeyin ve seçin **Tamam**.
    Bu adlı birincil kod dosyası ile basit bir proje oluşturur **Program.cs**.

## <a name="add-sdk-with-nuget"></a>SDK'sı NuGet ile ekleme

1. İçinde **Çözüm Gezgini**, adlı ağaç görünümü'nde projeyi seçin **QueryPrediction**, sonra sağ tıklayın. Menüden **NuGet paketlerini Yönet...** .
1. Seçin **Gözat** enter `Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime`. Paket bilgileri görüntülediğinde, seçin **yükleme** projeye paketi yükleyin. 
1. Aşağıdaki _kullanarak_ üst tarafına deyimlerini **Program.cs**. Varolan kaldırmayın _kullanarak_ bildirimi `System`. 

```csharp
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime;
using Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime.Models;
```

## <a name="create-a-new-method-for-the-prediction"></a>Tahmin için yeni bir yöntem oluşturma

Yeni bir yöntem oluşturma `GetPrediction` sorgu tahmin uç noktaya sorgu göndermek için. Yöntemi oluşturun ve gerekli tüm nesneleri yapılandırın ardından iade bir `Task` ile [ `LuisResult` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.models.luisresult?view=azure-dotnet) tahmin sonuçlarını. 

```csharp
static async  Task<LuisResult> GetPrediction() {
}
```

## <a name="create-credentials-object"></a>Kimlik bilgileri nesnesi oluşturma

Aşağıdaki kodu ekleyin `GetPrediction` istemci kimlik bilgileri, Bilişsel hizmet anahtarı ile oluşturmak için yöntemi.

Değiştirin `<REPLACE-WITH-YOUR-KEY>` Bilişsel hizmet anahtarının bölgeyle. Anahtar yer [Azure portalında](https://portal.azure.com) bu kaynak için anahtarlar sayfasında.

```csharp
// Use Language Understanding or Cognitive Services key
// to create authentication credentials
var endpointPredictionkey = "<REPLACE-WITH-YOUR-KEY>";
var credentials = new ApiKeyServiceClientCredentials(endpointPredictionkey);
```

## <a name="create-language-understanding-client"></a>Language Understanding istemcisi oluşturma

İçinde `GetPrediction` yöntemi, önceki koddan sonra ekleyin oluşturma yeni kimlik bilgilerini kullanmak için aşağıdaki kodu bir [ `LUISRuntimeClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient.-ctor?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Runtime_LUISRuntimeClient__ctor_Microsoft_Rest_ServiceClientCredentials_System_Net_Http_DelegatingHandler___) istemci nesnesi. 

Değiştirin `<REPLACE-WITH-YOUR-KEY-REGION>` , anahtarın bölgeyle gibi `westus`. İçindeki anahtar bölgedir [Azure portalında](https://portal.azure.com) bu kaynak için genel bakış sayfasında.

```csharp
// Create Luis client and set endpoint
// region of endpoint must match key's region, for example `westus`
var luisClient = new LUISRuntimeClient(credentials, new System.Net.Http.DelegatingHandler[] { });
luisClient.Endpoint = "https://<REPLACE-WITH-YOUR-KEY-REGION>.api.cognitive.microsoft.com";
```

## <a name="set-query-parameters"></a>Sorgu parametrelerini ayarla

İçinde `GetPrediction` yöntemi, önceki koddan sonra sorgu parametrelerini ayarlamak için aşağıdaki kodu ekleyin.

```csharp
// public Language Understanding Home Automation app
var appId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";

// query specific to home automation app
var query = "turn on the bedroom light";

// common settings for remaining parameters
Double? timezoneOffset = null;
var verbose = true;
var staging = false;
var spellCheck = false;
String bingSpellCheckKey = null;
var log = false;
```

## <a name="query-prediction-endpoint"></a>Sorgu tahmin uç noktası

İçinde `GetPrediction` yöntemi, önceki koddan sonra sorgu parametrelerini ayarlamak için aşağıdaki kodu ekleyin:

```csharp
// Create prediction client
var prediction = new Prediction(luisClient);

// get prediction
return await prediction.ResolveAsync(appId, query, timezoneOffset, verbose, staging, spellCheck, bingSpellCheckKey, log, CancellationToken.None);
```

## <a name="display-prediction-results"></a>Tahmin sonuçlarını görüntüleme

Değişiklik **ana** yeni çağrılacak yöntem `GetPrediction` yöntemi ve dönüş tahmin sonuçları:

```csharp
static void Main(string[] args)
{

    var luisResult = GetPrediction().Result;

    // Display query
    Console.WriteLine("Query:'{0}'", luisResult.Query);

    // Display most common properties of query result
    Console.WriteLine("Top intent is '{0}' with score {1}", luisResult.TopScoringIntent.Intent,luisResult.TopScoringIntent.Score);

    // Display all entities detected in query utterance
    foreach (var entity in luisResult.Entities)
    {
        Console.WriteLine("{0}:'{1}' begins at position {2} and ends at position {3}", entity.Type, entity.Entity, entity.StartIndex, entity.EndIndex);
    }

    Console.Write("done");

}
```

## <a name="run-the-project"></a>Projeyi çalıştırma

Studio'da projeyi oluşturun ve projeyi görmek sorgunun çıkışı çalıştırın:

```console
Query:'turn on the bedroom light'
Top intent is 'HomeAutomation.TurnOn' with score 0.809439957
HomeAutomation.Room:'bedroom' begins at position 12 and ends at position 18
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [.NET SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/) ve [.NET başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet). 

> [!div class="nextstepaction"] 
> [Öğretici: Kullanıcı amaçları belirlemek için LUIS uygulaması oluşturma](luis-quickstart-intents-only.md) 