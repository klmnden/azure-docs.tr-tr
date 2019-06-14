---
title: Varsayılan ınsights etiketi - Bing görsel arama
titleSuffix: Azure Cognitive Services
description: Bing görsel arama hakkında görüntü döndüren varsayılan öngörüleri hakkında ayrıntılar sağlar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: scottwhi
ms.openlocfilehash: b6bc323f4e8deaf975c292f92d862b1fbe0e2714
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60510131"
---
# <a name="default-insights-tag"></a>Varsayılan ınsights etiketi

Bir varsayılan ınsights etikettir `displayName` alan boş bir dize olarak ayarlayın. Aşağıdaki örnek, varsayılan ınsights (Eylemler) olası listesini gösterir. Yanıtı içeren eylemler listesi görüntüye göre değişir. Ve her eylem için özelliklerin listesini resim göre farklılık gösterir, böylece özelliği, kullanmayı denemeden önce olup olmadığını denetleyin.

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {
          "_type" : "ImageModuleAction",
          "actionType" : "PagesIncluding",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageShoppingSourcesAction",
          "actionType" : "ShoppingSources",
          "data" : {
            "offers" : [...]
          }
        },
        {
          "image" : {...},
          "actionType" : "MoreSizes"
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "VisualSearch",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRecipesAction",
          "actionType" : "Recipes",
          "data" : {
            "value" : [...]
          }
        },
        {
          "image" : {...},
          "actionType" : "ImageById"
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "ProductVisualSearch",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRelatedSearchesAction",
          "actionType" : "RelatedSearches",
          "data" : {
            "value" : [...]
          }
        },
        {
          "_type" : "ImageRelatedSearchesAction",
          "actionType" : "DocumentLevelSuggestions",
          "data" : {
            "value" : [...]
          }
        }
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

## <a name="pagesincluding-insight"></a>PagesIncluding Insight

PagesIncluding Insight, bu görüntüyü içeren Web sayfalarının bir listesini sağlar. Aslında bir listesi olan `Image` nesneleri ve `hostPageUrl` alan bir görüntü içerdiğinden Web sayfasının URL'sini içerir. Örnek kullanım için bkz: [PagesIncluding Insight örnek](./bing-insights-usage.md#pagesincluding-insight-example).

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "PagesIncluding",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https://www.bing.com\/images\/search?",
              "name" : "Today's smoking hot country",
              "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=OIP...",
              "datePublished" : "2017-09-20T12:00:00.0000000Z",
              "contentUrl" : "http:\/\/contoso.com\/wordstuff",
              "hostPageUrl" : "http:\/\/contoso.com\/2017\/09\/20\/car",
              "contentSize" : "122540 B",
              "encodingFormat" : "jpeg",
              "hostPageDisplayUrl" : "contoso.com\/2017\/09\/20\/car",
              "width" : 894,
              "height" : 1200,
              "thumbnail" : {
                "width" : 474,
                "height" : 636
              },
              "imageInsightsToken" : "ccid_CO5GEthj*mid_5323B1",
              "insightsMetadata" : {
                "pagesIncludingCount" : 12,
                "availableSizesCount" : 7
              },
              "imageId" : "5323B1900FB9087B6B45D176D234E1F2F23CD3A5",
              "accentColor" : "55585B"
            }
          ]
        }
      }
```

## <a name="shoppingsources-insight"></a>ShoppingSources Insight

Kullanıcı görüntüde gösterilen öğenin burada satın alabilirsiniz Web sitelerinin bir listesiyle ShoppingSources öngörü sağlar. Tekliflerinin listesini, kullanıcı öğesi burada satın alabilirsiniz Web sayfasının URL'sini, fiyat öğesi ve derecelendirme veya gözden geçirme ayrıntıları içerir. Örnek kullanım için bkz: [ShoppingSources örnek](./bing-insights-usage.md#shoppingsources-insight-example).

```json
      {
        "_type" : "ImageShoppingSourcesAction",
        "actionType" : "ShoppingSources",
        "data" : {
          "offers" : [
            {
              "name" : "Apple Pie",
              "url" : "https:\/\/contoso.com\/product_p\/l10.htm",
              "description" : "A taste of the crust, apple, and pie filling...",
              "seller" : {
                "name" : "Contoso"
              },
              "price" : 3.99,
              "priceCurrency" : "USD",
              "aggregateRating" : {
                "ratingValue" : 5
              },
              "lastUpdated" : "2018-04-16T00:00:00.0000000"
            }
          ]
        }
      }
