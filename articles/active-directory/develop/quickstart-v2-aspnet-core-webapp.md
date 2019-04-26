---
title: Microsoft kimlik platformu ASP.NET Core web uygulaması hızlı başlangıç | Azure
description: Microsoft oturum açma ASP.NET Core Web Openıd Connect'i kullanarak uygulama üzerinde uygulama hakkında bilgi edinin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1150e68167ad4e932acce744cdd5eba88e49a8c4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60302367"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-core-web-app"></a>Hızlı Başlangıç: Oturum açma Microsoft ile bir ASP.NET Core web uygulamasına ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta, ASP.NET Core web uygulaması kişisel hesapların nasıl kaydolabilirsiniz öğreneceksiniz (hotmail.com, outlook.com, diğerleri) ve iş ve Okul hesapları herhangi bir Azure Active Directory (Azure AD) örneğinden.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-aspnet-core-webapp/aspnetcorewebapp-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
>
> 1. Git [Azure Portalı - Uygulama kayıtları](https://aka.ms/aspnetcore2-1-aad-quickstart-v2).
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydedin ve uygulamanın kayıt bilgilerini çözümünüze el ile eklemek için bu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. Seçin **yeni kayıt**.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>    - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `AspNetCore-Quickstart`.
>    - İçinde **yeniden yönlendirme URI'si**, ekleme `https://localhost:44321/`seçip **kaydetme**.
> 1. Seçin **kimlik doğrulaması** menüsünü ve ardından aşağıdaki bilgileri ekleyin:
>    - İçinde **yeniden yönlendirme URI'leri**, ekleme `https://localhost:44321/signin-oidc`seçip **Kaydet**.
>    - İçinde **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL'si** için `https://localhost:44321/signout-oidc`.
>    - Altında **örtük vermeyi**, kontrol **kimlik belirteçlerini**.
>    - **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Yanıt URL'si olarak eklemek gereken çalışmak bu hızlı başlangıç için kod örneği için `https://localhost:44321/` ve `https://localhost:44321/signin-oidc`, oturum kapatma URL'si olarak ekleme `https://localhost:44321/signout-oidc`ve istek kimliği belirteçleri yetkilendirme uç noktası tarafından verilmesi.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-aspnet-core-project"></a>2. Adım: ASP.NET Core projenizi indirin

- [Visual Studio 2017 çözümünü indirme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/archive/aspnetcore2-2.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>3. Adım: Visual Studio projenizi yapılandırın

1. Örneğin, bir yerel klasör Kök klasörde - zip dosyasını ayıklayın **C:\Azure-Samples**
1. Visual Studio 2017 kullanırsanız, (isteğe bağlı) Visual Studio içinde çözümü açın.
1. Düzen **appsettings.json** dosya. Bulma `ClientId` ve değerini güncelleştirin `ClientId` ile **uygulama (istemci) kimliği** yeni kaydettiğiniz uygulamayı değeri. 

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
    options.Authority = options.Authority + "/v2.0/";         // Microsoft identity platform

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

İçeren satırda `.AddAzureAd` uygulamanıza Microsoft kimlik platformu doğrulama ekler. Ardından, Microsoft kimlik platformu uç noktayı kullanarak oturum açın şekilde yapılandırılır.

> |Konum  |  |
> |---------|---------|
> | ClientId  | Azure Portalı'nda kayıtlı uygulamadan uygulama (istemci) kimliği. |
> | Yetkili | Kullanıcının, kimlik doğrulaması STS uç noktası. Genellikle budur <https://login.microsoftonline.com/{tenant}/v2.0> {tenant} olduğu Kiracı veya Kiracı Kimliğinizi adı, genel bulut için veya *ortak* başvuru için ortak uç nokta (çok kiracılı uygulamalar için kullanılır) |
> | Tokenvalidationparameters değerini | Belirteç doğrulaması için parametre listesi. Bu durumda, `ValidateIssuer` ayarlanır `false` herhangi bir kişisel veya iş veya Okul hesabı oturum açma işlemleri kabul ettiğinizi belirtmek için. |


> [!NOTE]
> Ayar `ValidateIssuer = false` olduğu için bu hızlı başlangıçta bir basitleştirme. Gerçek uygulamalarda veren doğrulamanız gerekir.
> Bunun nasıl yapılacağını anlamak için örneklere bakın.

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Bir denetleyici veya denetleyici yöntemleri kullanarak koruyabilirsiniz `[Authorize]` özniteliği. Bu öznitelik yalnızca kimliği doğrulanmış kullanıcılar, kimlik doğrulama sınaması, herhangi bir kullanıcı kimliği doğrulanmamış denetleyici erişmeye başlatılabildiğinden yani vererek denetleyici veya yöntemleri için erişimi kısıtlar.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu yepyeni bir ASP.NET Core Web uygulaması için kimlik doğrulaması ekleme hakkında yönergeler dahil olmak üzere daha fazla bilgi için ASP.NET Core öğretici için GitHub deposunu denetleyin Microsoft Graph ve diğer Microsoft APIs çağırmak nasıl kendi API'leri çağırmak nasıl ekleme Yetkilendirme, oturum açma nasıl Ulusal bulutlarda ya da sosyal kimlikleri ve daha fazlasıyla kullanıcılar:

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması Öğreticisi](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/)
