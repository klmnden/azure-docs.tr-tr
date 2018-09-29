---
title: Bing haber arama uç noktaları | Microsoft Docs
description: Haber arama API'si uç noktası özeti.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: ab892e947566adf025499382b213a52ed3e96e35
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433693"
---
# <a name="news-search-endpoints"></a>Haber arama uç noktaları
**Haber arama API'si** makaleler, Web sayfaları, görüntüler, video, haber verir ve [varlıkları](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Varlıklar bir kişi, yer veya konu ile ilgili özet bilgiler içerir.
## <a name="endpoints"></a>Uç Noktalar
Bing API'sini kullanarak haber araması sonuçları almak için gönderin bir `GET` aşağıdaki uç noktalarından birinin isteği. Daha fazla başlık ve URL parametrelerini belirtimleri tanımlayın.

**Uç nokta 1:** kullanıcının arama sorgusuna göre haber öğelerini döndürür. Arama sorgusu boş ise, çağrı üst haber makalelerini döndürür. Sorgu `?q=""` seçeneği de kullanılabilir olan `/news` URL'si. Kullanılabilirlik için bkz: [desteklenen ülkeler ve pazarlara](language-support.md#supported-markets-for-news-search-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

**Uç nokta 2:** kategoriye göre üst haber öğelerini döndürür. Özellikle en uygun işletme, spor veya eğlence makaleleri kullanarak isteyebilir `category=business`, `category=sports`, veya `category=entertainment`.  `category` Parametresi yalnızca kullanılabilir ile `/news` URL'si. Kategori belirleme için bazı biçimsel gereksinimi yoktur; başvurmak `category` içinde [sorgu parametresi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters) belgeleri. Kullanılabilirlik için bkz: [desteklenen ülkeler ve pazarlara](language-support.md#supported-markets-for-news-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

**Uç noktası 3:** şu anda sosyal ağlarda Öne çıkanlar döndürür haber konuları. Zaman `/trendingtopics` seçeneği dahildir, Bing arama yoksayar diğer çeşitli parametreleri gibi `freshness` ve `?q=""`. Kullanılabilirlik için bkz: [desteklenen ülkeler ve pazarlara](language-support.md#supported-markets-for-news-trending-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics
```

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing haber arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) başvuru.
## <a name="response-json"></a>JSON yanıtı
Haber arama isteğine yanıt JSON nesneleri sonuçlarını içerir. Ayrıştırma sonuçlarını her türün öğelerini işlemek yordamları gerektirir. Bkz: [öğretici](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app) ve [kaynak kodu](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app-source) örnekler.

## <a name="next-steps"></a>Sonraki adımlar
**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.  Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Haber arama API'si kullanarak temel istekleri örnekleri için bkz: [Bing haber arama hızlı başlangıçları](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).
