---
title: Görüntü öngörü edinme | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Bing görüntü arama API görüntüyü hakkında daha fazla bilgi almak için nasıl kullanılacağını gösterir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 0BCD936E-D4C0-472D-AE40-F4B2AB6912D5
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: f651d9f773f475e633aed698e134aa6a7c07393b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354592"
---
# <a name="get-insights-about-an-image"></a>Bir görüntü ile ilgili Öngörüler alın

> [!IMPORTANT]
> Görüntü bilgileri elde etmek için/görüntüleri/Ayrıntılar endpoint kullanmak yerine, kullanmanız gereken [Visual arama](../bing-visual-search/overview.md) çünkü daha kapsamlı bilgiler sağlar.


Her görüntünün görüntü hakkında bilgi almak için kullanabileceğiniz bir Öngörüler belirteci içerir. Örneğin, burada görüldüğü ürün satın alabileceğiniz ilgili görüntüleri, görüntüyü içeren web sayfaları, bir koleksiyonu veya tüccarların listesini alabilirsiniz.  
  
Bir görüntü ile ilgili bilgileri elde etmek için görüntü yakalama [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken) yanıt belirteç. 

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

Ardından, görüntü ayrıntıları uç noktasını çağırmak ve ayarlayın [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) sorgu parametresi belirteç için `imageInsightsToken`.  

Almak istediğiniz bilgileri belirtmek için ayarlayın `modules` sorgu parametresi. Tüm bilgileri elde etmek için ayarlama `modules` için `All`. Yalnızca başlık ve toplama bilgileri elde etmek için ayarlama `modules` için `Caption%2CCollection`. Olası Öngörüler tam bir listesi için bkz: [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested). Tüm Öngörüler tüm görüntüleri için kullanılabilir. Yanıt, istenen tüm Öngörüler varsa içeriyor.

Aşağıdaki örnek önceki görüntüsü için tüm kullanılabilir Öngörüler ister.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_D6426898706EC7...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
``` 

## <a name="getting-insights-of-a-known-image"></a>Bilinen bir görüntü Öngörüler alma

Öngörüler almak için kullanmak istediğiniz bir resim URL'si varsa [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl) sorgu parametresi yerine [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) görüntüyü belirtmek için parametre. Ya da görüntü dosyası varsa, görüntünün ikili bir POST isteğinin gövdesinde gönderebilir. Bir POST isteği kullanırsanız `Content-Type` üstbilgi ayarlanmalıdır `multipart/data-form`. Her iki seçenek ile görüntü boyutu 1 MB aşamaz.  
  
Resim URL'si varsa, aşağıdaki örnekte Öngörüler görüntü isteği gösterilmektedir.

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&imgUrl=https%3A%2F%2Fwww.mydomain.com%2Fimages%2Fsunflower.png&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
``` 
  
## <a name="getting-all-image-insights"></a>Tüm görüntü Öngörüler alma  

Görüntü tüm Öngörüler istemek için ayarlanmış [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) sorgu parametresi için `All`. İlgili aramalar almak için istek kullanıcının sorgu dizesi içermelidir. Bu örnekte gösterilir kullanarak [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) görüntüyü belirtmek için.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=sailing+dinghy&insightsToken=mid_68364D764J...&modules=All&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Üst düzey nesne bir [ImageInsightsResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) yerine Nesne bir [görüntüleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) nesnesi.  
  
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

## <a name="recognizing-entities-in-an-image"></a>Bir görüntü varlıklarda tanıma  

Varlık tanıma özelliği şu anda yalnızca Kişiler görüntüyü varlıklarda tanımlar. Bir görüntü varlıklarda tanımlamak üzere ayarlamak [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) sorgu parametresi için `RecognizedEntities`.  

> [!NOTE]
> Bu modül sahip başka bir modül belirtemezsiniz. Bu modül diğer modüller ile belirtirseniz, yanıt tanınan varlıklar içermez.  
  
Aşağıdaki görüntü kullanarak belirtmek gösterilmiştir [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl) parametresi. URL'ye unutmayın sorgu parametrelerini kodlayın.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=faith+hill&insightsToken=mid_68364D764J...&modules=RecognizedEntities&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  
  
Önceki istek yanıtı gösterir. Görüntü iki kişinin içerdiğinden, yanıt her kişi için bir bölge tanımlar. Bu durumda, kişilerin CelebrityAnnotations ve CelebRecognitionAnnotations gruplarında tanınmıyor. Bing özgün görüntüsüne bizzat eşleşen olasılığını göre her grubundaki kişileri listeler. GÜVENİRLİK azalan sırada listesidir. CelebRecognitionAnnotations grubu en üst düzeye bir eşleşme doğru olduğunu güven sağlar.  
  
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
  
`region` Alan görüntünün nerede Bing tanınan varlık alanını tanımlar. Kişiler için kişinin yüz bölgeyi temsil eder.  
  
Değerler dikdörtgenin genişliği ve yüksekliği orijinal görüntünün göre ve 0,0 ile 1,0 arasındadır. Örneğin, görüntüyü 300 x 200 ve bölgenin üst ise, sol köşe noktada (10, 20) olan ve alt köşedeki noktada (290, 150) olduğundan, ardından normalleştirilmiş dikdörtgen:  
  
