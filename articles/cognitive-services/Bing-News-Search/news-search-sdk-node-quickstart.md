---
title: "Hızlı başlangıç: Bing Haber Arama SDK'sı, Node"
titleSuffix: Azure Cognitive Services
description: Bing Haber Arama SDK'sı konsol uygulaması kurulumu
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 075ce771b6fa92663f172734aabf57b089b294a9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316948"
---
# <a name="quickstart-bing-news-search-sdk-with-node"></a>Hızlı başlangıç: Node ile Bing Haber Arama SDK'sı

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
- _type: 'NewsArticle'
- _type: 'WebPage'
- _type: 'VideoObject'
- _type: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
