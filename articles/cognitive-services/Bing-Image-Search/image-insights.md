---
title: Bing resim arama API'si - resim öngörüleri alın
titleSuffix: Azure Cognitive Services
description: Bing resim arama API'si, bir görüntü ile ilgili daha fazla bilgi almak için kullanmayı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 0BCD936E-D4C0-472D-AE40-F4B2AB6912D5
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: article
ms.date: 03/04/2019
ms.author: scottwhi
ms.openlocfilehash: 8521566087690523359b753b800268e75437a257
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66384264"
---
# <a name="get-image-insights-with-the-bing-image-search-api"></a>Bing resim arama API'si ile resim Öngörüler elde edin

> [!IMPORTANT]
> Resim öngörüleri almak için/resimler/Ayrıntılar endpoint kullanmak yerine, kullanmalısınız [görsel arama](../bing-visual-search/overview.md) çünkü daha kapsamlı içgörüler sağlar.


Her görüntünün görüntü hakkında bilgi almak için kullanabileceğiniz bir ınsights belirteci içerir. Örneğin, burada görüntüde verilen ürün satın alabilirsiniz ilgili görüntü, görüntü içeren web sayfaları koleksiyonunu veya tüccarların listesini alabilirsiniz.  

Bir görüntü ile ilgili öngörüleri almak için görüntü yakalama [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image-imageinsightstoken) yanıt belirteç.

```json
"value" : [{
        . . .
        "name":"sailing dinghy.jpg",
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "insightsSourcesSummary" : {
            "shoppingSourcesCount" : 9,
            "recipeSourcesCount" : 0
        },
        . . .
}],
```

Ardından, görüntü ayrıntılarını uç noktasını çağırmak ve ayarlama [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) sorgu parametresi için belirteci `imageInsightsToken`.  

Almak istediğiniz öngörüleri belirtmek için ayarlayın `modules` sorgu parametresi. Tüm öngörüleri almak için ayarlanmış `modules` için `All`. Yalnızca başlığı ve koleksiyon öngörüleri almak için ayarlanmış `modules` için `Caption%2CCollection`. Olası ınsights tam bir listesi için bkz. [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested). Tüm ınsights, tüm görüntüler için kullanılabilir. Yanıt, istenen tüm ınsights varsa içeriyor.

Aşağıdaki örnek, bir önceki resimde için tüm kullanılabilir ınsights ister.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_D6426898706EC7...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

## <a name="getting-insights-of-a-known-image"></a>Bilinen bir görüntünün öngörü alma

Insights'ın almak için kullanmak istediğiniz resmin URL'si varsa [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imgurl) sorgu parametresi yerine [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) parametresini kullanarak görüntüyü belirtin. Veya resim dosyası varsa, bir POST isteğinin gövdesinden ikili görüntünün gönderebilir. Bir POST isteği kullanırsanız `Content-Type` üstbilgi ayarlanmalıdır `multipart/data-form`. Her iki seçenekte de görüntüsünün boyutu 1 MB aşamaz.  

Görüntünün bir URL'niz varsa, aşağıdaki örnekte ınsights görüntünün nasıl gösterir.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&imgUrl=https%3A%2F%2Fwww.mydomain.com%2Fimages%2Fsunflower.png&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

## <a name="getting-all-image-insights"></a>Tüm görüntü öngörü alma  

