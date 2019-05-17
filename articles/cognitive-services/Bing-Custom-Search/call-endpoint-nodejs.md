---
title: 'Hızlı Başlangıç: Node.js kullanarak Bing özel arama uç noktanızı arayın | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Arama sonuçlarını Node.js kullanarak Bing özel arama örneğinizin talep başlamak için bu Hızlı Başlangıç'ı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: 43b46b4dd035efa3117ce23728db42bb11a9ab6c
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790270"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-nodejs"></a>Hızlı Başlangıç: Node.js kullanarak Bing özel arama uç noktanızı arayın

Bing özel arama örneğinizin arama sonuçlarını talep başlamak için bu Hızlı Başlangıç'ı kullanın. Bu uygulamanın, JavaScript'te yazılmış olsa, Bing özel arama API'si bir RESTful web çoğu programlama dilleri ile uyumlu hizmetidir. Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingCustomSearchv7.js)’da mevcuttur.

## <a name="prerequisites"></a>Önkoşullar

- Bing özel arama örneği için. Bkz: [hızlı başlangıç: İlk Bing özel arama örneğinizin oluşturma](quick-start.md) daha fazla bilgi için.

- [Node.js](https://www.nodejs.org/)

- [JavaScript isteği kitaplığı](https://github.com/request/request)

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Yeni bir JavaScript dosyası, sık kullandığınız IDE veya düzenleyici oluşturma ve ekleme bir `require()` istekleri kitaplığı için bildirimi. Abonelik anahtarınız ve özel yapılandırma kimliği bir arama terimi için değişkenler oluşturun. 

    ```javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderip 

1. İsteğinizde gönderilen bilgileri depolamak için bir değişken oluşturun. Arama teriminizi ekleyerek istek URL'si oluşturmak `q=` sorgu parametresi ve yapılandırma kimliği özel arama örneğinizin `customconfig=`. parametrelerle ayrı bir `&` karakter. 

    ```javascript
    var info = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
            'q=' + searchTerm + "&" +
            'customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    ```

1. Bing özel arama örneğinizin arama isteği gönderme ve Web sayfasının son gezinilmesi istendiğinde bilgileri hakkında adı, url ve tarih gibi sonuçları yazdırmak için JavaScript isteği Kitaplığı'nı kullanın.

    ```javascript
    request(info, function(error, response, body){
            var searchResponse = JSON.parse(body);
            for(var i = 0; i < searchResponse.webPages.value.length; ++i){
                var webPage = searchResponse.webPages.value[i];
                console.log('name: ' + webPage.name);
                console.log('url: ' + webPage.url);
                console.log('displayUrl: ' + webPage.displayUrl);
                console.log('snippet: ' + webPage.snippet);
                console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
                console.log();
            }
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir özel arama web uygulaması derleme](./tutorials/custom-search-web-page.md)
