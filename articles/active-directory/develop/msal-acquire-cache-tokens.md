---
title: Belirteçleri (Microsoft kimlik doğrulama kitaplığı) yönetme | Azure
description: Alma ve Microsoft kimlik doğrulama kitaplığı (MSAL) kullanma belirteçlerini önbelleğe alma hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d32b56b28d9ce7425e782fc10fa9ffb67047ce0
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65139143"
---
# <a name="acquiring-and-caching-tokens-using-msal"></a>MSAL kullanarak belirteçleri önbelleğe alma ve alınıyor
[Erişim belirteçlerini](access-tokens.md) güvenli bir şekilde web Azure tarafından korunan API'leri çağırmak istemcileri etkinleştirir. Microsoft Authentication Library (MSAL) kullanarak bir belirteç almak için birçok yolu vardır. Bazı yönlerden bir web tarayıcısı üzerinden Kullanıcı etkileşimlerine gerektirir. Bazı herhangi bir kullanıcı etkileşimi gerekmez. Genel olarak, bir belirteç almak için uygulamanın genel istemci uygulaması (Masaüstü ve mobil uygulama) veya bir gizli bir istemci uygulama (Web uygulaması, Web API'sini veya arka plan programı gibi bir Windows hizmeti) olup olmadığını bağlıdır.

MSAL, sonra elde bir belirteç önbelleğe alır.  Uygulama kodu, başka yollarla bir belirteç alınırken önce ilk olarak, bir belirteç sessizce (önbellekten) almak denemelisiniz.

Ayrıca, önbellekten hesapları kaldırarak elde belirteç önbelleği temizleyebilirsiniz. Bu, tarayıcıda, ancak oturum tanımlama bilgisinin kaldırmaz.

## <a name="scopes-when-acquiring-tokens"></a>Belirteçleri alırken kapsamları
[Kapsamları](v2-permissions-and-consent.md) erişim istemek istemci uygulamaları için bir web API'SİNİN kullanıma sunduğu izinleri bulunur. İstemci uygulamaları, web API'lerine erişim belirteçlerini almak için kimlik doğrulama istekleri yaparken bu kapsam için kullanıcı onayı isteyin. MSAL, geliştiriciler (v1.0) ve Microsoft kimlik Platformu (v2.0) API'leri için Azure AD erişim belirteçlerini almak sağlar. v2.0 protokol, kaynak istekleri yerine kapsamları kullanır. Daha fazla bilgi için okuma [v1.0 ve v2.0 karşılaştırma](active-directory-v2-compare.md). Web API'SİNİN kabul ettiği belirteci sürüm yapılandırmasına bağlı olarak, v2.0 uç noktası için MSAL erişim belirtecini döndürür.

MSAL sayısı alma belirteci yöntemleri gerektiren bir *kapsamları* parametresi. Bu parametre istenen izinlere ve istenen kaynaklara bildirmek dizeleri basit bir listesidir. İyi bilinen kapsamları misiniz [Microsoft Graph izinleri](/graph/permissions-reference).

Ayrıca v1.0 kaynaklarına erişmek için MSAL mümkündür. Daha fazla bilgi için okuma [v1.0 uygulama için kapsamları](msal-v1-app-scopes.md).

### <a name="request-specific-scopes-for-a-web-api"></a>Belirli kapsamlar web API'si için istek
Uygulamanızı bir kaynak API için belirli izinlere sahip belirteçler istemek gerektiğinde, uygulama kimliği URI'si API içeren kapsamları iletmeniz gerekir, aşağıdaki biçimi:  *&lt;uygulama kimliği URI'si&gt; / &lt;kapsamı&gt;*

Örneğin, Microsoft Graph API'si için kapsamları: `https://graph.microsoft.com/User.Read`

Veya örneğin, bir özel web API'si için kapsamları: `api://abscdefgh-1234-abcd-efgh-1234567890/api.read`

Microsoft Graph API için yalnızca bir kapsam değeri `user.read` eşlendiği `https://graph.microsoft.com/User.Read` biçimlendirmek ve birbirlerinin yerine kullanılabilir.

