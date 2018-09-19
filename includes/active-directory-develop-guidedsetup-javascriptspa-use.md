---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 94d57abc95dabf1da579f6d2105ca6c74140a86f
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293741"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Kullanıcının oturum açmak için Microsoft kimlik doğrulama kitaplığı (MSAL) kullanma

1.  `app.js` adlı bir dosya oluşturun. Visual Studio kullanıyorsanız, ' % s'projesi (projenin kök klasöründe) seçin, sağ tıklatın ve seçin: `Add`  >  `New Item`  >  `JavaScript File`:
2.  Aşağıdaki kodu ekleyin, `app.js` dosyası:

```javascript
// Graph API endpoint to show user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used to obtain the access token to read user profile
var graphAPIScopes = ["https://graph.microsoft.com/user.read"];

// Initialize application
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});

//Previous version of msal uses redirect url via a property
if (userAgentApplication.redirectUri) {
    userAgentApplication.redirectUri = msalconfig.redirectUri;
}

window.onload = function () {
    // If page is refreshed, continue to display user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call the Microsoft Graph API and display the results on the page. Sign the user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user to sign in via loginRedirect.
        // This will redirect user to the Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // The call to loginRedirect above frontloads the consent to query Graph API during the sign-in.
        // If you want to use dynamic consent, just remove the graphAPIScopes from loginRedirect call.
        // As such, user will be prompted to give consent when requested access to a resource that 
        // he/she hasn't consented before. In the case of this application - 
        // the first time the Graph API call to obtain user's profile is executed.
    } else {
        // If user is already signed in, display the user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show sign-out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API to show the user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order to call the Graph API, an access token needs to be acquired.
        // Try to acquire the token used to query Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After the access token is acquired, call the Web API, sending the acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If the acquireTokenSilent() method fails, then acquire the token interactively via acquireTokenRedirect().
                // In this case, the browser will redirect user back to the Azure Active Directory v2 Endpoint so the user 
                // can reenter the current username/ password and/ or give consent to new permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get the token silently, the Graph API call results will be made and results will be displayed in the page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });
    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() to show results.
 * @param {string} errorDesc - If error occur, the error message
 * @param {object} token - The token received from sign-in
 * @param {object} error - The error string
 * @param {string} tokenType - The token type: For loginRedirect, tokenType = "id_token". For acquireTokenRedirect, tokenType:"access_token".
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in the page
 * @param {string} endpoint - the endpoint used for the error message
 * @param {string} error - Error string
 * @param {string} errorDesc - Error description
 */
function showError(endpoint, error, errorDesc) {
    var formattedError = JSON.stringify(error, null, 4);
    if (formattedError.length < 3) {
        formattedError = error;
    }
    document.getElementById("errorMessage").innerHTML = "An error has occurred:<br/>Endpoint: " + endpoint + "<br/>Error: " + formattedError + "<br/>" + errorDesc;
    console.error(error);
}

```

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

Bir kullanıcı tıklattıktan sonra *'Microsoft Graph API çağrısı'* düğmesini ilk kez `callGraphApi` yöntem çağrılarını `loginRedirect` kullanıcısı ile oturum. Kullanıcıya yeniden yönlendirme bu yöntem sonuçlarını *Microsoft Azure Active Directory v2 uç noktası* sor ve kullanıcının kimlik bilgilerini doğrulamak için. Bir oturum açma başarılı sonucu olarak, kullanıcının özgün durumuna geri yönlendirilir *index.html* sayfa ve bir belirteç alındığında, tarafından işlenen `msal.js` ve belirtecinde yer alan bilgileri önbelleğe alınır. Bu belirteci olarak da bilinen *kimlik belirteci* ve kullanıcı görünen adı gibi kullanıcıyla ilgili temel bilgileri içerir. Tüm amaçlar için bu belirteci tarafından sağlanan herhangi bir veri kullanmayı planlıyorsanız, bu belirteç, uygulamanız için geçerli bir kullanıcı için belirteç verildiğini güvence altına almak için arka uç sunucunuzu doğrulayan emin olmanız gerekir.

