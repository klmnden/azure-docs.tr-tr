---
title: Tek sayfalı uygulama (oturum açma) - Microsoft kimlik platformu
description: Tek sayfalı uygulama (oturum açma) oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc9c46ae28960387e6f8efc1ade20afa1c77ef55
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138796"
---
# <a name="single-page-application---sign-in"></a>Tek sayfalı uygulama - oturum açma

Tek sayfalı uygulama kodunu oturum açma ekleme hakkında bilgi edinin.

Uygulamanızda API'lere belirteçleri sağlayabilmek için önce bir kimliği doğrulanmış kullanıcı bağlamı gerekir. Kullanıcılara iki yolla MSAL.js uygulamanızda oturum açabilirsiniz:

* [Oturum açılan pencere oturum](#sign-in-with-a-pop-up-window) kullanarak `loginPopup` yöntemi
* [Yeniden yönlendirme oturum oturum](#sign-in-with-redirect) kullanarak `loginRedirect` yöntemi

Ayrıca isteğe bağlı olarak, kullanıcı oturum açma anında onay için ihtiyaç duyduğunuz API'leri kapsamını geçirebilirsiniz.

> [!NOTE]
> Uygulamanız zaten bir kimliği doğrulanmış kullanıcı bağlamı veya kimliği erişimi varsa belirteç, oturum açma adımı atlayın ve doğrudan belirteçlerini almak. Daha fazla ayrıntı için [sso msal.js oturum açma olmadan](msal-js-sso.md#sso-without-msaljs-login).

## <a name="choosing-between-a-pop-up-or-redirect-experience"></a>Bir açılır pencere ya da yeniden yönlendirme deneyimini arasında seçme

Uygulamanıza açılır ve yeniden yönlendirme yöntemlerinin bir birleşimini kullanamazsınız. Bir açılır pencere ya da yeniden yönlendirme deneyimi arasında seçim yapma, uygulama akışınız bağlıdır.

* Kullanıcı kimlik doğrulaması sırasında ana uygulama sayfadan ayrılmak gitmek için istemiyorsanız açılır yöntemini kullanmak için önerilir. Açılır pencerede kimlik doğrulaması yeniden yönlendirme yapıldığından, ana uygulama durumu korunur.

* Burada yeniden yönlendirme yöntemlerini kullanmanız gerekebilir bazı durumlar vardır. Açılır pencereler devre dışı olduğu uygulamanızın kullanıcılarının tarayıcı kısıtlamaları veya ilkeleri varsa, yeniden yönlendirme yöntemlerini kullanabilirsiniz. Olduğundan belirli yeniden yönlendirme yöntemleri ile Internet Explorer tarayıcısını kullanın [Internet Explorer ile ilgili bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) açılır pencereleri işlerken.

## <a name="sign-in-with-a-pop-up-window"></a>Bir açılır pencere bilgilerinizle oturum açın

### <a name="javascript"></a>JavaScript

```javascript
const loginRequest = {
    scopes: ["user.read", "user.write"]
}

userAgentApplication.loginPopup(loginRequest).then(function (loginResponse) {
    //login success
    let idToken = loginResponse.idToken;
}).catch(function (error) {
    //login failure
    console.log(error);
});
```

### <a name="angular"></a>Angular

Uygulamanızda belirli yollar yalnızca ekleyerek güvenli hale getirmek MSAL Angular sarmalayıcı sağlar `MsalGuard` rota tanımına. Bu koruma yönlendiren erişildiğinde oturum açmak için bir yöntem çağırır.

```javascript
// In app.routes.ts
{ path: 'product', component: ProductComponent, canActivate : [MsalGuard],
    children: [
      { path: 'detail/:id', component: ProductDetailComponent  }
    ]
   },
  { path: 'myProfile' ,component: MsGraphComponent, canActivate : [MsalGuard] },
```

Bir açılır pencere deneyimi için etkinleştirme `popUp` yapılandırma seçeneği. Şu şekilde onay gerektiren kapsamları de geçirebilirsiniz:

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                popUp: true,
                consentScopes: ["user.read", "user.write"]
            })]
         })
```

## <a name="sign-in-with-redirect"></a>Yeniden yönlendirme ile oturum açın

### <a name="javascript"></a>JavaScript

Yeniden yönlendirme yöntemleri, ana uygulama uzağa Gezinti nedeniyle promise gitmez. İşlem ve verilen belirteçler erişmek için yeniden yönlendirme yöntemleri çağrılmadan önce başarı ve hata geri aramaları kaydetme gerekecektir.

```javascript
function authCallback(error, response) {
    //handle redirect response
}

userAgentApplication.handleRedirectCallback(authCallback);

const loginRequest = {
    scopes: ["user.read", "user.write"]
}

userAgentApplication.loginRedirect(loginRequest);
```

### <a name="angular"></a>Angular

Kodu buraya bir açılır pencere bölümü oturum altında yukarıda açıklanan ile aynıdır. Yeniden yönlendirme varsayılan akışıdır.

> [!NOTE]
> Kimlik belirteci onay verilmiş kapsamları içermiyor ve yalnızca kimliği doğrulanmış kullanıcı temsil eder. Onay verilmiş kapsamları, sonraki adımda alacağı erişim belirteci döndürülür.

## <a name="sign-out"></a>Oturumu kapat

MSAL kitaplığı sağlayan bir `logout` tarayıcı depolama önbellekte temizler ve istek oturumunuzu Azure AD'ye gönderir. Sonra oturumunuzu, varsayılan olarak uygulama başlangıç sayfasına yönlendirir.

URI için onu yeniden yönlendirmesi işaretinden sonra out ayarlayarak yapılandırabilirsiniz `postLogoutRedirectUri`. Bu URI, ayrıca uygulama kaydınızı oturum kapatma URI olarak kaydedilmelidir.

### <a name="javascript"></a>JavaScript

```javascript
const config = {

    auth: {
        clientID: 'your_app_id',
        redirectUri: "your_app_redirect_uri", //defaults to application start page
        postLogoutRedirectUri: "your_app_logout_redirect_uri"
    }
}

const userAgentApplication = new UserAgentApplication(config);
userAgentApplication.logout();

```

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id',
                postLogoutRedirectUri: "your_app_logout_redirect_uri"
            })]
         })

// In app.component.ts
this.authService.logout();
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama için bir belirteç alınırken](scenario-spa-acquire-token.md)
