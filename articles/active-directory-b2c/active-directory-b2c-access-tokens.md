---
title: Erişim belirteçleri - Azure AD B2C isteyen | Microsoft Docs
description: Bu makalede bir istemci uygulaması kurulumu ve bir erişim belirteci edinmek üzere nasıl yapacağınızı gösterir.
services: active-directory-b2c
documentationcenter: android
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 08/09/2017
ms.author: davidmu
ms.openlocfilehash: bd919543072a8d2bf5fb0ebba17e69ba2f467218
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: İsteyen erişim belirteçleri

Bir erişim belirteci (olarak gösterilen **erişim\_belirteci** Azure AD B2C yanıtlarının içinde) tarafından güvence altına alınan bir istemci kaynaklara erişmek için kullanabileceğiniz güvenlik belirteci biçimidir bir [yetkilendirme sunucusu](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-protocols#the-basics), web API'si gibi. Erişim belirteci olarak temsil [Jwt'ler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#types-of-tokens) ve istenen kaynak sunucuda ve sunucuya verilen izinler hakkında bilgi içerir. Kaynak sunucuda çağrılırken erişim belirteci HTTP istek mevcut olması gerekir.

Bu makalede bir istemci uygulaması yapılandırın ve almak için web API'si nasıl ele bir **erişim\_belirteci**.

> [!NOTE]
> **Web API'si zincirleri (üzerinde-adına-ın) Azure AD B2C tarafından desteklenmiyor.**
>
> Birçok mimarisi web başka bir aşağı akış web API'si, hem Azure AD B2C tarafından güvence altına çağırmayı gerektiren API'si içerir. Bu senaryo, Azure AD grafik API'si gibi bir Microsoft online service sırayla çağıran bir web API'si arka ucuna sahip yerel istemcilerde yaygındır.
>
> Bu Zincirli web API'si senaryosu, aksi takdirde On-temsili akış bilinen OAuth 2.0 JWT taşıyıcı kimlik bilgisi grant kullanılarak desteklenebilir. Ancak, On-temsili akış şu anda Azure AD B2C'de uygulanmamıştır.

## <a name="register-a-web-api-and-publish-permissions"></a>Web API'si kaydetmek ve izinleri yayımlama

Bir erişim belirteci istemeden önce ilk web API'si kaydetmek ve istemci uygulamaya verilebilir izinleri (kapsam) yayımlama gerekir.

### <a name="register-a-web-api"></a>Web API’si kaydetme

1. Azure portalında Azure AD B2C özellikleri menüsünde tıklatın **uygulamaları**.
1. Tıklatın **+ Ekle** menüsünün üstünde.
1. Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. Örneğin, "Contoso API" girebilirsiniz.
1. **Web uygulamasını / web API’sini dahil et** anahtarını **Evet**’e getirin.
1. İçin rasgele bir değer girin **yanıt URL'leri**. Örneğin, `https://localhost:44316/` girin. Bir API belirteç doğrudan Azure AD B2C almalıdır değil bu yana değeri önemli değildir.
1. Bir **Uygulama Kimliği URI'si** girin. Bu değer, web API’niz için kullanılan tanımlayıcıdır. Örneğin, 'Not' kutuya girin. **Uygulama kimliği URI'si** sonra olacaktır `https://{tenantName}.onmicrosoft.com/notes`.
1. Uygulamanızı kaydetmek için **Create (Oluştur)** seçeneğine tıklayın.
1. Oluşturduğunuz uygulamaya tıklayın ve daha sonra kodunuzda kullanacağınız genel benzersiz **Uygulama İstemci Kimliğini** kopyalayın.

### <a name="publishing-permissions"></a>Yayımlama izinleri

Uygulamanızı bir API çağrılırken izinleri benzer kapsamları gereklidir. Bazı kapsamlar "Okuma" veya "yazma" olarak gösterilebilir. Web veya "API'den okumak için" Yerel uygulama istediğinizi varsayalım. Uygulamanızı Azure AD B2C çağırma ve "Okuma" kapsamına erişim sağlayan bir erişim belirteci isteyin. Bu tür bir erişim belirteci yaymak üzere Azure AD B2C için sırayla uygulamanın "belirli API'SİNDEN okuma" izni verilmiş olması gerekir. Bunu yapmak için API ilk "Okuma" kapsam yayımlamak gerekir.

1. Azure AD B2C içinde **uygulamaları** menüsünde, açık web API uygulamasını ("Contoso API").
1. **Yayımlanan kapsamlar**’a tıklayın. Burada diğer uygulamalara verilebilecek izinleri (kapsamları) tanımlarsınız.
1. Ekleme **kapsam değerleri** gerektiğinde (örneğin, "Okuma"). Varsayılan olarak, "user_impersonation" kapsamı tanımlanır. İsterseniz, bu göz ardı edebilirsiniz. Kapsam için bir açıklama girin **kapsam adı** sütun.
1. **Kaydet**’e tıklayın.

> [!IMPORTANT]
> **Kapsam adı** açıklaması **kapsam değeri**. Kapsam kullanırken kullandığınızdan emin olun **kapsam değeri**.

## <a name="grant-a-native-or-web-app-permissions-to-a-web-api"></a>Yerel vermek veya bir web API uygulama izni web

Bir API kapsamları yayımlamak için yapılandırıldıktan sonra istemci uygulaması bu kapsamları Azure Portalı aracılığıyla verilmiş olması gerekir.

1. Gidin **uygulamaları** Azure AD B2C özellikleri menü menüde.
1. Bir istemci uygulaması kaydetmek ([web uygulaması](active-directory-b2c-app-registration.md#register-a-web-app) veya [yerel istemci](active-directory-b2c-app-registration.md#register-a-mobile-or-native-app)) zaten yoksa. Bu kılavuz, başlangıç noktası olarak izliyorsanız, bir istemci uygulaması kaydetmeniz gerekir.
1. Tıklayın **API erişimini**.
1. **Ekle**'ye tıklayın.
1. Web API ve vermek istediğiniz kapsam (izinleri) seçin.
1. **Tamam**’a tıklayın.

> [!NOTE]
> Azure AD B2C, istemci uygulama kullanıcıları kendi onaylarının istemez. Bunun yerine, tüm onay, yukarıda açıklanan uygulamalar arasında yapılandırılmış izinlere dayalı olarak, yönetici tarafından sağlanır. Bir uygulama için bir izin verme iptal edilirse, bu izni almanız mümkün daha önce tüm kullanıcıların artık bunu mümkün olacaktır.

## <a name="requesting-a-token"></a>Bir belirteç isteme

İstemci uygulaması bir erişim belirteci isterken istenen izinler belirtilmesi gerekiyor **kapsam** isteğinin parametresi. Örneğin belirtmek için **kapsam değeri** "Okuma" sahip API için **uygulama kimliği URI'si** , `https://contoso.onmicrosoft.com/notes`, kapsam olacaktır `https://contoso.onmicrosoft.com/notes/read`. Bir yetkilendirme kodu isteği örneği aşağıdadır `/authorize` uç noktası.

> [!NOTE]
> Şu anda, özel etki alanlarını erişim belirteçleri ile birlikte desteklenmez. İstek URL'si tenantName.onmicrosoft.com etki alanınızda kullanmanız gerekir.

```
https://login.microsoftonline.com/<tenantName>.onmicrosoft.com/oauth2/v2.0/authorize?p=<yourPolicyId>&client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

Aynı istekte birden çok izinleri almak için birden çok girdi tek ekleyebileceğiniz **kapsam** boşluklarla ayırarak parametresi. Örneğin:

Kodunu çözdü URL'si:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

Kodlanmış URL'si:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

İstemci uygulamanız için verilen değerinden bir kaynak için daha fazla kapsam (izinleri) isteyebilir. Bu durumda, en az bir izin verilirse çağrı başarılı olur. Elde edilen **erişim\_belirteci** başarıyla verilmiş olan izinlerle doldurulmuş kendi "scp" talep sahip olur.

> [!NOTE] 
> İki farklı web kaynaklarına karşı isteyen izinleri aynı istekte desteklemiyoruz. Bu tür bir istek başarısız olur.

### <a name="special-cases"></a>Özel durumlar

Openıd Connect standart özel "scope" değerlerden belirtir. Aşağıdaki özel kapsamlar "kullanıcının profilini erişim" izni temsil eder:

* **openıd**: Bu bir kimliği belirteci istekleri
* **Çevrimdışı\_erişim**: Bu bir yenileme belirteci istekleri (kullanarak [kimlik doğrulama kodu akar](active-directory-b2c-reference-oauth-code.md)).

Varsa `response_type` parametresinde bir `/authorize` istek içerir `token`, `scope` parametresi en az bir kaynak kapsam içermelidir (dışında `openid` ve `offline_access`), verilecektir. Aksi takdirde `/authorize` istek bir hata ile sonlandırılacak.

## <a name="the-returned-token"></a>Döndürülen belirteç

Başarıyla minted içinde **erişim\_belirteci** (da `/authorize` veya `/token` uç nokta), aşağıdaki talep mevcut olacaktır:

| Ad | İste | Açıklama |
| --- | --- | --- |
|Hedef kitle |`aud` |**Uygulama kimliği** tek kaynağının erişim belirteci verir. |
|Kapsam |`scp` |Kaynak için izinler. Birden çok izin verilenler boşlukla ayrılmış. |
|Yetkili taraf |`azp` |**Uygulama kimliği** istek başlatılan istemci uygulaması. |

API'nizi zaman alan **erişim\_belirteci**, aşağıdakileri yapmalıdır [belirtecini doğrula](active-directory-b2c-reference-tokens.md) belirteç gerçek olduğunu ve doğru talep sahip kanıtlamak için.

Biz her zaman görüş ve öneriler için açık! Bu konu ile yaşarsanız etiketini kullanarak yığın taşması sonrası ['azure ad b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Özellik istekleri için bunları Ekle [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

## <a name="next-steps"></a>Sonraki adımlar

* Bir web API kullanarak yapı [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Bir web API kullanarak yapı [Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)