Bu kılavuzda oluşturulan SPA kullanmayan doğrudan kimlik belirteci kullanın: bunun yerine çağrı `acquireTokenSilent` ve/veya `acquireTokenRedirect` almak için bir *erişim belirteci* Microsoft Graph API sorgulamak için kullanılır. Kimlik belirteci doğrular bir örnek gerekirse göz atın [bu](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 örnek") github'da – örnek örnek uygulama bir ASP kullanır Belirteç doğrulama için .NET web API'si.

#### <a name="getting-a-user-token-interactively"></a>Etkileşimli bir kullanıcı belirteci alma

İlk oturum açma işleminden sonra bu nedenle kaynak – erişmek için bir belirteç istemek ihtiyaç duydukları her zaman yeniden kimlik doğrulamaya zorlayabilir talep etmek istemediğiniz *acquireTokenSilent* çoğu zaman belirteçlerini almak için kullanılmalıdır. Durumlar vardır ancak kullanıcıları, Azure Active Directory v2 uç noktası ile – etkileşim kurmak gereken bazı örnekler şunlardır:
- Kullanıcıların parola süresi dolduğundan, kimlik bilgilerini yeniden girmeniz gerekebilir
- Uygulamanızın kullanıcı onay gerektiren bir kaynağa erişim istiyor
- İki faktörlü kimlik doğrulaması gereklidir

Çağırma *acquireTokenRedirect(scope)* kullanıcılar Azure Active Directory v2 uç noktası için yeniden yönlendirme sonucu (veya *acquireTokenPopup(scope)* sonucu bir açılan pencere üzerinde) etkileşim kurmak, kullanıcıların nerede gerekir kimlik bilgilerini onaylayan, gerekli kaynak için izin vermek ya da Tamamlanıyor iki öğeli kimlik doğrulamasını.

#### <a name="getting-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma
` acquireTokenSilent` Belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenileme yöntemi işler. Sonra `loginRedirect` (veya `loginPopup`) ilk kez yürütülür `acquireTokenSilent` veya belirteçleri yenileme isteği için çağrıları sessizce yapıldıkça yapılan sonraki çağrılar için-korunan kaynaklara erişim için kullanılan belirteçleri elde etmek için yaygın kullanılan yöntemdir.
`acquireTokenSilent` başarısız olabilir bazı durumlarda – Örneğin, kullanıcı parolasının süresi doldu. Uygulamanız, bu özel durumun iki şekilde işleyebilir:

1.  Çağrı yapmak `acquireTokenRedirect` hemen sonuçlanır kullanıcının oturum açmasını isteyen içinde. Bu düzen çevrimiçi uygulamalarında yaygın olarak kullanılan bulunduğu kimliği doğrulanmamış içerik uygulamada kullanıcı tarafından kullanılabilir. Bu Kılavuzlu kurulum tarafından oluşturulan örnek bu deseni kullanır.

2. Uygulamaları bir etkileşimli oturum açma kullanıcı oturum açmak için doğru zamanda seçebilir ya da uygulama yeniden deneyebilir gerekli olan, kullanıcı için bir görsel gösterimi de yapabilir `acquireTokenSilent` daha sonra. Bu yaygın olarak kullanılan kullanıcı uygulamanın diğer işlevleri kesintiye olmadan kullanabilir - Örneğin, kimliği doğrulanmamış içerik uygulamada kullanılabilir olduğunda. Bu durumda, korumalı kaynağa erişmeye veya güncel olmayan bilgileri yenilemek için oturum açmak istediğinizde kullanıcı karar verebilirsiniz.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Yalnızca edindiğiniz belirteci kullanarak Microsoft Graph API çağırma

Aşağıdaki kodu ekleyin, `app.js` dosyası:

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used to display the results
 * @param {object} showTokenElement = HTML element used to display the RAW access token
 */
function callWebApiWithToken(endpoint, token, responseElement, showTokenElement) {
    var headers = new Headers();
    var bearer = "Bearer " + token;
    headers.append("Authorization", bearer);
    var options = {
        method: "GET",
        headers: headers
    };

    fetch(endpoint, options)
        .then(function (response) {
            var contentType = response.headers.get("content-type");
            if (response.status === 200 && contentType && contentType.indexOf("application/json") !== -1) {
                response.json()
                    .then(function (data) {
                        // Display response in the page
                        console.log(data);
                        responseElement.innerHTML = JSON.stringify(data, null, 4);
                        if (showTokenElement) {
                            showTokenElement.parentElement.classList.remove("hidden");
                            showTokenElement.innerHTML = token;
                        }
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            } else {
                response.json()
                    .then(function (data) {
                        // Display response as error in the page
                        showError(endpoint, data);
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            }
        })
        .catch(function (error) {
            showError(endpoint, error);
        });
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma hakkında daha fazla bilgi

Bu kılavuzda, oluşturulan örnek uygulamada `callWebApiWithToken()` yöntemi bir HTTP yapmak için kullanılan `GET` istemek için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana döndürmesi. Bu yöntem alınan belirteç ekler *HTTP yetkilendirme üst bilgisi*. Bu kılavuzda oluşturulan örnek uygulama için Microsoft Graph API kaynaktır *bana* endpoint – kullanıcı profili bilgilerini görüntüler.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcının oturumunu kapatmaz için yöntem ekleme

Aşağıdaki kodu ekleyin, `app.js` dosyası:

```javascript
/**
 * Sign out the user
 */
function signOut() {
    userAgentApplication.logout();
}
```
