---
title: Bing varlık arama API'si uç noktası
titlesuffix: Azure Cognitive Services
description: Bing varlık arama API'si uç noktası hakkında bilgi edinin ve istekleri göndermek.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: v-gedod
ms.openlocfilehash: 3c2aa4b22c8e679f73692978d9e1f8009f11a46b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875238"
---
# <a name="bing-entity-search-api-endpoint"></a>Bing varlık arama API'si uç noktası


Bing varlık arama API'si, bir sorguyu temel alan Web varlık döndüren bir uç nokta vardır. Bu arama sonuçları JSON biçiminde döndürülür.

## <a name="get-entity-results-from-the-endpoint"></a>Uç noktasından varlığı sonuçlarını alın

Varlık alma sonuçlarını kullanarak **Bing API**, gönderme bir `GET` aşağıdaki uç noktayı isteği. Kullanım [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#headers) ve [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query-parameters) arama isteğiniz özelleştirmek için. Arama istekleri kullanarak gönderilebilir `?q=` parametresi.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bing varlık arama API'si nedir?](overview.md)

## <a name="see-also"></a>Ayrıca bkz. 

Üst bilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hatalar ve daha fazla hakkında daha fazla bilgi için bkz: [Bing varlık arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) başvurusu makalesinde.
