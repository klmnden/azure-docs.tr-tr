---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 8785f89a335f0ae7d983f267176da1656aee57a0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65199561"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Kullanıcının oturum açmak için Microsoft kimlik doğrulama kitaplığı (MSAL) kullanma

1. Aşağıdaki kodu ekleyin, `index.html` içinde dosya `<script></script>` etiketler:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "Enter_the_Application_Id_here"
            authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };

    var graphConfig = {
        graphMeEndpoint: "https://graph.microsoft.com/v1.0/me"
    };

    // this can be used for login or token request, however in more complex situations
    // this can have diverging options
    var requestObj = {
        scopes: ["user.read"]
    };

    var myMSALObj = new Msal.UserAgentApplication(msalConfig);
    // Register Callbacks for redirect flow
    myMSALObj.handleRedirectCallback(authRedirectCallBack);


    function signIn() {

        myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
            //Login Success
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }).catch(function (error) {
            console.log(error);
        });
    }

    function acquireTokenPopupAndCallMSGraph() {
        //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
        myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error);
            // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
            // Call acquireTokenPopup(popup window)
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
                    callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
                }).catch(function (error) {
                    console.log(error);
                });
            }
        });
    }


    function graphAPICallback(data) {
        document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
    }


    function showWelcomeMessage() {
        var divWelcome = document.getElementById('WelcomeMessage');
        divWelcome.innerHTML = 'Welcome ' + myMSALObj.getAccount().userName + "to Microsoft Graph API";
        var loginbutton = document.getElementById('SignIn');
        loginbutton.innerHTML = 'Sign Out';
        loginbutton.setAttribute('onclick', 'signOut();');
    }


    //This function can be removed if you do not need to support IE
    function acquireTokenRedirectAndCallMSGraph() {
         //Always start with acquireTokenSilent to obtain a token in the signed in user from cache
         myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
             callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
         }).catch(function (error) {
             console.log(error);
             // Upon acquireTokenSilent failure (due to consent or interaction or login required ONLY)
             // Call acquireTokenRedirect
             if (requiresInteraction(error.errorCode)) {
                 myMSALObj.acquireTokenRedirect(requestObj);
             }
         });
     }


    function authRedirectCallBack(error, response) {
        if (error) {
            console.log(error);
        }
        else {
            if (response.tokenType === "access_token") {
                callMSGraph(graphConfig.graphEndpoint, response.accessToken, graphAPICallback);
            } else {
                console.log("token type is:" + response.tokenType);
            }
        }
    }

    function requiresInteraction(errorCode) {
        if (!errorCode || !errorCode.length) {
            return false;
        }
        return errorCode === "consent_required" ||
            errorCode === "interaction_required" ||
            errorCode === "login_required";
    }

    // Browser check variables
    var ua = window.navigator.userAgent;
    var msie = ua.indexOf('MSIE ');
    var msie11 = ua.indexOf('Trident/');
    var msedge = ua.indexOf('Edge/');
    var isIE = msie > 0 || msie11 > 0;
    var isEdge = msedge > 0;
    //If you support IE, our recommendation is that you sign-in using Redirect APIs
    //If you as a developer are testing using Edge InPrivate mode, please add "isEdge" to the if check
    // can change this to default an experience outside browser use
    var loginType = isIE ? "REDIRECT" : "POPUP";

    if (loginType === 'POPUP') {
        if (myMSALObj.getAccount()) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }
    }
    else if (loginType === 'REDIRECT') {
        document.getElementById("SignIn").onclick = function () {
            myMSALObj.loginRedirect(requestObj);
        };
        if (myMSALObj.getAccount() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenRedirectAndCallMSGraph();
        }
    } else {
        console.error('Please set a valid login type');
    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Ek bilgiler

Bir kullanıcı tıklattıktan sonra **oturum** düğmesini ilk kez `signIn` yöntem çağrılarını `loginPopup` kullanıcısı ile oturum. Bu yöntem ile bir açılan pencere açılırken sonuçları *Microsoft kimlik platformu uç nokta* sor ve kullanıcının kimlik bilgilerini doğrulamak için. Bir oturum açma başarılı sonucu olarak, kullanıcının özgün durumuna geri yönlendirilir *index.html* sayfa ve bir belirteç alındığında, tarafından işlenen `msal.js` ve belirtecinde yer alan bilgileri önbelleğe alınır. Bu belirteci olarak da bilinen *kimlik belirteci* ve kullanıcı görünen adı gibi kullanıcıyla ilgili temel bilgileri içerir. Tüm amaçlar için bu belirteci tarafından sağlanan herhangi bir veri kullanmayı planlıyorsanız, bu belirteç, uygulamanız için geçerli bir kullanıcı için belirteç verildiğini güvence altına almak için arka uç sunucunuzu doğrulayan emin olmanız gerekir.

Bu tarafından oluşturulan SPA Kılavuzu çağrıları `acquireTokenSilent` ve/veya `acquireTokenPopup` almak için bir *erişim belirteci* Microsoft Graph API için kullanıcı profili bilgileri sorgulamak için kullanılan. Kimlik belirteci doğrular bir örnek gerekirse göz atın [bu](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "GitHub active-directory-javascript-singlepageapp-dotnet-webapi-v2 örnek") github'da – örnek örnek uygulama bir ASP kullanır Belirteç doğrulama için .NET web API'si.

#### <a name="getting-a-user-token-interactively"></a>Kullanıcı belirtecini etkileşimli olarak alma

İlk oturum açma işleminden sonra bu nedenle kaynak – erişmek için bir belirteç istemek ihtiyaç duydukları her zaman yeniden kimlik doğrulamaya zorlayabilir talep etmek istemediğiniz *acquireTokenSilent* çoğu zaman belirteçlerini almak için kullanılmalıdır. Durumlar vardır ancak kullanıcıları, Microsoft kimlik platformu noktayla – etkileşim kurmak gereken bazı örnekler şunlardır:

- Parolanın süresi dolduğundan kullanıcıların kimlik bilgilerini yeniden girmesi gerektiğinde
- Uygulamanız kullanıcının onaylaması gereken bir kaynağa erişim istediğinde
- İki faktörlü kimlik doğrulama gerektiğinde

Çağırma *acquireTokenPopup* sonuçları bir açılır pencerede (veya *acquireTokenRedirect* sonuçları kullanıcılarını Microsoft kimlik platformu uç noktaya yönlendirme de) burada kullanıcıların gerekir ya da etkileşime geçmek gerekli kaynak için izin vermek ya da iki faktörlü kimlik doğrulamasını tamamlayan kimlik bilgilerini onaylanıyor.

#### <a name="getting-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

`acquireTokenSilent` Belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenileme yöntemi işler. Sonra `loginPopup` (veya `loginRedirect`) ilk kez yürütülür `acquireTokenSilent` veya belirteçleri yenileme isteği için çağrıları sessizce yapıldıkça yapılan sonraki çağrılar için-korunan kaynaklara erişim için kullanılan belirteçleri elde etmek için yaygın kullanılan yöntemdir.
`acquireTokenSilent` başarısız olabilir bazı durumlarda – Örneğin, kullanıcı parolasının süresi doldu. Uygulamanız, bu özel durumun iki şekilde işleyebilir:

1. Çağrı yapmak `acquireTokenPopup` hemen sonuçlanır kullanıcının oturum açmasını isteyen içinde. Bu düzen çevrimiçi uygulamalarında yaygın olarak kullanılan bulunduğu kimliği doğrulanmamış içerik uygulamada kullanıcı tarafından kullanılabilir. Bu Kılavuzlu kurulum tarafından oluşturulan örnek bu deseni kullanır.

2. Uygulamaları bir etkileşimli oturum açma kullanıcı oturum açmak için doğru zamanda seçebilir ya da uygulama yeniden deneyebilir gerekli olan, kullanıcı için bir görsel gösterimi de yapabilir `acquireTokenSilent` daha sonra. Bu yaygın olarak kullanılan kullanıcı uygulamanın diğer işlevleri kesintiye olmadan kullanabilir - Örneğin, kimliği doğrulanmamış içerik uygulamada kullanılabilir olduğunda. Bu durumda, korumalı kaynağa erişmeye veya güncel olmayan bilgileri yenilemek için oturum açmak istediğinizde kullanıcı karar verebilirsiniz.

> [!NOTE]
> Bu hızlı başlangıçta kullanılmaktadır `loginRedirect` ve `acquireTokenRedirect` kullanılan tarayıcı nedeni Internet Explorer olduğunda yöntemleri bir [bilinen sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) açılır pencereleri işlenmesi için Internet Explorer tarayıcı tarafından ilgili.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Yalnızca edindiğiniz belirteci kullanarak Microsoft Graph API çağırma

Aşağıdaki kodu ekleyin, `index.html` içinde dosya `<script></script>` etiketler:

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma hakkında daha fazla bilgi

Bu kılavuzda, oluşturulan örnek uygulamada `callMSGraph()` yöntemi bir HTTP yapmak için kullanılan `GET` istemek için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana döndürmesi. Bu yöntem alınan belirteç ekler *HTTP yetkilendirme üst bilgisi*. Bu kılavuzda oluşturulan örnek uygulama için Microsoft Graph API kaynaktır *bana* endpoint – kullanıcı profili bilgilerini görüntüler.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Kullanıcının oturumunu kapatmaz için yöntem ekleme

Aşağıdaki kodu ekleyin, `index.html` içinde dosya `<script></script>` etiketler:

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```
