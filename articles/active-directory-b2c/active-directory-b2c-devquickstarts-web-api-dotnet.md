---
title: Azure Active Directory B2C'in bir .NET web uygulamasından bir .NET web API'si çağırma | Microsoft Docs
description: Bir .NET Web uygulaması derleme ve bir web çağırmak nasıl Azure Active Directory B2C ve OAuth 2.0 erişim belirteçleri kullanarak API.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/17/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7296954a17b21183eb8be2744b42289522cf7f57
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012505"
---
# <a name="call-a-net-web-api-from-a-net-web-app-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'in bir .NET web uygulamasından bir .NET web API'si çağırma

Azure AD B2C'yi kullanarak web uygulamalarınız ve web API'leri için güçlü kimlik yönetimi özellikleri ekleyebilirsiniz. Bu makalede, erişim belirteçleri ve bir .NET yapma çağrıları bir "Yapılacaklar listesi".NET web uygulamasından web API'si isteği anlatılmaktadır.

Bu makalede, oturum açma, kaydolma nasıl uygulanacağını ve profil yönetimi ile Azure AD B2C kapsamaz. Kullanıcının zaten kimliği doğrulandıktan sonra web API'leri çağırmaya odaklanır. Henüz yapmadıysanız, şunları yapmalısınız:

