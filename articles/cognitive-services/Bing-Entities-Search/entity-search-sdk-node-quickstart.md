---
title: Varlık arama SDK düğümü hızlı başlangıç | Microsoft Docs
description: Varlık Ara SDK konsol uygulaması ayarlayın.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 2904ecfed33334458f9b6a9ca2500cd0bfef13bc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355738"
---
# <a name="entity-search-sdk-node-quickstart"></a>Varlık arama SDK düğümü hızlı başlangıç

Bing varlık arama SDK'sı varlık sorgular ve ayrıştırma sonuçları için REST API işlevselliğini içerir. 

[Kaynak kodu C# Bing varlık arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) Git hub'da kullanılabilir.
## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing varlık arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için çalıştırın `npm install azure-cognitiveservices-entitysearch` geliştirme ortamınızda.

## <a name="entity-search-client"></a>Varlık arama istemci
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. Bir örneğini oluşturmak `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemci oluşturma ve arama sonuçları için:
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
Kod yazdırır `result.value` herhangi bir metin ayrıştırma olmadan konsola öğeler.  Varsa her kategori, sonuçları içerir:
- _ad: 'Şey'
- _ad: 'ImageObject'

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)