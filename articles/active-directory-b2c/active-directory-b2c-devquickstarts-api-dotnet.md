<properties
    pageTitle="Azure AD B2C Önizlemesi | Microsoft Azure"
    description="Kimlik doğrulaması için OAuth 2.0 erişim belirteçleri ile güvenliği sağlanan Azure Active Directory B2C kullanarak .NET Web API'si oluşturma."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/16/2016"
    ms.author="dastrock"/>

# Azure Active Directory B2C önizlemesi: .NET web API'si oluşturma

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C ile OAuth 2.0 erişim belirteçleri kullanarak web API'si güvenliğini sağlayabilirsiniz. Bu belirteçler, Azure AD B2C kullanan istemci uygulamalarınızın API'ye ilişkin kimlik doğrulaması yapmasına olanak sağlar. Bu makalede kullanıcı kaydı, oturum açma ve profil yönetimini kapsayan .NET Model-View-Controller (MVC) "yapılacaklar listesi" uygulamasını nasıl oluşturulacağı gösterilir. Her kullanıcının yapılacaklar listesi bir görev hizmeti tarafından depolanır. Bu, kimliği doğrulanmış kullanıcıların, yapılacaklar listelerinde görevler oluşturmasına ve okumasına olanak sağlayan bir web API'sidir.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Azure AD B2C dizini oluşturma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce bu kılavuzda [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## Uygulama oluşturma

Ardından B2C dizininizde uygulama oluşturmanız gerekir. Bu, uygulamanız ile güvenli şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Bir uygulama oluşturmak için [bu talimatları](active-directory-b2c-app-registration.md) izleyin. Şunları yaptığınızdan emin olun:

- Uygulamaya bir **web uygulaması** veya **web API'si** ekleyin.
- `https://localhost:44316/`Web uygulaması için **yeniden yönlendirme tekdüzen kaynak tanımlayıcısını** kullanın. Bu konum bu kod örneği için web uygulaması sunucusunun varsayılan konumudur.
- Uygulamanıza atanan **uygulama kimliğini** kopyalayın. Buna daha sonra ihtiyacınız olacak.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## İlkelerinizi oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu kod örneğindeki istemci, üç kimlik deneyimi içerir: Kaydolma, oturum açma ve profil düzenleme. Her bir tür için [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy) tanımlanan şekilde bir ilke oluşturmanız gerekecektir. Üç ilkenizi oluştururken şunları yaptığınızdan emin olun:

- Kimlik sağlayıcılar dikey penceresinde **Kullanıcı Kimliği ile kayıt** veya **E-posta ile kayıt** yöntemini seçin.
- Kaydolma ilkenizde, **Görünen ad** ve diğer kaydolma özniteliklerini seçin.
- Her ilke için uygulamanın talep ettiği gibi **Görünen ad** ve **Nesne Kimliği** öğelerini seçin. Diğer talepleri de seçebilirsiniz.
- Oluşturduktan sonra her ilkenin **Adını** kaydedin. Bu ilke adlarına daha sonra ihtiyacınız olacak.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Üç ilkenizi başarıyla oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## Kodu indirme

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-devquickstarts-bug-fix.md)]

