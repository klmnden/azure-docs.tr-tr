---
title: Microsoft kimlik doğrulama kitaplığı (MSAL) hakkında bilgi edinin | Azure
description: Microsoft Authentication Library (MSAL), uygulama geliştiricilerin güvenli Web API'lerini çağırma için belirteçlerini almak sağlar. Bu Web API'leri, Microsoft Graph, diğer Microsoft APIS, üçüncü taraf Web API'leri veya kendi Web API'nizi olabilir. MSAL, birden çok uygulama mimarileri ve platformları destekler.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4bd3e7d47b6e3083af6f388a5cd750da240a76b6
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392895"
---
# <a name="overview-of-microsoft-authentication-library-msal"></a>Microsoft kimlik doğrulama kitaplığı (MSAL) genel bakış
Microsoft Authentication Library (MSAL), geliştiricilerin almaya sağlar [belirteçleri](developer-glossary.md#security-token) Microsoft kimlik platformu endpoint erişmek için Web API'leri güvenli. Bu Web API'leri, Microsoft Graph, diğer Microsoft APIS, üçüncü taraf Web API'leri veya kendi Web API'nizi olabilir. MSAL, .NET, JavaScript, Android ve iOS, birçok farklı uygulama mimarileri ve platformları destekler için kullanılabilir.

MSAL, çeşitli platformlar için tutarlı bir API ile belirteçlerini almak için çeşitli yollar sunar. MSAL kullanarak aşağıdaki avantajları sağlar:

* Doğrudan OAuth kitaplıkları veya protokole göre uygulamanızda kullanılması gerekmez.
* Bir kullanıcı adına veya bir uygulama (platforma uygulanabilirse) adına belirteçleri devralır.
* Bir belirteç önbelleğini korur ve süresi dolacak şekilde Kapat olduklarında belirteçleri sizin için yenilenir. Belirteç süre sonu kendi işleme gerek yoktur.
* Hangi hedef kitle, uygulamanızın oturum açma (kuruluş, birden fazla düzenlemeler, iş ve Okul ve kişisel Microsoft hesapları, sosyal kimliklerinizi Azure AD B2C, bağımsız ve Ulusal bulutlarda kullanıcılar ile) istediğinizi belirtmenize yardımcı olur.
* Yapılandırma dosyalarını uygulamanızdan ayarlamanıza yardımcı olur.
* Eyleme dönüştürülebilir özel durumlar, günlüğe kaydetme ve telemetri göstererek uygulamanızı gidermenize yardımcı olur.

## <a name="application-types-and-scenarios"></a>Uygulama türleri ve senaryolar
MSAL kullanarak, bir belirteç uygulama türleri arasında bir sayı elde: web uygulamaları, web API'leri, tek sayfa uygulamaları (JavaScript), mobil ve yerel uygulamaları ve Daemon'ları ve sunucu tarafı uygulamalar. 

MSAL, aşağıdakiler dahil birçok uygulama senaryolarda kullanılabilir:

* [Tek sayfa uygulamaları (JavaScript)](scenario-spa-overview.md) 
* [Web uygulaması kullanıcıların imzalama](scenario-web-app-sign-user-overview.md)
* [Web uygulaması kullanıcı olarak oturum ve kullanıcı adına bir web API'si çağırma](scenario-web-app-call-api-overview.md)
* [Yalnızca kimliği doğrulanmış kullanıcıların erişebilmesi için bir web API'sini koruma](scenario-protected-web-api-overview.md)
* [Web API'si oturum açmış kullanıcı adına başka bir aşağı akış web API'si çağırma](scenario-web-api-call-api-overview.md)
* [Masaüstü uygulaması oturum açmış kullanıcı adına bir web API'si çağırma](scenario-desktop-overview.md)
* [Mobil uygulama etkileşimli olarak oturum açmış kullanıcı adına bir Web API'sini çağıran](scenario-mobile-overview.md).
* [Masaüstü/hizmet daemon uygulamasının kendisi adına Web API'si çağırma](scenario-daemon-overview.md)

## <a name="languages-and-frameworks"></a>Diller ve çerçeveler

| Kitaplık | Desteklenen platformlar ve çerçeveler|
| --- | --- | 
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/>[MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)| .NET framework, .NET Core, Xamarin Android, Xamarin iOS, Evrensel Windows platformu|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/>[MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)| AngularJS, Ember.js veya Durandal.js gibi JavaScript/TypeScript çerçeveleri|
| ![Android için MSAL](media/sample-v2-code/logo_Android.png) <br/>[MSAL Android (Önizleme)](https://github.com/AzureAD/microsoft-authentication-library-for-android)|Android|
| ![İOS için MSAL](media/sample-v2-code/logo_iOS.png) <br/>[MSAL. Objective-C (Önizleme)](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|iOS|

## <a name="differences-between-adal-and-msal"></a>ADAL MSAL arasındaki farklar
Active Directory Authentication Library (ADAL), burada MSAL Microsoft kimlik Platformu (v2.0) uç noktası ile tümleştiren geliştiriciler (v1.0) uç noktası için Azure AD ile tümleştirilir. V1.0 uç noktası, iş hesapları, ancak değil kişisel hesapları destekler. V2.0 uç noktası kişisel Microsoft hesapları ve iş hesapları bir tek bir kimlik doğrulama sisteminde birleşmesidir. Ayrıca, MSAL ile Azure AD B2C kimlik doğrulamalarını da alabilirsiniz.

Hakkında daha ayrıntılı bilgi için okuma [ADAL.NET MSAL.NET için geçiş](msal-net-migration.md) ve [ADAL.js MSAL.js için geçiş](msal-compare-msal-js-and-adal-js.md).

            