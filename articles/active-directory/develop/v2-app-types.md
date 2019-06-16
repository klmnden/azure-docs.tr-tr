---
title: Microsoft kimlik platformu için uygulama türleri | Azure
description: Uygulamaları ve Microsoft kimlik Platformu (v2.0) uç noktası tarafından desteklenen senaryolarla türleri.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2019
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd7055794860398ecc6296d0174a691e7fac4450
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235246"
---
# <a name="application-types-for-microsoft-identity-platform"></a>Microsoft kimlik platformu için uygulama türleri

Microsoft kimlik Platformu (v2.0) uç noktası için endüstri standardı protokollerine göre tüm modern uygulama mimarilerine çeşitli kimlik doğrulamasını destekleyen [OAuth 2.0 veya Openıd Connect](active-directory-v2-protocols.md). Bu makalede, tercih edilen dil veya platformdan bağımsız olarak Microsoft kimlik platformu kullanarak oluşturabileceğiniz uygulama türleri açıklanmaktadır. Bilgiler, önce üst düzey senaryoları anlamanıza yardımcı olması için tasarlanmış [kodu ile çalışma başlangıç](v2-overview.md#getting-started).

> [!NOTE]
> Microsoft kimlik platformu uç nokta, tüm Azure Active Directory (Azure AD) senaryolarını ve özelliklerini desteklemez. Microsoft kimlik platformu uç noktasını kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [Microsoft Identity platform sınırlamaları](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Temel bilgiler

Microsoft kimlik platformu uç nokta yeni kullanan her bir uygulamayı kaydetmeniz gerekir [uygulama kayıt portalı](https://go.microsoft.com/fwlink/?linkid=2083908). Uygulama kayıt işlemi toplar ve uygulamanız için bu değerleri atar:

* Bir **uygulama (istemci) kimliği** uygulamanızı benzersiz şekilde tanımlayan
* A **yeniden yönlendirme URI'si** yanıtları uygulamanıza geri yönlendirmek için kullanabileceğiniz
* Birkaç diğer senaryoya özel değerler gibi desteklenen hesap türleri

Daha fazla bilgi için edinmek için nasıl [bir uygulamayı kaydetme](quickstart-register-app.md).

Uygulama kaydedildikten sonra uygulama uç noktaya istek göndererek Microsoft kimlik platformu ile iletişim kurar. Açık kaynak çerçeve ve bu isteklerin işlenmesi kitaplıkları sunuyoruz. Ayrıca, kimlik doğrulaması mantığı Bu uç noktaları için istek oluşturarak kendi başınıza uygulamak için seçeneğiniz de vardır:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

## <a name="single-page-apps-javascript"></a>Tek sayfa uygulamaları (JavaScript)

Birçok modern uygulamanın, JavaScript'te öncelikli olarak yazılmış tek sayfa uygulaması ön ucu vardır. Genellikle, Angular, React ve Vue gibi bir çerçeve kullanılarak yazılır. Microsoft kimlik platformu uç noktayı kullanarak bu uygulamaları destekler [OAuth 2.0 örtük akışını](v2-oauth2-implicit-grant-flow.md).

Bu akışta uygulama belirteçlerini doğrudan Microsoft kimlik platformu alır. herhangi bir sunucudan sunucuya değişimleri olmadan bir uç nokta yetkilendirin. Tüm kimlik doğrulama mantığı ve gerçekleştirilen işlemlerin işleme oturumu tamamen JavaScript istemcisi, ek sayfa yeniden yönlendirmeleri olmadan yerleştirin.

![Örtük kimlik doğrulama akışı](./media/v2-app-types/convergence-scenarios-implicit.svg)

Bu senaryoyu çalışırken görmek için tek sayfalı uygulama kodu örneklerinden birini deneyin [Başlarken Microsoft kimlik platformu](v2-overview.md#getting-started) bölümü.

## <a name="web-apps"></a>Web uygulamaları

Kullanıcı bir tarayıcıdan erişen web uygulamaları için (.NET, PHP, Java, Ruby, Python, düğüm) kullandığınız [Openıd Connect](active-directory-v2-protocols.md) kullanıcı oturum açma için. Openıd Connect web uygulamasına bir kimlik belirteci alır. Bir kimlik belirteci, kullanıcının kimliğini doğrular ve talep biçiminde kullanıcı hakkında bilgi sağlayan bir güvenlik belirtecidir:

```
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Microsoft kimlik platformu uç noktası kullanılan belirteçlerin farklı türlerde ek ayrıntılar kullanılabilir [erişim belirteci](access-tokens.md) başvuru ve [id_token başvurusu](id-tokens.md)

Web sunucu uygulamalarında oturum açma kimlik doğrulaması akışı şu üst düzey adımları gerçekleştirir:

![Web uygulama kimlik doğrulama akışı](./media/v2-app-types/convergence-scenarios-webapp.svg)

Microsoft kimlik platformu uç noktasından alınan bir ortak imzalama anahtarı ile kimlik belirteci doğrulayarak, kullanıcının kimliğini sağlayabilirsiniz. Sonraki sayfa isteklerinde kullanıcı tanımlamak için kullanılabilecek oturum tanımlama bilgisinin ayarlanır.

Bu senaryoyu çalışırken görmek için web uygulaması oturum açma kodu örneklerinden birini deneyin [Başlarken Microsoft kimlik platformu](v2-overview.md#getting-started) bölümü.

Basit oturum açma ek olarak, bir web sunucusu uygulamasının bir REST API gibi başka bir web hizmetine erişmesi gerekebilir. Bu durumda, web sunucusu uygulaması kullanarak birleşik bir Openıd Connect ve OAuth 2.0 akışı, ilgilenir [OAuth 2.0 yetkilendirme kod akışı](active-directory-v2-protocols.md). Bu senaryo hakkında daha fazla bilgi için okuyun [web uygulamaları ve Web API'leri ile çalışmaya başlama](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Web API'leri

Uygulamanızın RESTful Web API'si gibi web hizmetlerinin güvenliğini sağlamak için Microsoft kimlik platformu uç noktası'nı kullanabilirsiniz. Kimlik belirteçlerini ve oturum tanımlama bilgilerini yerine Web API'sini gelen isteklerin kimliklerini doğrular ve verilerinin güvenliğini sağlamak için bir OAuth 2.0 erişim belirteci kullanır. Bir Web API'si çağıran şunun gibi bir HTTP isteğinin yetkilendirme üst bilgisinde erişim belirteci ekler:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API API çağıranının kimliğini doğrulamak ve erişim belirtecinde kodlanmış talep arayanı hakkında bilgi ayıklamak için erişim belirtecini kullanır. Microsoft kimlik platformu uç noktası kullanılan belirteçlerin farklı türlerde ek ayrıntılar kullanılabilir [erişim belirteci](access-tokens.md) başvuru ve [id_token başvurusu](id-tokens.md)

Bir Web API'si kullanıcıların kabul et veya belirli işlevleri veya veri yetersiz izinler olarak da bilinen göstererek iyileştirilmiş power verebilirsiniz [kapsamları](v2-permissions-and-consent.md). Bir kapsam izni almak çağıran uygulama için kullanıcı akışı sırasında kapsamına onaylaması gerekir. Microsoft kimlik platformu uç noktası kullanıcı izni ister ve ardından Web API alan tüm erişim belirteçlerinde izinleri kaydeder. Web API'si her çağrıda alır ve yetkilendirme denetimleri gerçekleştirir erişim belirteçlerini doğrular.

Bir Web API uygulamaları, web sunucu uygulamaları, masaüstü ve mobil uygulamalar, tek sayfalık uygulamalar, sunucu tarafı Daemon'ları ve hatta diğer Web API'leri dahil olmak üzere tüm türlerden erişim belirteçleri alabilir. Web API'si için üst düzey akış şöyle görünür:

![Web API'si kimlik doğrulama akışı](./media/v2-app-types/convergence-scenarios-webapi.svg)

Web API kod örnekleri kullanıma OAuth2 erişim belirteçleri kullanarak Web API'si güvenliğini sağlamayı öğrenmek için [Başlarken Microsoft kimlik platformu](v2-overview.md#getting-started) bölümü.

Çoğu durumda, web API'leri de Giden istekleri diğer Aşağı Akış Web API'leri, Microsoft kimlik platformu tarafından güvenlik altına almanız gereken. Web API'leri avantajlarından yararlanabilirsiniz. Bunu yapmayı **On-Behalf-Of** akışı Giden istekleri kullanılmak üzere başka bir erişim belirteci için gelen bir erişim belirteci Exchange web API'si sağlar. Daha fazla bilgi için bkz. [Microsoft kimlik platformu ve OAuth 2.0 On-Behalf-Of flow](v2-oauth2-on-behalf-of-flow.md).

## <a name="mobile-and-native-apps"></a>Mobil ve yerel uygulamalar

Mobil ve Masaüstü uygulamaları gibi cihaz olarak yüklenen uygulamalar genellikle arka uç hizmetlerine veya Web API'leri, verilerini depolamak ve bir kullanıcı adına işlevleri gerçekleştiren erişmeniz gerekebilir. Bu uygulamaları oturum açma ve yetkilendirme için arka uç hizmetlerini kullanarak ekleyebilirsiniz [OAuth 2.0 yetkilendirme kod akışı](v2-oauth2-auth-code-flow.md).

Kullanıcı oturum açtığında bu akışta uygulama Microsoft kimlik platformu uç noktasından bir yetkilendirme kodunu alır. Yetkilendirme kodu uygulamanın, oturum açmış kullanıcı adına arka uç hizmetlerini çağırma izni temsil eder. Uygulama arka planda bir OAuth 2.0 erişim belirteci ve yenileme belirteci için yetkilendirme kodunu değiştirebilir. Uygulama, Web API'leri için HTTP isteklerinde kimlik doğrulaması için erişim belirtecini kullanır ve eski erişim belirteçlerin süresi dolduğunda yeni erişim belirteçlerini almak için yenileme belirtecini kullanın.

![Yerel uygulama kimlik doğrulama akışı](./media/v2-app-types/convergence-scenarios-native.svg)

## <a name="daemons-and-server-side-apps"></a>Daemon'ları ve sunucu tarafı uygulamalar

Uzun süre çalışan işlemler sahip olan veya bir kullanıcı etkileşimi olmadan çalışan uygulamalar da Web API'leri gibi güvenli kaynaklara erişmek için bir yol gerekir. Bu uygulamalar, kimlik doğrulaması ve uygulamanın kimliğini kullanarak belirteçleri almak yerine bir kullanıcının kimliği OAuth 2.0 istemci kimlik bilgileri akışı ile temsilci. Uygulamanın kimliğini istemci parolası veya sertifika kullanarak kanıtlayabilirsiniz. Daha fazla bilgi için bkz. [Microsoft kimlik platformu sertifikaları ile arka plan programı uygulamalarında kimlik doğrulama işlemi](https://azure.microsoft.com/resources/samples/active-directory-dotnet-daemon-certificate-credential/).

Bu akışta uygulama doğrudan etkileşime `/token` erişim edinmek için uç nokta:

![Arka plan programı uygulama kimlik doğrulama akışı](./media/v2-app-types/convergence-scenarios-daemon.svg)

Bir arka plan programı uygulaması oluşturmak için bkz. [istemci kimlik bilgileri belgeleri](v2-oauth2-client-creds-grant-flow.md), veya bir [.NET örnek uygulaması](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
