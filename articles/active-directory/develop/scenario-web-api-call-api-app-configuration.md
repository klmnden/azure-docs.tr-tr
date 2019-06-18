---
title: Web API'si, çağrıları Aşağı Akış web API'leri (uygulama kodu yapılandırma) - Microsoft kimlik platformu
description: Web aramaları API'ler (uygulama kodu yapılandırma) web API'si oluşturmayı öğrenin
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
ms.openlocfilehash: f62cf65e275d8a9b909bf60103ccbd84e91e4574
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65785066"
---
# <a name="web-api-that-calls-web-apis---code-configuration"></a>Web API çağrıları web API'leri - kod yapılandırma

Web API'nizi kaydettikten sonra uygulama kodunu yapılandırabilirsiniz.

Bir web API'sini korumak için kullanılan kod üzerinde web API'nizi Aşağı Akış web API'leri çağıran şekilde yapılandırmak için kod oluşturur. Daha fazla bilgi için bkz. [korumalı web API'si - uygulama yapılandırma](scenario-protected-web-api-app-configuration.md).

## <a name="code-subscribed-to-ontokenvalidated"></a>Kod için OnTokenValidated abone

API'nizi çağrıldığında, alınan taşıyıcı belirteç doğrulaması için abone olmanız herhangi korumalı web API'si için kod yapılandırma üzerinde:

```CSharp
/// <summary>
/// Protects the web API with Microsoft Identity Platform v2.0 (AAD v2.0)
/// This supposes that the configuration files have a section named "AzureAD"
/// </summary>
/// <param name="services">Service collection to which to add authentication</param>
/// <param name="configuration">Configuration</param>
/// <returns></returns>
public static IServiceCollection AddProtectedApiCallsWebApis(this IServiceCollection services,
                                                             IConfiguration configuration,
                                                             IEnumerable<string> scopes)
{
    services.AddTokenAcquisition();
    services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        // When an access token for our own web API is validated, we add it 
        // to MSAL.NET's cache so that it can be used from the controllers.
        options.Events = new JwtBearerEvents();

        options.Events.OnTokenValidated = async context =>
        {
            context.Success();

            // Adds the token to the cache, and also handles the incremental consent 
            // and claim challenges
            AddAccountToCacheFromJwt(context, scopes);
            await Task.FromResult(0);
        };
    });
    return services;
}
```

## <a name="on-behalf-of-flow"></a>On-behalf-of akışı

AddAccountToCacheFromJwt() yöntemi gerekir:

- MSAL gizli bir istemci uygulamanın örneği oluşturur.
- Çağrı `AcquireTokenOnBehalf` bir taşıyıcı belirteç aynı kullanıcı, ancak bir aşağı akış API'sini çağırmak API'mizi karşı Web API'si, istemci tarafından alındı taşıyıcı belirteç değişimi için.

### <a name="instantiate-a-confidential-client-application"></a>Gizli bir istemci uygulaması örneği

Korumalı web API'si için istemci kimlik bilgileri (istemci parolası veya sertifika) sağlar. böylece bu akışı yalnızca gizli istemci akışı kullanılabilir [ConfidentialClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder) aracılığıyla `WithClientSecret` veya `WithCertificate`yöntemleri, sırasıyla.

![image](https://user-images.githubusercontent.com/13203188/55967244-3d8e1d00-5c7a-11e9-8285-a54b05597ec9.png)

```CSharp
IConfidentialClientApplication app;

#if !VariationWithCertificateCredentials
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .Build();
#else
// Building the client credentials from a certificate
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .Build();
#endif
```

### <a name="how-to-call-on-behalf-of"></a>On-behalf-of çağırma

On-behalf-of (OBO) çağrı çağrılarak gerçekleştirilir [AcquireTokenOnBehalf](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenonbehalfofparameterbuilder) metodunda `IConfidentialClientApplication` arabirimi.

`ClientAssertion` Kendi istemcilerden gelen web API'si tarafından alınan taşıyıcı belirteç oluşturulur. Vardır [iki Oluşturucu](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientcredential.-ctor?view=azure-dotnet), bir JWT taşıyıcı belirteci alır ve kullanıcı onayı herhangi bir türden alır (başka bir tür güvenlik belirteci, hangi türü adlı ek bir parametre içinde belirtilen ardından `assertionType`).

![image](https://user-images.githubusercontent.com/13203188/37082180-afc4b708-21e3-11e8-8af8-a6dcbd2dfba8.png)

Uygulamada, OBO akışı genellikle bir aşağı akış API için bir belirteç almak ve diğer bölümlerini bir web API'si daha sonra üzerinde çağırabilirsiniz MSAL.NET kullanıcı belirteci önbelleğe depolamak için kullanılan [geçersiz kılmalar](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase.acquiretokensilent?view=azure-dotnet) , ``AcquireTokenOnSilent`` aşağı akış API'leri çağırmak için. Gerekirse bu belirteçleri yenileme etkisi vardır.

```CSharp
private void AddAccountToCacheFromJwt(IEnumerable<string> scopes, JwtSecurityToken jwtToken, ClaimsPrincipal principal, HttpContext httpContext)
{
 try
 {
  UserAssertion userAssertion;
  IEnumerable<string> requestedScopes;
  if (jwtToken != null)
  {
   userAssertion = new UserAssertion(jwtToken.RawData, "urn:ietf:params:oauth:grant-type:jwt-bearer");
   requestedScopes = scopes ?? jwtToken.Audiences.Select(a => $"{a}/.default");
  }
  else
  {
   throw new ArgumentOutOfRangeException("tokenValidationContext.SecurityToken should be a JWT Token");
  }

  // Create the application
  var application = BuildConfidentialClientApplication(httpContext, principal);

  // .Result to make sure that the cache is filled-in before the controller tries to get access tokens
  var result = application.AcquireTokenOnBehalfOf(requestedScopes.Except(scopesRequestedByMsalNet),
                                                  userAssertion)
                                        .ExecuteAsync()
                                        .GetAwaiter().GetResult();
 }
 catch (MsalException ex)
 {
  Debug.WriteLine(ex.Message);
  throw;
 }
}
```

## <a name="protocol"></a>Protocol

On-behalf-of protokolü hakkında daha fazla bilgi için bkz. [Microsoft kimlik platformu ve OAuth 2.0 On-Behalf-Of akış](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama için bir belirteç alınırken](scenario-web-api-call-api-acquire-token.md)
