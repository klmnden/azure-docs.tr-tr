---
title: Web ara uç noktası | Microsoft Docs
description: Web ara API uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 72fbe1a0eb4379ad032e649f7299e37a0214adfb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351538"
---
# <a name="web-search-endpoint"></a>Web ara uç noktası
**Web arama API** döndürür Web sayfaları, haber, görüntüler, videolar ve [varlıklar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Varlıklar bir kişi, yer ya da konu ilgili özet bilgiler içerir.
## <a name="endpoint"></a>Uç Nokta
Bing API'si kullanarak Web arama sonuçları almak için gönderin bir `GET` şu uç nokta isteği. Daha fazla URL parametreleri ve üstbilgiler belirtimleri tanımlayın.

**Uç nokta**: sorgu tarafından tanımlanan arama kullanıcının ilgili döndürür Web sonuçları `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Uç noktası: Üst bilgiler, parametreleri hakkında ayrıntılı bilgi için pazara kodları, yanıt nesneleri, hatalar, vb. bkz [Bing Web API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) başvuru.

## <a name="response-json"></a>Yanıt JSON
Bir Web araması isteğinin yanıtı tüm sonuçları JSON nesneler olarak içerir. Sonuç ayrıştırma her türdeki öğeleri işlemek yordamları gerektirir. Bkz: [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source) örnekleri için.

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API'lerini destekleyen tipine göre sonuçları döndüren arama eylemler. Tüm arama uç noktaları sonuçları JSON yanıt nesneler olarak döndürür.  Tüm uç noktaları boylam, enlem ve arama RADIUS tarafından belirli bir dil ve/veya konum döndüren sorgular destekler.

Her bitiş noktası tarafından desteklenen parametreleri hakkında tam bilgi için her tür başvuru sayfalarına bakın.
Web araması API kullanan temel isteklerinin örnekleri için bkz: [Web hızlı başlatır arama](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).