---
title: Kimlik doğrulaması, kaydolma, parola Azure Active Directory B2C'de sıfırlama | Microsoft Docs
description: Oturumu-kaydolma/oturum açma sahip bir web uygulaması oluşturma, profil düzenleme ve Azure Active Directory B2C kullanarak parola sıfırlama.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/17/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 66cd3644611c1d837de717661a3515a0c0d3956f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38308040"
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a>Azure Active Directory B2C kaydolma, oturum açma, profil düzenleme ve parola sıfırlama ile ASP.NET web uygulaması oluşturma

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Web uygulamanızı Azure AD B2C kimlik özellikleri ekleyin
> * Web uygulamanızı Azure AD B2C dizininizde kaydetme
> * Bir kullanıcı oturumu açma kaydolma/oturum açma, profil düzenleme ve web uygulamanız için parola sıfırlama ilkesi oluşturma

## <a name="prerequisites"></a>Önkoşullar

- B2C Kiracınızın bir Azure hesabına bağlamanız gerekir. Ücretsiz Azure hesabı oluşturabilirsiniz [burada](https://azure.microsoft.com/).
- Gereksinim duyduğunuz [Microsoft Visual Studio](https://www.visualstudio.com/) veya benzer bir program görüntüleyebilir ve örnek kodu değiştirin.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

Azure AD B2C kullanabilmeniz için bir kiracı oluşturmanız gerekir. Bir kiracının tüm kullanıcılar, uygulamaları, gruplar ve daha fazlası için bir kapsayıcıdır. Zaten yoksa, bu kılavuzda devam etmeden önce bir B2C kiracısı oluşturun.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> Azure AD B2C kiracınızı Azure aboneliğinize bağlanmak gerekir. Seçtikten sonra **Oluştur**seçin **Azure Aboneliğimi bağlantı var olan bir Azure AD B2C Kiracısına** seçeneğini ve ardından **Azure AD B2C Kiracısı** seçin, açılan menü Kiracı ilişkilendirmek istediğiniz.

## <a name="create-and-register-an-application"></a>Oluşturma ve bir uygulamayı kaydetme

Ardından, oluşturmak ve uygulamayı Azure AD B2C kiracınıza kaydetmek gerekir. Bu, Azure AD B2C, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri sağlar. 

Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin. Şimdi daha önce oluşturduğunuz Kiracı kullanıyor olması gerekir.

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

İşiniz bittiğinde, uygulama ayarlarında, bir API hem yerel bir uygulama gerekir.

## <a name="create-policies-on-your-b2c-tenant"></a>B2C kiracınızın ilkeleri oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu kod örneği, üç kimlik deneyimi içerir: **kaydolma ve oturum açma**, **düzenleme profili**, ve **parola sıfırlama**.  Her tür için [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md) tanımlanan şekilde bir ilke oluşturmanız gerekir. Her ilke için görünen ad özniteliği veya talep seçin ve daha sonra kullanmak için ilkenizin adını aşağı kopyalamak için emin olun.

### <a name="add-your-identity-providers"></a>Kimlik sağlayıcılarınızı Ekle

Ayarlarınızı seçin **kimlik sağlayıcıları** ve kullanıcı adı kayıt ya da e-posta kaydolabilirsiniz.

### <a name="create-a-sign-up-and-sign-in-policy"></a>Kaydolma ve oturum açma ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a>Profil düzenleme ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a>Parola sıfırlama ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

İlkelerinizi oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-sample-code"></a>Örnek kodu indirin

Bu öğretici için kod [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi)'da korunur. Örneği kopyalamak için şu komutu çalıştırın:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Örnek kodu indirdikten sonra başlamak için Visual Studio .sln dosyasını açın. Çözüm dosyası iki proje içerir: `TaskWebApp` ve `TaskService`. `TaskWebApp` kullanıcı ile etkileşime giren MVC web uygulamasıdır. `TaskService`, uygulamanın, her kullanıcının yapılacaklar listesini depolayan arka uç web API'sidir. Bu makalede yalnızca `TaskWebApp` uygulaması ele alınacaktır. Nasıl oluşturulacağını öğrenmek için `TaskService` Azure AD B2C kullanarak bkz [.NET web API Öğreticisi](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="update-code-to-use-your-tenant-and-policies"></a>Kiracı ve ilkelerini kullanmak için kodu güncelleştirme

Örneğimiz, tanıtım kiracımızın ilkelerini ve istemci kimliğini kullanacak şekilde yapılandırılmıştır. Kendi kiracınıza bağlamak için açmanız gerekir `web.config` içinde `TaskWebApp` proje ve aşağıdaki değerleri değiştirin:

* kiracı adınızla `ida:Tenant`
* Web uygulamanızın kimliği ile `ida:ClientId`
* Web uygulamanızın gizli anahtarı ile `ida:ClientSecret`
* "Kaydolma veya Oturum açma" ilkenizin adıyla `ida:SignUpSignInPolicyId`
* "Profil Düzenleme" ilkenizin adıyla `ida:EditProfilePolicyId`
* "Parola Sıfırlama" ilkenizin adıyla `ida:ResetPasswordPolicyId`

## <a name="launch-the-app"></a>Uygulamayı başlatın
Öğesinden, Visual Studio içinde uygulamayı başlatın. Yapılacaklar listesi sekmesine gidin ve URL'sini Not: https://login.microsoftonline.com/ *YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id =*YourclientID*.....

E-posta adresi veya kullanıcı adı kullanarak uygulamaya kaydoluyor. Çıkışı, oturum sonra yeniden oturum açın ve profili düzenlemek veya parolayı sıfırlayın. Oturumu kapatın ve farklı bir kullanıcı olarak oturum açın. 

## <a name="add-social-idps"></a>Sosyal Idp'yi Ekle

Şu anda kullanarak uygulamayı yalnızca kullanıcı kaydolma ve oturum açma destekler **yerel hesaplar**; B2C dizininizde saklanan hesapları, kullanıcı adı ve parolayı kullanın. Azure AD B2C'yi kullanarak desteği diğer ekleyebilirsiniz **kimlik sağlayıcıları** (IDP) değiştirmeden herhangi bir kod.

Sosyal Idp'yi uygulamanıza eklemek için şu makalelere ayrıntılı yönergeleri takip ederek başlayın. Desteklemek istediğiniz her bir IDP için bu sistemde bir uygulamayı kaydetme ve bir istemci kimliği almak gerekiyor

* [Facebook bir IDP ayarlayın](active-directory-b2c-setup-fb-app.md)
* [Google bir IDP ayarlayın.](active-directory-b2c-setup-goog-app.md)
* [Amazon bir IDP ayarlayın](active-directory-b2c-setup-amzn-app.md)
* [LinkedIn bir IDP ayarlayın](active-directory-b2c-setup-li-app.md)

Kimlik sağlayıcıları B2C dizininizi ekledikten sonra açıklandığı gibi her yeni IDP içerecek şekilde ilkelerinizi üç Düzenle [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md). İlkelerinizi kaydettikten sonra uygulamayı yeniden çalıştırın.  Oturum açma eklenen yeni IDP görmeniz gerekir ve her kimliğinizi kaydolma seçenekleri deneyimleri.

Uygulamanızın ilkelerini deneyebilir ve örnek uygulamanızı üzerindeki etkisini inceleyin. Ekleme veya Idp'yi kaldırmak, uygulama taleplerini yönetmek veya kaydolma özniteliklerini değiştirebilirsiniz. İlkeleri ve kimlik doğrulama isteklerini OWIN nasıl birbirine bağlayın görene kadar denemeler yapın.

## <a name="sample-code-walkthrough"></a>Örnek kod gözden geçirme
Aşağıdaki bölümlerde örnek uygulama kodu nasıl yapılandırıldığını gösterir. Bu, gelecekte uygulama geliştirme kılavuz olarak kullanabilirsiniz.

### <a name="add-authentication-support"></a>Kimlik doğrulama desteği ekleme

Artık uygulamanızı Azure AD B2C'yi kullanacak şekilde yapılandırabilirsiniz. Uygulamanızın Openıd Connect kimlik doğrulaması istek göndererek Azure AD B2C ile iletişim kurar. İstekleri ilkeyi belirterek yürütmek için uygulamanızı istediği kullanıcı deneyimini dikte. Microsoft OWIN Kitaplığı'ı, bu istekleri göndermek, ilkeleri yürütmek, kullanıcı oturumları ve daha fazlasını yönetmek için kullanabilirsiniz.

#### <a name="install-owin"></a>OWIN yükleme

Başlamak için Visual Studio Paket Yöneticisi Konsolu'nu kullanarak OWIN ara yazılımı NuGet paketleri projeye ekleyin.

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a>OWIN başlangıç sınıfı ekleme

`Startup.cs` adlı API’ye bir OWIN başlangıç sınıfı ekleyin.  Projeye sağ tıklayın, **Ekle**'yi ve **Yeni Öğe**'yi seçin, ardından OWIN'i aratın. OWIN ara yazılımı, uygulamanız başlatıldığında `Configuration(…)` yöntemini çağırır.

Örneğimizde, sınıf bildirimi `public partial class Startup` olarak değiştirilmiş ve sınıfın `App_Start\Startup.Auth.cs` içindeki diğer kısmı kullanılmıştır. `Configuration` yöntemine, `Startup.Auth.cs` içinde tanımlanan bir `ConfigureAuth` çağrısı eklenmiştir. Değişikliklerden sonra `Startup.cs` aşağıdaki gibi görünür:

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

#### <a name="configure-the-authentication-middleware"></a>Kimlik doğrulaması ara yazılımını yapılandırma

Dosyayı açmak `App_Start\Startup.Auth.cs` ve uygulama `ConfigureAuth(...)` yöntemi. İçinde sağladığınız parametreler `OpenIdConnectAuthenticationOptions` uygulamanızı Azure AD B2C ile iletişim kurmak için koordinatları işlevi görür. Belirli bir parametre belirtmezseniz, varsayılan değeri kullanır. Örneğin, biz belirtmeyin `ResponseType` örnekte, böylece varsayılan değer `code id_token` her bir giden istek Azure AD B2C için kullanılır.

Tanımlama bilgisi kimlik doğrulamasını kurmanız gerekir. Openıd Connect ara yazılımını, başka şeylerin yanında kullanıcı oturumlarını korumak için tanımlama bilgileri kullanır.

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure the OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate the metadata address using the tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify the callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify the claims to validate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
                Scope = $"openid profile offline_access {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement the "Notification" methods...
}
```

İçinde `OpenIdConnectAuthenticationOptions` Openıd Connect ara yazılım tarafından alınan belirli bildirimleri için geri çağırma işlevleri bir dizi yukarıdaki tanımlarız. Bu davranışların kullanılarak bildirilen bir `OpenIdConnectAuthenticationNotifications` nesne ve içinde depolanan `Notifications` değişkeni. Örneğimizde, olaya bağlı olarak üç farklı geri çağırmaları tanımlarız.

### <a name="using-different-policies"></a>Farklı ilkelerini kullanma

`RedirectToIdentityProvider` Bildirim, Azure AD B2C'ye bir istek yapıldığında tetiklenir. Geri çağırma işlevi olarak `OnRedirectToIdentityProvider`, farklı bir ilke kullanmak istiyorsanız şu giden çağrısında denetleyin. Parola sıfırlama yapabilir veya bir profili düzenlemek için ilgili ilkeyi gibi parola sıfırlama İlkesi yerine varsayılan "Kaydolma veya oturum açma" ilkesini kullanmanız gerekir.

Örneğimizde, bir kullanıcı parolasını sıfırlama veya profili düzenlemek istediğinde OWIN bağlamına kullanmayı tercih ederiz İlkesi ekleyeceğiz. Aşağıdakileri yaparak, yapılabilir:

```CSharp
    // Let the middleware know you are trying to use the edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

Ve geri çağırma işlevi uygulayabilirsiniz `OnRedirectToIdentityProvider` aşağıdakileri yaparak:

```CSharp
/*
*  On each call to Azure AD B2C, check if a policy (e.g. the profile edit or password reset policy) has been specified in the OWIN context.
*  If so, use that policy when making the call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

### <a name="handling-authorization-codes"></a>Yetkilendirme kodları işleme

`AuthorizationCodeReceived` Bildirimi bir yetkilendirme kodu geldiğinde tetiklenir. Openıd Connect ara yazılımını alışverişi kodları için erişim belirteçleri desteklemez. Bir geri çağırma işlevi belirteç kodunu el ile değiştirebilir. Daha fazla bilgi için lütfen bakmak [belgeleri](active-directory-b2c-devquickstarts-web-api-dotnet.md) bilgiler veren nasıl.

### <a name="handling-errors"></a>Hataları işleme

`AuthenticationFailed` Bildirim kimlik doğrulama başarısız olduğunda tetiklenir. İstediğiniz gibi geri çağırma yöntemi hataları başa çıkabilir. Ancak bir denetleyin hata kodu eklemelisiniz `AADB2C90118`. "Kaydolma veya oturum açma" ilkesini yürütülmesi sırasında seçme fırsatı kullanıcının bir **parolanızı mı unuttunuz?** bağlantı. Bu durumda, Azure AD B2C, uygulamanız uygulamanızı parola sıfırlama ilkesini bunun yerine bir isteğin yapmalısınız belirten bu hata kodu gönderir.

```CSharp
/*
* Catch any failures received by the authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle the error code that Azure AD B2C throws when trying to reset a password from the login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

### <a name="send-authentication-requests-to-azure-ad"></a>Azure AD ile kimlik doğrulama istekleri gönderme

Uygulamanız artık Openıd Connect kimlik doğrulama protokolü kullanarak Azure AD B2C ile iletişim kurmak için düzgün şekilde yapılandırılmıştır. OWIN kimlik doğrulama iletilerinde kaynaklı, Azure AD B2C belirteçleri doğrulama ve kullanıcı oturumunun bakımını yapma işlemlerinin ayrıntılarını yönetir. Kalan tek şey her kullanıcının akışını başlatmak için.

Bir kullanıcı seçtiğinde **oturum yukarı/oturum açma**, **profili Düzenle**, veya **parolayı Sıfırla** web uygulamasında ilişkili eylemin çağrılması `Controllers\AccountController.cs`:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign up or sign in
*/
public void SignUpSignIn()
{
    // Use the default policy to process the sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting to edit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let the middleware know you are trying to use the edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set the page to redirect to after editing the profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting to reset a password
*/
public void ResetPassword()
{
    // Let the middleware know you are trying to use the reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set the page to redirect to after changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

OWIN, uygulama kullanıcının oturum açmak için de kullanabilirsiniz. İçinde `Controllers\AccountController.cs` sunuyoruz:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting to sign out
*/
public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

Kullanabileceğiniz bir ilke açıkça çağırma yanı sıra bir `[Authorize]` kullanıcı imzalı değilse, bir ilke yürüten denetleyicilerinizi etiketinde. Açık `Controllers\HomeController.cs` ve ekleme `[Authorize]` talep denetleyiciye etiketi.  Son ilke yapılandırılmış OWIN seçer `[Authorize]` etiketi isabet.

```CSharp
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a>Kullanıcı bilgilerini görüntüle

Openıd Connect kullanarak kullanıcıların kimlik doğrulaması sırasında Azure AD B2C kimlik belirteci içeren uygulamaya döndürür. **talep**. Bu kullanıcı hakkındaki onaylardır. Talepler, uygulamanızı kişiselleştirmek için kullanabilirsiniz.

`Controllers\HomeController.cs` dosyasını açın. Kullanıcı taleplerini denetleyicilerinizi erişebilirsiniz `ClaimsPrincipal.Current` güvenlik sorumlusu nesnesi.

```CSharp
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Aynı şekilde, uygulamanın aldığı herhangi bir talep erişebilirsiniz.  Uygulamanın aldığı tüm taleplerin listesi için kullanılabilir **talep** sayfası.
