---
title: Web uygulaması çağrıları API'leri (kod yapılandırması) - web Microsoft kimlik platformu
description: Bir Web uygulaması oluşturmayı öğrenin çağrıları veritabanını web API'leri (uygulama kodu yapılandırma)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95a5e1ed89b6330a0b6a49cb20d8bf0ef3587d48
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074748"
---
# <a name="web-app-that-calls-web-apis---code-configuration"></a>Web uygulaması web API'leri - kod yapılandırma çağrıları

Görüldüğü [Web uygulaması oturum açtığında kullanıcı senaryosu](scenario-web-app-sign-user-overview.md)düşünüldüğünde, kullanıcı oturum açma Kimliğine temsilci (OIDC) ara yazılım bağlayın, kanca artırmayı OIDC işlemde istiyor. Bunu yapmanın bir yolu, bağlı olarak farklıdır, kullanılan (burada ASP.NET ve ASP.NET Core), ancak ara yazılım OIDC olaylarına abone sonunda. İlke olmasıdır:

- ASP.NET veya ASP.NET bildirelim çekirdek isteği bir yetkilendirme kodu. Yaparak bu ASP.NET/ASP.NET çekirdek işlem, kullanıcının oturum açın ve kullanıcının onaylaması izin ver
- Web uygulaması tarafından yetkilendirme kodu alımını abone olmak.
- Kimlik doğrulama kodunu aldığınızda kodu ve sonuçta elde edilen erişim belirteçleri almak ve belirteçleri deposunda belirteç önbelleği yenilemek için MSAL kitaplıkları'nı kullanacaksınız. Burada, önbellek diğer belirteçlerini sessiz bir şekilde almak için uygulamanın diğer kısımlarını kullanılabilir.

## <a name="libraries-supporting-web-app-scenarios"></a>Web uygulaması senaryoları destek kitaplıkları

Yetkilendirme kod akışı, Web uygulamaları için destek kitaplıkları şunlardır:

| MSAL kitaplığı | Açıklama |
|--------------|-------------|
| ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Desteklenen platformlar olan .NET Framework ve .NET Core platformlar (UWP değil, Xamarin.iOS ve Xamarin.Android bu platformları olarak ortak istemci uygulamaları oluşturmak için kullanılır) |
| ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Genel önizlemeye sunuldu - sürmekte olan geliştirme |
| ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Genel önizlemeye sunuldu - sürmekte olan geliştirme |

## <a name="aspnet-core-configuration"></a>ASP.NET Core yapılandırma

ASP.NET Core, olacaklar `Startup.cs` dosya. Abone olmak istediğiniz `OnAuthorizationCodeReceived` kimliği açık olay bağlanmak ve bu olaydan MSAL çağırın. NET'in yöntem `AcquireTokenFromAuthorizationCode` belirteç önbelleği ve istenen kapsamlar için erişim belirteci süre sonu yaklaştığında olduğunda erişim belirtecini yenilemek için ya da aynı kullanıcı adına bir belirteç almak için kullanılacak bir yenileme belirteci depolamak etkisi vardır , ancak farklı bir kaynak için.

Aşağıdaki kod açıklamaları, MSAL.NET ve ASP.NET Core weaving beceri gerektiren bazı yönlerini anlamanıza yardımcı olur

