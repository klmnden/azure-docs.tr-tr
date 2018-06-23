---
title: Görüntü arama SDK düğümü hızlı başlangıç | Microsoft Docs
description: Görüntü Ara SDK konsol uygulaması ayarlayın.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e4c8303e39accbb7caec15c0ef47d701971ce632
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355253"
---
# <a name="image-search-sdk-node-quickstart"></a>Görüntü arama SDK düğümü hızlı başlangıç

Bing görüntü arama SDK'sı görüntü sorgular ve ayrıştırma sonuçları için REST API işlevselliğini içerir. 

[Kaynak kodu düğümü Bing görüntü arama SDK örnekleri için](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) Git hub'da kullanılabilir.

## <a name="application-dependencies"></a>Uygulama bağımlılıkları

Bing görüntü arama SDK'yı kullanarak bir konsol uygulaması ayarlamak için çalıştırın `npm install azure-cognitiveservices-imagesearch` geliştirme ortamınızda.

## <a name="image-search-client"></a>Görüntü arama istemci
Alma bir [Bilişsel hizmetler erişim tuşu](https://azure.microsoft.com/try/cognitive-services/) altında *arama*. Bir örneğini oluşturmak `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Ardından, istemci örneği:
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
İstemci, bu durumda 'El Capitan' sorgu metinle aramak için kullanın:
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

[Bilişsel hizmetler Node.js SDK'sı örneği](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)