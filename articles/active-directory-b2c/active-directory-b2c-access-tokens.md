---
title: İstek bir erişim belirteci - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'den bir erişim belirteci isteği öğrenin.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 5670d8b3c97cc1f9f6d149e8eadaa60d527e45f5
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683945"
---
# <a name="request-an-access-token-in-azure-active-directory-b2c"></a>Azure Active Directory B2C, bir erişim belirteci isteği

Bir *erişim belirteci* Apı'leriniz için verilen izinleri belirlemek için Azure Active Directory (Azure AD) B2C'de kullanabileceğiniz talepleri içerir. Kaynak sunucuda çağrılırken bir erişim belirteci HTTP isteğinde bulunması gerekir. Bir erişim belirteci olarak gösterilir **access_token** içinde Azure AD B2C alınan yanıtları. 

Bu makalede bir web uygulaması ve web API'si için bir erişim belirteci isteği gösterilmektedir. Azure AD B2C belirteçleri hakkında daha fazla bilgi için bkz. [belirteçler Azure Active Directory B2C genel bakış](active-directory-b2c-reference-tokens.md).

> [!NOTE]
> **Web API'si zincirleri (On-Behalf-Of) Azure AD B2C tarafından desteklenmiyor.** -Çoğu mimari başka bir aşağı akış web API'si, hem Azure AD B2C ile güvenliği sağlanan çağırmak için gereken API web içerir. Bu senaryo, sırayla başka bir hizmeti çağıran bir web API arka ucu olan istemcilerde yaygındır. Bu Zincirli web API'si senaryosu, aksi takdirde On-Behalf-Of akışı bilinen OAuth 2.0 JWT taşıyıcı kimlik bilgileri verme kullanılarak desteklenebilir. Ancak, On-Behalf-Of akışı şu anda Azure AD B2C'de uygulanmamıştır.

## <a name="prerequisites"></a>Önkoşullar

- [Kullanıcı akışı Oluştur](tutorial-create-user-flows.md) etkinleştirme kullanıcıların kaydolmak ve uygulamanız için oturum açın.
- Bunu zaten bunu yapmadıysanız [bir web API uygulaması Azure Active Directory B2C kiracınıza ekleme](add-web-application.md).

## <a name="scopes"></a>Kapsamlar

Kapsamları korumalı kaynaklara izinleri yönetmek için bir yol sağlar. İstemci uygulama bir erişim belirteci istendiğinde, istenen izinler belirtilmesi gerekiyor **kapsam** isteğinin parametresi. Örneğin belirtmek için **kapsam değeri** , `read` sahip API **uygulama kimliği URI'si** , `https://contoso.onmicrosoft.com/api`, kapsamı olacaktır `https://contoso.onmicrosoft.com/api/read`.

Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Aynı istekte birden fazla izin almak için birden çok girişi tek ekleyebilirsiniz **kapsam** boşluklarla ayrılmış istek parametresi.

Aşağıdaki örnek, bir URL çözülmüş kapsamları gösterir:

```
scope=https://contoso.onmicrosoft.com/api/read openid offline_access
```

Aşağıdaki örnek, bir URL olarak kodlanmış kapsamları gösterir:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fapi%2Fread%20openid%20offline_access
```

En az bir izin verilirse, istemci uygulamanız için verilen değerinden daha fazla kapsam istemesi durumunda, çağrı başarılı olur. **Scp** elde edilen erişim belirtecinde talep, başarılı bir şekilde verilmiş izinleri doldurulur. Openıd Connect standart birkaç özel kapsam değerleri belirtir. Aşağıdaki kapsamlar, kullanıcının profilini erişim izni temsil eder:

- **openıd** -kimlik belirteci ister.
- **offline_access** -istekleri kullanarak bir yenileme belirteci [kimlik doğrulama kodu akışları](active-directory-b2c-reference-oauth-code.md).

Varsa **response_type** parametresinde bir `/authorize` istek içerir `token`, **kapsam** parametre içermelidir en az bir kaynak kapsamı dışında `openid` ve `offline_access`, verilecek. Aksi takdirde, `/authorize` istek başarısız olur.

## <a name="request-a-token"></a>Bir belirteç isteği

Bir erişim belirteci istemek için bir yetkilendirme kodu gerekir. Bir isteğin bir örnek aşağıdadır `/authorize` uç noktası için bir yetkilendirme kodu. Özel etki alanları, erişim belirteçleri ile kullanım için desteklenmez. İstek URL'sindeki Kiracı name.onmicrosoft.com etki alanınızı kullanın.

Aşağıdaki örnekte, bu değerleri değiştirin:

- `<tenant-name>` -Azure AD B2C kiracınızın adı.
- `<policy-name>` -Özel ilke veya kullanıcı akışınızı adı.
- `<application-ID>` -Kullanıcı akışını desteklemek için kayıtlı web uygulaması uygulama tanımlayıcısı.
- `<redirect-uri>` - **Yeniden yönlendirme URI'si** istemci uygulaması kaydolurken girdiğiniz.

```
GET https://<tenant-name>.b2clogin.com/tfp/<tenant-name>.onmicrosoft.com/<policy-name>/oauth2/v2.0/authorize?
client_id=<application-ID>
&nonce=anyRandomValue
&redirect_uri=https://jwt.ms
&scope=https://tenant-name>.onmicrosoft.com/api/read
&response_type=code 
```

Yetkilendirme kodu ile yanıt şu örneğe benzer olmalıdır:

```
https://jwt.ms/?code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
```

Başarılı yetkilendirme kodunu aldıktan sonra bunu bir erişim belirteci istemek için kullanabilirsiniz:

```
POST <tenant-name>.onmicrosoft.com/oauth2/v2.0/token?p=<policy-name> HTTP/1.1
Host: https://<tenant-name>.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&client_id=<application-ID>
&scope=https://<tenant-name>.onmicrosoft.com/api/read
&code=eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMC...
&redirect_uri=https://jwt.ms
&client_secret=2hMG2-_:y12n10vwH...
```

Aşağıdakine benzer bir şey görmeniz gerekir:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrN...",
    "token_type": "Bearer",
    "not_before": 1549647431,
    "expires_in": 3600,
    "expires_on": 1549651031,
    "resource": "f2a76e08-93f2-4350-833c-965c02483b11",
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiJjNjRhNGY3ZC0zMDkxLTRjNzMtYTcyMi1hM2YwNjk0Z..."
}
```

Kullanırken https://jwt.ms döndürülen erişim belirteci incelemek için aşağıdaki örneğe benzer bir şey görmeniz gerekir:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dl..."
}.{
  "iss": "https://contoso0926tenant.b2clogin.com/c64a4f7d-3091-4c73-a7.../v2.0/",
  "exp": 1549651031,
  "nbf": 1549647431,
  "aud": "f2a76e08-93f2-4350-833c-965...",
  "oid": "1558f87f-452b-4757-bcd1-883...",
  "sub": "1558f87f-452b-4757-bcd1-883...",
  "name": "David",
  "tfp": "B2C_1_signupsignin1",
  "nonce": "anyRandomValue",
  "scp": "read",
  "azp": "38307aee-303c-4fff-8087-d8d2...",
  "ver": "1.0",
  "iat": 1549647431
}.[Signature]
```

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi [Azure AD B2C'de belirteçleri yapılandırma](configure-tokens.md)
