---
title: (Kod yapılandırması) - kullanıcılar oturum açtığında web uygulaması Microsoft kimlik platformu
description: (Kod yapılandırması) kullanıcılar oturum açtığında bir web uygulaması oluşturmayı öğrenin
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
ms.openlocfilehash: b7484b627d3bc3f26fa01d4c38ee96047c70d007
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785474"
---
# <a name="web-app-that-signs-in-users---code-configuration"></a>Web uygulaması oturum açtığında kullanıcıların - kod yapılandırma

Oturum açtığında, kullanıcıların Web uygulamanız için kod yapılandırmayı öğrenin.

## <a name="libraries-used-to-protect-web-apps"></a>Web uygulamaları korumak için kullanılan kitaplıklar

<!-- This section can be in an include for Web App and Web APIs -->
Bir Web uygulaması (ve bir Web API'si) korumak için kullanılan kitaplıklar şunlardır:

| Platform | Kitaplık | Açıklama |
|----------|---------|-------------|
| ![.NET](media/sample-v2-code/logo_net.png) | [.NET kimlik modeli uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/wiki) | Doğrudan ASP.NET ve ASP.NET Core tarafından kullanılan .NET için Microsoft kimlik uzantıları DLL'ler hem .NET Framework ve .NET Core üzerinde çalışan bir dizi önerir. Bir ASP.NET/ASP.NET çekirdek Web uygulamasından belirteci doğrulaması kullanarak denetleyebilirsiniz **tokenvalidationparameters değerini** sınıfta (ISV bazı senaryolarda belirli) |

## <a name="aspnet-core-configuration"></a>ASP.NET Core yapılandırma

Bu makalede ve aşağıdaki kod parçacıkları ayıklanan [ASP.NET Core Web uygulaması artımlı Eğitmeni, bölüm 1](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-1-MyOrg). Bu öğretici için tam uygulama ayrıntılarını başvurmak isteyebilirsiniz.

### <a name="application-configuration-files"></a>Uygulama yapılandırma dosyaları

ASP.NET Core, bir Web uygulaması oturum açma kullanıcılarla Microsoft kimlik platformu üzerinden yapılandırılır `appsettings.json` dosya. Doldurmak için gereken ayarlar şunlardır:

- Bulut `Instance` uygulamanızın Ulusal bulutlarda çalışmasını istiyorsanız
- izleyiciye `tenantId`
- `clientId` Azure portaldan kopyaladığınız olarak uygulamanız için.

```JSon
{
  "AzureAd": {
    // Azure Cloud instance among:
    // "https://login.microsoftonline.com/" for Azure Public cloud.
    // "https://login.microsoftonline.us/" for Azure US government.
    // "https://login.microsoftonline.de/" for Azure AD Germany
    // "https://login.chinacloudapi.cn/" for Azure AD China operated by 21Vianet
    "Instance": "https://login.microsoftonline.com/",

    // Azure AD Audience among:
    // - the tenant Id as a a GUID obtained from the azure portal to sign-in users in your organization
    // - "organizations" to sign-in users in any work or school accounts
    // - "common" to sign-in users with any work and school account or Microsoft personal account
    // - "consumers" to sign-in users with Microsoft personal account only
    "TenantId": "[Enter the tenantId here]]",

    // Client Id (Application ID) obtained from the Azure portal
    "ClientId": "[Enter the Client Id]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc"
  }
}
```

ASP.NET Core URL'sini içeren başka bir dosya var. (`applicationUrl`) ve SSL bağlantı noktası (`sslPort`), uygulamanızın yanı sıra çeşitli profilleri.

```JSon
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:3110/",
      "sslPort": 44321
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "webApp": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:3110/"
    }
  }
}
```

Azure portalında, yanıt kaydetmek için gereken bir URI'leri **kimlik doğrulaması** uygulamanız için sayfa bu URL'leri eşleşmesi gerekir; diğer bir deyişle, yukarıdaki iki yapılandırma dosyaları için bunlar olur `https://localhost:44321/signin-oidc` applicationUrl olarak olan `http://localhost:3110` ancak `sslPort` belirtilen (44321), olduğundan ve `CallbackPath` olduğu `/signin-oidc` tanımlandığı gibi `appsettings.json`.
  
Aynı şekilde, URI oturumunuzu ayarlanır `https://localhost:44321/signout-callback-oidc`.

### <a name="initialization-code"></a>Başlatma kodu

ASP.NET Core Web uygulamaları (ve Web API'leri), uygulama başlatma yapılması kod bulunan `Startup.cs` dosyası ve Microsoft Identity Platformu (eski adıyla Azure AD) v2.0 kimlik doğrulama eklemek için aşağıdaki kodu eklemeniz gerekecektir. Kod açıklamaları açıklayıcı olmalıdır.

  > [!NOTE]
  > Projenizi Visual studio veya kullanarak varsayılan ASP.NET core web projesi ile başlatırsanız `dotnet new mvc` yöntemi `AddAzureAD` ilişkili paketleri otomatik olarak yüklendiği için varsayılan olarak kullanılabilir. Ancak sıfırdan bir projeyi derleme ve kullanmayı denemekte olduğunuz kod NuGet paketini eklemenizi öneririz **"Microsoft.AspNetCore.Authentication.AzureAD.UI"** yapmak için projenize `AddAzureAD` yöntemi kullanılabilir.
  
```CSharp
 services.AddAuthentication(AzureADDefaults.AuthenticationScheme)
         .AddAzureAD(options => configuration.Bind("AzureAd", options));

 services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
 {
  // The ASP.NET core templates are currently using Azure AD v1.0, and compute
  // the authority (as {Instance}/{TenantID}). We want to use the Microsoft Identity Platform v2.0 endpoint
  options.Authority = options.Authority + "/v2.0/";

  // If you want to restrict the users that can sign-in to specific organizations
  // Set the tenant value in the appsettings.json file to 'organizations', and add the
  // issuers you want to accept to options.TokenValidationParameters.ValidIssuers collection.
  // Otherwise validate the issuer
  options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ForAadInstance(options.Authority).ValidateAadIssuer;

  // Set the nameClaimType to be preferred_username.
  // This change is needed because certain token claims from Azure AD v1.0 endpoint
  // (on which the original .NET core template is based) are different in Azure AD v2.0 endpoint.
  // For more details see [ID Tokens](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
  // and [Access Tokens](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
  options.TokenValidationParameters.NameClaimType = "preferred_username";
  ...
```

## <a name="aspnet-configuration"></a>ASP.NET yapılandırması

ASP.NET'te, uygulama ile yapılandırılmış `Web.Config` dosyası

```XML
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
  https://go.microsoft.com/fwlink/?LinkId=301880
  -->
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:ClientId" value="[Enter your client ID, as obtained from the app registration portal]" />
    <add key="ida:ClientSecret" value="[Enter your client secret, as obtained from the app registration portal]" />
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}{1}" />
    <add key="ida:RedirectUri" value="https://localhost:44326/" />
    <add key="vs:EnableBrowserLink" value="false" />
  </appSettings>
```

ASP.NET Web uygulamasında kimlik doğrulamasını ilgili kod / Web API'leri bulunan `App_Start/Startup.Auth.cs` dosya.

```CSharp
 public void ConfigureAuth(IAppBuilder app)
 {
  app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

  app.UseCookieAuthentication(new CookieAuthenticationOptions());

  app.UseOpenIdConnectAuthentication(
    new OpenIdConnectAuthenticationOptions
    {
     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
     // The `Scope` describes the initial permissions that your app will need.
     //  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
     ClientId = clientId,
     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
     RedirectUri = redirectUri,
     Scope = "openid profile",
     PostLogoutRedirectUri = redirectUri,
    });
 }
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oturum açın ve oturum kapatma](scenario-web-app-sign-user-sign-in.md)
