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
ms.openlocfilehash: ba67acec778a48c084897095aa457e5637240a57
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227457"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Oturum açma Microsoft ile bir ASP.NET Core web uygulamasına ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta nasıl bir ASP.NET Core Web uygulaması kişisel oturum gösteren kod örneği içerir (hotmail.com, live.com, diğerleri) ve iş ve Okul hesapları herhangi bir Azure Active Directory örneğinden.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalışır?](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.png)


> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Uygulamanızı kaydedin ve hızlı başlangıç uygulamanızı indirin
>
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Kaydetme ve yapılandırma, uygulama ve kod örneği
> #### <a name="step-1-register-your-application"></a>1. adım: uygulamanızı kaydetme
> 
> 1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app).
> 1. Uygulamanız için bir ad girin, seçeneği emin olmak **destekli Kurulum** olarak işaretli değildir ve tıklayın **Oluştur**.
> 1. Tıklayın `Add Platform`, ardından `Web`.
> 1. Emin **izin örtük akış** olduğu *işaretli*.
> 1. İçinde **yeniden yönlendirme URL'lerini**, girin `https://localhost:3110/`.
> 1. Sayfanın en alta kadar kaydırın ilerleyecek ve **Kaydet**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. adım: uygulamanızı Azure portalında yapılandırma
> Çalışmak bu hızlı başlangıç için kod örneği için bir yanıt URL'si olarak eklemek istediğiniz `http://localhost:3110/`.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Benim için bu değişiklik yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Önceden yapılandırılmış](media/quickstart-v2-aspnet-core-webapp/green-check.png) uygulamanız bu öznitelikle yapılandırılana

#### <a name="step-2-download-your-aspnet-core-project"></a>2. adım: ASP.NET Core projenizi indirin

- [ASP.NET Core projesi indirme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>3. adım: Proje yapılandırma

1. Zip dosyasını yerel bir klasöre ayıklar (örneğin, **C:\Azure-Samples**)
1. Visual Studio 2017 kullanırsanız, projeyi (isteğe bağlı) Visual Studio'da açın.
1. Düzen **appsettings.json** ve değeri değiştirin `ClientId` yeni kaydettiğiniz uygulamadan uygulama kimliği:

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "common"
    ```
1. Uygulamanız bir *tek kiracılı uygulama* (yalnızca geçerli dizinde hesapları hedefleyen) içinde **appsettings.json** dosya, değeri Bul `TenantId` değiştirin`common`ile **Kiracı kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com). Kiracı adı edinebilirsiniz **genel bakış sayfasında**.

## <a name="more-information"></a>Daha fazla bilgi

Bu bölümde, oturum açma için gerekli kodu genel bir fikir veren kullanıcılar. Bu kod, main bağımsız değişkenleri, çalışma şeklini anlamanız yararlı olabilir ve ayrıca mevcut bir ASP.NET Core uygulamasına oturum açma eklemek istiyorsanız.

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
> |ClientID     |Azure portalında uygulama kimliği uygulamadan kayıtlı|
> |Yetkili | Kimlik doğrulaması STS uç noktası fo kullanıcı. Genellikle https://login.microsoftonline.com/{tenant}/v2.0 genel bulut, {tenant} Kiracı kimliği, kiracınızın adı olduğu için veya *ortak* başvuru için ortak uç nokta (çok kiracılı uygulamalar için kullanılır)|
> |Jwtformat |Kimlik doğrulama tanımlama, kimlik doğrulama belirteci eşleşmelidir gösterir|
> |RequireHttpsMetadata     |HTTPS'yi zorunlu tutmak için meta veri adresini veya yetkilisi. Bu değeri değiştirmek için önerilir `True`|
> |Tokenvalidationparameters değerini     | Belirteç doğrulama parametreleri listesi. Bu durumda, `ValidateIssuer` ayarlanır `false` herhangi bir kişisel veya iş veya Okul hesabı oturum açma işlemleri kabul ettiğinizi belirtmek için|

### <a name="initiate-an-authentication-challenge"></a>Bir kimlik doğrulaması sınaması başlatma

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

### <a name="protect-a-controller-or-a-controllers-method"></a>Bir denetleyici veya bir denetleyicinin yöntemi koruma

Yapı denetleyicisini veya yapı denetleyicisi kullanarak methodss Koruyabileceğiniz `[Authorize]` özniteliği. Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılar - kimlik doğrulama sınaması, herhangi bir kullanıcı kimliği doğrulanmamış denetleyici erişmeye başlatılabildiğinden anlamına gelir vererek denetleyici veya yöntemleri için erişimi kısıtlar.

## <a name="next-steps"></a>Sonraki Adımlar

Bu ASP.NET Core Hızlı Başlangıç için yeni bir ASP.NET Core Web uygulaması için kimlik doğrulaması ekleme hakkında yönergeler dahil olmak üzere daha fazla bilgi - GitHub deposunu denetleyin:

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması kod örneği](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]