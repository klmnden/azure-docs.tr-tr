---
title: Proje Yanıt Arama nedir?
titlesuffix: Azure Cognitive Services
description: Proje cevap arama giriş.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 87fe7b008e3e7c6cd8d1a9a870c0fb8ce2f6a7cd
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868267"
---
# <a name="what-is-project-answer-search"></a>Proje Yanıt Arama nedir?
Proje yanıt arama API'si Bing v7 uç nokta interrogative sorgularını yanıtlar almak için kullanır. "Dünya çevresi nedir?" A gibi soru bilgilerin ile bir yanıt döndürür.  Bir kişi, yer veya şey için bir sorgu, sorgu tarafından tanımlanan varlığı hakkındaki bilgileri döndürür. Bu senaryolar damıtarak konuşma bağlamında kullanılabilen botlar, Mesajlaşma uygulamaları, okuyucular, vb. gibi uygulamalarda yararlı olabilir.  

Sorguları bir sorgu senaryoya bağlıdır yanıtlarını döndürür: Web sayfalarının her zaman iade while [gerçekleri](fact-queries.md) ve/veya [varlıkları](entity-queries.md) uygunsa döndürülür.

## <a name="endpoint"></a>Uç Nokta
Soruların soru veya bir kişi, yer veya şey hakkında bilgi almak için yanıt arama API'si uç noktaya bir istek gönderin. Başlık ve URL parametrelerini çeşitli özelliklerini kullanın.  Dahil *Ocp-Apim-Subscription-Key* üstbilgisi geçerli bir belirteç ile.  Pazar parametresi gerekiyor. Yalnızca `en-us` market şu anda desteklenmiyor.

Aşağıdaki sorguda sorusunun yanıtlarını alır: "Dünya çevresi nedir?"

AL:
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=what+is+circumference+of+the=earth?&mkt=en-us

````

URL parametresi `q=` arama'nın belirtmek için gereklidir.

## <a name="response-object"></a>Yanıt nesnesi

Yanıt, HTTP üst bilgileri, Web sayfaları, olgular ve/veya varlıklar içeriyor.

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
Yanıt arama proje ve proje Video eğilimleri olan konusu [Bing arama kullanım ve görüntü gereksinimleri](use-display-requirements.md).

Veya sizin adınıza bir üçüncü taraf değil kullanın, korumak, depolamak, önbelleğe, paylaşın, test, geliştirme, eğitim, dağıtma veya Microsoft olmayan bir hizmette kullanılabilir hale getirme amacıyla URL önizleme API'sinden herhangi bir veri dağıtmak veya özellik. 

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="data-attribution"></a>Veri attribution  

Proje yanıt araması yanıtlarında, üçüncü taraflarca ait bilgileri içerir. Örneğin kullanıcı deneyiminizi ihtiyaç duyabilir herhangi creative commons lisansı ile uymak tarafından kullanımınız, uygun olduğundan emin olmak için sorumlu olursunuz.  
  
Bir yanıt veya sonuç içeriyorsa `contractualRules`, `attributions`, veya `provider` alanlar, veri özniteliği gerekir. Yanıtını bu alanlar içermiyorsa, hiçbir attribution gereklidir. Yanıt içeriyorsa `contractualRules` alan ve `attributions` ve/veya `provider` alanlar, veri özniteliği için sözleşmeye dayalı kurallar kullanmalısınız.  
  
Aşağıdaki örnek, bir MediaAttribution sözleşmeye dayalı kural ve içeren bir görüntü içeren bir varlık gösterir. bir `provider` alan. Görüntünün yoksay böylece MediaAttribution kural resmi, kuralın hedefi olarak tanımlar. `provider` alan ve MediaAttribution kural attribution sağlamak için kullanın.  
  
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
  
Sözleşmeye dayalı bir kural içeriyorsa `targetPropertyName` alan kuralı, yalnızca hedeflenen alana uygular. Aksi takdirde, kuralı içeren üst nesneye uygular `contractualRules` alan.  
  
  
Aşağıdaki örnekte, `LinkAttribution` kuralını içeren `targetPropertyName` alanında kuralın uygulanacağı şekilde `description` alan. Belirli alanlara uygulanan kuralları için sağlayıcının Web sitesinde bir köprü içeren hedeflenen verileri hemen izleyen bir çizgi içermesi gerekir. Bu durumda veri sağlayıcısı'nın Web sitesinde bir köprü içeren açıklama metnini aşağıdaki oluşturma hemen en.wikipedia.org bağlantısını örneğin Açıklama özniteliği için bir satır ekleyin.  
  
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

Sözleşmeye dayalı kurallar listesinin içeriyorsa bir [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) kural lisans uygulandığı içeriği takip satırında bildirim görüntülemelidir. `LicenseAttribution` Kural kullandığı `targetPropertyName` lisans uygulandığı özelliği tanımlamak için alan.  
  
Aşağıdakileri içeren bir örnek gösterir bir `LicenseAttribution` kuralı.  
  
![Lisans attribution](./media/licenseattribution.png)  
  
Görüntü lisans dikkat edin, köprü Lisansı hakkında bilgileri içeren bir Web sitesine eklemeniz gerekir. Genellikle, lisans adı bir köprü oluşturun. Örneğin dikkat edin, **SA tarafından CC lisansı altındaki metin** ve SA tarafından CC lisans adıdır, CC tarafından SA yapacağınız köprü.  
  
### <a name="link-and-text-attribution"></a>Bağlantı ve metin Attribution  

[LinkAttribution](reference.md#linkattribution) ve [TextAttribution](reference.md#textattribution) kuralları genellikle veri sağlayıcısı tanımlamak için kullanılır. `targetPropertyName` Kuralın uygulanacağı alanın alan tanımlar.  
  
Sağlayıcılar özniteliği atıfları geçerli içerik (örneğin, hedeflenen alana) takip bir satır ekleyin. Satır sağlayıcıları veri kaynağı olduğunu belirtmek için açıkça etiketlenmesi gereken. Örneğin, "veri: en.wikipedia.org". İçin `LinkAttribution` kuralları için sağlayıcının Web sitesinde bir köprü oluşturması gerekir.  
  
Aşağıdakileri içeren bir örnek gösterilmektedir `LinkAttribution` ve `TextAttribution` kuralları.  
  
![Bağlantı metni attribution](./media/linktextattribution.png)  

### <a name="media-attribution"></a>Medya Attribution  

Görüntü varlık içerir ve bunu görüntüleyebilir, sağlayıcının Web sitesi tıklama bağlantısını sağlamanız gerekir. Varlık içeriyorsa bir [MediaAttribution](reference.md#mediaattribution) kural, kuralın URL'si tıklama ile bağlantı oluşturmak için kullanın. Aksi takdirde, görüntü içinde bulunan URL'sini kullanarak `provider` tıklama ile bağlantı oluşturmak için alan.  
  
Aşağıdaki bir görüntü içeren bir örnek gösterilmektedir `provider` alan ve sözleşmeye dayalı kurallar. Örnek sözleşmeye dayalı kural içerdiğinden, görüntünün yoksayacak `provider` alan ve geçerli `MediaAttribution` kuralı.  
  
![Medya attribution](./media/mediaattribution.png)  

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](c-sharp-quickstart.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)