```

## <a name="moresizes-insight"></a>Öngörü moreSizes

Bing İnternette bulunan görüntünün boyutu (daha büyük veya küçük) sayısını MoreSizes Insight tanımlar (bkz `availableSizesCount` alan):

```json
      {
        "image" : {
          "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detai...",
          "name" : "Making Apple Pie",
          "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP....",
          "datePublished" : "2013-06-21T12:00:00.0000000Z",
          "contentUrl" : "http:\/\/contoso.com\/content\/uploads\/2013\/06\/apple-pie.jpg",
          "hostPageUrl" : "http:\/\/contoso.com\/2013\/06\/21\/making-apple-pie\/",
          "contentSize" : "134847 B",
          "encodingFormat" : "jpeg",
          "hostPageDisplayUrl" : "contoso.com\/2013\/06\/21\/making-apple-pie",
          "width" : 1050,
          "height" : 765,
          "thumbnail" : {
            "width" : 474,
            "height" : 345
          },
          "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CF...",
          "insightsMetadata" : {
            "recipeSourcesCount" : 6,
            "pagesIncludingCount" : 103,
            "availableSizesCount" : 28
          },
          "imageId" : "D12339146CFEDF3D409CC7A66D2C98D0D71904D4",
          "accentColor" : "3A0B01"
        },
        "actionType" : "MoreSizes"
      },
```

## <a name="visualsearch-insight"></a>VisualSearch Insight

VisualSearch Insight (Orijinal görüntüdeki gösterilen içeriği benzer içeriği içerir) özgün resmin görsel olarak benzer görüntülerin bir listesini sağlar. Örnek kullanım için bkz: [VisualSearch Insight örnek](./bing-insights-usage.md#visualsearch-insight-example).

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "VisualSearch",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=...",
              "name" : "An apple pie...",
              "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.z...",
              "datePublished" : "2017-03-18T00:17:00.0000000Z",
              "contentUrl" : "http:\/\/contoso.net\/images\/8\/8a\/an_apple_pie.png",
              "hostPageUrl" : "http:\/\/contoso.com\/wiki\/an_apple_pie.png",
              "contentSize" : "87930 B",
              "encodingFormat" : "png",
              "hostPageDisplayUrl" : "contoso.com\/wiki\/an_apple_pie.png",
              "width" : 263,
              "height" : 192,
              "thumbnail" : {
                "width" : 474,
                "height" : 346
              },
              "imageInsightsToken" : "ccid_zhRxfGkI*mid_1DCBA7AA6D231...",
              "insightsMetadata" : {
                "recipeSourcesCount" : 6,
                "pagesIncludingCount" : 103,
                "availableSizesCount" : 28
              },
              "imageId" : "1DCBA7AA6D23147F9DD06D47DB3A38EB25389",
              "accentColor" : "3E0D01"
            }
          ]
        }
      }
```

## <a name="recipes-insight"></a>Tarif içgörüleri

