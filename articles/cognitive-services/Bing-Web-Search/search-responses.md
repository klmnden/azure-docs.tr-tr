---
title: Arama yanıtları - Bing Web araması API'si
titleSuffix: Azure Cognitive Services
description: Yanıt türleri ve Bing Web araması API'si tarafından alınan yanıtları hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: conceptual
ms.date: 8/13/2018
ms.author: aahi
ms.openlocfilehash: c7626f08dcfad76724a4d1f5d88fbd10c78c62c6
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164793"
---
# <a name="bing-web-search-api-response-structure-and-answer-types"></a>Bing Web araması API'si yanıt yapısı ve yanıt türleri  

Bing Web araması arama isteği gönderdiğinizde, döndürür bir [ `SearchResponse` ](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) yanıt gövdesinde bir nesne. Bing sorgu ile ilgili belirlendi her yanıt için bir alan nesne içerir. Bu örnekte, Bing tüm yanıtları döndürdüyse bir yanıt nesnesi gösterilmektedir:

```json
{
    "_type": "SearchResponse",
    "queryContext": {...},
    "webPages": {...},
    "images": {...},
    "relatedSearches": {...},
    "videos": {...},
    "news": {...},
    "spellSuggestion": {...},
    "computation": {...},
    "timeZone": {...},
    "rankingResponse": {...}
}, ...
```

