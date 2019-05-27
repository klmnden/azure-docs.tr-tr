---
title: MSAL.js ADAL.js arasındaki farkları | Azure
description: JavaScript (MSAL.js) ve JavaScript (ADAL.js) için Azure AD kimlik doğrulama kitaplığı kullanılacağı seçme için Microsoft kimlik doğrulama kitaplığı arasındaki farklar hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10b5169d3f06e265b3effa3ec18ad8e4f69959d3
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121964"
---
# <a name="differences-between-msal-js-and-adal-js"></a>MSAL JS ADAL JS arasındaki farklar

JavaScript (MSAL.js) için Microsoft kimlik doğrulama Kitaplığı hem JavaScript (ADAL.js) için Azure AD kimlik doğrulama kitaplığı, varlıklar Azure AD kimlik doğrulaması ve Azure AD'den belirteçler istemek için kullanılır. Şimdiye kadar Çoğu geliştirici, Azure AD kimlikleri (iş ve Okul hesapları) belirteçleri kullanarak ADAL isteyerek kimlik doğrulaması (v1.0) geliştiricileri için Azure AD ile çalıştı. Şimdi, MSAL.js kullanılarak, Microsoft kimlik Platformu (v2.0) aracılığıyla Microsoft kimlikleri (Azure AD kimlikleri ve Microsoft hesapları ve sosyal ve yerel hesapları Azure AD B2C ile) daha geniş bir dizi doğrulayabilir.

Bu makalede, Microsoft kimlik doğrulama kitaplığı (MSAL.js) JavaScript için JavaScript (ADAL.js) için Azure AD kimlik doğrulama kitaplığı arasında seçim yapma açıklar ve iki kitaplıkları karşılaştırır.

## <a name="choosing-between-adaljs-and-msaljs"></a>ADAL.js ile MSAL.js arasında seçim yapma

Çoğu durumda, MSAL.js ve Microsoft kimlik platformu kullanmak istediğiniz Microsoft kimlik doğrulama kitaplıkları en yeni nesil olduğu. MSAL.js kullanılarak, uygulamanızı Azure AD (iş ve Okul hesaplarında) (Kişisel) Microsoft hesapları (MSA) ile oturum açan kullanıcılar için belirteçlerini almak veya Azure AD B2C.

V1.0 uç noktası (ve ADAL.js) biliyorsanız, okumak isteyebilirsiniz [v2.0 uç nokta hakkında farklı nedir?](active-directory-v2-compare.md).

Ancak, önceki sürümleriyle kullanıcılar oturum açmak uygulamanızın ihtiyaç duyduğu ADAL.js kullanırsanız yine [Active Directory Federasyon Hizmetleri (ADFS)](/windows-server/identity/active-directory-federation-services).

## <a name="key-differences-in-authentication-with-msaljs"></a>Anahtar kimlik doğrulaması ile MSAL.js farklılıkları

### <a name="core-api"></a>Core API'si

