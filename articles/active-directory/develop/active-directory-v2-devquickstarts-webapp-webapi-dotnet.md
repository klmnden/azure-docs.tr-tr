---
title: "Azure AD v2.0 .NET web uygulaması arama API Başlarken | Microsoft Docs"
description: "Kişisel Microsoft hesapları ve iş kullanarak bu çağrıları web .NET MVC Web uygulaması oluşturma hizmetleri veya Okul hesapları için oturum açma."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f59c9e2c523db319565c1cca13eb85f809b2bdd6
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="calling-a-web-api-from-a-net-web-app"></a>Bir .NET web uygulamasından web API'si çağırma
Hızlı bir şekilde v2.0 uç noktası ile kimlik doğrulaması web uygulamalarınıza eklemek ve hem kişisel Microsoft hesapları için destek ve iş veya Okul hesaplarıyla API web.  Burada, biz Openıd Connect, Microsoft'un OWIN ara yazılımı için biraz Yardım ile kullanarak kullanıcılar oturum açtığında bir MVC web uygulaması oluşturma.  Web uygulaması web API'si OAuth 2.0 tarafından sağlayan güvenli oluşturma için OAuth 2.0 erişim belirteçleri almak, okuma ve belirli kullanıcının "Yapılacaklar listesi üzerinde" silin.

