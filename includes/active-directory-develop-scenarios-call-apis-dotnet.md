---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 0196d39f5b131bc54e00412beb7fdf10b7352336
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075153"
---
### <a name="authenticationresult-properties-in-msalnet"></a>MSAL.NET AuthenticationResult özellikleri

Belirteçlerini almak için yöntemler döndürür bir `AuthenticationResult` (veya zaman uyumsuz yöntemler için bir `Task<AuthenticationResult>`.

İçinde MSAL.NET `AuthenticationResult` kullanıma sunar:

- `AccessToken` kaynaklara erişmek için Web API'si için. Bu parametre bir dizedir, genellikle bir base64 kodlamalı JWT ancak istemci hiçbir zaman içinde erişim belirteci görünmelidir. Biçim tutarlı kalmasını garanti yoktur ve kaynak için şifrelenebilir. Kişilere erişim belirteci istemci içeriğine bağlı olarak kod yazma hataları ve istemci mantıksal sonları büyük kaynaklarını biridir. Ayrıca bkz: [erişim belirteçleri](../articles/active-directory/develop/access-tokens.md)
- `IdToken` (Bu parametreyi kodlanmış bir JWT) kullanıcısı için. Bkz: [kimliği belirteçleri](../articles/active-directory/develop/id-tokens.md)
- `ExpiresOn` belirtecin süresinin sona erdiği tarih/saat bildirir
- `TenantId` kullanıcının bulunduğu Kiracı içerir. Konuk kullanıcılar (Azure AD B2B senaryoları) için Kiracı kimliği benzersiz kiracının Konuk Kiracı ' dir.
Bir kullanıcı için belirteç iletildiğinde `AuthenticationResult` bu kullanıcı hakkındaki bilgileri de içerir. Burada hiçbir kullanıcıyla (uygulama) belirteçleri istenen gizli istemci akışlar için bu kullanıcı bilgileri null olur.
- `Scopes` Belirteç verildiği için.
- Kullanıcının benzersiz kimliği.

### <a name="iaccount"></a>IAccount

MSAL.NET hesap kavramını tanımlar (aracılığıyla `IAccount` arabirimi). Bu değişiklik doğru semantiği sağlar: aynı kullanıcı farklı Azure'da çeşitli hesapların olabilir olgu AD dizinleri. Ayrıca hesap giriş bilgileri sağlanır MSAL.NET Konuk senaryoları, söz konusu olduğunda daha iyi bilgi sağlar.
Aşağıdaki diyagramda yapısını göstermektedir `IAccount` arabirimi:

![image](https://user-images.githubusercontent.com/13203188/44657759-4f2df780-a9fe-11e8-97d1-1abbffade340.png)

`AccountId` Sınıfı tanımlayan belirli bir kiracıda bir hesabı. Bunu, aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama |
|----------|-------------|
| `TenantId` | Hesabının bulunduğu Kiracı kimliği olan GUID dize temsili. |
| `ObjectId` | Kiracı hesap sahibi olan kullanıcının kimliği bir GUID dize temsili. |
| `Identifier` | Hesabı için benzersiz tanımlayıcı. `Identifier` birleşimi olan `ObjectId` ve `TenantId` bir virgül ve misiniz değil base64 kodlu tarafından ayrılmış. |

`IAccount` Arabirimi tek bir hesap bilgilerini temsil eder. Aynı kullanıcı farklı kiracıda bulunabilir, diğer bir deyişle, bir kullanıcı birden çok hesabı içerebilir. Üyeleri şunlardır:

| Özellik | Açıklama |
|----------|-------------|
| `Username` | UserPrincipalName (UPN) biçiminde görüntülenebilen değeri içeren bir dize john.doe@contoso.com. HomeAccountId ve HomeAccountId.Identifier null olmaz ancak bu dize null olabilir. Bu özellik değiştirir `DisplayableId` özelliği `IUser` MSAL.NET önceki sürümlerinde. |
| `Environment` | Bu hesabın, örneğin, kimlik sağlayıcısı içeren bir dize `login.microsoftonline.com`. Bu özellik değiştirir `IdentityProvider` özelliği `IUser`dışında `IdentityProvider` burada değeri yalnızca ana bilgileriyse Kiracı (ek olarak bulut ortamı) hakkında bilgi de gerekmektedir. |
| `HomeAccountId` | Kullanıcı için giriş hesabının hesap kimliği. Bu özellik, kullanıcının Azure AD kiracılarında benzersiz olarak tanımlar. |

### <a name="using-the-token-to-call-a-protected-api"></a>Korumalı bir API'yi çağırmak için belirteci kullanarak

Bir kez `AuthenticationResult` MSAL tarafından iade edildi (içinde `result`), HTTP yetkilendirme üst bilgisi için korumalı Web API'sine çağrı yapmadan önce eklemeniz gerekebilir.

```CSharp
httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Web API.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```