---
title: Web uygulaması web API'leri (oturum açma) - Microsoft kimlik platformu çağrıları
description: Çağrıları web API'leri (oturum açma) bir Web uygulaması oluşturmayı öğrenin
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
ms.openlocfilehash: 663cea72eb620217ad5fa8925d3bb00eedbf890c
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074568"
---
# <a name="web-app-that-calls-web-apis---sign-in"></a>Web API - oturum açma çağıran bir web uygulaması

Web uygulamanıza oturum açma ekleme biliyorsunuzdur. Uygulamasında bilgi [Web uygulamasına oturum açma oturum açtığında kullanıcılar - ekleme](scenario-web-app-sign-user-sign-in.md).

Ne buraya farklı, kullanıma, bu uygulama veya herhangi bir uygulamadan kullanıcı oturumu açtığı, kullanıcıyla ilişkili belirteçleri belirteç önbelleğinin kaldırmak istediğiniz.

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>Oturum kapatma - çoklu oturum kapatma sonrasında geri çağırma kesintiye

Uygulamanızı keserek sonra `logout` olay örneği için girişi oturumunuz hesapla ilişkili belirteç önbelleği temizleyin. Biz (hakkında bir Web API'sini çağıran Web uygulaması), bu öğreticinin ikinci bölümünde göreceksiniz, web uygulamasına erişim belirteçleri şu kullanıcı için bir önbellekte depolar. Kesintiye sonra `logout` geri çağırma kullanıcı belirteci önbelleğinden kaldırmak, web uygulamanızı sağlar. Bu mekanizma gösterilmiştir `AddMsal()` yöntemi [StartupHelper.cs L137 143](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L137-L143)

**Oturum kapatma URL'si** çoklu oturum kapatma uygulamak uygulamanızı sağlar kaydettiğinize göre. Microsoft kimlik platformu `logout` uç nokta çağıracaktır **oturum kapatma URL'si** uygulamanızla kayıtlı. Bu çağrı oturum kapatma web uygulamanızdan veya başka bir web uygulaması ya da tarayıcıya başlatıldı olur. Daha fazla bilgi için [çoklu oturum kapatmak](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc#single-sign-out) kavramsal belgelerinde.

```CSharp
public static IServiceCollection AddMsal(this IServiceCollection services, IEnumerable<string> initialScopes)
{
    services.AddTokenAcquisition();

    services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
    {
     ...
        // Handling the sign-out: removing the account from MSAL.NET cache
        options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
        {
            // Remove the account from MSAL.NET token cache
            var _tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
            await _tokenAcquisition.RemoveAccount(context);
        };
    });
    return services;
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Web uygulaması için bir belirteç alınırken](scenario-web-app-call-api-acquire-token.md)
