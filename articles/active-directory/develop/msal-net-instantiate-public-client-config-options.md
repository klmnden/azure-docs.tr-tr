---
title: Bir ortak istemci uygulaması (.NET için Microsoft kimlik doğrulama kitaplığı) seçeneklerle örneği | Azure
description: Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak yapılandırma seçenekleri ile birlikte ortak istemci uygulaması örneğini oluşturma konusunda bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/30/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 125bbf9aed54fb00f039aeffddd5cc1aad3360a6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544395"
---
# <a name="instantiate-a-public-client-application-with-configuration-options-using-msalnet"></a>Genel istemci uygulaması yapılandırma seçenekleriyle, MSAL.NET kullanarak örneği

Bu makalede örneği oluşturmak nasıl bir [genel istemci uygulaması](msal-client-applications.md) Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak.  Uygulama ayarları dosyasında tanımlanan yapılandırma seçenekleri ile başlatılır.

Bir uygulamayı başlatmadan önce öncelikle gerekir [kaydetme](quickstart-register-app.md) , böylece uygulamanız Microsoft kimlik platformu ile tümleştirilebilir. Kayıt sonrasında (Bu, Azure portalında bulunabilir) aşağıdaki bilgilere ihtiyacınız:

- İstemci kimliği (bir GUİD'i temsil eden bir dize)
- Kimlik sağlayıcısı URL'si (örnek olarak adlandırılır) ve uygulamanız için oturum açma İzleyici. Bu iki parametre topluca yetkilisi olarak bilinir.
- Yalnızca kuruluşunuz için (aynı zamanda tek kiracılı uygulama adlı) bir satır iş kolu uygulaması yazıyorsanız Kiracı kimliği.
- Web apps için ve bazen genel istemci uygulamalarında (belirli bir aracı kullanmak uygulamanızı ihtiyacı olduğunda) için ayrıca redirectUri burada kimlik sağlayıcı arka başvururlar güvenlik belirteçlerini uygulamanızla ayarladığınız.


Bir .NET Core konsol uygulaması aşağıdaki olabilir *appsettings.json* yapılandırma dosyası:

```json
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

Aşağıdaki kod .NET yapılandırma çerçevesini kullanarak bu dosyayı okur:

```csharp
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
        config.MicrosoftGraphBaseEndpoint = Configuration.GetValue<string>("WebAPI:MicrosoftGraphBaseEndpoint");
        return config;
    }
}
```

Aşağıdaki kod, yapılandırma ayarları dosyasından kullanarak uygulamanızı oluşturur:

```csharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .Build();
```

