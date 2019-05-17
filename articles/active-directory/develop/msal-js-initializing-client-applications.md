---
title: İstemci uygulamalarında (JavaScript için Microsoft kimlik doğrulama kitaplığı) başlatmak | Azure
description: JavaScript (MSAL.js) için Microsoft kimlik doğrulama Kitaplığı'nı kullanarak istemci uygulamalarını başlatma hakkında bilgi edinin.
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
ms.date: 04/12/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd26f36356affbc8c272bd093757a8482773baf2
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544020"
---
# <a name="initialize-client-applications-using-msaljs"></a>İstemci uygulamaları MSAL.js kullanılarak başlatılamıyor
Bu makalede başlatılırken Microsoft kimlik doğrulama kitaplığı (MSAL.js) JavaScript için bir kullanıcı aracısı uygulama örneği ile açıklanır. Kullanıcı aracı uygulama, istemci kodu bir web tarayıcısı gibi bir kullanıcı aracısına yürütüldü genel istemci uygulamasının bir biçimidir. Tarayıcı bağlam açık yayımlanmaması erişilebildiğinden, bu istemciler, gizli depolamayın. İstemci uygulama türleri ve uygulama yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için [genel bakış](msal-client-applications.md).

## <a name="prerequisites"></a>Önkoşullar
Bir uygulamayı başlatmadan önce öncelikle gerekir [Azure portalı ile kaydetme](scenario-spa-app-registration.md) böylece uygulamanız Microsoft kimlik platformu ile tümleştirilebilir. Kayıt sonrasında (Bu, Azure portalında bulunabilir) aşağıdaki bilgilere ihtiyacınız:

