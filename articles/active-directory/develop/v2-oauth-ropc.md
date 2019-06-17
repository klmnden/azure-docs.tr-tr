---
title: Kaynak sahibi parola kimlik bilgisi (ROPC) verme kullanarak kullanıcıların oturum açmak için Microsoft kimlik platformu kullan | Azure
description: Kaynak sahibi parola kimlik bilgisi yetkisi kullanılarak destek tarayıcı olmayan kimlik doğrulaması akar.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/20/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04d2be76072866da2b21718f60fd0c9a5923b15b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545121"
---
# <a name="microsoft-identity-platform-and-the-oauth-20-resource-owner-password-credential"></a>Microsoft kimlik platformu ve OAuth 2.0 kaynak sahibi parola kimlik bilgisi

Microsoft kimlik platformu destekleyen [kaynak sahibi parola kimlik bilgisi (ROPC) verme](https://tools.ietf.org/html/rfc6749#section-4.3), bir uygulama parolasını doğrudan işleyerek kullanıcının oturum açmasını sağlar. Yüksek derecede güven ve kullanıcı Etkilenme ROPC akışı gerektirir ve daha güvenli, diğer akışlar kullanıldığında bu akışı yalnızca kullanmanız gerekir.

> [!IMPORTANT]
>
> * Microsoft kimlik platformu uç nokta ROPC değil kişisel hesaplar Azure AD kiracıları için yalnızca destekler. Yani bir kiracıya özgü uç nokta kullanmanız gerekir (`https://login.microsoftonline.com/{TenantId_or_Name}`) veya `organizations` uç noktası.
> * Bir Azure AD kiracısına davet kişisel hesapları ROPC kullanamazsınız.
> * Parolaları olmayan hesapları ROPC oturum açamaz. Bu senaryo için farklı bir akış uygulamanız için bunun yerine kullanmanızı öneririz.
> * Kullanıcıların uygulamada oturum açmak için çok faktörlü kimlik doğrulaması (MFA) kullanmanız gerekiyorsa, bunun yerine engellenir.

## <a name="protocol-diagram"></a>Protokol diyagramı

Aşağıdaki diyagramda ROPC akışı gösterilmektedir.

![ROPC akışı](./media/v2-oauth2-ropc/v2-oauth-ropc.svg)

## <a name="authorization-request"></a>Yetkilendirme isteği

Tek bir istek ROPC akışıdır&mdash;IDP için istemci kimliği ve kullanıcı kimlik bilgilerini gönderir ve ardından belirteçleri iade alır. İstemci kullanıcının e-posta adresi (UPN) ve parola, bunu yapmadan önce istemeniz gerekir. Hemen başarılı bir isteği sonra istemci kullanıcının kimlik bilgilerini bellekten güvenli bir şekilde serbest bırakmalısınız. Bunları asla kaydetmeniz gerekir.

> [!TIP]
> Postman içinde bu isteğin yürütülmesi deneyin!
> [![Postman içinde çalıştırın](./media/v2-oauth2-auth-code-flow/runInPostman.png)](https://app.getpostman.com/run-collection/f77994d794bab767596d)


```
// Line breaks and spaces are for legibility only.

POST {tenant}/oauth2/v2.0/token
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=user.read%20openid%20profile%20offline_access
&username=MyUsername@myTenant.com
&password=SuperS3cret
&grant_type=password
```

| Parametre | Koşul | Açıklama |
| --- | --- | --- |
| `tenant` | Gerekli | Kullanıcının oturumunu günlüğe kaydetmek istediğiniz dizin Kiracı. Bu GUID veya kolay adı biçiminde olabilir. Bu parametre değerine ayarlanamaz `common` veya `consumers`, ancak ayarlanmış olabilir `organizations`. |
| `grant_type` | Gerekli | Ayarlanmalıdır `password`. |
| `username` | Gerekli | Kullanıcının e-posta adresi. |
| `password` | Gerekli | Kullanıcının parolası. |
| `scope` | Önerilen | Boşlukla ayrılmış bir listesini [kapsamları](v2-permissions-and-consent.md), ya da uygulamanın gerektirdiği izinler. Etkileşimli bir akışta, yönetici veya kullanıcı, bu kapsamlara önceden onaylamalıdır. |

### <a name="successful-authentication-response"></a>Başarılı kimlik doğrulaması yanıtını

Aşağıdaki örnek, başarılı bir belirteç yanıtı gösterir:

```json
{
    "token_type": "Bearer",
    "scope": "User.Read profile openid email",
    "expires_in": 3599,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD..."
}
```

| Parametre | Biçimi | Açıklama |
| --------- | ------ | ----------- |
| `token_type` | String | Her zaman `Bearer`. |
| `scope` | Ayrılmış boşluk dizeleri | Bu parametre, bir erişim belirteci döndürdüyse kapsamlar için erişim belirteci geçerliyse listeler. |
| `expires_in`| int | Dahil edilen erişim belirteci için geçerli olan saniye sayısı. |
| `access_token`| Donuk dize | Çıkarılan [kapsamları](v2-permissions-and-consent.md) , istendi. |
| `id_token` | JWT | Verilen if özgün `scope` parametresi dahil `openid` kapsam. |
| `refresh_token` | Donuk dize | Verilen if özgün `scope` parametresi dahil `offline_access`. |

Yeni erişim belirteçleri almak ve yenileme belirteçlerini açıklanan aynı akışı kullanarak yenileme belirtecini kullanabilirsiniz [OAuth kod flow belgeleri](v2-oauth2-auth-code-flow.md#refresh-the-access-token).

### <a name="error-response"></a>Hata yanıtı

Kullanıcı doğru kullanıcı adı veya parola sağlanan dolmadığından veya istemci istenen onay almadı, kimlik doğrulaması başarısız olur.

| Hata | Açıklama | İstemci eylemi |
|------ | ----------- | -------------|
| `invalid_grant` | Kimlik doğrulaması başarısız oldu | Kimlik bilgileri yanlış veya istemci istenen kapsam için izniniz yok. Kapsamlar verilirse, bu olmayan bir `consent_required` hata döndürülür. Bu meydana gelirse, istemci bir webview veya tarayıcı kullanarak etkileşimli bir istemi kullanıcı göndermesi gerekir. |
| `invalid_request` | İsteği hatalı oluşturulmuş | Üzerinde izin verme türü desteklenmiyor `/common` veya `/consumers` kimlik doğrulaması bağlamı.  Bunun yerine `/organizations` kullanın. |
| `invalid_client` | Uygulamanın düzgün ayarlanır | Bu durum oluşabilir `allowPublicClient` özelliği ayarlanmamış true olarak [uygulama bildirimini](reference-app-manifest.md). `allowPublicClient` ROPC verme bir yeniden yönlendirme URI'si olmadığı için özellik gerekiyor. Azure AD özelliği ayarlanmazsa uygulama genel istemci uygulamanız ya da gizli bir istemci uygulama olup olmadığını belirleyemiyor. ROPC yalnızca ortak istemci uygulamalar için desteklenir. |

## <a name="learn-more"></a>Daha fazla bilgi edinin

* ROPC kullanarak kendiniz için denemek [örnek konsol uygulaması](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2).
* V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md).
