---
title: Bing varlık arama nedir? | Microsoft Docs
description: Bing varlık arama API varlıkları ve yerler Web'de aramak için nasıl kullanılacağını öğrenin.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 0B54E747-61BF-42AA-8788-E25D63F625FC
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2016
ms.author: scottwhi
ms.openlocfilehash: f1b87c07d5b56307fd6b3fc68999598aeab6eb82
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354898"
---
# <a name="what-is-bing-entity-search"></a>Bing varlık arama nedir?

Bing varlık arama API için Bing arama sorgusu gönderir ve varlıkları ve basamak içeren sonuçları alır. Yerleştir sonuçları Restoran, otel veya diğer yerel işletmeler içerir. Sorgu yerel iş adını belirtir ya da bir tür bir iş (örneğin, Restoran bana yakın) ister Bing yerler döndürür. Sorgu iyi bilinen kişiler, yerler (turistik yerleri, durumları, ülke, vb.) veya şeyler belirtiyorsa Bing varlıklar döndürüyor.

## <a name="suggesting--using-search-terms"></a>Öneren & arama terimleri kullanma

Burada kullanıcının girdiği kendi arama terimi bir arama kutusu sağlayan kullanırsanız [Bing otomatik öneri API](../bing-autosuggest/get-suggested-search-terms.md) deneyimini iyileştirmek üzere. API kısmi arama terimleri kullanıcı türleri olarak dizeleri dayalı önerilen sorgu döndürür.

Kullanıcı kendi arama terimi girdikten sonra URL kodlama ayarlamadan önce terim [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) sorgu parametresi. Örneğin, kullanıcının girdiği *Marcus Appel*ayarlayın `q` için *Marcus + Appel* veya *Marcus % 20Appel*.

Arama terimi yazım hata içeriyorsa, arama yanıtı içeren bir [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) nesnesi. Nesne özgün yazım ve Bing arama için kullanılan düzeltilmiş yazım gösterir.

```json
"queryContext": {
    "originalQuery": "hollo wrld",
    "alteredQuery": "hello world",
    "alterationOverrideQuery": "+hollo wrld",
    "adultIntent": false
}
```

## <a name="requesting-entities"></a>İstekte bulunan varlık

Bir örnek isteği için bkz: [ilk isteği yapan](./quick-start.md).

## <a name="the-response"></a>Yanıt

Yanıtı içeren bir [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse) nesnesi. Bing bir varlık veya ilgili yere bulursa, nesneyi içeren `entities` alanı `places` alan veya her ikisi de. Aksi takdirde, yanıt nesnesi ya da alan içermez.

`entities` Alan bir [EntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entityanswer) listesini içeren nesne [varlık](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity) nesneleri (bkz `value` alan). Liste, tek bir baskın varlık, birden çok Kesinleştirme varlık veya her ikisini içerebilir. 

Bir baskın varlıktır Bing istek karşılayan yalnızca varlık olduğunu düşündüğünü bir varlık (varlık için istek karşılayan belirsizlik yoktur). Birden çok varlık isteği karşılamak, liste birden fazla Kesinleştirme varlık içerir. Örneğin, istek film Acentelik genel başlığı kullanıyorsa, büyük olasılıkla listesi Kesinleştirme varlıkları içerir. Ancak, isteği Acentelik belirli başlıktan belirtiyorsa, büyük olasılıkla listesi tek bir baskın varlık içeriyor.