```CSharp
  services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
  {
   // Response type. We ask ASP.NET to request an Auth Code, and an IDToken
   options.ResponseType = OpenIdConnectResponseType.CodeIdToken;

   // This "offline_access" scope is needed to get a refresh token when users sign in with
   // their Microsoft personal accounts
   // (it's required by MSAL.NET and automatically provided by Azure AD when users
   // sign in with work or school accounts, but not with their Microsoft personal accounts)
   options.Scope.Add(OidcConstants.ScopeOfflineAccess);
   options.Scope.Add("user.read"); // for instance

   // Handling the auth redemption by MSAL.NET so that a token is available in the token cache
   // where it will be usable from Controllers later (through the TokenAcquisition service)
   var handler = options.Events.OnAuthorizationCodeReceived;
   options.Events.OnAuthorizationCodeReceived = async context =>
   {
    // As AcquireTokenByAuthorizationCode is asynchronous we want to tell ASP.NET core
    // that we are handing the code even if it's not done yet, so that it does 
    // not concurrently call the Token endpoint.
    context.HandleCodeRedemption();

    // Call MSAL.NET AcquireTokenByAuthorizationCode
    var application = BuildConfidentialClientApplication(context.HttpContext,
                                                         context.Principal);
    var result = await application.AcquireTokenByAuthorizationCode(scopes.Except(scopesRequestedByMsalNet), context.ProtocolMessage.Code)
                                  .ExecuteAsync();

    // Do not share the access token with ASP.NET Core otherwise ASP.NET will cache it
    // and will not send the OAuth 2.0 request in case a further call to
    // AcquireTokenByAuthorizationCodeAsync in the future for incremental consent 
    // (getting a code requesting more scopes)
    // Share the ID Token so that the identity of the user is known in the application (in 
    // HttpContext.User)
    context.HandleCodeRedemption(null, result.IdToken);

    // Call the previous handler if any
    await handler(context);
   };
```

ASP.NET Core, gizli bir istemci uygulaması oluşturma HttpContext bilgileri kullanır. Web uygulaması ve oturum açmış kullanıcı bu HttpContext URL hakkında bilir (içinde bir `ClaimsPrincipal`). Ayrıca bir "AzureAD" bölümü vardır ve bağlı ASP.NET Core yapılandırma kullanır `_applicationOptions` veri yapısı. Son olarak uygulama belirteci önbellekler tutması gerekir.

```CSharp
/// <summary>
/// Creates an MSAL Confidential client application
/// </summary>
/// <param name="httpContext">HttpContext associated with the OIDC response</param>
/// <param name="claimsPrincipal">Identity for the signed-in user</param>
/// <returns></returns>
private IConfidentialClientApplication BuildConfidentialClientApplication(HttpContext httpContext, ClaimsPrincipal claimsPrincipal)
{
 var request = httpContext.Request;

 // Find the URI of the application)
 string currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, azureAdOptions.CallbackPath ?? string.Empty);

 // Updates the authority from the instance (including national clouds) and the tenant
 string authority = $"{azureAdOptions.Instance}{azureAdOptions.TenantId}/";

 // Instantiates the application based on the application options (including the client secret)
 var app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
               .WithRedirectUri(currentUri)
               .WithAuthority(authority)
               .Build();

 // Initialize token cache providers. In the case of Web applications, there must be one
 // token cache per user (here the key of the token cache is in the claimsPrincipal which
 // contains the identity of the signed-in user)
 if (this.UserTokenCacheProvider != null)
 {
  this.UserTokenCacheProvider.Initialize(app.UserTokenCache, httpContext, claimsPrincipal);
 }
 if (this.AppTokenCacheProvider != null)
 {
  this.AppTokenCacheProvider.Initialize(app.AppTokenCache, httpContext);
 }
 return app;
}
```

`AcquireTokenByAuthorizationCode` gerçekten ASP.NET tarafından istenen yetkilendirme kodu redeems ve MSAL.NET kullanıcı belirteci önbelleğe eklenen belirteçlerini alır. Buradan sonra oldukları ASP.NET Core denetleyicileri kullanılır.

## <a name="aspnet-configuration"></a>ASP.NET yapılandırması

ASP.NET şeyler işleme benzer dışında Openıdconnect ve aboneliğe yapılandırmasını `OnAuthorizationCodeReceived` olay gerçekleştiğinde `App_Start\Startup.Auth.cs` dosya. Burada RedirectUri biraz daha az yapılandırma dosyasında belirtmeniz gerekecek dışında benzer kavramları sağlam bulabilirsiniz:

