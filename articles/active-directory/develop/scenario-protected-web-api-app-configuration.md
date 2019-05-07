---
title: Korumalı Web API'sini - uygulama kodu yapılandırma | Azure
description: Korumalı Web API'si oluşturma ve uygulama kodu yapılandırma hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: e206cb29338445e30a7462bcbaf0079236e75510
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074973"
---
# <a name="protected-web-api---code-configuration"></a>Korumalı web API'si - kod yapılandırma

Korumalı web API'niz için kod başarıyla yapılandırmak için korumalı API'leri kılan, taşıyıcı belirteç yapılandırmak gerekenler ve belirteç doğrulama anlamanız gerekir.

## <a name="what-makes-aspnetaspnet-core-apis-protected"></a>ASP.NET/ASP.NET çekirdek korumalı API'leri yapan nedir?

Web Apps, ASP.NET/ASP.NET çekirdek gibi web API'lerini "korumalı" denetleyici eylemlerini ön eki olduğundan `[Authorize]` özniteliği. Bu nedenle, yetkili bir kimlikle API çağrılırsa denetleyici eylemleri yalnızca çağrılabilir.

Aşağıdaki soruları göz önünde bulundurun:

- Web API'si, (yalnızca bir uygulama bir web API'nizi çağırabileceği) çağıran uygulama kimliğini nasıl bilir?
- Uygulamanın kullanıcı adına web API'si çağrılırsa, kullanıcının kimliği nedir?

## <a name="bearer-token"></a>Taşıyıcı belirteç

Uygulama kimliği ve kullanıcı hakkındaki bilgileri (web uygulaması bir arka plan programı uygulamasından-hizmet çağrıları kabul sürece), uygulama çağırırken üstbilgisinde ayarlanan taşıyıcı belirteci tutulur.

İşte bir C# MSAL.NET belirteciyle aldıktan sonra API'yi çağırıp istemci gösteren kod örneği.

```CSharp
var scopes = new[] {$"api://.../access_as_user}";
var result = await app.AcquireToken(scopes)
                      .ExecuteAsync();

httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call the web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
```

> [!IMPORTANT]
> Taşıyıcı belirteç, Microsoft kimlik platformu uç nokta için bir istemci uygulaması tarafından istendi **web API'si için**. Web API belirteci doğrulamak ve içerdiği talep Ara gereken tek bir uygulamadır. İstemci uygulamalarını belirteçlere talep hiçbir zaman bakmak (web API'si, gelecekte bu belirteci şifrelenmesini talepleri karar verebilirsiniz ve bunu çözebilir istemci uygulaması kesintiye uğratacağını erişim belirteçleri açın).

## <a name="jwtbearer-configuration"></a>JwtBearer yapılandırma

Bu bölüm, taşıyıcı belirteç yapılandırmak için ihtiyacınız olanlar kapsar.

### <a name="config-file"></a>Yapılandırma dosyası

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line of business app)
      Otherwise you can leave them set to common
      This can be:
      - A guid (Tenant ID = Directory ID)
      - 'common' (Any organization and personal accounts)
      - 'organizations' (any organization)
      - 'consumers' (Microsoft personal accounts)
    */
    "TenantId": "common"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="code-initialization"></a>Kod başlatma

Uygulama denetleyici eylem tutma üzerinde adlı ne zaman bir `[Authorize]` özniteliği ASP.NET/ASP.NET çekirdek çağıran isteğin yetkilendirme üst bilgisinde taşıyıcı belirteç bakar ve ardından JwtBearer için iletilen erişim belirteci ayıklar Ara yazılım .NET için Microsoft kimlik modeli uzantıları çağırır.

Bu ara yazılımın başlatılır, ASP.NET Core `Startup.cs` dosya.

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Ara yazılım, aşağıdaki yönerge tarafından web API'sine eklenir:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Şu anda Azure AD v1.0 web API ASP.NET Core şablonları oluşturun. Ancak, kolayca bunları aşağıdaki kodu ekleyerek Microsoft kimlik platformu uç noktasını kullanacak şekilde değiştirebilirsiniz `Startup.cs` dosya.

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform v2.0 web API
    options.Authority += "/v2.0";

    // The web API accepts as audiences are both the Client ID (options.Audience) and api://{ClientID}
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line of business apps),
    // we inject our own multi-tenant validation logic (which even accepts both V1 and V2 tokens)
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ValidateAadIssuer;
});
```

## <a name="token-validation"></a>Belirteç doğrulama

Openıd Connect ara yazılımını web apps'te gibi JwtBearer ara yazılım tarafından yönlendirilen `TokenValidationParameters` belirteci doğrulamak için. Belirteci (gerekirse) şifresi, talepleri ayıklanır ve imza doğrulanır. Ardından, belirteci aşağıdaki veriler için kontrol ederek doğrulanır:

- Web API (dinleyici) yöneliktir
- Web API'si (sub) çağırmak için izin verilen bir uygulama için verilmiş
- Bir güvenilir güvenlik belirteci sunucu (STS) tarafından (veren) verilmiş
- Belirtecinin ömrü menzil (Bitiş)
- (İmza) ile değiştirilmiş değildi

Özel doğrulama olabilir. Örneğin, (bir belirtece katıştırılan) İmzalama anahtarları güvenilirdir ve belirtecin yeniden olmadığından emin doğrulamak mümkündür. Son olarak, bazı protokoller belirli doğrulamaları gerekir.

### <a name="validators"></a>Doğrulayıcıları

Doğrulama adımları da olan doğrulayıcıları içine yakalanır [.NET için Microsoft kimlik modeli uzantısı](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) bir kaynak dosyasında açık kaynak kitaplığı: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs)

Doğrulayıcıları, aşağıdaki tabloda açıklanmıştır:

| Doğrulayıcı | Açıklama |
|---------|---------|
| `ValidateAudience` | Belirteç (benim için) belirteci doğrular uygulamanın olmasını sağlar. |
| `ValidateIssuer` | Belirtecin güvenilen bir STS tarafından (birinden güvendiğim) verildiğini sağlar. |
| `ValidateIssuerSigningKey` | Doğrulama belirteci Güvenleri'ni uygulama (anahtar belirteç genellikle gerekli değildir, burada katıştırılmış özel durum) belirteci imzalamak için kullanılan anahtar sağlar. |
| `ValidateLifetime` | Belirteç hala (veya zaten) geçerli olmasını sağlar. Belirteç ömrünü kontrol ederek, işiniz (`notbefore`, `expires` talep) aralık. |
| `ValidateSignature` | Belirteç değiştirilmediğinden sağlar. |
| `ValidateTokenReplay` | Belirteç değil durumdayken sağlar (özel durum bazı tek seferlik kullanım protokoller için). |

Doğrulayıcıları özellikleri ile ilişkili tüm `TokenValidationParameters` sınıfı, kendilerini ASP.NET/ASP.NET çekirdek yapılandırmadan başlatılır. Çoğu durumda, parametreleri değiştirmek zorunda kalmazsınız. Tek bir kiracıya ait olmayan uygulamaların (yani herhangi bir kuruluş veya kişisel Microsoft hesapları kullanıcıların kabul web uygulamaları) bir istisna vardır; bu durumda, veren doğrulanmış olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-protected-web-api-production.md)
