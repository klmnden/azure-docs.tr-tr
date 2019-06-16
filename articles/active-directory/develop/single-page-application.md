---
title: Azure Active Directory'de tek sayfalı uygulamalar
description: Tek sayfa uygulamaları (Spa'lar) açıklayan olan ve temel Protokolü akış, kayıt ve bu uygulama türü için belirteci süre sonu.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f6f66779bec9ed4e38e5a662c2d3728ba2034b6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545293"
---
# <a name="single-page-applications"></a>Tek sayfa uygulamaları

Tek sayfa uygulamaları (Spa'lar), genellikle tarayıcısında çalışan bir JavaScript sunu katmanı (ön uç) ve uygulamanın iş mantığını uygular ve çalıştıran bir sunucuya bir web API arka ucu yapılandırılmıştır. Örtük yetki verme hakkında daha fazla bilgi edinin ve Uygulama senaryonuz için doğru olup olmadığını karar vermenize yardımcı olması için bkz: [anlama OAuth2 örtük verme flow'da Azure Active Directory](v1-oauth2-implicit-grant-flow.md).

Bu senaryoda, kullanıcı oturum açtığında JavaScript'in ön uç kullanan [Active Directory Authentication Library (ADAL JavaScript için. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) ve bir kimlik belirteci (id_token) Azure AD'den elde etmek için örtük yetki verme. Belirteç önbelleğe alınır ve istemci bunu isteği taşıyıcı belirteci olarak, Web API arka ucu, OWIN ara yazılımı kullanarak güvenliği çağrı yaparken ekler.

## <a name="diagram"></a>Diyagram

![Tek sayfalı uygulama diyagramı](./media/authentication-scenarios/single_page_app.png)

## <a name="protocol-flow"></a>Akış Protokolü

1. Kullanıcı, web uygulamasına gider.
1. Uygulamayı tarayıcıda JavaScript ön uç (sunu katmanı) döndürür.
1. Kullanıcı, oturum açma, örneğin bir oturum açma bağlantısına tıklayarak başlatır. Tarayıcı, bir kimlik belirteci istemek için Azure AD yetkilendirme uç noktası için bir GET gönderir. Bu istek sorgu parametrelerinde uygulama kimliği ve yanıt URL'sini içerir.
1. Azure AD yanıt URL'si Azure portalında yapılandırılmış kayıtlı yanıt URL'si karşı doğrular.
1. Oturum açma sayfasında oturum açtığında kullanıcı.
1. Kimlik doğrulaması başarılı olursa, Azure AD kimlik belirteci oluşturur ve uygulamanın yanıt URL'si için bir URL parçası belirtmesini (#) döndürür. Bir üretim uygulaması için bu yanıt URL'si HTTPS olmalıdır. Döndürülen belirteç, Azure AD ve kullanıcı belirteci doğrulamak için uygulama tarafından gerekli olan talepleri içerir.
1. Tarayıcıda çalışan JavaScript istemci kodu uygulamanın web API geri son çağrısına güvenliğini kullanılacak yanıt belirteç ayıklar.
1. Geri API ile kimlik belirteci yetkilendirme üst bilgisinde son uygulamanın web tarayıcı çağırır. Azure AD kimlik doğrulama hizmeti kaynak istemci kimliği ile aynı olduğunda, bir taşıyıcı belirteci kullanılabilir bir kimlik belirteci veren (uygulamanın kendi arka uç web API'si olduğu gibi bu durumda, true olur).

## <a name="code-samples"></a>Kod örnekleri

Bkz: [kod örneklerini tek sayfalı uygulama senaryoları için](sample-v1-code.md#single-page-applications). Geri kontrol ettiğinizden emin olun sık sık yeni örnekler eklendikçe.

## <a name="app-registration"></a>Uygulama kaydı

* Tek bir Kiracı - yalnızca kuruluşunuz için bir uygulama oluşturuyorsanız, bunu şirketinizin dizininde Azure portalını kullanarak kayıtlı olması gerekir.
* Çok kiracılı - kuruluşunuzun dışındaki kullanıcılar tarafından kullanılabilen bir uygulama oluşturuyorsanız bunu şirketinizin dizinde kayıtlı olması gerekir, ancak da uygulamayı kullanarak her kuruluşun dizininde kayıtlı olması gerekir. Uygulamanızı directory'lerinde kullanılabilir hale getirme için uygulamanıza onay verme tanıyan müşterileriniz için bir kayıt işlemi dahil edebilirsiniz. Bunlar, uygulamanız için oturum açarken uygulama izinleri gösteren bir iletişim kutusu, ardından onay seçeneği ile sunulur. Gerekli izinlere bağlı olarak diğer kuruluştaki bir yöneticisinin izni vermek için gerekebilir. Kullanıcı veya yönetici onay verdiğinde, uygulama, dizinde kayıtlı.

Uygulamayı kaydettikten sonra OAuth 2.0 örtülü izin protokolünü kullanmak için yapılandırılmalıdır. Varsayılan olarak, bu protokolü, uygulamalar için devre dışıdır. Uygulamanız için OAuth2 örtük verme Protokolü etkinleştirmek için Azure portalından, uygulama bildirimini düzenleme ve "oauth2AllowImplicitFlow" değeri true olarak ayarlayın. Daha fazla bilgi için bkz. [uygulama bildirimini](reference-app-manifest.md).

## <a name="token-expiration"></a>Belirteç süre sonu

İle ADAL.js kullanarak yardımcı olur:

* süresi dolmuş bir belirteç yenileniyor
* bir web API'si kaynağına çağırmak için bir erişim belirteci istenirken

Başarılı bir kimlik doğrulamasından sonra Azure AD, kullanıcının tarayıcısında oturum oluşturmak için bir tanımlama bilgisi yazar. Azure AD (arasında değil, kullanıcı ve web uygulaması) ve kullanıcı arasında oturum var unutmayın. Bir belirtecin süresi dolduğunda, ADAL.js sessizce farklı bir belirteç almak için bu oturumu kullanır. ADAL.js gizli bir iFrame OAuth örtük vermeyi protokolü kullanarak istek alıp göndermek için kullanır. ADAL.js sessizce destek çıkış noktaları arası kaynak paylaşımı (CORS), bu kaynakları sürece uygulama çağırır ve diğer web API'si kaynaklarının kullanıcının dizinde kayıtlı ve tüm gerekli izni olan erişim belirteçleri almak için de aynı Bu mekanizma kullanabilirsiniz oturum açma sırasında kullanıcı tarafından verilir.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında daha fazla bilgi [uygulama türleri ve senaryolar](app-types.md)
* Azure AD hakkında bilgi edinin [kimlik doğrulaması temelleri](authentication-scenarios.md)