Genellikle, Bing Web araması yanıtlar bir alt kümesi döndürür. Örneğin, sorgu terimine olduysa *Yelkenli dinghies*, yanıt içerebilir `webPages`, `images`, ve `rankingResponse`. Kullandığınız sürece [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#responsefilter) Web sayfalarını filtrelemek için yanıt her zaman içeriyor `webpages` ve `rankingResponse` yanıtlar.

## <a name="webpages-answer"></a>Web sayfası yanıt

[Web sayfalarını](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) yanıt, Bing Web araması sorgu ile ilgili olan belirlenen Web sayfalarına bağlantılar listesini içerir. Her [web sayfası](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webpage) içinde liste sayfanın adı, url, görüntü URL'sini içerir, içerik ve Bing tarih kısa açıklamasını içeriği bulunamadı.

```json
{
    "id": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
    "name": "Dinghy sailing",
    "url": "https:\/\/www.bing.com\/cr?IG=3A43CA5...",
    "displayUrl": "https:\/\/en.contoso.com\/wiki\/Dinghy_sailing",
    "snippet": "Dinghy sailing is the activity of sailing small boats...",
    "dateLastCrawled": "2017-04-05T16:25:00"
}, ...
```

Kullanım `name` ve `url` kullanıcının Web sayfasına alan köprü oluşturmak için.

<!-- Remove until this can be replaced with a sanitized version.
The following shows an example of how you might display the webpage in a search results page.

![Rendered webpage example](./media/cognitive-services-bing-web-api/bing-rendered-webpage-example.PNG)
-->

## <a name="images-answer"></a>Görüntüleri yanıt

[Görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) yanıtı Bing sorgu ile ilgili olduğunu düşündük görüntülerin bir listesini içerir. Her [görüntü](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) listesinde görüntü, boyutunu, boyutlar ve kodlama, biçim URL'sini içerir. Görüntü nesnesi aynı zamanda görüntü küçük resminin URL'sini ve küçük resmin boyutlarını içerir.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=3A43CA5CA64...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP....",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/upload.contoso.com\/sailing\/...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=3A43CA5CA6464....",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "http:\/\/en.contoso.com\/wiki\/File...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    }
}, ...
```

Kullanıcının cihaza bağlı olarak kullanıcının kalan resimleri görüntülemek bir seçenek ile küçük bir alt kümesi genelde görüntülenecekti.

<!-- Remove until this can be replaced with a sanitized version.
![List of thumbnail images](./media/cognitive-services-bing-web-api/bing-web-image-thumbnails.PNG)
-->

Küçük resimlerin, kullanıcı imleci üzerine getirdiğinde büyütülmesini de sağlayabilirsiniz. Büyütmeniz halinde görüntüye bağlam sağlamayı unutmayın. Örneğin konaktan ayıklama tarafından `hostPageDisplayUrl` ve aşağıdaki resim görüntüleme. Küçük resmi yeniden boyutlandırma hakkında bilgi için bkz. [Küçük Resimleri Yeniden Boyutlandırma ve Kırpma](./resize-and-crop-thumbnails.md).

<!-- Remove until this can be replaced with a sanitized version.
![Expanded view of thumbnail image](./media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Küçük resim kullanıcı tıklar kullanırsanız `webSearchUrl` görüntülerin bir desende içeren Bing'in arama sonuçları sayfasını görüntüler için kullanıcıya yapılacak.

Görüntü yanıt ve görüntüleri hakkında daha fazla ayrıntı için bkz: [resim arama API'si](../bing-image-search/search-the-web.md).

## <a name="related-searches-answer"></a>İlgili aramalar yanıt

[RelatedSearches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse-relatedsearches) yanıt diğer kullanıcılar tarafından yapılan en popüler ilgili sorgular bir listesini içerir. Her [sorgu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query_obj) listesinde bir sorgu dizesi içeren (`text`), isabet vurgulama karakter içeren bir sorgu dizesi (`displayText`) ve bir URL (`webSearchUrl`) için bu sorgu için Bing'in arama sonuçları sayfası.

```json
{
    "text": "dinghy racing teams",
    "displayText": "dinghy racing teams",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=96C4CF214A0..."
}, ...
```

Kullanım `displayText` sorgu dizesi ve `webSearchUrl` URL Bing arama'nın kullanıcının gerçekleştirdiği köprü oluşturmak için sonuçları ilgili sorgu sayfası. Ayrıca kullanabileceğinizi `text` sorgu kendi Web arama API'si sorgu dizesi ve sonuçları kendiniz görüntüleyin.

Vurgu işaretlerinin içinde nasıl ele alınacağını öğrenmek için `displayText`, bkz: [isabet vurgulama](./hit-highlighting.md).

Aşağıdaki örnek ilgili sorgular kullanımı Bing.com içinde gösterir.

![İlgili aramalar örnek bing'de](./media/cognitive-services-bing-web-api/bing-web-rendered-relatedsearches.GIF)

## <a name="videos-answer"></a>Videoları yanıt

[Videoları](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) yanıt Bing sorgu ile ilgili olduğunu düşündük videoların bir listesini içerir. Her [video](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video) listesinde video, süresi, boyutlar ve kodlama, biçim URL'sini içerir. Video nesnesi aynı zamanda video küçük resminin URL'sini ve küçük resmin boyutlarını içerir.

```json
{
    "name": "Sailing dinghy",
    "description": "Northwind Traders is a 12 foot gunter rigged...",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=1CAE739681D84...",
    "thumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OVP.wsKiL...",
    "datePublished": "2013-11-06T01:56:28",
    "publisher": [{
        "name": "Fabrikam"
    }],
    "contentUrl": "https:\/\/www.fabrikam.com\/watch?v=MrVBWZpJjX",
    "hostPageUrl": "https:\/\/www.bing.com\/cr?IG=1CAE739681D8400DB...",
    "encodingFormat": "mp4",
    "hostPageDisplayUrl": "https:\/\/www.fabrikam.com\/watch?v=MrBWZpJjXo",
    "width": 1280,
    "height": 720,
    "duration": "PT3M47S",
    "motionThumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OM.oa...",
    "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"http:\/\/www....><\/iframe>",
    "allowHttpsEmbed": true,
    "viewCount": 19089,
    "thumbnail": {
        "width": 300,
        "height": 168
    },
    "allowMobileEmbed": true,
    "isSuperfresh": false
}, ...
```

Kullanıcının cihaza bağlı olarak, genellikle bir alt kümesini kullanıcının diğer videoları bir seçenek ile videoları görüntülenecekti. Bir küçük resim video uzunluğu, videonun görüntüleyecektir açıklaması (ad) ve atıf (yayımcı).

<!-- Remove until this can be replaced with a sanitized version.
![List of video thumbnails](./media/cognitive-services-bing-web-api/bing-web-video-thumbnails.PNG)
-->

Küçük resmin üzerinde kullanıcı gezinen olarak kullanabileceğiniz `motionThumbnailUrl` videonun küçük bir sürüm yürütülecek. Hareket küçük resmini görüntülediğinizde öznitelik belirlediğinizden emin olun.

<!-- Remove until this can be replaced with a sanitized version.
![Motion thumbnail of a video](./media/cognitive-services-bing-web-api/bing-web-video-motion-thumbnail.PNG)
-->

Kullanıcı küçük resme tıklarsa, aşağıdaki video görüntüleme seçenekleri sunulur:

- Kullanım `hostPageUrl` konak Web sitesinde (örneğin, YouTube) videoyu görüntülemek için
- Kullanım `webSearchUrl` Bing video tarayıcıda videoyu görüntülemek için
- Kullanım `embedHtml` kendi deneyimi video eklemek için

Videolar ve video yanıt hakkında daha fazla ayrıntı için bkz: [Video arama API'si](../bing-video-search/search-the-web.md).

## <a name="news-answer"></a>Haber yanıt

[Haber](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) yanıt Bing sorgu ile ilgili olduğunu düşündük haber makaleleri bir listesini içerir. Listedeki her [haber makalesi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle), makalenin adı, açıklaması ve konağın web sitesindeki makalenin URL’sini içerir. Makale bir görüntü içeriyorsa, nesne görüntünün küçük resmini içerir.

```json
{
    "name": "WC Sailing Qualifies for America Trophy with...",
    "url": "http:\/\/www.bing.com\/cr?IG=3445EEF15DAF4FFFBF7...",
    "image": {
        "contentUrl": "http:\/\/www.contoso.com\/sports\/sail...",
        "thumbnail": {
            "contentUrl": "https:\/\/www.bing.com\/th?id=ON.1...",
            "width": 400,
            "height": 272
        }
    },
    "description": "The WC sailing team qualified for a...",
    "provider": [{
        "_type": "Organization",
        "name": "contoso.com"
    }],
    "datePublished": "2017-04-16T21:56:00"
}, ...
```

Kullanıcının cihaza bağlı olarak, haber makaleleri kullanıcının kalan makaleleri görüntülemek bir seçenek kümesini görüntüleyebilir. Kullanıcıyı konağın sitesindeki haber makalesine götüren bir köprü bağlantı oluşturmak için `name` ve `url` kullanın. Makale bir görüntü içerdiğinden, görüntü tıklanabilir kullanarak olun `url`. Makaleyi ilişkilendirmek için `provider` kullandığınızdan emin olun.

<!-- Remove until this can be replaced with a sanitized version.
The following shows an example of how you might display articles in a search results page.

![List of news articles](./media/cognitive-services-bing-web-api/bing-web-news-list.PNG)
-->

Haber makaleleri ve haber yanıt hakkında daha fazla ayrıntı için bkz: [haber arama API'si](../bing-news-search/search-the-web.md).

## <a name="computation-answer"></a>Hesaplama yanıt

Kullanıcı, bir matematik ifadesindeki veya birim dönüştürme sorgusunu girerse, yanıt içerebilir bir [hesaplama](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#computation) yanıt. `computation` Yanıt normalleştirilmiş ifade ve sonucunu içerir.

Bir birim dönüştürme sorgu başka bir birimi dönüştüren bir sorgudur. Örneğin, *10 metre olarak kaç ayak?* veya *1/4 cup'ta kaç çorba kaşığına?*

Aşağıdaki gösterildiği `computation` için yanıt *10 metre olarak kaç ayak?*

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "10 meters",
    "value": "32.808399 feet"
}, ...
```

