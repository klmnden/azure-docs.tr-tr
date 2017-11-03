---
title: "Azure AD .NET web uygulaması Başlarken | Microsoft Docs"
description: "Oturum açma için Azure AD ile tümleşen bir .NET MVC web uygulaması oluşturun."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 3c1e558c9d41e385f80939203a3457b74e30973b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a>ASP.NET web uygulaması oturum açma ve Azure AD ile oturum kapatma
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Yalnızca birkaç kod satırıyla, tek bir oturum açma ve oturum kapatma sağlayarak, Azure Active Directory (Azure AD), web uygulaması kimlik yönetimi dış için kolaylaştırır. .NET (OWIN) ara yazılımı için açık Web arabirimi Microsoft uyarlamasını kullanarak ASP.NET web uygulamaları ve kullanıcıların imzalayabilirsiniz. OWIN ara yazılımı topluluk odaklı .NET Framework 4. 5 ' bulunur. Bu makale için OWIN kullanmayı gösterir:

* Kullanıcıların kimlik sağlayıcısı olarak Azure AD kullanarak web uygulamaları için oturum açın.
* Bazı kullanıcı bilgileri görüntüler.
* Kullanıcıların uygulamaları dışında oturum açın.

## <a name="before-you-get-started"></a>Başlamadan önce
* Karşıdan [uygulama çatıyı](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) veya karşıdan [tamamlanan örnek](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).
* Ayrıca uygulamanın kaydedileceği Azure AD kiracısı gerekir. Azure AD kiracısı, zaten yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

Hazır olduğunuzda İleri dört bölümlerdeki yordamları izleyin.

## <a name="step-1-register-the-new-app-with-azure-ad"></a>1. adım: Azure AD ile yeni uygulamayı Kaydet
Kullanıcıların kimliğini doğrulamak için önce aşağıdakileri yaparak kiracınızda kaydetmeniz uygulama ayarlamak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızın adını tıklatın. Altında **Directory** listesinde, uygulama kaydetmek istediğiniz Active Directory Kiracı seçin.
3. Tıklatın **daha Hizmetleri** sol bölmesinde ve seçip **Azure Active Directory**.
4. Tıklatın **uygulama kayıtlar**ve ardından **Ekle**.
5. Yeni bir oluşturmak için istemleri izleyin **Web uygulaması ve/veya Webapı**.
  * **Ad** kullanıcılara uygulamasının açıklar.
  * **Oturum açma URL'si** uygulamasının temel URL'si. Çatıyı ait varsayılan URL'dir https://localhost:44320 /.
6. Kayıt tamamladıktan sonra Azure AD uygulama benzersiz uygulama kimliği atar. Sonraki bölümlerde kullanmak için uygulama sayfasında değerini kopyalayın.
7. Gelen **ayarları** -> **özellikleri** sayfasında uygulamanız için uygulama kimliği URI'si güncelleştirin. **Uygulama kimliği URI'si** uygulama için benzersiz bir tanımlayıcıdır. Adlandırma kuralı `https://<tenant-domain>/<app-name>` (örneğin, `https://contoso.onmicrosoft.com/my-first-aad-app`).

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a>2. adım: uygulama OWIN kimlik doğrulaması ardışık kullanmak için ayarlama.
Bu adımda, Openıd Connect kimlik doğrulama protokolünü kullanmak için OWIN ara yazılımını yapılandırın. Oturum açma ve oturum kapatma isteklerini yürütmek, kullanıcı oturumlarını yönetmek, kullanıcı bilgilerini almak ve benzeri için OWIN kullanın.

