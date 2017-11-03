---
title: "Azure Active Directory v2.0 uç noktası için uygulama türleri | Microsoft Docs"
description: "Uygulamalar ve Azure Active Directory v2.0 uç noktası tarafından desteklenen senaryoları türleri."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 9d59e7f0e8f326c40be86e199d7712f6c565cc13
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="app-types-for-the-azure-active-directory-v20-endpoint"></a>Azure Active Directory v2.0 uç noktası için uygulama türleri
Azure Active Directory (Azure AD) v2.0 uç tümünün endüstri standardı protokollerine dayalı modern uygulama mimarilerinin çeşitli kimlik doğrulamasını destekliyor [OAuth 2.0 veya Openıd Connect](active-directory-v2-protocols.md). Bu makalede, tercih edilen dilden veya platformdan bağımsız olarak Azure AD v2.0 kullanarak oluşturabileceğiniz uygulama türleri açıklanmaktadır. Bu makaledeki bilgileri, önce üst düzey senaryoları anlamanıza yardımcı olmak için tasarlanmış [kodu ile çalışma başlangıç](active-directory-appmodel-v2-overview.md#getting-started).

> [!NOTE]
> V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez. V2.0 uç kullanması gerekip gerekmediğini belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

## <a name="the-basics"></a>Temel bilgiler
V2.0 uç kullanan her bir uygulama kaydettirmelisiniz [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com). Uygulama kayıt işlemi toplar ve uygulamanız için bu değerleri atar:

* Bir **uygulama kimliği** uygulamanızı benzersiz olarak tanımlayan
* A **yeniden yönlendirme URI'si** yanıtları uygulamanıza geri yönlendirmek için kullanabileceğiniz
* Birkaç diğer senaryoya özel değerler

Ayrıntılar için bilgi nasıl [uygulama kaydetmeyi](active-directory-v2-app-registration.md).

Uygulama kaydedildikten sonra uygulamayı Azure AD v2.0 uç noktasına istek göndererek Azure AD ile iletişim kurar. Açık kaynak çerçeveler ve bu istekleri ayrıntılarını işlemek kitaplıklar sunuyoruz. Ayrıca, kimlik doğrulaması mantığı kendiniz Bu uç noktalar isteklerine oluşturarak uygulayacaksınız seçeneğiniz de vardır:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Web uygulamaları
Kullanıcı bir tarayıcıdan erişen web uygulamaları için (.NET, PHP, Java, Ruby, Python, düğüm), kullandığınız [Openıd Connect](active-directory-v2-protocols.md) kullanıcı oturum açma için. Openıd Connect web uygulaması kimliği belirteci alır. Bir kimliği belirteci, kullanıcının kimliğini doğrular ve talep biçiminde kullanıcı hakkında bilgi sağlayan bir güvenlik belirteci şöyledir:

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

Bir uygulama için kullanılabilir talep ve belirteç tüm türleri hakkında bilgi edinebilirsiniz [v2.0 belirteçler başvuru](active-directory-v2-tokens.md).

Web sunucusu uygulamalarında oturum açma kimlik doğrulaması akışı aşağıdaki üst düzey adımları gerçekleştirir:

![Web uygulama kimlik doğrulama akışı](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Kullanıcının kimliğini v2.0 uç noktasından alınan bir ortak imzalama anahtarı ile kimliği belirteci doğrulayarak emin olabilirsiniz. Sonraki sayfa isteklerinde kullanıcı tanımlamak için kullanılabilecek oturum tanımlama bilgisi ayarlanır.

Bu senaryoyu çalışırken görmek için bizim v2.0 web uygulaması oturum açma kodu örneklerinden birini deneyin [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) bölümü.

Basit oturum açma ek olarak, bir REST API'si gibi başka bir web hizmetine erişmek bir web sunucusu uygulaması gerekebilir. Bu durumda, web sunucusu uygulaması kullanarak birleşik bir Openıd Connect ve OAuth 2.0 akışı, prosese [OAuth 2.0 yetkilendirme kodu akışını](active-directory-v2-protocols.md). Bu senaryo hakkında daha fazla bilgi için bilgiyi [web uygulamaları ve Web API ile çalışmaya başlama](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Web API'leri
Uygulamanızın RESTful Web API'si gibi web hizmetlerini güvence altına alma v2.0 uç noktası kullanabilirsiniz. Kimlik belirteçlerini ve oturum tanımlama bilgileri yerine, bir Web API, verilerin güvenliğini sağlamak için ve gelen isteklerin kimliğini doğrulamak için bir OAuth 2.0 erişim belirteci kullanır. Bir Web API'si çağıran bir erişim belirteci bu gibi bir HTTP isteğinin yetkilendirme üst ekler:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Web API API çağıranının kimliğini doğrulamak ve erişim belirteçte kodlanmış taleplere çağıran hakkında bilgi ayıklamak için erişim belirtecini kullanır. Bir uygulama için kullanılabilir talep ve belirteç tüm türleri hakkında bilgi edinmek için [v2.0 belirteçler başvuru](active-directory-v2-tokens.md).

Bir Web API kullanıcıların kabul veya belirli işlevleri veya veri yetersiz izinler olarak da bilinen göstererek opt güç verebilirsiniz [kapsamları](active-directory-v2-scopes.md). Bir kapsam için izin almak üzere bir arama uygulaması için kullanıcı akışı sırasında kapsama onaylaması gerekir. V2.0 uç noktası kullanıcı için izin ister ve sonra Web API alan tüm erişim belirteçleri izinleri kaydeder. Web API her çağrıda alır ve yetkilendirme denetimleri gerçekleştirir erişim belirteci doğrular.

Bir Web API uygulamaları, web sunucu uygulamaları, masaüstü ve mobil uygulamalar, tek sayfa uygulamaları, sunucu tarafı deamon'lar ve hatta diğer Web API'leri dahil olmak üzere tüm türlerinden erişim belirteçleri alabilir. Bir Web API için üst düzey akışı şu şekildedir:

![Web API kimlik doğrulama akışı](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Web API kod örnekleri kullanıma OAuth2 erişim belirteçleri kullanarak Web API'SİNİN güvenliğini öğrenmek için bizim [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) bölümü.

Çoğu durumda, web API'leri de Giden istekleri diğer Aşağı Akış Web API'leri Azure Active Directory tarafından güvenlik altına almanız gereken.  Bunu yapmak için web API'leri, Azure AD yararlanabilir **adına, üzerinde** Giden istekleri kullanılacak başka bir erişim belirteci için gelen bir erişim belirteci Exchange web API'si sağlar akış.  V2.0 uç noktanın akış adına açıklanan [burada ayrıntı](active-directory-v2-protocols-oauth-on-behalf-of.md).

## <a name="mobile-and-native-apps"></a>Mobil ve yerel uygulamalar
Mobil ve Masaüstü uygulamaları gibi cihaz yüklü uygulamalar genellikle arka uç hizmetlerine veya verileri depolamak ve bir kullanıcı adına işlevleri gerçekleştirmek Web API'lerine erişmesi gerekir. Bu uygulamaları oturum açma ve yetkilendirme için arka uç hizmetlerini kullanarak ekleyebilirsiniz [OAuth 2.0 yetkilendirme kodu akışını](active-directory-v2-protocols-oauth-code.md).

Kullanıcı oturum açtığında bu akışta uygulama v2.0 uç noktasından bir yetkilendirme kodu alır. Uygulamanın, oturum açan kullanıcı adına arka uç hizmetlerini çağırma izni yetki kodunu temsil eder. Uygulama arka planda bir OAuth 2.0 erişim belirteci ve bir yenileme belirteci için yetkilendirme kodu değiştirebilir. Uygulama için Web API HTTP isteklerinde kimlik doğrulaması için erişim belirtecini kullanır ve eski erişim belirteçleri sona erdiğinde, yeni erişim belirteçleri almak için yenileme belirteci kullanın.

![Yerel uygulama kimlik doğrulama akışı](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Tek sayfa uygulamaları (JavaScript)
Birçok modern uygulamanın, JavaScript'te öncelikli olarak yazılmış tek sayfa uygulaması ön ucu sahip. Sıklıkla AngularJS, Ember.js veya Durandal.js gibi framework kullanılarak yazılır. Azure AD v2.0 uç kullanarak bu uygulamaları destekler [OAuth 2.0 örtük akışını](active-directory-v2-protocols-implicit.md).

Bu akışta uygulama belirteçlerini doğrudan v2.0 aldığı herhangi bir sunucudan sunucuya alışverişleri olmadan son noktanın yetkilendirilmesi. Tüm kimlik doğrulaması mantığı ve gerçekleştirilen işlemlerin işleme oturumu tamamen ek sayfa yeniden yönlendirmeleri olmadan JavaScript istemci yerleştirin.

![Örtük kimlik doğrulama akışı](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Bu senaryoyu çalışırken görmek için tek sayfalı uygulama kodu örneklerinden birini deneyin bizim [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) bölümü.

## <a name="daemons-and-server-side-apps"></a>Arka plan programları ve sunucu tarafı uygulamalar
Uzun süre çalışan işlemler olması veya, bir kullanıcı etkileşimi olmadan çalışan uygulamalar da Web API'leri gibi güvenli kaynaklara erişmek için bir yol gerekir. Bu uygulamaları kimlik doğrulaması ve uygulamanın kimliğini kullanarak belirteçleri almak yerine bir kullanıcı kimliğiyle OAuth 2.0 istemci kimlik bilgileri akışının temsilcisi.

Bu akışta uygulama doğrudan etkileşimde `/token` uç noktaları edinmek için uç nokta:

![Arka plan programı uygulama kimlik doğrulama akışı](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Arka plan programı uygulamanızı oluşturmak için istemci kimlik bilgileri belgelerine bakın bizim [Başlarken](active-directory-appmodel-v2-overview.md#getting-started) bölümünde veya deneyin bir [.NET örnek uygulaması](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
