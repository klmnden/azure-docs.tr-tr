---
title: Bing Varlık Arama nedir? | Microsoft Docs
description: Bing varlık arama API'si, varlıkları ve basamak için Web'de arama yapmak için kullanmayı öğrenin.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 0B54E747-61BF-42AA-8788-E25D63F625FC
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2016
ms.author: scottwhi
ms.openlocfilehash: 275430bc6ee8f935978243e61f68713974648189
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008119"
---
# <a name="what-is-bing-entity-search"></a>Bing Varlık Arama nedir?

Bing varlık arama API'si, Bing arama sorgusu gönderir ve varlıkları ve basamak içeren sonuçları alır. Bir yerde sonuçları Restoran, otel veya diğer yerel işletmeler içerir. Bing sorgu yerel iş adını belirtir veya bir tür bir iş (örneğin, Yakınımdaki restoranlar) ister yerler döndürür. Sorgu iyi bilinen kişiler, yerler (turistik yerleri, durumları, ülke, vb.) veya şeyler belirtiyorsa Bing varlıkları döndürür.

## <a name="suggesting--using-search-terms"></a>Arama terimlerini önerme ve kullanma

Kullanıcıların arama terimlerini gireceği bir arama kutusu sağlıyorsanız deneyimi geliştirmek için [Bing Otomatik Öneri API'sini](../bing-autosuggest/get-suggested-search-terms.md) kullanın. API, kullanıcı yazarken kısmi arama terimlerine dayalı önerilen sorgu dizelerini yönetin.

Kullanıcı arama terimini girdikten sonra [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) sorgu parametresini ayarlamadan önce terimi URL ile kodlayın. Örneğin, kullanıcının girdiği *Marcus Appel*ayarlayın `q` için *Marcus + Appel* veya *Marcus % 20Appel*.

Arama terimi yazım hatası içeriyorsa, arama yanıt içeren bir [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) nesne. Nesne, özgün yazım ve Bing arama için kullanılan düzeltilmiş yazım denetimi gösterir.

```json
"queryContext": {
    "originalQuery": "hollo wrld",
    "alteredQuery": "hello world",
    "alterationOverrideQuery": "+hollo wrld",
    "adultIntent": false
}
```

## <a name="requesting-entities"></a>İstekte bulunan varlık

Bir örnek istek için bkz: [ilk istekte](./quick-start.md).

## <a name="the-response"></a>Yanıt

Yanıt içeren bir [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse) nesne. Bing varlık veya ilgili bir yerde bulursa, nesneyi içeren `entities` alan `places` alan veya her ikisi de. Aksi takdirde, yanıt nesnesini ya da alanı içermez.
> [!NOTE]
> Varlık yanıtları birden fazla Pazarı desteklese de, yerler yanıt yalnızca ABD iş konumları destekler. 

`entities` Alan bir [EntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entityanswer) listesini içeren nesne [varlık](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity) nesneleri (bkz `value` alan). Listeden tek bir baskın varlık, birden çok Kesinleştirme varlıkları veya her ikisi de içerebilir. 

Baskın varlık olduğundan istek karşılayan yalnızca varlık Bing düşündüğü bir varlık (varlık için istek karşılayan bir belirsizlik olmaz yoktur). Birden çok varlık isteği karşılamak, listeyi birden fazla Kesinleştirme varlık içerir. Örneğin, istek bir film franchise genel başlığı kullanıyorsa, büyük olasılıkla liste Kesinleştirme varlıkları içerir. Ancak, isteği franchise belirli bir başlığı belirtiyorsa, büyük olasılıkla listesi tek bir baskın varlık içerir.

İyi bilinen inancı singers, aktör, atletlerine, modelleri vb. gibi varlıkları içerir.; basamakları ve bağlama Sunucu1 veya Lincoln Anma gibi yer işareti; ve Muz, goldendoodle, kitap veya film başlık gibi şeyler. [EntityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) alan varlığın türü tanımlayan ipuçları içerir. Örneğin, bir kişi, film, donatarak veya çekim ise. Olası türlerinin bir listesi için bkz. [varlık türleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "DominantEntity",
    "entityTypeHints": ["Attraction"],
    "entityTypeDisplayHint": "Mountain"
}, ...
```

Baskın ve Kesinleştirme bir varlık içeren bir yanıt aşağıda gösterilmiştir.

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

Varlık içeren bir `name`, `description`, ve `image` alan. Bu alanlar kullanıcı deneyiminizi görüntülediğinizde, bunları özniteliği gerekir. `contractualRules` Alan uygulamalısınız özniteliklerinin bir listesini içerir. Sözleşme kural attribution uygulandığı alanın tanımlar. Attribution uygulama hakkında daha fazla bilgi için bkz: [Attribution](#data-attribution).

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

Varlık bilgilerini (ad, açıklama ve resim) görüntülediğinizde, URL'de de kullanmanız gerekir `webSearchUrl` Bing arama bağlamak için alan sonuçları varlığı içeren sayfa.


`places` Alan bir [LocalEntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#localentityanswer) listesini içeren nesne [yerde](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#place) nesneleri (bkz `value` alan). Liste, istek uygun bir veya daha fazla yerel varlık içerir.

Basamak Restoran, otel ve yerel işletmeler içerir. [EntityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) alan yerel varlığın türü tanımlayan ipuçları içerir. Listesi bir yerde, LocalBusiness, Restoran ipuçları içerir. Art arda gelen her ipucu dizisinde, varlığın türü daraltır. Olası türlerinin bir listesi için bkz. [varlık türleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "ListItem",
    "entityTypeHints": ["Place",
    "LocalBusiness",
    "Restaurant"]
}, ...
```
> [!NOTE]
> Varlık yanıtları birden fazla Pazarı desteklese de, yerler yanıt yalnızca ABD iş konumları destekler. 

Yerel kullanan varlık gibi sorguların *Restoran Yakınımda* doğru sonuçlar sağlamak için kullanıcının konumuna gerektirir. İsteklerinizi her zaman, kullanıcının konumunu belirtmek için X MSEdge Clientıp üstbilgileri ve X arama konumunu kullanmalısınız. Bing sorgu kullanıcının konumdan yararlı gördüğü değilse, ayarlar `askUserForLocation` alanını [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) için **true**. 

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

Bir yerde sonuç varlığın Web sitesine konumunun ad, adres, telefon numarası ve URL içerir. Varlık bilgilerini görüntülediğinizde de URL'yi kullanmanız gerekir `webSearchUrl` Bing arama bağlamak için alan sonuçları varlığı içeren sayfa.

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
> Veya sizin adınıza bir üçüncü taraf değil kullanın, korumak, depolamak, önbellek, paylaşın, herhangi bir veri varlıkları API'sinden test, geliştirme, eğitim, dağıtma veya Microsoft olmayan bir hizmette kullanılabilir hale getirme amacıyla dağıtmak veya özellik.  

## <a name="data-attribution"></a>Veri attribution

Bing varlık API yanıtları, üçüncü taraflarca ait bilgileri içerir. Örneğin kullanıcı deneyiminizi ihtiyaç duyabilir herhangi creative commons lisansı ile uymak tarafından kullanımınız, uygun olduğundan emin olmak için sorumlu olursunuz.

Bir yanıt veya sonuç içeriyorsa `contractualRules`, `attributions`, veya `provider` alanlar, veri özniteliği gerekir. Yanıtını bu alanlar içermiyorsa, hiçbir attribution gereklidir. Yanıt içeriyorsa `contractualRules` alan ve `attributions` ve/veya `provider` alanlar, veri özniteliği için sözleşmeye dayalı kurallar kullanmalısınız.

Aşağıdaki örnek, bir MediaAttribution sözleşmeye dayalı kural ve içeren bir görüntü içeren bir varlık gösterir. bir `provider` alan. Görüntünün yoksay böylece MediaAttribution kural resmi, kuralın hedefi olarak tanımlar. `provider` alan ve MediaAttribution kural attribution sağlamak için kullanın.  

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

Sözleşmeye dayalı bir kural içeriyorsa `targetPropertyName` alan kuralı, yalnızca hedeflenen alana uygular. Aksi takdirde, kuralı içeren üst nesneye uygular `contractualRules` alan.

Aşağıdaki örnekte, `LinkAttribution` kuralını içeren `targetPropertyName` alanında kuralın uygulanacağı şekilde `description` alan. Belirli alanlara uygulanan kuralları için sağlayıcının Web sitesinde bir köprü içeren hedeflenen verileri hemen izleyen bir çizgi içermesi gerekir. Bu durumda veri sağlayıcısı'nın Web sitesinde bir köprü içeren açıklama metnini aşağıdaki oluşturma hemen contoso.com için bir bağlantı gibi Açıklama özniteliği için bir satır ekleyin.

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

Sözleşmeye dayalı kurallar listesinin içeriyorsa bir [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) kural lisans uygulandığı içeriği takip satırında bildirim görüntülemelidir. `LicenseAttribution` Kural kullandığı `targetPropertyName` lisans uygulandığı özelliği tanımlamak için alan.

Aşağıdakileri içeren bir örnek gösterir bir `LicenseAttribution` kuralı.

![Lisans attribution](./media/cognitive-services-bing-entities-api/licenseattribution.png)

Görüntü lisans dikkat edin, köprü Lisansı hakkında bilgileri içeren bir Web sitesine eklemeniz gerekir. Genellikle, lisans adı bir köprü oluşturun. Örneğin dikkat edin, **SA tarafından CC lisansı altındaki metin** ve SA tarafından CC lisans adıdır, CC tarafından SA yapacağınız köprü.

### <a name="link-and-text-attribution"></a>Bağlantı ve metin attribution

[LinkAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#linkattribution) ve [TextAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#textattribution) kuralları genellikle veri sağlayıcısı tanımlamak için kullanılır. `targetPropertyName` Kuralın uygulanacağı alanın alan tanımlar.

Sağlayıcılar özniteliği atıfları geçerli içerik (örneğin, hedeflenen alana) takip bir satır ekleyin. Satır sağlayıcıları veri kaynağı olduğunu belirtmek için açıkça etiketlenmesi gereken. Örneğin, "veri: contoso.com". İçin `LinkAttribution` kuralları için sağlayıcının Web sitesinde bir köprü oluşturması gerekir.

Aşağıdakileri içeren bir örnek gösterilmektedir `LinkAttribution` ve `TextAttribution` kuralları.

![Bağlantı metni attribution](./media/cognitive-services-bing-entities-api/linktextattribution.png)

### <a name="media-attribution"></a>Medya attribution

Görüntü varlık içerir ve bunu görüntüleyebilir, sağlayıcının Web sitesi tıklama bağlantısını sağlamanız gerekir. Varlık içeriyorsa bir [MediaAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mediaattribution) kural, kuralın URL'si tıklama ile bağlantı oluşturmak için kullanın. Aksi takdirde, görüntü içinde bulunan URL'sini kullanarak `provider` tıklama ile bağlantı oluşturmak için alan.

Aşağıdaki bir görüntü içeren bir örnek gösterilmektedir `provider` alan ve sözleşmeye dayalı kurallar. Örnek sözleşmeye dayalı kural içerdiğinden, görüntünün Yoksay `provider` alan ve geçerli `MediaAttribution` kuralı.

![Medya attribution](./media/cognitive-services-bing-entities-api/mediaattribution.png)

### <a name="search-or-search-like-experience"></a>Arama veya arama deneyimini

Gibi ile Bing Web araması API'si, Bing varlık arama API'si yalnızca doğrudan kullanıcı sorgusu veya arama sonucunda ya da bir uygulama veya mantıksal olarak bir kullanıcının arama talep yorumlanabilir deneyimi içinde bir eylem sonucu olarak kullanılabilir. Gösterim amacıyla, kabul edilebilir bir arama ya da benzer arama deneyimleri bazı örnekleri şunlardır:

- Kullanıcı, doğrudan bir uygulamada arama kutusuna bir sorgu girer.
- Kullanıcı belirli bir metin veya görüntü ve istekleri "daha fazla bilgi" veya "ek bilgileri" seçer.
- Kullanıcı Arama bot hakkında belirli bir konu ister.
- Kullanıcı belirli bir nesne veya bir görsel arama türü senaryosunda varlık dwells

Deneyiminizi bir arama deneyimini kabul edilebilir olup olmadığından emin değilseniz, Microsoft ile kontrol etmenizi öneririz.

## <a name="throttling-requests"></a>İstekleri azaltma

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi hızlıca başlamak için bkz: [yapmadan bilgisayarınızı ilk istek](./quick-start.md).

İle kendinizi alıştırın [Bing varlık arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) başvuru. Başvuru arama sonuçları istemek için kullandığınız sorgu parametreleri ve üst bilgileri içerir. Ayrıca yanıt nesnelerinin tanımları da bulunur. 

Arama kutusu kullanıcı deneyiminizi geliştirmek için bkz. [Bing Otomatik Öneri API'si](../bing-autosuggest/get-suggested-search-terms.md). Kullanıcı sorgu terimini girerken bu API'yi çağırarak başkaları tarafından kullanılan ilgili sorgu terimlerini alabilirsiniz.

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./use-display-requirements.md)'ni okumayı unutmayın.