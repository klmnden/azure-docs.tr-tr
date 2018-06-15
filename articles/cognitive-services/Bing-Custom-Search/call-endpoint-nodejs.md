---
title: Uç nokta Node.js - Bing özel arama - Microsoft Bilişsel hizmetler kullanarak çağırma
description: Bu hızlı başlangıç Bing özel arama uç noktasını çağırmak için Node.js kullanarak özel arama örneğinden arama sonuçlarında istek gösterilmektedir.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 48fc234e15ce3b9172d766f6fae11b51a017ce70
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355732"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Çağrı Bing özel arama uç noktası (Node.js)

Bu hızlı başlangıç Bing özel arama uç noktasını çağırmak için Node.js kullanarak özel arama örneğinden arama sonuçlarında istek gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Bir özel arama örneği. Bkz: [ilk Bing özel arama örneğinizi oluşturmak](quick-start.md).

- [Node.js](https://www.nodejs.org/) yüklü.

-  [Bilişsel hizmetler API hesabını](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Bing arama API'leri**. [Ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) Bu Hızlı Başlangıç için yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirmek ya da Ücretli abonelik anahtarı Azure panonuza kullanabilir sağlanan erişim anahtarı gerekir.

## <a name="run-the-code"></a>Kodu çalıştırma

Bing özel arama uç noktasını çağırmak için aşağıdaki adımları izleyin:

1. Kodunuz için bir klasör oluşturun.
2. Bir komut istemi veya terminal, az önce oluşturduğunuz klasöre gidin.
3. Yükleme **isteği** düğümü Modülü:
    <pre>
    npm install request
    </pre>
4. BingCustomSearch.js dosyası oluşturun ve aşağıdaki kodu kopyalayın.
5. Değiştir **YOUR ABONELİK anahtarı** ve **YOUR-özel-CONFIG-ID** anahtar ve yapılandırma Kimliğine sahip (1. adım bakın).

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
- [Metni vurgulama için decoration işaretlerini kullanın](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)