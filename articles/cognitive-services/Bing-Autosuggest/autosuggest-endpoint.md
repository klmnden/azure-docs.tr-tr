---
title: Uç nokta otomatik öneri | Microsoft Docs
description: Otomatik öneri API uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 5aaddd09006cb6f1812bb6ae213a2f5e6720fb1b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354491"
---
# <a name="autosuggest-endpoint"></a>Uç nokta otomatik öneri

**Otomatik öneri API** bir kısmi arama terimi önerilen sorguların listesini döndüren bir uç nokta içerir.

## <a name="endpoint"></a>Uç Nokta

Bing API'si kullanarak önerilen sorguları almak için gönderin bir `GET` şu uç nokta isteği. Üstbilgiler ve URL parametreleri daha ayrıntılı belirtimleri tanımlamak için kullanın.

**Uç noktası:** tarafından tanımlanan kullanıcı girişi için uygun olan JSON sonuçları döndürür arama önerileri `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Üstbilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hatalar, hakkındaki ayrıntılar için vs., bkz: [Bing otomatik öneri API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) başvuru.

## <a name="response-json"></a>Yanıt JSON

Bir autosuggest isteğine yanıt JSON nesnelerinin sonuçlarını içerir. Sonuçları ayrıştırma örnekleri görmek için [öğretici](tutorials/autosuggest.md) ve [kaynak kodu](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Sonraki adımlar

**Bing** API'lerini destekleyen tipine göre sonuçları döndüren arama eylemler. Tüm arama uç noktaları sonuçları JSON yanıt nesneler olarak döndürür.
Tüm uç noktaları boylam, enlem ve arama RADIUS tarafından belirli bir dil ve/veya konum döndüren sorgular destekler.

Her bitiş noktası tarafından desteklenen parametreleri hakkında tam bilgi için her tür başvuru sayfalarına bakın.
Otomatik öneri API kullanarak temel istekleri örnekleri için bkz: [otomatik öneri Quickstarts](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).