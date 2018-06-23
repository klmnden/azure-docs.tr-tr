---
title: Proje yanıt arama varlığı sorgusu - Microsoft Bilişsel hizmetler | Microsoft Docs
description: Proje yanıt arama'yı kullanarak varlıklar için sorgu
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 2b8382b791c02514e5110097700e223d98fafd6a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353980"
---
# <a name="query-for-entities"></a>Varlıklar için sorgu

Sorgu bir kişi, yer veya şey hakkındaki bilgileri isterse, yanıt içerebilir bir `entities` yanıt.  Sorguları, Web sayfaları, her zaman döndürür [bulguları](fact-queries.md) ve/veya [varlıklar](entity-queries.md) sorgu bağımlıdır.

Varlıkları üç sorgu senaryoları destekler: 
-   DominantEntity — kullanıcının sorgu ve amacı eşleşen tek bir varlık yok. Örneğin, sorgu, alan iğnenin DominantEntity senaryodur. 
-   Kesinleştirme — kullanıcının sorgu ve amacı eşleşen birden fazla varlık ve doğru varlığı seçin kullanıcıya kadar olur. Örneğin, oyun, Thrones sorgu televizyon programını ve defteri serisini döndüren bir Kesinleştirme senaryodur. 
-   Liste — kullanıcının sorgu ve amacı eşleşen birden çok varlık vardır. Örneğin, "Endangered sağındaki listesi" sorgu satır ve hücre görüntülemek biçimlendirilmiş tablo değer döndüren bir liste senaryodur. 
 
Sorgu senaryo belirlemek için `queryScenario` alanını `entities` nesnesi. Varlık içeren verileri varlığın türüne bağlıdır. Aynı temel bilgileri varlıkları içerse de, bazı varlıklar turistik faaliyetler ya da books gibi ek özellikler içerir. Ek özellikler dahil varlıklar dahil `_type` seri hale getirici tarafından kullanılan bir ipucu içeren alan. Aşağıdaki varlıklar ek özellikleri içerir: 
-   Rehberi 
-   MusicRecording 
-   Kişi 
-   Çekim 
 
Yanıtı içeren varlık türünü belirlemek için `entityTypeHints` alan için fatura ağ geçitlerini sorguda gösterildiği gibi.
````
        },
        "description": "Bill Gates is an American business man and philanthropist, co-founder of Microsoft",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Person"
          ]
        },
        "bingId": "6d7d66a7-2cb8-0ae9-637c-f81fd749dc9a"
      }
````
Alanı iğnenin yönelik bir sorgu verilmiştir:
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=space+needle&mkt=en-us
````
Yanıt içeriyor `entities` yanıt. Not `entityScenario` ve `entityTypeHints` alanları. 
````
  "entities": {
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#Entities.0",
        "contractualRules": [
          {
            "_type": "ContractualRules/LicenseAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "license": {
              "name": "CC-BY-SA",
              "url": "http://creativecommons.org/licenses/by-sa/3.0/"
            },
            "licenseNotice": "Text under CC-BY-SA license"
          },
          {
            "_type": "ContractualRules/LinkAttribution",
            "targetPropertyName": "description",
            "mustBeCloseToContent": true,
            "text": "Wikipedia",
            "url": "http://en.wikipedia.org/wiki/Space_Needle"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://en.wikipedia.org/wiki/Space_Needle"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q\u003dSpace+Needle\u0026filters\u003dsid:%22f8dd5b08-206d-2554-6e4a-893f51f4de7e%22\u0026elv\u003dAXXfrEiqqD9r3GuelwApulpmymQx!ODfuQu*veOQHkvP0!Zbvi5F5tVcMSDJvDEWiQWwrdueYTtIszgj03oFQHykYYLYgq3q5!Sf00QxXGIS",
        "name": "Space Needle",
        "image": {
          "name": "Space Needle",
          "thumbnailUrl": "https://www.bing.com/th?id\u003dA15d336cf119b9b5c7e0ab37e271421d3\u0026w\u003d110\u0026h\u003d110\u0026c\u003d7\u0026rs\u003d1\u0026qlt\u003d80\u0026cdv\u003d1\u0026pid\u003d16.1",
          "provider": [
            {
              "_type": "Organization",
              "url": "http://en.wikipedia.org/wiki/Space_Needle"
            }
          ],
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg",
          "width": 110,
          "height": 110,
          "sourceWidth": 152,
          "sourceHeight": 300
        },
        "description": "The Space Needle is an observation tower in Seattle, Washington, a landmark of the Pacific Northwest, and an icon of Seattle. It was built in the Seattle Center for the 1962 World\u0027s Fair, which drew over 2.3 million visitors, when nearly 20,000 people a day used its elevators.",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Attraction"
          ]
        },
        "bingId": "f8dd5b08-206d-2554-6e4a-893f51f4de7e"
      }
    ]
  },
````

İlgili ise bir sorgu listesini dönebilirsiniz.

**Sorgu:** aşağıdaki sorgu endangered sağındaki listesini bulur:

````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=list+of+endangered+species

````

**Yanıt:** yanıt görüntülenmek üzere tablo değerleri olarak biçimlendirilmiş bir listesini içerir:
````
  "facts": {
    "id": "https://www.bingapis.com/api/v7/#Facts",
    "contractualRules": [
      {
        "_type": "ContractualRules/LinkAttribution",
        "text": "www.worldwildlife.org/species/directory?direction=desc&sort=e…",
        "url": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
      }
    ],
    "attributions": [
      {
        "providerDisplayName": "www.worldwildlife.org/species/directory?direction=desc&sort=e…",
        "seeMoreUrl": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
      }
    ],
    "value": [
      {
        "subjectName": "Species Directory",
        "richCaption": {
          "_type": "StructuredValue/TabularData",
          "header": [
            "Common name",
            "Scientific name",
            "Conservation status ↓"
          ],
          "rows": [
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Amur Leopard",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/amur-leopard"
                },
                {
                  "text": "Panthera pardus orientalis"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Black Rhino",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/black-rhino"
                },
                {
                  "text": "Diceros bicornis"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Bornean Orangutan",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/bornean-orangutan"
                },
                {
                  "text": "Pongo pygmaeus"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Cross River Gorilla",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/cross-river-gorilla"
                },
                {
                  "text": "Gorilla gorilla diehli"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            }
          ],
          "seeMoreUrl": {
            "text": "46 more rows",
            "url": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
          }
        }
      }
    ]
  },

````


## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)