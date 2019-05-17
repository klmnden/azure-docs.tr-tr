---
title: Azure Active Directory'de yerel uygulamalar
description: Akış Protokolü, kayıt ve bu uygulama türü için belirteci süre sonu yerel uygulamaları nelerdir ve temel bilgileri açıklar.
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
ms.openlocfilehash: a6bf24124c4b072a64ef59500b2f723ff6abbb0e
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545851"
---
# <a name="native-apps"></a>Yerel uygulamalar

Bir kullanıcı adına bir web API'si çağırma uygulamaları yerel uygulamalardır. Bu senaryo genel bir istemci ile OAuth 2.0 yetkilendirme kodu verme türü, 4.1 bölümünde anlatıldığı gibi üzerine inşa edilmiştir [OAuth 2.0 belirtimini](https://tools.ietf.org/html/rfc6749). Yerel uygulama OAuth 2.0 protokolünü kullanarak bu kullanıcı için bir erişim belirteci alır. Bu erişim belirteci daha sonra kullanıcı yetkisi verir ve istenen kaynağa döndüren API web isteği gönderilir.

## <a name="diagram"></a>Diyagram

![Web API'si diyagramı yerel uygulama](./media/authentication-scenarios/native_app_to_web_api.png)

## <a name="protocol-flow"></a>Akış Protokolü

Protokol aşağıda açıklanan ayrıntıların birçoğu AD kimlik doğrulama kitaplıkları kullanıyorsanız, tarayıcı açılır pencere, belirteç önbelleğe alma ve yenileme belirteçleri işlenmesini gibi işlenir.

1. Yerel uygulama yetkilendirme uç noktası için Azure AD'de istekte açılan bir tarayıcı kullanarak. Bu istek, uygulama kimliği ve yeniden yönlendirme URI'si yerel uygulamanın gösterildiği Azure portalı ve web API'si için uygulama kimliği URI'si içerir. Kullanıcı zaten oturum taşınmadığından, bunlar yeniden oturum açmanız istenir
1. Azure AD, kullanıcının kimliğini doğrular. Çok kiracılı bir uygulama ise ve uygulamayı kullanma izni gereklidir, kullanıcı Bunlar zaten yapmadıysanız onay gerekecektir. Onay verme sonra ve başarılı kimlik doğrulamadan sonra bir yetkilendirme kodu yanıt istemci uygulamanın yeniden yönlendirme URI'si Azure AD'ye verir.
1. Azure AD dön yeniden yönlendirme URI'si bir yetkilendirme kodu yanıt verdiğinde, istemci uygulama tarayıcı etkileşimi durdurur ve yetkilendirme kodu yanıttan ayıklar. Bu yetkilendirme kodunu kullanarak, istemci uygulama bir istek yetki kodunu içerir, istemci uygulaması (uygulama kimliği ve yeniden yönlendirme URI'si) ve istenen kaynağın ayrıntıları Azure AD'nin belirteç uç noktası gönderir (uygulama kimliği URI'si için Web API'si için).
1. Yetkilendirme kodunu ve istemci uygulaması ve web API'si hakkında bilgi, Azure AD tarafından doğrulanır. Doğrulama başarıyla tamamlandıktan sonra Azure AD iki belirteçleri döndürür: JWT erişim belirteci ve JWT yenileme belirteci. Ayrıca, Azure AD, görünen adı ve Kiracı kimliği gibi kullanıcıyla ilgili temel bilgileri döndürür
1. HTTPS üzerinden istemci uygulama bir "Bearer" atama JWT dizesiyle isteğin yetkilendirme üst bilgisinde web API'sine eklemek için döndürülen JWT erişim belirtecini kullanır. Web API'si daha sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.
1. Erişim belirtecinin süresi dolduğunda, istemci uygulama, kullanıcının yeniden kimlik doğrulaması yapması belirten bir hata alırsınız. Uygulamanın geçerli yenileme belirteci varsa yeniden oturum açmak için kullanıcıya sormadan yeni bir erişim belirteci almak için kullanılabilir. Yenileme belirtecinin süresi dolarsa, uygulamanın etkileşimli kullanıcı yeniden kimlik doğrulaması yapmanız gerekir.

> [!NOTE]
> Azure AD tarafından verilen bir yenileme belirteci, birden çok kaynağa erişmek için kullanılabilir. Örneğin, iki web API'lerini çağırma izni olan bir istemci uygulaması varsa, yenileme belirtecini erişim diğer web API'sine de belirteci almak için kullanılabilir.

## <a name="code-samples"></a>Kod örnekleri

Web API senaryoları için yerel uygulama için kod örneklere bakın. Ve sıkça tekrar kontrol edin - yeni örnekler sık ekleriz. [Web API'si yerel uygulamaya](sample-v1-code.md#desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api).

## <a name="app-registration"></a>Uygulama kaydı

Bir uygulamayı Azure AD'ye v1.0 uç noktası ile kaydetmek için bkz: [bir uygulamayı kaydetme](quickstart-register-app.md).

* Tek Kiracı - hem yerel bir uygulama ve web API'si aynı dizinde Azure AD'de kayıtlı olmalıdır. Web API'si, yerel uygulamanın kaynaklarına erişimi sınırlamak için kullanılan bir izin kümesi kullanıma sunmak için yapılandırılabilir. İstemci uygulama, Azure portalında "İzinleri için diğer uygulamaları" açılan menüsünden istenen izinleri seçer.
* Çok kiracılı - ilk olarak, yerel uygulama her zaman sadece bir geliştirici veya yayımcının dizinde kayıtlı. İkinci olarak, yerel uygulama işlevsel olmasını gerektiren izinleri belirtmek için yapılandırılır. Bir kullanıcının veya yöneticinin hedef dizinde kuruluşları için kullanılabilir hale getirir uygulamaya izin verilirse bu gerekli izinlerin listesi iletişim kutusunda gösterilir. Bazı uygulamalar, yalnızca kuruluşunuzdaki herhangi bir kullanıcı onay verebildiği kullanıcı düzeyi izinleri gerektirir. Diğer uygulamalar, kuruluşunuzdaki bir kullanıcı onay veremez yönetici düzeyi izinlerini gerektirir. Dizin Yöneticisi yalnızca bu izin düzeyini gerektiren uygulamalar için izin verebilirsiniz. Kullanıcı veya yönetici onay verdiğinde, yalnızca web API'si, dizinde kayıtlı. 

## <a name="token-expiration"></a>Belirteç süre sonu

Yerel uygulama, JWT erişim belirteci almak için yetkilendirme kodunu kullandığında, ayrıca bir JWT yenileme belirtecini alır. Erişim belirtecinin süresi dolduğunda yenileme belirtecini kullanıcının tekrar oturum açmaya gerek kalmadan yeniden kimlik doğrulaması için kullanılabilir. Bu yenileme belirteci daha sonra kullanıcının kimliğini doğrulamak için kullanılan bir yeni bir erişim belirteci ve yenileme belirteci sonuçlanır.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer hakkında daha fazla bilgi [uygulama türleri ve senaryolar](app-types.md)
- Azure AD hakkında bilgi edinin [kimlik doğrulaması temelleri](authentication-scenarios.md)
