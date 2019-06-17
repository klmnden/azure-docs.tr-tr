---
title: Korumalı Web API'sini - uygulama kodu yapılandırma | Azure Active Directory
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
ms.openlocfilehash: b700825be9a7fe23fe4b50a2d69d4de71f7dc038
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116460"
---
# <a name="protected-web-api---adding-authorization-to-your-api"></a>Korumalı web API'si - yetkilendirme için API'nize ekleme

Bu makalede, yetkilendirme Web API'nizi nasıl ekleyebileceğinizi açıklar. Yalnızca tarafından çağrılır, bu koruma sağlar:

- doğru kapsamlar ile kullanıcılar adına uygulamalar 
- veya doğru uygulama rolleri ile arka plan programları.

Bir ASP.NET / ASP.NET Core Web API'si, korunacak eklemeniz gerekir `[Authorize]` özniteliği:

- Denetleyicinin kendisini korunacak denetleyicinin tüm eylemleri istiyorsanız
- veya API'niz için ayrı ayrı denetleyicisinin eylem.

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Ancak, bu koruma yeterli değildir. Yalnızca ASP.NET garantilediği / ASP.NET Core, belirteci doğrulamak. API, Web API'sini çağırmak için kullanılan belirteci, özellikle bekler talepleri ile istendi doğrulamak gerekir:

- **kapsamları** API bir kullanıcı adına çağrılırsa
- **uygulama rolleri** , API, bir arka plan programı uygulamasından çağrılabilir.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>Adlı kullanıcılar adına API kapsamlarda doğrulanıyor

API'nizi bir kullanıcı adına bir istemci uygulaması tarafından çağrılır, sonra API için belirli kapsamları ile bir taşıyıcı belirteci istemek gereken, (bkz [kod yapılandırma | Taşıyıcı belirteç](scenario-protected-web-api-app-configuration.md#bearer-token))

```CSharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The Web API will only accept tokens 1) for users, 2) having the `access_as_user` scope for
    /// this API
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result
        ...
    }
...
}
```

`VerifyUserHasAnyAcceptedScope` Yöntemi aşağıdaki gibi bir şey yapmak:

- adlı bir talep olduğunu doğrulayın `http://schemas.microsoft.com/identity/claims/scope` veya `scp`
- Talep API tarafından beklenen kapsamı içeren bir değeri olduğunu doğrulayın.

```CSharp
    /// <summary>
    /// When applied to an <see cref="HttpContext"/>, verifies that the user authenticated in the 
    /// Web API has any of the accepted scopes.
    /// If the authenticated user does not have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized with the message telling which scopes are expected in the token
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

Bu örnek kod için ASP.NET Core ' dir. ASP.NET yalnızca değiştirme için `HttpContext.User` tarafından `ClaimsPrincipal.Current`ve talep türünü `"http://schemas.microsoft.com/identity/claims/scope"` tarafından `"scp"` (Ayrıca bkz. aşağıdaki kod parçacığı)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>Uygulama rolleri tarafından arka plan programları olarak adlandırılan API'lerindeki doğrulanıyor

Web API'sı çağrılan varsa bir [Daemon uygulamasının](scenario-daemon-overview.md), sonra da bu uygulama, Web API'niz için bir uygulama izni istemeniz gerekir. [Scenario-protected-web-api-app-registration.md#how-to-expose-application-permissions--app-roles-] içinde API'nizi bu izinleri sunan gördük (örneğin olarak `access_as_application` uygulama rol).
Apı'lerinizi aldığı belirteci içerdiğini doğrulamak artık gereksinim `roles` talepleri ve bu talep bekler değerine sahiptir. Bu doğrulamayı yapan kod için test yerine temsilci izinleri, tek fark, doğrular kod benzer `scopes`, denetleyici eylem için test edecek `roles`:

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
    // In this case we look for a `role` value of `access_as_application`
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

Bu örnek kod için ASP.NET aynıdır. ASP.NET Core için yalnızca değiştirin `ClaimsPrincipal.Current` tarafından `HttpContext.User` ve `"roles"` adına göre talep `"http://schemas.microsoft.com/identity/claims/roles"` (Ayrıca bkz. Yukarıdaki kod parçacığında)

### <a name="accepting-app-only-tokens-if-the-web-api-should-only-be-called-by-daemon-apps"></a>Web API'sini yalnızca arka plan programları tarafından çağrılmalıdır, uygulama yalnızca belirteçleri kabul etme

`roles` Kullanıcı atama desenleri kullanıcılar için de talep kullanılır (bkz [nasıl yapılır: Uygulama rolleri uygulamanıza ekleyin ve bunları belirteç almak](howto-add-app-roles-in-azure-ad-apps.md)). Rolleri her ikisi de atanabilir ise bu nedenle rolleri denetimini kullanıcıları ve başka bir şekilde, geçici olarak oturum açmak uygulama izin verir. Bu Karışıklığı önlemek kullanıcılar ve uygulamalar için bildirilen farklı rollere sahip öneririz.

Yalnızca arka plan programı uygulamaları, Web API'sini çağırmak izin vermek istiyorsanız, uygulama rolü doğruladığınızda belirteci bir salt uygulama belirteci olduğunu bir koşul eklemek isteyeceksiniz:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid");
string sub = ClaimsPrincipal.Current.FindFirst("sub");
bool isAppOnlyToken = oid == sub;
```

Ters koşulu kontrol etmeyi, API'yi çağırmak için bir kullanıcı oturum açtığında uygulamaları izin verir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-protected-web-api-production.md)
