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
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/09/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4849ffcc6fd71a0b88b270f2e6cbdb23b18ecc76
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51611650"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Hızlı Başlangıç: Microsoft ile ASP.NET web uygulamasına oturum açma ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta, ASP.NET Core web uygulaması kişisel hesapların nasıl kaydolabilirsiniz öğreneceksiniz (hotmail.com, outlook.com, diğerleri) ve iş ve Okul hesapları herhangi bir Azure Active Directory (Azure AD) örneğinden.

![Bu hızlı başlangıcın oluşturduğu örnek uygulama nasıl çalışır](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.png)


> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Hızlı] [1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırma ve ardından kod örneğinizi indirme](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Git [Azure Portalı - Uygulama kayıtları (Önizleme)](https://aka.ms/aspnetcore2-1-aad-quickstart-v2).
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydedin ve uygulamanın kayıt bilgilerini çözümünüze el ile eklemek için bu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve ardından **Uygulama kayıtları (Önizleme)** > **Yeni kayıt** seçeneğini belirleyin.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>    - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `AspNetCore-Quickstart`.
>    - İçinde **yanıt URL'si**, ekleme `https://localhost:44321/`seçip **kaydetme**.
> 1. Seçin **kimlik doğrulaması** menüsünü ve ardından aşağıdaki bilgileri ekleyin:
>    - İçinde **yanıt URL'si**, ekleme `https://localhost:44321/signin-oidc`seçip **kaydetme**.
>    - İçinde **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL'si** için `https://localhost:44321/signout-oidc`.
>    - Altında **örtük vermeyi**, kontrol **kimlik belirteçlerini**.
>    - **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. adım: uygulamanızı Azure portalında yapılandırma
> Yanıt URL'si olarak eklemek gereken çalışmak bu hızlı başlangıç için kod örneği için `https://localhost:44321/` ve `https://localhost:44321/signin-oidc`, oturum kapatma URL'si olarak ekleme `https://localhost:44321/signout-oidc`ve istek kimliği belirteçleri yetkilendirme uç noktası tarafından verilmesi.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-aspnet-core-project"></a>2. adım: ASP.NET Core projenizi indirin

- [Visual Studio 2017 çözümünü indirme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>3. Adım: Visual Studio projenizi yapılandırma

1. Örneğin, bir yerel klasör Kök klasörde - zip dosyasını ayıklayın **C:\Azure-Samples**
1. Visual Studio 2017 kullanırsanız, (isteğe bağlı) Visual Studio içinde çözümü açın.
1. Düzen **appsettings.json** dosya. Bulma `ClientId` değiştirin `Enter_the_Application_Id_here` ile **uygulama (istemci) kimliği** yeni kaydettiğiniz uygulamayı değeri. 

    ```json
    "ClientId": "Enter_the_Application_Id_here"
    "TenantId": "Enter_the_Tenant_Info_Here"
    ```

> [!div renderon="docs"]
> Konumlar:
> - `Enter_the_Application_Id_here` -olan **uygulama (istemci) kimliği** Azure Portalı'nda kayıtlı uygulama için. Bulabilirsiniz **uygulama (istemci) kimliği** uygulamasının **genel bakış** sayfası.
> - `Enter_the_Tenant_Info_Here` -şunlardan biridir:
>   - Uygulamanız destekliyorsa **hesapları yalnızca kuruluş bu dizinde**, bu değeri ile değiştirin **Kiracı kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com)
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Tüm Microsoft hesabı kullanıcıları** yaklaşımını destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

## <a name="more-information"></a>Daha fazla bilgi

Bu bölümde, kullanıcıların oturumunu açmak için gereken koda genel bir bakış sağlanır. Bu kod, main bağımsız değişkenleri, çalışma şeklini anlamanız yararlı olabilir ve ayrıca mevcut bir ASP.NET Core uygulamasına oturum açma eklemek istiyorsanız.

### <a name="startup-class"></a>Başlangıç sınıfı

*Microsoft.AspNetCore.Authentication* ara yazılım barındırma işlemi başlatıldığında çalıştırılan bir başlangıç sınıfı kullanır:

```csharp
public void ConfigureServices(IServiceCollection services)
{
  services.Configure<CookiePolicyOptions>(options =>
  {
    // This lambda determines whether user consent for non-essential cookies is needed for a given request.
    options.CheckConsentNeeded = context => true;
    options.MinimumSameSitePolicy = SameSiteMode.None;
  });

  services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
          .AddAzureAD(options => Configuration.Bind("AzureAd", options));

  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
    options.Authority = options.Authority + "/v2.0/";         // Azure AD v2.0

    options.TokenValidationParameters.ValidateIssuer = false; // accept several tenants (here simplified)
  });

  services.AddMvc(options =>
  {
     var policy = new AuthorizationPolicyBuilder()
                     .RequireAuthenticatedUser()
                     .Build();
     options.Filters.Add(new AuthorizeFilter(policy));
  })
  .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
}
```

Yöntem `AddAuthentication` tarayıcı senaryolara kullanılan yanı sıra Openıd Connect için sınama kümesi tanımlama bilgisi tabanlı kimlik doğrulaması eklemek için hizmetini yapılandırır. 

İçeren satırda `.AddAzureAd` uygulamanızı Azure AD kimlik doğrulaması ekler. Ardından, Azure AD v2.0 uç noktası kullanarak oturum açın şekilde yapılandırılır.

> |Konum  |  |
> |---------|---------|
> | ClientId  | Azure Portalı'nda kayıtlı uygulamadan uygulama (istemci) kimliği. |
> | Yetkili | Kullanıcının, kimlik doğrulaması STS uç noktası. Genellikle budur https://login.microsoftonline.com/{tenant}/v2.0 {tenant} olduğu Kiracı veya Kiracı Kimliğinizi adı, genel bulut için veya *ortak* başvuru için ortak uç nokta (çok kiracılı uygulamalar için kullanılır) |
> | Tokenvalidationparameters değerini | Belirteç doğrulaması için parametre listesi. Bu durumda, `ValidateIssuer` ayarlanır `false` herhangi bir kişisel veya iş veya Okul hesabı oturum açma işlemleri kabul ettiğinizi belirtmek için. |

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Bir denetleyici veya denetleyici yöntemleri kullanarak koruyabilirsiniz `[Authorize]` özniteliği. Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılar, kimlik doğrulama sınaması, herhangi bir kullanıcı kimliği doğrulanmamış denetleyici erişmeye başlatılabildiğinden yani vererek denetleyici veya yöntemleri için erişimi kısıtlar.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu yepyeni bir ASP.NET Core Web uygulaması için kimlik doğrulaması ekleme hakkında yönergeler dahil olmak üzere daha fazla bilgi için ASP.NET Core hızlı için GitHub deposunu inceleyin:

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması kod örneği](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)