Bu öğretici için kod [GitHub'da korunur](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). İşlemlere devam ederken örneği oluşturmak için [ bir çatı projesini .zip dosyası olarak indirebilirsiniz](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Ayrıca çatıyı kopyalayabilirsiniz:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Tamamlanan uygulama aynı zamanda [.zip dosyası olarak](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) veya aynı deponun `complete` dalı üzerinde de kullanılabilir.

Örnek kodu indirdikten sonra başlamak için Visual Studio .sln dosyasını açın. Çözüm dosyası iki proje içerir: `TaskWebApp` ve `TaskService`. `TaskWebApp` kullanıcı ile etkileşime giren bir MVC web uygulamasıdır. `TaskService` her kullanıcının yapılacaklar listesini depolayan arka uç uygulamasının web API'sidir.

## Görev web uygulamasını yapılandırma

Bir kullanıcı `TaskWebApp` ile etkileşime girdiğinde istemci Azure AD'ye istekler gönderir ve `TaskService` web API'sini çağırmak için kullanılabilen belirteçleri geri alır. Kullanıcıyla oturum açmak ve belirteçleri almak için uygulamanız hakkında bazı bilgiler ile `TaskWebApp` sağlamanız gerekir. `TaskWebApp` projesi içinde proje kökündeki `web.config` dosyasını açın ve `<appSettings>` bölümündeki değerleri değiştirin:

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="{Enter the name of your B2C directory, e.g. contoso.onmicrosoft.com}" />
    <add key="ida:ClientId" value="{Enter the Application ID assigned to your app by the Azure Portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609}" />
    <add key="ida:ClientSecret" value="{Enter the Application Secret you created in the Azure Portal, e.g. yGNYWwypRS4Sj1oYXd0443n}" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="[Enter your sign up policy name, e.g. b2c_1_sign_up]" />
    <add key="ida:SignInPolicyId" value="[Enter your sign in policy name, e.g. b2c_1_sign_in]" />
    <add key="ida:UserProfilePolicyId" value="[Enter your edit profile policy name, e.g. b2c_1_profile_edit" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Ayrıca oturum açma ilkenizin adını sağlamanızı gerektiren iki `[PolicyAuthorize]` dekoratörü vardır. `[PolicyAuthorize]` özniteliği, bir kullanıcı kimlik doğrulaması gerektiren uygulamadaki bir sayfaya erişmeye çalıştığında, belirli bir ilke çağırmak için kullanılır.

```C#
// Controllers\HomeController.cs

[PolicyAuthorize(Policy = "{Enter the name of your sign in policy, e.g. b2c_1_my_sign_in}")]
public ActionResult Claims()
{
```

```C#
// Controllers\TasksController.cs

[PolicyAuthorize(Policy = "{Enter the name of your sign in policy, e.g. b2c_1_my_sign_in}")]
public class TasksController : Controller
{
```

`TaskWebApp` gibi bir web uygulamasının Azure AD B2C'yi nasıl kullandığını öğrenmek için bkz. [.NET web uygulaması oluşturma](active-directory-b2c-devquickstarts-web-dotnet.md).

## API güvenliğini sağlama

Kullanıcılar adına API çağıran bir istemciniz olduğunda `TaskService` güvenliğini OAuth 2.0 taşıyıcı belirteçlerini kullanarak sağlayabilirsiniz. API'niz .NET (OWIN) kitaplığı için Microsoft'un Açık Web Arabirimi'ni kullanarak belirteçleri kabul edebilir ve doğrulayabilir.

### OWIN yükleme
OWIN OAuth kimlik doğrulaması işlem hattı yükleyerek çalışmaya başlayın:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### B2C bilgilerinizi girme
`TaskService` projesi kökündeki `web.config` dosyasını açın ve `<appSettings>` bölümündeki değerleri değiştirin: API ve OWIN kitaplığı boyunca bu değerler kullanılır.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/{1}/{2}?p={3}" />
    <add key="ida:Tenant" value="{Enter the name of your B2C tenant - it usually looks like constoso.onmicrosoft.com}" />
    <add key="ida:ClientId" value="{Enter the Application ID assigned to your app by the Azure Portal}" />
    <add key="ida:PolicyId" value="{Enter the name of one of the policies you created, like `b2c_1_my_sign_in_policy`}" />
</appSettings>
```

### OWIN başlangıç sınıfı ekleme
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

### OAuth 2.0 kimlik doğrulamasını yapılandırma
`App_Start\Startup.Auth.cs` dosyasını açın ve `ConfigureAuth(...)` yöntemini uygulayın:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string commonPolicy = ConfigurationManager.AppSettings["ida:PolicyId"];
    private const string discoverySuffix = ".well-known/openid-configuration";

    public void ConfigureAuth(IAppBuilder app)
    {   
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API accepts tokens only from its own clients
            ValidAudience = clientId,
        };

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {   
            // This SecurityTokenProvider fetches the Azure AD B2C metadata and signing keys from the OpenID Connect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, "v2.0", discoverySuffix, commonPolicy)))
        });
    }
}
```

### Görev denetleyicisinin güvenliğini sağlama
Uygulama OAuth 2.0 kimlik doğrulaması kullanacak şekilde yapılandırıldıktan sonra görev denetleyicisine `[Authorize]` etiketi ekleyerek web API'nizin güvenliğini sağlayabilirsiniz. Bu, tüm yapılacaklar listesi işlemelerinin yapıldığı denetleyicidir, yani tüm denetleyicinin sınıf düzeyinde güvenliğini sağlamanız gerekir. Ayrıca daha ayrıntılı denetim için bireysel işlemlere `[Authorize]` etiketi ekleyebilirsiniz.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### Belirteçten kullanıcı bilgilerini alma
`TasksController` görevleri, her görevin göreve "sahip olan" kullanıcı ile ilişkilendirildiği bir veritabanında depolar. Sahip, kullanıcının **nesne kimliği** ile tanımlanır. (Tüm ilkelerinize uygulama talebi olarak nesne kimliği eklemeniz gerekmesinin nedeni budur.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## Örnek uygulamayı çalıştırma

Son olarak hem `TaskWebApp` hem de `TaskService` öğesini oluşturup çalıştırın. Bir e-posta adresi veya kullanıcı adı kullanarak uygulamaya kaydolun. Kullanıcının yapılacaklar listesinde bazı görevler oluşturun ve istemciyi durdurmanız ve yeniden başlatmanız sonrasında bile API içinde nasıl kalıcı olduğuna dikkat edin.

## İlkelerinizi düzenleme

Azure AD B2C kullanarak API güvenliğini sağladıktan sonra uygulamanızın ilkelerini deneyebilir ve API üzerindeki etkileri (veya eksiklikleri) görüntüleyebilirsiniz. Ayrıca <!--add **identity providers** to the policies, allowing you users to sign into the Task Client using social accounts.  You can also --> ilkelerdeki uygulama talepleri denetleyebilir ve web API'sinde kullanılabilen kullanıcı bilgilerini değiştirebilirsiniz. Eklediğiniz tüm talepler bu makalede daha önce açıklandığı gibi `ClaimsPrincipal` nesnesindeki .NET MVC web API'nizde kullanılabilir olacaktır.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->



<!---HONumber=Jun16_HO2-->


