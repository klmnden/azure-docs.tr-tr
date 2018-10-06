---
title: Bing Otomatik Öneri uç noktası
titlesuffix: Azure Cognitive Services
description: Bing otomatik öneri API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: c2d1c97ad2af266558f9b664162526d5006d2092
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830453"
---
# <a name="bing-autosuggest-endpoint"></a>Bing Otomatik Öneri uç noktası

**Bing otomatik öneri API'si** bir kısmi arama terimi önerilen sorgular listesi döndüren bir uç nokta içerir.

## <a name="endpoint"></a>Uç Nokta

Bing API'si kullanarak önerilen sorgular elde etmek için Gönder bir `GET` aşağıdaki uç noktayı isteği. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

**Uç noktası:** tarafından tanımlanan kullanıcı girişi için uygun olan JSON sonucu döndürür arama önerileri `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing otomatik öneri API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) başvuru.

## <a name="response-json"></a>JSON yanıtı

Otomatik öneri isteğine yanıt JSON nesneleri sonuçlarını içerir. Ayrıştırma sonuçlarını örnekleri görmek için [öğretici](tutorials/autosuggest.md) ve [kaynak kodu](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Sonraki adımlar

**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.
Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Otomatik öneri API'si kullanarak temel istekleri örnekleri için bkz: [otomatik öneri hızlı Başlangıçlar](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).