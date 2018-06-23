---
title: Haber arama API'si hızlı başlangıç | Microsoft Docs
description: Bing Haberler arama API'si ile çalışmaya başlamak gösterilmiştir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 9CF6EAF3-42D8-4321-983C-4AC3896E8E03
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 2aa1b3de07bd61eebfc1d410cda76c32fc7c958e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354868"
---
# <a name="your-first-news-search-query"></a>İlk haber arama sorgunuzu

İlk aramanız yapabilmeniz için önce bir Bilişsel hizmetler abonelik anahtarı almanız gerekir. Bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api).

Yalnızca haber arama sonuçları almak için aşağıdaki uç noktaya bir GET isteği gönder:

```http
https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

İstek HTTPS protokolünü kullanmanız gerekir.

Tüm istekleri bir sunucusundan kaynaklanan öneririz. Bir istemci uygulaması bir parçası olarak anahtar dağıtma erişmek kötü amaçlı bir üçüncü taraf için daha fazla fırsatı sağlar. Ayrıca, bir sunucudan çağrıları yapma tek bir yükseltme noktası API gelecek sürümlerinde sağlar.

İstek belirtmelisiniz [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query) kullanıcının arama terimi içeren sorgu parametresi. İsteğe bağlı olsa da, istek de belirtmeniz gerekir [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#mkt) alınması için sonuçları istediğiniz Pazar tanımlayan sorgu parametresi. Listesini isteğe bağlı parametreleri gibi sorgu `freshness` ve `textDecorations`, bkz: [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters). Tüm sorgu parametre değerleri URL kodlanmış olmalıdır.

İstek belirtmelisiniz [Apim abonelik anahtar Ocp](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#subscriptionkey) üstbilgi. İsteğe bağlı olsa da, aşağıdaki üst bilgiler de belirtmeniz önerilir:

- [Kullanıcı Aracısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#useragent)
- [X MSEdge ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#clientid)
- [X arama ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#clientip)
- [X arama konumu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#location)

İstemci IP ve konum üstbilgileri konumu kullanan içerik döndürmek için önemlidir.

Tüm istek ve yanıt üstbilgileri listesi için bkz: [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#headers).

## <a name="the-request"></a>İstek

Tüm üstbilgiler ve önerilen sorgu parametreleri içeren bir haber isteği gösterir. Herhangi bir Bing API'leri çağırma ilk kez ise, istemci kimliği üstbilgisi eklemeyin. Yalnızca istemci kimliği, daha önce Bing API'si çağırdıktan ve kullanıcı ve aygıt bileşimi için bir istemci kimliği Bing döndürülen içerir.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

## <a name="the-response"></a>Yanıt

Önceki istek yanıtı gösterir. Bu örnek ayrıca Bing özgü yanıt üstbilgilerini gösterir.

```http
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "News",
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",
    "totalEstimatedMatches" : 88400,
    "value" : [{
        "name" : "Sailing Vies for Four Trophies",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.9C23AA5...",
                "width" : 650,
                "height" : 341
            }
        },
        "description" : "Sailing College Rankings, presented by Zim...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "contoso.com"
        }],
        "datePublished" : "2017-04-14T15:28:00"
    },
    {
        "name" : "Reunion at Fabrikam Lakes Sailing Club, celebrates 50 years...",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891F...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.38210...",
                "width" : 650,
                "height" : 366
            }
        },
        "description" : "The reunion on April 29, at Fabrikam Lakes Sailing...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "Contoso"
        }],
        "datePublished" : "2017-04-14T13:08:00"
    },
    {
        "name" : "Sailing Club to host Dinghy Sailing World...",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891FE99...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.364AB41...",
                "width" : 448,
                "height" : 300
            }
        },
        "description" : "The sailing club that trained Olympian Ben...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "Contoso"
        }],
        "datePublished" : "2017-04-04T11:02:00",
        "category" : "Sports"
    },
    {
        "name" : "A 24-Carat Dinghy",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891F...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.6CC2...",
                "width" : 700,
                "height" : 466
            }
        },
        "description" : "“Hard dinghies are for purists, the kind of people who...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "contoso.com"
        }],
        "datePublished" : "2017-04-03T12:14:00",
        "category" : "Politics"
    }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Out API deneyin. Git [haber arama API sınama Konsolu'nu](https://dev.cognitive.microsoft.com/docs/services/56b43f72cf5ff8098cef380a/operations/56f02400dbe2d91900c68553).

Yanıt nesneleri kullanma hakkında daha fazla bilgi için bkz [Bing Haberler arama nedir?](./search-the-web.md). Ayrıca, aşağıdaki genel eylemler hakkında daha fazla bilgi bulabilirsiniz:

- [Günümüzün üst haber alma](./search-the-web.md#getting-todays-top-news)
- [Kategoriye göre haber alma](./search-the-web.md#getting-news-by-category)
- [Oluşturan eğilim haber alma](./search-the-web.md#getting-trending-news)
