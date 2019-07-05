---
title: Microsoft kimlik platformu ASP.NET web sunucusu hızlı başlangıç | Azure
description: OpenID Connect kullanarak ASP.NET web uygulamasında Microsoft Oturum Açma işlemini yapmayı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d813cea28d968ae7f14d55d47855b361141d6089
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565419"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Hızlı Başlangıç: Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta, ASP.NET web uygulamasının herhangi bir Azure Active Directory (Azure AD) örneğinden kişisel hesaplarda (hotmail.com, outlook.com ve diğerleri), iş ve okul hesaplarında nasıl oturum açabildiğini öğreneceksiniz.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-aspnet-webapp/aspnetwebapp-intro.svg)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1\. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
>
> 1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs) bölmesi.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'e tıklayın.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2\. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. Seçin **yeni kayıt**.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>      - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `ASPNET-Quickstart`.
>      - Ekleme `https://localhost:44368/` içinde **yeniden yönlendirme URI'si**, tıklatıp **kaydetme**.
**Kimlik doğrulaması** menüsünü seçin, **Örtük izin verme** bölümünde **kimlik belirteçlerini** ayarlayın ve **Kaydet**'i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1\. adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlangıçtaki kod örneğinin çalışması için `https://localhost:44368/` gibi bir yanıt URL’si eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özellikle yapılandırıldı

#### <a name="step-2-download-your-project"></a>2\. adım: Projenizi indirin

[Visual Studio 2019 çözümü indirin](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>3\. adım: Visual Studio projenizi yapılandırın

1. Zip dosyasını kök klasöre yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) açın
1. Çözümü Visual Studio’da açın (AppModelv2-WebApp-OpenIDConnect-DotNet.sln)
1. Visual Studio sürümüne bağlı olarak, projeye sağ tıklamanız gerekebilir `AppModelv2-WebApp-OpenIDConnect-DotNet` ve **geri NuGet paketleri**
1. Paket Yöneticisi konsolunu açın (Görüntüle -> diğer Windows Paket Yöneticisi konsolu ->) ve çalıştırma `Update-Package Microsoft.CodeDom.Providers.DotNetCompilerPlatform -r`
1. **Web.config** dosyasını düzenleyip `ClientId` ve `Tenant` parametrelerini şu şekilde değiştirin:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="Tenant" value="Enter_the_Tenant_Info_Here" />
    ```
> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Bu hızlı başlangıçta Enter_the_Supported_Account_Info_Here destekler. 

> [!div renderon="docs"]
> Konumlar:
> - `Enter_the_Application_Id_here` - Kaydettiğiniz uygulamanın Uygulama Kimliği değeridir.
> - `Enter_the_Tenant_Info_Here` - Aşağıdaki seçeneklerden biridir:
>   - Uygulamanız **Yalnızca kuruluşum** yaklaşımını destekliyorsa bu değeri **Kiracı Kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com) ile değiştirin
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Tüm Microsoft hesabı kullanıcıları** yaklaşımını destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > *Uygulama Kimliği*, *Dizin (kiracı) Kimliği* ve *Desteklenen hesap türleri* değerlerini bulmak için **Genel bakış** sayfasına gidin

## <a name="more-information"></a>Daha fazla bilgi

Bu bölümde, kullanıcıların oturumunu açmak için gereken koda genel bir bakış sağlanır. Bu genel bakışta kodu, main bağımsız değişkenleri, çalışma şeklini anlamanız için yararlı olabilir ve ayrıca mevcut bir ASP.NET uygulamasına oturum açma eklemek istiyorsanız.

### <a name="owin-middleware-nuget-packages"></a>OWIN ara yazılımı NuGet paketleri

OWIN Ara Yazılımı paketleriyle ASP.NET’te OpenID Connect’i kullanarak, tanımlama bilgisi tabanlı kimlik doğrulamasıyla kimlik doğrulaması işlem hattını ayarlayabilirsiniz. Visual Studio’nun **Paket Yöneticisi Konsolu**’nda şu komutları çalıştırarak bu paketleri yükleyebilirsiniz:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb  
```

### <a name="owin-startup-class"></a>OWIN Startup Sınıfı

OWIN ara yazılımı kullanan bir *başlangıç sınıfı* barındırma işlemi başlatıldığında çalışır. Bu hızlı başlangıçta, *startup.cs* Kök klasörde bulunan dosya. Aşağıdaki kod bu hızlı başlangıcın kullandığı parametreyi gösterir:

