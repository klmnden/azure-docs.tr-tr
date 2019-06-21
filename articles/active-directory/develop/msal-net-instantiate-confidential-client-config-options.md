---
title: Bir gizli istemci uygulaması (.NET için Microsoft kimlik doğrulama kitaplığı) seçeneklerle örneği | Azure
description: Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak yapılandırma seçenekleri ile birlikte bir gizli bir istemci uygulaması örneğini oluşturma konusunda bilgi edinin.
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
ms.openlocfilehash: 7814ff6b7575fedc19e63676ce3353c2a62a62b4
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154431"
---
# <a name="instantiate-a-confidential-client-application-with-configuration-options-using-msalnet"></a>Yapılandırma seçenekleriyle, MSAL.NET kullanarak bir gizli bir istemci uygulaması örneği

Bu makalede örneği oluşturmak nasıl bir [gizli bir istemci uygulaması](msal-client-applications.md) Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak.  Uygulama ayarları dosyasında tanımlanan yapılandırma seçenekleri ile başlatılır.

Bir uygulamayı başlatmadan önce öncelikle gerekir [kaydetme](quickstart-register-app.md) , böylece uygulamanız Microsoft kimlik platformu ile tümleştirilebilir. Kayıt sonrasında (Bu, Azure portalında bulunabilir) aşağıdaki bilgilere ihtiyacınız:

- İstemci kimliği (bir GUİD'i temsil eden bir dize)
- Kimlik sağlayıcısı URL'si (örnek olarak adlandırılır) ve uygulamanız için oturum açma İzleyici. Bu iki parametre topluca yetkilisi olarak bilinir.
- Yalnızca kuruluşunuz için (aynı zamanda tek kiracılı uygulama adlı) bir satır iş kolu uygulaması yazıyorsanız Kiracı kimliği.
- Uygulama gizli anahtarı (istemci gizli dize) veya (X509Certificate2 türünde) sertifikası bir gizli bir istemci uygulaması ise.
- Web apps için ve bazen genel istemci uygulamalarında (belirli bir aracı kullanmak uygulamanızı ihtiyacı olduğunda) için ayrıca redirectUri burada kimlik sağlayıcı arka başvururlar güvenlik belirteçlerini uygulamanızla ayarladığınız.

## <a name="configure-the-application-from-the-config-file"></a>Yapılandırma dosyasından uygulamayı yapılandırma
Seçenekler, MSAL.NET özelliklerini adıyla eşleşmesi özelliklerini adını `AzureADOptions` içinde ASP.NET Core, bu nedenle gerekmez Birleştirici kodlar yazmak.

Bir ASP.NET Core Uygulama Yapılandırması bölümünde açıklanan bir *appsettings.json* dosyası:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "[Enter the domain of your tenant, e.g. contoso.onmicrosoft.com]",
    "TenantId": "[Enter 'common', or 'organizations' or the Tenant Id (Obtained from the Azure portal. Select 'Endpoints' from the 'App registrations' blade and use the GUID in any of the URLs), e.g. da41245a5-11b3-996c-00a8-4d99re19f292]",
    "ClientId": "[Enter the Client Id (Application ID obtained from the Azure portal), e.g. ba74781c2-53c2-442a-97c2-3d60re42f403]",
    "CallbackPath": "/signin-oidc",
    "SignedOutCallbackPath ": "/signout-callback-oidc",

    "ClientSecret": "[Copy the client secret added to the app from the Azure portal]"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

MSAL.NET v3.x başlayarak, gizli istemci uygulamanızı yapılandırma dosyasından yapılandırabilirsiniz.

Uygulamanızı oluşturmak ve yapılandırmak istediğiniz sınıfında bildirmenize gerek bir `ConfidentialClientApplicationOptions` nesne.  (Appconfig.json dosyası dahil) kaynaktan okunan yapılandırma kullanarak uygulama seçenekleri örneğine bağlamak `IConfigurationRoot.Bind()` yönteminden [Microsoft.Extensions.Configuration.Binder nuget paketini](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder):

```csharp
using Microsoft.Identity.Client;

private ConfidentialClientApplicationOptions _applicationOptions;
_applicationOptions = new ConfidentialClientApplicationOptions();
configuration.Bind("AzureAD", _applicationOptions);
```

Bu "AzureAD" bölümünü içeriğini sağlar *appsettings.json* için karşılık gelen özelliklere bağlı dosya `ConfidentialClientApplicationOptions` nesne.  Ardından, yapı bir `ConfidentialClientApplication` nesnesi:

```csharp
IConfidentialClientApplication app;
app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
        .Build();
```

## <a name="add-runtime-configuration"></a>Çalışma zamanı Yapılandırması Ekle
Bir gizli istemci uygulamasında, kullanıcı başına bir önbellek genellikle sahiptir. Bu nedenle kullanıcıyla ilişkili önbelleğe alma ve bunu kullanmak istediğiniz uygulama Oluşturucu bildirmeniz gerekir. Aynı şekilde, dinamik olarak hesaplanan bir yeniden yönlendirme URI'si olabilir. Bu durumda kod aşağıda verilmiştir:

```csharp
IConfidentialClientApplication app;
var request = httpContext.Request;
var currentUri = UriHelper.BuildAbsolute(request.Scheme, request.Host, request.PathBase, _azureAdOptions.CallbackPath ?? string.Empty);
app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(_applicationOptions)
       .WithRedirectUri(currentUri)
       .Build();
TokenCache userTokenCache = _tokenCacheProvider.SerializeCache(app.UserTokenCache,httpContext, claimsPrincipal);
```

