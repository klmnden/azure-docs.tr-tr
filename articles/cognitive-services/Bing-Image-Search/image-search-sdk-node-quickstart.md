---
title: "Hızlı Başlangıç: İstek ve Node.js SDK'sını kullanarak görüntüleri Filtrele"
description: Bu hızlı başlangıçta, istek ve Node.js kullanarak Bing resim arama tarafından döndürülen görüntüleri Filtrele.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e88c045b220192a617e6b8caf5d8d53f70a25b5e
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "41994397"
---
# <a name="quickstart-request-and-filter-images-using-the-sdk-and-nodejs"></a>Hızlı Başlangıç: Node.js ve SDK'sı kullanarak istek ve filtre görüntüleri

Bing görüntü arama SDK'sı, görüntü sorgular ve ayrıştırma sonuçları için REST API işlevselliğini içerir. 

[Kaynak düğüm Bing resim arama SDK örnekleri için kodu](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing görüntü arama SDK'sını kullanarak bir konsol uygulaması ayarlamak için çalıştırın `npm install azure-cognitiveservices-imagesearch` geliştirme ortamınızda.

## <a name="image-search-client"></a>Görüntü arama istemci
Alma bir [Bilişsel hizmetler erişim anahtarını](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. Bir örneğini oluşturmak `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemci örneği:
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
İstemci ile bu durumda 'El Capitan' sorgu metni aramak için kullanın:
```
client.imagesOperations.search('El Capitan', function (err, result, request, response) {
    if (err) throw err;
    console.log(result.value);
});

```
<!-- Need to sanitize result
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'ImageObjectElementType'

![Imageresults](media/node-sdk-quickstart-image-results.png)
-->

## <a name="next-steps"></a>Sonraki adımlar

[Bilişsel hizmetler Node.js SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)