---
title: Azure AD v2.0 ASP.NET Core web uygulaması hızlı başlangıç | Microsoft Docs
description: Microsoft oturum açma ASP.NET Core Web Openıd Connect'i kullanarak uygulama üzerinde uygulama hakkında bilgi edinin
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4ab3d0b74e8305d67af862020197c69b15221086
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830234"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Oturum açma Microsoft ile bir ASP.NET Core web uygulamasına ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta nasıl bir ASP.NET Core Web uygulaması kişisel oturum gösteren kod örneği içerir (hotmail.com, live.com, diğerleri) ve iş ve Okul hesapları herhangi bir Azure Active Directory örneğinden.

![Bu Hızlı Başlangıcın oluşturduğu örnek uygulama nasıl çalışır?](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.png)


> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Uygulamanızı kaydetme ve hızlı başlangıç uygulamanızı indirme
>
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Uygulamanızı ve kod örneğinizi kaydedip yapılandırma
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> 
> 1. [Microsoft Uygulama Kayıt Portalı](https://apps.dev.microsoft.com/portal/register-app)'na gidin.
> 1. Uygulamanız için bir ad girin, **Destekli Kurulum** seçeneğinin işaretli olmadığından emin olun ve **Oluştur**’a tıklayın.
> 1. `Add Platform` seçeneğine tıklayın ve `Web` öğesini seçin.
> 1. **Örtük Akışa İzin Ver**’in *işaretli* olduğundan emin olun.
> 1. **Yeniden Yönlendirme URL’leri** için `http://localhost:3110/` girin.
> 1. Sayfanın en altına kaydırın ve **Kaydet**’e tıklayın.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için `http://localhost:3110/` gibi bir yanıt URL’si eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-core-webapp/green-check.png) Uygulamanız bu özellikle yapılandırıldı

#### <a name="step-2-download-your-aspnet-core-project"></a>2. adım: ASP.NET Core projenizi indirin

- [ASP.NET Core projesi indirme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>3. Adım: Projenizi yapılandırma

1. Örneğin, yerel bir klasöre yakın kök klasörü - zip dosyasını ayıklayın **C:\Azure-Samples**
1. Visual Studio 2017 kullanırsanız, projeyi (isteğe bağlı) Visual Studio'da açın.
1. Düzen **appsettings.json** ve değeri değiştirin `ClientId` yeni kaydettiğiniz uygulamadan uygulama kimliği:

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "common"
    ```
1. Uygulamanız bir *tek kiracılı uygulama* (yalnızca geçerli dizinde hesapları hedefleyen) içinde **appsettings.json** dosya, değeri Bul `TenantId` değiştirin`common`ile **Kiracı kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com). Kiracı adını **Genel Bakış sayfasından** alabilirsiniz.

## <a name="more-information"></a>Daha fazla bilgi

Bu bölümde, kullanıcıların oturumunu açmak için gereken koda genel bir bakış sağlanır. Bu kod, main bağımsız değişkenleri, çalışma şeklini anlamanız yararlı olabilir ve ayrıca mevcut bir ASP.NET Core uygulamasına oturum açma eklemek istiyorsanız.

### <a name="startup-class"></a>Başlangıç sınıfı

*Microsoft.AspNetCore.Authentication* ara yazılım barındırma işlemi başlatıldığında çalıştırılan bir başlangıç sınıfı kullanır:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddAzureAd(options => Configuration.Bind("AzureAd", options))
    .AddCookie();

    services.AddMvc();
}
```

Yöntem `AddAuthentication` yanı sıra tarayıcı senaryosunda kullanılan tanımlama bilgisi tabanlı kimlik doğrulaması - eklemek için Openıdconnect sınama kümesi için hizmet yapılandırır. 

İçeren satırda `.AddAzureAd` uygulamanızı Azure Active Directory kimlik doğrulaması ekler.

Bu, dosyanın yanı sıra **AzureAdAuthenticationBuilderExtensions.cs** genişletme yöntemi AzureAd kimlik doğrulaması ardışık düzenine ekler. Bu genişletme yöntemi, Azure AD kimlik doğrulaması için gerekli öznitelikler yapılandırın. `Configure` Yönteminde `IConfigureNamedOptions` arabirimi aşağıdakileri içerir:

```csharp
public void Configure(string name, OpenIdConnectOptions options)
{
    options.ClientId = _azureOptions.ClientId;
    options.Authority = $"{_azureOptions.Instance}common/v2.0";   // V2 specific
    options.UseTokenLifetime = true;
    options.RequireHttpsMetadata = false;
    options.TokenValidationParameters.ValidateIssuer = false;     // accept several tenants (here simplified)
}
```
> |Konum  |  |
> |---------|---------|
> |ClientId     |Azure portalında uygulama kimliği uygulamadan kayıtlı|
> |Yetkili | Kimlik doğrulaması STS uç noktası fo kullanıcı. Çoğunlukla, genel bulut için https://login.microsoftonline.com/{tenant}/v2.0; burada {tenant}, kiracınızın adı, kiracınızın kimliği veya ortak uç noktaya başvuru olarak *common* değeridir (çok kiracılı uygulamalarda kullanılır)|
> |Jwtformat |Kimlik doğrulama tanımlama, kimlik doğrulama belirteci eşleşmelidir gösterir|
> |RequireHttpsMetadata     |HTTPS'yi zorunlu tutmak için meta veri adresini veya yetkilisi. Bu değeri değiştirmek için önerilir `True`|
> |Tokenvalidationparameters değerini     | Belirteç doğrulaması için parametre listesi. Bu durumda, `ValidateIssuer` ayarlanır `false` herhangi bir kişisel veya iş veya Okul hesabı oturum açma işlemleri kabul ettiğinizi belirtmek için|

### <a name="initiate-an-authentication-challenge"></a>Kimlik doğrulaması sınamasını başlatma

Kullanıcının bir kimlik doğrulama sınaması, denetleyicideki olduğu gibi talep ederek oturum açmasını zorunlu kılabilirsiniz **AccountController.cs**:

```csharp
public IActionResult SignIn()
{
    var redirectUrl = Url.Action(nameof(HomeController.Index), "Home");
    return Challenge(
        new AuthenticationProperties { RedirectUri = redirectUrl },
        OpenIdConnectDefaults.AuthenticationScheme);
}
```

> [!TIP]
> Yukarıdaki yöntemini kullanarak bir kimlik doğrulaması sınaması isteğinde bir görünüm kimliği doğrulanmış ve doğrulanmamış kullanıcılar için kullanılabilir olmasını istediğiniz zaman isteğe bağlıdır ve commonsly kullanılır. Denetleyicileri, sonraki bölümde açıklanan yöntemi kullanarak koruyabilirsiniz.

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Yapı denetleyicisini veya yapı denetleyicisi kullanarak methodss Koruyabileceğiniz `[Authorize]` özniteliği. Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılar - kimlik doğrulama sınaması, herhangi bir kullanıcı kimliği doğrulanmamış denetleyici erişmeye başlatılabildiğinden anlamına gelir vererek denetleyici veya yöntemleri için erişimi kısıtlar.

## <a name="next-steps"></a>Sonraki Adımlar

Bu ASP.NET Core Hızlı Başlangıç için yeni bir ASP.NET Core Web uygulaması için kimlik doğrulaması ekleme hakkında yönergeler dahil olmak üzere daha fazla bilgi - GitHub deposunu denetleyin:

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması kod örneği](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]