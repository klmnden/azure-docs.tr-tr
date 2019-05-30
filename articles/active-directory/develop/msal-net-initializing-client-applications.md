---
title: İstemci uygulamaları (Microsoft kimlik doğrulama kitaplığı .NET için) başlatmak | Azure
description: Genel istemci ve gizli istemci uygulamaların Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak başlatma hakkında bilgi edinin.
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
ms.date: 04/12/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2f22ff41e380a16af2aa45df9a61eefbf293ff83
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544328"
---
# <a name="initialize-client-applications-using-msalnet"></a>İstemci uygulamaları, MSAL.NET kullanarak başlatma
Bu makalede, ortak istemci ve gizli istemci uygulamaların Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanarak başlatma açıklanmaktadır.  İstemci uygulama türleri ve uygulama yapılandırma seçenekleri hakkında daha fazla bilgi edinmek için [genel bakış](msal-client-applications.md).

MSAL.NET ile 3.x, bir uygulama oluşturmak için önerilen yöntem olduğu uygulama derleyicileri kullanarak: `PublicClientApplicationBuilder` ve `ConfidentialClientApplicationBuilder`. Bunlar, uygulama kodu veya bir yapılandırma dosyasından ya da bile her iki yaklaşım karıştırma yapılandırmak için güçlü bir mekanizma sağlar.

## <a name="prerequisites"></a>Önkoşullar
Bir uygulamayı başlatmadan önce öncelikle gerekir [kaydetmek](quickstart-register-app.md) böylece uygulamanız Microsoft kimlik platformu ile tümleştirilebilir.  Kayıt sonrasında (Bu, Azure portalında bulunabilir) aşağıdaki bilgilere ihtiyacınız:

- İstemci kimliği (bir GUİD'i temsil eden bir dize)
- Kimlik sağlayıcısı URL'si (örnek olarak adlandırılır) ve uygulamanız için oturum açma İzleyici. Bu iki parametre topluca yetkilisi olarak bilinir.
- Yalnızca kuruluşunuz için (aynı zamanda tek kiracılı uygulama adlı) bir satır iş kolu uygulaması yazıyorsanız Kiracı kimliği.
- Uygulama gizli anahtarı (istemci gizli dize) veya (X509Certificate2 türünde) sertifikası bir gizli bir istemci uygulaması ise.
- Web apps için ve bazen genel istemci uygulamalarında (belirli bir aracı kullanmak uygulamanızı ihtiyacı olduğunda) için ayrıca redirectUri burada kimlik sağlayıcı arka başvururlar güvenlik belirteçlerini uygulamanızla ayarladığınız.

## <a name="ways-to-initialize-applications"></a>Uygulamaları başlatma yolları
İstemci uygulamaları oluşturmak için birçok farklı yolu vardır.

### <a name="initializing-a-public-client-application-from-code"></a>Kod bir ortak istemci uygulamasından başlatma

Aşağıdaki kod imzalama, kullanıcılar kendi iş ve Okul hesapları veya kişisel Microsoft hesapları ile Microsoft Azure genel bulutta bir ortak istemci uygulaması başlatır.

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-code"></a>Kod bir gizli bir istemci uygulamasından başlatma

Aynı şekilde, aşağıdaki kodu gizli bir uygulama örneği oluşturur (bir Web uygulaması konumundaki `https://myapp.azurewebsites.net`) belirteçleri kullanıcıların kendi iş ve Okul hesapları veya kişisel Microsoft hesapları ile Microsoft Azure genel bulutta işleme. Uygulama, kimlik sağlayıcısı ile bir istemci gizli anahtarı paylaşarak tanımlanır:

```csharp
string redirectUri = "https://myapp.azurewebsites.net";
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithClientSecret(clientSecret)
    .WithRedirectUri(redirectUri )
    .Build();
```

Üretim ortamında bilirsiniz yerine gibi bir istemci gizli anahtarını kullanarak, Azure AD ile bir sertifika paylaşmak isteyebilirsiniz. Kodu daha sonra aşağıdaki gibi olabilir:

```csharp
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
    .WithCertificate(certificate)
    .WithRedirectUri(redirectUri )
    .Build();
```

### <a name="initializing-a-public-client-application-from-configuration-options"></a>Yapılandırma seçeneklerine bir genel istemci uygulamasından başlatma

Aşağıdaki kod bir ortak istemci uygulamasından doldurulmuş program aracılığıyla açma veya bir yapılandırma dosyasından okuma bir yapılandırma nesnesi oluşturur:

```csharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
    .Build();
```

### <a name="initializing-a-confidential-client-application-from-configuration-options"></a>Yapılandırma seçenekleri gizli bir istemci uygulamasından başlatma

Aynı tür desenlere gizli istemci uygulamaları için geçerlidir. Kullanarak diğer parametreleri de ekleyebilirsiniz `.WithXXX` değiştiriciler (burada bir sertifika).

```csharp
ConfidentialClientApplicationOptions options = GetOptions(); // your own method
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.CreateWithApplicationOptions(options)
    .WithCertificate(certificate)
    .Build();
```

## <a name="builder-modifiers"></a>Oluşturucu değiştiriciler

Uygulama Oluşturucular, bir dizi kullanarak kod parçacıkları olarak `.With` yöntemi değiştirici uygulanabilir (örneğin, `.WithCertificate` ve `.WithRedirectUri`). 

### <a name="modifiers-common-to-public-and-confidential-client-applications"></a>Genel ve özel istemci uygulamaları için ortak değiştiriciler

Genel istemci ya da gizli bir istemci uygulama Oluşturucusu ayarlayabilirsiniz değiştiricilerdir:

|Parametre | Açıklama|
|--------- | --------- |
|`.WithAuthority()` 7 geçersiz kılmaları | Uygulama varsayılan yetkilisi Azure Bulutu, İzleyici (Kiracı kimliği veya etki alanı adı), Kiracı seçmek veya doğrudan yetkili URI'si sağlama olanağı ile bir Azure AD yetkilisi olarak ayarlar.|
|`.WithAdfsAuthority(string)` | Uygulama varsayılan yetkilisi, bir ADFS yetkili olarak ayarlar.|
|`.WithB2CAuthority(string)` | Uygulama varsayılan yetkilisi, bir Azure AD B2C yetkili olarak ayarlar.|
|`.WithClientId(string)` | İstemci kimliği geçersiz kılar|
|`.WithComponent(string)` | (Telemetri nedenlerle) MSAL.NET kullanarak kitaplık adını ayarlar. |
|`.WithDebugLoggingCallback()` | Çağrılırsa, uygulama çağıracak `Debug.Write` yalnızca etkinleştirme hata ayıklama izlemeleri. Bkz: [günlüğü](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging) daha fazla bilgi için.|
|`.WithExtraQueryParameters(IDictionary<string,string> eqp)` | Tüm kimlik doğrulama isteğine gönderilen uygulama düzeyinde ek sorgu parametrelerini ayarlayın. Bu her belirteç edinme yöntemi düzeyinde geçersiz kılınabilir (aynı `.WithExtraQueryParameters pattern`).|
|`.WithHttpClientFactory(IMsalHttpClientFactory httpClientFactory)` | Gelişmiş bir HTTP Proxy'si için veya belirli bir HttpClient (Örneğin ASP.NET Core web apps/API'leri) kullanmak için MSAL zorlamak için yapılandırma gibi senaryoları etkinleştirir.|
|`.WithLogging()` | Çağrılırsa, uygulama hata ayıklama izlemeleri ile bir geri çağırma işlemini çağırır. Bkz: [günlüğü](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/logging) daha fazla bilgi için.|
|`.WithRedirectUri(string redirectUri)` | Varsayılan yeniden yönlendirme URI'si geçersiz kılar. Genel istemci uygulamaları söz konusu olduğunda, bu aracı içeren senaryolar için faydalı olacaktır.|
|`.WithTelemetry(TelemetryCallback telemetryCallback)` | Telemetri göndermek için kullanılan temsilci ayarlar.|
|`.WithTenantId(string tenantId)` | Kiracı kimliği veya Kiracı açıklaması geçersiz kılar.|

