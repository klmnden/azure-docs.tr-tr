---
title: İstemci uygulaması yapılandırması (Microsoft kimlik doğrulama kitaplığı) | Azure
description: Uygulamaları Microsoft Authentication Library (MSAL) genel istemci ve istemci gizli yapılandırma seçeneklerinin öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a48eea9fedd2d82f44693d58b31ee0d5c8c288d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138544"
---
# <a name="application-configuration-options"></a>Uygulama yapılandırma seçenekleri

Kodunuzda, yeni genel veya gizli bir istemci (veya kullanıcı aracısı için MSAL.js) başlatma kimliğini doğrulamak ve belirteçlerini almak için uygulama.  MSAL istemci uygulamasında başlatılırken ayarlanabilir farklı yapılandırma seçeneği vardır. Bu seçenekler ikiye ayrılabilir:

- Kayıt seçenekleri dahil olmak üzere:
    - [Yetkilisi](#authority) (Kimlik sağlayıcısı oluşan [örneği](#cloud-instance) ve oturum açma [İzleyici](#application-audience) uygulama ve büyük olasılıkla Kiracı kimliği için).
    - [İstemci kimliği](#client-id)
    - [Yeniden yönlendirme URI'si](#redirect-uri)
    - [İstemci gizli anahtarı](#client-secret) (için gizli istemci uygulamalar için).
- [Günlüğe kaydetme seçeneklerini](#logging)günlük düzeyi, kişisel verilerin denetimini ve kitaplığını kullanarak bileşen adı.

## <a name="authority"></a>Yetkili
MSAL belirteçleri isteyebileceği bir dizin belirten bir URL yetkilisidir. Her zamanki yetkilileri şunlardır:

- https://login.microsoftonline.com/&lt; Kiracı&gt;/ burada &lt;Kiracı&gt; Azure AD kiracısı ya da bu Azure AD Kiracı ile ilişkilendirilen bir etki alanı Kiracı Kimliğini gösterir.  Yalnızca belirli bir kuruluşun kullanıcıları imzalamak için kullanılır.
- https://login.microsoftonline.com/common/. Kullanıcılar iş ve Okul hesapları veya kişisel Microsoft hesabı ile imzalamak için kullanılır.
- https://login.microsoftonline.com/organizations/. Kullanıcılar iş ve Okul hesapları ile imzalamak için kullanılır.
- https://login.microsoftonline.com/consumers/. Kullanıcılar yalnızca kişisel Microsoft hesabı (live) ile imzalamak için kullanılır.

Yetkilisi ayarı uygulama kayıt Portalı'nda bildirilen ile tutarlı olmalıdır.

Yetkilisi URL'sini, örneği ve hedef kitle oluşur.

Yetkilisi olabilir:
- bir Azure Active directory bulut yetkilisi
- bir Azure AD B2C yetkilisi. Bkz: [B2C özellikleri](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics).
- bir ADFS yetkilisi. Bkz: [ADFS Destek](https://aka.ms/msal-net-adfs-support).

Azure AD bulut yetkilileri iki bölümden oluşur:
- Kimlik sağlayıcısı **örneği**
- oturum açma **İzleyici** uygulama

Örneği ve hedef kitlesi birleştirilmiş ve yetkilisi URL'si sağlanmadı. MSAL önce MSAL.NET sürümlerinde 3.x tablonuz yetkilisini kendiniz hedef ve oturum açma İzleyici istediğiniz bulut bağlı olarak oluşturmak.  Aşağıdaki diyagramda yetkilisi URL'sini nasıl oluşturulacağını gösterir.

![Yetkili](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Bulut örneği
**Örneği** kullanıcılar Microsoft Azure ortak bulutuna veya Ulusal bulut uygulamanızı imzalama olmadığını belirtmek için kullanılır. MSAL, kodunuzu kullanarak, Azure bulut örneği numaralandırması kullanarak veya URL'sini geçirme ayarlanabilir [Ulusal bulut örneği](authentication-national-cloud.md#azure-ad-authentication-endpoints) olarak `Instance` (biliyorsanız) üye.

Her iki MSAL.NET açık bir özel durum oluşturur `Instance` ve `AzureCloudInstance` belirtilir. 

Bir örneği belirtmezseniz, uygulamanızı Azure genel bulut örneği hedeflediğini (URL örneği `https://login.onmicrosoftonline.com`).

## <a name="application-audience"></a>Uygulamanın hedef kitle

Oturum açma İzleyici uygulamanızın iş ihtiyaçlarına bağlıdır:
- İş kolu (LOB) geliştiricisi bir satır varsa, yalnızca kuruluşunuzda kullanılacak tek kiracılı bir uygulama, büyük olasılıkla oluşturmak. Bu durumda, bu kuruluşu, kendi Kiracı kimliği (Azure Active Directory kimliği) veya bu Azure Active Directory ile ilişkili bir etki alanı adı nedir belirtmeniz gerekir.
- Bir ISV iseniz, kullanıcıların iş kullanıcıların oturum açmasını isteyebilirsiniz ve Okul hesapları herhangi bir kuruluş veya bazı kuruluşların (çok kiracılı uygulama), ancak Ayrıca kullanıcıların kendi kişisel Microsoft hesabı ile oturum sahip olmak isteyebilirsiniz.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Kod/yapılandırma İzleyici belirtme
MSAL kullanarak kodunuzda kullanarak hedef kitleye belirtin:
- ya da Azure AD yetkilisi İzleyici sabit listesi. 
- ya da olabilir Kiracı kimliği:
  - GUID (Azure Active Directory kimliği), tek kiracılı uygulamalar için
  - Azure Active Directory (aynı zamanda tek kiracılı uygulamalar için) ile ilişkili bir etki alanı adı
- veya Kiracı kimliği yerine Azure AD yetkilisi İzleyici numaralandırması olarak bu yer tutucuları biri:
    - `organizations` çok kiracılı bir uygulama için
    - `consumers` Kullanıcıların kişisel hesaplarıyla yalnızca oturum açmak için
    - `common` Kullanıcılar kendi iş ve Okul hesabı veya kişisel Microsoft hesabı ile oturum açmak için

Azure AD yetkilisi İzleyici hem Kiracı kimliğini belirtirseniz MSAL'ın anlamlı bir özel durum oluşturur 

Bir izleyici belirtmezseniz uygulamanızı Azure AD'ye hedeflediğini ve bir hedef kitle olarak kişisel Microsoft hesapları (diğer bir deyişle `common`).

### <a name="effective-audience"></a>Geçerli hedef kitle
Uygulamanız için geçerli İzleyici (bir kesişimi yoksa) en az uygulamanızda ayarladığınız Hedef kitleniz ve uygulama kaydında belirtilen hedef kitle olacaktır. Aslında, uygulama kaydı deneyimini ([uygulama kaydı](https://aka.ms/appregistrations)) uygulama için hedef kitle (desteklenen hesap türleri için) belirtmenize olanak tanır. Bkz: [hızlı başlangıç: Microsoft kimlik platformu bir uygulamayı kaydetme](quickstart-register-app.md) daha fazla bilgi için.

Ayarlamak için şu anda, kullanıcılar yalnızca kişisel Microsoft hesapları ile oturum açmak için bir uygulamayı almanın tek yolu verilmiştir:
- uygulama kayıt İzleyici "İş ve Okul hesapları ve kişisel hesapları" ayarlayın ve
- ve İzleyici kodunuzda ayarlı / yapılandırmasına `AadAuthorityAudience.PersonalMicrosoftAccount` (veya `TenantID `"tüketicilerin" =)

## <a name="client-id"></a>İstemci Kimliği
Uygulamanıza uygulama kaydedilirken Azure AD tarafından atanmış benzersiz uygulama (istemci) kimliği.

## <a name="redirect-uri"></a>Yönlendirme URI'si
Yeniden yönlendirme URI'si, burada kimlik sağlayıcı güvenlik belirteçleri geri gönderir URI'dir. 

### <a name="redirect-uri-for-public-client-applications"></a>Genel istemci uygulamaları için yeniden yönlendirme URI'si
MSAL kullanarak genel bir istemci uygulaması geliştiricileri için:
- Geçirmeniz gerekmez ``RedirectUri`` gibi MSAL tarafından otomatik olarak hesaplanır. Bu yeniden yönlendirme URI'si, platforma bağlı olarak aşağıdaki değerlere ayarlanır:

- ``urn:ietf:wg:oauth:2.0:oob`` tüm Windows platformları için
- ``msal{ClientId}://auth`` Xamarin Android ve iOS için

Ancak, yeniden yönlendirme URI'si olarak yapılandırılması gerekir [uygulama kaydı](https://aka.ms/appregistrations).

![Portalda yeniden yönlendirme URI'si](media/msal-client-application-configuration/redirect-uri.png)

Yeniden yönlendirme URI'sini kullanarak geçersiz kılmak mümkündür `RedirectUri` özelliği örneği için aracıları kullanıyorsanız. Bu durumda yeniden yönlendirme URI'leri bazı örnekleri şunlardır:

- `RedirectUriOnAndroid` = "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos` = $"msauth.{Bundle.ID}://auth";

Android özellikleri ayrıntıları görmek için ve [iOS özellikleri](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS)

### <a name="redirect-uri-for-confidential-client-applications"></a>Gizli istemci uygulamaları için yeniden yönlendirme URI'si
Web apps için yeniden yönlendirme URI'si (veya yanıt URI'si), hangi Azure AD uygulama belirteci ile geri başvururlar URI oluyor. Bu Web uygulamasının URL'si olabilir / gizli bunlardan biri ise, Web API'si.  Bu yeniden yönlendirme URI'si uygulama kaydında kayıtlı olması gerekir. Başlangıçta yerel olarak test ettiğiniz bir uygulamayı dağıttığınızda bu seçenek özellikle önemlidir. Ardından uygulama kayıt Portalı'nda dağıtılan uygulamanın yanıt URL'si eklemeniz gerekir.

Arka plan programı uygulamaları için bir yeniden yönlendirme URI'si belirtmeniz gerekmez.

## <a name="client-secret"></a>Gizli anahtar
Gizli bir istemci uygulama istemci gizli anahtarı. Bu gizli dizi (uygulama parola), uygulama kayıt portalı tarafından sağlanan veya uygulama kaydı sırasında Azure AD PowerShell AzureAD, PowerShell AzureRM veya Azure CLI ile sağlanan.

## <a name="logging"></a>Günlüğe kaydetme
Diğer seçenekler, günlüğe kaydetme ve sorun giderme etkinleştirin. Daha fazla bilgi için [günlüğü](msal-logging.md) bunların nasıl kullanılacağı hakkında daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [MSAL.NET kullanarak istemci uygulamalarını örnekleme](msal-net-initializing-client-applications.md).

Hakkında bilgi edinin [MSAL.js kullanan istemci uygulamalar örnekleme](msal-js-initializing-client-applications.md).
