---
title: "Hızlı Başlangıç: Haber arama - Bing haber arama SDK'sı için Node.js gerçekleştirin"
titleSuffix: Azure Cognitive Services
description: Yanıta işlemek ve bu hızlı başlangıçta Node.js için Bing haber arama SDK'sını kullanarak haber arama için kullanın.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.custom: seodec2018
ms.openlocfilehash: 0fa78474d6400f890626859c32915c8657f92d4e
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53249354"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-nodejs"></a>Hızlı Başlangıç: Node.js için Bing haber arama SDK'sı ile haber araması

Bing Haber Arama SDK'sı, haber sorguları ve sonuçları ayrıştırmak için REST API işlevselliğini içerir. 

[Node Bing Haber Arama SDK'sı örnekleri için kaynak kodu](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) Git Hub'dan edinilebilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları
**Arama** altından bir [Bilişsel Hizmetler erişim anahtarı](https://azure.microsoft.com/try/cognitive-services/) alın.  Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/). 

Bing haber arama SDK'sını kullanarak bir konsol uygulaması ayarlamak için:
* Çalıştırma `npm install ms-rest-azure` geliştirme ortamınızda.
* Çalıştırma `npm install azure-cognitiveservices-newssearch` geliştirme ortamınızda.

## <a name="news-search-client"></a>Haber Arama istemcisi
*Arama* altından bir [Bilişsel Hizmetler erişim anahtarı](https://azure.microsoft.com/try/cognitive-services/) alın. `CognitiveServicesCredentials` nesnesinin bir örneğini oluşturun:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemciyi örneklendirin:
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
İstemciyle 'Winter Olympics' sorgu metnini arayın:
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
Kod konsola `result.value` öğelerini yazdırır ve metin ayrıştırması gerçekleştirmez. Varsa sonuçlar kategorilere ayrılmış şekilde şunları içerir:
- _türü: 'NewsArticle'
- _türü: 'Web'
- _türü: 'VideoObject'
- _türü: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
