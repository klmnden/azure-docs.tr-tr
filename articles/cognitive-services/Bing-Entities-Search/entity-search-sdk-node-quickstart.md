---
title: "Hızlı başlangıç: Bing Varlık Arama SDK'sı, Node"
titleSuffix: Azure Cognitive Services
description: Node ile Varlık Arama SDK'sı konsol uygulaması kurulumu.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 69bcca7871d9bf4bebf64c0c0ae1b54cd8408927
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684129"
---
# <a name="quickstart-bing-entity-search-sdk-with-node"></a>Hızlı Başlangıç: Node ile Bing Varlık Arama SDK'sı

Bing Varlık Arama SDK'sı, varlık sorguları ve sonuçları ayrıştırmak için REST API işlevselliğini içerir. 

[C# Bing Varlık Arama SDK'sı örneklerinin kaynak kodu](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) Git Hub'dan edinilebilir.
## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing varlık arama SDK'sını kullanarak bir konsol uygulaması ayarlamak için:
* Çalıştırma `npm install ms-rest-azure` geliştirme ortamınızda.
* Çalıştırma `npm install azure-cognitiveservices-entitysearch` geliştirme ortamınızda.

## <a name="entity-search-client"></a>Varlık Arama istemcisi
*Arama* altından bir [Bilişsel Hizmetler erişim anahtarı](https://azure.microsoft.com/try/cognitive-services/) alın. `CognitiveServicesCredentials` nesnesinin bir örneğini oluşturun:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından istemciyi başlatın ve sonuç arayın:
```
const EntitySearchAPIClient = require('azure-cognitiveservices-entitysearch');

let entitySearchApiClient = new EntitySearchAPIClient(credentials);

entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
    console.log(result.queryContext);
    console.log(result.entities.value);
    console.log(result.entities.value[0].description);
}).catch((err) => {
    throw err;
});

```
Kod konsola `result.value` öğelerini yazdırır ve metin ayrıştırması gerçekleştirmez.  Varsa sonuçlar kategorilere ayrılmış şekilde şunları içerir:
- _type: 'Thing'
- _type: 'ImageObject'

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK'sı örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)