> [!NOTE]
> Bazı web API'lerini Azure Resource Manager API'si gibi (https://management.core.windows.net/) beklediğiniz bir sonda ' /' izleyiciye erişim belirteci (aud) talep. Bu durumda, kapsam olarak geçirmek önemli olan https://management.core.windows.net//user_impersonation (çift eğik çizgi unutmayın) için API geçerli olması için belirteci.

### <a name="request-dynamic-scopes-for-incremental-consent"></a>Dinamik kapsam artımlı onay iste
V1.0 kullanan uygulamalar oluştururken, uygulama kullanıcının oturum açma sırasında onayını istemeniz için gerekli izinler (statik kapsam) tam kümesini kaydetmek gerekiyordu. V2.0, kapsam parametresi kullanarak gerektiği gibi ek izinler isteyebilir. Bu, dinamik kapsam olarak adlandırılır ve kapsamlar için artımlı rıza sağlamanın izin verin.

Örneğin, başlangıçta kullanıcı oturum ve bunları her tür erişim engelle. Daha sonra bunları alma belirteci yöntemleri Takvim kapsamda isteyerek kullanıcının Takvim okumak ve kullanıcının onayı alma olanağı verebilirsiniz.

Örneğin: `https://graph.microsoft.com/User.Read` ve `https://graph.microsoft.com/Calendar.Read`

## <a name="acquiring-tokens-silently-from-the-cache"></a>Sessizce (önbellekten) belirteçlerini almak
MSAL, bir belirteç önbelleği (veya gizli bir istemci uygulamaları için iki önbellekler) korur ve sonra elde bir belirteç önbelleğe alır.  Çoğu durumda, sessiz bir belirteci alınmaya çalışılırken farklı bir belirteç ile bir belirteç önbelleğe göre daha fazla kapsam alacaksınız. Ayrıca, bir belirteç yenileme (belirteç önbelleği yenileme belirteci içerecek şekilde) süresi doluyor olduğunda yeteneğine sahiptir.

### <a name="recommended-call-pattern-for-public-client-applications"></a>Genel istemci uygulamaları için önerilen arama deseni
Uygulama kodu, sessizce (önbellekten), önce bir belirteç almak üzere denemelisiniz.  Yöntem çağrısı bir "Kullanıcı Arabirimi gerekli" hata veya özel durum döndürürse, başka yollarla bir belirteç alınırken deneyin. 

Bununla birlikte, önce iki akışı vardır, **barındırmamalıdır** sessiz bir belirteç almak çalışır:

- [istemci kimlik bilgileri akışı](msal-authentication-flows.md#client-credentials), hangi kullanmayan kullanıcı belirteç önbelleği, ancak bir uygulama belirteci önbelleği. Bu yöntem bu uygulama belirteç önbelleği STS'ye isteği göndermeden önce doğrulama üstlenir.
- [Yetkilendirme kodu akışı](msal-authentication-flows.md#authorization-code) Web uygulamalarında olarak redeems uygulama kullanıcı oturum imzalama ve bunları zorunda tarafından alındı bir kod daha fazla kapsam için onay. Bir kod, bir parametre ve bir hesap geçirilir. bu yana yöntemi önbellekte gerektirir, yine de, kod kullanılmadan önce hizmetine bir çağrı arayın olamaz.

### <a name="recommended-call-pattern-in-web-apps-using-the-authorization-code-flow"></a>Tavsiye edilen yetkilendirme kod akışı kullanarak Web uygulamalarında arama deseni 
Kullanan Web uygulamaları için [Openıd Connect yetkilendirme kod akışı](v2-protocols-oidc.md), önerilen Düzen denetleyicileri içinde:

- Özelleştirilmiş seri hale getirme ile bir belirteç önbelleği ile bir gizli bir istemci uygulama örneği oluşturur. 
- Yetkilendirme kodu akışını kullanarak belirteç Al

## <a name="acquiring-tokens"></a>Belirteç alınırken
Genel olarak, belirteç alınırken bir yöntemi genel istemci veya gizli bir istemci uygulaması mı olduğuna bağlıdır.

### <a name="public-client-applications"></a>Genel istemci uygulamaları
Genel istemci uygulamaları (Masaüstü ve mobil uygulama) için:
- Genellikle belirteçleri etkileşimli bir kullanıcı Arabirimi veya açılır pencere oturum kullanıcıdan edinin.
- İçin [bir belirteç, oturum açmış kullanıcı için sessiz bir şekilde almak](msal-authentication-flows.md#integrated-windows-authentication) Masaüstü uygulamasını bir Windows bilgisayarda çalışıyorsa tümleşik Windows kimlik doğrulaması (IWA/Kerberos) kullanarak birleştirilmiş bir etki alanına veya Azure.
- İçin [bir kullanıcı adı ve parola ile bir belirteç almak](msal-authentication-flows.md#usernamepassword) uygulamaları, ancak bu .NET framework Masaüstü istemcisinde önerilmez. Kullanıcı adı/parola gizli istemci uygulamalarında kullanmayın.
- Aracılığıyla bir belirteç elde edebilir [cihaz kod akışını](msal-authentication-flows.md#device-code) çalıştıran cihazlarda bir web tarayıcı olmayan uygulamalarda. Kullanıcı, bir URL ve ardından başka bir cihazda bir web tarayıcısına gider ve kodu girer ve oturum açtığında bir kod ile sağlanır.  Ardından Azure AD, bir belirteç tarayıcı olmayan cihaza geri gönderir.

### <a name="confidential-client-applications"></a>Gizli istemci uygulamaları 
Gizli istemci uygulamalar (Web uygulaması, Web API'sini veya arka plan programı uygulaması Windows hizmeti gibi),:
- Belirteçlerini almak **uygulamanın kendisinin** ve bir kullanıcı için kullanarak [istemci kimlik bilgileri akışı](msal-authentication-flows.md#client-credentials). Bu, eşitleme araçları veya genel kullanıcıların işleyen Araçlar ve belirli bir kullanıcı için kullanılabilir. 
- Kullanım [On-behalf-of akışı](msal-authentication-flows.md#on-behalf-of) için bir web API'si kullanıcı adına bir API'yi çağırmak için. Uygulama, bir kullanıcı onayı (veya örneğin, bir JWT belirteci SAML) dayalı bir belirteç almak için istemci kimlik bilgileri ile tanımlanır. Bu akış, hizmetten hizmete çağrılar belirli bir kullanıcının kaynaklara erişmeye gerek duyan uygulamalar tarafından kullanılır.
- Belirteçleri kullanarak elde [yetkilendirme kod akışı](msal-authentication-flows.md#authorization-code) kullanıcı oturum açtığında yetkilendirme aracılığıyla istek URL'si sonra web apps'te. Openıd Connect uygulama, bu kullanıcının oturum açma kimliği kullanarak bağlanmasına olanak sağlayan bir mekanizma ve ardından kullanıcı adına erişim web API'leri genellikle kullanın.


## <a name="authentication-results"></a>Kimlik doğrulama sonuçları 
İstemciniz bir erişim belirteci isteğinde bulunduğunda, Azure AD erişim belirteci hakkında bazı meta veriler içeren bir kimlik doğrulaması sonucu da döndürür. Bu bilgiler, kapsamlar için geçerlidir ve erişim belirteci süre sonu içerir. Bu veriler, akıllı erişim belirteçlerinin önbelleğe erişim belirteci ayrıştırılamadı gerek kalmadan yapmak uygulamanızı sağlar.  Kimlik doğrulaması sonucu gösterir:

- [Erişim belirteci](access-tokens.md) kaynaklara erişmek için web API'si için. Bu bir dizedir, genellikle bir base64 kodlamalı JWT ancak istemci hiçbir zaman içinde erişim belirteci görünmelidir. Biçim tutarlı kalmasını garanti yoktur ve kaynak için şifrelenebilir. Kişilere erişim belirteci istemci içeriğine bağlı olarak kod yazma hataları ve istemci mantıksal sonları büyük kaynaklarını biridir.
- [Kimlik belirteci](id-tokens.md) (Bu, bir JWT) kullanıcı.
- Belirtecin süresinin sona erdiği tarih/saat belirten belirteç süre sonu.
- Kiracı kimliği, kullanıcının bulunduğu Kiracı içerir. Konuk kullanıcılar (Azure AD B2B senaryoları) için Kiracı kimliği benzersiz kiracının Konuk Kiracı ' dir. Belirteç, bir kullanıcı adına gönderilir, kimlik doğrulaması sonucu bu kullanıcı hakkındaki bilgileri de içerir. Burada hiçbir kullanıcıyla (uygulama) belirteçleri istenen gizli istemci akışlar için bu kullanıcı bilgileri null olur.
- Belirteç verildiği kapsamları.
- Kullanıcının benzersiz kimliği.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [hataları ve özel durumları işleme](msal-handling-exceptions.md). 
