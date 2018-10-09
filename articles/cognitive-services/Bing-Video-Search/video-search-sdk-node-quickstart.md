---
title: "Hızlı Başlangıç: Bing Video Arama SDK'sı, Node"
titleSuffix: Azure Cognitive Services
description: Bing Video Arama SDK'sı konsol uygulaması için kurulum.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: rosh
ms.openlocfilehash: 4dcc9220d4d38bfe34514edd6a3ad47c7a7d4ba8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225625"
---
# <a name="quickstart-bing-video-search-sdk-with-node"></a>Hızlı Başlangıç: Node ile Bing Video Arama SDK'sı

Bing Video Arama SDK'sı, video sorguları ve sonuçları ayrıştırma için REST API işlevselliğini içerir. 

[Node Bing Video Arama SDK'sı örnekleri için kaynak kodu](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js) Git Hub'dan edinilebilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing Video Arama SDK'sını kullanan bir konsol uygulaması kurmak için geliştirme ortamınızda `npm install azure-cognitiveservices-videosearch` komutunu çalıştırın.

## <a name="video-search-client"></a>Video Arama istemcisi
*Arama* altından bir [Bilişsel Hizmetler erişim anahtarı](https://azure.microsoft.com/try/cognitive-services/) alın. `CognitiveServicesCredentials` nesnesinin bir örneğini oluşturun:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemciyi örneklendirin:
```
const VideoSearchAPIClient = require('azure-cognitiveservices-videosearch');
let client = new VideoSearchAPIClient(credentials);
```
Sonuçları arayın.
```
client.videosOperations.search('Interstellar Trailer').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```

<!-- Remove until the response can be replace with a sanitized version.
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'VideoObjectElementType'

![Video results](media/video-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
