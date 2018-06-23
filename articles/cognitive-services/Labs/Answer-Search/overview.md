---
title: Yanıt arama genel bakış - Microsoft Bilişsel hizmetler proje | Microsoft Docs
description: Proje yanıt arama giriş.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: d87cf1390970d2c815b94bcaee7e07c19bc03cce
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354118"
---
# <a name="what-is-project-answer-search"></a>Proje yanıt arama nedir?
Proje yanıt arama API Bing v7 endpoint interrogative sorgulara yanıt almak için kullanır. "Dünya çevresi nedir?" A gibi soru factual bilgilerle yanıt verir.  Bir kişi, yer ya da bir şey için bir sorgu sorgu tarafından tanımlanan varlığı hakkında bilgi verir. Bu senaryolar konuşma bot, Mesajlaşma uygulamalar, okuyucular, vb. gibi uygulamalarda yararlı olabilir.  

Sorguları sorgu senaryoya bağlıdır yanıtlarını döndürür: Web sayfalarının her zaman döndürdü, ancak [bulguları](fact-queries.md) ve/veya [varlıklar](entity-queries.md) ilgiliyse döndürülür.

## <a name="endpoint"></a>Uç Nokta
Soruların bir soru veya bir kişi, yer veya şey hakkında bilgi almak için yanıt arama API uç noktası için bir istek gönderin. Üstbilgiler ve URL parametreleri için çeşitli belirtimleri kullanın.  Dahil *Apim abonelik anahtar Ocp* geçerli bir belirteci ile üstbilgisi.  Pazar parametresi gereklidir. Yalnızca `en-us` Pazar şu anda desteklenmiyor.

Aşağıdaki sorgu sorusunun yanıtlarını alır: "Dünya çevresi nedir?"

AL:
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=what+is+circumference+of+the=earth?&mkt=en-us

````

URL parametresi `q=` arama nesnesinin belirtmeniz gerekir.

## <a name="response-object"></a>Yanıt nesnesi

Yanıt, HTTP üst bilgileri, Web sayfaları, bulguları ve/veya varlık içeriyor.

````
BingAPIs-TraceId: AB2E75C998614ADB8EBF5110DF648298
X-MSEdge-ClientID: 1E48FC4F7B8768C80B14F7997A106906
BingAPIs-SessionId: 0504DDD6DAE84861A4842306F8DA7A58
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: AB2E75C998614ADB8EBF5110DF648298 Ref B: CO1EDGE0322 Ref C: 2018-04-19T19:57:13Z

JSON Response:

