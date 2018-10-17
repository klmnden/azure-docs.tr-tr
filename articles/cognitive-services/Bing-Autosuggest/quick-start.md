---
title: "Hızlı başlangıç: Bing Otomatik Öneri API'si"
titlesuffix: Azure Cognitive Services
description: Bing Otomatik Öneri API'sini kullanmaya başlamayı göstermektedir.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: quickstart
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 446d271854a4e45bcea8c261a0dc078e549f8229
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830762"
---
# <a name="quickstart-making-your-first-autosuggest-query"></a>Hızlı başlangıç: İlk Otomatik Öneri sorgunuzu oluşturma

İlk çağrınızı yapmadan önce bir Bilişsel Hizmetler abonelik anahtarı almanız gerekir. Anahtar alma için bkz: [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=autosuggest-api).

Web araması sonuçlarını almak için aşağıdaki uç noktaya bir GET isteği gönderirsiniz:

```http
https://api.cognitive.microsoft.com/bing/v5.0/Suggestions
```

> [!NOTE]
> V7 Önizleme uç noktası:
>
> ```http
> https://api.cognitive.microsoft.com/bing/v7.0/Suggestions
> ```

İstek, HTTPS protokolünü kullanmalıdır.

Tüm isteklerin bir sunucudan gönderilmesini öneririz. Anahtarı bir istemci uygulamanın parçası olarak dağıtmak, kötü amaçlı bir üçüncü tarafa anahtara erişmek için daha fazla fırsat sunar. Ayrıca bir sunucudan çağrı yapmak API'nin gelecek sürümleri için tek bir yükseltme noktası sağlar.

İstek kullanıcının kısmi arama terimini içeren [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query) sorgu parametresini belirtmelidir. Tercihe bağlı olmakla birlikte, istek, sonuçların gelmesini istediğiniz pazarı tanımlayan [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt) sorgu parametresini de belirtmelidir. İsteğe bağlı sorgu parametrelerinin bir listesi için bkz. [Sorgu Parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters). Tüm sorgu parametre değerleri URL olarak kodlanmış olmalıdır.

İstek [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey) üstbilgisini belirtmelidir. İsteğe bağlı olmakla birlikte şu üstbilgileri de belirtmeniz önerilir:

- [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

İstemci IP'si ve konum üstbilgileri konuma duyarlı içerik döndürmek için önemlidir.

Tüm istek ve yanıt üstbilgilerinin bir listesi için bkz. [Üstbilgiler](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

## <a name="the-request"></a>İstek

İstek, önerilen sorgu parametrelerinin ve üst bilgilerin tamamını içermelidir. Kullanıcı arama kutusuna her yeni karakter yazdığında bu API’yi çağırırsınız. Sorgu dizesinin eksiksiz olması, API’nin döndürdüğü önerilen sorgu terimlerinin alakasını etkiler. Sorgu dizesi ne kadar eksiksizse, önerilen sorgu terimlerinin listesi de o kadar alakalıdır. Örneğin, API’nin *s* için döndürebileceği öneriler, *sailing dinghies* için döndürdüğü sorgulardan daha az alakalıdır. 

Aşağıdaki örnekte, *sail* için önerilen sorgu dizelerini döndüren bir istek gösterilmektedir.

```http
GET https://api.cognitive.microsoft.com/bing/v5.0/suggestions?q=sail&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

> [!NOTE]
> V7 Önizleme isteği:

> ```http
> GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.cognitive.microsoft.com
> ```

Bing API'lerinden birini ilk kez çağırıyorsanız istemci kimliği üst bilgisini eklemeyin. İstemci kimliği üst bilgisini yalnızca önceden bir Bing API'sini çağırdıysanız ve Bing, kullanıcı ve cihaz birleşimi için bir istemci kimliği döndürdüyse dahil edin.

Aşağıdaki kod önceki isteğin yanıtını göstermektedir. Yanıtta arama sorgusu önerilerinin listesini içeren bir web önerisi grubu bulunmaktadır. Her öneri `displayText`, `query` ve `url` alanlarını içerir.

`displayText` alanı, arama kutunuzun açılır listesini doldurmak için kullandığınız önerilen sorguyu içerir. Yanıtın içerdiği tüm önerileri, verilen sırada görüntülemeniz gerekir.  

Kullanıcı, açılır listeden bir sorgu seçerse, `query` alanındaki sorgu dizesini kullanarak [Bing Arama API’sini](../bing-web-search/search-the-web.md) çağırır ve sonuçları kendiniz görüntülersiniz. Veya kullanıcıyı Bing arama sonuçları sayfasına göndermek için `url` alanındaki URL’yi kullanabilirsiniz.

```  
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Suggestions",
    "queryContext" : {
        "originalQuery" : "sail"
    },
    "suggestionGroups" : [{
        "name" : "Web",
        "searchSuggestions" : [{
            "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
            "displayText" : "sailing lessons seattle",
            "query" : "sailing lessons seattle",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+moon+news&FORM=USBAPI",
            "displayText" : "sailor moon news",
            "query" : "sailor moon news",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+jack%27s+lincoln+city&FORM=USBAPI",
            "displayText" : "sailor jack's lincoln city",
            "query" : "sailor jack's lincoln city",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailing+anarchy&FORM=USBAPI",
            "displayText" : "sailing anarchy",
            "query" : "sailing anarchy",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale&FORM=USBAPI",
            "displayText" : "sailboats for sale",
            "query" : "sailboats for sale",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailstn.mylabsplus.com&FORM=USBAPI",
            "displayText" : "sailstn.mylabsplus.com",
            "query" : "sailstn.mylabsplus.com",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailusfood&FORM=USBAPI",
            "displayText" : "sailusfood",
            "query" : "sailusfood",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale+seattle&FORM=USBAPI",
            "displayText" : "sailboats for sale seattle",
            "query" : "sailboats for sale seattle",
            "searchKind" : "WebSearch"
        }]
    }]
}
```

## <a name="next-steps"></a>Sonraki adımlar

API'yi deneyin. [Otomatik Öneri API'si Test Konsolu](https://dev.cognitive.microsoft.com/docs/services/56c7694ecf5ff801a090fbd1/operations/56c769a2cf5ff801a090fbd2)'na gidin.

Yanıt nesnelerini kullanmanın ayrıntıları için bkz. [Önerilen Arama Terimlerini Alma](./get-suggested-search-terms.md).
