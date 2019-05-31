---
title: Sayfa yeniden yükler (JavaScript için Microsoft kimlik doğrulama kitaplığı) kaçınmak | Azure
description: Sayfa yüklemelere alırken önlemek ve Microsoft kimlik doğrulama kitaplığı (MSAL.js) için JavaScript kullanarak sessiz yenileyen belirteçleri öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/29/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 162811221e6dde89ad11f358b2ec8f32f3c82522
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420476"
---
# <a name="avoid-page-reloads-when-acquiring-and-renewing-tokens-silently-using-msaljs"></a>Sayfa yeniden yükler alırken ve sessiz bir şekilde MSAL.js kullanılarak yenileyen belirteçleri kaçının
Microsoft kimlik doğrulama kitaplığı (MSAL.js) JavaScript kullanımlar gizli `iframe` almak ve yenileme belirteçleri arka planda sessizce öğeleri. Azure AD geri belirteç isteğinde belirtilen kayıtlı redirect_uri belirteç döndürür (varsayılan olarak uygulama kök sayfası budur). Bir 302 yanıt olduğundan, HTML için ilgili sonuçları `redirect_uri` içinde yüklenmekte `iframe`. Genellikle uygulamanın `redirect_uri` kök sayfa ve bu da yeniden yüklemek neden olur.

Uygulamanın kök sayfasına gidin, kimlik doğrulaması gerektiriyorsa, diğer durumlarda, bu çok iç içe geçmiş yol açabilecek `iframe` öğeleri veya `X-Frame-Options: deny` hata.

MSAL.js Azure AD tarafından verilen 302 kapatılamıyor ve döndürülen belirteç işlemi için gerekli olduğundan önleyemez `redirect_uri` içinde yüklenmekte gelen `iframe`.

Tüm uygulamayı yeniden yeniden yüklemeyi veya diğer hatalar nedeniyle bunun önlemek için lütfen bu çözümler izleyin.

## <a name="specify-different-html-for-the-iframe"></a>İframe için farklı bir HTML belirtin

Ayarlama `redirect_uri` kimlik doğrulaması gerektirmeyen basit bir sayfaya, yapılandırma özelliği. İle eşleştiğinden emin olmak sahip olduğunuz `redirect_uri` Azure Portalı'nda kayıtlı. MSAL, başlangıç sayfası kaydedildikçe kullanıcı oturum açma işlemi başlar ve oturum açma tamamlandıktan sonra tam konumuna yeniden yönlendirir. Bu kullanıcının oturum açma deneyimi etkilemez.

## <a name="initialization-in-your-main-app-file"></a>Ana uygulama dosyanızdaki başlatma

Yoktur uygulama başlatma, Yönlendirme ve başka şeyler tanımlayan bir merkezi Javascript dosyası, uygulamanızı yapılandırılırsa olup uygulama içinde yüklüyor üzerinde tabanlı uygulama modüllerinizi koşullu olarak yükleyebilir bir `iframe` veya yok. Örneğin:

AngularJS,: app.js

```javascript
// Check that the window is an iframe and not popup
if (window !== window.parent && !window.opener) {
angular.module('todoApp', ['ui.router', 'MsalAngular'])
    .config(['$httpProvider', 'msalAuthenticationServiceProvider','$locationProvider', function ($httpProvider, msalProvider,$locationProvider) {
        msalProvider.init(
            // msal configuration
        );

        $locationProvider.html5Mode(false).hashPrefix('');
    }]);
}
else {
    angular.module('todoApp', ['ui.router', 'MsalAngular'])
        .config(['$stateProvider', '$httpProvider', 'msalAuthenticationServiceProvider', '$locationProvider', function ($stateProvider, $httpProvider, msalProvider, $locationProvider) {
            $stateProvider.state("Home", {
                url: '/Home',
                controller: "homeCtrl",
                templateUrl: "/App/Views/Home.html",
            }).state("TodoList", {
                url: '/TodoList',
                controller: "todoListCtrl",
                templateUrl: "/App/Views/TodoList.html",
                requireLogin: true
            })

            $locationProvider.html5Mode(false).hashPrefix('');

            msalProvider.init(
                // msal configuration
            );
        }]);
}
```

Angular içinde: app.module.ts

```javascript
// Imports...
@NgModule({
  declarations: [
    AppComponent,
    MsalComponent,
    MainMenuComponent,
    AccountMenuComponent,
    OsNavComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production }),
    MsalModule.forRoot(environment.MsalConfig),
    SuiModule,
    PagesModule
  ],
  providers: [
    HttpServiceHelper,
    {provide: HTTP_INTERCEPTORS, useClass: MsalInterceptor, multi: true},
    AuthService
  ],
  entryComponents: [
    AppComponent,
    MsalComponent
  ]
})
export class AppModule {
  constructor() {
    console.log('APP Module Constructor!');
  }

  ngDoBootstrap(ref: ApplicationRef) {
    if (window !== window.parent && !window.opener)
    {
      console.log("Bootstrap: MSAL");
      ref.bootstrap(MsalComponent);
    }
    else
    {
    //this.router.resetConfig(RouterModule);
      console.log("Bootstrap: App");
      ref.bootstrap(AppComponent);
    }
  }
}
```

MsalComponent:

```javascript
import { Component} from '@angular/core';
import { MsalService } from '@azure/msal-angular';

// This component is used only to avoid Angular reload
// when doing acquireTokenSilent()

@Component({
  selector: 'app-root',
  template: '',
})
export class MsalComponent {
  constructor(private Msal: MsalService) {
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [tek sayfalı uygulama (SPA) oluşturma](scenario-spa-overview.md) MSAL.js kullanılarak.