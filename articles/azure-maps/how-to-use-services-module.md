---
title: Azure haritalar Hizmetleri modülü - kullanarak | Microsoft Docs
description: Azure haritalar Hizmetleri modülü kullanmayı öğrenin.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: e614758a91cb3ff02822eeeeb8ae7e80d2123e5d
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578739"
---
# <a name="using-the-azure-maps-services-module"></a>Azure haritalar Hizmetleri Modülünü Kullanma

Azure haritalar Web SDK'sı, web veya JavaScript veya TypeScript kullanarak Node.js uygulamaları Azure haritalar REST hizmetleri kullanmak üzere kolay bir yardımcı kitaplık bir Hizmetleri modülü sağlar.

## <a name="using-the-services-module-in-a-web-page"></a>Bir web sayfasında services modülü kullanma

1. Yeni bir HTML dosyası oluşturun.
2. Azure haritalar Hizmetleri modülüne yükleyin. Bu yapılabilir; iki seçenekten birini kullanma

    a. Betik başvuru ekleyerek Azure haritalar Hizmetleri modülü genel olarak barındırılan CDN sürümünü kullanmanız <head> öğesi:
    
    ```html
    <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=2"></script>
    ```
    
    b. Alternatif olarak, Azure haritalar Web SDK kaynak kodu kullanarak yerel olarak yükleme [azure haritalar rest](https://www.npmjs.com/package/azure-maps-rest) NPM paketini ve uygulamanızı barındırın. Bu paket ayrıca TypeScript tanımları içerir.
    
    > npm yükleme azure-haritalar-rest
    
    Ardından bir komut dosyası başvuru ekleyin `<head>` öğesi:
    
    ```html
    <script src="node_modules/azure-maps-rest/dist/js/atlas-service.min.js"></script>
    ```

3. Hizmet URL'si istemci uç noktasını başlatmak için bir kimlik doğrulama işlem hattını oluşturmanız gerekir. Arama hizmeti istemcinin kimliğini doğrulamak için kendi Azure haritalar hesabı anahtarı veya Azure Active Directory (AAD) kimlik bilgilerini kullanın. Bu örnekte, arama hizmeti URL'si istemcisi oluşturulur. Bir abonelik anahtarı kimlik doğrulaması için kullanıyorsanız:

    ```javascript
    //Get an Azure Maps key at https://azure.com/maps
    var subscriptionKey = '<Your Azure Maps Key>';
    
    //Use SubscriptionKeyCredential with a subscription key.
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(subscriptionKey);
    
    //Use subscriptionKeyCredential to create a pipeline.
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential, {
      retryOptions: { maxTries: 4 } // Retry options
    });
    
    //Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);
    ```
    
    Azure Active Directory (AAD) kimlik doğrulaması için kullanıyorsanız:

    ```javascript
    // Enter your Azure Actiuve Directory client ID.
    var clientId = "<Your Azure Active Directory Client Id>";
    
    // Use TokenCredential with OAuth token (AAD or Anonymous).
    var aadToken = await getAadToken();
    var tokenCredential = new atlas.service.TokenCredential(clientId, aadToken);
    
    // Create a repeating timeout that will renew the AAD token.
    // This timeout must be cleared once the TokenCredential object is no longer needed.
    // If the timeout is not cleared the memory used by the TokenCredential will never be reclaimed.
    var renewToken = async () => {
        try {
            console.log("Renewing token");
            var token = await getAadToken();
            tokenCredential.token = token;
            tokenRenewalTimer = setTimeout(renewToken, getExpiration(token));
        } catch (error) {
            console.log("Caught error when renewing token");
            clearTimeout(tokenRenewalTimer);
            throw error;
        }
    }
    tokenRenewalTimer = setTimeout(renewToken, getExpiration(aadToken));
    
    // Use tokenCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(tokenCredential, {
        retryOptions: { maxTries: 4 } // Retry options
    });
    
    //Create an instance of the SearchURL client.
    var searchURL = new atlas.service.SearchURL(pipeline);

    function getAadToken() {
        //Use the logged in auth context to get a token.
        return new Promise((resolve, reject) => {
            //The resource should always be https://atlas.microsoft.com/.
            const resource = "https://atlas.microsoft.com/";
            authContext.acquireToken(resource, (error, token) => {
                if (error) {
                    reject(error);
                } else {
                    resolve(token);
                }
            });
        })
    }

    function getExpiration(jwtToken) {
        //Decode the JWT token to get the expiration timestamp.
        const json = atob(jwtToken.split(".")[1]);
        const decode = JSON.parse(json);

        //Return the milliseconds until the token needs renewed.
        //Reduce the time until renew by 5 minutes to avoid using an expired token.
        //The exp property is the timestamp of the expiration in seconds.
        const renewSkew = 300000;
        return (1000 * decode.exp) - Date.now() - renewSkew;
    }
    ```

    Daha fazla bilgi için [Azure Haritalar ile kimlik doğrulaması](azure-maps-authentication.md).

4. Aşağıdaki kod geocode bir adres, yeni oluşturulan arama hizmeti URL'si istemciye kullanır "1, Microsoft, yol, Redmond, WA" kullanarak `searchAddress` işlev ve sonuçlar sayfasının gövdesinde bir tablo olarak görüntüler. 

    ```javascript
    //Search for "1 microsoft way, redmond, wa".
    searchURL.searchAddress(atlas.service.Aborter.timeout(10000), '1 microsoft way, redmond, wa').then(response => {
      var html = [];
      
      //Display the total results.
      html.push('Total results: ', response.summary.numResults, '<br/><br/>');
     
      //Create a table of the results.
      html.push('<table><tr><td></td><td>Result</td><td>Latitude</td><td>Longitude</td></tr>');
      
      for(var i=0;i<response.results.length;i++){
        html.push('<tr><td>', (i+1), '.</td><td>', 
                    response.results[i].address.freeformAddress, 
                    '</td><td>', 
                    response.results[i].position.lat,
                    '</td><td>', 
                    response.results[i].position.lon,
                    '</td></tr>');
      }
      
      html.push('</table>');
      
      //Add the result HTML to the body of the page.
      document.body.innerHTML = html.join('');
    });
    ```

    Tam olarak çalışan kod örneği aşağıdadır:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Hizmetleri Modülünü Kullanma" src="//codepen.io/azuremaps/embed/zbXGMR/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/zbXGMR/'>Services modülü kullanarak</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan yöntemleri ve sınıfları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [MapsURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.mapsurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SearchURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [RouteURL](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SubscriptionKeyCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.subscriptionkeycredential?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TokenCredential](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.tokencredential?view=azure-iot-typescript-latest)

Hizmetleri modülü kullanmak daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Harita üzerinde arama sonuçlarını göster](./map-search-location.md)

> [!div class="nextstepaction"]
> [Bir Koordinattan bilgi alma](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [Yönergeleri A'dan B'ye yönleri gösterme](./map-route.md)