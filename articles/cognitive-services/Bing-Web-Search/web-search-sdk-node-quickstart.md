---
title: "Hızlı Başlangıç: Bing Web araması SDK'sı için Node.js kullanma"
description: Web araması için SDK konsol uygulaması ayarlayın.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 08/16/2018
ms.author: v-gedod, erhopf
ms.openlocfilehash: e25c295fc0fc144110325d3c494a513ea35aeb05
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888599"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-nodejs"></a>Hızlı Başlangıç: Bing Web araması SDK'sı için Node.js kullanma

Bing Web araması SDK'sı web sorgular ve ayrıştırma sonuçları için REST API işlevlerini içerir.

[Kaynak düğüm Bing Web araması SDK'sı örnekleri için kodu](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/webSearch.js) Github'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Web araması SDK'sını kullanarak bir konsol uygulaması ayarlamak için çalıştırın `npm install azure-cognitiveservices-websearch` geliştirme ortamınızda.

## <a name="web-search-client"></a>Web araması istemcisi
Alma bir [Bilişsel hizmetler abonelik anahtarı](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. Bir örneğini oluşturmak `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemci örneği:
```
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```
Sonuçları Ara:
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
Kod yazdırır `result.value` herhangi bir metni ayrıştırma olmadan konsola öğeleri.  Varsa, kategori başına sonuçlar şunları içerir:
- _türü: 'ImageObject'
- _türü: 'NewsArticle'
- _türü: 'Web'
- _türü: 'VideoObjectElementType'

<!-- Remove until this can be replaced with a sanitized version.
![Video results](media/web-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
