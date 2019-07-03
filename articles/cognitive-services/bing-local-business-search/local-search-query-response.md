---
title: Gönderme ve Bing yerel iş arama API'si sorgularının ve yanıtlarının kullanarak | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Gönder ve Bing yerel iş arama API'si ile arama sorguları kullanma hakkında bilgi edinmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: article
ms.date: 06/26/2018
ms.author: rosh; v-gedod
ms.openlocfilehash: e2911ebe9157507534717a4177d4380813dd2ff6
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541563"
---
# <a name="sending-and-using-bing-local-business-search-api-queries-and-responses"></a>Gönderme ve Bing yerel iş arama API'si sorgularının ve yanıtlarının kullanma

Kendi uç noktasına bir arama sorgusu gönderip, dahil olmak üzere Bing yerel iş arama API'den yerel sonuçlar alabilirsiniz `Ocp-Apim-Subscription-Key` üst bilgisi gereklidir. Kullanılabilir birlikte [üstbilgileri](local-search-reference.md#headers) ve [parametreleri](local-search-reference.md#query-parameters), aramaları belirterek özelleştirilebilir [coğrafi sınırlar](specify-geographic-search.md) aranacak alan ve [kategorileri](local-search-query-response.md) basamak döndürdü.

## <a name="creating-a-request"></a>Bir isteği oluşturma

Bing yerel iş arama API'si için bir istek göndermek için bir arama terimi için ekleme `q=` parametresi için API uç noktası ekleme ve dahil olmak üzere önce `Ocp-Apim-Subscription-Key` başlığı. Örneğin:

`https://api.cognitive.microsoft.com/bing/localbusinesses/v7.0/search?q=restaurant+in+Bellevue`

Tam istek URL'si sözdizimi aşağıda gösterilmiştir. Bing yerel iş arama API'si bkz [hızlı başlangıçlar](quickstarts/local-quickstart.md)ve başvuru içeriğini [üstbilgileri](local-search-reference.md#headers) ve [parametreleri](local-search-reference.md#query-parameters) istekleri gönderme hakkında daha fazla bilgi için. 

Yerel arama kategorileri hakkında daha fazla bilgi için bkz: [Bing yerel iş arama API'si için kategorileri arama](local-categories.md).

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search[?q][&localCategories][&cc][&mkt][&safesearch][&setlang][&count][&first][&localCircularView][&localMapView]
```

## <a name="using-responses"></a>Yanıtlarını kullanma

Bing yerel iş arama API'si, JSON yanıt içeren bir `SearchResponse` nesne. API alakalı arama sonuçları döndürür `places` alan. Sonuç bulunursa `places` alan yanıta dahil edilmeyecek.

[!INCLUDE [cognitive-services-bing-url-note](../../../includes/cognitive-services-bing-url-note.md)]

```
{
   "_type": "SearchResponse",
   "queryContext": {
      "originalQuery": "restaurant in Bellevue"
   },
   "places": {
      "totalEstimatedMatches": 10,
. . . 
```

### <a name="search-result-attributes"></a>Arama sonucu öznitelikleri

API tarafından döndürülen JSON sonuçları öznitelikleri şunlardır:

* _type
* Adresi
* entityPresentationInfo
* Coğrafi
* id
* name
* routeablePoint
* Telefon
* url

Üst bilgiler, parametreleri, Pazar kodları, yanıt nesneleri, hataları, hakkında genel bilgiler vb., bkz: [yerel Bing arama API'si v7](local-search-reference.md) başvuru.

> [!NOTE]
> Veya sizin adınıza bir üçüncü taraf değil kullanın, korumak, depolamak, önbellek, paylaşım, test, geliştirme, eğitim, dağıtma veya Microsoft olmayan bir hizmette kullanılabilir hale getirme amacıyla yerel arama API'si tüm veriler dağıtmak veya özellik. 


## <a name="example-json-response"></a>Örnek JSON yanıtı

Sorgu tarafından belirtilen arama sonuçları aşağıdaki JSON yanıtı içeren `?q=restaurant+in+Bellevue`.

```json
Vary: Accept-Encoding
BingAPIs-TraceId: 5376FFEB65294E24BB9F91AD70545826
BingAPIs-SessionId: 06ED7CEC80F746AA892EDAAC97CB0CB4
X-MSEdge-ClientID: 112C391E72C0624204153594738C63DE
X-MSAPI-UserState: aeab
BingAPIs-Market: en-US
X-Search-ResponseInfo: InternalResponseTime=659,MSDatacenter=CO4
X-MSEdge-Ref: Ref A: 5376FFEB65294E24BB9F91AD70545826 Ref B: BY3EDGE0306 Ref C: 2018-10-16T16:26:15Z
apim-request-id: fe54f585-7c54-4bf5-8b92-b9bede2b710a
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Cache-Control: max-age=0, private
Date: Tue, 16 Oct 2018 16:26:15 GMT
P3P: CP="NON UNI COM NAV STA LOC CURa DEVa PSAa PSDa OUR IND"
Content-Length: 978
Content-Type: application/json; charset=utf-8
Expires: Tue, 16 Oct 2018 16:25:15 GMT

{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "restaurant Bellevue"
  },
  "places": {
    "totalEstimatedMatches": 50,
    "value": [{
      "_type": "LocalBusiness",
      "id": "https:\/\/cognitivegblppe.azure-api.net\/api\/v7\/#Places.0",
      "name": "Facing East Taiwanese Restaurant",
      "url": "http:\/\/litadesign.wix.com\/facingeastrestaurant",
      "entityPresentationInfo": {
        "entityScenario": "ListItem",
        "entityTypeHints": ["Place", "LocalBusiness", "Restaurant"]
      },
      "geo": {
        "latitude": 47.6199188232422,
        "longitude": -122.202796936035
      },
      "routablePoint": {
        "latitude": 47.6199188232422,
        "longitude": -122.201713562012
      },
      "address": {
        "streetAddress": "1075 Bellevue Way NE Ste B2",
        "addressLocality": "Bellevue",
        "addressRegion": "WA",
        "postalCode": "98004",
        "addressCountry": "US",
        "neighborhood": "Bellevue",
        "text": "1075 Bellevue Way NE Ste B2, Bellevue, WA 98004"
      },
      "telephone": "(425) 688-2986"
    }],
    "searchAction": {
      "location": [{
        "name": "Bellevue, Washington"
      }],
      "query": "restaurant"
    }
  }
}
 
```

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="next-steps"></a>Sonraki adımlar
- [Yerel iş arama hızlı başlangıç](quickstarts/local-quickstart.md)
- [Yerel iş arama Java hızlı başlangıç](quickstarts/local-search-java-quickstart.md)
- [Yerel iş arama düğümü hızlı başlangıç](quickstarts/local-search-node-quickstart.md)
- [Yerel iş arama Python hızlı başlangıç](quickstarts/local-search-python-quickstart.md)