```CSharp
private void ConfigureAuth(IAppBuilder app)
{
 app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

 app.UseCookieAuthentication(new CookieAuthenticationOptions { });

 app.UseOpenIdConnectAuthentication(
 new OpenIdConnectAuthenticationOptions
 {
  Authority = Globals.Authority,
  ClientId = Globals.ClientId,
  RedirectUri = Globals.RedirectUri,
  PostLogoutRedirectUri = Globals.RedirectUri,
  Scope = Globals.BasicSignInScopes, // a basic set of permissions for user sign in & profile access
  TokenValidationParameters = new TokenValidationParameters
  {
  // We'll inject our own issuer validation logic below.
  ValidateIssuer = false,
  NameClaimType = "name",
  },
  Notifications = new OpenIdConnectAuthenticationNotifications()
  {
   AuthorizationCodeReceived = OnAuthorizationCodeReceived,
  }
 });
}
```

```CSharp
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification context)
{
 // Upon successful sign in, get & cache a token using MSAL
 string userId = context.AuthenticationTicket.Identity.FindFirst(ClaimTypes.NameIdentifier).Value;
 IConfidentialClientApplication clientapp;
 clientApp = ConfidentialClientApplicationBuilder.Create(Globals.ClientId)
                  .WithClientSecret(Globals.ClientSecret)
                  .WithRedirectUri(Globals.RedirectUri)
                  .WithAuthority(new Uri(Globals.Authority))
                  .Build();

  EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);

 AuthenticationResult result = await clientapp.AcquireTokenByAuthorizationCode(scopes, context.Code)
                                              .ExecuteAsync();
}
```

### <a name="msalnet-token-cache-for-a-aspnet-core-web-app"></a>(Temel) ASP.NET Web uygulaması için MSAL.NET belirteç önbelleği

Web apps (veya web API sağlasa da, olgu olarak), belirteç önbelleği Masaüstü uygulamaları belirteç önbelleği uygulamalarının farklı uygulamasıdır (genellikle olduğu [dosya tabanlı](scenario-desktop-acquire-token.md#file-based-token-cache). ASP.NET/ASP.NET çekirdek oturumu veya bir Redis önbelleği veya bir veritabanı veya hatta Azure blogu depolama kullanabilirsiniz. Kod parçacığı bunun üzerinde nesnesidir `EnablePersistence(HttpContext, clientApp.UserTokenCache, clientApp.AppTokenCache);` bağlanan Azure önbellek hizmeti yöntemi araması. Ayrıntı ne İşte senaryo bu kılavuzun kapsamı dışındadır, ancak aşağıda bağlantıları verilen'olmuyor.

> [!IMPORTANT]
> Hayata geçirmek için çok önemli bir şey, web uygulamaları ve web API'leri için olması gereken bir belirteç önbelleği kullanıcı (başına hesabı) olabilir. Her hesap için belirteç önbelleği serileştirmek gerekir.

Belirteç önbellekler için Web apps ve web API'leri nasıl kullanılacağını örnekleri kullanılabilir [ASP.NET Core Web uygulaması Öğreticisi](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) aşamada [2-2 belirteç önbelleği](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-2-TokenCache). Uygulamaları aşağıdaki klasöre bakmak için [TokenCacheProviders](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web/TokenCacheProviders) içinde [microsoft-kimlik doğrulama-extensions-için-dotnet](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet) kitaplığı (içinde [ Microsoft.Identity.Client.Extensions.Web](https://github.com/AzureAD/microsoft-authentication-extensions-for-dotnet/tree/master/src/Microsoft.Identity.Client.Extensions.Web) klasör.

## <a name="next-steps"></a>Sonraki adımlar

Bu noktada, kullanıcı oturum açtığında bir belirteç belirteci önbellekte depolanır. Web uygulamasının diğer bölümlerinde ardından nasıl kullanıldığını görelim.

> [!div class="nextstepaction"]
> [Web uygulamasına oturum açın](scenario-web-app-call-api-sign-in.md)