-   Sol: 10 / 300 = 0.03333...  
-   Üst: 20 / 200 = 0,1  
-   Sağ: 290 / 300 = 0.9667...  
-   Alt: 150 / 200 = 0,75  
  
Bing sonraki Öngörüler çağrılarında döndürür bölge kullanabilirsiniz. Örneğin, tanınan varlık görsel olarak benzer görüntüleri alma. Daha fazla bilgi için bkz: [görsel olarak benzer ve varlık tanıma modülleri ile kullanılacak görüntüleri kırpma](#croppingimages). Aşağıdaki bölge alanları ve sorgu parametreleri arasında eşleme, görüntüleri kırpma kullanılacağını gösterir.  
  
-   Sol eşlendiğini [cal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal)  
-   Top eşlenir [kat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat)  
-   Sağ eşlendiğini [araba](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car)  
-   Alt eşlenir [cab](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)  

## <a name="finding-visually-similar-images"></a>Görsel olarak benzer görüntüleri bulma  

Görsel olarak özgün görüntüsüne benzer görüntüleri bulmak için ayarlanmış [modülleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) sorgu SimilarImages parametresi.  
  
Aşağıdaki isteği görsel olarak benzer görüntüleri alma gösterir. Talep kullanan [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) sorgu parametresi özgün görüntüsünü belirleyin. İlgi artırmak için kullanıcının sorgu dizesi içermelidir.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?insightsToken=mid_68364D764J...&modules=SimilarImages&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

  
Önceki istek yanıtı gösterir.  
  
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
  
## <a name="cropping-images-to-use-with-visually-similar-and-entity-recognition-modules"></a>İle kullanılacak görüntüleri kırpma görsel olarak benzer ve varlık tanıma modülleri  

Bing görüntüleri görsel olarak benzer olup olmadığını belirlemek için veya varlık tanıma gerçekleştirmek için kullandığı görüntüsünün bölgesinden belirtmek için kullanın [cal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal), [kat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat), [cab](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)ve [araba](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car) sorgu parametreleri. Varsayılan olarak, görüntünün tamamını Bing kullanır.  
  
Parametreler, üst sol köşesinde ve alt, Bing karşılaştırma için kullandığı bölge sağ köşesindeki belirtin. Değerleri özgün resmin genişlik ve yükseklik kesirlerini belirtin. Kesirli değerlerin başlayın (0.0, 0.0), sol köşe ve üstündeki ile biten (1.0, 1.0) sağ alt köşesindeki adresindeki. Örneğin, üst, sol köşe biçimini çeyreği aşağı üst ve sol taraftaki şekilde çeyreği başlayacağını belirtmek için ayarlar `cal` 0,25 için ve `cat` 0,25.  
  
Aşağıdaki çağrıları dizisini kırpma bölgesini belirterek etkisini gösterir. İlk çağrıda kırpma içermez ve yan yana görüntünün ortasında duran iki kişinin Bing tanır.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```  

Yanıt iki tanınan varlık gösterir.  
  
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
  
İkinci çağrı dikey orta aşağı resmi kırpar ve Bing tek bir kişinin görüntünün sağına tanınmıyor.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?cal=0.5&cat=0.0&car=1.0&cab=1.0&modules=RecognizedEntities&imgurl=https%3A%2F%2Ftse1.mm.bing.net%2Fth%3Fid%3DOIP.M0cbee6fadb43f35b2344e53da7a23ec1o0%26pid%3DApi&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Yanıt, bir tanınan varlık gösterir.  
  
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

Orijinal görüntüdeki bulundu ürün görsel olarak benzer ürünleri içeren resimler bulmak için ayarlanmış [modueles](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) sorgu SimilarProducts parametresi.  
  
Aşağıdaki isteği görsel olarak benzer ürün görüntüleri alma gösterir. Talep kullanan [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) özgün tanımlamak için parametresi görüntü sorgu, önceki bir istekte döndürüldü. İlgi artırmak için kullanıcının sorgu dizesi içermelidir.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?q=anne+klein+dresses&modules=SimilarProducts&insightsToken=ccid_WOeyfoSp*mid_4B0A357&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```
  
Önceki istek yanıtı gösterir. Yanıt benzer bir ürün görüntüsünü içerir ve kaç tüccarların çevrimiçi ürün teklif ürün derecelendirmeleri olup olmadığını gösterir ve en düşük fiyatı bulundu (bkz `aggregateOffer` alan).  
  
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
  
Çevrimiçi ürün teklif tüccarların listesini almak için (bkz [offerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offer-offercount) alanı), API yeniden çağırın ve ayarlayın `modules` ShoppingSources için. Ardından, `insightsToken` sorgu parametresi belirtecine ürün Özet görüntüde bulundu.  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/details?modules=ShoppingSources&insightsToken=ccid_hb3uRvUk*mid_BF5C252A47F2C765...&mkt=en-us HTTP/1.1    
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com
```

Önceki istek yanıtı verilmiştir.  
  
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
