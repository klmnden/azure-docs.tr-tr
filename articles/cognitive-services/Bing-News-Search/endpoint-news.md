---
title: Bing Haberler arama uç noktaları | Microsoft Docs
description: Haber arama API uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: b8ae99698926a818bf22b0b4027af8610413aaa6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351509"
---
# <a name="news-search-endpoints"></a>Haber arama uç noktaları
**Haber arama API** haber makaleleri, Web sayfaları, görüntüler, videolar döndürür ve [varlıklar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Varlıklar bir kişi, yer ya da konu ilgili özet bilgiler içerir. 
## <a name="endpoints"></a>Uç Noktalar
Bing API'si kullanılarak haber arama sonuçları almak için gönderin bir `GET` aşağıdaki uç noktalardan biri isteği. Daha fazla URL parametreleri ve üstbilgiler belirtimleri tanımlayın.

**Uç nokta 1:** haber öğeleri kullanıcının arama sorgusuna dayalı olarak döndürür. Arama sorgusu boş ise, çağrı üst haber makalelerini döndürür. Sorgu `?q=""` seçeneği de kullanılabilir olan `/news` URL. Kullanılabilirlik için bkz: [desteklenen ülke ve pazarda](supported-countries-markets.md#supported-markets-for-news-search-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search 
``` 

**Uç nokta 2:** kategoriye göre üst haber öğeleri döndürür. Üst iş, spor veya kullanan eğlence makaleler özellikle isteği `category=business`, `category=sports`, veya `category=entertainment`.  `category` Parametresi yalnızca kullanılabilir ile `/news` URL. Kategoriler belirtmek için bazı resmi gereksinimi yoktur; başvurmak `category` içinde [sorgu parametresi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters) belgeleri. Kullanılabilirlik için bkz: [desteklenen ülke ve pazarda](supported-countries-markets.md#supported-markets-for-news-endpoint). 
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

**Uç noktası 3:** sosyal ağlar üzerinde şu anda eğilim haber konuları döndürür. Zaman `/trendingtopics` seçeneği bulunur, Bing arama yoksayar diğer çeşitli parametreleri gibi `freshness` ve `?q=""`. Kullanılabilirlik için bkz: [desteklenen ülke ve pazarda](supported-countries-markets.md#supported-markets-for-news-trending-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics 
``` 

Üstbilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hatalar, hakkındaki ayrıntılar için vs., bkz: [Bing Haberler arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) başvuru.
## <a name="response-json"></a>Yanıt JSON
Bir haber arama isteğine yanıt JSON nesnelerinin sonuçlarını içerir. Sonuçları ayrıştırma her türdeki öğeleri işlemek yordamları gerektirir. Bkz: [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app-source) örnekleri için.

## <a name="next-steps"></a>Sonraki adımlar
**Bing** API'lerini destekleyen tipine göre sonuçları döndüren arama eylemler. Tüm arama uç noktaları sonuçları JSON yanıt nesneler olarak döndürür.  Tüm uç noktaları boylam, enlem ve arama RADIUS tarafından belirli bir dil ve/veya konum döndüren sorgular destekler.

Her bitiş noktası tarafından desteklenen parametreleri hakkında tam bilgi için her tür başvuru sayfalarına bakın.
Haber arama API kullanan temel isteklerinin örnekleri için bkz: [Bing Haberler arama hızlı-başlatır](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).