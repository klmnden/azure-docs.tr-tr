---
title: Azure Active Directory B2C | Microsoft Docs
description: "Oturumu-up/oturum açma sahip bir web uygulamasının nasıl oluşturulacağını, profil düzenleme ve parola sıfırlama ve Azure Active Directory B2C kullanarak."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: mtillman
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: e7a10ab2e523a98bd8762e209d0f4a13b12ef187
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a>Azure Active Directory B2C kaydolma, oturum açma profili düzenleme ve parola sıfırlama ile bir ASP.NET web uygulaması oluşturma

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Web uygulamanızı Azure AD B2C kimlik özellik ekleme
> * Azure AD B2C dizininizde web uygulamanızı kaydetme
> * Kullanıcı oturumu-up/oturum açma, profil düzenleme ve web uygulamanız için parola sıfırlama ilkesi oluşturma

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabına B2C Kiracınızın bağlanmanız gerekir. Ücretsiz bir Azure hesabı oluşturabilirsiniz [burada](https://azure.microsoft.com/en-us/).
- Gereksinim duyduğunuz [Microsoft Visual Studio](https://www.visualstudio.com/) görüntülemek ve örnek kod değiştirmek için veya benzer bir program.

## <a name="create-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini oluşturma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Bir dizin, tüm kullanıcıların, uygulamaları, grupları ve daha fazlası için bir kapsayıcıdır. Zaten yoksa, bu kılavuza devam etmeden önce bir B2C dizini oluşturun.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> B2C Kiracı Azure aboneliğinize bağlanmak için gerekir. Seçtikten sonra **oluşturma**seçin **Azure Aboneliğim bağlantı var olan bir Azure AD B2C Kiracısına** seçeneğini ve ardından **Azure AD B2C Kiracısı** açılan, ilişkilendirmek istediğiniz Kiracı seçin.

## <a name="create-and-register-an-application"></a>Oluşturma ve bir uygulamayı kaydetme

Ardından, oluşturma ve uygulama B2C dizininizde kaydetmeniz gerekir. Bu, Azure AD B2C uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri sağlar. 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

İşiniz bittiğinde, uygulamanızın ayarlarını bir API ve yerel bir uygulamaya gerekir.

## <a name="create-policies-on-your-b2c-tenant"></a>B2C kiracınızın ilkeleri oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu kod örneği üç kimlik deneyimi içerir: **kaydolma ve oturum açma**, **düzenleme profil**, ve **parola sıfırlama**.  Her tür için [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md) tanımlanan şekilde bir ilke oluşturmanız gerekir. Her ilke için bir görünen ad özniteliği ya da talep seçin ve ilkeniz adı daha sonra kullanmak için aşağı kopyalamak için emin olun.

### <a name="add-your-identity-providers"></a>Kimlik sağlayıcısı ekleyin

Ayarlarınızı seçin **kimlik sağlayıcıları** ve kullanıcı adı kayıt ya da e-posta'i seçin.

### <a name="create-a-sign-up-and-sign-in-policy"></a>Kaydolma ve oturum açma ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a>İlke düzenleme profil oluşturma

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a>Bir parola sıfırlama ilkesi oluşturma

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

İlkelerinizi oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-sample-code"></a>Örnek kodu indirin

Bu öğretici için kod [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi)'da korunur. Örneği kopyalamak için şu komutu çalıştırın:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Örnek kodu indirdikten sonra başlamak için Visual Studio .sln dosyasını açın. Çözüm dosyası iki proje içerir: `TaskWebApp` ve `TaskService`. `TaskWebApp`kullanıcı ile etkileşime giren MVC bir web uygulamasıdır. `TaskService`, uygulamanın, her kullanıcının yapılacaklar listesini depolayan arka uç web API'sidir. Bu makalede yalnızca `TaskWebApp` uygulaması ele alınacaktır. Nasıl yapılandıracağınızı öğrenmek için `TaskService` Azure AD B2C kullanarak bkz [.NET web API'si öğreticimizi](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="update-code-to-use-your-tenant-and-policies"></a>Kiracı ve ilkelerini kullanmak için kodu güncelleştirme

Örneğimiz, tanıtım kiracımızın ilkelerini ve istemci kimliğini kullanacak şekilde yapılandırılmıştır. Kendi Kiracı bağlanmak için açmanız gerekir `web.config` içinde `TaskWebApp` proje ve aşağıdaki değerleri değiştirin:

* kiracı adınızla `ida:Tenant`
* Web uygulamanızın kimliği ile `ida:ClientId`
* Web uygulamanızın gizli anahtarı ile `ida:ClientSecret`
* "Kaydolma veya Oturum açma" ilkenizin adıyla `ida:SignUpSignInPolicyId`
* "Profil Düzenleme" ilkenizin adıyla `ida:EditProfilePolicyId`
* "Parola Sıfırlama" ilkenizin adıyla `ida:ResetPasswordPolicyId`

## <a name="launch-the-app"></a>Uygulamayı başlatın
Gelen Visual Studio içinde uygulamasını başlatın. Yapılacaklar listesi sekmesine gidin ve URl olduğuna dikkat edin: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id =*YourclientID*...

E-posta adresi veya kullanıcı adı kullanarak uygulama için kaydolun. Çıkışı, oturum sonra yeniden oturum açın ve profil düzenleme veya parola sıfırlama. Oturumu kapatın ve farklı bir kullanıcı olarak oturum açın. 

## <a name="add-social-idps"></a>Sosyal IDPs Ekle

Şu anda kullanarak uygulamayı yalnızca kullanıcının kaydolma ve oturum açma destekler **yerel hesaplar**; B2C dizininizde depolanan hesapları, bir kullanıcı adı ve parola kullanın. Azure AD B2C kullanarak diğer için destek ekleyebilmesi **kimlik sağlayıcıları** (IDPs) herhangi birini kodunuzu değiştirmeden.

Sosyal IDPs uygulamanıza eklemek için bu makaleler ayrıntılı yönergeleri izleyerek başlayın. Desteklemek istediğiniz her IDP için bu sistemde bir uygulamayı kaydetme ve bir istemci kimliği elde gerekir

* [Facebook bir IDP ayarlayın](active-directory-b2c-setup-fb-app.md)
* [Google bir IDP ayarlayın](active-directory-b2c-setup-goog-app.md)
* [Amazon bir IDP ayarlayın](active-directory-b2c-setup-amzn-app.md)
* [LinkedIn bir IDP ayarlayın](active-directory-b2c-setup-li-app.md)

B2C dizininize kimlik sağlayıcıları ekledikten sonra açıklandığı gibi her yeni IDPs eklemek için üç ilkenizi Düzenle [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md). İlkelerinizi kaydettikten sonra uygulamayı yeniden çalıştırın.  Oturum açma eklenen yeni IDPs görmeniz gerekir ve kaydolma seçenekleri her kimliğinizi karşılaştığında.

İlkelerinizle denemeler ve örnek uygulamanızı üzerindeki etkisini gözlemleyin. Ekleyebilir veya IDPs kaldırmak, uygulamanın talep değiştirmek veya kaydolma özniteliklerini değiştirebilirsiniz. İlkeleri ve kimlik doğrulama isteklerini OWIN nasıl birbirine bağlayan görene kadar deneyin.

## <a name="sample-code-walkthrough"></a>Örnek kod gözden geçirme
Aşağıdaki bölümlerde, örnek uygulama kodu nasıl yapılandırıldığını gösterir. Bu kılavuz olarak gelecekte uygulama geliştirmede kullanabilir.

### <a name="add-authentication-support"></a>Kimlik doğrulama desteği ekleme

Artık uygulamanızı Azure AD B2C kullanmak için yapılandırabilirsiniz. Uygulamanız, Openıd Connect kimlik doğrulama istekleri göndererek Azure AD B2C ile iletişim kurar. İstekleri ilkesini belirterek yürütmek için uygulamanızı istediği kullanıcı deneyimi dikte. Microsoft'un OWIN Kitaplığı'ı, bu istekleri göndermek, ilkeleri yürütmek, kullanıcı oturumları ve daha fazlasını yönetmek için kullanabilirsiniz.

#### <a name="install-owin"></a>OWIN yükleme

Başlamak için Visual Studio Paket Yöneticisi konsolu kullanılarak OWIN ara yazılımı NuGet paketlerini projeye ekleyin.

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

Dosyayı açmak `App_Start\Startup.Auth.cs` ve uygulamanıza `ConfigureAuth(...)` yöntemi. Sağladığınız içinde parametreleri `OpenIdConnectAuthenticationOptions` uygulamanız için Azure AD B2C ile iletişim kurmak için koordinatları olarak hizmet. Belirli parametreleri belirtmezseniz, varsayılan değeri kullanır. Örneğin, biz belirtmeyin `ResponseType` örnekte, böylece varsayılan değer `code id_token` Azure AD B2C giden her istekte kullanılır.

Tanımlama bilgisi kimlik doğrulamasını kurmanız gerekir. Openıd Connect ara yazılım, başka şeylerin kullanıcı oturumlarını korumak için tanımlama bilgilerini kullanır.

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
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement the "Notification" methods...
}
```

İçinde `OpenIdConnectAuthenticationOptions` geri çağırma işlevleri Openıd Connect ara yazılım tarafından alınan belirli bildirimleri için bir dizi yukarıdaki tanımlarız. Bu davranışların kullanarak tanımlanan bir `OpenIdConnectAuthenticationNotifications` nesne ve içine depolanmış `Notifications` değişkeni. Bizim örnek olay bağlı olarak üç farklı geri aramalar tanımlayın.

### <a name="using-different-policies"></a>Farklı ilkelerini kullanma

`RedirectToIdentityProvider` Bildirim Azure AD B2C'ye bir istek yapıldığında tetiklenir. Geri arama işlevinde `OnRedirectToIdentityProvider`, biz başka bir ilke kullanmak istiyorsanız, biz giden çağrısında denetleyin. Parola sıfırlama yapın veya bir profili düzenlemek için varsayılan "Kaydolma veya oturum açma" ilke yerine İlkesi parola sıfırlama gibi ilgili İlkesi kullanmanız gerekir.

Bir kullanıcı parolasını sıfırlama veya profil düzenleme istediğinde Örneğimizde, biz biz OWIN bağlamına kullanmayı tercih ilkesi ekleyin. Aşağıdakileri yaparak yapılabilir:

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

`AuthorizationCodeReceived` Bildirim bir yetkilendirme kodu alındığında tetiklenir. Openıd Connect ara yazılımı için erişim belirteçleri değiş tokuşu kodlarını desteklemez. Bir geri çağırma işlevini belirteç kodunu el ile değiştirebilir. Daha fazla bilgi için lütfen bakmak [belgelerine](active-directory-b2c-devquickstarts-web-api-dotnet.md) , açıklar nasıl.

### <a name="handling-errors"></a>Hataları işleme

`AuthenticationFailed` Bildirim kimlik doğrulama başarısız olduğunda tetiklenir. İstediğiniz gibi kendi geri çağırma yönteminde hataları işleyebilir. Hata kodu denetle ancak eklemelisiniz `AADB2C90118`. "Kaydolma veya oturum açma" ilke yürütülmesi sırasında kullanıcı seçin fırsatına sahip bir **parolanızı mı unuttunuz?** bağlantı. Bu durumda, uygulamanızı parolası sıfırlama ilkesini kullanmayı bir istek olmalısınız belirten bu hata kodu uygulamanızı Azure AD B2C gönderir.

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

### <a name="send-authentication-requests-to-azure-ad"></a>Azure AD kimlik doğrulama isteklerini gönderme

Uygulamanız artık Openıd Connect kimlik doğrulama protokolü kullanarak Azure AD B2C ile iletişim kurmak için düzgün şekilde yapılandırılmıştır. OWIN kimlik doğrulama iletileri hazırlayın, Azure AD B2C gelen belirteçleri doğrulamak ve kullanıcı oturumu koruyarak ayrıntılarını yönetir. Kalan tek şey her kullanıcının akış başlatmak için.

Bir kullanıcı seçtiğinde **oturum yukarı/oturum açma**, **Düzenle profili**, veya **parola sıfırlama** web uygulamasında ilişkili eylemin çağrılması `Controllers\AccountController.cs`:

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

OWIN, uygulama kullanıcıdan çıkışı imzalamak için de kullanabilirsiniz. İçinde `Controllers\AccountController.cs` sunuyoruz:

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

Kullanabileceğiniz bir ilke açıkça çağırma yanı sıra bir `[Authorize]` kullanıcı imzalı değilse bir ilke yürütür etiketinde denetleyicilerinizi. Açık `Controllers\HomeController.cs` ve ekleme `[Authorize]` talep denetleyicisine etiketi.  Son ilke yapılandırılan OWIN seçer `[Authorize]` etiketi isabet.

```CSharp
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a>Kullanıcı bilgilerini görüntüleme

Openıd Connect kullanarak kullanıcıların kimlik doğrulaması sırasında Azure AD B2C kimliği belirteci içeren uygulamaya döndürür. **talep**. Bu, kullanıcı hakkında onaylamalardır. Uygulamanızı kişiselleştirmek için talep kullanabilirsiniz.

`Controllers\HomeController.cs` dosyasını açın. Denetleyicilerinizi, kullanıcı talepleri erişim `ClaimsPrincipal.Current` güvenlik sorumlusu nesnesi.

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

Aynı şekilde, uygulamanızın alan talep erişebilir.  Uygulama aldığı tüm talepler listesinin, üzerinde kullanılabilir **talep** sayfası.
