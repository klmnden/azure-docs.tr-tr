---
title: İstemci uygulamaları (Microsoft kimlik doğrulama kitaplığı) | Azure
description: Genel istemci ve gizli istemci uygulamaların Microsoft Authentication Library (MSAL) öğrenin.
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
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 096aa5e5ce2f33467457cef22220f338ae49b708
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65139111"
---
# <a name="authentication-flows"></a>Kimlik doğrulama akışları

Bu makalede, Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından sağlanan farklı kimlik doğrulama akışları açıklanır.  Bu akış, farklı uygulama senaryolarında çeşitli kullanılabilir.

| Akış | Açıklama | Kullanılan|  
| ---- | ----------- | ------- | 
| [Örtülü izin](#implicit-grant) | Bir arka uç sunucusu kimlik bilgisi alışverişinin yapmadan belirteçlerini almak okumasına izin verir. Bu, kullanıcının oturumunu, oturumu korumak ve istemcideki tüm diğer web API'leri belirteçleri JavaScript kodu alma okumasına izin verir.| [Tek sayfa uygulamaları (SPA)](scenario-spa-overview.md) |
| [Yetkilendirme kodu](#authorization-code) | Web API'leri gibi korunan kaynakları erişim kazanmak için cihazda yüklü uygulamalar kullanılır. Bu, oturum açın ve API'ye erişmek için mobil ve Masaüstü uygulamaları eklemenize olanak sağlar. | [Masaüstü uygulamaları](scenario-desktop-overview.md), [mobil uygulamalar](scenario-mobile-overview.md), [Web uygulamaları](scenario-web-app-call-api-overview.md) | 
| [On-behalf-of](#on-behalf-of) | Bir uygulama hizmeti/sırayla başka bir hizmet/web API'sini çağırmak için gereken web API'sini çağırır. İstek zincirinin aracılığıyla izinleri ve yetkilendirilmiş kullanıcının kimlik yayılması olur. | [Web API'leri](scenario-web-api-call-api-overview.md) |
| [İstemci kimlik bilgileri](#client-credentials) | Uygulama kimliğini kullanarak web bulunan kaynaklara erişim sağlar. Hemen bir kullanıcı etkileşimi olmadan arka planda çalışması gereken sunucudan sunucuya etkileşimleri için yaygın olarak kullanılır. | [arka plan programları](scenario-daemon-overview.md) |
| [Cihaz kodu](#device-code) | Giriş kısıtlı cihazları akıllı TV gibi IOT cihaz veya yazıcı oturum açmalarını sağlar. | [Masaüstü/Mobile apps](scenario-desktop-acquire-token.md#command-line-tool-without-web-browser) |
| [Tümleşik Windows kimlik doğrulaması](scenario-desktop-acquire-token.md#integrated-windows-authentication) | Etki alanı ya da Azure AD uygulamaları bilgisayarlara sessiz bir şekilde (olmadan herhangi bir kullanıcı Arabirimi etkileşimi kullanıcıdan) bir belirteç almak için birleştirilmiş sağlar.| [Masaüstü/Mobile apps](scenario-desktop-acquire-token.md#integrated-windows-authentication) |
| [Kullanıcı adı/parola](scenario-desktop-acquire-token.md#username--password) | Bir uygulamanın doğrudan parolalarını işleyerek kullanıcının oturum açmasını sağlar. Bu akış önerilmez. | [Masaüstü/mobile apps](scenario-desktop-acquire-token.md#username--password) | 


## <a name="implicit-grant"></a>Örtük izin verme

MSAL destekler [2 OAuth örtük izin akışı](v2-oauth2-implicit-grant-flow.md), belirteçleri Microsoft kimlik platformu ' bir arka uç sunucusuna gerçekleştirmeden kimlik bilgisi alışverişinin ve uygulamayı sağlar. Bu, kullanıcının oturumunu, oturumu korumak ve istemcideki tüm diğer web API'leri belirteçleri JavaScript kodu alma okumasına izin verir.

![Örtük verme akışı](media/msal-authentication-flows/implicit-grant.svg)

Birçok modern web uygulamaları, JavaScript veya Angular ve Vue.js React.js gibi bir SPA framework kullanılarak yazılmış istemci-tarafı tek sayfalı uygulamalar olarak oluşturulur. Bu uygulamalar, bir web tarayıcısında çalıştırma ve geleneksel sunucu tarafı web uygulamaları'den farklı kimlik doğrulama özellikleri vardır. Tek sayfalı uygulama kullanıcılarının oturumunu ve arka uç hizmetlerine veya web API'lerine örtük verme akışı kullanarak erişim belirteçleri almak Microsoft kimlik platformu sağlar. Örtük akış, kimliği doğrulanmış bir kullanıcıyı temsil eder ve ayrıca korunan API'leri çağırmak için gereken belirteçlerini erişmek için kullanılan kimlik belirteçlerini almak için uygulama izin verir.

Bu kimlik doğrulama akışı, bunlar daha fazla özellikleri yerel platformları ile etkileşim gerektiren bu yana Elektron ve React-Native gibi platformlar arası JavaScript çerçevesi kullanılarak uygulama senaryoları içermez.

## <a name="authorization-code"></a>Yetkilendirme kodu
MSAL destekler [OAuth 2 yetkilendirme kodu verme](v2-oauth2-auth-code-flow.md), web API'leri gibi korunan kaynakları erişim kazanmak için cihazda yüklü olan uygulamalarda kullanılabilir. Bu, oturum açın ve API'ye erişmek için mobil ve Masaüstü uygulamaları eklemenize olanak sağlar. 

Web uygulaması, web uygulamaları (web siteleri) kullanıcılar oturum açtığında bir yetkilendirme kodu alır.  Yetkilendirme kodu, web API'leri çağırmak için bir belirteç almak için kullanılan. ASP.NET'te / ASP.NET core web uygulamaları, yalnızca amacı `AcquireTokenByAuthorizationCode` belirteç önbelleği için bir belirteç eklemek için böylelikle yalnızca bir belirteç elde etmek için bir API kullanarak uygulama (genellikle, denetleyiciler) tarafından ardından kullanılabilir `AcquireTokenSilent`.

![Yetkilendirme kod akışı](media/msal-authentication-flows/authorization-code.png)

### <a name="considerations"></a>Dikkat edilmesi gerekenler
- Yetkilendirme kodu yalnızca bir kez bir belirteç almak için kullanılabilir. (Açıkça standart protokol belirtimi tarafından yasaklanmış) birden çok kez aynı yetkilendirme kodu ile bir belirteç almak çalışmayın. Size kodu birkaç kez kasıtlı olarak kullanmak ya da bir çerçeve de bunu sizin için halleder, farkında olmadığından, bir hata alırsınız: `AADSTS70002: Error validating credentials. AADSTS54005: OAuth2 Authorization code was already redeemed, please retry with a new valid code or use an existing refresh token.`

- Bir ASP.NET/ASP.NET Core uygulaması yazıyorsanız, ASP.NET/Core framework söyleme, yetkilendirme kodu zaten yararlandınız gerçekleşebilir. Bunun için çağırmanız gerekir `context.HandleCodeRedemption()` yöntemi `AuthorizationCodeReceived` olay işleyicisi.

- Erişim belirteci doğru gerçekleştiği artımlı onay engelleyebilir ASP.NET ile paylaşmaktan kaçının.  Daha fazla bilgi için [sorun #693](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/693).

## <a name="on-behalf-of"></a>On-behalf-of

MSAL destekler [OAuth 2 on-behalf-of kimlik doğrulama akışı](v2-oauth2-on-behalf-of-flow.md).  Bu akış, bir uygulama hizmeti/sırayla başka bir hizmet/web API'sini çağırmak için gereken web API'si, istediğinde kullanılır. İstek zincirinin aracılığıyla izinleri ve yetkilendirilmiş kullanıcının kimlik yayılması olur. Orta katman hizmet aşağı akış hizmeti için kimliği doğrulanmış isteğinde bulunmak Microsoft kimlik platformu, kullanıcı adına bir erişim belirteci güvenliğini sağlamak gerekir.

![On-behalf-of akışı](media/msal-authentication-flows/on-behalf-of.png)

1. Web API'si için bir erişim belirteci alır
2. HTTP isteğinin kimlik doğrulama üst bilgisindeki bir taşıyıcı belirteç olarak erişim belirteci ekleme korumalı Web API'si, bir istemci (Web, masaüstü, mobil, tek sayfalı uygulama) çağırır. Web API'si, kullanıcının kimliğini doğrular.
3. İstemci Web API'sini çağıran, Web API'si kullanıcı farklı bir belirteç on-behalf-of ister.  
4. Korumalı Web API'si, kullanıcı on-behalf-of bir aşağı akış Web API'sini çağırmak için bu belirteci kullanır.  Web API'si daha sonra da, diğer aşağı akış API'leri (ancak yine de aynı kullanıcı adına) belirteçleri isteyebilir.

## <a name="client-credentials"></a>İstemci kimlik bilgileri

MSAL destekler [OAuth 2 istemci kimlik bilgileri akışı](v2-oauth2-client-creds-grant-flow.md). Bu akış bir uygulamanın kimliğini kullanarak web bulunan kaynaklara erişim sağlar. Bu tür bir verme, hemen bir kullanıcı etkileşimi olmadan arka planda çalışması gereken sunucudan sunucuya etkileşimleri için yaygın olarak kullanılır. Bu tür uygulamalar, Daemon'ları veya hizmet hesapları da anılır. 

İstemci kimlik bilgileri, başka bir web hizmetini çağırırken bir kullanıcının kimliğine bürünmek yerine kendi kimlik bilgilerini kullanmak için bir web hizmetinin (gizli istemci) akış izinleri verin. Bu senaryoda istemci genellikle bir orta katman web hizmeti, bir arka plan programı hizmeti veya web sitesi olur. Daha yüksek bir güvence düzeyi için Microsoft identity platformuna (yerine, paylaşılan gizlilik) bir sertifika bir kimlik bilgisi olarak kullanılacak arama hizmeti de sağlar.

> [!NOTE]
> Bunlar yalnızca ortak istemci uygulamaları destekleyen mobil platformlarda (UWP, Xamarin.iOS ve Xamarin.Android) gizli istemci akışı kullanılamıyor.  Genel istemci uygulamaları, uygulamanın kimliğini kanıtlamak için kimlik sağlayıcısı nasıl yapılandıracağımı bilmiyorum. Güvenli bir bağlantı üzerinde web uygulaması elde edilebilir veya web API arka-bir sertifika dağıtarak sona erer.

MSAL.NET üç istemci kimlik bilgileri türünü destekler:

- Uygulama gizli dizilerini <BR>![Parola ile gizli istemci](media/msal-authentication-flows/confidential-client-password.png)
- Sertifikalar <BR>![Gizli bir istemci sertifikası ile](media/msal-authentication-flows/confidential-client-certificate.png)
- En iyi duruma getirilmiş istemci onaylama<BR>![Gizli bir istemci ile onaylar](media/msal-authentication-flows/confidential-client-assertions.png)

Bu istemci kimlik bilgileri gerekir:

- Azure AD'ye kayıtlı.
- Gizli bir istemci uygulama kodunuzda yapımı sırasında geçirildi.


## <a name="device-code"></a>Cihaz kodu
MSAL destekler [OAuth 2 cihaz kod akışını](v2-oauth2-device-code.md), giriş kısıtlı cihazları akıllı TV gibi IOT cihaz veya yazıcı oturum açmalarını sağlar. Etkileşimli kimlik doğrulaması Azure AD ile bir web tarayıcısı gerektirir. Cihaz kod akışını etkileşimli olarak burada cihaz veya işletim sistemi bir Web tarayıcısı sağlamaz oturum açmak için (örneğin başka bir bilgisayara veya bir cep telefonu) başka bir cihaz kullanın izin verir.

Cihaz kod akışı kullanarak, uygulama belirteçleri bu cihazları/OS için özellikle tasarlanmış iki adımlı bir işlemle alır. IOT cihazları veya komut satırı araçlarını (CLI) üzerinde çalışan uygulamalar söz konusu uygulamalar örnekleridir. 

![Cihaz kod akışı](media/msal-authentication-flows/device-code.png)

1. Kullanıcı kimlik doğrulaması gerekli olduğunda, uygulama bir kod sağlar ve kullanıcıdan bir URL'ye gidin (örneğin, bir İnternet'e bağlı smartphone) başka bir cihaz kullanın (örneğin, http://microsoft.com/devicelogin), burada kullanıcı kodunu girmeniz istenir. Bitti, web sayfası kullanıcı onayı istemlerini ve çok faktörlü kimlik doğrulaması gerekirse dahil olmak üzere bir normal kimlik doğrulama deneyimi aracılığıyla önünü açacak olduğunu.

2. Başarılı kimlik doğrulamadan sonra komut satırı uygulaması ile arka kanal gerekli belirteçleri alır ve ihtiyaç duyduğu web API çağrıları gerçekleştirmek için kullanın.

### <a name="constraints"></a>Kısıtlamalar

- Cihaz kod akışı, yalnızca ortak istemci uygulamalar üzerinde kullanılabilir.
- Genel istemci uygulaması oluşturma olması gerektiğinde yetkilisi geçirilen:
  - kiralanan (form `https://login.microsoftonline.com/{tenant}/` burada `{tenant}` Kiracı kimliği veya Kiracı ile ilişkilendirilen bir etki alanını temsil eden GUID.
  - veya, tüm iş ve Okul hesapları (`https://login.microsoftonline.com/organizations/`).
- Kişisel Microsoft hesapları, Azure AD v2.0 uç noktası tarafından henüz desteklenmiyor (kullanamazsınız `/common` veya `/consumers` kiracılar).

## <a name="integrated-windows-authentication"></a>Tümleşik Windows Kimlik Doğrulaması
MSAL, Masaüstü için tümleşik Windows kimlik doğrulaması (IWA) destekler veya Windows bilgisayar bir etki alanına katılmış veya Azure AD üzerinde çalışan mobil uygulamalar birleştirilmiş. IWA kullanarak bu uygulamaları sessizce (olmadan herhangi bir kullanıcı Arabirimi etkileşimi kullanıcıdan) bir belirteç elde edebilirsiniz. 

![Tümleşik Windows Kimlik Doğrulaması](media/msal-authentication-flows/integrated-windows-authentication.png)

### <a name="constraints"></a>Kısıtlamalar

IWA destekler **federe** yalnızca kullanıcılar.  Kullanıcılar, bir Active Directory içinde oluşturulan ve Azure Active Directory tarafından desteklenir. Doğrudan AD yedekleme olmadan Azure AD'de oluşturulan kullanıcıları (**yönetilen** kullanıcılar) bu kimlik doğrulama akışı kullanamazsınız. Bu sınırlama etkilemez [kullanıcı adı/parola akış](#usernamepassword).

.NET Framework, .NET Core ve evrensel Windows platformu platformlar için yazılmış uygulamalar IWA içindir.

IWA MFA (çok faktörlü kimlik doğrulamasını) atlamaz. MFA yapılandırıldıysa, MFA kullanıcı etkileşimi gerektirdiğinden MFA testini gerekliyse IWA başarısız olabilir. Bu zor olur. IWA etkileşimli olmayan, ancak iki öğeli yetkilendirme (2FA) kullanıcı etkileşimini gerektirir. Kimlik sağlayıcısı gerçekleştirilecek 2FA istediğinde, kontrol etmez, Kiracı Yöneticisi yapar. Bizim gözlemler farklı bir ülkeden bağlı olmayan VPN aracılığıyla kurumsal ağ ve VPN bağlandığında bazen bile oturum 2FA gereklidir. Belirleyici bir kural kümesi beklemiyoruz, sürekli olarak 2fa'yı gerekli olup olmadığını öğrenmek için yapay ZEKA Azure Active Directory kullanır. Geri dönüş için bir kullanıcı istemi gerekir (https://aka.ms/msal-net-interactive) IWA başarısız olursa.

Genel istemci uygulaması oluşturma olması gerektiğinde yetkilisi geçirilen:
- kiralanan (form `https://login.microsoftonline.com/{tenant}/` burada `tenant` Kiracı kimliği veya Kiracı ile ilişkilendirilen bir etki alanını temsil eden GUID.
- için herhangi bir iş ve Okul hesapları (`https://login.microsoftonline.com/organizations/`). Kişisel Microsoft hesapları desteklenmez (kullanamazsınız `/common` veya `/consumers` kiracılar).

Sessiz bir akış IWA olduğu için:
- uygulamayı kullanmak için uygulamanızın kullanıcı önceden onaylı gerekir. 
- veya, Kiracı Yöneticisi daha önce uygulamayı kullanmak için kiracıdaki tüm kullanıcılar gerekir.
- Bunun anlamı:
    - bir geliştirici olarak ya da basılı **Grant** kendiniz için Azure portalında düğmesi 
    - ya da bir kiracı Yöneticisi bastığını **{Kiracı etki alanı} için yönetici onayı vermek/iptal etmek** düğmesine **API izinleri** kaydı uygulama için sekmesinde (bkz [izinleri ekleyin erişim web API'leri](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis))
    - veya, sağladığınız uygulama onayı bir yol (bkz [tek tek kullanıcı onayı isteyen](v2-permissions-and-consent.md#requesting-individual-user-consent))
    - veya uygulama için onayı Kiracı Yöneticisi için bir yol sağladığınız (bkz [yönetici onayı](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant))

IWA akışı, .NET Masaüstü, .NET Core ve evrensel Windows platformu uygulamaları için etkinleştirilir. .NET Core, .NET Core platform işletim sistemi için kullanıcı adı sorduğunuzda yalnızca kullanıcı adı alan aşırı yükleme kullanılabilir.
  
Onay hakkında daha fazla bilgi için bkz. [v2.0 izinler ve onay](v2-permissions-and-consent.md).

## <a name="usernamepassword"></a>Kullanıcı adı/parola 
MSAL destekler [OAuth 2 kaynak sahibi parola kimlik bilgileri verme](v2-oauth-ropc.md), bir uygulama parolasını doğrudan işleyerek kullanıcının oturum açmasını sağlar. Masaüstü uygulamanızı sessizce belirteç almak için kullanıcı adı/parola akışı kullanabilirsiniz. Kullanıcı Arabirimi, uygulamayı kullanırken gereklidir.

![Kullanıcı adı/parola akış](media/msal-authentication-flows/username-password.png)

> [!WARNING]
> Bu akış **önerilmez** yüksek derecede güven ve kullanıcı Etkilenme gerektirdiğinden.  Bu akış, yalnızca daha güvenli, diğer akışlar kullanıldığında kullanmalısınız. Bu sorun hakkında daha fazla bilgi için bkz. [bu makalede](https://news.microsoft.com/features/whats-solution-growing-problem-passwords-says-microsoft/). 

Windows etki alanına katılmış makinelerde sessiz bir belirteç almak için tercih edilen akışı [tümleşik Windows kimlik doğrulaması](#integrated-windows-authentication). Aksi takdirde, ayrıca kullanabileceğiniz [cihaz kod akışı](#device-code)

Kendi kullanıcı Arabirimi sağlarsınız etkileşimli senaryolar kullanıcı adı/parola kullanmak istiyorsanız bu bazı durumlarda (DevOps senaryolarını) kullanışlı olsa da, gerçekten uzağa bu taşımayla ilgili almalısınız. Kullanıcı adı/parola kullanarak, veren etmenizi Yukarı:
- modern kimlik kiracılar Çekirdek: parola fished, yeniden yürütülmesi. Çünkü bu kavramı, geçirilebilir bir paylaşım gizli dizi sahip olabiliyoruz.
Bu, parolasız ile uyumlu değil.
- MFA yapmak için gereken kullanıcılar etkileşimi (olduğu gibi) oturum açmanız mümkün olmayacaktır.
- Kullanıcılara çoklu oturum açmayı mümkün olmayacaktır.

### <a name="constraints"></a>Kısıtlamalar

Gelen apart [tümleşik Windows kimlik doğrulaması kısıtlamaları](#integrated-windows-authentication), ayrıca aşağıdaki kısıtlamalar uygulanır:

- Kullanıcı adı/parola akış koşullu erişim ve çok faktörlü kimlik doğrulaması ile uyumlu değil: Sonuç olarak, uygulamanız nerede Kiracı Yöneticisi çok faktörlü kimlik doğrulaması gerektiren bir Azure AD kiracısında çalışıyorsa, bu akışı kullanamazsınız. Birçok kuruluş bu gerçekleştirin.
- Yalnızca iş için çalışır ve Okul hesapları (MSA değil)
- Flow, .NET Masaüstü ve .NET core, ancak çalıştırılmadı Evrensel Windows platformu üzerinde kullanılabilir.

### <a name="azure-ad-b2c-specifics"></a>Azure AD B2C özellikleri

MSAL.NET ve Azure AD B2C'yi kullanma hakkında daha fazla bilgi için okuma [ROPC kullanarak Azure AD B2C (MSAL.NET)](msal-net-aad-b2c-considerations.md#resource-owner-password-credentials-ropc-with-azure-ad-b2c).