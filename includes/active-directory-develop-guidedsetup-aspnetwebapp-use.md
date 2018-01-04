
## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Oturum açma ve oturum kapatma isteklerini işlemek için bir denetleyici ekleyin

Bu adım, oturum açma ve oturum kapatma yöntemlerini kullanıma sunmak için yeni bir denetleyici oluşturulacağını gösterir.

1.  Sağ tıklayın `Controllers` klasörü ve seçin`Add` > `Controller`
2.  `MVC (.NET version) Controller – Empty` öğesini seçin.
3.  Tıklatın *Ekle*
4.  Bu ad `HomeController` tıklatıp *Ekle*
5.  Ekleme *OWIN* sınıfına başvuruyor:

```csharp
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
```
<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Oturum kapatma ve oturum açma işlemek için aşağıdaki iki yöntem, bir kimlik doğrulaması sınaması kodu aracılığıyla başlatarak denetleyicinize ekleyin:
</li>
</ol>

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

## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Kullanıcılar bir oturum açma düğmesi aracılığıyla imzalamak için uygulamanın giriş sayfası oluşturun

Visual Studio'da Oturum Aç düğmesini ekleyin ve kimlik doğrulamasından sonra kullanıcı bilgilerini görüntülemek için yeni bir görünüm oluşturun:

1.  Sağ tıklayın `Views\Home` klasörü ve seçin`Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Oturum açma düğmesi içerir, aşağıdaki HTML dosyaya ekleyin:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Guide</title>
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
### <a name="more-information"></a>Daha Fazla Bilgi
> Bu sayfa bir oturum açma düğmesi siyah bir arka plan ile SVG biçiminde ekler:<br/>![Microsoft ile oturum aç](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Daha fazla düğme'nın oturum açma Lütfen gidin [bu sayfayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "yönergeleri marka").
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Kullanıcının talepleri görüntülemek için bir denetleyici ekleyin
Bu denetleyici kullanımını gösteren `[Authorize]` bir denetleyici korumak için öznitelik. Bu öznitelik, yalnızca kimliği doğrulanan kullanıcılar vererek, denetleyiciye erişimi sınırlandırır. Aşağıdaki kodu yapar alındı kullanıcı talepleri, oturum açma parçası olarak görüntülemek için özniteliğini kullanın.

1.  Sağ tıklayın `Controllers` klasörü:`Add` > `Controller`
2.  `MVC {version} Controller – Empty` öğesini seçin.
3.  Tıklatın *Ekle*
4.  Adlandırın`ClaimsController`
5.  Denetleyici sınıfının kodu aşağıdaki - bu ekler değiştirin `[Authorize]` öznitelik sınıfı:

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
        var claimsPrincipalCurrent = System.Security.Claims.ClaimsPrincipal.Current;
        //You get the user’s first and last name below:
        ViewBag.Name = claimsPrincipalCurrent.FindFirst("name").Value;

        // The 'preferred_username' claim can be used for showing the username
        ViewBag.Username = claimsPrincipalCurrent.FindFirst("preferred_username").Value;

        // The subject claim can be used to uniquely identify the user across the web
        ViewBag.Subject = claimsPrincipalCurrent.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier).Value;

        // TenantId is the unique Tenant Id - which represents an organization in Azure AD
        ViewBag.TenantId = claimsPrincipalCurrent.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;

        return View();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi
> Kullanımı nedeniyle `[Authorize]` özniteliği, bu denetleyicinin tüm yöntemleri, yalnızca kullanıcının kimliği doğrulanırsa çalıştırılabilir. Kullanıcı kimliği doğrulanmamış ve denetleyici erişmeye çalışırsa, OWIN kimlik doğrulaması sınaması başlatabilir ve kullanıcının kimlik doğrulamasını zorla. Yukarıdaki kod talep koleksiyonu görünür `ClaimsPrincipal.Current` kullanıcının belirteçte dahil belirli kullanıcı öznitelikleri için örneği. Bu öznitelikler, kullanıcının tam adını ve kullanıcı adı yanı sıra, genel kullanıcı tanımlayıcısı konu içerir. Ayrıca içerdiği *Kiracı kimliği*, kullanıcının kuruluş Kimliğini temsil eder. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Kullanıcının talepleri görüntülemek için bir görünüm oluşturma

Visual Studio'da bir web sayfasında kullanıcının talepleri görüntülemek için yeni bir görünüm oluşturun:

1.  Sağ tıklayın `Views\Claims` klasörü ve:`Add View`
2.  Bunu, `Index` olarak adlandırın.
3.  Aşağıdaki HTML dosyaya ekleyin:

```html
<html>
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Sign-In with Microsoft Sample</title>
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
