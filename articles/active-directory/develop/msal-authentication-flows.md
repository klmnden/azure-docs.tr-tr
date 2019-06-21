---
title: Kimlik doğrulama akışları (Microsoft kimlik doğrulama kitaplığı) | Azure
description: Microsoft Authentication Library (MSAL) tarafından kullanılan verir ve kimlik doğrulama akışları hakkında bilgi edinin.
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
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 821143d39f8a4c06501ee38ef598a9d06d267d72
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273106"
---
# <a name="authentication-flows"></a>Kimlik doğrulama akışları

Bu makalede, Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından sağlanan farklı kimlik doğrulama akışları açıklanır.  Bu akış, farklı uygulama senaryolarında çeşitli kullanılabilir.

| Akış | Açıklama | Kullanılan|  
| ---- | ----------- | ------- | 
| [Etkileşimli](#interactive) | Kullanıcıdan kimlik bilgilerini tarayıcı veya açılır penceresi ile etkileşimli bir işlem aracılığıyla belirteç alır. | [Masaüstü uygulamaları](scenario-desktop-overview.md), [mobil uygulamalar](scenario-mobile-overview.md) |
| [Örtülü izin](#implicit-grant) | Arka uç sunucu kimlik bilgisi alışverişinin yapmadan belirteçlerini almak okumasına izin verir. Bu, kullanıcının oturumunu, oturumu korumak ve istemcideki tüm diğer web API'leri belirteçleri JavaScript kodu alma okumasına izin verir.| [Tek sayfa uygulamaları (SPA)](scenario-spa-overview.md) |
| [Yetkilendirme kodu](#authorization-code) | Web API'leri gibi korunan kaynakları erişim kazanmak için cihazda yüklü uygulamalar kullanılır. Bu, mobil ve Masaüstü uygulamaları için oturum açma ve API erişimi eklemenize olanak sağlar. | [Masaüstü uygulamaları](scenario-desktop-overview.md), [mobil uygulamalar](scenario-mobile-overview.md), [web uygulamaları](scenario-web-app-call-api-overview.md) | 
| [On-behalf-of](#on-behalf-of) | Bir hizmet ya da başka bir hizmete çağrı yapma veya web API'si için sırayla gereken web API'si, bir uygulamayı çağırır. İstek zincirinin aracılığıyla izinleri ve yetkilendirilmiş kullanıcının kimlik yayılması olur. | [Web API'leri](scenario-web-api-call-api-overview.md) |
| [İstemci kimlik bilgileri](#client-credentials) | Uygulama kimliğini kullanarak web bulunan kaynaklara erişim sağlar. Hemen bir kullanıcı etkileşimi olmadan arka planda çalışması gereken sunucudan sunucuya etkileşimleri için yaygın olarak kullanılır. | [arka plan programları](scenario-daemon-overview.md) |
| [Cihaz kodu](#device-code) | Giriş kısıtlı cihazları akıllı TV gibi IOT cihaz veya yazıcı oturum açmalarını sağlar. | [Masaüstü/mobile apps](scenario-desktop-acquire-token.md#command-line-tool-without-web-browser) |
| [Tümleşik Windows kimlik doğrulaması](scenario-desktop-acquire-token.md#integrated-windows-authentication) | Sessizce (olmadan herhangi bir kullanıcı Arabirimi etkileşimi kullanıcıdan) bir belirteç almak etki alanı veya Azure Active Directory (Azure AD) uygulama alanına katılmış bilgisayarların sağlar.| [Masaüstü/mobile apps](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [Kullanıcı adı/parola](scenario-desktop-acquire-token.md#username--password) | Bir uygulamanın doğrudan parolalarını işleyerek kullanıcının oturum açmasını sağlar. Bu akış önerilmez. | [Masaüstü/mobile apps](scenario-desktop-acquire-token.md#username--password) | 

## <a name="interactive"></a>Etkileşimli
MSAL, etkileşimli olarak oturum açın ve bu kimlik bilgileri kullanılarak bir belirteç almak için kimlik bilgilerini girmesini özelliğini destekler.

![Etkileşimli Akış Diyagramı](media/msal-authentication-flows/interactive.png)

Etkileşimli olarak belirli platformlarda belirteçlerini almak için MSAL.NET kullanarak daha fazla bilgi için bkz:
- [Xamarin Android](msal-net-xamarin-android-considerations.md)
- [Xamarin iOS](msal-net-xamarin-ios-considerations.md)
- [Evrensel Windows Platformu](msal-net-uwp-considerations.md)

MSAL.js etkileşimli çağrılar hakkında daha fazla bilgi için bkz. [istemi davranışı MSAL.js etkileşimli isteklerinde](msal-js-prompt-behavior.md).

## <a name="implicit-grant"></a>Örtülü izin

MSAL destekler [2 OAuth örtük izin akışı](v2-oauth2-implicit-grant-flow.md), belirteçleri Microsoft kimlik platformu ' bir arka uç sunucu gerçekleştirmeden kimlik bilgisi alışverişinin ve uygulamayı sağlar. Bu, kullanıcının oturumunu, oturumu korumak ve istemcideki tüm diğer web API'leri belirteçleri JavaScript kodu alma okumasına izin verir.

![Örtük verme akışı diyagramı](media/msal-authentication-flows/implicit-grant.svg)

Birçok modern web uygulamaları, JavaScript veya Angular ve Vue.js React.js gibi bir SPA framework kullanılarak yazılan istemci tarafı ve tek sayfa uygulamaları olarak oluşturulur. Bu uygulamalar, bir web tarayıcısında çalıştırma ve geleneksel sunucu tarafı web uygulamaları'den farklı kimlik doğrulama özellikleri vardır. Tek sayfalı uygulama kullanıcılarının oturumunu ve örtük verme akışı kullanarak arka uç hizmetlerine veya web API'leri ve erişim belirteçleri almak Microsoft kimlik platformu sağlar. Örtük akış kimliği kimliği doğrulanmış kullanıcı temsil eder ve korunan API'leri çağırmak için gereken belirteçlerini ayrıca erişim belirteçleri elde etmek için uygulamaya izin verir.

Bunlar daha özellikleri yerel platformları ile etkileşim gerektirdiğinden, bu kimlik doğrulama akışı Elektron ve React-Native gibi platformlar arası JavaScript çerçeveleri kullanan uygulama senaryoları içermez.

## <a name="authorization-code"></a>Yetkilendirme kodu
MSAL destekler [OAuth 2 yetkilendirme kodu verme](v2-oauth2-auth-code-flow.md). Bu izin, web API'leri gibi korunan kaynakları erişim kazanmak için cihazda yüklü olan uygulamalarda kullanılabilir. Bu, mobil ve Masaüstü uygulamaları için oturum açma ve API erişimi eklemenize olanak sağlar. 

Web uygulaması, web uygulamaları (Web siteleri) kullanıcılar oturum açtığında bir yetkilendirme kodu alır.  Yetkilendirme kodu, web API'leri çağırmak için bir belirteç almak için kullanılan. ASP.NET ve ASP.NET Core web uygulamalarında tek amacı, `AcquireTokenByAuthorizationCode` belirteç önbelleği için bir belirteç eklemektir. Belirteç daha sonra uygulama tarafından kullanılabilir (genellikle denetleyicileri, yalnızca bir belirteç için bir API kullanarak elde `AcquireTokenSilent`).

![Yetkilendirme kodu akışı diyagramı](media/msal-authentication-flows/authorization-code.png)

Yukarıdaki diyagramda, uygulama:

1. Bir erişim belirteci için kullanılan bir yetkilendirme kodu ister.
2. Bir web API'sini çağırmak için erişim belirtecini kullanır.

### <a name="considerations"></a>Dikkat edilmesi gerekenler
- Bir belirteç kullanmak için yetkilendirme kodu yalnızca bir kez kullanabilirsiniz. (Açıkça standart protokol belirtimi tarafından yasaklanmış) birden çok kez aynı yetkilendirme kodu ile bir belirteç almak çalışmayın. Size kodu birkaç kez kasıtlı olarak kullanmak ya da bir çerçeve de bunu sizin için halleder, farkında olmadığından, şu hatayı alırsınız: `AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- Bir ASP.NET veya ASP.NET Core uygulaması yazıyorsanız, çerçeve, söyleme, yetkilendirme kodu zaten kullanmışsınız gerçekleşebilir. Bunun için çağırmanız gerekir `context.HandleCodeRedemption()` yöntemi `AuthorizationCodeReceived` olay işleyicisi.

- Erişim belirteci doğru gerçekleştiği artımlı onay engelleyebilir ASP.NET ile paylaşmaktan kaçının. Daha fazla bilgi için [sorun #693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693).

## <a name="on-behalf-of"></a>On-behalf-of

MSAL destekler [OAuth 2 on-behalf-of kimlik doğrulama akışı](v2-oauth2-on-behalf-of-flow.md).  Bu akış, bir hizmet ya da başka bir hizmete çağrı yapma veya web API'si için sırayla gereken web API'si, bir uygulamayı çağırır olduğunda kullanılır. İstek zincirinin aracılığıyla izinleri ve yetkilendirilmiş kullanıcının kimlik yayılması olur. Orta katman hizmet aşağı akış hizmeti için kimliği doğrulanmış isteğinde bulunmak Microsoft kimlik platformu, kullanıcı adına bir erişim belirteci güvenliğini sağlamak gerekir.

![On-behalf-of akışı diyagramı](media/msal-authentication-flows/on-behalf-of.png)

Yukarıdaki şemada:

1. Uygulama, web API'si için bir erişim belirteci alır.
2. Bir istemci (web, masaüstü, mobil veya tek sayfalı uygulama) korumalı web API'si, HTTP isteğinin kimlik doğrulama üst bilgisindeki bir taşıyıcı belirteç olarak erişim belirteci ekleme çağırır. Web API'si, kullanıcının kimliğini doğrular.
3. İstemci web API'si çağırdığında, web API'si kullanıcı farklı bir belirteç on-behalf-of ister.  
4. Korumalı web API'sini kullanıcı on-behalf-of bir aşağı akış web API'sini çağırmak için bu belirteci kullanır.  Web API'si aynı zamanda daha sonra istek belirteçlerini diğer aşağı akış API'leri (ancak yine de aynı kullanıcı adına).

## <a name="client-credentials"></a>İstemci kimlik bilgileri

MSAL destekler [OAuth 2 istemci kimlik bilgileri akışı](v2-oauth2-client-creds-grant-flow.md). Bu akış bir uygulamanın kimliğini kullanarak web bulunan kaynaklara erişim sağlar. Bu tür bir verme, hemen bir kullanıcı etkileşimi olmadan arka planda çalışması gereken sunucudan sunucuya etkileşimleri için yaygın olarak kullanılır. Bu tür uygulamalar, Daemon'ları veya hizmet hesapları da anılır. 

İstemci kimlik bilgileri, başka bir web hizmetini çağırırken bir kullanıcının kimliğine bürünmek yerine kendi kimlik bilgilerini kullanmak için bir web hizmetinin (gizli bir istemci) akış izinleri verin. Bu senaryoda istemci genellikle bir orta katman web hizmeti, bir arka plan programı hizmeti veya Web sitesi olur. Daha yüksek bir güvence düzeyi için Microsoft identity platformuna (yerine, paylaşılan gizlilik) bir sertifika bir kimlik bilgisi olarak kullanılacak arama hizmeti de sağlar.

> [!NOTE]
> Bunlar yalnızca ortak istemci uygulamaları desteklemediğinden, gizli istemci akışı mobil platformlarda (UWP, Xamarin.iOS ve Xamarin.Android) kullanılamaz. Genel istemci uygulamaları, uygulamanın kimliğini kanıtlamak için kimlik sağlayıcısı nasıl yapılandıracağımı bilmiyorum. Güvenli bir bağlantı üzerinde web uygulaması elde edilebilir veya web API geri sertifika dağıtarak sona erer.

İki tür istemci kimlik bilgileri MSAL.NET destekler. Bu istemci kimlik bilgileri Azure AD'ye kayıtlı olması gerekir. Kimlik bilgileri gizli bir istemci uygulama kodunuzda oluşturucuları geçirilir.

### <a name="application-secrets"></a>Uygulama gizli dizilerini 

![Parola ile gizli istemci diyagramı](media/msal-authentication-flows/confidential-client-password.png)

Yukarıdaki diyagramda, uygulama:

1. Uygulama gizli anahtarı veya parola kimlik bilgilerini kullanarak bir belirteç alır.
2. Kaynak isteğinde bulunmak için bu belirteci kullanır.

### <a name="certificates"></a>Sertifikalar 

![Gizli bir istemci sertifikası ile diyagramı](media/msal-authentication-flows/confidential-client-certificate.png)

Yukarıdaki diyagramda, uygulama:

1. Sertifika kimlik bilgileri kullanılarak bir belirteç alır.
2. Kaynak isteğinde bulunmak için bu belirteci kullanır.

Bu istemci kimlik bilgileri gerekir:
- Azure AD'ye kayıtlı.
- Gizli bir istemci uygulama kodunuzda yapımı sırasında geçirildi.


## <a name="device-code"></a>Cihaz kodu
MSAL destekler [OAuth 2 cihaz kod akışını](v2-oauth2-device-code.md), akıllı TV gibi giriş kısıtlı cihazları, IOT cihaz veya yazıcı oturum açmalarını sağlar. Etkileşimli kimlik doğrulaması Azure AD ile bir web tarayıcısı gerektirir. Cihaz kod akışını etkileşimli olarak burada cihaz veya işletim sistemi bir web tarayıcısı sağlamaz oturum açmak için (başka bir bilgisayara veya bir cep telefonu gibi) başka bir cihaz kullanın izin verir.

Cihaz kod akışı kullanarak, uygulama belirteçleri bu cihazlar ya da işletim sistemleri için özellikle tasarlanmış iki adımlı bir işlemle alır. IOT cihazları veya komut satırı araçlarını (CLI) üzerinde çalışan bu uygulamaların örnekleri içerir. 

![Cihaz kod Akış Diyagramı](media/msal-authentication-flows/device-code.png)

Yukarıdaki şemada:

1. Kullanıcı kimlik doğrulaması gerekli olduğunda, uygulama bir kod sağlar ve kullanıcıdan bir URL'ye gidin (örneğin, bir İnternet'e bağlı smartphone) başka bir cihaz kullanın (örneğin, https://microsoft.com/devicelogin). Kullanıcı, ardından kodunu girmesi istenir ve gerekirse çok faktörlü kimlik doğrulaması ve onay istekleri dahil olmak üzere, bir normal kimlik doğrulaması deneyimi devam eder.

2. Başarılı kimlik doğrulamadan sonra komut satırı uygulaması ile arka kanal gerekli belirteçleri alır ve bunları gerekli web API çağrıları gerçekleştirmek için kullanır.

### <a name="constraints"></a>Kısıtlamalar

- Cihaz kod akışı, yalnızca ortak istemci uygulamalar üzerinde kullanılabilir.
- Genel istemci uygulamasını oluştururken aşağıdakilerden biri olması gerektiğinde geçirilen yetkilisi:
  - Kiralanan (form `https://login.microsoftonline.com/{tenant}/` burada `{tenant}` Kiracı kimliği veya Kiracı ile ilişkilendirilen bir etki alanını temsil eden GUID).
  - için herhangi bir iş ve Okul hesapları (`https://login.microsoftonline.com/organizations/`).
- Kişisel Microsoft hesapları Azure AD v2.0 uç noktası tarafından henüz desteklenmeyen (kullanamazsınız `/common` veya `/consumers` kiracılar).

## <a name="integrated-windows-authentication"></a>Tümleşik Windows kimlik doğrulaması
MSAL, Masaüstü için tümleşik Windows kimlik doğrulaması (IWA) destekler veya Windows bilgisayar bir etki alanına katılmış veya Azure AD üzerinde çalışan mobil uygulamalar birleştirilmiş. IWA kullanarak bu uygulamaları sessizce (olmadan herhangi bir kullanıcı Arabirimi etkileşimi kullanıcıdan) bir belirteç elde edebilirsiniz. 

![Tümleşik Windows kimlik doğrulama diyagramı](media/msal-authentication-flows/integrated-windows-authentication.png)

Yukarıdaki diyagramda, uygulama:

1. Tümleşik Windows kimlik doğrulaması kullanarak bir belirteç alır.
2. Kaynak isteğinde bulunmak için bu belirteci kullanır.

### <a name="constraints"></a>Kısıtlamalar

Kullanıcıların Active Directory'de oluşturulan ve Azure AD tarafından desteklenen anlamı Federasyon kullanıcıları yalnızca IWA destekler. Active Directory yedekleme doğrudan Azure AD'de oluşturulan kullanıcıları (yönetilen kullanıcılar), bu kimlik doğrulama akışı kullanamazsınız. Bu sınırlama etkilemez [kullanıcı adı/parola akış](#usernamepassword).

.NET Framework, .NET Core ve evrensel Windows platformu platformlar için yazılmış uygulamalar IWA içindir.

Çok faktörlü kimlik doğrulaması IWA atlama değil. Çok faktörlü kimlik doğrulaması yapılandırılmışsa, bir çok faktörlü kimlik doğrulaması sınaması gerekiyorsa IWA başarısız olabilir. Çok faktörlü kimlik doğrulaması, kullanıcı etkileşimi gerektirir.

Kimlik sağlayıcısı gerçekleştirilecek iki öğeli kimlik doğrulama istediğinde denetim yok. Kiracı Yöneticisi yapar. Genellikle, farklı bir ülkeden oturum açtığınızda, ne zaman, VPN şirket ağına bağlı değilsiniz ve hatta bazen zaman VPN bağlı olup olmadığınızı iki öğeli kimlik doğrulaması gereklidir. Azure AD, sürekli olarak iki öğeli kimlik doğrulama gerekli olup olmadığını öğrenmek için yapay ZEKA kullanır. IWA başarısız olursa, bir kullanıcı istemine geri dönmesi (https://aka.ms/msal-net-interactive).

Genel istemci uygulamasını oluştururken aşağıdakilerden biri olması gerektiğinde geçirilen yetkilisi:
- Kiralanan (form `https://login.microsoftonline.com/{tenant}/` burada `tenant` Kiracı kimliği veya Kiracı ile ilişkilendirilen bir etki alanını temsil eden GUID).
- için herhangi bir iş ve Okul hesapları (`https://login.microsoftonline.com/organizations/`). Kişisel Microsoft hesapları desteklenmez (kullanamazsınız `/common` veya `/consumers` kiracılar).

Sessiz bir akış IWA olduğu için aşağıdakilerden biri true olması gerekir:
- uygulamayı kullanmak için uygulamanızın kullanıcı önceden onaylı gerekir. 
- Uygulamayı kullanmak için kiracıdaki tüm kullanıcılar için Kiracı Yöneticisi önceden onaylı gerekir.

Bu, aşağıdakilerden biri true olduğu anlamına gelir:
- Bir geliştirici olarak, seçtiğiniz **Grant** kendiniz için Azure portalında.
- Bir kiracı Yöneticisi seçilmiş **{Kiracı etki alanı} için yönetici onayı vermek/iptal etmek** içinde **API izinleri** kaydı uygulama için sekmesinde (bkz [web API'lerine erişim izni ekleyin ](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)).
- Uygulama onayı bir yol sağladığınız (bkz [tek tek kullanıcı onayı isteyen](v2-permissions-and-consent.md#requesting-individual-user-consent)).
- Uygulama için onayı Kiracı Yöneticisi için bir yol sağladığınız (bkz [yönetici onayı](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant)).

IWA akışı, .NET Masaüstü, .NET Core ve evrensel Windows platformu uygulamaları için etkinleştirilir. .NET Core'da yalnızca kullanıcı adı alan aşırı yükleme kullanılabilir. .NET Core platformu, kullanıcı adı işletim sistemine istenemez.
  
Onay hakkında daha fazla bilgi için bkz. [v2.0 izinler ve onay](v2-permissions-and-consent.md).

## <a name="usernamepassword"></a>Kullanıcı adı/parola 
MSAL destekler [OAuth 2 kaynak sahibi parola kimlik bilgileri verme](v2-oauth-ropc.md), bir uygulama parolasını doğrudan işleyerek kullanıcının oturum açmasını sağlar. Masaüstü uygulamanızı sessizce belirteç almak için kullanıcı adı/parola akışı kullanabilirsiniz. Kullanıcı Arabirimi, uygulamayı kullanırken gereklidir.

![Kullanıcı adı/parola Akış Diyagramı](media/msal-authentication-flows/username-password.png)

Yukarıdaki diyagramda, uygulama:

1. Kullanıcı adı ve parola kimlik sağlayıcısına göndererek bir belirteç alır.
2. Belirteci kullanarak bir web API'sini çağırır.

> [!WARNING]
> Bu akış önerilmez. Yüksek derecede güven ve kullanıcı Etkilenme gerektiriyor.  Bu akış, yalnızca daha güvenli, diğer akışlar kullanıldığında kullanmalısınız. Daha fazla bilgi için [parolaların büyüyen soruna çözüm nedir?](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

Sessizce makinelerinde Windows etki alanına katılmış bir belirteç almak için tercih edilen akışı [tümleşik Windows kimlik doğrulaması](#integrated-windows-authentication). Aksi takdirde, ayrıca kullanabileceğiniz [cihaz kod akışını](#device-code).

Kendi kullanıcı Arabirimi sağlarsınız etkileşimli senaryolar kullanıcı adı/parola kullanmak istiyorsanız bu bazı durumlarda (DevOps senaryolarını) kullanışlı olsa da, bunu kaçınmaya çalışın. Kullanıcı adı/parola kullanarak:
- Çok faktörlü kimlik doğrulaması yapmak için gereken kullanıcılar etkileşimi (olduğu gibi) oturum açmanız mümkün olmayacaktır.
- Kullanıcılara çoklu oturum açmayı mümkün olmayacaktır.

### <a name="constraints"></a>Kısıtlamalar

Gelen apart [tümleşik Windows kimlik doğrulaması kısıtlamaları](#integrated-windows-authentication), ayrıca aşağıdaki kısıtlamalar uygulanır:

- Kullanıcı adı/parola akış koşullu erişim ve multi-Factor authentication ile uyumlu değildir. Sonuç olarak, uygulamanız nerede Kiracı Yöneticisi çok faktörlü kimlik doğrulaması gerektiren bir Azure AD kiracısında çalışıyorsa, bu akışı kullanamazsınız. Birçok kuruluş bu gerçekleştirin.
- Bu, yalnızca iş ve Okul hesapları için (Microsoft hesapları değil) çalışır.
- Flow, .NET Masaüstü ve .NET Core, ancak çalıştırılmadı Evrensel Windows platformu üzerinde kullanılabilir.

### <a name="azure-ad-b2c-specifics"></a>Azure AD B2C özellikleri

MSAL.NET ve Azure AD B2C'yi kullanma hakkında daha fazla bilgi için bkz. [ROPC kullanarak Azure AD B2C (MSAL.NET)](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc-with-azure-ad-b2c).
