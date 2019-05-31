---
title: Kimlik doğrulama istekleri (JavaScript için Microsoft kimlik doğrulama kitaplığı) özel durumda geçirin | Azure
description: JavaScript (MSAL.js) için Microsoft kimlik doğrulama kitaplığı kullanarak kimlik doğrulama isteği bir özel durum parametresi değeri geçirin öğrenin.
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
ms.openlocfilehash: f29d84838ddb11ac359d7a04dbce8e39dd05ac01
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420506"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>Özel bir durum geçişi MSAL.js kullanılarak kimlik doğrulaması istekleri
*Durumu* parametresi, OAuth 2.0 tarafından tanımlandığı gibi bir kimlik doğrulama isteğine dahil ve siteler arası istek sahteciliği saldırılarına önlemek için belirteci yanıt olarak da döndürülür. Varsayılan olarak, Microsoft kimlik doğrulama kitaplığı JavaScript (MSAL.js) için rastgele oluşturulmuş geçirir benzersiz *durumu* kimlik doğrulama isteklerini parametre değeri.

State parametresi, bilgileri yeniden yönlendirme önce uygulamanın durumu kodlamak için de kullanılabilir. Uygulamasında, bu parametre için giriş olarak oldukları sayfası ya da görünümü gibi kullanıcı durumunun geçirebilirsiniz. MSAL.js kitaplığı, özel durum durumu parametre olarak geçirmenize olanak `Request` nesnesi:

```javascript
// Request type
export type AuthenticationParameters = {
    scopes?: Array<string>;
    extraScopesToConsent?: Array<string>;
    prompt?: string;
    extraQueryParameters?: QPDict;
    claimsRequest?: string;
    authority?: string;
    state?: string;
    correlationId?: string;
    account?: Account;
    sid?: string;
    loginHint?: string;
};
```

Örneğin:

```javascript
let loginRequest = {
    scopes: ["user.read", "user.write"],
    state: “page_url”
}

myMSALObj.loginPopup(loginRequest);
```

Geçirilen durumu isteği gönderirken MSAL.js tarafından ayarlamak için benzersiz GUID eklenir. Yanıt döndürüldüğünde, MSAL.js durumu eşleştirmek için denetler ve durumda geçirilen özel döndürür `Response` nesnesinin `accountState`.

```javascript
export type AuthResponse = {
    uniqueId: string;
    tenantId: string;
    tokenType: string;
    idToken: IdToken;
    accessToken: string;
    scopes: Array<string>;
    expiresOn: Date;
    account: Account;
    accountState: string;
};
```

Daha fazla bilgi okuyun [tek sayfalı uygulama (SPA) oluşturma](scenario-spa-overview.md) MSAL.js kullanılarak.