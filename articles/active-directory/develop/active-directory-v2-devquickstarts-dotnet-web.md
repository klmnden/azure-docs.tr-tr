---
title: "Azure AD v2.0 .NET web uygulaması oturum Başlarken | Microsoft Docs"
description: "Kullanıcıların hem kişisel Microsoft Account oturum ve iş veya Okul hesapları imzalar .NET MVC Web uygulaması oluşturma."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c8b97ac6-0a06-4367-81b6-7d1d98152b14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ba5bdf7daba6086b70aec54ebe25d4445fa708c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Oturum açma bir .NET MVC web uygulaması'na ekleyin.
V2.0 uç noktası ile kolayca web uygulamalarınızı hem kişisel Microsoft hesapları için destek ile kimlik doğrulaması ve iş veya Okul hesapları ekleyebilirsiniz.  ASP.NET web uygulamalarında bunu .NET Framework 4. 5 ' dahil Microsoft'un OWIN ara yazılımı kullanarak gerçekleştirebilirsiniz.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>
>

 Burada size kullanıcı oturum açma, kullanıcı hakkındaki bazı bilgileri görüntülemek ve uygulama dışı kullanıcı oturum OWIN kullanan bir web uygulaması oluşturma.

## <a name="download"></a>İndir
Bu öğretici için kod [GitHub'da](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet) korunur.  İzlemek için [uygulamanın çatısını bir .zip karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) veya çatıyı kopyalayın:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

De Bu öğretici sonunda tamamlanmış uygulama sağlanır.

## <a name="register-an-app"></a>Bir uygulamayı kaydetme
En yeni bir uygulama oluşturma [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya adımları izleyin: [ayrıntılı adımları](active-directory-v2-app-registration.md).  Emin olun:

* Aşağı kopyalama **uygulama kimliği** uygulamanıza atanan, bunu en kısa sürede ihtiyacınız vardır.
* Ekleme **Web** uygulamanız için platform.
* Doğru girin **yeniden yönlendirme URI'si**. Bu öğretici için varsayılan burada kimlik doğrulama yanıtları yönlendirileceğini - Azure AD ile yeniden yönlendirme URI'si gösterir `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Yükleme & OWIN kimlik doğrulamasını yapılandırma
Burada, Openıd Connect kimlik doğrulama protokolünü kullanmak için OWIN ara yazılımı yapılandıracağız.  OWIN oturum açma ve oturum kapatma isteklerini yürütmek, kullanıcının oturumunu yönetmek ve başka şeyler arasında kullanıcı hakkında bilgi almak için kullanılır.

1. Başlamak için açın `web.config` dosya proje kök dizininde ve uygulamanızın yapılandırma değerlerini girin `<appSettings>` bölümü.

  * `ida:ClientId` Olan **uygulama kimliği** kayıt portalında uygulamanıza atanan.
  * `ida:RedirectUri` Olan **yeniden yönlendirme URI'si** portalda girdiğiniz.

2. Ardından, OWIN ara yazılımı NuGet paketleri Paket Yöneticisi konsolu kullanılarak projeye ekleyin.

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. "OWIN başlangıç adlı sınıfı" projeye eklemek `Startup.cs` sağ tıklatın projeye--> **Ekle** --> **yeni öğe** "OWIN" arayın.  OWIN ara yazılımı, uygulamanız başlatıldığında `Configuration(...)` yöntemini çağırır.
4. Sınıf bildirimi değiştirme `public partial class Startup` -zaten bu sınıfın parçası sizin için başka bir dosyaya uyguladık.  İçinde `Configuration(...)` yöntemi, bir web uygulamanız için kimlik doğrulaması ayarlamak için ConfigureAuth(...) çağrı yapma  

        ```C#
        [assembly: OwinStartup(typeof(Startup))]
        
        namespace TodoList_WebApp
        {
            public partial class Startup
            {
                public void Configuration(IAppBuilder app)
                {
                    ConfigureAuth(app);
                }
            }
        }
        ```

5. Dosyayı açmak `App_Start\Startup.Auth.cs` ve uygulamanıza `ConfigureAuth(...)` yöntemi.  Sağladığınız içinde parametreleri `OpenIdConnectAuthenticationOptions` uygulamanız için Azure AD ile iletişim kurmak için koordinatları olarak hizmet verecektir.  Tanımlama bilgisi kimlik doğrulamasını kurmanız gerekir - Openıd Connect Ara kapsar altında tanımlama bilgilerini kullanır.

        ```C#
        public void ConfigureAuth(IAppBuilder app)
                     {
                             app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);
        
                             app.UseCookieAuthentication(new CookieAuthenticationOptions());
        
                             app.UseOpenIdConnectAuthentication(
                                     new OpenIdConnectAuthenticationOptions
                                     {
                                             // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                                             // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                             // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.
        
                                             ClientId = clientId,
                                             Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
                                             RedirectUri = redirectUri,
                                             Scope = "openid email profile",
                                             ResponseType = "id_token",
                                             PostLogoutRedirectUri = redirectUri,
                                             TokenValidationParameters = new TokenValidationParameters
                                             {
                                                     ValidateIssuer = false,
                                             },
                                             Notifications = new OpenIdConnectAuthenticationNotifications
                                             {
                                                     AuthenticationFailed = OnAuthenticationFailed,
                                             }
                                     });
                     }
        ```

## <a name="send-authentication-requests"></a>Kimlik doğrulama isteklerini gönderme
Uygulamanız artık Openıd Connect kimlik doğrulama protokolü kullanarak v2.0 uç noktası ile iletişim kurmak için düzgün şekilde yapılandırılmıştır.  OWIN geçen tüm kimlik doğrulama iletileri hazırlayın, Azure AD'den belirteçleri doğrulamak ve kullanıcı oturumu koruyarak çirkin ayrıntılarını verdiğiniz.  Kalan tek şey, kullanıcılarınızın bir şekilde oturum açabilir ve oturumu vermek için.

- Kullanabilirsiniz, kullanıcı oturum açtığında belirli bir sayfaya erişmeden önce gerektirecek şekilde denetleyicilerinizi etiketlerinde yetkilendirmek.  Açık `Controllers\HomeController.cs`ve ekleme `[Authorize]` hakkında denetleyicisine etiketi.
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- OWIN, doğrudan kimlik doğrulama isteklerini kodunuzu içinde vermek için de kullanabilirsiniz.  Açık `Controllers\AccountController.cs`.  SignIn() ve SignOut() Eylemler, Openıd Connect sınama ve oturum kapatma isteklerini sırasıyla gönderin.

        ```C#
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- Şimdi, açmak `Views\Shared\_LoginPartial.cshtml`.  Bu, kullanıcı uygulamanızın oturum açma ve oturum kapatma bağlantıları göstermek ve kullanıcının adını görünümünde yazdırmak yerdir.

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@
        
                        Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
                    </li>
                    <li>
                        @Html.ActionLink("Sign out", "SignOut", "Account")
                    </li>
                </ul>
            </text>
        }
        else
        {
            <ul class="nav navbar-nav navbar-right">
                <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
            </ul>
        }
        ```

## <a name="display-user-information"></a>Kullanıcı bilgilerini görüntüleme
Openıd Connect ile kullanıcıların kimlik doğrulamasını yaparken, v2.0 uç noktası bir id_token talep ya da kullanıcı hakkında onaylar içeren bir uygulamaya döndürür.  Uygulamanızı kişiselleştirmek için bu talepler kullanabilirsiniz:

- `Controllers\HomeController.cs` dosyasını açın.  Denetleyicilerinizi kullanıcının talepleri erişebilmeniz için `ClaimsPrincipal.Current` güvenlik sorumlusu nesnesi.

        ```C#
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // The object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // The subject or nameidentifier claim can be used to uniquely identify the user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a>Çalıştırın
Son olarak, yapı ve uygulamanızı çalıştırın!   Kişisel bir Microsoft Account veya bir iş veya Okul hesabınızla oturum açın ve kullanıcının kimliğini üst gezinti çubuğunda nasıl yansıtılır dikkat edin.  Artık, hem kişisel hem de iş/Okul hesaplarını ile kullanıcıların kimliklerini doğrulayabilir endüstri standardı protokoller kullanılarak güvenli bir web uygulaması sahipsiniz.

(Yapılandırma değerleriniz olmadan) tamamlanan örnek, başvuru için [burada bir .zip sağlanan](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), veya Github'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Sonraki adımlar
Şimdi daha gelişmiş konu başlıklarına geçebilirsiniz.  Denemek isteyebilirsiniz:

[Bir Web API ile güvenli v2.0 uç noktası >>](active-directory-devquickstarts-webapi-dotnet.md)

Ek kaynaklar için gözden geçirin:

* [V2.0 Geliştirici Kılavuzu >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "azure-active-directory" etiketi >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma
[Bu sayfayı](https://technet.microsoft.com/security/dd252948) ziyaret ederek ve Güvenlik Önerisi Uyarılarına abone olarak güvenlik olaylarının ne zaman ortaya çıkacağı hakkında bildirimleri almanızı öneririz.