Aşağıdaki örnekler matematik sorguları ve bunlara karşılık gelen gösterir `computation` yanıtlar.

```
Query: (5+3)(10/2)+8
Encoded query: %285%2B3%29%2810%2F2%29%2B8
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "((5+3)*(10\/2))+8",
    "value": "48"
}
```

```
Query: sqrt(4^2+8^2)
Encoded query: sqrt%284^2%2B8^2%29
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "sqrt((4^2)+(8^2))",
    "value": "8.94427191"
}
```

```
Query: 30 6/8 - 18 8/16
Encoded query: 30%206%2F8%20-%2018%208%2F16
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#WolframAlpha",
    "expression": "30 6\/8-18 8\/16",
    "value": "12.25"
}
```

```
Query: 8^2+11^2-2*8*11*cos(37)
Encoded query: 8^2%2B11^2-2*8*11*cos%2837%29
```

```json
"computation": {
        "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
        "expression": "(8^2)+(11^2)-(2*8*11*cos(37))",
        "value": "44.4401502"
}
```

Matematik ifadesi şu simgeleri içerebilir:

|Sembol|Açıklama|
|------------|-----------------|
|+|Ekleme|
|-|Çıkarma|
|/|Bölme|
|*|Çarpma|
|^|Güç|
|!|Faktöriyelini|
|.|Ondalık|
|()|Öncelik gruplandırma|
|[]|İşlev|

Matematik ifadesi aşağıdaki sabitler içerebilir:

|Sembol|Açıklama|
|------------|-----------------|
|Pi|olarak 3,14159...|
|Derece|Derece|
|Ben|Sanal numarası|
|E|e 2.71828...|
|GoldenRatio|Altın oranını 1.61803...|

Matematik ifadesi aşağıdaki işlevleri içerebilir:

|Sembol|Açıklama|
|------------|-----------------|
|Sqrt|Karekök|
|Sin [x], Cos [x], Bronz [x]<br />CSC [x], [x] sn Cot [x]|Trigonometrik işlevler (ile radyan cinsinden bağımsız değişkenler)|
|ArcSin [x], [x] ArcCos ArcTan [x]<br />ArcCsc [x], [x] ArcSec ArcCot [x]|Ters trigonometrik işlevler (radyan cinsinden sonuçları vermiş)|
|Exp [x] E ^ x|Üstel işlevi|
|Günlük [x]|Doğal logaritma|
|SİNH [x], [x] Cosh Tanh [x]<br />Csch [x], [x] Sech Coth [x]|Hiperbolik İşlevler|
|ArcSinh [x], [x] ArcCosh ArcTanh [x]<br />ArcCsch [x], [x] ArcSech ArcCoth [x]|Ters hiperbolik İşlevler|

