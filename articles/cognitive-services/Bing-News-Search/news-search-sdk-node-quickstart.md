---
title: Haber arama SDK düğümü hızlı başlangıç | Microsoft Docs
description: Haber arama SDK konsol uygulama ayarlama
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 4ae99aa100b697a0dd75863c6f0c3c556dfa3d21
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355283"
---
# <a name="news-search-sdk-node-quickstart"></a>Haber arama SDK düğümü hızlı başlangıç

Bing Haberler arama SDK haber sorgular ve ayrıştırma sonuçları için REST API işlevselliğini içerir. 

[Kaynak kodu düğümü Bing Haberler arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Haberler arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için çalıştırın `npm install azure-cognitiveservices-newssearch` geliştirme ortamınızda.

## <a name="news-search-client"></a>Haber Arama İstemcisi
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. Bir örneğini oluşturmak `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemci örneği:
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
İstemci, bu durumda 'Kış Olimpiyatlar' sorgu metinle aramak için kullanın:
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
Kod yazdırır `result.value` herhangi bir metin ayrıştırma olmadan konsola öğeler. Varsa her kategori, sonuçları içerir:
- _ad: 'NewsArticle'
- _ad: 'Web sayfası'
- _ad: 'VideoObject'
- _ad: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