Varlıkları içerir singers, aktörler, sporcu, modelleri vb. gibi bilinen kullanıcının kişisel özelliklerine.; yerler ve bağlama Sunucu1 veya Lincoln Anma gibi bilinen yerler; ve Muz, goldendoodle, kitap veya film başlık gibi şeyler. [EntityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) alan varlığın tür belirleme ipuçları içerir. Örneğin, bir kişinin, film, hayvan ya da çekim ise. Olası türlerinin listesi için bkz: [varlık türleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "DominantEntity",
    "entityTypeHints": ["Attraction"],
    "entityTypeDisplayHint": "Mountain"
}, ...
```

Baskın ve Kesinleştirme varlığı içeren bir yanıtı gösterir.

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Mount Rainier"
    },
    "entities": {
        "value": [{
            "contractualRules": [{
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
                "text": "contoso.com",
                "url": "http://contoso.com/mount_rainier"
            },
            {
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount-rainier"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier...",
            "name": "Mount Rainier",
            "url": "http://www.northwindtraders.com/",
            "image": {
                "name": "Mount Rainier",
                "thumbnailUrl": "https://www.bing.com/th?id=A4ae343983daa4...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier"
                }],
                "hostPageUrl": "http://contoso.com/commons/7/72/mount_rain...",
                "width": 110,
                "height": 110
            },
            "description": "Mount Rainier is 14,411 ft tall and the highest mountain...",
            "entityPresentationInfo": {
                "entityScenario": "DominantEntity",
                "entityTypeHints": ["Attraction"]
            },
            "bingId": "38b9431e-cf91-93be-0584-c42a3ecbfdc7"
        },
        {
            "contractualRules": [{
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount_rainier_national_park"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier%20National...",
            "name": "Mount Rainier National Park",
            "url": "http://worldwideimporters.com/",
            "image": {
                "name": "Mount Rainier National Park",
                "thumbnailUrl": "https://www.bing.com/th?id=A91bdc5a1b648a695a39...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier_national_park"
                }],
                "hostPageUrl": "http://contoso.com/en/7/7a...",
                "width": 50,
                "height": 50
            },
            "description": "Mount Rainier National Park is a United States National Park...",
            "entityPresentationInfo": {
                "entityScenario": "DisambiguationItem",
                "entityTypeHints": ["Organization"]
            },
            "bingId": "29d4b681-227a-3924-7bb1-8a54e8666b8c"
        }]
    }
}
```

