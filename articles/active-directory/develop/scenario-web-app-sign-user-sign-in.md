---
title: Microsoft kimlik Platformu (oturum açma) - kullanıcılar oturum açtığında web uygulaması
description: Oturum açtığında kullanıcıların (oturum açma) bir web uygulaması oluşturmayı öğrenin
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
ms.openlocfilehash: 3fb7fbba7ec48da580d2a630ae51aa20b3307848
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074628"
---
# <a name="web-app-that-signs-in-users---sign-in"></a>Kullanıcılar oturum açtığında uygulama web - oturum aç

Oturum açma web uygulamanız için kod oturum açtığında, kullanıcıların nasıl ekleneceğini öğrenin.

## <a name="sign-in"></a>Oturum açma

Önceki makalede yaptıklarımız kod [uygulamanın kod yapılandırma](scenario-web-app-sign-user-app-configuration.md) yeterlidir uygulamak için oturum kapatma. Kullanıcı uygulamanızda oturum açmış sonra büyük olasılıkla oturumu kapatmak bunları etkinleştirmek istiyorsunuz. ASP.NET core oturum kapatma sizin yerinize çözer.

## <a name="what-sign-out-involves"></a>Ne oturumunuzu içerir

Bir web uygulamasından oturum kapatma web uygulamasının durumundan birden fazla oturum açma hesabı hakkında bilgi kaldırma hakkında olur.
Web uygulaması, ayrıca kullanıcı için Microsoft kimlik platformu v2.0 yönlendirmelisiniz `logout` oturumu kapatmak için uç nokta. Web uygulamanızı kullanıcıya ne zaman yeniden yönlendiren `logout` uç nokta, bu uç nokta, kullanıcının oturumunu bir tarayıcıdan temizler. Uygulamanız için gitmedi varsa `logout` uç noktası, kullanıcı sağlamalarını uygulamanıza kimlik bilgilerini yeniden girmeye gerek kalmadan, geçerli tek bir oturum açma oturumu Microsoft Identity platform v2.0 uç noktası ile olmadığı için.

Daha fazla bilgi için bkz. [oturum kapatma isteği Gönder](v2-protocols-oidc.md#send-a-sign-out-request) konusundaki [Microsoft Identity platform v2.0 ve Openıd Connect protokolünü](v2-protocols-oidc.md) kavramsal belgeler.

## <a name="application-registration"></a>Uygulama kaydı

Uygulama kaydı sırasında kaydolduğundan bir **URI oturum kapatma sonrası**. Müşterilerimize öğreticide kaydettiğiniz `https://localhost:44321/signout-oidc` içinde **oturum kapatma URL'si** alanını **Gelişmiş ayarlar** konusundaki **kimlik doğrulaması** sayfası. Ayrıntılar için bkz [ webApp uygulamayı kaydetme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg#register-the-webapp-app-webapp)

## <a name="aspnet-core-code"></a>ASP.NET Core kod

### <a name="signout-button"></a>Oturum kapatma düğmesi

Oturumu kapatma düğmesi de sağlanmaktadır `Views\Shared\_LoginPartial.cshtml` ve yalnızca (kullanıcının daha önce imzalanmış) bir kimliği doğrulanmış hesap olduğunda görüntülenir.

```html
@using Microsoft.Identity.Web
@if (User.Identity.IsAuthenticated)
{
    <ul class="nav navbar-nav navbar-right">
        <li class="navbar-text">Hello @User.GetDisplayName()!</li>
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignOut">Sign out</a></li>
    </ul>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="AzureAD" asp-controller="Account" asp-action="SignIn">Sign in</a></li>
    </ul>
}
```

### <a name="signout-action-of-the-accountcontroller"></a>`Signout()` eylemi `AccountController`

Tuşuna basarak **oturumunuzu** web uygulaması Tetikleyicileri düğmesinde `SignOut` eylem `Account` denetleyicisi. ASP.NET core şablonları önceki sürümlerinde `Account` denetleyicisi web uygulaması ile katıştırılmış, ancak artık ASP.NET Core framework parçası olarak artık durum budur. 

Kodu `AccountController` ASP.NET core depodan kullanılabilir [AccountController.cs](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Controllers/AccountController.cs). Hesabı Denetimi:

- Bir Openıd kümeleri yeniden yönlendirme URI'si `/Account/SignedOut` böylece geri Azure AD'ye oturumunuzu gerçekleştirdiğinde denetleyicisi olarak adlandırılır
- Çağrıları `Signout()`, Microsoft kimlik platformu başvurun Openıdconnect ara yazılım sağlar `logout` uç noktası olan:

  - Tarayıcı oturumu tanımlama bilgisinin temizler ve
  - Son aramaları geri **oturum kapatma URL'si**, hangi) varsayılan olarak, imzalanmış görünüm sayfası görüntüler [SignedOut.html](https://github.com/aspnet/AspNetCore/blob/master/src/Azure/AzureAD/Authentication.AzureAD.UI/src/Areas/AzureAD/Pages/Account/SignedOut.cshtml) aynı zamanda ASP.NET Core bir parçası olarak sağlanır.

### <a name="intercepting-the-call-to-the-logout-endpoint"></a>Çağrı kesintiye `logout` uç noktası

ASP.NET Core Openıdconnect ara yazılımı Microsoft kimlik platformu çağrısı ıntercept olanak tanır `logout` adlı Openıdconnect olay sağlayarak uç nokta `OnRedirectToIdentityProviderForSignOut`. Web uygulaması oturumunuzu, kullanıcıya sunulan için hesabı seçin iletişim kutusunu önlemek için denemeniz için kullanır. Bu durdurma yapılır `AddAzureAdV2Authentication` , `Microsoft.Identity.Web` yeniden kullanılabilir bir kitaplık. Bkz: [StartupHelpers.cs L58-L66](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L58-L66)

```CSharp
public static IServiceCollection AddAzureAdV2Authentication(this IServiceCollection services,
                                                            IConfiguration configuration)
{
    ...
    services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
    {
        ...
        options.Authority = options.Authority + "/v2.0/";
        ...
        // Attempt to avoid displaying the select account dialog when signing out
        options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
        {
            var user = context.HttpContext.User;
            context.ProtocolMessage.LoginHint = user.GetLoginHint();
            context.ProtocolMessage.DomainHint = user.GetDomainHint();
            await Task.FromResult(0);
        };
    }
}
```

## <a name="aspnet-code"></a>ASP.NET kodu

ASP.NET'te, oturum kapatma (örneğin AccountController) denetleyicisinde SignOut() yönteminden tetiklenir. Bu yöntem ASP.NET framework (aykırı ASP.NET Core ne) bir parçası değildir ve zaman uyumsuz kullanmaz ve İşte bu da:

- bir Openıd oturum kapatma sınaması gönderir
- Önbellek
- istediği sayfasına yönlendirir

```CSharp
/// <summary>
/// Send an OpenID Connect sign-out request.
/// </summary>
public void SignOut()
{
 HttpContext.GetOwinContext()
            .Authentication
            .SignOut(CookieAuthenticationDefaults.AuthenticationType);
 MsalAppBuilder.ClearUserTokenCache();
 Response.Redirect("/");
}
```

## <a name="protocol"></a>Protocol

ASP.NET Core veya ASP.NET kullanmak istemiyorsanız, kullanılabilir Protokolü belgeleri, göz atabilirsiniz [Open ID Connect](./v2-protocols-oidc.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-web-app-sign-user-production.md)
