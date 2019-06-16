---
title: Çoklu oturum açma (JavaScript için Microsoft kimlik doğrulama kitaplığı) | Azure
description: JavaScript (MSAL.js) için Microsoft kimlik doğrulama kitaplığı kullanarak çoklu oturum açma deneyimlerini oluşturma hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9f1f102307256852ac92616c7fb707e0e2739e5d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544148"
---
# <a name="single-sign-on-with-msaljs"></a>Çoklu oturum açma MSAL.js ile

Çoklu oturum açma (SSO) oturum açın ve yeniden kimlik doğrulaması için gerek kalmadan birden çok uygulamada yeniden kullanılabilir bir oturumu kurmak için bir kez kimlik bilgilerini girmesini sağlar. Bu kullanıcı için sorunsuz bir deneyim sağlar ve yinelenen kimlik bilgilerini ister azaltır.

Azure AD, kullanıcı ilk kez doğruladığında bir oturum tanımlama bilgisinin ayarlayarak uygulamaları için SSO özelliklerini sağlar. MSAL.js kitaplık uygulamaları birkaç yöntemle kullanmasına izin verir.

## <a name="sso-between-browser-tabs"></a>Tarayıcı sekmeler arasında SSO

Uygulamanız birden fazla sekmelerde açın ve bir sekmesinde kullanıcı ilk kez oturum, kullanıcı ayrıca diğer sekmelerde istenmeden oturumu açık. MSAL.js önbelleğe alır, kimlik belirteci kullanıcının tarayıcıdaki `localStorage` ve kullanıcının uygulamaya diğer sekmelerdeki açık oturum açılacaktır.

Varsayılan olarak, MSAL.js kullanır `sessionStorage` hangi izin vermiyor sekmeler arasında paylaşılması için oturumu. Sekmeler arasında SSO almak için ayarladığınızdan emin olun `cacheLocation` MSAL.js için de `localStorage` aşağıda gösterildiği gibi.

```javascript
const config = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”
    },
    cache: {
        cacheLocation: 'localStorage'
    }
}

const myMSALObj = new UserAgentApplication(config);
```

## <a name="sso-between-apps"></a>Uygulamalar arasında SSO

Bir kullanıcı kimliği doğruladığında bir oturum tanımlama bilgisinin tarayıcıdaki Azure AD etki alanı üzerinde ayarlanır. MSAL.js kullanıcı farklı uygulamalar arasında SSO sağlamak için bu oturum tanımlama bilgisini kullanır. MSAL.js ayrıca kimlik ve erişim belirteçler kullanıcının her uygulama etki alanı tarayıcı depolamadaki önbelleğe alır. Sonuç olarak, farklı durumlar için SSO davranışı değişir:  

### <a name="applications-on-the-same-domain"></a>Aynı etki alanında uygulamalar

Aynı etki alanında barındırılan uygulamalar, kullanıcı uygulamaya bir kez oturum açabilir ve ardından bir istem olmadan diğer uygulamalara kimliği. SSO sağlamak etki alanındaki kullanıcı için önbelleğe belirteçleri MSAL.js yararlanır.

### <a name="applications-on-different-domain"></a>Farklı bir etki alanında bulunan uygulamalar

B etki alanındaki MSAL.js tarafından uygulamaların farklı etki alanlarında barındırıldığında, önbelleğe alınan etki A belirteçleri erişilemez

Bu, kullanıcılar'na gitmek için bir uygulama etki alanı B etki alanında oturum açtığınızda, yeniden yönlendirilen veya kaldırılacak ile Azure AD sayfasının istenir, anlamına gelir. Azure AD yine de kullanıcı oturum tanımlama bilgisi olduğundan, kullanıcı imzalar ve kimlik bilgilerini yeniden girmeniz gerekmez. Kullanıcı Azure AD ile bir oturumda birden çok kullanıcı hesabı varsa, kullanıcının oturum açmak için ilgili hesabı seçmeniz istenir.

### <a name="automatically-select-account-on-azure-ad"></a>Otomatik olarak Azure AD hesabı seçin

Bazı durumlarda, uygulama kullanıcının kimlik doğrulaması bağlamı erişimi olan ve birden çok hesap oturum açtığınızda Azure AD hesabı seçimi istemini ister.  Bu, birkaç farklı yöntemle gerçekleştirilebilir:

**Oturum kimliği (SID) kullanma**

Oturum kimliği bir [isteğe bağlı bir talep](active-directory-optional-claims.md) kimliği belirteçlerinde yapılandırılabilir. Bu talep, kullanıcının Azure AD oturum kullanıcının hesap adı veya kullanıcı adı bağımsız belirlemek üzere uygulamayı sağlar. İstek parametreleri için SID geçirebilirsiniz `acquireTokenSilent` çağırın. Bu hesap seçimi atlamak Azure AD olanak tanır. SID oturum tanımlama bağlıdır ve tarayıcı bağlamları çapraz değil.

```javascript
var request = {
    scopes: ["user.read"],
    sid: sid
}

userAgentApplication.acquireTokenSilent(request).then(function(response) {
        const token = response.accessToken
    }
).catch(function (error) {  
        //handle error
});
```