Değişkenleri (örneğin, 4 x + x değişken olduğu 6 = 18,) içeren Matematik ifadeler desteklenmez.

## <a name="timezone-answer"></a>Saat dilimi yanıt

Kullanıcının bir tarih ya da sorgu girerse, yanıt içerebilir bir [saat dilimi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#timezone) yanıt. Bu yanıt, örtük veya açık sorguları destekler. Örtük bir sorgu gibi *olduğu ne zaman?*, kullanıcının konumuna göre yerel saati döndürür. Açık bir sorgu gibi *Seattle olduğu ne zaman?*, Seattle, WA için yerel saati döndürür.

`timeZone` Yanıt konumu, geçerli UTC tarihi ve saati adı belirtilen konumda sağlar ve UTC'ye uzaklık. Konum sınırları içinde birden fazla saat dilimlerini ise, geçerli UTC tarih ve saat, tüm saat dilimlerini sınırları içinde yanıt içerir. Örneğin, Florida durumu içinde iki saat dilimlerini kaldığından, yanıt yerel tarih ve saat, her iki saat dilimlerini içerir.  

Sorgu bir eyalet veya ülke süresini isterse, Bing konumun coğrafi sınırları içinde birincil Şehir belirler ve bunu döndürür `primaryCityTime` alan. Sınır birden fazla saat dilimlerini içeriyorsa, kalan saat dilimlerini döndürülür `otherCityTimes` alan.

Aşağıdaki örnekte gösterildiği döndüren sorgular `timeZone` yanıt.

```
Query: What time is it?

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Redmond, Washington, United States",
        "time": "2015-10-27T08:38:12.1189231Z",
        "utcOffset": "UTC-7"
    }
}

Query: What time is it in the Pacific time zone?

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Pacific Time Zone",
        "time": "2015-10-23T12:33:19.0728146Z",
        "utcOffset": "UTC-7"
    }
}

Query: Time in Florida?

"timeZone": {
        "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
        "primaryCityTime": {
            "location": "Tallahassee, Florida, United States",
            "time": "2015-10-23T13:04:56.6774389Z",
            "utcOffset": "UTC-4"
        },
        "otherCityTimes": [{
            "location": "Pensacola",
            "time": "2015-10-23T12:04:56.6664294Z",
            "utcOffset": "UTC-5"
        }]
}

Query: What time is it in the U.S.

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Washington, D.C., United States",
        "time": "2015-10-23T15:27:59.8892745Z",
        "utcOffset": "UTC-4"
    },
    "otherCityTimes": [{
        "location": "Honolulu",
        "time": "2015-10-23T09:27:59.8892745Z",
        "utcOffset": "UTC-10"
    },
    {
        "location": "Anchorage",
        "time": "2015-10-23T11:27:59.8892745Z",
        "utcOffset": "UTC-8"
    },
    {
        "location": "Phoenix",
        "time": "2015-10-23T12:27:59.8892745Z",
        "utcOffset": "UTC-7"
    },
    {
        "location": "Los Angeles",
        "time": "2015-10-23T12:27:59.8942788Z",
        "utcOffset": "UTC-7"
    },
    {
        "location": "Denver",
        "time": "2015-10-23T13:27:59.8812681Z",
        "utcOffset": "UTC-6"
    },
    {
        "location": "Chicago",
        "time": "2015-10-23T14:27:59.8892745Z",
        "utcOffset": "UTC-5"
    }]
}
```

## <a name="spellsuggestion-answer"></a>SpellSuggestion yanıt

Bing belirlerse, kullanıcının farklı bir şey için aranacak hedeflenen, yanıt içeren bir [SpellSuggestions](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#spellsuggestions) nesne. Örneğin, kullanıcı *carlos kalem*, Bing kullanıcının büyük olasılıkla Carlos Pena için bunun yerine arama geçmesini belirlemek (başkaları tarafından son aramaları tabanlı *carlos kalem*). Bir örnek yazım yanıt aşağıda gösterilmiştir.

```json
"spellSuggestions": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#SpellSuggestions",
    "value": [{
        "text": "carlos pena",
        "displayText": "carlos pena"
    }]
}, ...
```

Bing yazım önerisi nasıl kullandığını gösterir.

![Bing yazım önerisi örneği](./media/cognitive-services-bing-web-api/bing-web-spellingsuggestion.GIF)  

## <a name="next-steps"></a>Sonraki adımlar  

* Gözden geçirme [istek azaltma](throttling-requests.md) belgeleri.  

## <a name="see-also"></a>Ayrıca bkz.  

* [Bing Web araması API'si başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