Varlık içeren bir `name`, `description`, ve `image` alan. Kullanıcı deneyiminiz bu alanlar görüntülediğinizde, bunları öznitelik gerekir. `contractualRules` Alan uygulamalısınız nitelikleri listesini içerir. Sözleşme kural attribution uygulandığı alan tanımlar. Attribution uygulama hakkında daha fazla bilgi için bkz: [Attribution](#data-attribution).

```json
"contractualRules": [{
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
    "text": "contoso.com",
    "url": "http://contoso.com/wiki/Mount_Rainier"
},
{
    "_type": "ContractualRules/MediaAttribution",
    "targetPropertyName": "image",
    "mustBeCloseToContent": true,
    "url": "http://contoso.com/wiki/Mount_Rainier"
}], ...
```

Varlık bilgileri (ad, açıklama ve görüntü) görüntülediğinizde, URL'de de kullanmanız gerekir `webSearchUrl` Bing arama bağlamak için alan sonuçları varlığı içeren sayfa.


`places` Alan bir [LocalEntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#localentityanswer) listesini içeren nesne [yer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#place) nesneleri (bkz `value` alan). Liste isteği karşılamak bir veya daha fazla yerel varlıkları içerir.

Yerler Restoran, Oteller veya yerel işletmeler içerir. [EntityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) alan yerel varlığın tür belirleme ipuçları içerir. Liste Yerleştir, LocalBusiness, Restoran gibi ipucu listesi içerir. Dizideki her art arda ipucu varlığın türü daraltır. Olası türlerinin listesi için bkz: [varlık türleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "ListItem",
    "entityTypeHints": ["Place",
    "LocalBusiness",
    "Restaurant"]
}, ...
```

Yerel odaklı varlığın sorgular gibi *bana yakın Restoran* doğru sonuçlar sağlamak için kullanıcının konumunu gerektirir. İsteklerinizi, kullanıcının konumunu belirtmek için X arama konumunu ve X MSEdge ClientIP üstbilgileri her zaman kullanmalısınız. Sorgu, kullanıcının konumdan yararlı Bing düşündüğü, ayarlar `askUserForLocation` alanını [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) için **doğru**. 

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Sinful Bakery and Cafe",
        "askUserForLocation": true
    },
    ...
}
```

Bir yerde sonuç varlığın Web sitesine konumunun ad, adres, telefon numarası ve URL içerir. Varlık bilgilerini görüntülediğinizde de URL'de kullanmalısınız `webSearchUrl` Bing arama bağlamak için alan sonuçları varlığı içeren sayfa.

```json
"places": {
    "value": [{
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Sinful%20Bakery...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "http://libertysdelightfulsinfulbakeryandcafe.com/",
        "entityPresentationInfo": {
            "entityScenario": "ListItem",
            "entityTypeHints": ["Place",
            "LocalBusiness",
            "Restaurant"]
        },
        "address": {
            "addressLocality": "Seattle",
            "addressRegion": "WA",
            "postalCode": "98112",
            "addressCountry": "US",
            "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
    }]
}
```

> [!NOTE]
> Siz veya bir üçüncü taraf sizin adınıza kullanın, korumak, depolamaz, önbellek, paylaşım, veya test, geliştirme, eğitim, dağıtma veya herhangi bir Microsoft dışı hizmeti kullanılabilir hale getirme amacıyla varlıklar API'sinden herhangi bir veri dağıtmak veya özellik.  

## <a name="data-attribution"></a>Veri attribution

Bing varlık API yanıtları üçüncü taraflarca ait bilgileri içerir. Örneğin, kullanıcı deneyimi üzerinde dayanabileceği herhangi bir creative commons lisans ile uymak tarafından kullanımınız uygun olduğundan emin olmak için sorumlu.

Bir yanıt veya sonuç içeriyorsa `contractualRules`, `attributions`, veya `provider` alanları, veri özniteliği gerekir. Yanıt bu alanlar içermiyorsa hiçbir attribution gereklidir. Yanıt içeriyorsa `contractualRules` alan ve `attributions` ve/veya `provider` alanları, veri özniteliği için sözleşme kurallarını kullanmalısınız.

Aşağıdaki örnek, bir MediaAttribution sözleşme kural ve içeren bir görüntü içeren bir varlık gösterir bir `provider` alan. Resmin görmezden MediaAttribution kural resmi kuralın hedefi tanımlar. `provider` alan ve bunun yerine MediaAttribution kural attribution sağlamak için kullanın.  

```json
"value": [{
    "contractualRules": [
        ...
        {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://contoso.com/mount_rainier"
        }
    ],
    ...
    "image": {
        "name": "Mount Rainier",
        "thumbnailUrl": "https://www.bing.com/th?id=A46378861201...",
        "provider": [{
            "_type": "Organization",
            "url": "http://contoso.com/mount_rainier"
        }],
        "hostPageUrl": "http://www.graphicdesigninstitute.com/Uploaded...",
        "width": 110,
        "height": 110
    },
    ...
}]
```

Bir sözleşme kural içeriyorsa `targetPropertyName` alan, kural, yalnızca hedeflenen alan için geçerlidir. Aksi takdirde, kuralı içeren üst nesne uygular `contractualRules` alan.

Aşağıdaki örnekte, `LinkAttribution` kuralını içeren `targetPropertyName` alan kuralın uygulanacağı şekilde `description` alan. Belirli alanlara uygulama kuralları için sağlayıcının Web sitesine köprü içeren hedeflenen verileri hemen ardından bir satır içermelidir. Bu durumda oluşturmak için sağlayıcının Web sitesinde veri köprü içeren açıklama metnini aşağıdaki hemen contoso.com bağlantı Örneğin, Açıklama özniteliği için bir satır ekleyin.

```json
"entities": {
    "value": [{
            ...
            "description": "Marcus Appel is a former American....",
            ...
            "contractualRules": [{
                    "_type": "ContractualRules/LinkAttribution",
                    "targetPropertyName": "description",
                    "mustBeCloseToContent": true,
                    "text": "contoso.com",
                    "url": "http://contoso.com/cr?IG=B8AD73..."
                 },
            ...
  
```

### <a name="license-attribution"></a>Lisans attribution

Sözleşme kurallar listesinin içeriyorsa, bir [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) kural lisans uygulandığı içerik hemen ardından satırında bildirim görüntülemelidir. `LicenseAttribution` Kural kullanır `targetPropertyName` alan lisans uygulandığı özelliği tanımlayın.

Aşağıdakileri içeren bir örnektir bir `LicenseAttribution` kuralı.

![Lisans attribution](./media/cognitive-services-bing-entities-api/licenseattribution.png)

Görüntü lisans bildirim lisans bilgilerini içeren Web sitesinin köprü eklemeniz gerekir. Genellikle, lisans adını bir köprü oluşturun. Örneğin, bildirim ise **SA tarafından CC lisans altındaki metin** ve SA tarafından CC lisans adıdır, SA tarafından CC yapacağı köprü.

### <a name="link-and-text-attribution"></a>Bağlantı ve metin attribution

[LinkAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#linkattribution) ve [TextAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#textattribution) kuralları genellikle veri sağlayıcısı tanımlamak için kullanılır. `targetPropertyName` Alan kuralın uygulanacağı alanı tanımlar.

Sağlayıcılar özniteliği için hemen nitelikleri uygulama içeriği (örneğin, hedeflenen alana) aşağıdaki satırı ekleyin. Satır sağlayıcıları veri kaynağını olduğunu belirtmek için açıkça etiketli. Örneğin, "verilerden: contoso.com". İçin `LinkAttribution` kuralları için sağlayıcının Web sitesinde köprü oluşturmanız gerekir.

Aşağıdakileri içeren bir örnektir `LinkAttribution` ve `TextAttribution` kuralları.

![Bağlantı metni attribution](./media/cognitive-services-bing-entities-api/linktextattribution.png)

### <a name="media-attribution"></a>Medya attribution

Varlık bir görüntüsünü içerir ve onu görüntülemek, sağlayıcının Web sitesinde tıklatın aracılığıyla bağlantısını sağlamanız gerekir. Varlık içeriyorsa, bir [MediaAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mediaattribution) kural, kuralın URL'yi tıklatın bağlantıyı oluşturmak için kullanın. Görüntüye ait dahil URL kullanmayacak `provider` tıklatın bağlantıyı oluşturmak için alan.

Aşağıdaki görüntünün içeren bir örnektir `provider` alan ve sözleşme kuralları. Örnek sözleşme kural içerdiğinden, görüntünün Yoksay `provider` alan ve geçerli `MediaAttribution` kuralı.

![Medya attribution](./media/cognitive-services-bing-entities-api/mediaattribution.png)

### <a name="search-or-search-like-experience"></a>Arama veya arama benzer deneyim

Gibi Bing Web arama API'si ile Bing varlık arama API yalnızca doğrudan kullanıcı sorgu veya arama sonucunda ya da bir uygulama ya da mantıksal bir kullanıcının arama isteği yorumlanabilen deneyimi içinde bir eylem sonucu olarak kullanılabilir. Gösterim amaçları için kabul edilebilir arama veya arama benzeri deneyimleri bazı örnekleri verilmiştir.

- Kullanıcı, bir uygulama Ara kutusuna doğrudan bir sorgu girer.
- Belirli bir metin veya görüntü ve istekleri "daha fazla bilgi" veya "ek bilgiler" kullanıcı seçer
- Kullanıcı bir arama bot ile ilgili belirli bir konu sorar.
- Belirli nesne veya varlık visual arama türü senaryoda kullanıcı dwells

Deneyiminizi bir arama deneyimini kabul edilebilir değilse emin değilseniz, Microsoft ile denetleyin önerilir.

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

İlk isteklerinize hızlı bir şekilde başlamak için bkz: [yapmadan bilgisayarınızı ilk istek](./quick-start.md).

İle öğrenmeniz [Bing varlık arama API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) başvuru. Üstbilgiler ve arama sonuçlarında istek için kullandığınız sorgu parametreleri başvurusu içeriyor. Ayrıca yanıt nesnelerin tanımları içerir. 

Arama kutusuna kullanıcı deneyimini artırmak için bkz: [Bing otomatik öneri API](../bing-autosuggest/get-suggested-search-terms.md). Kendi sorgu Terime kullanıcının girdiği gibi başkaları tarafından kullanılan ilgili sorgu terimlerinin almak için bu API çağırabilir.

Okuduğunuzdan emin olun [Bing kullanın ve görüntü gereksinimleri](./use-display-requirements.md) arama sonuçlarını kullanma hakkında kurallardan herhangi birinin kesmeyin şekilde.