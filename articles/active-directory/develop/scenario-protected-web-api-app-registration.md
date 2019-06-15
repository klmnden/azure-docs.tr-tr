---
title: Korumalı Web API'sini - uygulama kaydı | Azure
description: Korumalı Web API'si ve uygulamayı kaydetmek için gereken bilgileri oluşturmayı öğrenin.
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
ms.openlocfilehash: 22fe71c38678ae789a93ecbc956f24f0b0ebeb01
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111123"
---
# <a name="protected-web-api---app-registration"></a>Korumalı web API'si - uygulama kaydı

Bu makalede, korumalı web API'si için uygulama kaydı özellikleri açıklanmaktadır.

Bkz: [hızlı başlangıç: Microsoft kimlik platformu bir uygulamayı kaydetme](quickstart-register-app.md) uygulamanın nasıl kaydedileceği hakkında genel adımlar için.

## <a name="accepted-token-version"></a>Belirteci kabul edilen sürüm

Microsoft kimlik platformu uç nokta, iki tür belirteçleri verebilir: v1.0 ve v2.0 belirteçler. Bu belirteçleri hakkında daha fazla bilgi [erişim belirteçlerini](access-tokens.md). Kabul edilen belirteci sürüm bağımlı **desteklenen hesap türleri** uygulamanızı oluştururken seçtiğiniz:

- Varsa değerini **desteklenen hesap türleri** olduğu **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları (örneğin, Skype, Xbox, Outlook.com) hesaplarında**, kabul edilen belirteci sürüm v2.0 olacaktır.
- Aksi takdirde, kabul edilen belirteci sürüm v1.0 olacaktır.

Uygulamayı oluşturduktan sonra aşağıdaki adımları izleyerek kabul edilen belirteci sürüm değiştirebilirsiniz:

1. Azure portalında, uygulamanızı seçin ve ardından **bildirim** uygulamanız için.
2. Bildirimde, arama **"accessTokenAcceptedVersion"** , değeri görmek ve **2**. Bu özellik, Azure AD web API'si v2.0 belirteçleri kabul ettiğini bildiğiniz olanak tanır. Eğer öyleyse **null**, kabul edilen belirteci sürüm v1.0 olacaktır.
3. **Kaydet**’i seçin.

> [!NOTE]
> Bu web API'sine (v1.0 veya v2.0) belirteci hangi sürümü kabul ettiği karar aittir. İstemcileri, web API'sini kullanarak Microsoft kimlik platformu v2.0 uç noktası için bir belirteç istediğinde, web API'si tarafından hangi sürümü kabul gösteren bir belirteç elde edersiniz.

## <a name="no-redirect-uri"></a>Yeniden yönlendirme URI'si

Web API'leri, hiçbir kullanıcı, oturum açmış etkileşimli olarak olduğu gibi bir yeniden yönlendirme URI'si kaydolması gerekmez.

## <a name="expose-an-api"></a>Bir API'yi kullanıma sunmak

Başka bir web API'leri için belirli açık bir API ve sunulan kapsamları ayarıdır.

### <a name="resource-uri-and-scopes"></a>Kaynak URI'si ve kapsamları

Kapsamları genellikle şu biçimdedir `resourceURI/scopeName`. Microsoft Graph için kısayolları gibi kapsamlar `User.Read`, ancak bu dize yalnızca bir kısayol bulunur `https://graph.microsoft.com/user.read`.

Uygulama kaydı sırasında aşağıdaki parametreleri tanımlamanız gerekir:

