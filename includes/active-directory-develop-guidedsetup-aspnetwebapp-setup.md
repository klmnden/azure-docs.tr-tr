---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: dcfc341b89a3cfebcb5538f88481fd2fbb2936a7
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188978"
---
## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu bölüm, yükleme ve Openıd Connect'i kullanarak bir ASP.NET projesi üzerinde OWIN ara yazılımı üzerinden kimlik doğrulaması işlem hattı yapılandırma adımlarını gösterir.

> Bunun yerine bu örneği ait Visual Studio projeyi indirmek tercih ediyorsunuz? [Bir projeyi indirirken](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) ve atlamak [yapılandırma adımı](#register-your-application) çalıştırmadan önce kodu örneği yapılandırmak için.

### <a name="create-your-aspnet-project"></a>ASP.NET projenizi oluşturun

1. Visual Studio'da: `File` > `New` > `Project`
2. Altında *Visual C# \Web*seçin `ASP.NET Web Application (.NET Framework)`.
3. Uygulamanızı adlandırın ve tıklayın *Tamam*
4. Seçin `Empty` eklemek için onay kutusunu seçip `MVC` başvuruları

## <a name="add-authentication-components"></a>Kimlik doğrulama bileşenleri ekleme

1. Visual Studio'da: `Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Paket Yöneticisi Konsolu penceresinde aşağıdakileri yazarak *OWIN ara yazılım NuGet paketlerini* ekleyin:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Bu kitaplıklar hakkında
> Yukarıdaki kitaplıklar, tanımlama bilgisi tabanlı kimlik doğrulaması aracılığıyla OpenID Connect kullanarak çoklu oturum açma (SSO) sağlar. Kimlik doğrulaması tamamlandıktan ve kullanıcıyı temsil eden belirteç uygulamanıza gönderildikten sonra OWIN ara yazılımı bir oturum tanımlama bilgisi oluşturur. Tarayıcı daha sonra bu tanımlama bilgisi sonraki isteklerde authenticateasync kullanıcı parolayı gerekmez ve hiçbir ek doğrulama gerektiği şekilde kullanır.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Kimlik doğrulaması işlem hattı yapılandırın

Aşağıdaki adımlar, bir OWIN ara yazılımını Openıd Connect kimlik doğrulamasını yapılandırmak için başlangıç sınıfı oluşturmak için kullanılır. Bu sınıf, IIS işlemi başladığında, otomatik olarak yürütülür.

> [!TIP]
> Projenizin kök klasöründe `Startup.cs` adlı bir dosya yoksa:
> 1. Projenin kök klasörüne sağ: > `Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Bunu, `Startup.cs` olarak adlandırın.
>
>> Seçilen sınıfın standart bir C# sınıfı değil OWIN Başlangıç Sınıfı olduğundan emin olun. Doğrulamak için ad alanının üzerinde `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` yazıp yazmadığını kontrol edin.

1. Ekleme *OWIN* ve *Microsoft.IdentityModel* başvurular `Startup.cs`:

    ```csharp
    using Microsoft.Owin;
    using Owin;
    using Microsoft.IdentityModel.Protocols.OpenIdConnect;
    using Microsoft.IdentityModel.Tokens;
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    using Microsoft.Owin.Security.Notifications;
    ```

2. Başlangıç sınıfı aşağıdaki kodla değiştirin:

    ```csharp
    public class Startup
    {
        // The Client ID is used by the application to uniquely identify itself to Azure AD.
        string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

        // RedirectUri is the URL where the user will be redirected to after they sign in.
        string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

        // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
        static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

        // Authority is the URL for authority, composed by Azure Active Directory v2.0 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
        string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

        /// <summary>
        /// Configure OWIN to use OpenIdConnect 
        /// </summary>
        /// <param name="app"></param>
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
                        ValidateIssuer = false // This is a simplification
                    },
                    // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed
                    }
                }
            );
        }

        /// <summary>
        /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
        /// </summary>
        /// <param name="context"></param>
        /// <returns></returns>
        private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
        {
            context.HandleResponse();
            context.Response.Redirect("/?errormessage=" + context.Exception.Message);
            return Task.FromResult(0);
        }
    }
    ```

> [!NOTE]
> Ayar `ValidateIssuer = false` olduğu için bu hızlı başlangıçta bir basitleştirme. Gerçek sağlayıcısını doğrulamak için ihtiyacınız olan uygulamaları örnekleri bunun nasıl yapılacağını anlamak için bkz.

<!--start-collapse-->
> ### <a name="more-information"></a>Daha Fazla Bilgi
> *OpenIDConnectAuthenticationOptions* içinde sağladığınız parametreler uygulamanın Azure AD ile iletişim kurmak için kullanacağı koordinatlara benzer. Openıd Connect ara yazılımı arka planda tanımlama bilgileri kullandığından, ayrıca tanımlama bilgisi kimlik doğrulamasını gösterir yukarıdaki kodu olarak ayarlamanız gerekir. *ValidateIssuer* değeri OpenIdConnect öğesine erişimi tek bir kuruluşla sınırlamamasını söyler.
<!--end-collapse-->
