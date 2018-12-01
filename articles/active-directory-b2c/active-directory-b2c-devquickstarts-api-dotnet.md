---
title: Azure Active Directory B2C'yi bir ASP.NET web API'SİNİN güvenliğini sağlama | Microsoft Docs
description: Kimlik doğrulaması için OAuth 2.0 erişim belirteçleri ile güvenliği sağlanan Azure Active Directory B2C kullanarak .NET Web API'si oluşturma.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3283e7b0e0b7e20d4b8522f08ab2460504fa355f
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52723440"
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: .NET web API'si oluşturma

Azure Active Directory (Azure AD) B2C ile OAuth 2.0 erişim belirteçleri kullanarak web API'si güvenliğini sağlayabilirsiniz. Bu belirteçler, istemci uygulamalarınızın API'ye ilişkin kimlik doğrulaması yapmasına olanak sağlar. Bu makalede istemci uygulamanızın kullanıcılarının CRUD görevlerini gerçekleştirmesine imkan tanıyan bir .NET MVC "yapılacaklar listesi" API’sini oluşturma işlemi gösterilmektedir. Web API’sinin güvenliği Azure AD B2C kullanılarak sağlanır ve yapılacaklar listesini yalnızca kimliği doğrulanmış kullanıcıların yönetmesine izin verilir.

## <a name="create-an-azure-ad-b2c-directory"></a>Azure AD B2C dizini oluşturma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir. Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce bu kılavuzda [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

> [!NOTE]
> İstemci uygulaması ve web API’si aynı Azure AD B2C dizinini kullanmalıdır.
>

## <a name="create-a-web-api"></a>Web API’si oluşturma

Ardından B2C dizininizde bir web API uygulaması oluşturmanız gerekir. Bu, uygulamanız ile güvenli şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Bir uygulama oluşturmak için [bu talimatları](active-directory-b2c-app-registration.md) izleyin. Şunları yaptığınızdan emin olun:

* Uygulamaya bir **web uygulaması** veya **web API'si** ekleyin.
* Web uygulamasının **Yeniden Yönlendirme URI’sini** `https://localhost:44332/` kullanın. Bu konum bu kod örneği için web uygulaması sunucusunun varsayılan konumudur.
* Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Buna daha sonra ihtiyacınız olacak.
* **Uygulama Kimliği URI'si** alanına bir uygulama tanımlayıcısı girin. Tam **Uygulama Kimliği URI'si** değerini kopyalayın. Buna daha sonra ihtiyacınız olacak.
* **Yayımlanmış kapsamlar** menüsü üzerinden izinler ekleyin.

## <a name="create-your-user-flows"></a>Kullanıcı akışlarınızı oluşturun

Azure AD B2C'de, her kullanıcı deneyimi tarafından tanımlanan bir [kullanıcı akışı](active-directory-b2c-reference-policies.md). Azure AD B2C ile iletişim kurmak için bir kullanıcı akışı oluşturmanız gerekir. Birleştirilmiş kullanıcı oturumu açma kaydolma/oturum açma akışını kullanarak açıklandığı öneririz [kullanıcı akışı başvurusu makalesinde](active-directory-b2c-reference-policies.md). Kullanıcı akış oluşturduğunuzda, emin olun:

* Seçin **görünen ad** ve diğer kaydolma özniteliklerini kullanıcı akışınızı.
* Seçin **görünen ad** ve **nesne kimliği** her kullanıcı akış için uygulamanın talep ettiği gibi. Diğer talepleri de seçebilirsiniz.
* Kopyalama **adı** oluşturduktan sonra her kullanıcı akış. Userjourney adı daha sonra ihtiyacınız olacak.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kullanıcı akışı başarıyla oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

## <a name="download-the-code"></a>Kodu indirme