Tarif içeren Insight görüntüde verilen Gıda oluşturmaya yönelik bir tarif içeren Web sayfalarının bir listesini sağlar. Örnek kullanım için bkz: [tarifleri Insight örnek](./bing-insights-usage.md#recipes-insight-example).

```json
      {
        "_type" : "ImageRecipesAction",
        "actionType" : "Recipes",
        "data" : {
          "value" : [
            {
              "name" : "Granny's Apple Pie",
              "url" : "http:\/\/contoso.com\/recipes\/appetizer\/apple-pie.html",
              "description" : "I love Granny's apple pie. Sooooo delicious...",
              "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=A63002cd9",
              "creator" : {
                "_type" : "Person",
                "name" : "Charlene Whitney"
              },
              "aggregateRating" : {
                "text" : "5",
                "ratingValue" : 5,
                "bestRating" : 5,
                "reviewCount" : 1
              },
              "cookTime" : "PT45M",
              "prepTime" : "PT1H",
              "totalTime" : "PT1H45M"
            }
          ]
        }
      }
```


## <a name="imagebyid-insight"></a>ImageById Insight

ImageById sağlayan bir `Image` Öngörüler için istenen görüntünün nesnesi:

```json
      {
        "image" : {
          "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=deta...",
          "name" : "Making Apple Pie",
          "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP...",
          "datePublished" : "2013-06-21T12:00:00.0000000Z",
          "contentUrl" : "http:\/\/contoso.com\/content\/uploads\/2013\/06\/apple-pie.jpg",
          "hostPageUrl" : "http:\/\/contoso.com\/2013\/06\/21\/making-apple-pie\/",
          "contentSize" : "134847 B",
          "encodingFormat" : "jpeg",
          "hostPageDisplayUrl" : "contoso.com\/2013\/06\/21\/making-apple-pie",
          "width" : 1050,
          "height" : 765,
          "thumbnail" : {
            "width" : 474,
            "height" : 345
          },
          "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFE...",
          "insightsMetadata" : {
            "recipeSourcesCount" : 6,
            "pagesIncludingCount" : 103,
            "availableSizesCount" : 28
          },
          "imageId" : "D12339146CFEDF3D409A66D2C98D0D71904D4",
          "accentColor" : "3A0B01"
        },
        "actionType" : "ImageById"
      },
```

## <a name="productvisualsearch-insight"></a>ProductVisualSearch Insight

Ürün Orijinal görüntüdeki görsel olarak benzer ürünleri görüntülerin listesini ProductVisualSearch öngörü sağlar. `insightsMetadata` Alan burada satın alabilirsiniz ürün ve ürünün fiyatı teklifleri hakkında bilgi içeriyor olabilir.

```json
      {
        "_type" : "ImageModuleAction",
        "actionType" : "ProductVisualSearch",
        "data" : {
          "value" : [
            {
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?view=detail...",
              "name" : "Contoso 4-Piece Kitchen Package...",
              "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.l9hzaabu-RJd...",
              "datePublished" : "2017-07-16T04:28:00.0000000Z",
              "contentUrl" : "https:\/\/www.contoso.com\/assets\/media\/images\/prod...",
              "hostPageUrl" : "https:\/\/www.contoso.com\/4-piece-kitchen-package...",
              "contentSize" : "13594 B",
              "encodingFormat" : "jpeg",
              "hostPageDisplayUrl" : "https:\/\/www.contoso.com\/4-piece-kitchen-package...",
              "width" : 450,
              "height" : 332,
              "thumbnail" : {
                "width" : 474,
                "height" : 349
              },
              "imageInsightsToken" : "ccid_l9hzaabu*mid_70A8B616355D681DB9A5A...",
              "insightsMetadata" : {
                "shoppingSourcesCount" : 1,
                "recipeSourcesCount" : 0,
                "aggregateOffer" : {
                  "name":"4-Piece Kitchen Package with...",
                  "priceCurrency":"USD",
                  "lowPrice":2756,
                  "offers" : [
                    {
                      "name" : "4-Piece Kitchen Package with...",
                      "url" : "https:\/\/www.fabrikam.com\/1234.html?ref=bing",
                      "description" : "This 36 Frenchdoor refrigerator by...",
                      "seller" : {
                        "name" : "Fabrikam",
                        "image" : {
                          "url" : "https:\/\/tse1.mm.bing.net\/th?id=A818f811..."
                        }
                      },
                      "price" : 2756,
                      "priceCurrency" : "USD",
                      "availability" : "InStock",
                      "lastUpdated" : "2018-02-20T00:00:00.0000000"
                    }
                  ],
                  "offerCount":1
                },
                "pagesIncludingCount" : 4,
                "availableSizesCount" : 2
              },
              "imageId" : "70A8B616355D681DA5980A8D0514BCC995A3",
              "accentColor" : "60646B"
            }
          ]
        }
      }
```

## <a name="relatedsearches-insight"></a>RelatedSearches Insight

RelatedSearches Insight (diğer kullanıcıların arama koşullarınızda göre) başkaları tarafından yapılan ilgili aramalar bir listesini sağlar. Örnek kullanım için bkz: [RelatedSearches Insight örnek](./bing-insights-usage.md#relatedsearches-insight-example).

```json
      {
        "_type" : "ImageRelatedSearchesAction",
        "actionType" : "RelatedSearches",
        "data" : {
          "value" : [
            {
              "text" : "Homemade Apple Pies Recipes",
              "displayText" : "Homemade Apple Pies Recipes",
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Homemade...",
              "thumbnail" : {
                "url" : "https:\/\/tse1.mm.bing.net\/th?q=Homemade+Apple+Pies"
              }
            }
          ]
        }
      }
```

## <a name="documentlevelsuggestions-insight"></a>DocumentLevelSuggestions Insight

DocumentLevelSuggestions Insight görüntüsünün içeriğine göre önerilen arama terimlerini listesi sağlar:

```json
      {
        "_type" : "ImageRelatedSearchesAction",
        "actionType" : "DocumentLevelSuggestions",
        "data" : {
          "value" : [
            {
              "text" : "American Apple Pie",
              "displayText" : "American Apple Pie",
              "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=American",
              "thumbnail" : {
                "url" : "https:\/\/tse3.mm.bing.net\/th?q=American+Apple+Pie."
              }
            }
          ]
        }
      }
```

## <a name="next-steps"></a>Sonraki adımlar

Kullanıma [ınsights kullanım örnekleri, Bing](bing-insights-usage.md) Bing görsel Öngörüler nasıl görüntüleyebilir görmek için.

İlk isteğinizi hızlıca başlamak için hızlı başlangıçları bakın: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).
