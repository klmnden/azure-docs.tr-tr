---
title: V1.0 uygulama (Microsoft kimlik doğrulama kitaplığı) için kapsamları | Azure
description: Microsoft Authentication Library (MSAL) kullanarak bir v1.0 uygulama için kapsamları hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44aad6b2fab7e0ab2ff11d8469782b001b1f4d18
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545889"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>V1.0 belirteçleri kabul eden bir Web API'si için kapsamları

OAuth2, istemci uygulamaları için bir Azure AD geliştiricilerin (v1.0) web API (kaynak) uygulamasının sunan izin kapsamları izinlerdir. Bu izin kapsamları, onay işlemi sırasında istemci uygulamalara verilebilir. Bölümüne bakın `oauth2Permissions` içinde [Azure Active Directory Uygulama bildirimi başvurusu](reference-app-manifest.md#manifest-reference).

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Belirli OAuth2 izinleri v1.0 uygulamanın erişim istemek için kapsamları
V1.0 uygulamasının belirli kapsamlarının belirteçlerini almak istiyorsanız (örneğin olan Azure AD graph https://graph.windows.net), kapsamları bir istenen kaynak tanımlayıcısı bu kaynak için istenen bir OAuth2 izinle birleştirerek oluşturmanız gerekir.

Örneğin, uygulama kimliği URI'SİNİN olduğu v1.0 web API'si kullanıcı adına erişim için `ResourceId`:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

Okuma ve yazma MSAL.NET Azure Azure AD graph API'sini kullanarak Active Directory ile istiyorsanız (https://graph.windows.net/), kapsamları aşağıdaki gibi bir liste oluşturursunuz:

```csharp
string ResourceId = "https://graph.windows.net/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://graph.windows.net/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

Azure Resource Manager API'si için karşılık gelen kapsam yazmak istiyorsanız (https://management.core.windows.net/), aşağıdaki kapsamın (Not iki eğik çizgi) istemek: gerekir

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> Azure Resource Manager API'si bir eğik çizgi, hedef kitle talebinin (aud) bekler ve ardından kapsamı API addan ayırmak için bir eğik çizgi yoktur çünkü iki eğik çizgi kullanmanız gerekir.

Azure AD tarafından kullanılan mantıksal aşağıda verilmiştir:

- Bir v1.0 erişim belirteci (tek olası) aud ile ADAL (v1.0) uç noktası için kaynak =
- MSAL aud v2.0 belirteç kabul eden bir kaynak için bir erişim belirteci isteyen (Microsoft kimlik Platformu (v2.0) uç noktası) için kaynak =. Uygulama Kimliği
- Azure AD, bir erişim belirteci (yukarıdaki söz konusu olduğu) v1.0 bir erişim belirteci kabul eden bir kaynak için soran MSAL için (v2.0 uç noktası), son eğik çizgiden önce her şeyi alma ve kaynak tanımlayıcısı kullanılarak istenen hedef istenen kapsamından ayrıştırır. Bu nedenle, https://database.windows.net hedef kitlesine bekliyor "https://database.windows.net/", istek bir kapsam gerekir"https://database.windows.net//.default". Ayrıca bkz. GitHub sorunu [#747: Kaynak URL'nin sonunda eğik çizgi atlanırsa, hangi sql kimlik doğrulama hatası nedeniyle](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747).

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>V1.0 uygulamasının tüm izinleri erişim istemek için kapsamları
V1.0 uygulamanın statik tüm kapsamlar için bir belirteç almak istiyorsanız, ".default" uygulama kimliği URI'si API ekleyin:

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-client-credential-flow--daemon-app"></a>Kapsamlar için istemci istemek için kimlik bilgisi akış / daemon uygulama
Söz konusu olduğunda istemci kimlik bilgileri akışı, geçirilecek kapsamını da olacaktır `/.default`. Bu Azure AD'ye söyler: "Yönetici uygulama kaydında seçtiği tüm uygulama düzeyinde izinleri.