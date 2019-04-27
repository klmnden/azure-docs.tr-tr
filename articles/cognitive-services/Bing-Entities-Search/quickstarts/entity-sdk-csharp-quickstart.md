---
title: "Hızlı Başlangıç: Bing varlık arama için SDK'sı ile varlıkları AraC#"
titleSuffix: Azure Cognitive Services
description: Bu Hızlı Başlangıç için Bing varlık arama SDK'sı ile varlıkları aramak için kullanın C#.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 02/01/2019
ms.author: v-gedod
ms.openlocfilehash: 99b7f05304b48b7d885a80705d05fbe24854150f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60708415"
---
# <a name="send-a-search-request-with-the-bing-entity-search-sdk-for-c"></a>Bing varlık arama SDK ile arama isteği gönderC#

Bing varlık arama için SDK'sı ile varlıkları için aramaya başlamak için bu hızlı başlangıçta kullanmak C#. Bing varlık arama çoğu programlama dilleri ile uyumlu bir REST API olsa da SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingEntitySearch).


## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’nin herhangi bir sürümü.
* NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi.
* Linux/MacOS kullanıyorsanız bu uygulama, [Mono](https://www.mono-project.com/) kullanılarak çalıştırılabilir.
* [Bing haber arama SDK'sı NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0). Bu paketi yüklerken Ayrıca aşağıdakileri yükler:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Bing varlık arama SDK'sı, Visual Studio projenize eklemek için `Manage NuGet Packages` seçeneği Çözüm Gezgini'nden ve ekleme `Microsoft.Azure.CognitiveServices.Search.EntitySearch` paket.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-an-application"></a>Oluşturma ve bir uygulama başlatma

1. Yeni bir C# çözümünü Visual Studio'da konsol. Ardından ana kod dosyasına aşağıdakileri ekleyin.

    ```csharp
    using System;
    using System.Linq;
    using System.Text;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
    using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;
    using Newtonsoft.Json;
    ```

## <a name="create-a-client-and-send-a-search-request"></a>Bir istemci oluşturmanız ve arama isteği gönder

1. Yeni bir arama istemci oluşturun. Yeni bir abonelik anahtarınızı ekleme `ApiKeyServiceClientCredentials`.

    ```csharp
    var client = new EntitySearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```

1. İstemcinin kullanmak `Entities.Search()` sorgunuz için aranacak işlev:
    
    ```csharp
    var entityData = client.Entities.Search(query: "Satya Nadella");
    ```

## <a name="get-and-print-an-entity-description"></a>Alma ve bir varlık açıklaması yazdırma

1. API arama sonuçları döndürülürse, ana varlık kümesinden varlık alma `entityData`.

    ```csharp
    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();
    ```

2. Yazdırma ana varlık açıklaması 

    ```csharp
    Console.WriteLine(mainEntity.Description);
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing varlık arama API'si nedir?](../overview.md )