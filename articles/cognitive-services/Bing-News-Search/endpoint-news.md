---
title: Bing Haber Arama uç noktaları
titlesuffix: Azure Cognitive Services
description: Haber arama API'si uç noktası özeti.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: 6ea218da23d65696c76008cb15e183fcc4bbda10
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66383220"
---
# <a name="bing-news-search-api-endpoints"></a>Bing haber arama API'si uç noktaları

**Haber arama API'si** makaleler, Web sayfaları, görüntüler, video, haber verir ve [varlıkları](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Varlıklar bir kişi, yer veya konu ile ilgili özet bilgiler içerir.

## <a name="endpoints"></a>Uç Noktalar

Bing haber arama API'si kullanarak arama sonuçlarını haberleri almak için gönderin bir `GET` aşağıdaki uç noktalarından birinin isteği. Daha fazla başlık ve URL parametrelerini belirtimleri tanımlayın.

### <a name="news-items-by-search-query"></a>Arama sorgusu haber öğeleri

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

Bir arama sorgusuna göre haber öğelerini döndürür. Arama sorgusu boş ise, API üst haber makalelerini farklı kategorileri döndürür. Arama teriminizi kodlama ve ona ekleme URL bir sorgu`q=""` parametresi. Kullanılabilirlik için bkz: [desteklenen ülkeler/bölgeler ile pazarlar](language-support.md#supported-markets-for-news-search-endpoint).

### <a name="top-news-items-by-category"></a>Kategoriye göre üst haber öğeleri

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

Kategoriye göre üst haber öğelerini döndürür. Özellikle en uygun işletme, spor veya eğlence makaleleri kullanarak isteyebilir `category=business`, `category=sports`, veya `category=entertainment`.  `category` Parametresi yalnızca kullanılabilir ile `/news` URL'si. Kategori belirleme için bazı biçimsel gereksinimi yoktur; başvurmak `category` içinde [sorgu parametresi](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference#query-parameters) belgeleri. Arama teriminizi kodlama ve ona ekleme URL bir sorgu`q=""` parametresi. Kullanılabilirlik için bkz: [desteklenen ülkeler/bölgeler ile pazarlar](language-support.md#supported-markets-for-news-endpoint).

### <a name="trending-news-topics"></a>Popüler haber konularında 

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics
```

Şu anda sosyal ağlarda Öne çıkanlar haber konularında döndürür. Zaman `/trendingtopics` seçeneği dahildir, Bing arama yoksayar diğer çeşitli parametreleri gibi `freshness` ve `?q=""`. Kullanılabilirlik için bkz: [desteklenen ülkeler/bölgeler ile pazarlar](language-support.md#supported-markets-for-news-trending-endpoint).

## <a name="next-steps"></a>Sonraki adımlar

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing haber arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) başvuru.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Haber arama API'si kullanarak temel istekleri örnekleri için bkz: [Bing haber arama hızlı başlangıçları](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).
