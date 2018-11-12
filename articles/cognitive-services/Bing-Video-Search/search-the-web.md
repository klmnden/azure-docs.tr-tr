---
title: Bing Video Arama nedir?
titlesuffix: Azure Cognitive Services
description: Web’de video aramak için Bing Video Arama API'sinin nasıl kullanılacağını gösterir.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: overview
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: e48a0a056628e0c863330de792f8edfaa48aae34
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261868"
---
# <a name="what-is-bing-video-search"></a>Bing Video Arama nedir?

Bing Video Arama API’si, [Bing Videolar](https://www.bing.com/videos) ile benzer (tam olarak aynı değil) bir deneyim sağlar. Bing Video Arama API’si, Bing’e bir arama sorgusu göndermenize ve ilgili videoların bir listesini geri almanıza olanak tanır.

Kullanıcının arama sorgusuyla ilgili videoları bulmak için yalnızca video içeren bir arama sonuçları sayfası oluşturuyorsanız daha genel [Bing Web Araması API'si](../bing-web-search/search-the-web.md) yerine bu API'yi çağırın. Videoların yanı sıra web sayfaları, haberler ve görüntüler gibi diğer içerik türlerini de istiyorsanız Bing Web Araması API'sini çağırın.

## <a name="suggesting--using-search-terms"></a>Arama terimlerini önerme ve kullanma

Kullanıcıların arama terimlerini gireceği bir arama kutusu sağlıyorsanız deneyimi geliştirmek için [Bing Otomatik Öneri API'sini](../bing-autosuggest/get-suggested-search-terms.md) kullanın. API, kullanıcı yazarken kısmi arama terimlerine dayalı önerilen sorgu dizelerini yönetin.

Kullanıcı arama terimini girdikten sonra [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query) sorgu parametresini ayarlamadan önce terimi URL ile kodlayın. Örneğin kullanıcı *sailing dinghies* terimini girerse `q` öğesini `sailing+dinghies` veya `sailing%20dinghies` olarak ayarlayın.

## <a name="getting-videos"></a>Videoları alma

Web'den kullanıcının arama terimiyle ilgili videoları almak için aşağıdaki GET isteğini gönderin:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Tüm isteklerin bir sunucudan yapılması gerekir.

Bing API'lerinden birini ilk kez çağırıyorsanız istemci kimliği üst bilgisini eklemeyin. İstemci kimliğini yalnızca önceden bir Bing API'sini çağırdıysanız ve Bing, kullanıcı ve cihaz birleşimi için bir istemci kimliği döndürdüyse dahil edin.

Belirli bir etki alanındaki videoları almak için [site:](https://msdn.microsoft.com/library/ff795613.aspx) dize işlecini kullanın.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

Yanıt, Bing'in sorguyla ilişkili olduğunu düşündüğü videoları bir listesini içeren [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) yanıtı içerir. Listedeki her [Video](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video) nesneyi videonun URL’sini, süresini, boyutlarını, kodlama biçimini ve diğer özniteliklerini içerir. Video nesnesi aynı zamanda video küçük resminin URL'sini ve küçük resmin boyutlarını içerir.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545...",
    "totalEstimatedMatches" : 1000,
    "value" : [
        {
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear when...",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7...",
            "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.DYW...",
            "datePublished" : "2014-03-04T11:51:53",
            "publisher" : [
                {
                    "name" : "Fabrikam"
                }
            ],
            "creator" : 
            {
                "name" : "Marcus Appel"
            },
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjZ--g",
            "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D569...",
            "encodingFormat" : "h264",
            "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjZ--g",
            "width" : 1280,
            "height" : 720,
            "duration" : "PT2M47S",
            "motionThumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OM.Y62...",
            "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"https:...><\/iframe>",
            "allowHttpsEmbed" : true,
            "viewCount" : 8743,
            "thumbnail" : 
            {
                "width" : 300,
                "height" : 168
            },
            "videoId" : "6DB795E11A6E3CBAAD636DB795E113CBAAD63",
            "allowMobileEmbed" : true,
            "isSuperfresh" : false
        },
        ...
    ],
    "queryExpansions" : [...],
    "nextOffsetAddCount" : 0,
    "pivotSuggestions" : [...]
}
```

Videoların küçük resimlerinden oluşan bir kolaj veya küçük resimlerin alt kümesini görüntüleyebilirsiniz. Alt küme görüntülerseniz kullanıcıya kalan videoları görüntüleme seçeneği sunun. Videoları yanıtta iletilen sırayla görüntülemeniz gerekir. Küçük resmi yeniden boyutlandırma hakkında bilgi için bkz. [Küçük Resimleri Yeniden Boyutlandırma ve Kırpma](./resize-and-crop-thumbnails.md). 

Kullanıcı küçük resmin üzerine geldiğinde videonun küçük resim bir sürümünü oynatmak için [motionThumbnailUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-motionthumbnailurl) kullanabilirsiniz. Hareket küçük resmini görüntülediğinizde öznitelik belirlediğinizden emin olun.

<!-- Removing until the images can be sanitized.
![Motion thumbnail of a video](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-video-motion-thumbnail.PNG)
-->

Kullanıcı küçük resme tıklarsa, aşağıdaki video görüntüleme seçenekleri sunulur:

- Videoyu konak web sitesinde (örneğin YouTube) görüntülemek için [hostPageUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-hostpageurl) kullanın
- Videoyu Bing video tarayıcısında görüntülemek için [webSearchUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-websearchurl) kullanın
- Videoyu kendi deneyiminize eklemek için [embdedHtml](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-embedhtml) kullanın 

Videoyu oynatırken yayımcı ve oluşturucu özniteliklerini kullandığınızdan emin olun.

Video hakkında öngörüler almak için [videoId](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-videoid) kullanma hakkında daha fazla bilgi için bkz. [Video Öngörüleri](./video-insights.md).

## <a name="filtering-videos"></a>Videoları filtreleme

Video Arama API'si varsayılan olarak sorguyla ilgili tüm videoları döndürür. Yalnızca ücretsiz videolar veya uzunluğu beş dakikayı aşmayan videolar istiyorsanız, aşağıdaki filtre sorgu parametrelerini kullanırsınız:

- [pricing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#pricing)&mdash;Videoları fiyatlandırmaya göre filtreleyin (örneğin, ücretsiz videolar veya ücret ödemeniz gereken videolar)
- [resolution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#resolution)&mdash;Videoları çözünürlüğe göre filtrele (örneğin, 720p veya daha yüksek çözünürlüğe sahip videolar)
- [videoLength](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videolength)&mdash;Videoları video uzunluğuna göre filtreleyin (örneğin, beş dakikadan kısa videolar)
- [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#freshness)&mdash;Videoları yaşa göre filtreleyin (Bing'in geçen hafta keşfettiği videolar gibi)

Belirli bir etki alanındaki videoları almak için sorgu dizesine [site:](https://msdn.microsoft.com/library/ff795613.aspx) dize işlecini ekleyin.

> [!NOTE]
> Sorguya bağlı olarak `site:` sorgu işlecini kullanmanız halinde [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#safesearch) ayarından bağımsız olarak yanıtta yetişkinlere yönelik içerik bulunabilir. `site:` işlecini yalnızca sitenin içeriği hakkında bilgi sahibiyseniz ve senaryonuz, yetişkinlere yönelik içeriğin mevcut olma ihtimalini destekliyorsa kullanın.

Aşağıdaki örnekte ContosoSailing.com adresinden 720p veya daha iyi çözünürlüğe sahip ve Bing’in geçen ay içinde bulduğu ücretsiz videoların nasıl alındığı gösterilmektedir.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies+site:contososailing.com&pricing=free&freshness=month&resolution=720p&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

## <a name="expanding-the-query"></a>Sorguyu genişletme

Bing özgün aramayı daraltmak için sorguyu genişletebiliyorsa [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) nesnesi `queryExpansions` alanını içerir. Örneğin, sorgu *Boşluk Temizleme* ise, genişletilmiş sorgular şunlar olabilir: Boşluk Temizleme **Araçları**, **Sıfırdan** Boşluk Temizleme, Boşluk Temizleme **Makinesi** ve **Kolay** Boşluk Temizleme.

Aşağıdaki örnekte *Boşluk Temizleme* için genişletilmiş sorgular gösterilmektedir.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC5...",
    "totalEstimatedMatches" : 1000,
    "value" : [...],
    "nextOffsetAddCount" : 4,
    "queryExpansions" : [
        {
            "text" : "Gutter Cleaning Tools",
            "displayText" : "Tools",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FB....",
            "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5...",
            "thumbnail" : {
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Gutter..."
            }
        },
        ...
    ]
    "pivotSuggestions" : [...],
}
```

`queryExpansions` alanı [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query_obj) nesnelerinin listesini içerir. `text` alanı genişletilmiş sorguyu, `displayText` alanı ise genişletme terimini içerir. Metin ve küçük resim alanlarını kullanarak genişletilmiş sorgu dizelerini kullanıcıya gösterebilir ve kullanıcının bu genişletilmiş sorgu dizelerinden seçim yapmasını sağlayabilirsiniz. `webSearchUrl` URL veya `searchLink` URL kullanarak küçük resmi ve metni tıklanabilir hale getirebilirsiniz. `webSearchUrl` kullanarak kullanıcıyı Bing arama sonuçlarına gönderebilir veya `searchLink` ile kendi sonuç sayfanızı sunabilirsiniz.

## <a name="pivoting-the-query"></a>Sorguyu özetleme

Bing özgün arama sorgusunu parçalara ayırabiliyorsa [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) nesnesi `pivotSuggestions` alanını içerir. Örneğin, özgün sorgu *Boşluk Temizleme* ise Bing bunu *Boşluk* ve *Temizleme* olmak üzere ikiye bölebilir.

Aşağıdaki örnekte *Boşluk Temizleme* için öneri özetleri gösterilmektedir.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC...",
    "totalEstimatedMatches" : 1000,
    "value" : [...],
    "nextOffsetAddCount" : 0,
    "queryExpansions" : [...],
    "pivotSuggestions" : [
        {
            "pivot" : "cleaning",
            "suggestions" : [
                {
                    "text" : "Gutter Repair",
                    "displayText" : "Repair",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5\/videos...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=Gutter..."
                    }
                },
                ...
            ]
        },
        {
            "pivot" : "gutters",
            "suggestions" : [
                {
                    "text" : "Window Cleaning",
                    "displayText" : "Window",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC59...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=Window..."
                    }
                },
                ...
            ]
        }
    ]
}
```

Her bir özet terim için gelen yanıtta önerilen sorguları içeren [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query_obj) nesnelerinin listesi yer alır. `text` alanı önerilen sorguyu, `displayText` alanı ise özgün sorguda özetin yerine kullanılan terimi içerir. Örneğin, Pencere Temizleme.

`text` ve `thumbnail` alanlarını kullanarak genişletilmiş sorgu dizelerini kullanıcıya gösterebilir ve kullanıcının bu genişletilmiş sorgu dizelerinden seçim yapmasını sağlayabilirsiniz. `webSearchUrl` URL veya `searchLink` URL kullanarak küçük resmi ve metni tıklanabilir hale getirebilirsiniz. `webSearchUrl` kullanarak kullanıcıyı Bing arama sonuçlarına gönderebilir veya `searchLink` ile kendi sonuç sayfanızı sunabilirsiniz.

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi hızlı bir şekilde başlatmak için bkz. [İlk İsteğinizi Yapma](./quick-start.md).

Abonelik anahtarınızı almak için bkz. [Abonelik Anahtarları](https://azure.microsoft.com/try/cognitive-services/?api=bing-video-search-api).

[Bing Video Arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) başvurusunu inceleyin. Başvuruda arama sonuçlarını istemek için kullanabileceğiniz uç noktaların, üst bilgilerin ve sorgu parametrelerinin listesi yer alır. Ayrıca yanıt nesnelerinin tanımları da bulunur. 

Arama kutusu kullanıcı deneyiminizi geliştirmek için bkz. [Bing Otomatik Öneri API'si](../bing-autosuggest/get-suggested-search-terms.md). Kullanıcı sorgu terimini girerken bu API'yi çağırarak başkaları tarafından kullanılan ilgili sorgu terimlerini alabilirsiniz.

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./useanddisplayrequirements.md)'ni okumayı unutmayın.

Video Arama API'sini çağırdığınızda Bing, sonuç listesini döndürür. Bu liste sorguyla ilgili tüm sonuçların alt kümesidir. Yanıtın `totalEstimatedMatches` alanı, görüntülenebilecek tahmini video sayısını içerir. Kalan videoları sayfalama hakkında ayrıntılı bilgi için bkz. [Videoları Sayfalama](./paging-videos.md).

Bir video hakkında içgörüler elde etmenin ayrıntıları için bkz. [Video İçgörüleri](./video-insights.md).

Popüler videolar hakkında ayrıntılar için, bkz. [Popüler Videolar](./trending-videos.md).