Bu öğretici için kod [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi)'da korunur. Örneği kopyalamak için şu komutu çalıştırın:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Örnek kodu indirdikten sonra başlamak için Visual Studio .sln dosyasını açın. Çözüm dosyası iki proje içerir: `TaskWebApp` ve `TaskService`. `TaskWebApp`, kullanıcının etkileşime geçtiği bir MVC web uygulamasıdır. `TaskService`, uygulamanın, her kullanıcının yapılacaklar listesini depolayan arka uç web API'sidir. Bu makalede yalnızca `TaskService` uygulaması ele alınacaktır. Azure AD B2C kullanarak bir `TaskWebApp` derlemeyi öğrenmek için bkz. [.NET web uygulaması öğreticisi](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>Azure AD B2C yapılandırmasını güncelleştirme

Bizim örneğimiz, tanıtım kiracımızın istemci kimliği ve kullanıcı akışları kullanmak için yapılandırılır. Kendi kiracınızı kullanmak istiyorsanız aşağıdakileri yapmanız gerekir:

1. `TaskService` projesinde `web.config` öğesini açın ve şu değerleri değiştirin:
    * kiracı adınızla `ida:Tenant`
    * Web API uygulamanızın kimliği ile `ida:ClientId`
    * `ida:SignUpSignInPolicyId` ile "Kaydolma veya oturum açma" kullanıcı Akış adı

2. `TaskWebApp` projesinde `web.config` öğesini açın ve şu değerleri değiştirin:
    * kiracı adınızla `ida:Tenant`
    * Web uygulamanızın kimliği ile `ida:ClientId`
    * Web uygulamanızın gizli anahtarı ile `ida:ClientSecret`
    * `ida:SignUpSignInPolicyId` ile "Kaydolma veya oturum açma" kullanıcı Akış adı
    * `ida:EditProfilePolicyId` "Profili Düzenle" kullanıcı akışı adıyla
    * `ida:ResetPasswordPolicyId` ile "Parolayı Sıfırla" kullanıcı Akış adı
    * "Uygulama İlkesi URI'si" ile `api:ApiIdentifier`


## <a name="secure-the-api"></a>API güvenliğini sağlama

API’nizi çağıran bir istemciniz olduğunda API’nizin (örn. `TaskService`) güvenliğini OAuth 2.0 taşıyıcı belirteçlerini kullanarak sağlayabilirsiniz. Bunun yapılması, API’nize gönderilen her isteğin yalnızca istekte bir taşıyıcı belirteç varsa geçerli olmasını sağlar. API'niz .NET (OWIN) kitaplığı için Microsoft'un Açık Web Arabirimi'ni kullanarak taşıyıcı belirteçleri kabul edebilir ve doğrulayabilir.

### <a name="install-owin"></a>OWIN yükleme

İlk olarak Visual Studio Paket Yöneticisi Konsolu’nu kullanarak OWIN OAuth kimlik doğrulama işlem hattını yükleyin.

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

Bunun yapılması, taşıyıcı belirteçleri kabul edip doğrulayacak OWIN ara yazılımını yükler.

### <a name="add-an-owin-startup-class"></a>OWIN başlangıç sınıfı ekleme

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

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0 kimlik doğrulamasını yapılandırma

`App_Start\Startup.Auth.cs` dosyasını açın ve `ConfigureAuth(...)` yöntemini uygulayın. Örneğin, aşağıdaki gibi görünebilir:

```CSharp
// App_Start\Startup.Auth.cs

 public partial class Startup
    {
        // These values are pulled from web.config
        public static string AadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
        public static string Tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
        public static string SignUpSignInPolicy = ConfigurationManager.AppSettings["ida:SignUpSignInPolicyId"];
        public static string DefaultPolicy = SignUpSignInPolicy;

        /*
         * Configure the authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where the audience of the token is equal to the client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-the-task-controller"></a>Görev denetleyicisinin güvenliğini sağlama

Uygulama OAuth 2.0 kimlik doğrulaması kullanacak şekilde yapılandırıldıktan sonra görev denetleyicisine `[Authorize]` etiketi ekleyerek web API'nizin güvenliğini sağlayabilirsiniz. Bu, tüm yapılacaklar listesi işlemelerinin yapıldığı denetleyicidir, yani tüm denetleyicinin sınıf düzeyinde güvenliğini sağlamanız gerekir. Ayrıca daha ayrıntılı denetim için bireysel işlemlere `[Authorize]` etiketi ekleyebilirsiniz.

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Belirteçten kullanıcı bilgilerini alma

`TasksController`, görevleri, her görevin göreve "sahip olan" kullanıcı ile ilişkilendirildiği bir veritabanında depolar. Sahip, kullanıcının **nesne kimliği** ile tanımlanır. (Bir uygulama olarak nesne kimliği eklemeniz gerekmesinin nedeni budur tüm akışlarınız kullanıcı talebi.)

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-the-permissions-in-the-token"></a>Belirteçteki izinleri doğrulama

Web API’lerine yönelik genel bir gereksinim, belirteçteki mevcut "kapsamların" doğrulanmasıdır. Bunun yapılması, kullanıcının yapılacaklar listesi hizmetine erişmesi için gereken izinleri onaylamasını sağlar.

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "The Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Son olarak hem `TaskWebApp` hem de `TaskService` öğesini oluşturup çalıştırın. Kullanıcının yapılacaklar listesinde bazı görevler oluşturun ve istemciyi durdurmanız ve yeniden başlatmanız sonrasında bile API içinde nasıl kalıcı olduğuna dikkat edin.

## <a name="edit-your-user-flows"></a>Kullanıcı akışlarınızı düzenleyin

Azure AD B2C kullanarak API sağladıktan sonra oturum-içinde açma/kaydolma kullanıcı akışı ile denemeler yapın ve etkileri görüntüleyebilir (veya eksiklikleri) API. Kullanıcı akışları uygulama talepleri işlemek ve web API'SİNDE kullanılabilen kullanıcı bilgilerini değiştirebilirsiniz. Eklediğiniz tüm talepler bu makalede daha önce açıklandığı gibi `ClaimsPrincipal` nesnesindeki .NET MVC web API'nizde kullanılabilir olacaktır.
