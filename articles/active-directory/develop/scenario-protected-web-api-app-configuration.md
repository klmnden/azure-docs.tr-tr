---
title: Korumalı Web API'sini - uygulama kodu yapılandırma | Azure
description: Korumalı web API'si oluşturma ve uygulama kodu yapılandırma hakkında bilgi edinin.
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
ms.openlocfilehash: fa262c1c6a091575a70c5670b1c7a7c96e8e2128
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536894"
---
# <a name="protected-web-api-code-configuration"></a>Korumalı web API'sini: Kod yapılandırması

Korumalı web API'niz için kod yapılandırmak için API'leri korumalı olarak tanımlar, bir taşıyıcı belirteç yapılandırma ve doğrulama belirteci anlamanız gerekir.

## <a name="what-defines-aspnetaspnet-core-apis-as-protected"></a>Korumalı olarak ASP.NET/ASP.NET Core API'leri tanımlar?

Web apps gibi ASP.NET/ASP.NET çekirdek web API'leri "korumalı" denetleyici eylemlerini ön eki olduğundan `[Authorize]` özniteliği. Bu nedenle yalnızca API yetkili bir kimlikle çağrılırsa denetleyici eylemleri çağrılabilir.

Aşağıdaki soruları göz önünde bulundurun:

- Web API'si çağıran bir uygulamanın kimliğini nasıl bilir? (Yalnızca uygulama bir web API'si çağırabilirsiniz.)
- Bir kullanıcı adına API web uygulaması çağrılır, kullanıcının kimliği nedir?

## <a name="bearer-token"></a>Taşıyıcı belirteç

Uygulama kimliği ve kullanıcı hakkındaki bilgileri (web uygulaması bir arka plan programı uygulamasından-hizmet çağrıları kabul sürece), uygulama çağrıldığında, üstbilgisinde ayarlanan taşıyıcı belirteci tutulur.

İşte bir C# (MSAL.NET) .NET için Microsoft kimlik doğrulama kitaplığı ile bir belirteç edinme sonra API'yi çağırıp istemci gösteren kod örneği:

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
> Taşıyıcı belirteç, Microsoft kimlik platformu uç nokta için bir istemci uygulaması tarafından istendi *web API'si için*. Web API belirteci doğrulamak ve içerdiği talep görüntülemek gereken tek bir uygulamadır. İstemci uygulamaları belirteçlere talep incelemek hiçbir zaman denemelisiniz. (Web API'si, gelecekte belirteç şifrelenmesini gerektirebilir. Bu gereksinim erişim belirteçleri görüntüleyebilirsiniz istemci uygulamalar için erişimi engeller.)

## <a name="jwtbearer-configuration"></a>JwtBearer yapılandırma

Bu bölümde, bir taşıyıcı belirteç yapılandırma açıklanmaktadır.

### <a name="config-file"></a>Yapılandırma dosyası

```Json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "ClientId": "[Client_id-of-web-api-eg-2ec40e65-ba09-4853-bcde-bcb60029e596]",
    /*
      You need specify the TenantId only if you want to accept access tokens from a single tenant
     (line-of-business app).
      Otherwise, you can leave them set to common.
      This can be:
      - A GUID (Tenant ID = Directory ID)
      - 'common' (any organization and personal accounts)
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

Bir uygulama tutan bir denetleyici eylemi adlı ne zaman bir `[Authorize]` öznitelik ASP.NET/ASP.NET çekirdek çağıran isteğin yetkilendirme üst bilgisinde taşıyıcı belirteç bakar ve erişim belirteci ayıklar. Ardından belirteci JwtBearer ara yazılım, .NET için Microsoft IdentityModel uzantıları çağırır iletilir.

ASP.NET Core, bu ara yazılım Startup.cs dosyasındaki başlatılır:

```CSharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
```

Ara yazılım, bu yönerge tarafından web API'sine eklenir:

```CSharp
 services.AddAzureAdBearer(options => Configuration.Bind("AzureAd", options));
