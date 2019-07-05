---
title: Korumalı Web API'sini - uygulama kodu yapılandırma | Azure Active Directory
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
ms.openlocfilehash: 23ff03316a1f9409d4d6e4b7ddf52d0c8cc7a909
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551532"
---
# <a name="protected-web-api-adding-authorization-to-your-api"></a>Korumalı web API'sini: Yetkilendirme için API'nize ekleme

Bu makalede, yetkilendirme web API'nize nasıl ekleyebileceğinizi açıklar. Bu koruma API'sı yalnızca çağrılan sağlar:

- Doğru kapsamlar sahip kullanıcılar adına uygulamalar.
- Doğru uygulama rolleri olan arka plan programı uygulamaları için.

Bir ASP.NET/ASP.NET Core web API'sini korumak için eklemeniz gerekecektir `[Authorize]` özniteliği bunlardan biri:

- Denetleyicinin kendisini, korunacak denetleyicinin tüm eylemleri istiyorsanız
- API'niz için ayrı ayrı denetleyicisinin eylemi

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Ancak, bu koruma yeterli değildir. Yalnızca ASP.NET/ASP.NET çekirdek belirteci doğrular garanti eder. Web API ile talep istendi çağırmak için kullanılan belirteci bu, özellikle beklediğini doğrulamak API'nizi gerekir:

- *Kapsamları*, API, bir kullanıcı adına çağrılır.
- *Uygulama rolleri*, API, bir arka plan programı uygulamasından çağrılabilir.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>Adlı kullanıcılar adına API kapsamlarda doğrulanıyor

Bir kullanıcı adına bir istemci uygulaması tarafından API'nizi çağrılırsa, API için belirli kapsamı olan bir taşıyıcı belirteci istemek gerekir. (Bkz [kod yapılandırma | Taşıyıcı belirteç](scenario-protected-web-api-app-configuration.md#bearer-token).)

```CSharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result.
        ...
    }
...
}
```

`VerifyUserHasAnyAcceptedScope` Yöntemi aşağıdaki gibi bir şey yapmak:

- Adlı bir talep olduğunu doğrulayın `http://schemas.microsoft.com/identity/claims/scope` veya `scp`.
- Talep API tarafından beklenen kapsamı içeren bir değeri olduğunu doğrulayın.

```CSharp
    /// <summary>
    /// When applied to a <see cref="HttpContext"/>, verifies that the user authenticated in the 
    /// web API has any of the accepted scopes.
    /// If the authenticated user doesn't have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized error with a message noting which scopes are expected in the token.
    /// </summary>
    /// <param name="acceptedScopes">Scopes accepted by this API</param>
    /// <exception cref="HttpRequestException"/> with a <see cref="HttpResponse.StatusCode"/> set to 
    /// <see cref="HttpStatusCode.Unauthorized"/>
    public static void VerifyUserHasAnyAcceptedScope(this HttpContext context,
                                                     params string[] acceptedScopes)
    {
        if (acceptedScopes == null)
        {
            throw new ArgumentNullException(nameof(acceptedScopes));
        }
        Claim scopeClaim = HttpContext?.User
                                      ?.FindFirst("http://schemas.microsoft.com/identity/claims/scope");
        if (scopeClaim == null || !scopeClaim.Value.Split(' ').Intersect(acceptedScopes).Any())
        {
            context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
            string message = $"The 'scope' claim does not contain scopes '{string.Join(",", acceptedScopes)}' or was not found";
            throw new HttpRequestException(message);
        }
    }
```

Bu örnek kod için ASP.NET Core ' dir. ASP.NET, yalnızca değiştirin `HttpContext.User` ile `ClaimsPrincipal.Current`, talep türünü değiştirin `"http://schemas.microsoft.com/identity/claims/scope"` ile `"scp"`. (Ayrıca bu makalenin devamındaki kod parçacığına bakın.)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>Uygulama rolleri tarafından arka plan programları olarak adlandırılan API'lerindeki doğrulanıyor

Web API'nizi çağrılırsa bir [arka plan programı uygulama](scenario-daemon-overview.md), bu uygulama bir web API uygulama izni istemeniz gerekir. İçinde anlatıldığı [uygulama izinleri (uygulama rolleri) gösterme](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles) API'nizi bu izinleri gösterir (örneğin, `access_as_application` uygulama rol).
Apı'lerinizi aldığı belirteci içerdiğini doğrulamak artık gereksinim `roles` talebi ve bu talep bekler değerine sahiptir. Bu doğrulamayı yapan kod için test yerine temsilci izinleri, tek fark, doğrular kod benzer `scopes`, denetleyici eylem için test edecek `roles`:

```CSharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

`ValidateAppRole()` Yöntemi aşağıdaki gibi olabilir:

```CSharp
private void ValidateAppRole(string appRole)
{
    //
    // The `role` claim tells you what permissions the client application has in the service.
    // In this case, we look for a `role` value of `access_as_application`.
    //
    Claim roleClaim = ClaimsPrincipal.Current.FindFirst("roles");
    if (roleClaim == null || !roleClaim.Value.Split(' ').Contains(appRole))
    {
        throw new HttpResponseException(new HttpResponseMessage 
        { StatusCode = HttpStatusCode.Unauthorized, 
            ReasonPhrase = $"The 'roles' claim does not contain '{appRole}' or was not found" 
        });
    }
}
}
```

Bu örnek kod için ASP.NET aynıdır. ASP.NET Core için yalnızca değiştirin `ClaimsPrincipal.Current` ile `HttpContext.User`ve yerine `"roles"` adıyla talep `"http://schemas.microsoft.com/identity/claims/roles"`. (Ayrıca bu makalenin önceki kısımlarında kod parçacığına bakın.)

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Web API'si, yalnızca arka plan programları tarafından çağrılmalıdır, yalnızca uygulama belirteçleri kabul etme

`roles` Talep kullanıcı atama desenleri kullanıcılar için de kullanılır. (Bkz [nasıl yapılır: Uygulama rolleri uygulamanıza ekleyin ve bunları belirteç almak](howto-add-app-roles-in-azure-ad-apps.md).) Rolleri her ikisi de atanabilir ise bu nedenle rolleri denetimini kullanıcıları ve başka bir şekilde, geçici olarak oturum açmak uygulama izin verir. Bu Karışıklığı önlemek, kullanıcılar ve uygulamalar için farklı roller bildirdiğiniz öneririz.

Yalnızca arka plan programı uygulamaları, web API'sini çağırmasına izin vermek istiyorsanız, uygulama rolü doğrularken bir koşul belirteci bir salt uygulama belirteci olduğunu ekleyin:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid");
string sub = ClaimsPrincipal.Current.FindFirst("sub");
bool isAppOnlyToken = oid == sub;
```

Ters koşulu kontrol etmeyi yalnızca API çağırmak için bir kullanıcının oturumunu uygulamalara izin verir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-protected-web-api-production.md)
