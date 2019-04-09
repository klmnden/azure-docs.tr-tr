---
title: Azure AD v2.0 tarafından desteklenen yetkilendirme protokolleri hakkında bilgi edinin | Microsoft Docs
description: Azure AD v2.0 uç noktası tarafından desteklenen protokolleri bir kılavuz.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c56970091da74cfc389d60ad91f430fcb64d4bba
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266979"
---
# <a name="v20-protocols---oauth-20-and-openid-connect"></a>v2.0 protokolleri - OAuth 2.0 ve Openıd Connect

V2.0 uç noktası kimlik-bir hizmet olarak sektör standardı protokolleri, Openıd Connect ve OAuth 2.0 ile Azure Active Directory (Azure AD) kullanabilirsiniz. Hizmet standartlara uygun olsa da, bu protokolleri herhangi iki uygulamaları arasındaki farklar olabilir. Buradaki bilgileri kodunuzu doğrudan gönderme ve HTTP isteklerini işlemek yazın veya bir üçüncü taraf açık kaynak kitaplığı kullanmayı tercih ederseniz yararlı olacak birini kullanmak yerine bizim [açık kaynak kitaplıkları](reference-v2-libraries.md).

> [!NOTE]
> Bazı Azure AD senaryoları ve özellikleri v2.0 uç noktasında desteklenmez. V2.0 uç noktası kullanıyorsanız belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Temel bilgiler

Neredeyse tüm OAuth 2.0 ve Openıd Connect akışlar Exchange'de kullanılan dört taraflar vardır:

![OAuth 2.0 rolleri](../../media/active-directory-v2-flows/protocols_roles.png)

* **Yetkilendirme sunucusu** v2.0 uç nokta ve sorumlu kullanıcının kimliğini sağlamaya yönelik, belirteçleri verme ve kaynaklara erişimi iptal etme ve verme. Yetkilendirme sunucusu kimlik sağlayıcısı olarak da bilinen - onu güvenli bir şekilde kullanıcının bilgileri, bunların erişim ve bir akışta taraflar arasındaki güven ilişkilerinin ile yapmak için herhangi bir şey işler.
* **Kaynak sahibi** genellikle son kullanıcısıdır. Verilerin sahibi ve üçüncü tarafların, veri veya kaynak erişim izin vermek için güç olan taraftır.
* **OAuth istemcisi** uygulamanız, kendi uygulama kimliği tarafından belirtilen OAuth istemcisi genellikle son kullanıcının etkileşime taraf ve yetkilendirme sunucusundan belirteçleri ister. İstemci kaynak sahibi tarafından kaynağa erişim izni verilmesi gerekir.
* **Kaynak sunucusu** kaynağı veya veri bulunduğu olduğu. Güvenler güvenli bir şekilde kimlik doğrulaması ve OAuth istemci yetki vermek için yetkilendirme sunucusu ve taşıyıcı erişim belirteçleri, bir kaynağa erişim izni sağlamak için kullanır.

## <a name="app-registration"></a>Uygulama kaydı

Hem kişisel ve iş veya Okul hesaplarını almayı isteyen her uygulama, yeni kaydedilmelidir **uygulama kayıtları (Önizleme)** deneyimini [Azure portalında](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) önce bu kullanıcılar oturum açabilir OAuth 2.0 veya Openıd Connect kullanarak. Uygulama kayıt işlemi, toplamak ve uygulamanız için bazı değerler atayın:

* Bir **uygulama kimliği** uygulamanızı benzersiz şekilde tanımlayan
* A **yeniden yönlendirme URI'si** veya **paket tanımlayıcısı** yanıtları uygulamanıza geri yönlendirmek için kullanılabilir
* Diğer birkaç senaryoya özel değerler.

Daha ayrıntılı bilgi için [uygulama kaydetmeyi](quickstart-v2-register-an-app.md) öğrenin.

## <a name="endpoints"></a>Uç Noktalar

Kaydedildikten sonra uygulama v2.0 uç noktasına istek göndererek Azure AD ile iletişim kurar:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Burada `{tenant}` dört farklı değerden birini alabilir:

