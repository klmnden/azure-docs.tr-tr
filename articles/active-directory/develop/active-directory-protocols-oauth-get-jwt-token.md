---
title: Azure AD kimlik doğrulaması ve JWT belirteci kullanarak OAuth 2.0 alma
description: Güvenli web uygulamaları ve web API'leri kuruluşunuzdaki erişmek için OAuth 2.0 kullanarak Azure Active Directory ile kimlik doğrulaması yapmayı gösteren kod örneği.
services: active-directory
author: rloutlaw
manager: angerobe
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: routlaw
ms.custom: aaddev
ms.openlocfilehash: d77af898d5baef4fa7970132b0eb8deddb8f68cb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981806"
---
# <a name="request-an-access-token-using-oauth-20-to-access-web-apis-and-applications-secured-by-azure-active-directory"></a>OAuth 2.0 erişim web API'leri ve uygulamaları Azure Active Directory tarafından güvenli kullanarak bir erişim belirteci isteği

Bu makalede, bir JSON Web Token (JWT) Azure AD tarafından güvenli hale getirilmiş kaynaklara erişmek için alınacağı gösterilmektedir. Sahip olduğunuz varsayılır bir [yetkilendirme belirteci](/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code) kullanıcı izni veya aracılığıyla bir [hizmet sorumlusu](/azure/active-directory/develop/active-directory-application-objects).

## <a name="build-the-request"></a>Derleme isteği

Aşağıdaki `POST` belirli bir uygulama veya Azure AD tarafından güvenli API erişimi için bir JWT almak için HTTP isteği.

```http
POST https://{tenant}/oauth2/v2.0/token?client_id={client-id}
&scope={scope}
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh   
```

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki üst bilgiler gereklidir:

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
| *Ana bilgisayar:* | https://login.microsoftonline.com |
| *İçerik türü:*| Application/x-www-form-urlencoded işlemek |
 

### <a name="uri-parameters"></a>URI parametreleri

| Parametre     |                       | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                |
|---------------|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| kiracı        | gerekli              | `{tenant}` İstek yolunda değer, uygulamaya oturum denetimi için kullanılabilir. İzin verilen değerler `common`, `organizations`, `consumers`ve Kiracı tanımlayıcıları. Daha fazla ayrıntı için [protokolü temel](active-directory-v2-protocols.md#endpoints).                                                                                                                                                   |
| client_id     | gerekli              | Uygulama kimliği kayıt portalı ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) uygulamanızı atanmış.                                                                                                                                                                                                                             |
| grant_type değeri    | gerekli              | Olmalıdır `authorization_code` yetkilendirme kod akışı için.                                                                                                                                                                                                                                                                                                                                                                            |
| scope         | gerekli              | Kapsamları boşlukla ayrılmış listesi. İçinde bu oluşturan istenen kapsam ile eşdeğerdir veya kapsamları çağrısında bir alt kümesi olmalıdır `/authorize`. Ardından bu istekte belirtilen kapsamlar birden çok kaynak sunucusu yayılıyorsa, v2.0 uç noktası ilk kapsamında belirtilen kaynak için bir belirteç döndürür. Kapsamlar hakkında daha ayrıntılı açıklaması için başvurmak [izinler ve onay kapsamları](v2-permissions-and-consent.md). |
| Kod          | gerekli              | Çağrısında aldığınız authorization_code `/authorize`                                                                                                                                                                                                                                                                                                                                                                |
| redirect_uri  | gerekli              | Authorization_code almak için kullanılan aynı redirect_uri değer.                                                                                                                                                                                                                                                                                                                                                             |
| client_secret | Web apps için gerekli | Uygulama kayıt Portalı'nda uygulamanız için oluşturduğunuz uygulama gizli anahtarı. Yerel bir uygulamada kullanmayın, çünkü client_secrets cihazlarda güvenilir bir şekilde depolanamaz. Web uygulamaları ve web client_secret güvenli bir şekilde sunucu tarafında depolama yeteneği olan API'leri için gereklidir.  İstemci gizli dizileri gönderilmeden önce URL kodlamalı olmalıdır.                                                                                 |
| code_verifier | isteğe bağlı              | Authorization_code elde etmek için kullanılan aynı code_verifier. PKCE bir yetkilendirme kodu verme istekte kullanılan gereklidir. Daha fazla bilgi için [PKCE RFC](https://tools.ietf.org/html/rfc7636)                                                                                                                                                                                                                                                                                             |
## <a name="handle-the-response"></a>Yanıtı işleme

Başarılı bir token yanıt JWT belirtecini içerir ve görünür:

```json
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametre     | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| access_token  | İstenen [erişim belirteci](access-tokens.md). Uygulama, web API'si gibi güvenli kaynağına kimliğini doğrulamak için bu belirteci kullanabilirsiniz.                                                                                                                                                                                                                                                                                                                                    |
| token_type    | Belirteç türü değeri gösterir. Azure AD destekleyen tek taşıyıcı türüdür                                                                                                                                                                                                                                                                                                                                                                           |
| expires_in    | Ne kadar süreyle erişim belirteci (saniye olarak) geçerli değil.                                                                                                                                                                                                                                                                                                                                                                                                       |
| scope         | Access_token için geçerli olan kapsamları.                                                                                                                                                                                                                                                                                                                                                                                                         |
| refresh_token | OAuth 2.0 yenileme belirteci. Bu belirteç kullanabilecek geçerli erişim belirtecinin süresi dolduktan sonra ek erişim belirteçlerini almak. Refresh_tokens uzun süreli ve uzun süre için kaynaklarına erişimi korumak için kullanılabilir. Daha fazla ayrıntı için başvurmak [v2.0 kod başvurusu vermek](v2-oauth2-auth-code-flow.md#refresh-the-access-token). <br> **Not:** yalnızca sağlanan if `offline_access` kapsam istendi.                                               |
| id_token      | Bir işaretsiz JSON Web Token (JWT). Uygulama isteği açan kullanıcı hakkında bilgi için bu belirteci parçalarını çözebilen. Uygulama değerleri önbelleğe ve bunları görüntüleyebilirsiniz, ancak, bunlar üzerinde herhangi bir yetkilendirme veya güvenlik sınırları için doğrulamamalısınız. İd_tokens hakkında daha fazla bilgi için bkz: [ `id_token reference` ](id-tokens.md). <br> **Not:** yalnızca sağlanan if `openid` kapsam istendi. |