```

 Şu anda ASP.NET Core şablonları kuruluşunuz veya kişisel hesaplar ile değil, tüm kuruluş içindeki kullanıcıları Azure Active Directory (Azure AD) web oturum API'ler oluşturun. Ancak, bunları bu kod Startup.cs dosyasına ekleyerek Microsoft kimlik platformu uç noktasını kullanacak şekilde kolayca değiştirebilirsiniz:

```CSharp
services.Configure<JwtBearerOptions>(AzureADDefaults.JwtBearerAuthenticationScheme, options =>
{
    // This is a Microsoft identity platform v2.0 web API.
    options.Authority += "/v2.0";

    // The web API accepts as audiences both the Client ID (options.Audience) and api://{ClientID}.
    options.TokenValidationParameters.ValidAudiences = new []
    {
     options.Audience,
     $"api://{options.Audience}"
    };

    // Instead of using the default validation (validating against a single tenant,
    // as we do in line-of-business apps),
    // we inject our own multitenant validation logic (which even accepts both v1 and v2 tokens).
    options.TokenValidationParameters.IssuerValidator = AadIssuerValidator.ValidateAadIssuer;
});
```

## <a name="token-validation"></a>Belirteç doğrulama

Openıd Connect ara yazılımını web apps'te gibi JwtBearer ara yazılım tarafından yönlendirilen `TokenValidationParameters` belirteci doğrulamak için. Belirteci (gereken şekilde) şifresi, talepleri ayıklanır ve imza doğrulanır. Ara yazılım için bu verileri kontrol ederek ardından belirteci doğrular:

- Web API (dinleyici) hedeflenir.
- Bu, web API'si (sub) çağırmak için izin verilen bir uygulama için verilmiş.
- Bu, bir güvenilen güvenlik belirteci hizmeti (STS) (veren) tarafından verilmiş.
- Yaşam süresi de (Bitiş) aralığındadır.
- Bu, (imza) ile değiştirilmiş değildi.

Özel doğrulama olabilir. Örneğin, (bir belirtece katıştırılan) İmzalama anahtarları güvenilirdir ve belirtecin yeniden olmadığından emin doğrulamak mümkündür. Son olarak, bazı protokoller belirli doğrulamaları gerekir.

### <a name="validators"></a>Doğrulayıcıları

Doğrulama adımları da olan doğrulayıcıları kaydedilir [.NET için Microsoft IdentityModel uzantıları](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) bir kaynak dosyasında açık kaynak kitaplığı: [Microsoft.IdentityModel.Tokens/Validators.cs](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet/blob/master/src/Microsoft.IdentityModel.Tokens/Validators.cs).

Doğrulayıcılar, bu tabloda açıklanmıştır:

| Doğrulayıcı | Açıklama |
|---------|---------|
| `ValidateAudience` | Belirteç (benim için) belirteci doğrular uygulamanın sağlar. |
| `ValidateIssuer` | Belirteç (birinden güvendiğim) güvenilir bir STS tarafından verilen sağlar. |
| `ValidateIssuerSigningKey` | Doğrulama belirteci güvenleri uygulama, belirteç imzalamak için kullanılan anahtar sağlar. (Özel durum burada anahtar belirteci katıştırılır. Genellikle gerekli değil.) |
| `ValidateLifetime` | Yine de (veya zaten) geçerli belirteç sağlar. Doğrulayıcı denetler, belirteç ömrünü (`notbefore` ve `expires` talep) aralık. |
| `ValidateSignature` | Belirteç kurcalanmadığı sağlar. |
| `ValidateTokenReplay` | Belirteci yeniden değil sağlar. (Bazı tek seferlik kullanım protokoller için özel durum.) |

Doğrulayıcıları özellikleri ile ilişkili tüm `TokenValidationParameters` sınıfı, kendilerini ASP.NET/ASP.NET çekirdek yapılandırmadan başlatılır. Çoğu durumda, parametreleri değiştirmek zorunda kalmazsınız. Tek kiracılar olmayan uygulamalar için bir istisna vardır. (Diğer bir deyişle, kullanıcıların herhangi bir kuruluş veya kişisel Microsoft hesapları kabul uygulamalar web.) Bu durumda, veren doğrulanmış olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kapsamlar ve uygulama rolleri kodunuzda doğrulayın](scenario-protected-web-api-verification-scope-app-roles.md)