Bu öğretici edinmeli ve erişim belirteçleri tam olarak tanımlanan bir web uygulamasında kullanmak için öncelikle MSAL üzerinden odaklanacaktır [burada](active-directory-v2-flows.md#web-apps).  Önkoşul olarak ilk bilgi edinmek isteyebilirsiniz nasıl [temel oturum açma için bir web uygulaması eklemek](active-directory-v2-devquickstarts-dotnet-web.md) veya nasıl [düzgün bir web API güvenliğini](active-directory-v2-devquickstarts-dotnet-api.md).

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

## <a name="download-sample-code"></a>Örnek kodu indirin
Bu öğretici için kod [GitHub'da](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet) korunur.  İzlemek için [uygulamanın çatısını bir .zip karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) veya çatıyı kopyalayın:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Alternatif olarak, [tamamlanmış uygulamayı .zip olarak karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) veya tamamlanan uygulama:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Bir uygulamayı kaydetme
En yeni bir uygulama oluşturma [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya adımları izleyin: [ayrıntılı adımları](active-directory-v2-app-registration.md).  Emin olun:

* Aşağı kopyalama **uygulama kimliği** uygulamanıza atanan, bunu en kısa sürede ihtiyacınız vardır.
* Oluşturma bir **uygulama gizli anahtarı** , **parola** türü ve değeri için daha sonra kopyalayın
* Ekleme **Web** uygulamanız için platform.
* Doğru girin **yeniden yönlendirme URI'si**. Bu öğretici için varsayılan burada kimlik doğrulama yanıtları yönlendirileceğini - Azure AD ile yeniden yönlendirme URI'si gösterir `https://localhost:44326/`.

## <a name="install-owin"></a>OWIN yükleme
OWIN ara yazılımı NuGet paketleri ekleme `TodoList-WebApp` Paket Yöneticisi konsolu kullanılarak proje.  OWIN ara yazılımı, oturum açma ve oturum kapatma isteklerini yürütmek, kullanıcının oturumunu yönetmek ve başka şeyler arasında kullanıcı hakkında bilgi almak için kullanılır.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Kullanıcı oturum aç
Kullanılacak OWIN Ara yazılımının şimdi yapılandırmak [Openıd Connect kimlik doğrulama protokolü](active-directory-v2-protocols.md).  

* Açık `web.config` kökündeki dosyasında `TodoList-WebApp` proje ve uygulamanızın yapılandırma değerlerini girin `<appSettings>` bölümü.
  * `ida:ClientId` Olan **uygulama kimliği** kayıt portalında uygulamanıza atanan.
  * `ida:ClientSecret` Olan **uygulama gizli anahtarı** kayıt Portalı'nda oluşturulan.
  * `ida:RedirectUri` Olan **yeniden yönlendirme URI'si** portalda girdiğiniz.
* Açık `web.config` kökündeki dosyasında `TodoList-Service` proje ve değiştirme `ida:Audience` aynı **uygulama kimliği** olarak yukarıdaki.
* Dosyayı açmak `App_Start\Startup.Auth.cs` ve ekleme `using` deyimleri üstten kitaplıkları için.
* Aynı dosyada uygulamak `ConfigureAuth(...)` yöntemi.  Sağladığınız içinde parametreleri `OpenIDConnectAuthenticationOptions` uygulamanız için Azure AD ile iletişim kurmak için koordinatları olarak hizmet verecektir.

```csharp
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
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Erişim belirteci almak için MSAL kullanın
İçinde `AuthorizationCodeReceived` bildirim, biz kullanmak istediğiniz [Openıd Connect ile OAuth 2.0 art arda](active-directory-v2-protocols.md) Yapılacaklar listesi hizmeti için bir erişim belirteci authorization_code kullanmak için.  MSAL bu işlemi sizin için kolaylaştırır:

* İlk olarak, MSAL önizleme sürümünü yükleyin:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* Ve başka bir tane eklemek `using` ifadesine `App_Start\Startup.Auth.cs` MSAL dosyası.
* Şimdi yeni bir yöntem ekleyin `OnAuthorizationCodeReceived` olay işleyicisi.  Bu işleyici MSAL Yapılacaklar listesi API için bir erişim belirteci almak için kullanır ve belirteç MSAL'ın belirteci önbelleğinde daha sonrası için depolar:

```csharp
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* Web uygulamalarında MSAL belirteçleri depolamak için kullanılan genişletilebilir bir belirteç önbelleğe sahiptir.  Bu örnek uygulayan `NaiveSessionCache` önbellek belirteçleri için http oturum depolama kullanır.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Web API'si çağırma
Şimdi 3. adımda aldığınız access_token gerçekten kullandığınız zamanı geldi.  Web uygulamanızın açmak `Controllers\TodoListController.cs` Yapılacaklar listesi API'sine tüm CRUD istek yaptığında dosya.

* MSAL yeniden buraya access_tokens MSAL önbellekten getirileceği kullanabilirsiniz.  İlk olarak, ekleyin bir `using` bu dosyaya MSAL bildirimi.
  
    `using Microsoft.Identity.Client;`
* İçinde `Index` eylemin, kullanım MSAL `AcquireTokenSilentAsync` Yapılacaklar listesi hizmetinden veri okumak için kullanılan bir access_token almak için yöntemi:

```csharp
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* Örnek HTTP GET isteği elde edilen belirteç sonra ekler `Authorization` Yapılacaklar listesi hizmetinin istek kimliğini doğrulamak için kullandığı üstbilgi.
* Yapılacaklar listesi hizmet döndürürse bir `401 Unauthorized` yanıtı access_tokens MSAL içinde geçersiz hale herhangi bir nedenden dolayı.  Bu durumda, MSAL önbellekten herhangi access_tokens bırakın ve kullanıcı yeniden hangi belirteç edinme akışı yeniden içinde oturum gerekebilir görüntülenmesi gerekir.

```csharp
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

* Benzer şekilde, MSAL herhangi bir nedenle bir access_token döndüremedi ise, kullanıcı yeniden oturum açmak için istemeniz gerekir.  Bu herhangi bir yakalama olarak kadar basittir `MSALException`:

```csharp
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

* Aynı tam `AcquireTokenSilentAsync` çağrıdır içinde implementd `Create` ve `Delete` eylemler.  Web uygulamalarında uygulamanızda ihtiyaç duyduğunuzda access_tokens almak için bu MSAL yöntemi kullanabilirsiniz.  MSAL alınırken, önbelleğe alma ve belirteçleri, yenileme ilgilenebilmek.

Son olarak, yapı ve uygulamanızı çalıştırın!  Microsoft Account veya bir Azure AD hesabının oturum açın ve kullanıcının kimliğini üst gezinti çubuğunda nasıl yansıtılır dikkat edin.  Ekleyin ve eylem API çağrılarında OAuth 2.0 güvenli görmek için kullanıcının Yapılacaklar listesinde bazı öğeleri silin.  Artık bir web uygulaması ve web API, hem kullanıcıların hem kişisel hem de iş/Okul hesaplarını ile doğrulanabilir endüstri standardı protokoller üzerine kullanılarak güvenlik altına sahipsiniz.

(Yapılandırma değerleriniz olmadan) tamamlanan örnek, başvuru için [burada sağlanan](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Sonraki Adımlar
Ek kaynaklar için gözden geçirin:

* [V2.0 Geliştirici Kılavuzu >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "azure-active-directory" etiketi >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma
[Bu sayfayı](https://technet.microsoft.com/security/dd252948) ziyaret ederek ve Güvenlik Önerisi Uyarılarına abone olarak güvenlik olaylarının ne zaman ortaya çıkacağı hakkında bildirimleri almanızı öneririz.

