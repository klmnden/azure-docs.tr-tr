---
title: Korumalı Web API'sini - uygulama kaydı | Azure
description: Korumalı web API'si ve uygulamayı kaydetmek için gereken bilgileri oluşturmayı öğrenin.
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
ms.openlocfilehash: e4622cffedc159ce85166eafe571ccb26c2c1b4d
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536861"
---
# <a name="protected-web-api-app-registration"></a>Korumalı web API'sini: Uygulama kaydı

Bu makale, korumalı web API'si için uygulama kaydı ayrıntılarını açıklar.

Bkz: [hızlı başlangıç: Microsoft kimlik platformu bir uygulamayı kaydetme](quickstart-register-app.md) bir uygulamayı kaydetmeye yönelik yaygın adımlar için.

## <a name="accepted-token-version"></a>Belirteci kabul edilen sürüm

Microsoft kimlik platformu uç nokta, iki tür belirteçleri verebilir: v1.0 ve v2.0 belirteçler. Bu belirteçleri hakkında daha fazla bilgi için bkz: [erişim belirteçlerini](access-tokens.md). Kabul edilen belirteci sürüm bağımlı **desteklenen hesap türleri** uygulamanızı oluştururken seçtiğiniz:

- Varsa değerini **desteklenen hesap türleri** olduğu **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları (örneğin, Skype, Xbox, Outlook.com) hesaplarında**, kabul edilen belirteci sürüm v2.0 olacaktır.
- Aksi takdirde, kabul edilen belirteci sürüm v1.0 olacaktır.

Uygulamayı oluşturduktan sonra belirlemek ya da aşağıdaki adımları izleyerek kabul edilen bir belirteç sürüm değiştirin:

1. Azure portalında, uygulamanızı seçin ve ardından **bildirim** uygulamanız için.
2. Bildirimde, arama **"accessTokenAcceptedVersion"** . Değeri Not **2**. Bu özellik, Azure Active Directory (Azure AD) belirtir. web API'ın v2.0 belirteçleri kabul eder. Değer ise **null**, kabul edilen belirteci v1.0 sürümüdür.
3. Belirteç sürüm değiştirdiyseniz, seçin **Kaydet**.

> [!NOTE]
> Web API'si (v1.0 veya v2.0) belirteci hangi sürümü kabul ettiği belirtir. İstemcileri, web API'niz için bir belirteç Microsoft kimlik platformu v2.0 uç noktasından istediğinde, web API'si tarafından hangi sürümü kabul gösteren bir belirteç elde edersiniz.

## <a name="no-redirect-uri"></a>Yeniden yönlendirme URI'si

Web API'leri, hiçbir kullanıcı etkileşimli olarak imzalı olduğu için bir yeniden yönlendirme URI'si kaydolması gerekmez.

## <a name="expose-an-api"></a>Bir API'yi kullanıma sunmak

Başka bir web API'leri için belirli açık bir API ve sunulan kapsamları ayarıdır.

### <a name="resource-uri-and-scopes"></a>Kaynak URI'si ve kapsamları

Kapsamları misiniz genellikle biçiminde `resourceURI/scopeName`. Microsoft Graph için kısayolları gibi kapsamlar `User.Read`. Bu dize için bir kısayoldur `https://graph.microsoft.com/user.read`.

Uygulama kaydı sırasında bu parametreleri tanımlamanız gerekir:

