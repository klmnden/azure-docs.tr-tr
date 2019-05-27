---
title: Masaüstü uygulaması çağrıları API'leri (kod yapılandırması) - web Microsoft kimlik platformu
description: Bir masaüstü uygulaması oluşturmayı öğrenin çağrıları veritabanını web API'leri (uygulama kodu yapılandırma)
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
ms.openlocfilehash: bc0042d6392891e8282c563afea2212031a0f49a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121876"
---
# <a name="desktop-app-that-calls-web-apis---code-configuration"></a>Web API'leri - kod yapılandırma çağrıları masaüstü uygulaması

Uygulama oluşturduğunuza göre kodu uygulamanın koordinatlarıyla yapılandırma öğreneceksiniz.

## <a name="msal-libraries"></a>MSAL kitaplıkları

Masaüstü uygulamalarını bugün destekleyen tek MSAL MSAL.NET kitaplıktır

## <a name="public-client-application"></a>Genel istemci uygulaması

Bir kod açısından bakıldığında, Masaüstü uygulamaları genel istemci uygulamalardır ve neden ve yapı MSAL.NET işlemek `IPublicClientApplication`. Yeniden şeyler veya etkileşimli kimlik doğrulaması kullanıp biraz farklı olacaktır.

![IPublicClientApplication](media/scenarios/public-client-application.png)

### <a name="exclusively-by-code"></a>Özel kod tarafından

Aşağıdaki kod, bir ortak istemci uygulaması, oturum açmak, kullanıcıların bir iş ve Okul hesabı veya kişisel Microsoft hesabı ile Microsoft Azure genel bulutunda başlatır.

```CSharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

Yukarıda görüldüğü gibi etkileşimli kimlik doğrulaması kullanmak istiyorsanız, kullanmak istediğiniz `.WithRedirectUri` değiştiricisi:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithRedirectUri("https://login.microsoftonline.com/common/oauth2/nativeclient")
        .Build();
```

### <a name="using-configuration-files"></a>Yapılandırma dosyalarını kullanma

Aşağıdaki kod bir ortak istemci uygulamasından doldurulmuş program aracılığıyla açma veya bir yapılandırma dosyasından okuma bir yapılandırma nesnesi oluşturur

```CSharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
        .WithRedirectUri("https://login.microsoftonline.com/common/oauth2/nativeclient")
        .Build();
```

### <a name="more-elaborated-configuration"></a>Daha ayrıntılı yapılandırma

Değiştiriciler sayısını ekleyerek oluşturmakta uygulama özenli. Örneğin, uygulamanızın (burada ABD kamu) Ulusal bulut çok kiracılı bir uygulamada olmasını istiyorsanız, şunu yazabilirsiniz:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithRedirectUri("https://login.microsoftonline.com/common/oauth2/nativeclient")
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment,
                         AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

MSAL.NET de ADFS 2019 için'bir değiştirici içerir:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Son olarak, bir Azure AD B2C kiracısı için belirteçlerini almak istiyorsanız, aşağıdaki kod parçacığında gösterildiği gibi Kiracı belirtebilirsiniz:

```CSharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```

### <a name="learn-more"></a>Daha fazla bilgi edinin

MSAL.NET Masaüstü uygulamasını yapılandırma hakkında daha fazla bilgi için:

- Kullanılabilir tüm değiştiricilere listesinde `PublicClientApplicationBuilder`, başvuru belgelerine bakın [PublicClientApplicationBuilder](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods)
- İçinde kullanıma sunulan tüm seçeneklerinin açıklaması için `PublicClientApplicationOptions` bkz [PublicClientApplicationOptions](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.publicclientapplicationoptions), başvuru belgelerinde

## <a name="complete-example-with-configuration-options"></a>Yapılandırma seçenekleri ile birlikte tam bir örnek

Aşağıdakilerin bir .NET Core konsol uygulaması Imagine `appsettings.json` yapılandırma dosyası:

```JSon
{
  "Authentication": {
    "AzureCloudInstance": "AzurePublic",
    "AadAuthorityAudience": "AzureAdMultipleOrgs",
    "ClientId": "ebe2ab4d-12b3-4446-8480-5c3828d04c50"
  },

  "WebAPI": {
    "MicrosoftGraphBaseEndpoint": "https://graph.microsoft.com"
  }
}
```

.NET framework yapılandırma sağlanan kullanarak bu dosyayı okumak için çok az kod gerekir;

```CSharp
public class SampleConfiguration
{
 /// <summary>
 /// Authentication options
 /// </summary>
 public PublicClientApplicationOptions PublicClientApplicationOptions { get; set; }

 /// <summary>
 /// Base URL for Microsoft Graph (it varies depending on whether the application is ran
 /// in Microsoft Azure public clouds or national / sovereign clouds
 /// </summary>
 public string MicrosoftGraphBaseEndpoint { get; set; }

 /// <summary>
 /// Reads the configuration from a json file
 /// </summary>
 /// <param name="path">Path to the configuration json file</param>
 /// <returns>SampleConfiguration as read from the json file</returns>
 public static SampleConfiguration ReadFromJsonFile(string path)
 {
  // .NET configuration
  IConfigurationRoot Configuration;
  var builder = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile(path);
  Configuration = builder.Build();

  // Read the auth and graph endpoint config
  SampleConfiguration config = new SampleConfiguration()
  {
   PublicClientApplicationOptions = new PublicClientApplicationOptions()
  };
  Configuration.Bind("Authentication", config.PublicClientApplicationOptions);
  config.MicrosoftGraphBaseEndpoint =
  Configuration.GetValue<string>("WebAPI:MicrosoftGraphBaseEndpoint");
  return config;
 }
}
```

Artık, uygulamanızı oluşturmak için yalnızca aşağıdaki kod yazma gerekir:

```CSharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .WithRedirectUri("https://login.microsoftonline.com/common/oauth2/nativeclient")
           .Build();
```

ve çağırmadan önce `.Build()` yöntemi çağrılarıyla yapılandırmanızı kılabilir `.WithXXX` önceden görüldüğü gibi yöntemleri.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir masaüstü uygulaması için bir belirteç alınırken](scenario-desktop-acquire-token.md)