| Değer | Açıklama |
| --- | --- |
| `common` | Kullanıcıların hem kişisel Microsoft hesapları hem de iş/Okul hesapları ile Azure AD uygulamasına oturum açmak için izin verir. |
| `organizations` | Yalnızca iş/Okul hesabı olan kullanıcılar Azure AD uygulamasına oturum açmak için izin verir. |
| `consumers` | Kişisel Microsoft hesapları (MSA) uygulamasına oturum açmak için yalnızca bir kullanıcılarla sağlar. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` or `contoso.onmicrosoft.com` | Uygulamasına oturum açmak için yalnızca belirli bir Azure ad iş/Okul hesabı olan kullanıcılar Kiracı sağlar. Azure AD Kiracı kolay etki alanı adını veya kiracının GUID tanımlayıcısı kullanılabilir. |

Bu uç noktaları ile etkileşim öğrenmek için bir belirli uygulama türü seçin [protokolleri](#protocols) bölümünde ve daha fazla bilgi için bağlantıları izleyin.

> [!TIP]
> Kişisel hesaplarında oturum yoksa bile Azure AD'de kayıtlı herhangi bir uygulama, v2.0 uç noktası kullanabilirsiniz.  Bu şekilde, v2.0 için mevcut uygulamaları geçirme ve [MSAL](reference-v2-libraries.md) uygulamanızı yeniden oluşturmadan.  

## <a name="tokens"></a>Belirteçler

OAuth 2.0 ve Openıd Connect v2.0 uygulamasını taşıyıcı belirteçler, taşıyıcı belirteçleri Jwt'ler temsil dahil olmak üzere kapsamlı kullanımını olun. Taşıyıcı belirteç korumalı bir kaynağın "bearer" erişim veren bir basit güvenlik belirtecidir. Bu anlamda belirteç sunabilir herhangi bir tarafa "bearer" olur. Gerekli adımları iletilmesini ve depolanmasını belirteci güvenliğini sağlamak için alınır değil, bir tarafın ilk taşıyıcı belirteç almak için Azure AD kimlik doğrulaması gerekir ancak kesildi ve istenmeyen bir şahıs tarafından kullanılır. Bazı güvenlik belirteçleri yetkisiz taraflar bunları tüketmesini için yerleşik bir mekanizma olsa da, taşıyıcı belirteçleri Bu mekanizma yoktur ve Aktarım Katmanı Güvenliği (HTTPS) gibi güvenli bir kanal taşınan gerekir. Açık bir şekilde bir taşıyıcı belirteç iletilirse, kötü amaçlı bir taraf, belirteci almak ve korunan bir kaynağa yetkisiz erişim için kullanmak üzere bir adam-de-ortadaki adam saldırısı kullanabilirsiniz. Depolama veya daha sonra kullanmak için taşıyıcı belirteçlerini önbelleğe alma aynı güvenlik ilkeleri uygulayın. Uygulamanızı iletir ve güvenli bir şekilde taşıyıcı belirteçleri depolar her zaman emin olmalısınız. Taşıyıcı belirteçleri hakkında daha fazla güvenlik konuları için bkz. [RFC 6750 bölüm 5](https://tools.ietf.org/html/rfc6750).

Daha ayrıntılı bilgi v2.0 uç noktası kullanılan belirteçlerin farklı türdeki kullanılabilir [v2.0 uç noktası belirteç başvurusu](v2-id-and-access-tokens.md).

## <a name="protocols"></a>Protokoller

Bazı örnek isteklerini görmek hazırsanız, biri ile çalışmaya başlama öğreticileri aşağıda. Her biri için bir belirli kimlik doğrulama senaryosu karşılık gelir. Sizin için doğru akışı olduğu saptarken yardıma ihtiyacınız varsa, kullanıma [derleme v2.0 uygulama türlerini](v2-app-types.md).

* [OAuth 2.0 ile Mobil ve yerel uygulaması oluşturma](v2-oauth2-auth-code-flow.md)
* [Openıd Connect ile Web uygulamaları oluşturun](v2-protocols-oidc.md)
* [OAuth 2.0 örtük akışını kullanarak tek sayfalı uygulamalar oluşturun](v2-oauth2-implicit-grant-flow.md)
* [Daemon'ları veya OAuth 2.0 istemci kimlik bilgileri akışı ile sunucu tarafı işlemleri](v2-oauth2-client-creds-grant-flow.md)
* [OAuth 2.0 on-behalf-of akışı ile bir Web API belirteçleri Al](v2-oauth2-on-behalf-of-flow.md)
