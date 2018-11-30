---
title: 'Hızlı başlangıç: Node.js kullanarak uç nokta çağırma - Bing Özel Arama'
titlesuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta Bing Özel Arama uç noktasını çağırmak için Node.js kullanarak özel arama örneğinizden arama sonuçlarını isteme adımları gösterilmektedir.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: aahi
ms.openlocfilehash: c0c97dd52f8fc3ff590c86f32f794beeb00f4b05
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52310261"
---
# <a name="quickstart-call-bing-custom-search-endpoint-nodejs"></a>Hızlı başlangıç: Bing Özel Arama uç noktasını çağırma (Node.js)

Bu hızlı başlangıçta Bing Özel Arama uç noktasını çağırmak için Node.js kullanarak özel arama örneğinizden arama sonuçlarını isteme adımları gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

- Kullanıma hazır özel arama örneği. Bkz. [İlk Bing Özel Arama örneğinizi oluşturma](quick-start.md).
- [Node.js](https://www.nodejs.org/) uygulamasının yüklenmiş olması.
- Abonelik anahtarı. Abonelik anahtarını [ücretsiz denemenizi](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) etkinleştirdikten sonra alabilir veya Azure panonuzdan ücretli abonelik anahtarı (bkz. [Bilişsel Hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)) kullanabilirsiniz.   Ayrıca bkz: [Bilişsel hizmetler fiyatlandırması - Bing arama API'si](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-code"></a>Kodu çalıştırma

Bu örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Kodunuz için bir klasör oluşturun.  
  
2. Bir komut isteminden veya terminalden az önce oluşturduğunuz klasöre gidin.  
  
3. **request** Node modülünü yükleyin:
    <pre>
    npm install request
    </pre>  
    
4. Oluşturduğunuz klasörde BingCustomSearch.js adlı bir dosya oluşturun ve aşağıdaki kodu içine kopyalayın. **YOUR-SUBSCRIPTION-KEY** ve **YOUR-CUSTOM-CONFIG-ID** yerine abonelik anahtarınızı ve yapılandırma kimliğinizi yazın.  
  
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
  
6. Şu komutu kullanarak kodu çalıştırın:  
  
    ```    
    node BingCustomSearch.js
    ``` 

## <a name="next-steps"></a>Sonraki adımlar
- [Barındırılan kullanıcı arabirimi deneyiminizi yapılandırma](./hosted-ui.md)
- [Metni vurgulamak için süsleme işaretçilerini kullanma](./hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