- İstemci kimliği (uygulamanız için bir GUİD'i temsil eden bir dize)
- Kimlik sağlayıcısı URL'si (örnek olarak adlandırılır) ve uygulamanız için oturum açma İzleyici. Bu iki parametre topluca yetkilisi olarak bilinir.
- Yalnızca kuruluşunuz için (aynı zamanda tek kiracılı uygulama adlı) bir iş kolu satır uygulama yazıyorsanız, Kiracı kimliği.
- Web apps için de redirectUri burada kimlik sağlayıcı güvenlik belirteçlerini uygulamanızla dönersiniz ayarlamanız gerekir.

## <a name="initializing-applications"></a>Uygulama başlatma

MSAL.js düz bir JavaScript/Typescript uygulamada şu şekilde kullanabilirsiniz. Örnekleme tarafından MSAL kimlik doğrulaması bağlamı başlatılamıyor `UserAgentApplication` ile bir yapılandırma nesnesi. MSAL.js başlatmak için gerekli en düşük yapılandırma uygulama kayıt Portalı'ndan almalısınız, uygulamanızın ClientID ' dir.

Kimlik doğrulama yöntemlerini akışlar yönlendirmek için (`loginRedirect` ve `acquireTokenRedirect`), başarı veya hata için bir geri çağırma açıkça kaydetmek ihtiyacınız olacak `handleRedirectCallback()` yöntemi. Bir açılır deneyimiyle yöntemleri olarak yeniden yönlendirme akış gösterir döndürmeyen olduğundan bu gereklidir.

```javascript
// Configuration object constructed
const config = {
    auth: {
        clientId: “abcd-ef12-gh34-ikkl-ashdjhlhsdg”
    }
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(config);

function authCallback(error, response) {
    //handle redirect response
}

// (optional when using redirect methods) register redirect call back for Success or Error
myMSALObj.handleRedirectCallback(authCallback);
```

Tek örnek ve yapılandırılmasını sağlamak için tasarlanan MSAL.js `UserAgentApplication` tek bir kimlik doğrulaması bağlamını temsil etmek için. Tarayıcıda çakışan önbellek girişlerinin ve davranış neden birden çok örneği önerilmez.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

MSAL.js sahip bir yapılandırma nesnesi, gösterilen yapılandırılabilir seçeneklerin bir örneğini oluşturmak için kullanılabilir bir gruplandırmasını sağlar `UserAgentApplication`.

```javascript
type storage = "localStorage" | "sessionStorage";

// Protocol Support
export type AuthOptions = {
    clientId: string;
    authority?: string;
    validateAuthority?: boolean;
    redirectUri?: string | (() => string);
    postLogoutRedirectUri?: string | (() => string);
    navigateToLoginRequestUrl?: boolean;
};

// Cache Support
export type CacheOptions = {
    cacheLocation?: CacheLocation;
    storeAuthStateInCookie?: boolean;
};

// Library support
export type SystemOptions = {
    logger?: Logger;
    loadFrameTimeout?: number;
    tokenRenewalOffsetSeconds?: number;
};

// Developer App Environment Support
export type FrameworkOptions = {
    isAngular?: boolean;
    unprotectedResources?: Array<string>;
    protectedResourceMap?: Map<string, Array<string>>;
};

// Configuration Object
export type Configuration = {
    auth: AuthOptions,
    cache?: CacheOptions,
    system?: SystemOptions,
    framework?: FrameworkOptions
};
```

Yapılandırma nesnesi şu anda desteklenen yapılandırılabilir seçeneklerin toplam kümesini aşağıdadır:

- **ClientID**: Gereklidir. Uygulamanızın ClientID, bu uygulama kayıt Portalı'ndan almalısınız.

- **Yetkilisi**: İsteğe bağlı. MSAL belirteçleri isteyebileceği bir dizin belirten bir URL. Varsayılan değer: `https://login.microsoftonline.com/common`.
    * İsteğe bağlı olarak Azure AD'de form https:// olan&lt;örneği&gt;/&lt;İzleyici&gt;burada &lt;örneği&gt; kimlik sağlayıcısı etki alanıdır (örneğin, `https://login.microsoftonline.com`) ve &lt;İzleyici&gt; oturum İzleyici temsil eden bir tanımlayıcıdır. Bu, aşağıdaki değerleri olabilir:
        * `https://login.microsoftonline.com/<tenant>`-Kiracı etki alanı contoso.onmicrosoft.com veya temsil eden GUID gibi Kiracı ile ilişkili olduğu `TenantID` yalnızca belirli bir kuruluşun kullanıcıları imzalamak için kullanılan dizininin özelliği.
        * `https://login.microsoftonline.com/common`-Kullanıcıların iş ve Okul hesapları veya kişisel Microsoft hesabı ile imzalamak için kullanılır.
        * `https://login.microsoftonline.com/organizations/`-Kullanıcıların iş ve Okul hesapları ile imzalamak için kullanılır.
        * `https://login.microsoftonline.com/consumers/` -Kullanıcılar yalnızca kişisel Microsoft hesabı (live) ile imzalamak için kullanılır.
    * İsteğe bağlı olarak Azure AD B2C'de, formu şöyledir `https://<instance>/tfp/<tenant>/<policyName>/`örneği Azure AD B2C'yi etki alanının nerede Kiracı Azure AD B2C kiracısı adını, policyName uygulanacak B2C İlkesi adıdır.


- **validateAuthority**: İsteğe bağlı.  Belirteçler veren doğrulayın. `true` varsayılan değerdir. B2C uygulamaları için yetkili değeri verilir ve her ilke, farklı olabilir yetkilisi doğrulama çalışmaz ve ayarlanması gereken `false`.

- **redirectUri**: İsteğe bağlı.  Yeniden yönlendirme URI'si uygulamanızın, burada kimlik doğrulama yanıtlarının gönderilebilen veya uygulamanız tarafından alındı. Bu URL olarak kodlanmış olması dışında tam olarak yeniden yönlendirme URI'leri Portalı'nda kayıtlı biriyle eşleşmelidir. Varsayılan olarak `window.location.href`.

- **postLogoutRedirectUri**: İsteğe bağlı.  Kullanıcıya yeniden yönlendiren `postLogoutRedirectUri` oturumu kapattıktan sonra. Varsayılan, `redirectUri` değeridir.

- **navigateToLoginRequestUrl**: İsteğe bağlı. Oturum açtıktan sonra Başlangıç sayfası için varsayılan gezinti devre dışı bırakma olanağı. Varsayılan değer True'dur. Bu yalnızca yeniden yönlendirme akış için kullanılır.

- **cacheLocation**: İsteğe bağlı.  Tarayıcı depolama ya da ayarlar `localStorage` veya `sessionStorage`. Varsayılan, `sessionStorage` değeridir.

- **storeAuthStateInCookie**: İsteğe bağlı.  Bu bayrak MSAL.js v0.2.2 olarak ilişkin bir düzeltme kullanıma sunulmuştur [kimlik doğrulama döngüsü sorunları](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#1-issues-due-to-security-zones) Microsoft Internet Explorer ve Microsoft Edge. Etkinleştirme bayrağını `storeAuthStateInCookie` true yararlanma bu düzeltmek için. Bu etkinleştirildiğinde, MSAL.js tarayıcı tanımlama bilgileri kimlik doğrulama akışları doğrulanması için gerekli kimlik doğrulama isteği durumu depolar. Varsayılan olarak bu bayrağı ayarlanır `false`.

- **Günlükçü**: İsteğe bağlı.  Günlükçü nesnesi kullanma ve günlükleri özel bir şekilde yayımlamak için geliştirici tarafından sağlanan bir geri çağırma örneği. Günlükçü nesne geçirme hakkında daha fazla bilgi için bkz [msal.js ile günlüğe kaydetme](msal-logging.md).

- **loadFrameTimeout**: İsteğe bağlı.  Azure AD'den bir belirteç yenileme yanıt düşünülmesi gereken önce işlem yapılmadan geçen milisaniye sayısını zaman aşımına uğradı. Varsayılan değer 6 saniyedir.

- **tokenRenewalOffsetSeconds**: İsteğe bağlı. Pencerenin uzaklık kaldığında belirteci yenilemek için gereken ayarlar milisaniye sayısı. 300 milisaniye varsayılandır.

Bunlar yalnızca MSAL Angular sarmalayıcı kitaplıktan geçirilmesi için geçerlidir:
- **unprotectedResources**: İsteğe bağlı.  Korumasız kaynaklar bir URI'leri dizisi. MSAL, bu URI'sine sahip Giden istekleri için bir belirteç eklemez. Varsayılan olarak `null`.

- **protectedResourceMap**: İsteğe bağlı.  Bu, kapsamlara erişim belirteçlerinde web API çağrıları otomatik olarak eklemek için MSAL tarafından kullanılan kaynakların eşliyor. Tek bir erişim belirteci, kaynak için elde edilir. Gibi belirli kaynak yolu eşleyebilmeniz: {"https://graph.microsoft.com/v1.0/me", ["user.read"]}, veya kaynak uygulama URL'si: {"https://graph.microsoft.com/", ["user.read", "mail.send"]}. Bu, CORS çağrıları için gereklidir. Varsayılan olarak `null`.