- Kaynak URI'si. Uygulama kayıt portalı varsayılan olarak, önerir, kullanılacak `api://{clientId}`. Bu kaynak URI'si benzersiz ancak insan değil okunabilir. Bunu değiştirmek, ancak yeni değerin benzersiz olduğundan emin olun.
- Bir veya daha fazla *kapsamları*. (İstemci uygulamalar, bunlar olarak gösterilir *temsilci izinleri* web API'niz için.)
- Bir veya daha fazla *uygulama rolleri*. (İstemci uygulamalar, bunlar olarak gösterilir *uygulama izinleri* web API'niz için.)

Kapsam ayrıca uygulamanızın son kullanıcılara görüntülenen onay ekranında görüntülenir. Bu nedenle, kapsamı tanımlamak karşılık gelen dizeleri sağlamanız gerekir:

- Son kullanıcı tarafından görülen.
- Kiracı Yöneticisi olarak görülen, kimin yönetici onayı verebilir.

### <a name="exposing-delegated-permissions-scopes"></a>Temsilci izinleri (kapsamları) gösterme

1. Seçin **bir API'yi kullanıma sunmak** uygulama kaydı bölümünde.
1. **Kapsam ekle**’yi seçin.
1. İstenirse, önerilen uygulama kimliği URI'si kabul (`api://{clientId}`) seçerek **Kaydet ve devam et**.
1. Bu parametreleri girin:
      - İçin **kapsam adı**, kullanın **access_as_user**.
      - İçin **kimin onay**, emin **yöneticileri ve kullanıcılar** seçilir.
      - İçinde **yönetici onayı görünen adı**, girin **bir kullanıcı olarak erişim TodoListService**.
      - İçinde **yönetici onayı açıklaması**, girin **bir kullanıcı olarak TodoListService Web API'sine erişen**.
      - İçinde **kullanıcı onayı görünen adı**, girin **bir kullanıcı olarak erişim TodoListService**.
      - İçinde **kullanıcı onayı açıklaması**, girin **bir kullanıcı olarak TodoListService Web API'sine erişen**.
      - Tutun **durumu** kümesine **etkin**.
      - Seçin **kapsamı Ekle**.

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>Web API'NİZİN arka plan programı uygulama tarafından çağrılırsa

Bu bölümde, güvenli bir arka plan programları tarafından çağrılabilir için korumalı web API'si kaydetme öğreneceksiniz.

- kullanıma sunmak ihtiyacınız olacak *uygulama izinleri*. Temsilci izinleri anlamsız olmasını şekilde arka plan programları, kullanıcılarla etkileşimde bulunmaz çünkü yalnızca uygulama izinleri bildirmeniz.
- Kiracı yöneticileri, Azure Active Directory (Azure AD) erişim biri web API'SİNİN uygulama izinleri, kayıtlı uygulamalar için web API'niz için sorun belirteçleri gerektirebilir.

#### <a name="exposing-application-permissions-app-roles"></a>Uygulama izinleri (uygulama rolleri) gösterme

Uygulama izinleri kullanıma sunmak için bildirimi düzenleyin gerekecektir.

1. Uygulamanız için uygulama kaydında seçin **bildirim**.
1. Bularak bildirimi düzenleyin `appRoles` ayarlama ve bir veya daha fazla uygulama rolleri ekleme. Rol tanımı aşağıdaki örnek JSON bloğunda sağlanır. Bırakın `allowedMemberTypes` kümesine `"Application"` yalnızca. Emin `id` benzersiz bir GUID ve `displayName` ve `value` boşluk içermediğini.
1. Bildirim kaydedin.

Aşağıdaki örnek içeriğini gösterir `appRoles`. ( `id` Benzersiz bir GUID olabilir.)

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

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>Azure AD belirteçleri API'sine, istemciler yalnızca izin verilen web için sağlama

Web API'si uygulama rolü için denetler. (Uygulama izinleri kullanıma sunmak için geliştirici yolu değil.) Ancak, API'ye erişmek için Kiracı yönetici tarafından onaylanan uygulamalara web API'niz için bir belirteç vermek için Azure AD'ye de yapılandırabilirsiniz. Bu daha yüksek güvenlik eklemek için:

1. Uygulamasında **genel bakış** sayfasında, uygulama kaydı için bağlantı altında uygulamanızın adı ile **uygulama yerel dizinde yönetilen**. Bu alan için başlığının kesilmiş. Örneğin, görebilirsiniz **yönetilen uygulama...**

   > [!NOTE]
   >
   > Bu bağlantıyı seçtiğinizde, gideceğiniz **Kurumsal uygulamasına genel bakış** uygulamanızı oluşturduğunuz bu kiracıda hizmet sorumlusu ile ilişkili sayfası. Tarayıcınızın Geri düğmesini kullanarak uygulama kayıt sayfasına gidebilirsiniz.

1. Seçin **özellikleri** sayfasını **Yönet** Kurumsal uygulama sayfaları bölümü.
1. Azure AD, yalnızca belirli istemcilerden web API'nize erişim vermek istiyorsanız ayarlayın **kullanıcı ataması gerekli mi?** için **Evet**.

   > [!IMPORTANT]
   >
   > Ayarlarsanız **kullanıcı ataması gerekli mi?** için **Evet**, Azure AD web API'si için bir erişim belirteci istediklerinde, istemcilerin uygulama rolü atamalarını denetler. İstemcinin herhangi bir uygulama rolü atanmamış, Azure AD hata döndürecektir `invalid_client: AADSTS501051: Application <application name> is not assigned to a role for the <web API>`.
   >
   > Tutarsanız **kullanıcı ataması gerekli?** kümesine **Hayır**, *istemci web API'niz için bir erişim belirteci isteğinde bulunduğunda, Azure AD uygulama rolü atamalarını denetleyin olmaz*. Herhangi bir arka plan programı istemci (diğer bir deyişle, istemci kimlik bilgileri akışı kullanarak tüm istemci), hedef kitle belirterek API için bir erişim belirteci almak mümkün olacaktır. Herhangi bir uygulama için izin istemek zorunda kalmadan API'ye erişmek mümkün olacaktır. Ancak, web API'niz her zaman, önceki bölümde açıklandığı gibi uygulama (Kiracı yönetici tarafından yetkilendirilir) doğru rolüne sahip olduğunu doğrulayabilirsiniz. API erişim belirtecini bir rolü olan doğrulayarak bu doğrulamayı gerçekleştiren talep ve bu talep değeri doğru olduğunu. (Bu örnekte, değerdir `access_as_application`.)

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırma](scenario-protected-web-api-app-configuration.md)