Görüntünün tüm ınsights istemek için ayarlanmış [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) sorgu parametresi için `All`. İlgili aramalar almak için istek kullanıcının sorgu dizesi içermelidir. Bu örnek gösterir kullanarak [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) görüntüyü belirtmek için.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_68364D764J...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Üst düzey nesnedir bir [ImageInsightsResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse) yerine Nesne bir [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) nesne.  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "bcid_3297E6A54E4787A5F51C49D9BA342B9A*ccid_Fe2Hx...",
    "bestRepresentativeQuery" : {
        "text" : "Sailing Dinghy",
        "displayText" : "Sailing Dinghy",
        "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Sailing+Dinghy...",
    },
    "pagesIncluding" : {
        "value" : [
            {
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
                "name" : "Powerboating Dublin, Dinghy Sailing Courses...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP....",
                "datePublished" : "2017-01-20T00:41:00.0000000Z",
                "contentUrl" : "http:\/\/www.contoso.ie\/content...",
                "hostPageUrl" : "http:\/\/www.contoso.ie\/powerboating...",
                "contentSize" : "59063 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "www.contoso.ie\/powerboating...",
                "width" : 800,
                "height" : 600,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 225
                },
                "imageInsightsToken" : "ccid_pHjQIA0x*mid_17F61B1316A39C92214...",
                "imageId" : "17F61B1316A39C922143FFDE9DFB5B0FB41171",
                "accentColor" : "0997C2"
            },
            . . .
        ]
    },
    "relatedSearches" : {
        "value" : [
            {
                "text" : "Sailing Fun",
                "displayText" : "Sailing Fun",
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Sailing...",
                "thumbnail" : {
                    "url" : "https:\/\/tse1.mm.bing.net\/th?q=Sailing+Fun..."
                }
            },
            . . .
        ]
    },
    "visuallySimilarImages" : {
        "value" : [
            {
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
                "name" : "Weekend On the Water",
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP...",
                "datePublished" : "2010-09-05T12:00:00.0000000Z",
                "contentUrl" : "http:\/\/1.bp.contoso.com\/_dc_6...",
                "hostPageUrl" : "http:\/\/contoso.com\/2010...",
                "contentSize" : "203806 B",
                "encodingFormat" : "jpeg",
                "hostPageDisplayUrl" : "contoso.com\/2010...",
                "width" : 1600,
                "height" : 1249,
                "thumbnail" : {
                    "width" : 300,
                    "height" : 234
                },
                "imageInsightsToken" : "ccid_Jg1Kwuc4*mid_5B7DA43976D3A422...",
                "imageId" : "5B7DA43976D3A422BA679A3AB019BB52C08DBC",
                "accentColor" : "0B2543"
            },
            . . .
        ]
    },
    "imageTags" : {
        "value" : [
            {
                "name" : "sail boat"
            },
            . . .
        ]
    }
}
```

## <a name="recognizing-entities-in-an-image"></a>Varlıkları bir resim tanıma  

Varlık tanıma özelliği, şu anda yalnızca kişilerin resim varlıkları tanımlar. Görüntü varlıkları tanımlamak üzere ayarlamak [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) sorgu parametresi için `RecognizedEntities`.  

> [!NOTE]
> Bu modül ile başka bir modül belirtemezsiniz. Bu modül diğer modüllerle belirtirseniz, yanıt tanınan varlıkları içermez.  

Aşağıdaki görüntü kullanarak belirteceğiniz gösterilmektedir [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imgurl) parametresi. URL unutmayın sorgu parametreleri kodlayın.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=faith+hill&insightsToken=mid_68364D764J...&modules=RecognizedEntities&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  

Aşağıda, bir önceki isteğin yanıtı gösterilmektedir. Görüntü iki kişinin içerdiğinden, yanıt bir bölgeyi her kişi için tanımlar. Bu durumda, kişilerin CelebrityAnnotations ve CelebRecognitionAnnotations gruplarında tanınmıyor. Bing kişinin Orijinal görüntüdeki eşleştikleri olasılığına göre her gruptaki kişilerin listeler. Güven azalan sırada listesidir. CelebRecognitionAnnotations grubu, eşleşme doğru olduğunu güvenle en üst düzey sağlar.  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "recognizedEntityGroups" : {
        "value" : [
            {
                "recognizedEntityRegions" : [...],
                "name" : "CelebRecognitionAnnotations"
            },
            {
                "recognizedEntityRegions" : [...],
                "name" : "CelebrityAnnotations"
            }
        ]
    }
}
```

`region` Alan Bing varlığı olduğu kabul edilen görüntünün alanını tanımlar. Kişiler için yüz kişinin bölgeyi temsil eder.  