{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "what is the circumference of earth"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q\u003dwhat+is+the+circumference+of+earth",
    "totalEstimatedMatches": 217000,
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#WebPages.0",
        "name": "Circumference of the Earth - Universe Today",
        "url": "https://www.universetoday.com/26461/circumference-of-the-earth/",
        "isFamilyFriendly": true,
        "displayUrl": "https://www.universetoday.com/26461/circumference-of-the-earth",
        "snippet": "The circumference of the Earth in kilometers is 40,075 km, and the circumference of the Earth in miles is 24,901. In other words, if you could drive your car around the equator of the Earth (yes, even over the oceans), youâ€™d put on an extra 40,075 km on the odometer.",
        "deepLinks": [
          {
            "name": "About Earth",
            "url": "https://www.universetoday.com/14382/10-interesting-facts-about-planet-earth/"
          }
        ],
        "dateLastCrawled": "2018-04-12T14:13:00.0000000Z",
        "language": "en"
      },
      {
        "id": "https://www.bingapis.com/api/v7/#WebPages.1",
        "name": "Earth - Wikipedia",
        "url": "https://en.wikipedia.org/wiki/Earth",
        "about": [
          {
            "name": "Earth"
          },
          {
            "name": "Earth"
          }
        ],
        "isFamilyFriendly": true,
        "displayUrl": "https://en.wikipedia.org/wiki/Earth",
        "snippet": "Circumference: 40 075.017 km equatorial (24 901.461 mi) ... Earth is the third planet from the Sun and the only object in the Universe known to harbor life.",
        "deepLinks": [
          {
            "name": "Moon",
            "url": "https://en.wikipedia.org/wiki/Moon"
          },
          {
            "name": "Planet",
            "url": "https://en.wikipedia.org/wiki/Planet"
          },
          {
            "name": "Quasi-Satellites",
            "url": "https://en.wikipedia.org/wiki/Quasi-satellite"
          },
          {
            "name": "World Population",
            "url": "https://en.wikipedia.org/wiki/World_population"
          },
   . . .

    ]
  },
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
            "url": "http://en.wikipedia.org/wiki/Earth"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://en.wikipedia.org/wiki/Earth"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q\u003dEarth\u0026filters\u003dsid:%226ddb3372-4801-5567-321e-e8a53bd774a4%22\u0026elv\u003dAXXfrEiqqD9r3GuelwApulpmymQx!ODfuQu*veOQHkvP0!Zbvi5F5tVcMSDJvDEWiQWwrdueYTtIszgj03oFQHykYYLYgq3q5!Sf00QxXGIS",
        "name": "Earth",
        "image": {
          "name": "Earth",
          "thumbnailUrl": "https://www.bing.com/th?id\u003dA3ab623665ab412f386c162bd29f0683a\u0026w\u003d110\u0026h\u003d110\u0026c\u003d7\u0026rs\u003d1\u0026qlt\u003d80\u0026cdv\u003d1\u0026pid\u003d16.1",
          "provider": [
            {
              "_type": "Organization",
              "url": "http://en.wikipedia.org/wiki/Earth"
            }
          ],
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/commons/9/97/The_Earth_seen_from_Apollo_17.jpg",
          "width": 110,
          "height": 110,
          "sourceWidth": 799,
          "sourceHeight": 800
        },
        "description": "Earth is the third planet from the Sun and the only object in the Universe known to harbor life. According to radiometric dating and other sources of evidence, Earth formed over 4.5 billion years ago. Earth\u0027s gravity interacts with other objects in space, especially the Sun and the Moon, Earth\u0027s only natural satellite. Earth revolves around the Sun in 365.26 days, a period known as an Earth year. During this time, Earth rotates about its axis about 366.26 times.",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Generic"
          ]
        },
        "bingId": "6ddb3372-4801-5567-321e-e8a53bd774a4"
      }
    ]
  },
  "facts": {
    "id": "https://www.bingapis.com/api/v7/#Facts",
    "contractualRules": [
      {
        "_type": "ContractualRules/LinkAttribution",
        "text": "www.universetoday.com/26461/circumference-of-the-earth/",
        "url": "http://www.universetoday.com/26461/circumference-of-the-earth/"
      }
    ],
    "attributions": [
      {
        "providerDisplayName": "www.universetoday.com/26461/circumference-of-the-earth/",
        "seeMoreUrl": "http://www.universetoday.com/26461/circumference-of-the-earth/"
      }
    ],
    "value": [
      {
        "description": "The circumference of the Earth in kilometers is 40,075 km, and the circumference of the Earth in miles is 24,901. In other words, if you could drive your car around the equator of the Earth (yes, even over the oceans), youâ€™d put on an extra 40,075 km on the odometer.",
        "subjectName": ""
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "Facts",
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Facts"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.0"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.1"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 2,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.2"
          }
        },
        
        . . . 
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        }
      ]
    }
  }
}

````

## <a name="terms-of-use"></a>Kullanım koşulları
Proje yanıt arama ve proje Video eğilimleri olan tabi [Bing arama kullanın ve görüntü gereksinimleri](use-display-requirements.md).

Siz veya bir üçüncü taraf sizin adınıza kullanın, korumak, depolamaz, önbellek, paylaşım, veya test, geliştirme, eğitim, dağıtma veya herhangi bir Microsoft dışı hizmeti kullanılabilir hale getirme amacıyla URL önizleme API'sinden herhangi bir veri dağıtmak veya özellik. 

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="data-attribution"></a>Veri attribution  

Proje yanıt arama yanıtları üçüncü taraflarca ait bilgileri içerir. Örneğin, kullanıcı deneyimi üzerinde dayanabileceği herhangi bir creative commons lisans ile uymak tarafından kullanımınız uygun olduğundan emin olmak için sorumlu.  
  
Bir yanıt veya sonuç içeriyorsa `contractualRules`, `attributions`, veya `provider` alanları, veri özniteliği gerekir. Yanıt bu alanlar içermiyorsa hiçbir attribution gereklidir. Yanıt içeriyorsa `contractualRules` alan ve `attributions` ve/veya `provider` alanları, veri özniteliği için sözleşme kurallarını kullanmalısınız.  
  
Aşağıdaki örnek, bir MediaAttribution sözleşme kural ve içeren bir görüntü içeren bir varlık gösterir bir `provider` alan. Resmin görmezden MediaAttribution kural resmi kuralın hedefi tanımlar. `provider` alan ve bunun yerine MediaAttribution kural attribution sağlamak için kullanın.  
  
