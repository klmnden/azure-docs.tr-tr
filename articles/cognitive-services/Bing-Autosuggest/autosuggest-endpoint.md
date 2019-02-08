---
title: Bing Otomatik Öneri uç noktası
titlesuffix: Azure Cognitive Services
description: Bing otomatik öneri API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 9e80e81a79f45f132baf07af7c2e7be8f98ceec4
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55857558"
---
# <a name="bing-autosuggest-endpoint"></a>Bing Otomatik Öneri uç noktası

**Bing otomatik öneri API'si** bir kısmi arama terimi önerilen sorgular listesi döndüren bir uç nokta içerir.

## <a name="endpoint"></a>Uç Nokta

Bing API'si kullanarak önerilen sorgular elde etmek için Gönder bir `GET` aşağıdaki uç noktayı isteği. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

**Uç noktası:** Tarafından tanımlanan kullanıcı girişi için uygun olan JSON sonucu döndürür arama önerileri `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing otomatik öneri API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) başvuru.

## <a name="response-json"></a>Response JSON

Otomatik öneri isteğine yanıt JSON nesneleri sonuçlarını içerir. Ayrıştırma sonuçlarını örnekleri görmek için [öğretici](tutorials/autosuggest.md) ve [kaynak kodu](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Sonraki adımlar

**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.
Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Otomatik öneri API'si kullanarak temel istekleri örnekleri için bkz: [otomatik öneri hızlı Başlangıçlar](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).
