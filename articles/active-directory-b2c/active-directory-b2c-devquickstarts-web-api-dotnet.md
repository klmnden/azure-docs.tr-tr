---
title: Azure Active Directory B2C | Microsoft Docs
description: ".NET Web uygulaması oluşturma ve bir web çağırmak nasıl Azure Active Directory B2C ve OAuth 2.0 erişim belirteçleri kullanarak API."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: d3888556-2647-4a42-b068-027f9374aa61
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 48452eb68f826d1c7aa61d5e5531f941ac1422b0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a>Azure AD B2C: bir .NET web uygulamasından .NET web API'si çağırma

Azure AD B2C kullanarak web uygulamaları ve web API'leri için güçlü kimlik yönetimi özellikleri ekleyebilirsiniz. Bu makalede erişim belirteçleri ve bir .NET yapma çağrıları .NET "Yapılacaklar listesi" web uygulamasından web API'si istemek nasıl anlatılmaktadır.

Bu makalede, oturum açma, kaydolma nasıl uygulanacağını ve profil Yönetimi Azure AD B2C ile kapsamaz. Kullanıcının kimliği zaten doğrulanmış sonra web API'leri çağırmaya odaklanır. Henüz yapmadıysanız, aşağıdakileri yapmalısınız:

* Kullanmaya başlama bir [.NET web uygulaması](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Kullanmaya başlama bir [.NET web API'si](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Önkoşul

Bir web çağıran bir web uygulaması oluşturmak için API, gerekir:

1. [Bir Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md).
2. [Bir web kayıt API](active-directory-b2c-app-registration.md#register-a-web-api).
3. [Bir web uygulaması kaydetmek](active-directory-b2c-app-registration.md#register-a-web-app).
4. [İlkeleri Ayarla](active-directory-b2c-reference-policies.md).
5. [Web uygulaması web kullanma izni vermek API](active-directory-b2c-access-tokens.md#publishing-permissions).

> [!IMPORTANT]
> İstemci uygulaması ve web API’si aynı Azure AD B2C dizinini kullanmalıdır.
>

## <a name="download-the-code"></a>Kodu indirme

Bu öğretici için kod [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi)'da korunur. Örneği kopyalamak için şu komutu çalıştırın:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Örnek kodu indirdikten sonra başlamak için Visual Studio .sln dosyasını açın. Çözüm dosyası iki proje içerir: `TaskWebApp` ve `TaskService`. `TaskWebApp`kullanıcı ile etkileşime giren bir MVC bir web uygulamasıdır. `TaskService`, uygulamanın, her kullanıcının yapılacaklar listesini depolayan arka uç web API'sidir. Bu makalede yapı kapsamaz `TaskWebApp` web uygulaması veya `TaskService` web API'si. Azure AD B2C kullanarak .NET web uygulaması oluşturmayı öğrenmek için bkz: bizim [.NET web uygulaması Öğreticisi](active-directory-b2c-devquickstarts-web-dotnet-susi.md). .NET web API'si Azure AD B2C kullanarak güvenliği oluşturma konusunda bilgi almak için bkz: bizim [.NET web API'si Öğreticisi](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-the-azure-ad-b2c-configuration"></a>Azure AD B2C yapılandırmasını güncelleştirme

Örneğimiz, tanıtım kiracımızın ilkelerini ve istemci kimliğini kullanacak şekilde yapılandırılmıştır. Kendi Kiracı kullanmak istiyorsanız:

1. `TaskService` projesinde `web.config` öğesini açın ve şu değerleri değiştirin:

    * kiracı adınızla `ida:Tenant`
    * `ida:ClientId`web API uygulama Kimliğinizle
    * "Kaydolma veya Oturum açma" ilkenizin adıyla `ida:SignUpSignInPolicyId`

2. `TaskWebApp` projesinde `web.config` öğesini açın ve şu değerleri değiştirin:

    * kiracı adınızla `ida:Tenant`
    * Web uygulamanızın kimliği ile `ida:ClientId`
    * Web uygulamanızın gizli anahtarı ile `ida:ClientSecret`
    * "Kaydolma veya Oturum açma" ilkenizin adıyla `ida:SignUpSignInPolicyId`
    * "Profil Düzenleme" ilkenizin adıyla `ida:EditProfilePolicyId`
    * "Parola Sıfırlama" ilkenizin adıyla `ida:ResetPasswordPolicyId`



## <a name="requesting-and-saving-an-access-token"></a>İsteme ve bir erişim belirteci kaydetme

### <a name="specify-the-permissions"></a>İzinleri belirtin

Web API çağrısı yapmak için (oturumu-up/oturum açma ilkeniz kullanarak) kullanıcı kimlik doğrulaması gerekir ve [bir erişim belirteci almak](active-directory-b2c-access-tokens.md) Azure AD B2C'ndan. Bir erişim belirteci almak için erişim belirteci vermek istediğiniz izinleri belirtin. İzinleri belirtilen `scope` isteği yaptığınızda parametresi `/authorize` uç noktası. Örneğin, uygulama kimliği URI'si kaynak uygulamanın "Okuma" izni olan bir erişim belirteci alması için `https://contoso.onmicrosoft.com/tasks`, kapsam olacaktır `https://contoso.onmicrosoft.com/tasks/read`.

Bizim örnek kapsamını belirtmek için dosyayı açmak `App_Start\Startup.Auth.cs` ve tanımlayın `Scope` OpenIdConnectAuthenticationOptions değişkeni.

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-the-authorization-code-for-an-access-token"></a>Bir erişim belirteci yetkilendirme kodu değişimi

Bir kullanıcı kaydı veya oturum açma deneyimi tamamlandıktan sonra uygulamanızı Azure AD B2C ' bir yetkilendirme kodu alırsınız. OWIN Openıd Connect Ara kodunu depolar, ancak bir erişim belirteci için exchange değil. Kullanabileceğiniz [MSAL Kitaplığı](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) değişikliği yapmak için. Bir yetkilendirme kodu alındığında Örneğimizde, biz bildirimi geri araması Openıd Connect Ara yapılandırılmış. Geri arama, bir belirteç kodunu exchange ve belirteç önbelleğe kaydetmek için MSAL kullanın.

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

Bu bölümde sırasında alınan simgesinin nasıl kullanılacağına ilişkin anlatılmaktadır oturumu-up/oturum açma web API'si erişmek için Azure AD B2C ile.

### <a name="retrieve-the-saved-token-in-the-controllers"></a>Denetleyicileri kaydedilmiş belirteçte alma

`TasksController` Web API ile iletişim kurmasını ve okuma, oluşturun ve görevleri silme API'sine HTTP istekleri göndermek için sorumludur. API Azure AD B2C tarafından korumalı olduğundan, ilk ve Yukarıdaki adımda kaydettiğiniz belirtecini almak gerekir.

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

### <a name="read-tasks-from-the-web-api"></a>Web API görevlerini okuma

Bir belirteç sahip olduğunuzda, HTTP iliştirebilirsiniz `GET` içindeki istek `Authorization` güvenli bir şekilde çağırmak için üstbilgi `TaskService`:

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

### <a name="create-and-delete-tasks-on-the-web-api"></a>Oluşturma ve web API görevleri silme

Gönderdiğiniz aynı düzeni uygular `POST` ve `DELETE` önbellekten erişim belirteci almak için MSAL kullanarak API, web istekleri.

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Son olarak, yapı ve her iki uygulamaları çalıştırın. Kaydolma ve oturum açın ve oturum açmış kullanıcı için Görevler oluşturun. Oturumu kapatın ve farklı bir kullanıcı olarak oturum açın. Bu kullanıcı için Görevler oluşturun. API, aldığı belirtecinden kullanıcının kimliğini ayıkladığı için görevlerin kullanıcı başına API üzerinde nasıl olduğuna dikkat edin. Ayrıca kapsamlarla yürütmeyi deneyin. "Yazma" ve bir görev eklemeyi deneyin iznini kaldırın. Yeni kapsam değiştirme her zaman aşımına uğrar imzalamak emin olun.