* Kullanmaya başlama bir [.NET web uygulaması](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Kullanmaya başlama bir [.NET web API'si](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Önkoşul

Web çağıran bir web uygulamasının API'sini gerekir:

1. [Bir Azure AD B2C kiracısı oluşturmayı](active-directory-b2c-get-started.md).
2. [Web kaydetme API](active-directory-b2c-app-registration.md).
3. [Bir web uygulaması kaydetme](active-directory-b2c-app-registration.md).
4. [İlkeleri ayarlama](active-directory-b2c-reference-policies.md).
5. [Web uygulaması web kullanma izni vermek API](active-directory-b2c-access-tokens.md).

> [!IMPORTANT]
> İstemci uygulaması ve web API’si aynı Azure AD B2C dizinini kullanmalıdır.
>

## <a name="download-the-code"></a>Kodu indirme

Bu öğretici için kod [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi)'da korunur. Örneği kopyalamak için şu komutu çalıştırın:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Örnek kodu indirdikten sonra başlamak için Visual Studio .sln dosyasını açın. Çözüm dosyası iki proje içerir: `TaskWebApp` ve `TaskService`. `TaskWebApp` kullanıcı ile etkileşime giren bir MVC web uygulamasıdır. `TaskService`, uygulamanın, her kullanıcının yapılacaklar listesini depolayan arka uç web API'sidir. Bu makalede derlenmesini kapsamaz `TaskWebApp` web uygulaması veya `TaskService` web API'si. Azure AD B2C kullanarak .NET web uygulaması oluşturmayı öğrenmek için bkz. bizim [.NET web uygulaması Öğreticisi](active-directory-b2c-devquickstarts-web-dotnet-susi.md). .NET web API'si Azure AD B2C kullanılarak güvende tutulan oluşturmayı öğrenmek için bkz. bizim [.NET web API Öğreticisi](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>Azure AD B2C yapılandırmasını güncelleştirme

Örneğimiz, tanıtım kiracımızın ilkelerini ve istemci kimliğini kullanacak şekilde yapılandırılmıştır. Kendi kiracınızı kullanmak istiyorsanız:

1. `TaskService` projesinde `web.config` öğesini açın ve şu değerleri değiştirin:

    * kiracı adınızla `ida:Tenant`
    * `ida:ClientId` web API uygulamanızın kimliği ile
    * "Kaydolma veya Oturum açma" ilkenizin adıyla `ida:SignUpSignInPolicyId`

2. `TaskWebApp` projesinde `web.config` öğesini açın ve şu değerleri değiştirin:

    * kiracı adınızla `ida:Tenant`
    * Web uygulamanızın kimliği ile `ida:ClientId`
    * Web uygulamanızın gizli anahtarı ile `ida:ClientSecret`
    * "Kaydolma veya Oturum açma" ilkenizin adıyla `ida:SignUpSignInPolicyId`
    * "Profil Düzenleme" ilkenizin adıyla `ida:EditProfilePolicyId`
    * "Parola Sıfırlama" ilkenizin adıyla `ida:ResetPasswordPolicyId`



## <a name="requesting-and-saving-an-access-token"></a>İsteme ve bir erişim belirteci kaydediliyor

### <a name="specify-the-permissions"></a>İzinleri belirtme

Web API'sine çağrı yapmak için (oturumu-kaydolma/oturum açma ilkenizin kullanarak) kullanıcının kimliğini doğrulamak gerekir ve [bir erişim belirteci alma](active-directory-b2c-access-tokens.md) Azure AD B2C'den. Bir erişim belirteci almak için erişim belirteci vermek istediğiniz izinleri belirtin. İzinleri belirtilen `scope` için isteğinde bulunduğunda parametre `/authorize` uç noktası. Örneğin, kaynak uygulamanın uygulama kimliği URI'si "Okuma" iznine sahip bir erişim belirteci almak için `https://contoso.onmicrosoft.com/tasks`, kapsamı olacaktır `https://contoso.onmicrosoft.com/tasks/read`.

Örneğimizde kapsamını belirtmek için dosyayı açmak `App_Start\Startup.Auth.cs` ve tanımlama `Scope` OpenIdConnectAuthenticationOptions değişkeninde.

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify the scope by appending all of the scopes requested into one string (separated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-the-authorization-code-for-an-access-token"></a>Exchange için bir erişim belirteci yetkilendirme kodu

Bir kullanıcı kaydolma veya oturum açma deneyimi tamamlandıktan sonra uygulamanızı Azure AD B2C'den bir yetkilendirme kodu alırsınız. Ara yazılımlar OWIN Openıd Connect kodunu depolar, ancak bir erişim belirteci için exchange değil. Kullanabileceğiniz [MSAL Kitaplığı](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) değişikliği yapmak için. Bir yetkilendirme kodu alındığında Örneğimizde, biz bir bildirim geri çağırması Openıd Connect ara yazılımını yapılandırılmış. Geri çağırma içinde bir belirteç kodunu exchange ve belirteç önbelleğe kaydetmek için MSAL kullanın.

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract the code from the response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange the code for a token. Make sure to specify the necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-the-web-api"></a>Web API'si çağırma

Bu bölümde ele alınmaktadır sırasında alınan belirteç kullanma oturumu-kaydolma/oturum açma web API'sine erişmek için Azure AD B2C ile.

### <a name="retrieve-the-saved-token-in-the-controllers"></a>Denetleyicileri kaydedilmiş belirteci alma

`TasksController` Web API'si ile iletişim kurmak ve okuma, oluşturma ve görevleri silme API'sine HTTP istekleri göndermek için sorumludur. Azure AD B2C tarafından API korumalı olduğundan, ilk ve Yukarıdaki adımda kaydettiğiniz belirteci almak gerekir.

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL to retrieve the token from the cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve the token using the provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-the-web-api"></a>Web API'si görevlerini okuyun

Bir belirteç varsa, HTTP için iliştirebilirsiniz `GET` içinde istek `Authorization` güvenli bir şekilde çağırmak için üst bilgi `TaskService`:

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve the token with the specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token to the Authorization header and make the request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Oluşturma ve web API'si görevleri silme

Gönderdiğiniz aynı deseni takip `POST` ve `DELETE` istekleri Web API'sine, önbellekten erişim belirteci almak için MSAL kullanarak.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Son olarak, derleme ve her iki uygulama çalıştırın. Kaydolma ve oturum açın ve oturum açmış kullanıcı için Görevler oluşturun. Oturumu kapatın ve farklı bir kullanıcı olarak oturum açın. Bu kullanıcı için Görevler oluşturun. API, aldığı belirtecinden kullanıcının kimliğini ayıkladığı için görevlerin'ın kullanıcı başına depolanmasına API üzerinde nasıl olduğuna dikkat edin. Ayrıca, kapsamlar ile yürütmeyi deneyin. "Yazma" ve sonra bir görev eklemeyi deneyin izni kaldırın. Yalnızca kapsamını değiştirmek, her zaman aşımına oturum emin olun.

