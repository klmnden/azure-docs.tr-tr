---
title: Microsoft Bilişsel hizmetler - Bing özel arama - Node.js kullanarak uç noktasını çağırmak
description: Bu hızlı başlangıçta, Bing özel arama uç noktasını çağırmak için Node.js kullanarak arama sonuçlarını özel arama örneğinizin isteği gösterilmektedir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 5d9391cc486dc868a1a291ccc7095291cddd3e4c
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858468"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Çağrı Bing özel arama uç noktası (Node.js)

Bu hızlı başlangıçta, Bing özel arama uç noktasını çağırmak için Node.js kullanarak arama sonuçlarını özel arama örneğinizin isteği gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Bir özel arama örneği. Bkz: [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).

- [Node.js](https://www.nodejs.org/) yüklü.

-  [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirin ya da Ücretli abonelik anahtarı, Azure panosundan kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="run-the-code"></a>Kodu çalıştırma

Bing özel arama uç noktasını çağırmak için bu adımları izleyin:

1. Kodunuz için bir klasör oluşturun.

2. Bir komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.

3. Yükleme **isteği** düğüm Modülü:
    <pre>
    npm install request
    </pre>
    
4. ' % S'dosyası BingCustomSearch.js oluşturun ve aşağıdaki kodu kopyalayın.

5. Değiştirin **YOUR-SUBSCRIPTION-KEY** ve **YOUR-özel-CONFIG-ID** anahtarınızı ve yapılandırma kimliğinizle (1. adıma bakın).

    ``` javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    
    var options = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
          'q=' + searchTerm + 
          '&customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    
    request(options, function(error, response, body){
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
    })
    ```
6. Aşağıdaki komutu kullanarak kodu çalıştırın.
    ```    
    node BingCustomSearch.js
   ``` 

## <a name="next-steps"></a>Sonraki adımlar
- [Barındırılan UI deneyiminizi yapılandırın](./hosted-ui.md)
- [Metni vurgulayacak şekilde decoration işaretçileri kullanma](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)