### <a name="modifiers-specific-to-xamarinios-applications"></a>Xamarin.iOS uygulamaları için özel değiştiriciler

Genel istemci uygulama oluşturucu üzerinde Xamarin.iOS ayarlayabilirsiniz değiştiricilerdir:

|Parametre | Açıklama|
|--------- | --------- |
|`.WithIosKeychainSecurityGroup()` | **Xamarin.iOS yalnızca**: İOS anahtar zinciri güvenlik grubu (cache kalıcılığı) ayarlar.|

### <a name="modifiers-specific-to-confidential-client-applications"></a>Gizli istemci uygulamalarına özel değiştiriciler

Gizli bir istemci uygulama oluşturucu üzerinde ayarladığınız değiştiricilerdir:

|Parametre | Açıklama|
|--------- | --------- |
|`.WithCertificate(X509Certificate2 certificate)` | Azure AD ile uygulamasını tanımlayan sertifika ayarlar.|
|`.WithClientSecret(string clientSecret)` | Azure AD ile uygulamasını tanımlayan istemci gizli anahtarı (uygulama parolası) ayarlar.|

Bu değiştiriciler karşılıklı olarak birbirini dışlar. MSAL, her ikisi de sağlarsanız, anlamlı bir özel durum oluşturur.

### <a name="example-of-usage-of-modifiers"></a>Değiştiricilere ait kullanım örneği

Uygulamanın yalnızca kuruluşunuz için bir iş kolu satır uygulama olduğunu varsayalım.  Sonra şunu yazabilirsiniz:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzurePublic, tenantId)
        .Build();
```

Burada ilginç hale programlama Ulusal Bulutlar için artık Basitleştirilmiş olur. Uygulamanızın Ulusal bulut çok kiracılı bir uygulamada olmasını istiyorsanız, şunu yazabilirsiniz, örneğin:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment, AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

Ayrıca AD FS için bir geçersiz kılma yoktur (ADFS 2019 şu anda desteklenmiyor):
```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Son olarak, bir Azure AD B2C geliştiriciyseniz, bu gibi kiracınız belirtebilirsiniz:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```
