---
title: API quickstart otomatik öneri | Microsoft Docs
description: Bing otomatik öneri API'si ile çalışmaya başlamak gösterilmiştir.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 1482E781-7352-4A3F-B1D5-B896381348C4
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: a7b54a1fb0b7c76eb72097357a6b51aa02e6e2fd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354580"
---
# <a name="making-your-first-autosuggest-query"></a>İlk Autosuggest sorgunuzu yapma

İlk aramanız yapabilmeniz için önce bir Bilişsel hizmetler abonelik anahtarı almanız gerekir. Bir anahtar almak için bkz: [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=autosuggest-api).

Web arama sonuçları almak için aşağıdaki uç noktaya bir GET isteği gönder:

```http
https://api.cognitive.microsoft.com/bing/v5.0/Suggestions
```

> [!NOTE]
> V7 Önizleme uç noktası:
>
> ```http
> https://api.cognitive.microsoft.com/bing/v7.0/Suggestions
> ```

İstek HTTPS protokolünü kullanmanız gerekir.

Tüm istekleri bir sunucusundan kaynaklanan öneririz. Bir istemci uygulaması bir parçası olarak anahtar dağıtma bir kötü amaçlı üçüncü erişmek taraf için daha fazla fırsatı sağlar. Ayrıca, bir sunucudan çağrıları yapma tek bir yükseltme noktası API gelecek sürümlerinde sağlar.

İstek belirtmelisiniz [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query) kullanıcının kısmi arama terimi içeren sorgu parametresi. İsteğe bağlı olsa da, istek de belirtmeniz gerekir [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt) alınması için sonuçları istediğiniz Pazar tanımlayan sorgu parametresi. İsteğe bağlı sorgu parametrelerin listesi için bkz: [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters). Tüm sorgu parametre değerleri URL kodlanmış olmalıdır.

İstek belirtmelisiniz [Apim abonelik anahtar Ocp](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey) üstbilgi. İsteğe bağlı olsa da, aşağıdaki üst bilgiler de belirtmeniz önerilir:

- [Kullanıcı Aracısı](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X MSEdge ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X arama ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X arama konumu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

İstemci IP ve konum üstbilgileri konumu kullanan içerik döndürmek için önemlidir.

Tüm istek ve yanıt üstbilgileri listesi için bkz: [üstbilgileri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

## <a name="the-request"></a>İstek

İstek tüm üstbilgiler ve önerilen sorgu parametrelerini içermelidir. Kullanıcı Ara kutusuna yeni bir karakter türleri her zaman bu API'yi çağırırsınız. API koşulları önerilen sorgu alaka sorgu dizesi etkileri tamlığını döndürür. Daha fazla bilgi listesi sorgu koşulları önerilen sorgu dizesi, daha ilgili tamamlayın. Örneğin, API için döndürebilir önerileri *s* döndürür için sorguları daha az ilgili etkilenebilir *Yelkenli dinghies*. 

Aşağıdaki örnek, önerilen sorgu dizeleri için döndüren bir istek gösterir *sail*.

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

Herhangi bir Bing API'leri çağırma ilk kez ise, istemci kimliği üstbilgisi eklemeyin. Yalnızca istemci kimliği üstbilgisi daha önce Bing API'si çağırdıktan ve kullanıcı ve aygıt bileşimi için bir istemci kimliği Bing döndürülen içerir.

Önceki isteğinin yanıtı gösterir. Yanıt arama Sorgu önerileri listesini içeren bir web öneri grubu içeriyor. Her bir öneri içeren bir `displayText`, `query`, ve `url` alan.

`displayText` Alan arama kutunun açılan listeyi doldurmak için kullanacağınız önerilen sorgu içerir. Yanıtı içeren tüm öneriler görüntülemeniz gerekir ve belirtilen sırayla.  

Kullanıcı aşağı açılan listeden bir sorgu seçerse, sorgu dizesinde kullanabilir `query` çağırmak için alan [Bing arama API](../bing-web-search/search-the-web.md) ve kendiniz sonuçları görüntüleyin. Veya URL'de kullanabilir `url` Bing arama kullanıcıya gönderilecek alan sonuçları sayfası.

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

Out API deneyin. Git [konsol test API otomatik öneri](https://dev.cognitive.microsoft.com/docs/services/56c7694ecf5ff801a090fbd1/operations/56c769a2cf5ff801a090fbd2).

Yanıt nesneleri kullanma hakkında daha fazla bilgi için bkz [önerilen arama terimleri alma](./get-suggested-search-terms.md).
