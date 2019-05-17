---
title: 'Hızlı Başlangıç: Bing yazım denetlemek için SDK ile yazım denetimiC#'
titlesuffix: Azure Cognitive Services
description: Bing yazım denetimi REST API'si yazım ve dilbilgisi denetimini kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: adbb60c7ddbc72b8b7e5cb31c6909117ce3a10cb
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798373"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-sdk-for-c"></a>Hızlı Başlangıç: Bing yazım denetlemek için SDK ile yazım denetimiC#

Bing yazım denetlemek için SDK ile yazım başlamak için bu hızlı başlangıçta kullanmak C#. Bing yazım denetimi çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/SpellCheck).

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

* Herhangi bir sürümünü [Visual Studio 2017 veya üstü](https://visualstudio.microsoft.com/downloads/).
* Bing yazım denetimi [NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.SpellCheck)

Bing yazım denetleme SDK'sını projenize eklemek için seçin **NuGet paketlerini Yönet** gelen **Çözüm Gezgini** Visual Studio'da. `Microsoft.Azure.CognitiveServices.Language.SpellCheck` paketini ekleyin. Paket, aşağıdaki bağımlılıkları da yükler:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Yeni bir C# çözümünü Visual Studio'da konsol. Ardından aşağıdakileri ekleyin `using` deyimi.
    
    ```csharp
    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.SpellCheck;
    using Microsoft.Azure.CognitiveServices.Language.SpellCheck.Models;
    ```

2. Yeni bir sınıf oluşturun. Çağrılan zaman uyumsuz bir işlev oluşturup `SpellCheckCorrection()` , bir abonelik anahtarı alır ve yazım denetleme isteği gönderir.

3. İstemci yeni bir örneğini `ApiKeyServiceClientCredentials` nesne. 

    ```csharp
    public static class SpellCheckSample{
        public static async Task SpellCheckCorrection(string key){
            var client = new SpellCheckClient(new ApiKeyServiceClientCredentials(key));
        }
        //...
    }
    ```

## <a name="send-the-request-and-read-the-response"></a>İstek göndermek ve yanıtın okuma

1. Yukarıda oluşturduğunuz işlevde harcanan, aşağıdaki adımları gerçekleştirin. İstemcisi ile yazım onay isteği gönderin. İçin denetlenecek metin ekleyin `text` parametresi ve modunun `proof`.  
    
    ```csharp
    var result = await client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas", mode: "proof");
    ```

2. Varsa, ilk yazım denetimi sonucu alın. Döndürülen ilk yazılmış sözcüğün (belirteç), belirteç türü ve öneriler sayısı yazdırın.

    ```csharp
    if (firstspellCheckResult != null){
        var firstspellCheckResult = result.Body.FlaggedTokens.FirstOrDefault();
    
        Console.WriteLine("SpellCheck Results#{0}", result.Body.FlaggedTokens.Count);
        Console.WriteLine("First SpellCheck Result token: {0} ", firstspellCheckResult.Token);
        Console.WriteLine("First SpellCheck Result Type: {0} ", firstspellCheckResult.Type);
        Console.WriteLine("First SpellCheck Result Suggestion Count: {0} ", firstspellCheckResult.Suggestions.Count);
    }
    ```

3. Varsa, ilk önerilen düzeltmeyi alın. Öneri puanı ve önerilen sözcük yazdırın. 

    ```csharp
            var suggestions = firstspellCheckResult.Suggestions;

            if (suggestions?.Count > 0)
            {
                var firstSuggestion = suggestions.FirstOrDefault();
                Console.WriteLine("First SpellCheck Suggestion Score: {0} ", firstSuggestion.Score);
                Console.WriteLine("First SpellCheck Suggestion : {0} ", firstSuggestion.Suggestion);
            }
   }

## Next steps

> [!div class="nextstepaction"]
> [Create a single page web-app](tutorials/spellcheck.md)

- [What is the Bing Spell Check API?](overview.md)
- [Bing Spell Check C# SDK reference guide](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingspellcheck?view=azure-dotnet)