> [!Note]
> SID ile yaptığı sessiz kimlik doğrulama istekleri kullanılabilir `acquireTokenSilent` MSAL.js çağırın.
Uygulama bildirimine isteğe bağlı bir talep yapılandırma adımları bulabilirsiniz [burada](active-directory-optional-claims.md).

**Oturum açma ipucunu kullanarak**

Yapılandırılmış talep veya etkileşimli kimlik doğrulaması çağrılarındaki hesap seçimi istemini atlamak gereken SID'e sahip değilse, sağlayarak bunu yapabilirsiniz bir `login_hint` istek parametrelerini ve isteğe bağlı olarak bir `domain_hint` olarak `extraQueryParameters` MSAL.js içinde Etkileşimli yöntemleri (`loginPopup`, `loginRedirect`, `acquireTokenPopup` ve `acquireTokenRedirect`). Örneğin:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.loginRedirect(request);
```

Değerleri, kullanıcının kimlik belirteci iade talepleri okuyarak login_hint ve domain_hint alabilirsiniz.

* **loginHint** ayarlanmalıdır `preferred_username` kimliği belirtecinde talep.

* **domain_hint** / Common kullanırken geçirilecek yalnızca gerekli yetkilisi. Etki alanı ipucu ID(tid) Kiracı tarafından belirlenir.  Varsa `tid` talebi kimliği belirteçteki `9188040d-6c67-4c5b-b112-36a304b66dad` tüketiciler olduğu. Aksi halde, kuruluşlara kalır.

Okuma [burada](v2-oauth2-implicit-grant-flow.md) oturum açma ipucu ve etki alanı ipucu değerleri hakkında daha fazla bilgi için.

> [!Note]
> Aynı anda SID ve login_hint geçiremezsiniz. Bu hata yanıtında neden olur.

## <a name="sso-without-msaljs-login"></a>SSO MSAL.js oturum açma olmadan

Tasarım gereği, oturum açma yöntemi için API'leri belirteçleri almadan önce bir kullanıcı içeriği oluşturmak için çağrılan MSAL.js gerektirir. Etkileşimli oturum açma yöntemleri olduğundan, kullanıcı bir ileti görür.

Uygulamaları kimliği doğrulanmış kullanıcının içerik erişimi olduğu bazı durumlar vardır veya kimlik belirteci kimlik doğrulaması üzerinden başka bir uygulamada başlatılan ve ilk MSAL.js oturum açma olmadan belirteçlerini almak için SSO yararlanmak isteyen.

Bunun bir örneği verilmiştir: Bir kullanıcı, bir eklenti veya eklentisi olarak çalışan başka bir JavaScript uygulamasını barındıran üst web uygulamasına imzalanır.

Bu senaryoda, SSO bir deneyim şu şekilde sağlanabilir:

Geçirmek `sid` varsa (veya `login_hint` ve isteğe bağlı olarak `domain_hint`) MSAL.js parametreleri istek `acquireTokenSilent` gibi çağırın:

```javascript
var request = {
    scopes: ["user.read"],
    loginHint: preferred_username,
    extraQueryParameters: {domain_hint: 'organizations'}
}

userAgentApplication.acquireTokenSilent(request).then(function(response) {
        const token = response.accessToken
    }
).catch(function (error) {  
        //handle error
});
```

## <a name="sso-in-adaljs-to-msaljs-update"></a>SSO, MSAL.js güncelleştirilecek ADAL.js

Azure AD kimlik doğrulama senaryoları için özellik eşliği ile ADAL.js MSAL.js getirir. Geçiş için MSAL.js ADAL.js kolaylaştırmak ve yeniden oturum açmak için kullanıcılarınızın isteyen önlemek için kitaplık ADAL.js önbellekte kullanıcının oturumunu temsil eden kimlik belirteci okur ve sorunsuz bir şekilde MSAL.js kullanıcı oturum açar.  

Çoklu oturum açma (SSO) davranışı ADAL.js güncelleştirirken yararlanmak için kitaplıkları kullandığından emin olun gerekecektir `localStorage` belirteçlerini önbelleğe alma. Ayarlama `cacheLocation` için `localStorage` şekilde atandığında MSAL.js ve ADAL.js yapılandırması:


```javascript
// In ADAL.js
window.config = {
   clientId: 'g075edef-0efa-453b-997b-de1337c29185',
   cacheLocation: 'localStorage'
};

var authContext = new AuthenticationContext(config);


// In latest MSAL.js version
const config = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”
    },
    cache: {
        cacheLocation: 'localStorage'
    }
}

const myMSALObj = new UserAgentApplication(config);
```

Bu yapılandırıldıktan sonra MSAL.js ADAL.js kimliği doğrulanmış kullanıcının önbelleğe alınan durumu okuyamadı ve SSO, MSAL.js sağlamak kullanan mümkün olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [çoklu oturum açma oturumu ve belirteç ömrü](active-directory-configurable-token-lifetimes.md) Azure ad'deki değerleri.
