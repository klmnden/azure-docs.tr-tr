---
title: Mobil uygulama çağrıları API uygulaması için bir belirteç alınırken - web | Microsoft kimlik platformu
description: Web API'ları (uygulama için bir belirteç alınırken) çağıran bir mobil uygulama oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6933bfbbff574495655ef9065a786fa313b02bd6
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075183"
---
# <a name="mobile-app-that-calls-web-apis---acquire-a-token"></a>Web API - çağıran mobil uygulama bir belirteç Al

Korumalı çağırma başlamadan önce web API'leri, uygulamanıza bir erişim belirteci gerekir. Bu bölümde Microsoft kimlik doğrulama kitaplığı (MSAL) kullanarak bir belirteç almak üzere sürecinde yardımcı olur.

## <a name="scopes-to-request"></a>İstek için kapsamları

Bir kapsam, her zaman için belirteç isterken gereklidir. Uygulamanıza erişmek hangi verilerin kapsamı belirler.  

Birleştirmek istediğiniz web API'nin en basit yaklaşımdır `App ID URI` kapsamlı `.default`. Bu, Microsoft Identity uygulamanız için gerekli tüm kapsamlar portalında bildirir.

Android
```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
```

iOS
```swift
let scopes: [String] = ["https://graph.microsoft.com/.default"]
```

Xamarin
```CSharp 
var scopes = new [] {"https://graph.microsoft.com/.default"};
```

## <a name="acquiring-tokens"></a>Belirteç alınırken

### <a name="via-msal"></a>MSAL

MSAL uygulamaları sessizce ve etkileşimli olarak belirteçlerini almak sağlar. Yalnızca bu yöntemleri çağırmak ve MSAL istenen kapsamlar için erişim belirteci geri döndürür. Sessiz isteği ve geri dönüş için etkileşimli bir isteği gerçekleştirmek için doğru desendir bakın.

#### <a name="android"></a>Android

```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);

// Check if there are any accounts we can sign in silently
// Result is in our silent callback (success or error)
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {

        if (accounts.isEmpty() && accounts.size() == 1) {
            // TODO: Create a silent callback to catch successful or failed request
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts or >1 account */
        }
    }
});    

[...]

// No accounts found, interactively request a token 
// TODO: Create an interactive callback to catch successful or failed request
sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());        
```

#### <a name="ios"></a>iOS

```swift
// Initialize our app 
guard let authorityURL = URL(string: kAuthority) else {
    self.loggingText.text = "Unable to create authority URL"
    return
}
let authority = try MSALAADAuthority(url: authorityURL)
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)

// Get tokens
let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
applicationContext.acquireTokenSilent(with: parameters) { (result, error) in
    if let error = error {
        let nsError = error as NSError

        // interactionRequired means we need to ask the user to sign-in. This usually happens
        // when the user's Refresh Token is expired or if the user has changed their password
        // among other possible reasons.
        if (nsError.domain == MSALErrorDomain) {
            if (nsError.code == MSALError.interactionRequired.rawValue) {    
                DispatchQueue.main.async {
                    guard let applicationContext = self.applicationContext else { return }
                    let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
                    applicationContext.acquireToken(with: parameters) { (result, error) in
                        if let error = error {
                            self.updateLogging(text: "Could not acquire token: \(error)")
                            return
                        }

                        guard let result = result else {
                            self.updateLogging(text: "Could not acquire token: No result returned")
                            return
                        }
                        
                        // Token is ready via interaction!
                        self.accessToken = result.accessToken
                    }
                }
                return
            }
        }

        self.updateLogging(text: "Could not acquire token silently: \(error)")
        return
    }
    guard let result = result else {
        self.updateLogging(text: "Could not acquire token: No result returned")
        return
    }

    // Token is ready via silent acquisition 
    self.accessToken = result.accessToken
}
```

#### <a name="xamarin"></a>Xamarin

```CSharp
string[] scopes = new string["https://graph.microsoft.com/.default"];
var app = PublicClientApplicationBuilder.Create(clientId).Build();
var accounts = await app.GetAccountsAsync();
AuthenticationResult result;
try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
             .ExecuteAsync();
}
catch(MsalUiRequiredException e)
{
 result = await app.AcquireTokenInteractive(scopes)
             .ExecuteAsync();
}
```

### <a name="via-protocol"></a>Protokolü aracılığıyla

Biz geçmeden doğrudan protokole göre öneri yok. Uygulamanız çok sayıda tek oturum açma (SSO) senaryoları özelliğine sahip olmayacaktır ve tüm cihaz yönetimi ve koşullu erişim senaryoları destekleyecek şekilde mümkün olmayacaktır.

Belirteçleri protokolü kullanarak mobil uygulamalar için alırken 2 isteklerde gerekir: bir yetkilendirme kodu alın ve için bir belirteç değişimi. 

#### <a name="getting-authorization-code"></a>Yetkilendirme kodu alma

```Text
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=<CLIENT_ID>
&response_type=code
&redirect_uri=<ENCODED_REDIRECT_URI>
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2F.default
&state=12345
```

#### <a name="getting-access-and-refresh-token"></a>Erişim ve yenileme belirteci alma

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=<CLIENT_ID>
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=<ENCODED_REDIRECT_URI>
&grant_type=authorization_code
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'si çağırma](scenario-mobile-call-api.md)
