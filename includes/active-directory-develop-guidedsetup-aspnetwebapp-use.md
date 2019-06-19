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
ms.date: 04/19/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 2b9d696ca896d0c8f0801f055000b9763d65d7ff
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188983"
---
## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Oturum açma ve oturum kapatma istekleri işlemek üzere bir denetleyici ekleyin

Bu adım, oturum açma ve oturum kapatma yöntemlerini ortaya çıkarmak için yeni bir denetleyici oluşturma işlemi gösterilmektedir.

1.  Sağ tıklayın `Controllers` klasörü ve seçin `Add` > `Controller`
2.  `MVC (.NET version) Controller – Empty` öğesini seçin.
3.  Tıklayın *Ekle*
4.  Adlandırın `HomeController` tıklatıp *Ekle*
5.  Ekleme *OWIN* sınıf başvuruları:

    ```csharp
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    ```
    
6. Oturum açma işlemek için aşağıdaki ve oturum kapatma iki yöntem, bir kod aracılığıyla kimlik doğrulaması sınaması başlatarak denetleyicinize ekleyin:
    
    ```csharp
    /// <summary>
    /// Send an OpenID Connect sign-in request.
    /// Alternatively, you can just decorate the SignIn method with the [Authorize] attribute
    /// </summary>
    public void SignIn()
    {
        if (!Request.IsAuthenticated)
        {
            HttpContext.GetOwinContext().Authentication.Challenge(
                new AuthenticationProperties{ RedirectUri = "/" },
                OpenIdConnectAuthenticationDefaults.AuthenticationType);
        }
    }
    
    /// <summary>
    /// Send an OpenID Connect sign-out request.
    /// </summary>
    public void SignOut()
    {
        HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
    }
    ```

## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Kullanıcılar bir oturum açma düğmesi aracılığıyla oturum açmak için uygulama giriş sayfası oluşturma

Visual Studio'da oturum açma düğmesini eklemek ve kimlik doğrulaması sonrasında kullanıcı bilgilerini görüntülemek için yeni bir görünüm ekleyin:

1.  Sağ tıklayın `Views\Home` klasörü ve seçin `Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Oturum açma düğmesini de içeren aşağıdaki HTML kodunu dosyaya ekleyin:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Guide</title>
    </head>
    <body>
    @if (!Request.IsAuthenticated)
    {
        <!-- If the user is not authenticated, display the sign-in button -->
        <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
            <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
            <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
            <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
            <rect x="150" y="129" width="122" height="122" fill="#F35325" />
            <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
            <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
            <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
            <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
            </svg>
        </a>
    }
    else
    {
        <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
        <br /><br />
        @Html.ActionLink("See Your Claims", "Index", "Claims")
        <br /><br />
        @Html.ActionLink("Sign out", "SignOut", "Home")
    }
    @if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
    {
        <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
    }
    </body>
    </html>
    ```

<!--start-collapse-->
> ### <a name="more-information"></a>Daha Fazla Bilgi
> Bu sayfa, siyah bir arka plan SVG biçiminde bir oturum açma düğmesi ekler:<br/>![Microsoft'ta oturum açma](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Daha fazla düğme'nın oturum açma için lütfen Git [bu sayfayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "marka yönergeleri").
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Kullanıcı talepleri görüntülemek için denetleyici ekleme
Bu denetleyici bir denetleyiciyi koruma amacıyla `[Authorize]` özniteliğini kullanma şeklini gösterir. Bu öznitelik yalnızca kimliği doğrulanan kullanıcılara izin vererek denetleyici erişimini sınırlar. Aşağıdaki kod yapar alınan kullanıcı taleplerini oturum açmanın bir parçası olarak görüntülenecek özniteliğini kullanın.

1.  Sağ tıklayın `Controllers` klasörü: `Add` > `Controller`
2.  `MVC {version} Controller – Empty` öğesini seçin.
3.  Tıklayın *Ekle*
4.  Bunu, `ClaimsController` olarak adlandırın.
5.  Bu ekler, denetleyici sınıfının kodunu aşağıdaki - kodla değiştirin `[Authorize]` öznitelik sınıfı:

    ```csharp
    [Authorize]
    public class ClaimsController : Controller
    {
        /// <summary>
        /// Add user's claims to viewbag
        /// </summary>
        /// <returns></returns>
        public ActionResult Index()
        {
            var userClaims = User.Identity as System.Security.Claims.ClaimsIdentity;
    
            //You get the user’s first and last name below:
            ViewBag.Name = userClaims?.FindFirst("name")?.Value;
    
            // The 'preferred_username' claim can be used for showing the username
            ViewBag.Username = userClaims?.FindFirst("preferred_username")?.Value;
    
            // The subject/ NameIdentifier claim can be used to uniquely identify the user across the web
            ViewBag.Subject = userClaims?.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
    
            // TenantId is the unique Tenant Id - which represents an organization in Azure AD
            ViewBag.TenantId = userClaims?.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid")?.Value;
    
            return View();
        }
    }
    ```

<!--start-collapse-->
> ### <a name="more-information"></a>Daha Fazla Bilgi
> `[Authorize]` özniteliğinin kullanılması nedeniyle bu denetleyicinin tüm metotları yalnızca kullanıcının kimliğinin doğrulanması durumunda yürütülebilir. Kullanıcı kimliği doğrulanmamış, ve denetleyiciye erişmeye OWIN kimlik doğrulaması sınaması başlatmak ve kullanıcının kimlik doğrulamasını zorla. Yukarıdaki kod, talepler kullanıcının kimliği belirtecinde bulunan belirli bir kullanıcı özniteliklerinin listesi bakar. Bu öznitelik kullanıcının tam adını, kullanıcı adını ve genel kullanıcı tanımlayıcısı nesnesini içerir. Ayrıca kullanıcının kuruluşunun kimliğini temsil eden *Kiracı Kimliği* değerini de içerir. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Kullanıcı talepleri görüntülemek için bir görünüm oluşturma

Visual Studio'da kullanıcının taleplerini bir web sayfasında görüntülemek için yeni bir görünüm oluşturun:

1.  Sağ tıklayın `Views\Claims` klasörü ve: `Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Aşağıdaki HTML kodunu dosyaya ekleyin:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Sample</title>
        <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
    </head>
    <body style="padding:50px">
        <h3>Main Claims:</h3>
        <table class="table table-striped table-bordered table-hover">
            <tr><td>Name</td><td>@ViewBag.Name</td></tr>
            <tr><td>Username</td><td>@ViewBag.Username</td></tr>
            <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
            <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
        </table>
        <br />
        <h3>All Claims:</h3>
        <table class="table table-striped table-bordered table-hover table-condensed">
        @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
        {
            <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
        }
        </table>
        <br />
        <br />
        @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
    </body>
    </html>
    ```
