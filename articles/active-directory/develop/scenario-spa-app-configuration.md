---
title: Tek sayfalı uygulama (uygulama kodu yapılandırma) - Microsoft kimlik platformu
description: Tek sayfalı uygulama (uygulama kodu yapılandırma) oluşturmayı öğrenin
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
ms.openlocfilehash: 6236f0fa2400113225e04ffd4884cf743d1e250a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138282"
---
# <a name="single-page-application---code-configuration"></a>Tek sayfalı uygulama - kod yapılandırma

Tek sayfalı uygulama (SPA) kodunu yapılandırmayı öğrenin.

## <a name="msal-libraries-supporting-implicit-flow"></a>Örtük akış destekleyen MSAL kitaplıkları

Microsoft kimlik platformu, MSAL.js kitaplığı sektör kullanarak örtük akış desteklemek için önerilen güvenli uygulamalar sağlar.  

Örtük akış destekleyen kitaplıkları şunlardır:

| MSAL kitaplığı | Açıklama |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | Angular, Vue.js, React.js vb. gibi JavaScript veya SPA çerçeveler kullanılarak oluşturulan tüm istemci tarafı web uygulamaları kullanmak için düz JavaScript kitaplığı. |
| ![MSAL Angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Angular framework ile oluşturulan tek sayfa uygulamaları kullanımda basitleştirmek için çekirdek MSAL.js kitaplığı sarmalayıcı. Bu kitaplık Önizleme aşamasındadır ve sahip [bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) belirli Angular sürümleri ve tarayıcılar ile. |

## <a name="application-code-configuration"></a>Uygulama kodu yapılandırma

MSAL Kitaplığı'nda uygulama kayıt bilgileri Yapılandırma kitaplığı başlatma sırasında geçirilir.

### <a name="javascript"></a>JavaScript

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientID: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```
Yapılandırılabilir seçenekleri hakkında daha fazla bilgi için bkz. [MSAL.js uygulamayla başlatma](msal-js-initializing-client-applications.md).

### <a name="angular"></a>Angular

```javascript
//In app.module.ts
@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id'
            })]
         })

  export class AppModule { }
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oturum açma ve oturum kapatma](scenario-spa-sign-in.md)
