---
title: Azure AD B2C | Microsoft Belgeleri
description: "Kimlik doğrulaması için OAuth 2.0 erişim belirteçleri ile güvenliği sağlanan Azure Active Directory B2C kullanarak .NET Web API&quot;si oluşturma."
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 1062af9abfd167dd251621a43943dea399aed027


---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: .NET web API'si oluşturma
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C ile OAuth 2.0 erişim belirteçleri kullanarak web API'si güvenliğini sağlayabilirsiniz. Bu belirteçler, Azure AD B2C kullanan istemci uygulamalarınızın API'ye ilişkin kimlik doğrulaması yapmasına olanak sağlar. Bu makalede kullanıcıların CRUD görevlerini gerçekleştirmesine imkan tanıyan bir .NET Model-View-Controller (MVC) "yapılacaklar listesi" API’sini oluşturma işlemi gösterilmektedir. Web API’sinin güvenliği Azure AD B2C kullanılarak sağlanır ve yapılacaklar listesini yalnızca kimliği doğrulanmış kullanıcıların yönetmesine izin verilir.

## <a name="create-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini oluşturma
Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce bu kılavuzda [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## <a name="create-an-application"></a>Uygulama oluşturma
Ardından B2C dizininizde uygulama oluşturmanız gerekir. Bu, uygulamanız ile güvenli şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Bir uygulama oluşturmak için [bu talimatları](active-directory-b2c-app-registration.md) izleyin. Şunları yaptığınızdan emin olun:

* Uygulamaya bir **web uygulaması** veya **web API'si** ekleyin.
* `https://localhost:44316/`Web uygulaması için **yeniden yönlendirme tekdüzen kaynak tanımlayıcısını** kullanın. Bu konum bu kod örneği için web uygulaması sunucusunun varsayılan konumudur.
* Uygulamanıza atanan **uygulama kimliğini** kopyalayın. Buna daha sonra ihtiyacınız olacak.

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>İlkelerinizi oluşturma
Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu kod örneğindeki istemci, üç kimlik deneyimi içerir: Kaydolma, oturum açma ve profil düzenleme. Her bir tür için [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#create-a-sign-up-policy) tanımlanan şekilde bir ilke oluşturmanız gerekecektir. Üç ilkenizi oluştururken şunları yaptığınızdan emin olun:

* Kimlik sağlayıcılar dikey penceresinde **Kullanıcı Kimliği ile kayıt** veya **E-posta ile kayıt** yöntemini seçin.
* Kaydolma ilkenizde, **Görünen ad** ve diğer kaydolma özniteliklerini seçin.
* Her ilke için uygulamanın talep ettiği gibi **Görünen ad** ve **Nesne Kimliği** öğelerini seçin. Diğer talepleri de seçebilirsiniz.
* Oluşturduktan sonra her ilkenin **Adını** kaydedin. Bu ilke adlarına daha sonra ihtiyacınız olacak.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Üç ilkenizi başarıyla oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-code"></a>Kodu indirme
Bu öğretici için kod [GitHub'da korunur](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). İşlemlere devam ederken örneği oluşturmak için [ bir çatı projesini .zip dosyası olarak indirebilirsiniz](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Ayrıca çatıyı kopyalayabilirsiniz:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Tamamlanan uygulama aynı zamanda [.zip dosyası olarak](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) veya aynı deponun `complete` dalı üzerinde de kullanılabilir.

Örnek kodu indirdikten sonra başlamak için Visual Studio .sln dosyasını açın. Çözüm dosyası iki proje içerir: `TaskWebApp` ve `TaskService`. `TaskWebApp`, kullanıcının etkileşime geçtiği bir MVC web uygulamasıdır. `TaskService`, uygulamanın, her kullanıcının yapılacaklar listesini depolayan arka uç web API'sidir.

## <a name="configure-the-task-web-app"></a>Görev web uygulamasını yapılandırma
Bir kullanıcı `TaskWebApp` ile etkileşime girdiğinde istemci Azure AD'ye istekler gönderir ve `TaskService` web API'sini çağırmak için kullanılabilen belirteçleri geri alır. Kullanıcıyla oturum açmak ve belirteçleri almak için uygulamanız hakkında bazı bilgiler ile `TaskWebApp` sağlamanız gerekir. `TaskWebApp` projesi içinde proje kökündeki `web.config` dosyasını açın ve `<appSettings>` bölümündeki değerleri değiştirin.  `AadInstance`, `RedirectUri`, ve `TaskServiceUrl` değerlerini olduğu gibi bırakabilirsiniz.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

Bu makale `TaskWebApp` istemcisinin derlenmesini kapsamaz.  Azure AD B2C kullanarak bir web uygulaması derlemeyi öğrenmek için bkz. [.NET web uygulaması öğreticisi](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>API güvenliğini sağlama
Kullanıcılar adına API çağıran bir istemciniz olduğunda `TaskService` güvenliğini OAuth 2.0 taşıyıcı belirteçlerini kullanarak sağlayabilirsiniz. API'niz .NET (OWIN) kitaplığı için Microsoft'un Açık Web Arabirimi'ni kullanarak belirteçleri kabul edebilir ve doğrulayabilir.

### <a name="install-owin"></a>OWIN yükleme
OWIN OAuth kimlik doğrulaması işlem hattı yükleyerek çalışmaya başlayın:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>B2C bilgilerinizi girme
`TaskService` projesi kökündeki `web.config` dosyasını açın ve `<appSettings>` bölümündeki değerleri değiştirin: API ve OWIN kitaplığı boyunca bu değerler kullanılır.  `AadInstance` değerini değiştirmeden bırakabilirsiniz.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>OWIN başlangıç sınıfı ekleme
`Startup.cs` adlı `TaskService` projesine OWIN başlangıç sınıfı ekleyin.  Projeye sağ tıklayın, **Ekle**'yi ve **Yeni Öğe**'yi seçin, ardından OWIN'i aratın.

```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0 kimlik doğrulamasını yapılandırma
`App_Start\Startup.Auth.cs` dosyasını açın ve `ConfigureAuth(...)` yöntemini uygulayın:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Görev denetleyicisinin güvenliğini sağlama
Uygulama OAuth 2.0 kimlik doğrulaması kullanacak şekilde yapılandırıldıktan sonra görev denetleyicisine `[Authorize]` etiketi ekleyerek web API'nizin güvenliğini sağlayabilirsiniz. Bu, tüm yapılacaklar listesi işlemelerinin yapıldığı denetleyicidir, yani tüm denetleyicinin sınıf düzeyinde güvenliğini sağlamanız gerekir. Ayrıca daha ayrıntılı denetim için bireysel işlemlere `[Authorize]` etiketi ekleyebilirsiniz.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Belirteçten kullanıcı bilgilerini alma
`TasksController`, görevleri, her görevin göreve "sahip olan" kullanıcı ile ilişkilendirildiği bir veritabanında depolar. Sahip, kullanıcının **nesne kimliği** ile tanımlanır. (Tüm ilkelerinize uygulama talebi olarak nesne kimliği eklemeniz gerekmesinin nedeni budur.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma
Son olarak hem `TaskWebApp` hem de `TaskService` öğesini oluşturup çalıştırın. Bir e-posta adresi veya kullanıcı adı kullanarak uygulamaya kaydolun. Kullanıcının yapılacaklar listesinde bazı görevler oluşturun ve istemciyi durdurmanız ve yeniden başlatmanız sonrasında bile API içinde nasıl kalıcı olduğuna dikkat edin.

## <a name="edit-your-policies"></a>İlkelerinizi düzenleme
Azure AD B2C kullanarak API güvenliğini sağladıktan sonra uygulamanızın ilkelerini deneyebilir ve API üzerindeki etkileri (veya eksiklikleri) görüntüleyebilirsiniz. Ayrıca ilkelerdeki uygulama talepleri denetleyebilir ve web API'sinde kullanılabilen kullanıcı bilgilerini değiştirebilirsiniz. Eklediğiniz tüm talepler bu makalede daha önce açıklandığı gibi `ClaimsPrincipal` nesnesindeki .NET MVC web API'nizde kullanılabilir olacaktır.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->



<!--HONumber=Dec16_HO4-->