1. Başlamak için Paket Yöneticisi konsolu kullanılarak OWIN ara yazılımı NuGet paketlerini projeye ekleyin.

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. Bir OWIN başlangıç sınıfı adlı projeye eklemek için `Startup.cs`, projeye sağ tıklayın, seçin **Ekle**seçin **yeni öğe**, arayın ve sonra **OWIN**. OWIN ara yazılımı çağırır **Configuration(...)**  uygulama başlatıldığında yöntemi.
3. Sınıf bildirimi değiştirme `public partial class Startup`. Zaten bu sınıfın parçası sizin için başka bir dosyaya uyguladık. İçinde **Configuration(...)**  yöntemi çağrısına duruma **ConfgureAuth(...)**  uygulaması için kimlik doğrulamasını ayarlamak için.  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. App_Start\Startup.Auth.cs dosyasını açın ve ardından uygulama **ConfigureAuth(...)**  yöntemi. Sağladığınız içinde parametreleri *OpenIDConnectAuthenticationOptions* Azure AD ile iletişim kurmak için uygulama koordinatları olarak hizmet. Openıd Connect ara yazılımı arka planda tanımlama bilgileri kullandığından ayrıca tanımlama bilgisi kimlik doğrulamasını kurmanız gerekir.

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
                 Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = context =>
                        {
                            context.HandleResponse();
                            context.Response.Redirect("/Error?message=" + context.Exception.Message);
                            return Task.FromResult(0);
                        }
                    }
             });
     }
     ```

5. Proje kök dizininde web.config dosyasını açın ve ardından yapılandırma değerlerini girin `<appSettings>` bölümü.
  * `ida:ClientId`: Azure portalında kendisinden kopyaladığınız GUID "1. adım: Azure AD ile yeni uygulamayı kaydedin."
  * `ida:Tenant`: Azure AD kiracınız (örneğin, contoso.onmicrosoft.com) adı.
  * `ida:PostLogoutRedirectUri`: Bir kullanıcı oturum kapatma isteği başarıyla tamamlandıktan sonra burada yönlendirilmesi gereken Azure AD bildirir göstergesi.

## <a name="step-3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>3. adım: Kullanım için Azure AD oturum açma ve oturum kapatma isteklerini yürütmek için OWIN
Uygulama artık Openıd Connect kimlik doğrulama protokolü kullanarak Azure AD ile iletişim kurmak için düzgün şekilde yapılandırılmıştır. OWIN tüm kimlik doğrulama iletileri hazırlayın, Azure AD'den belirteçleri doğrulamak ve kullanıcı oturumlarını Bakım Ayrıntıları ele. Kalan tek şey, kullanıcılarınızın bir şekilde oturum açabilir ve oturumu vermek için.

1. Kullanabileceğiniz belirli sayfalarını erişmeden önce oturum açmalarını gerektirecek şekilde denetleyicilerinizi etiketlerinde yetkilendirin. Bunu yapmak için Controllers\HomeController.cs açın ve ardından ekleyin `[Authorize]` hakkında eylem etiketi.

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. OWIN, doğrudan kimlik doğrulama isteklerini kodunuzu içinde vermek için de kullanabilirsiniz. Bunu yapmak için Controllers\AccountController.cs açın. Ardından, SignIn() ve SignOut() Eylemler, Openıd Connect sınama ve oturum kapatma isteklerini gönderin.

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. Görünümler/paylaşılan açmak\_LoginPartial.cshtml kullanıcı uygulama oturum açma ve oturum kapatma bağlantıları göstermek için ve kullanıcının adını görünümünde yazdırmak için.

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
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

## <a name="step-4-display-user-information"></a>4. adım: kullanıcı bilgilerini görüntüleme
Openıd Connect ile kullanıcıların kimliğini doğrular, Azure AD bir id_token "talep" ya da kullanıcı hakkında onaylar içeren bir uygulamaya döndürür. Aşağıdakileri yaparak app kişiselleştirmek için bu talepler kullanabilirsiniz:

1. Controllers\HomeController.cs dosyasını açın. Denetleyicilerinizi kullanıcının talepleri erişebilmeniz için `ClaimsPrincipal.Current` güvenlik sorumlusu nesnesi.

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. Derleme ve uygulamayı çalıştırın. Onmicrosoft.com etki alanı ile kiracınızda zaten yeni bir kullanıcı oluşturmadıysanız, bunu yapmak için gereken süre sunulmuştur. Bunu yapmak için:

  a. Oturum, kullanıcı oturum açabilir ve kullanıcının kimliğini üst çubuğunda nasıl yansıtılır not edin.

  b. Oturumu kapatın ve kiracınızda başka bir kullanıcı olarak yeniden oturum açın.

  c. Özellikle hırslı hissediyorsanız kaydetmek ve bu uygulamayla (kendi ClientID) başka bir örneğini çalıştıran ve çoklu oturum açma eylem izleyin.

## <a name="next-steps"></a>Sonraki adımlar
Başvuru için bkz: [tamamlanan örnek](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (yapılandırma değerleriniz olmadan).

Artık daha ileri seviyeli konu başlıklarına geçebilirsiniz. Örneğin, [Azure AD ile Web API güvenliğini](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