* ADAL.js kullanan [Authenticationcontext'i](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Config-authentication-context#authenticationcontext) yetkilendirme sunucusu veya kimlik sağlayıcısı aracılığıyla yetkilisi URL'sini bağlantısı, uygulamanızın bir örneğini temsili olarak. Tam, MSAL.js API kullanıcı aracısının istemci uygulamasına tasarlanmıştır (ortak istemci uygulaması, istemci kodu yürütülürse bir web tarayıcısı gibi bir kullanıcı aracısı şeklinde). Sağladığı `UserAgentApplication` yetkilendirme sunucusu ile kimlik doğrulaması bağlamı uygulamanın bir örneğini temsil eden sınıf. Daha fazla ayrıntı için [MSAL.js kullanılarak başlatılamıyor](msal-js-initializing-client-applications.md).

* ADAL.js içinde belirteçlerini almak için yöntemleri kümesinde tek bir yetkisine sahip ilişkilendirilmiş `AuthenticationContext`. MSAL.js içinde ne ayarlanır değerinden farklı yetkilisi değerleri alma belirteci isteklerini sürebilir `UserAgentApplication`. Bu, MSAL.js almak ve birden çok Kiracı ve aynı uygulamada kullanıcı hesapları için ayrı ayrı önbellek belirteçleri sağlar.

* Yöntem almak ve belirteçleri kullanıcılar istemeden sessizce yenileme adlı `acquireToken` ADAL.js içinde. Bu yöntem MSAL.js adlandırılır `acquireTokenSilent` işlevselliği daha açıklayıcı olması.

### <a name="authority-value-common"></a>Yetkili değeri `common`

V1.0 içinde kullanarak `https://login.microsoftonline.com/common` yetkilisi yapılması (tüm kuruluşlar için) herhangi bir Azure AD hesabıyla oturum açmasına izin verir.

V2.0 içinde kullanarak `https://login.microsoftonline.com/common` yetkilisi, herhangi bir Azure AD kuruluş hesabı veya bir Microsoft Kişisel hesabı (MSA) kullanarak oturum açma olanağı sağlayacaktır. Oturum açma yalnızca Azure AD hesapları (aynı davranışı ADAL.js ile) sınırlamak için kullanmanız gerekir `https://login.microsoftonline.com/organizations`. Ayrıntılar için bkz `authority` yapılandırma seçeneğinde [MSAL.js kullanılarak başlatılamıyor](msal-js-initializing-client-applications.md).

### <a name="scopes-for-acquiring-tokens"></a>Belirteçlerini almak için kapsamları
* Kimlik doğrulama isteklerini belirteçlerini almak için kaynak parametresi yerine kapsamı

    v2.0 protokol, kaynak istekleri yerine kapsamları kullanır. MS Graph gibi bir kaynak için izinlere sahip belirteçler istemek uygulamanızın ihtiyaç duyduğu, diğer bir deyişle, fark kitaplığı yöntemlere iletilen değerleri şu şekildedir:

    v1.0: resource=https://graph.microsoft.com

    v2.0: kapsam = https://graph.microsoft.com/User.Read

    Herhangi bir kaynak API URI'sini şu biçimde kullanarak API için kapsamları isteyebilir: appidURI/kapsam örneğin: https:\//mytenant.onmicrosoft.com/myapi/api.read

    MS Graph API, bir kapsam değeri için yalnızca `user.read` eşlendiği https://graph.microsoft.com/User.Read ve birbirlerinin yerine kullanılabilir.

    ```javascript
    var request = {
        scopes = ["User.Read"];
    };

    acquireTokenPopup(request);   
    ```

* Dinamik kapsam artımlı onay.

    V1.0 kullanan uygulamalar oluştururken, uygulama kullanıcının oturum açma sırasında onay gereken izinler (statik kapsam) tam kümesini kaydetmek gerekli. V2.0, kapsam parametresi, istediğiniz zaman izin istemek için kullanabilirsiniz. Bunlar, dinamik kapsam olarak adlandırılır. Bu, kullanıcının kapsamlara artımlı rıza sağlamanın izin verir. Bu nedenle başında, yalnızca uygulamanız için oturum açmak için kullanıcının istediğiniz ve her tür erişim gerekmez, bunu yapabilirsiniz. Daha sonra kullanıcının Takvim okuma yeteneği gerekiyorsa, ardından acquireToken yöntemleri Takvim kapsamda istek ve kullanıcının onay alın. Örneğin:

    ```javascript
    var request = {
        scopes = ["https://graph.microsoft.com/User.Read", "https://graph.microsoft.com/Calendar.Read"];
    };

    acquireTokenPopup(request);   
    ```

* V1.0 API için kapsamları

    Belirteçler için V1.0 MSAL.js kullanılarak API'leri alırken ekleyerek API'yi kayıtlı tüm statik kapsamlar isteyebilir `.default` için kapsam olarak API uygulama kimliği URI'si. Örneğin:

    ```javascript
    var request = {
        scopes = [ appidURI + "/.default"];
    };

    acquireTokenPopup(request);
    ```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [v1.0 ve v2.0 karşılaştırma](active-directory-v2-compare.md).
