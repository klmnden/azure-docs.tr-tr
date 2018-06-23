---
title: Web araması SDK düğümü hızlı başlangıç | Microsoft Docs
description: Web ara SDK konsol uygulaması ayarlayın.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 44f7f97f6c442df3fbb1e5e08189b8db7d4b9db0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355300"
---
# <a name="web-search-sdk-node-quickstart"></a>Web arama SDK düğümü hızlı başlangıç

Bing Web arama SDK'sı web sorguları ve ayrıştırma sonuçları için REST API işlevselliğini içerir.

[Kaynak kodu düğümü Bing Web arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/webSearch.js) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Web arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için çalıştırın `npm install azure-cognitiveservices-websearch` geliştirme ortamınızda.

## <a name="web-search-client"></a>Web ara istemcisi
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. Bir örneğini oluşturmak `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemci örneği:
```
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```
Arama sonuçları için:
```
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})

```
Kod yazdırır `result.value` herhangi bir metin ayrıştırma olmadan konsola öğeler.  Varsa her kategori, sonuçları içerir:
- _ad: 'ImageObject'
- _ad: 'NewsArticle'
- _ad: 'Web sayfası'
- _ad: 'VideoObjectElementType'

<!-- Remove until this can be replaced with a sanitized version.
![Video results](media/web-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
