## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Kullanıcıyla oturum açmak için Microsoft kimlik doğrulama kitaplığı (MSAL) kullanın

1.  Adlı bir dosya oluşturun `app.js`. Visual Studio kullanıyorsanız, projeyi (Proje kök klasöründe) seçin, sağ tıklatın ve seçin: `Add`  >  `New Item`  >  `JavaScript File`:
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

        // Show sign-off button
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

Bir kullanıcı tıklattıktan sonra *'Microsoft Graph API çağrısı'* düğmesine ilk kez `callGraphApi` yöntem çağrılarını `loginRedirect` kullanıcıyla oturum açmak için. Bu yöntem kullanıcı için yeniden yönlendirme sonucu *Microsoft Azure Active Directory v2 endpoint* ister ve kullanıcının kimlik bilgilerini doğrulamak için. Bir başarılı oturum açma sonucu olarak, kullanıcının özgün durumuna geri yönlendirildiği *index.html* sayfası ve bir belirteç alındığında, tarafından işlenen `msal.js` ve belirtecinde yer alan bilgileri önbelleğe alınır. Bu belirteç olarak bilinen *kimliği belirteci* ve gibi kullanıcı görünen adı kullanıcı hakkındaki temel bilgileri içerir. Tüm amaçlar için bu belirteci tarafından sağlanan herhangi bir veri kullanmayı planlıyorsanız, bu belirteç belirteç uygulamanız için geçerli bir kullanıcı için verilmiş olduğunu güvence altına almak için arka uç sunucu tarafından doğrulandığından emin olmanız gerekir.

Bu kılavuz tarafından oluşturulan SPA kullanmayan doğrudan kimliği belirteci kullanın-bunun yerine, çağıran `acquireTokenSilent` ve/veya `acquireTokenRedirect` edinmeye bir *erişim belirteci* Microsoft Graph API sorgulamak için kullanılır. Kimliği belirteci doğrular bir örnek gerekiyorsa, bir göz atalım [bu](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 örnek") örnek uygulama github'da – örnek bir ASP kullanır .NET web API'si belirteci doğrulama için.

#### <a name="getting-a-user-token-interactively"></a>Kullanıcı etkileşimli olarak belirteci alma

Kullanıcıları bir kaynağa – şekilde erişmek için bir belirteç istemek ihtiyaç duydukları her zaman kimlik doğrulamaya isteyin istemediğiniz ilk oturum açma işleminden sonra *acquireTokenSilent* belirteçleri almak üzere çoğu zaman kullanılmalıdır. Durumlar vardır ancak kullanıcıların Azure Active Directory v2 noktayla – etkileşim kurmasına zorlamak gereken bazı örnekler şunlardır:
- Kullanıcıların parolanızın süresi dolduğu için kimlik bilgilerini yeniden girmeniz gerekebilir
- Uygulamanız için onayı için kullanıcının gerektiren bir kaynağa erişim isteme
- İki faktörlü kimlik doğrulama gereklidir

Çağırma *acquireTokenRedirect(scope)* neden kullanıcıların Azure Active Directory v2 uç noktasına yeniden yönlendirme (veya *acquireTokenPopup(scope)* bir açılır pencere sonucuna) kullanıcılar nerede gereken etkileşim kurmak kimlik bilgilerini onaylayan, gerekli kaynağa izin vermiş veya Tamamlanıyor tarafından iki öğeli kimlik doğrulamasını.

#### <a name="getting-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma
` acquireTokenSilent` Yöntemi belirteci satın almalar ve herhangi bir kullanıcı etkileşimi olmadan yenileme işler. Sonra `loginRedirect` (veya `loginPopup`) ilk kez yürütüldüğünde `acquireTokenSilent` veya belirteçleri yenileme isteği için çağrıları sessizce gerçekleştirilmediğinden sonraki çağrılar için-korumalı kaynaklara erişmek için kullanılan belirteçleri elde etmek için yaygın olarak kullanılan yöntem.
`acquireTokenSilent` başarısız olabileceği bazı durumlarda – Örneğin, kullanıcının parolasının süresi doldu. Uygulamanız bu özel durumun iki yolla işleyebilir:

1.  Çağırmaya `acquireTokenRedirect` hemen hangi sonuçları oturum açmak için kullanıcıdan içinde. Bu desen çevrimiçi uygulamalarında yaygın olarak kullanılan söz konusu olduğunda kimliği doğrulanmamış içerik uygulamada kullanıcı tarafından kullanılabilir. Bu Destekli kurulum tarafından oluşturulan örnek bu deseni kullanır.

2. Uygulamalar bir etkileşimli oturum açma kullanıcı oturum açmak için doğru zamanı seçebilir ya da uygulama deneyebilirsiniz gerekli olan kullanıcı için görsel bir gösterge de yapabilirsiniz `acquireTokenSilent` daha sonra. Bunun yaygın olarak kullanılan kullanıcı kesintiye olmadan diğer uygulamanın işlevselliğini kullanabilir - Örneğin, kimliği doğrulanmamış içerik uygulamada kullanılabilir olduğunda. Bu durumda, korumalı kaynağa erişmek için ya da eski bilgileri yenilemek için giriş yapmak istediğinizde kullanıcının karar verebilirsiniz.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle kullanarak Microsoft Graph API çağrısı

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

Bu kılavuzu tarafından oluşturulan örnek uygulamasında `callWebApiWithToken()` yöntemi bir HTTP yapmak için kullanılan `GET` isteği için bir belirteç gerekiyor korunan bir kaynağa karşı ve ardından içeriği çağırana dönün. Bu yöntem alınan belirteç ekler *HTTP Authorization Üstbilgisi*. Bu kılavuz tarafından oluşturulan örnek bir uygulama için Microsoft Graph API kaynaktır *bana* endpoint – kullanıcı profili bilgilerini görüntüler.

<!--end-collapse-->

## <a name="add-a-method-to-sign-off-the-user"></a>Kullanıcı oturumunu imzalamak için bir yöntem ekleyin

Aşağıdaki kodu ekleyin, `app.js` dosyası:

```javascript
/**
 * Sign off the user
 */
function signOut() {
    userAgentApplication.logout();
}
```
