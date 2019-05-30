---
title: Bing otomatik öneri API'si için gönderme istekleri
titlesuffix: Azure Cognitive Services
description: Bing otomatik öneri API'si için istekleri göndereceğinizi öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: scottwhi
ms.openlocfilehash: abcce52e126e01d25434a90260a220c9aa337f5b
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66382708"
---
# <a name="sending-requests-to-the-bing-autosuggest-api"></a>Bing otomatik öneri API'si isteği gönderme.

Uygulamanızın tüm Bing arama API'leri sorguları gönderirse, Bing otomatik öneri API'si kullanıcılarınızın arama deneyimini geliştirmek için kullanabilirsiniz. Bing otomatik öneri API'si, bir arama kutusu kısmi sorgu dizesine göre önerilen sorgular listesi döndürür. Uygulamanızın arama kutusuna karakter girdiniz gibi aşağı açılan listede önerileri görüntüleyebilirsiniz. Bu API'ye istek göndererek hakkında daha fazla bilgi için bu makaleyi kullanın.

## <a name="bing-autosuggest-api-endpoint"></a>Bing otomatik öneri API'si uç noktası

**Bing otomatik öneri API'si** bir kısmi arama terimi önerilen sorgular listesi döndüren bir uç nokta içerir.

Bing API'si kullanarak önerilen sorgular elde etmek için Gönder bir `GET` aşağıdaki uç noktayı isteği. Daha fazla özellikleri tanımlamak için başlık ve URL parametrelerini kullanın.

**Uç noktası:** Tarafından tanımlanan kullanıcı girişi için uygun olan JSON sonucu döndürür arama önerileri `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Üstbilgileri, parametreleri, Pazar kodları, yanıt nesneleri, hata hakkındaki ayrıntılar için vb., bkz: [Bing otomatik öneri API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference) başvuru.

**Bing** türüne göre sonuçları arama eylemleri API'lerini desteklemektedir. Tüm arama uç noktaları sonuçlar JSON yanıtı nesneler olarak döndürür.
Tüm uç noktalar, belirli bir dil ve/veya konum boylam, enlem ve arama RADIUS döndüren sorgular destekler.

Her bir uç noktası tarafından desteklenen parametreleri hakkında tam bilgi için her türü için başvuru sayfalarına bakın.
Otomatik öneri API'si kullanarak temel istekleri örnekleri için bkz: [otomatik öneri hızlı Başlangıçlar](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).

## <a name="bing-autosuggest-api-requests"></a>Bing otomatik öneri API'si istekleri

> [!NOTE]
> Bing otomatik öneri API'si için istekler HTTPS protokolünü kullanmalıdır.

Tüm isteklerin bir sunucudan gönderilmesini öneririz. Anahtarı bir istemci uygulamanın bir parçası dağıtma, daha fazla fırsat kötü amaçlı üçüncü taraf erişim sağlar. Ayrıca, çağrı yapmaya bir sunucudan tek bir yükseltme noktası gelecekteki güncelleştirmeleri sağlar.

İstek kullanıcının kısmi arama terimini içeren [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query) sorgu parametresini belirtmelidir. Tercihe bağlı olmakla birlikte, istek, sonuçların gelmesini istediğiniz pazarı tanımlayan [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt) sorgu parametresini de belirtmelidir. İsteğe bağlı sorgu parametrelerinin bir listesi için bkz. [Sorgu Parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters). Tüm sorgu parametre değerleri URL olarak kodlanmış olmalıdır.

İstek [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey) üstbilgisini belirtmelidir. İsteğe bağlı olmakla birlikte şu üstbilgileri de belirtmeniz önerilir:

- [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

İstemci IP'si ve konum üstbilgileri konuma duyarlı içerik döndürmek için önemlidir.

Tüm istek ve yanıt üstbilgilerinin bir listesi için bkz. [Üst Bilgiler](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

> [!NOTE]
> Bing otomatik öneri API'si JavaScript'ten çağırdığınızda, tarayıcınızın yerleşik güvenlik özellikleri bu üstbilgi değerlerini erişmesini engelleyebilir.

Bu sorunu çözmek için Bing otomatik öneri API'si isteği bir CORS Ara sunucu aracılığıyla yapabilirsiniz. Böyle bir ara sunucu yanıtı sahip bir `Access-Control-Expose-Headers` üst bilgi, beyaz yanıt üst bilgileri ve JavaScript için kullanılabilir hale getirir.

İzin vermek için bir CORS Ara Sunucusu'nun yükleneceği kolaydır bizim [öğretici uygulama](../tutorials/autosuggest.md) isteğe bağlı istemci üstbilgileri erişmek için. İlk olarak, henüz yüklemediyseniz [Node.js'yi yükleyin](https://nodejs.org/en/download/). Ardından bir komut isteminde aşağıdaki komutu girin.

    npm install -g cors-proxy-server

Ardından, Bing otomatik öneri API'si uç nokta HTML dosyasındaki değiştirin:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Son olarak, aşağıdaki komutla CORS ara sunucusunu başlatın:

    cors-proxy-server

Öğretici uygulamasını kullanırken komut penceresini açık bırakın; pencere kapatılırsa ara sunucu durdurulur. Arama sonuçlarının altındaki genişletilebilir HTTP Üst Bilgileri bölümünde artık `X-MSEdge-ClientID` üst bilgisini (diğerleriyle birlikte) görebilir ve bunun her istekte aynı olduğunu doğrulayabilirsiniz.

İstekleri tüm önerilen sorgu parametrelerinin ve üst bilgileri içermelidir. 

Aşağıdaki örnekte, *sail* için önerilen sorgu dizelerini döndüren bir istek gösterilmektedir.

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

Kullanıcının açılan listeden bir sorgu seçerse, biri olan arama için kullanabileceğiniz [Bing arama API'leri](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/bing-api-comparison?toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fbing-autosuggest%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json) ve sonuçları kendiniz görüntülemek veya kullanıcı döndürülen kullanarak Bing sonuçları sayfasına gönderin `url` alan. Aşağıdaki örnek, Bing Web araması API'si kullanır.

```json
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

- [Bing Otomatik Öneri nedir?](../get-suggested-search-terms.md)
- [Bing Otomatik Öneri API’si v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference)
- [Bing otomatik öneri API'si önerilen arama terimlerini alma](get-suggestions.md)
