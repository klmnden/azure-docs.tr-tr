---
title: Web API'leri (uygulama için bir belirteç almak) - Microsoft kimlik platformu çağıran bir web uygulaması
description: Web API'ları (uygulama için bir belirteç alınırken) çağıran bir Web uygulaması oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 653db995000308bb3ef78a9183696cd9d8ed1056
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074808"
---
# <a name="web-app-that-calls-web-apis---acquire-a-token-for-the-app"></a>Web API'leri çağıran uygulama web - uygulama için bir belirteç Al

İstemci uygulama nesnesi derlediğiniz artık sonra web API'leri çağırmak için kullanacağınız bir belirteç almak için kullanacaksınız. ASP.NET veya ASP.NET Core web API denetleyicisi ardından yapılır çağrılıyor. Bu, ilgili değil:

- Web API'si kullanarak belirteç önbelleği için bir belirteç alınıyor. Bunun için çağırın `AcquireTokenSilent`
- Erişim belirteci ile korumalı API çağırma

## <a name="aspnet-core"></a>ASP.NET Core

Denetleyici yöntemleri tarafından korunan bir `[Authorize]` kullanıcılar Web uygulamasını kullanmak için kimliğinin zorlar özniteliği. Microsoft Graph çağıran kodu

```CSharp
[Authorize]
public class HomeController : Controller
{
 ...
}
```

Microsoft Graph'i çağırmaya yönelik bir belirteç alır HomeController eylemin basitleştirilmiş bir kod aşağıda verilmiştir.

```CSharp
public async Task<IActionResult> Profile()
{
 var application = BuildConfidentialClientApplication(HttpContext, HttpContext.User);
 string accountIdentifier = claimsPrincipal.GetMsalAccountId();
 string loginHint = claimsPrincipal.GetLoginHint();

 // Get the account
 IAccount account = await application.GetAccountAsync(accountIdentifier);

 // Special case for guest users as the Guest iod / tenant id are not surfaced.
 if (account == null)
 {
  var accounts = await application.GetAccountsAsync();
  account = accounts.FirstOrDefault(a => a.Username == loginHint);
 }

 AuthenticationResult result;
 result = await application.AcquireTokenSilent(new []{"user.read"}, account)
                            .ExecuteAsync();
 var accessToken = result.AccessToken;
 ...
 // use the access token to call a web API
}
```

2\. Aşama bu senaryo için gerekli kodu toplamı ayrıntılarında anlamak istiyorsanız, bkz [2-1-Web Uygulama çağrıları Microsoft Graph](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-1-Call-MSGraph) adımında [ms-kimlik-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) Öğreticisi

Gibi çok sayıda ek karmaşıklık vardır:

- bir belirteç önbelleği Web uygulamasının (öğretici sunmak çeşitli uygulamalar) uygulama
- Kullanıcı işaretleri genişletme zaman önbellekten hesap kaldırılıyor
- Artımlı onay sahip dahil olmak üzere birkaç API çağırma

## <a name="aspnet"></a>ASP.NET

ASP.NET'te benzer şunlardır:

- Bir [Authorize] özniteliği tarafından korunan bir denetleyici eylemi, Kiracı kimliği ve kullanıcı kimliği ayıklamak `ClaimsPrincipal` denetleyici üyesi (ASP.NET kullanan `HttpContext.User`)
- İsteğe bağlı olarak burada bir MSAL.NET oluşturur `IConfidentialClientApplication`
- BT çağrı `AcquireTokenSilent` gizli istemci uygulamasının yöntemi 

kodu için ASP.NET Core gösterilen koda benzer.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web API çağrısı](scenario-web-app-call-api-call-api.md)