```csharp
public void Configuration(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());
    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            // Sets the ClientId, authority, RedirectUri as obtained from web.config
            ClientId = clientId,
            Authority = authority,
            RedirectUri = redirectUri,
            // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
            PostLogoutRedirectUri = redirectUri,
            Scope = OpenIdConnectScope.OpenIdProfile,
            // ResponseType is set to request the id_token - which contains basic information about the signed-in user
            ResponseType = OpenIdConnectResponseType.IdToken,
            // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
            // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
            // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter
            TokenValidationParameters = new TokenValidationParameters()
            {
                ValidateIssuer = false // Simplification (see note below)
            },
            // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = OnAuthenticationFailed
            }
        }
    );
}
```

> |Konum  |  |
> |---------|---------|
> | `ClientId`     | Azure portalına kaydedilen uygulamaya ait Uygulama Kimliği |
> | `Authority`    | Kimlik doğrulaması yapılacak kullanıcı için STS uç noktası. Çoğunlukla, genel bulut için <https://login.microsoftonline.com/{tenant}/v2.0>; burada {tenant}, kiracınızın adı, kiracınızın kimliği veya ortak uç noktaya başvuru olarak *common* değeridir (çok kiracılı uygulamalarda kullanılır) |
> | `RedirectUri`  | Microsoft kimlik platformu uç nokta kimlik doğrulaması sonra kullanıcıların nereye gönderileceğini URL'si |
> | `PostLogoutRedirectUri`     | Oturum kapatıldıktan sonra kullanıcıların gönderildiği URL |
> | `Scope`     | İstenen kapsamların listesi, boşlukla ayrılmış |
> | `ResponseType`     | Kimlik doğrulamadan gelen yanıtın kimlik belirteci içerdiği istek |
> | `TokenValidationParameters`     | Belirteç doğrulaması için parametre listesi. Bu durumda, tüm kişisel, iş veya okul hesabı türleriyle yapılan okurum açma işlemlerini kabul edebileceğini belirtmek için `ValidateIssuer` `false` olarak ayarlanır |
> | `Notifications`     | Farklı *OpenIdConnect* iletilerinde yürütülebilen temsilcilerin listesi |


> [!NOTE]
> Ayar `ValidateIssuer = false` olduğu için bu hızlı başlangıçta bir basitleştirme. Gerçek uygulamalarda veren doğrulamanız gerekir.
> Bunun nasıl yapılacağını anlamak için örneklere bakın.

### <a name="initiate-an-authentication-challenge"></a>Kimlik doğrulaması sınamasını başlatma

Denetleyicinizde kimlik doğrulaması sınaması isteyerek kullanıcıyı oturum açmaya zorlayabilirsiniz:

```csharp
public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties{ RedirectUri = "/" },
            OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
```

> [!TIP]
> Yukarıdaki yöntem kullanılarak kimlik doğrulaması sınamasının istenmesi isteğe bağlıdır ve normalde hem kimlik doğrulamalı hem de kimlik doğrulamasız kullanıcıların erişebildiği bir görünüm istediğinizde kullanılır. Alternatif olarak, bir sonraki bölümde açıklanan yöntemle denetleyicileri koruyabilirsiniz.

### <a name="protect-a-controller-or-a-controllers-method"></a>Denetleyiciyi veya denetleyici yöntemini koruma

Denetleyiciyi veya denetleyici eylemlerini `[Authorize]` özniteliğini kullanarak koruyabilirsiniz. Bu öznitelik, yalnızca kimliği doğrulanmış kullanıcıların denetleyicideki eylemlere erişmesine izin vererek denetleyiciye veya eylemlere erişimi kısıtlar. Başka bir deyişle kimlik doğrulama sınaması, *kimliği doğrulanmamış* bir kullanıcı `[Authorize]` özniteliğini içeren eylemlerden birine veya denetleyiciye erişmeyi denediğinde otomatik olarak gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın tam bir açıklamasının da içinde olduğu yeni özellikleri ve uygulamaları oluşturma hakkında eksiksiz adım adım kılavuz için ASP.NET öğreticisini deneyin.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Bu hızlı başlangıçta kullanılan uygulamayı oluşturma adımlarını öğrenin

> [!div class="nextstepaction"]
> [Oturum açma öğreticisi](./tutorial-v2-asp-webapp.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
