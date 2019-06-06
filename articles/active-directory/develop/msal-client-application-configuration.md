---
title: İstemci uygulaması yapılandırması (Microsoft kimlik doğrulama kitaplığı) | Azure
description: Uygulamaları Microsoft Authentication Library (MSAL) genel istemci ve istemci gizli yapılandırma seçeneklerinin öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
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
ms.openlocfilehash: b5e175a8cdd1622add90bd80df63303fe914ab9c
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430815"
---
# <a name="application-configuration-options"></a>Uygulama yapılandırma seçenekleri

Kodunuzda, yeni genel veya gizli bir istemci (veya kullanıcı aracısı için MSAL.js) başlatma kimliğini doğrulamak ve belirteçlerini almak için uygulama. Microsoft Authentication Library (MSAL) istemci uygulamasını başlattığınızda, çeşitli yapılandırma seçeneklerini ayarlayabilirsiniz. Bu seçenekler ikiye ayrılır:

- Kayıt seçenekleri dahil olmak üzere:
    - [Yetkilisi](#authority) (Kimlik sağlayıcısı oluşan [örneği](#cloud-instance) ve oturum açma [İzleyici](#application-audience) uygulama ve büyük olasılıkla Kiracı kimliği için).
    - [İstemci kimliği](#client-id).
    - [Yeniden yönlendirme URI'si](#redirect-uri).
    - [İstemci gizli anahtarı](#client-secret) (için gizli istemci uygulamalar için).
- [Günlüğe kaydetme seçeneklerini](#logging)günlük düzeyi, kişisel verilerin denetimini ve kitaplığını kullanarak bileşen adı.

## <a name="authority"></a>Yetkilisi
MSAL belirteçleri isteyebileceği bir dizin belirten bir URL yetkilisidir. Ortak yetkilileri şunlardır:

- https://login.microsoftonline.com/&lt; Kiracı&gt;/ burada &lt;Kiracı&gt; Azure Active Directory (Azure AD) kiracısı ya da bu Azure AD Kiracı ile ilişkilendirilen bir etki alanı Kiracı Kimliğini gösterir. Yalnızca belirli bir kuruluşun kullanıcıları imzalamak için kullanılır.
- https://login.microsoftonline.com/common/. Kullanıcılar iş ve Okul hesapları veya kişisel Microsoft hesapları ile imzalamak için kullanılır.
- https://login.microsoftonline.com/organizations/. Kullanıcılar iş ve Okul hesapları ile imzalamak için kullanılır.
- https://login.microsoftonline.com/consumers/. Kullanıcılar (eski adıyla Windows Live ID hesabı olarak da bilinir) yalnızca kişisel Microsoft hesapları ile imzalamak için kullanılır.

Yetkilisi ayarı, uygulama kayıt Portalı'nda bildirilen ile tutarlı olması gerekiyor.

Yetkilisi URL'sini, örneği ve hedef kitle oluşur.

Yetkilisi olabilir:
- Bir Azure AD bulut yetkilisi.
- bir Azure AD B2C yetkilisi. Bkz: [B2C özellikleri](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics).
- Bir Active Directory Federasyon Hizmetleri (ADFS) yetkilisi. Bkz: [ADFS Destek](https://aka.ms/msal-net-adfs-support).

Azure AD bulut yetkilileri iki bölümden oluşur:
- Kimlik sağlayıcısı *örneği*
- Oturum açma *İzleyici* uygulama

Örneği ve hedef kitlesi birleştirilmiş ve yetkilisi URL'si sağlanmadı. Sürümlerinde MSAL.NET MSAL 3'ten önceki. *x*, yetkilisini kendiniz compose gerekiyordu, bulutta hedeflemek için değiştiriyorsanız ve oturum açma İzleyici göre.  Bu diyagram yetkilisi URL'sini nasıl oluşturulacağını gösterir:

![Yetkilisi URL'sini nasıl oluştuğunu](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Bulut örneği
*Örneği* kullanıcılar Azure genel bulutunda veya Ulusal bulut uygulamanızı imzalama, belirtmek için kullanılır. MSAL, kodunuzu kullanarak, Azure bulut örneği numaralandırması kullanarak veya URL'sini geçirme ayarlayabilirsiniz [Ulusal bulut örneği](authentication-national-cloud.md#azure-ad-authentication-endpoints) olarak `Instance` (biliyorsanız) üye.

Her iki MSAL.NET açık bir özel durum oluşturur `Instance` ve `AzureCloudInstance` belirtilir.

Bir örneği belirtmezseniz, uygulamanızı Azure genel bulut örneği hedeflediğini (URL örneği `https://login.onmicrosoftonline.com`).

## <a name="application-audience"></a>Uygulamanın hedef kitle

Oturum açma İzleyici uygulamanızın iş ihtiyaçlarına bağlıdır:
- Siz bir satır iş kolu (LOB) geliştiricisi gibiyseniz, büyük olasılıkla yalnızca kuruluşunuzdaki kullanılacak tek kiracılı bir uygulama oluşturmak. Bu durumda, kuruluş, Kiracı kimliği (Azure AD Örneğinize kimliği) veya Azure AD örneğiyle ilişkili bir etki alanı adı belirtmeniz gerekir.
- Bir ISV iseniz, herhangi bir kuruluştaki ya da (çok kiracılı uygulama) bazı kuruluşlar kendi iş ve Okul hesaplarına sahip kullanıcılar oturum açma isteyebilirsiniz. Ancak, kullanıcıların kendi kişisel Microsoft hesapları ile oturum isteyebilirsiniz.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Kod/yapılandırma İzleyici belirtme
MSAL kullanarak kodunuzda aşağıdaki değerlerden birini kullanarak hedef kitleye belirtin:
- Azure AD yetkilisi İzleyici numaralandırması
- Kiracı kimliği olabilir:
  - Tek kiracılı uygulamalar için bir GUID (Azure AD Örneğinize kimliği)
  - (Ayrıca tek kiracılı uygulamalar için) Azure AD örneğinizle ilgili bir etki alanı adı
- Kiracı kimliği yerine Azure AD yetkilisi İzleyici numaralandırması olarak bu yer tutucuları biri:
    - `organizations` çok kiracılı bir uygulama için
    - `consumers` Kullanıcıların kişisel hesaplarıyla yalnızca oturum açmak için
    - `common` Kullanıcılar kendi iş ve Okul hesapları veya kişisel Microsoft hesapları ile oturum açmak için

Azure AD yetkilisi İzleyici hem Kiracı kimliğini belirtirseniz MSAL'ın anlamlı bir özel durum oluşturur

Bir izleyici belirtmezseniz, uygulamanızı Azure AD'ye hedeflediğini ve bir hedef kitle olarak kişisel Microsoft hesapları. (Diğer bir deyişle, gibi ancak davranış göstereceği `common` belirtildi.)

### <a name="effective-audience"></a>Geçerli hedef kitle
Uygulamanız için geçerli İzleyici (bir kesişimi yoksa) en az uygulamanızda ayarladığınız Hedef kitleniz ve uygulama kaydında belirtilen bir izleyici olacaktır. Aslında, [uygulama kayıtları](https://aka.ms/appregistrations) deneyimini uygulamaya (desteklenen hesap türleri için) kitlesini belirtmenize olanak sağlar. Daha fazla bilgi için [hızlı başlangıç: Microsoft kimlik platformu bir uygulamayı kaydetme](quickstart-register-app.md).

Şu anda, hem de bu ayarları yapılandırmak için kullanıcıların kişisel Microsoft hesapları ile oturum açmak için bir uygulamayı almanın tek yolu verilmiştir:
- Uygulama kayıt İzleyici kümesine `Work and school accounts and personal accounts`.
- Kod/yapılandırma için hedef kitle kümesindeki `AadAuthorityAudience.PersonalMicrosoftAccount` (veya `TenantID` = "tüketicilerin").

## <a name="client-id"></a>İstemci Kimliği
Uygulamanıza uygulama kaydedilirken Azure AD tarafından atanan benzersiz uygulama (istemci) kimliği istemci kimliğidir.

## <a name="redirect-uri"></a>Yeniden yönlendirme URI'si
Yeniden yönlendirme URI'si, kimlik sağlayıcı güvenlik belirteçlerini geri gönderir URI'dir.

### <a name="redirect-uri-for-public-client-apps"></a>Genel istemci uygulamaları için yeniden yönlendirme URI'si
MSAL kullanarak bir ortak istemci uygulama Geliştirici kullanıyorsanız:
- Geçirmeniz gerekmez `RedirectUri` çünkü MSAL tarafından otomatik olarak hesaplanır. Bu yeniden yönlendirme URI'si platforma bağlı olarak bu değerlerden birine ayarlayın:
   - `urn:ietf:wg:oauth:2.0:oob` tüm Windows platformları için
   - `msal{ClientId}://auth` Xamarin Android ve iOS için

- Yeniden yönlendirme yapılandırmanız gereken URI [uygulama kayıtları](https://aka.ms/appregistrations):

   ![Uygulama kayıtları yeniden yönlendirme URI'si](media/msal-client-application-configuration/redirect-uri.png)

Kullanarak yeniden yönlendirme URI'si geçersiz kılabilirsiniz `RedirectUri` özelliği (örneğin, aracıları kullanın). Yeniden yönlendirme URI'leri ilgili senaryonun bazı örnekleri aşağıda verilmiştir:

- `RedirectUriOnAndroid` = "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos` = $"msauth.{Bundle.ID}://auth";

Ayrıntılar için bkz [Android ve iOS için belgeleri](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS).

### <a name="redirect-uri-for-confidential-client-apps"></a>Gizli istemci uygulamaları için yeniden yönlendirme URI'si
Web apps için yeniden yönlendirme URI'si (veya yanıt URI'si), Azure AD'nin belirteç uygulamasına geri göndermek için kullanacağı URI'dir. Gizli uygulama bunlardan biri ise bu web uygulaması/Web API URL'si olabilir. Yeniden yönlendirme URI'si uygulama kaydında kayıtlı olması gerekir. Başlangıçta yerel olarak test ettiğiniz bir uygulamayı dağıttığınızda, bu kayıt özellikle önemlidir. Ardından uygulama kayıt Portalı'nda dağıtılan uygulamanın yanıt URL'si eklemeniz gerekir.

Arka plan programı uygulamaları için bir yeniden yönlendirme URI'si belirtmeniz gerekmez.

## <a name="client-secret"></a>Gizli anahtar
Bu seçenek, gizli bir istemci uygulaması için gizli belirtir. Bu gizli dizi (uygulama parolası) uygulama kayıt portalı tarafından sağlanan veya uygulama kaydı sırasında Azure ad PowerShell AzureAD, PowerShell AzureRM veya Azure CLI ile sağlanan.

## <a name="logging"></a>Günlüğe kaydetme
Diğer yapılandırma seçenekleri, günlüğe kaydetme ve sorun giderme etkinleştirin. Bkz: [günlüğü](msal-logging.md) makale bunların nasıl kullanıldığı hakkında ayrıntılar için.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [istemci uygulamaları, MSAL.NET kullanarak örnekleme](msal-net-initializing-client-applications.md).

Hakkında bilgi edinin [MSAL.js kullanılarak istemci uygulamaları örnekleme](msal-js-initializing-client-applications.md).
