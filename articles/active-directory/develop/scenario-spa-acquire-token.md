---
title: Tek sayfalı uygulama (bir API'yi çağırmak için bir belirteç almak) - Microsoft kimlik platformu
description: Tek sayfalı uygulama (bir API'yi çağırmak için alma bir belirteç) oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f4c842db8a0874d3619e0dc59b90aa12226cb984
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138810"
---
# <a name="single-page-application---acquire-a-token-to-call-an-api"></a>Tek sayfalı uygulama - API çağırmak için bir belirteç Al

MSAL.js API'leriyle önce kullanarak bir sessiz belirteç isteği denemesi için belirteçlerini almak için bir desen `acquireTokenSilent` yöntemi. Bu yöntem çağrıldığında, kitaplık önce geçerli bir belirteç var ve döndürür, görmek için tarayıcıyı depolama önbellekte denetler. Önbelleğinde geçerli belirteç olduğunda gizli iframe'den Azure Active Directory (Azure AD) sessiz bir belirteç isteğini gönderir. Bu yöntem ayrıca yenileme belirteçleri için kitaplık sağlar. Çoklu oturum açma oturumu ve Azure AD'de, belirteç ömrü değerleri hakkında daha fazla ayrıntı için [belirteç ömürleri](active-directory-configurable-token-lifetimes.md).

Azure AD sessiz belirteç isteklerini, süresi dolmuş bir Azure AD oturum veya bir parola değiştirme gibi bazı nedenlerle başarısız olabilir. Bu durumda, belirteçlerini almak için (Bu kullanıcıyı uyarır) etkileşimli yöntemlerinden birini çağırabilirsiniz.

* [Bir açılır pencere ile belirteç almak](#acquire-token-with-a-pop-up-window) kullanma `acquireTokenPopup`
* [Yeniden yönlendirme ile belirteç almak](#acquire-token-with-redirect) kullanma `acquireTokenRedirect`

**Bir açılır pencere ya da yeniden yönlendirme deneyimini arasında seçme**

 Uygulamanıza açılır ve yeniden yönlendirme yöntemlerinin bir birleşimini kullanamazsınız. Bir açılır pencere ya da yeniden yönlendirme deneyimi arasında seçim yapma, uygulama akışınız bağlıdır.

* Kullanıcı kimlik doğrulaması sırasında ana uygulama sayfadan ayrılmak gitmek için istemiyorsanız açılır yöntemini kullanmak için önerilir. Açılır pencerede kimlik doğrulaması yeniden yönlendirme yapıldığından, ana uygulama durumu korunur.

* Burada yeniden yönlendirme yöntemlerini kullanmanız gerekebilir bazı durumlar vardır. Açılır pencereler devre dışı olduğu uygulamanızın kullanıcılarının tarayıcı kısıtlamaları veya ilkeleri varsa, yeniden yönlendirme yöntemlerini kullanabilirsiniz. Ayrıca olduğundan belirli Internet Explorer tarayıcı ile yeniden yönlendirme yöntemlerini kullanmayı önerilir [Internet Explorer ile ilgili bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) açılır pencereleri işlerken.

Erişim belirteci, erişim belirteci isteği oluşturma sırasında dahil etmek istediğiniz API kapsamları ayarlayabilirsiniz. İstenen tüm kapsamlara erişim belirtecinde verilen değil ve kullanıcının onay bağlıdır unutmayın.

## <a name="acquire-token-with-a-pop-up-window"></a>Bir açılır pencere ile belirteci alma

### <a name="javascript"></a>JavaScript

Bir yöntemle açılan bir deneyim için yukarıdaki Desen:

```javascript
const accessTokenRequest = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, send an interactive request.
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenPopup(accessTokenRequest).then(function(accessTokenResponse) {
            // Acquire token interactive success
        }).catch(function(error) {
            // Acquire token interactive failure
            console.log(error);
        });
    }
    console.log(error);
});
```

### <a name="angular"></a>Angular

MSAL Angular sarmalayıcı HTTP dinleyiciyi ekleme kolaylık sunar `MsalInterceptor` , otomatik olarak erişim belirteçlerini sessiz bir şekilde almak ve HTTP isteklerini API'lerine ekleme.

API'leri için kapsamları belirtebilirsiniz `protectedResourceMap` MsalInterceptor otomatik olarak belirteçleri alırken isteyecek yapılandırma seçeneği.

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                protectedResourceMap: {"https://graph.microsoft.com/v1.0/me", ["user.read", "mail.send"]}
            })]
         })

providers: [ ProductService, {
        provide: HTTP_INTERCEPTORS,
        useClass: MsalInterceptor,
        multi: true
    }
   ],
```

Başarı ve hata sessiz belirteç edinme için MSAL Angular geri çağırmaları için abone olabilirsiniz sağlar. Aboneliğinizi iptal etmek unutmamak önemlidir.

```javascript
// In app.component.ts
 ngOnInit() {
    this.subscription=  this.broadcastService.subscribe("msal:acquireTokenFailure", (payload) => {
    });
}

ngOnDestroy() {
   this.broadcastService.getMSALSubject().next(1);
   if(this.subscription) {
     this.subscription.unsubscribe();
   }
 }
```

Alternatif olarak, aynı zamanda açıkça alma belirteci yöntemleri çekirdek MSAL.js Kitaplığı'nda açıklanan şekilde kullanarak belirteçleri elde edebilirsiniz.

## <a name="acquire-token-with-redirect"></a>Yeniden yönlendirme ile belirteci alma

### <a name="javascript"></a>JavaScript

Etkileşimli olarak belirteç almak için bir yeniden yönlendirme yöntemi ile gösterilen ancak yukarıda olarak açıklanan modelidir. Yukarıda da belirtildiği gibi yeniden yönlendirme geri çağırma kaydı gerektiğine dikkat edin.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const accessTokenRequest: AuthenticationParameters = {
    scopes: ["user.read"]
}

userAgentApplication.acquireTokenSilent(accessTokenRequest).then(function(accessTokenResponse) {
    // Acquire token silent success
    // call API with token
    let accessToken = accessTokenResponse.accessToken;
}).catch(function (error) {
    //Acquire token silent failure, send an interactive request.
    console.log(error);
    if (error.errorMessage.indexOf("interaction_required") !== -1) {
        userAgentApplication.acquireTokenRedirect(accessTokenRequest);
    }
});
```

### <a name="angular"></a>Angular

Yukarıda açıklandığı gibi budur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API'si çağırma](scenario-spa-call-api.md)