Değerler dikdörtgenin genişliğini ve yüksekliğini özgün resmin göreli ve 0,0-1,0 arasındadır. Örneğin, görüntü 300 x 200 ve bölgenin üst ise, sol üst köşedeki noktada (10, 20) olduğu ve alt sağ köşesinde (290, 150) noktasında olduğu ve ardından normalleştirilmiş dikdörtgen:  

-   Sol: 10 / 300 = 0.03333...  
-   Sayfanın Üstü:  20 / 200 = 0.1  
-   Sağ: 290 / 300 = 0.9667...  
-   Alt: 150 / 200 = 0.75  

Bing sonraki ınsights çağrılarında döndürür bölge kullanabilirsiniz. Örneğin, görsel açıdan benzer resimler tanınan bir varlığın almak için şunu yazın. Görüntüleri kırpma görsel olarak benzer ve varlık tanıma modülleri ile kullanmak için daha fazla bilgi için bkz. Aşağıdaki bölge alanları ve sorgu parametreleri arasındaki eşleme, görüntüleri kırpma kullanılacağını gösterir.  

-   Sol eşlendiğini [cal](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cal)  
-   Top eşlenir [kat](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cat)  
-   Sağa doğru eşlendiğini [araba](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#car)  
-   Alt eşlenir [cab](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cab)  

## <a name="finding-visually-similar-images"></a>Görsel olarak benzer resimler bulma  

Orijinal görüntünün görsel olarak benzer resimler bulmak için ayarlanmış [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) sorgu parametresi SimilarImages için.  

Aşağıdaki isteği, görsel açıdan benzer resimler alma işlemi gösterilmektedir. Talep kullanan [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) sorgu parametresi özgün resmin tanımlamak için. İlgi düzeyi artırmak için kullanıcının sorgu dizesi içermelidir.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?insightsToken=mid_68364D764J...&modules=SimilarImages&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```


Aşağıda, bir önceki isteğin yanıtı gösterilmektedir.  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "visuallySimilarImages" : {
        "value" : [
            {
                "name" : "typical Hawaiian Sunset! :) | Scenes of Hawaii",
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detailv2...",
                "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP.Mda2a86...",
                 . . .
            }
        ]
    }
```

## <a name="cropping-images-to-use-with-visually-similar-and-entity-recognition-modules"></a>Kullanılacak görüntüleri kırpma görsel olarak benzer ve varlık tanıma modülleri  

Bing görsel olarak benzer resimler olup olmadığını belirlemek için veya varlık tanıma gerçekleştirmek için kullandığı görüntünün bölgeyi belirtmek için kullanın [cal](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cal), [cat](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cat), [cab](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cab)ve [araba](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#car) sorgu parametreleri. Varsayılan olarak, Bing, görüntünün tamamını kullanır.  

Üst, sol üst köşedeki ve altında karşılaştırma için Bing kullanan bölgeye sağ köşesindeki parametreleri belirtin. Değerleri özgün resmin genişliği ve yüksekliği kesirler olarak belirtin. Kesirli değer başlayın (0.0, 0.0) üst, sol üst köşedeki ve sonu (1.0, 1.0) sağ alt köşesindeki konumunda. Örneğin, üst, sol üst köşedeki biçimini çeyreği aşağı üst ve sol taraftaki şekilde çeyreği başladığını belirtmek için ayarlanmış `cal` 0,25 için ve `cat` 0,25.  

Aşağıdaki çağrıları dizisi kırpma bölgesini belirterek etkisini gösterir. İlk çağrı, kırpma içermez ve yan yana görüntünün ortasında duran iki kişinin Bing tanır.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  

Yanıt iki tanınan varlıkları gösterir.  

```json
{  
    "_type" : "ImageInsights",  
    "recognizedEntityGroups" : {
        "value": [  
            . . .  
            {  
                "recognizedEntityRegions" : [{  
                    "region" : {  
                        "left" : 0.5066667,  
                        "top" : 0.1955556,  
                        "right" : 0.75,  
                        "bottom" : 0.52  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Charlene Whitney",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                },  
                {  
                    "region" : {  
                        "left" : 0.25,  
                        "top" : 0.2488889,  
                        "right" : 0.4466667,  
                        "bottom" : 0.5111111  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Marcus Appel",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                }],  
            "name" : "CelebRecognitionAnnotations"
        }]
    }  
}  
```  

Görüntüyü dikey orta ikinci çağrı kırpar ve Bing resmin sağ tarafında tek bir kişi tanınır.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?cal=0.5&cat=0.0&car=1.0&cab=1.0&modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Yanıt, tanınan bir varlığın gösterir.  

```json  
{  
    "_type" : "ImageInsights",  
    "recognizedEntityGroups" : {
        "value" : [  
            . . .  
            {  
                "recognizedEntityRegions" : [{  
                    "region" : {  
                        "left" : 0.5066667,  
                        "top" : 0.1955556,  
                        "right" : 0.75,  
                        "bottom" : 0.52  
                    },  
                    "matchingEntities" : [{  
                        "entity" : {  
                            "_type" : "Person",  
                            "name" : "Charlene Whitney",  
                            . . .  
                        },  
                        "matchConfidence" : 0.9961388  
                    }]  
                }],  
                "name" : "CelebRecognitionAnnotations"  
            }
        ]
    }
}  
```  

## <a name="finding-visually-similar-products"></a>Görsel olarak benzer ürünleri bulma  

Ürün Orijinal görüntüdeki bulunamadı görsel olarak benzer ürünleri içeren görüntü bulmak için ayarlanmış [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) sorgu parametresi SimilarProducts için.  

Aşağıdaki isteği, görsel olarak benzer çok ürünlerin görüntülerini alma işlemi gösterilmektedir. Talep kullanan [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken) sorgu parametresi özgün tanımlamak için görüntü, önceki bir istekte döndürüldü. İlgi düzeyi artırmak için kullanıcının sorgu dizesi içermelidir.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=anne+klein+dresses&modules=SimilarProducts&insightsToken=ccid_WOeyfoSp*mid_4B0A357&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Aşağıda, bir önceki isteğin yanıtı gösterilmektedir. Yanıt benzer bir ürün görüntüsü içerir ve kaç tüccarların çevrimiçi ürün teklifi ürün derecelendirmeleri olup olmadığını gösterir ve en düşük fiyat bulundu (bkz `aggregateOffer` alan).  

```json
{
    "_type" : "ImageInsights",
    "imageInsightsToken" : "ccid_ldi5nX38*mid_29470780DE0E6F969...",
    "visuallySimilarProducts" : {
        "value" : [
            {
                "name" : "Sequin One-Shoulder Twist-Drape Dress",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=de...",  
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP.M85bdee...",  
                . . .
            },
            . . .
        ]
    }
}
```

Çevrimiçi ürün teklifi tüccarların listesini almak için (bkz [offerCount](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) alan), API yeniden çağırın ve ayarlama `modules` ShoppingSources için. Ardından, `insightsToken` ürün Özet görüntüde belirtecine sorgu parametresi bulunamadı.  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=ShoppingSources&insightsToken=ccid_hb3uRvUk*mid_BF5C252A47F2C765...&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Önceki isteğin yanıtını verilmiştir.  

```json  
{  
    "_type" : "ImageInsights",  
    "shoppingSources" : {  
        "offers" : [{  
            "url" : "http:\/\/www.contoso.com\/dp\/B00O...",  
            "seller" : {  
                "name" : "Contoso",  
                "image" : {  
                    "url" : "https:\/\/tse3.mm.bing.net\/th?id=A10d50fe..."  
                }  
            },  
            "price" : 126.87,  
            "priceCurrency" : "USD",  
            "availability" : "InStock"  
        },  
        {  
            "url" : "http:\/\/www.adatum.com\/product\/heritage...\/",  
            "seller" : {  
                "name" : "fabrikam.com"  
            },  
            "price" : 495,  
            "priceCurrency" : "USD",  
            "availability" : "InStock"  
        }]  
    }  
}  
```
