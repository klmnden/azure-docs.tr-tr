---
title: "Oturum açma bir .NET MVC web API kullanarak Azure AD v2.0 uç ekleme | Microsoft Docs"
description: "Hem kişisel Microsoft Account belirteçleri kabul eder .NET MVC Web API'si oluşturma ve iş veya Okul hesapları."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: e77bc4e0-d0c9-4075-a3f6-769e2c810206
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 5d56e74c6344580760f55506d7d90dac3e90721d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="secure-an-mvc-web-api"></a>Bir MVC web API güvenliğini sağlama
Azure Active Directory ile v2.0 uç noktası, bir Web API kullanarak koruyabilirsiniz [OAuth 2.0](active-directory-v2-protocols.md) erişim belirteçleri, kullanıcıların hem kişisel Microsoft hesabı ile etkinleştirme ve iş veya Okul güvenli erişim sağlamak, Web API hesaplar.

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
>
>

ASP.NET web API'da, bunu .NET Framework 4. 5 ' dahil Microsoft'un OWIN ara yazılımı kullanarak gerçekleştirebilirsiniz.  OWIN oluşturmak ve bir kullanıcının yapılacaklar listesinden görevleri okumak istemcilerin bir "Yapılacaklar listesi" MVC Web API oluşturmak için buraya kullanacağız.  Web API gelen istekleri geçerli erişim belirteci içeren ve doğrulama korumalı bir rotaya geçmeyin istekleri reddedecek doğrular.  Bu örnek, Visual Studio 2015 kullanılarak oluşturuldu.

## <a name="download"></a>İndir
Bu öğretici için kod [GitHub'da](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet) korunur.  İzlemek için [uygulamanın çatısını bir .zip karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) veya çatıyı kopyalayın:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

İskelet uygulama için basit bir API tüm Demirbaş kod içerir, ancak kimlikle ilgili şey eksik. Takip istemiyorsanız, bunun yerine, kopyalayabilir veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Bir uygulamayı kaydetme
En yeni bir uygulama oluşturma [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya adımları izleyin: [ayrıntılı adımları](active-directory-v2-app-registration.md).  Emin olun:

* Aşağı kopyalama **uygulama kimliği** uygulamanıza atanan, bunu en kısa sürede ihtiyacınız vardır.

Bu visual studio çözümü "basit bir WPF uygulaması olan bir TodoListClient", de içerir.  TodoListClient nasıl bir kullanıcı oturum açtığında ve bir istemci isteklerini Web API'nizi nasıl verebilir göstermek için kullanılır.  Bu durumda, TodoListClient ve TodoListService aynı uygulama tarafından temsil edilir.  TodoListClient yapılandırmak için de yapmalısınız:

* Ekleme **mobil** uygulamanız için platform.

## <a name="install-owin"></a>OWIN yükleme
Bir uygulama kaydınız, gelen istekleri & belirteçleri doğrulamak için v2.0 uç noktası ile iletişim kurmak için uygulamanızı ayarlamanız gerekir.

* Başlamak için çözümü açın ve OWIN ara yazılımı NuGet paketleri Paket Yöneticisi konsolu kullanılarak TodoListService projeye ekleyin.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>OAuth kimlik doğrulamasını yapılandırma
* Adlı TodoListService projesine OWIN başlangıç sınıfı ekleyin `Startup.cs`.  Projeye sağ tıklatma--> **Ekle** --> **yeni öğe** "OWIN" arayın.  OWIN ara yazılımı, uygulamanız başlatıldığında `Configuration(…)` yöntemini çağırır.
* Sınıf bildirimi değiştirme `public partial class Startup` -zaten bu sınıfın parçası sizin için başka bir dosyaya uyguladık.  İçinde `Configuration(…)` yöntemi, bir web uygulamanız için kimlik doğrulaması ayarlamak için ConfgureAuth(...) çağrısı yapın.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* Dosyayı açmak `App_Start\Startup.Auth.cs` ve uygulamanıza `ConfigureAuth(…)` Web API v2.0 uç noktasından belirteçleri kabul ayarlar yöntemi.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* Kullanabileceğiniz artık `[Authorize]` denetleyicileri ve eylemleri OAuth 2.0 taşıyıcı kimlik doğrulaması ile korumak için öznitelikler.  İşaretleme `Controllers\TodoListController.cs` bir authorize etiketiyle sınıfı.  Bu sayfayı erişmeden önce oturum açmak için kullanıcının zorlar.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* Ne zaman bir yetkili çağıran başarıyla çağırır birini `TodoListController` API'leri, eylem çağıran hakkında bilgilere erişimi gerekebilir.  OWIN taşıyıcı belirteci içinde talep erişim sağlar `ClaimsPrincipal` nesnesi.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* Son olarak, açık `web.config` dosya TodoListService proje kök dizininde ve yapılandırma değerlerinizi girin `<appSettings>` bölümü.
  * `ida:Audience` Olan **uygulama kimliği** portalda girdiğiniz uygulamanın.

## <a name="configure-the-client-app"></a>İstemci uygulamasını yapılandırma
Eylem Yapılacaklar listesi hizmetinde görebilmek v2.0 uç noktasından belirteç almak ve hizmet çağrı yapmak için yapılacaklar listesi istemci yapılandırmanız gerekir.

* TodoListClient projeyi açın `App.config` ve yapılandırma değerlerinizi girin `<appSettings>` bölümü.
  * `ida:ClientId` Uygulama portaldan kopyaladığınız kimliği.

Son olarak, temiz, yapı ve her proje çalıştırın!  Artık hem kişisel Microsoft hesaplarını gelen belirteçleri ve iş veya Okul hesapları kabul eden bir .NET MVC Web API vardır.  Oturum TodoListClient ve web çağrı görevler kullanıcının yapılacaklar listesine eklemek için API.

(Yapılandırma değerleriniz olmadan) tamamlanan örnek, başvuru için [burada bir .zip sağlanan](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), veya Github'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Sonraki adımlar
Artık ek konu başlıklarına geçebilirsiniz.  Denemek isteyebilirsiniz:

[Bir Web uygulamasından Web API'si çağırma >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Ek kaynaklar için gözden geçirin:

* [V2.0 Geliştirici Kılavuzu >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "azure-active-directory" etiketi >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma
[Bu sayfayı](https://technet.microsoft.com/security/dd252948) ziyaret ederek ve Güvenlik Önerisi Uyarılarına abone olarak güvenlik olaylarının ne zaman ortaya çıkacağı hakkında bildirimleri almanızı öneririz.