```  
        "value" : [{
            "contractualRules" : [
                . . .
                {
                    "_type" : "ContractualRules\/MediaAttribution",
                    "targetPropertyName" : "image",
                    "mustBeCloseToContent" : true,
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Space_Needle"
                }
            ],
            . . .
            "image" : {
                "name" : "Space Needle",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A46378861201...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Space_Needle"
                }],
                "hostPageUrl" : "http:\/\/www.citydictionary.com\/Uploaded...",
                "width" : 110,
                "height" : 110
            },
            . . .
        }]
```  
  
Bir sözleşme kural içeriyorsa `targetPropertyName` alan, kural, yalnızca hedeflenen alan için geçerlidir. Aksi takdirde, kuralı içeren üst nesne uygular `contractualRules` alan.  
  
  
Aşağıdaki örnekte, `LinkAttribution` kuralını içeren `targetPropertyName` alan kuralın uygulanacağı şekilde `description` alan. Belirli alanlara uygulama kuralları için sağlayıcının Web sitesine köprü içeren hedeflenen verileri hemen ardından bir satır içermelidir. Bu durumda oluşturmak için sağlayıcının Web sitesinde veri köprü içeren açıklama metnini aşağıdaki hemen en.wikipedia.org bağlantı Örneğin, Açıklama özniteliği için bir satır ekleyin.  
  
```  
"entities" : {  
    "value" : [{  
            . . .  
            "description" : "Peyton Williams Manning is a former American....",  
            . . .  
            "contractualRules" : [{  
                    "_type" : "ContractualRules\/LinkAttribution",  
                    "targetPropertyName" : "description",  
                    "mustBeCloseToContent" : true,  
                    "text" : "en.wikipedia.org",  
                    "url" : "http:\/\/www.bing.com\/cr?IG=B8AD73..."  
                 },  
            . . .  
  
```  

### <a name="license-attribution"></a>Lisans Attribution  

Sözleşme kurallar listesinin içeriyorsa, bir [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) kural lisans uygulandığı içerik hemen ardından satırında bildirim görüntülemelidir. `LicenseAttribution` Kural kullanır `targetPropertyName` alan lisans uygulandığı özelliği tanımlayın.  
  
Aşağıdakileri içeren bir örnektir bir `LicenseAttribution` kuralı.  
  
![Lisans attribution](./media/licenseattribution.png)  
  
Görüntü lisans bildirim lisans bilgilerini içeren Web sitesinin köprü eklemeniz gerekir. Genellikle, lisans adını bir köprü oluşturun. Örneğin, bildirim ise **SA tarafından CC lisans altındaki metin** ve SA tarafından CC lisans adıdır, SA tarafından CC yapacağı köprü.  
  
### <a name="link-and-text-attribution"></a>Bağlantı ve metin Attribution  

[LinkAttribution](reference.md#linkattribution) ve [TextAttribution](reference.md#textattribution) kuralları genellikle veri sağlayıcısı tanımlamak için kullanılır. `targetPropertyName` Alan kuralın uygulanacağı alanı tanımlar.  
  
Sağlayıcılar özniteliği için hemen nitelikleri uygulama içeriği (örneğin, hedeflenen alana) aşağıdaki satırı ekleyin. Satır sağlayıcıları veri kaynağını olduğunu belirtmek için açıkça etiketli. Örneğin, "verilerden: en.wikipedia.org". İçin `LinkAttribution` kuralları için sağlayıcının Web sitesinde köprü oluşturmanız gerekir.  
  
Aşağıdakileri içeren bir örnektir `LinkAttribution` ve `TextAttribution` kuralları.  
  
![Bağlantı metni attribution](./media/linktextattribution.png)  

### <a name="media-attribution"></a>Medya Attribution  

Varlık bir görüntüsünü içerir ve onu görüntülemek, sağlayıcının Web sitesinde tıklatın aracılığıyla bağlantısını sağlamanız gerekir. Varlık içeriyorsa, bir [MediaAttribution](reference.md#mediaattribution) kural, kuralın URL'yi tıklatın bağlantıyı oluşturmak için kullanın. Görüntüye ait dahil URL kullanmayacak `provider` tıklatın bağlantıyı oluşturmak için alan.  
  
Aşağıdaki görüntünün içeren bir örnektir `provider` alan ve sözleşme kuralları. Örnek sözleşme kural içerdiğinden, görüntünün yoksayacak. `provider` alan ve geçerli `MediaAttribution` kuralı.  
  
![Medya attribution](./media/mediaattribution.png)  

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)