- Uygulama kayıt portalı varsayılan kaynak URI'si - önerir, kullanılacak `api://{clientId}`. Bu kaynak URI'si benzersiz ancak insan değil okunabilir. Bunu değiştirmek, ancak bunun benzersiz olduğundan emin olun.
- Bir veya daha fazla **kapsamları** (istemci uygulamalar, bunlar olarak görünecek **temsilci izinleri** Web API'niz için)
- Bir veya daha fazla **uygulama rolleri** (istemci uygulamalar, bunlar olarak görünecek **uygulama izinleri** Web API'niz için)

Kapsam Ayrıca uygulamanızı kullanan son kullanıcılar için sunulan onay ekranında görüntülenir. Bu nedenle, kapsamı tanımlamak karşılık gelen dizeleri sağlamanız gerekir:

- Son kullanıcı tarafından görülen
- Kimlerin yönetici onayı verebilir Kiracı Yöneticisi tarafından görülen

### <a name="how-to-expose-delegated-permissions-scopes"></a>Temsilci izinleri (kapsamları) nasıl sunacağınızı öğrenin

1. Seçin **bir API'yi kullanıma sunmak** uygulama kaydı bölümünde ve:
   1. **Kapsam ekle**’yi seçin.
   1. İstenirse, önerilen uygulama kimliği URI'si kabul (API :// {ClientID}) seçerek **Kaydet ve devam et**.
   1. Aşağıdaki parametreleri girin:
      - İçin **kapsam adı**, kullanın `access_as_user`.
      - İçin **kimin onay**, emin olun **yöneticileri ve kullanıcılar** seçeneği belirlenir.
      - İçinde **yönetici onayı görünen adı**, türü `Access TodoListService as a user`.
      - İçinde **yönetici onayı açıklaması**, türü `Accesses the TodoListService Web API as a user`.
      - İçinde **kullanıcı onayı görünen adı**, türü `Access TodoListService as a user`.
      - İçinde **kullanıcı onayı açıklaması**, türü `Accesses the TodoListService Web API as a user`.
      - Tutun **durumu** kümesine **etkin**.
      - Seçin **kapsamı Ekle**.

### <a name="case-where-your-web-api-is-called-by-daemon-application"></a>Web API'NİZİN arka plan programı uygulaması tarafından çağrılan olduğu durum

Bu bir paragraf içinde böylece güvenli bir arka plan programı uygulamaları tarafından çağrılabilir, korumalı Web API'si kaydetme öğreneceksiniz:

- kullanıma sunmak ihtiyacınız olacak **uygulama izinleri**. Uygulama izinleri, arka plan programı uygulamaları kullanıcılarla etkileşimde bulunmazlar ve bu nedenle temsilci izinleri anlamlı olmaz yalnızca bildirir.
- Kiracı yöneticileri, Web uygulamanız için yalnızca bir Web API uygulamaları veya erişmek istediğiniz kayıtlı uygulamalar için Azure AD'ye sorunu belirteçleri gerektirebilir.

#### <a name="how-to-expose-application-permissions-app-roles"></a>Uygulama izinleri (uygulama rolleri) nasıl sunacağınızı öğrenin

Uygulama izinleri kullanıma sunmak için bildirimi düzenleyin gerekecektir.

1. Uygulamanız için uygulama kaydında tıklayın **bildirim**.
1. Bularak bildirimi düzenleyin `appRoles` ayarlama ve bir veya birden çok uygulama rolleri ekleme. Rol tanımı aşağıdaki örnek JSON bloğunda sağlanır.  Bırakın `allowedMemberTypes` "Uygulama" yalnızca. Lütfen emin olun **kimliği** benzersiz bir GUID'dir ve **displayName** ve **değer** boşluk içermeyen.
1. Bildirim kaydedin.

İçeriği `appRoles` aşağıdaki gibi olmalıdır ( `id` benzersiz bir GUID olabilir)

```JSon
"appRoles": [
    {
    "allowedMemberTypes": [ "Application" ],
    "description": "Accesses the TodoListService-Cert as an application.",
    "displayName": "access_as_application",
    "id": "ccf784a6-fd0c-45f2-9c08-2f9d162a0628",
    "isEnabled": true,
    "lang": null,
    "origin": "Application",
    "value": "access_as_application"
    }
],
```

#### <a name="how-to-ensure-that-azure-ad-issues-tokens-for-your-web-api-only-to-allowed-clients"></a>Azure AD Web API'niz için yalnızca belirteçleri emin olmak nasıl izin istemcileri

Web API (yani yapmakta Geliştirici yol) uygulama rolü için denetler. Ancak, API'ye erişmek için Kiracı yönetici tarafından onaylanan uygulamalara, Web API'niz için bir belirteç vermek üzere Azure Active Directory bile yapılandırabilirsiniz. Bu ek güvenlik eklemek için:

1. Uygulamasında **genel bakış** sayfasında, uygulama kaydı için uygulamanızın adıyla köprüyü seçin **uygulama yerel dizinde yönetilen**. Bu alan için başlığının kesilmiş. Örneğin, okuma olabilir: `Managed application in ...`

   > [!NOTE]
   >
   > Gitmek için bu bağlantıyı seçtiğinizde **Kurumsal uygulamasına genel bakış** uygulamanızı oluşturduğunuz bu kiracıda hizmet sorumlusu ile ilişkili sayfası. Tarayıcınızın Geri düğmesini kullanarak uygulama kayıt sayfasına gidebilirsiniz.

1. Seçin **özellikleri** sayfasını **Yönet** bölümünü Kurumsal uygulama sayfaları
1. AAD Web API'nize erişim yalnızca belirli istemcilerden uygulamak istiyorsanız ayarlayın **kullanıcı ataması gerekli mi?** için **Evet**.

   > [!IMPORTANT]
   >
   > Ayarlayarak **kullanıcı ataması gerekli mi?** için **Evet**, Web API'si için bir erişim belirteci istediklerinde, AAD uygulama rolü atamalarını istemcilerin denetler. İstemci olmayan olması atanma, tüm AppRoles için AAD yalnızca şu hatayı döndürür: `invalid_client: AADSTS501051: Application xxxx is not assigned to a role for the xxxx`
   >
   > Tutarsanız **kullanıcı ataması gerekli mi?** için **Hayır**, <span style='background-color:yellow; display:inline'>bir istemci, Web API'niz için bir erişim belirteci isteğinde bulunduğunda, Azure AD uygulama rolü atamalarını denetleyin olmaz</span>. Bu nedenle, (yani istemci kimlik bilgileri akışını kullanarak herhangi bir istemci) herhangi bir arka plan programı istemci hala, İzleyici belirterek API için bir erişim belirteci almak hazırdır. Herhangi bir uygulama olacaktır API istek izinlerine gerek kalmadan erişebilir. Web API'nizi her zaman, sonraki bölümde açıklandığı gibi erişim belirtecine sahip olduğunu doğrulayarak uygulama (Bu Kiracı Yöneticisi tarafından yetkilendirildi) doğru rolüne sahip olduğunu doğrulayabilirsiniz gibi artık, bu ardından, sonuna değil bir `roles` talebi ve doğru t değeri kendi talep (bizim durumumuzda `access_as_application`).

1. Seçin **Kaydet**

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırma](scenario-protected-web-api-app-configuration.md)
