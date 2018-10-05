---
title: Bing haber arama SDK'sı
titleSuffix: Azure Cognitive Services
description: Web araması uygulamaları için Bing haber arama SDK.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 1/24/2018
ms.author: v-gedod
ms.openlocfilehash: b1d9eaa35416adfa11647f2116171256f82fe095
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801242"
---
# <a name="bing-search-sdk"></a>Bing arama SDK'sı
Bing haber arama API'si örnekleri senaryoları dahil eden:
1. Haber arama koşulları için sorgu `market` ve `count` parametre sonuçları sayısını doğrulayın ve yazdırabilirsiniz `totalEstimatedMatches`, adı, URL, açıklama, yayımlanma zamanına ve ilk haber sonucun sağlayıcısının adı.
2. Sorgu en son haberler için arama terimlerini ile `freshness` ve `sortBy` parametre sonuçları sayısını doğrulayın ve yazdırabilirsiniz `totalEstimatedMatches`, URL, açıklama, yayımlanma zamanına ve ilk haber sonucun sağlayıcısının adı.
3. Sorgu için bir kategori haber `movie` ve `TV entertainment` güvenli arama sonuçları sayısını doğrulamanız ve kategorisi, adı, URL, açıklama, yayımlanma zamanına ve ilk haber sonucun sağlayıcısının adını yazdırma.
4. Sorgu, Bing, haber popüler konularını sonuç sayısı doğrulayın ve adı, sorgu metin yazdırma `webSearchUrl`, `newsSearchUrl` ve resim URL'si ilk haber sonucu.

Bing arama SDK'ları web arama işlevselliği aşağıdaki programlama dillerinde kolayca erişilebilir hale getirir:
* Kullanmaya başlama [.NET örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
    * [NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0)
    * Ayrıca bkz: [.NET kitaplıkları](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingNewsSearch) tanımlar ve bağımlılıkları için.
* Kullanmaya başlama [Node.js Örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Ayrıca bkz: [Node.js kitaplıkları](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/newsSearch) tanımlar ve bağımlılıkları için.
* Kullanmaya başlama [Java örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Ayrıca bkz: [Java kitaplıkları](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) tanımlar ve bağımlılıkları için.
* Kullanmaya başlama [Python örnekleri](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Ayrıca bkz: [Python kitaplıkları](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-newssearch) tanımlar ve bağımlılıkları için.

SDK'sı örnekleri her dil için Önkoşullar ve yükleme/çalışan örnekleri ayrıntılarını içeren bir benioku dosyası içerir.