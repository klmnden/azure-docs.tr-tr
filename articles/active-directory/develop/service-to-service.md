---
title: Azure Active Directory'de Hizmet uygulamalar
description: Akış Protokolü, kayıt ve bu uygulama türü için belirteci süre sonu hangi hizmet uygulamalar ve temel bilgileri açıklar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 09/24/2018
ms.date: 11/07/2018
ms.author: v-junlch
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.openlocfilehash: e0ced89ce97d5f22270d9968fdeb0ddb3fad1e4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251989"
---
# <a name="service-to-service-apps"></a>Hizmet uygulamalar

Hizmetten hizmete uygulamaları bir web API'sini kaynakları almak için gereken arka plan programı veya sunucu bir uygulama olabilir. Bu bölüm için geçerli iki alt senaryo vardır:

- OAuth 2.0 istemci kimlik bilgileri verme türünü üzerinde oluşturulmuş bir web API'si çağırmayı gerektiren bir arka plan programı

    Bu senaryoda birkaç şey anlamak önemlidir. İlk olarak, kullanıcı etkileşimi uygulamayı kendi kimliğini gerektirir ve arka plan programı uygulama ile mümkün değildir. Daemon uygulamasının bir batch işi veya arka planda çalışan bir işletim sistemi hizmet örneğidir. Bu tür bir uygulama bir erişim belirteci kullanarak uygulama kimliğini ve bunun uygulama kimliği, kimlik bilgisi (parola veya sertifika) ve uygulama sunma ister Azure AD'ye kimliği URI'si. Başarılı kimlik doğrulamasından sonra arka plan programı, web API'sini çağırmak için kullanılır, Azure AD'den bir erişim belirteci alır.

- Web API'si, OAuth 2.0 On-Behalf-Of taslak belirtimi yerleşik çağırmak için gereken sunucu uygulaması (örneğin, bir web API'si)

    Bu senaryoda, bir kullanıcı, yerel bir uygulama üzerinde yapıldığını ve bu yerel uygulama bir web API'sini çağırmak gereken düşünün. Azure AD, web API'sini çağırmak için bir JWT erişim belirteci verir. Web API'si, başka bir aşağı akış web API'si çağırma yapması gerekiyorsa, kullanıcının kimliğini temsilci ve ikinci katman web API'sine kimliğini doğrulamak için on-behalf-of akışı kullanabilirsiniz.

## <a name="diagram"></a>Diyagram

![Daemon veya sunucu uygulaması için Web API'si diyagramı](./media/authentication-scenarios/daemon_server_app_to_web_api.png)

## <a name="dprotocol-flow"></a>DProtocol akışı

### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Uygulama kimliği ile OAuth 2.0 istemci kimlik bilgileri verme

1. İlk olarak, insan etkileşimi gibi etkileşimli bir oturum açma iletişim kutusu olmadan kendisini olarak Azure AD kimlik doğrulaması gerçekleştirmek sunucu uygulaması gerekir. Kimlik bilgisi, uygulama kimliği ve uygulama kimliği URI'si sağlama Azure AD'nin belirteç uç noktası için bir talep gönderir.
1. Azure AD uygulama ve web API'sini çağırmak için kullanılan bir JWT erişim belirtecini döndürür.
1. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle isteğin yetkilendirme üst bilgisinde web API'sine eklemek için döndürülen JWT erişim belirtecini kullanır. Web API'si daha sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Yetkilendirilmiş kullanıcının kimliği OAuth 2.0 On-Behalf-Of taslak belirtimi ile

Aşağıda açıklanan akış, bir kullanıcı başka bir uygulamada (örneğin, yerel bir uygulama) kimliği ve kullanıcı kimliklerini ilk katman Web API'sine erişim belirteci almak için kullanılan varsayar.

1. Yerel uygulama erişim belirteci ilk katman web API'sine gönderir.
1. İlk katman web API'si, kullanıcının erişim belirtecini yanı sıra uygulama Kimliğini ve kimlik bilgilerini sağlayarak Azure AD'nin belirteç uç noktası için bir istek gönderir. Ayrıca, istek ile gönderilen bir özgün kullanıcı adına bir aşağı akış web API'sini çağırmak için yeni bir belirteç isteyen web gösteren parametresi on_behalf_of API.
1. Azure AD, ilk katman web API'si, ikinci katman web API'sine erişim izni olduğundan ve döndüren bir JWT belirteç isteği doğrular ve bir JWT belirteci ilk katman Web API'sine Yenile doğrular.
1. HTTPS üzerinden ilk katman web API'si daha sonra ikinci katman web API'si istekteki yetkilendirme üst bilgisi belirteç dizesinde ekleyerek çağırır. İlk katman web API'sine erişim belirteci ve yenileme belirteçleri geçerli olduğu sürece ikinci katman web API'sini çağırmak devam edebilirsiniz.

## <a name="code-samples"></a>Kod örnekleri

Daemon veya Web API senaryoları için sunucu uygulaması için kod örneklere bakın. Ve, sık sık yeni örnekler eklendikçe yeniden denetleyin. [Sunucu veya Web API arka plan programı uygulama](sample-v1-code.md#daemon-applications-accessing-web-apis-with-the-applications-identity)

## <a name="app-registration"></a>Uygulama kaydı

- Tek Kiracı - uygulama kimliği ve yetkilendirilmiş kullanıcının için kimliğinin servis talepleri, daemon veya Azure AD'de sunucu uygulaması aynı dizinde kaydedilmesi gerekir. Web API'si, bir arka plan programı veya sunucunun kaynaklarına erişimi sınırlamak için kullanılan bir izin kümesi kullanıma sunmak için yapılandırılabilir. Yetkilendirilmiş kullanıcının kimlik türü kullanılıyorsa, Azure portalında "İzinleri için diğer uygulamaları" aşağı açılan menüden istediğiniz izinleri seçin sunucu uygulaması gerekir. Uygulama kimlik türü kullanılıyorsa bu adım gerekli değildir.
- Çok kiracılı-First, arka plan programı veya sunucu uygulaması, işlev olmasını gerektiren izinleri belirtmek için yapılandırılır. Bir kullanıcının veya yöneticinin hedef dizinde kuruluşları için kullanılabilir hale getirir uygulamaya izin verilirse bu gerekli izinlerin listesi iletişim kutusunda gösterilir. Bazı uygulamalar, yalnızca kuruluşunuzdaki herhangi bir kullanıcı onay verebildiği kullanıcı düzeyi izinleri gerektirir. Diğer uygulamalar, kuruluşunuzdaki bir kullanıcı onay veremez yönetici düzeyi izinlerini gerektirir. Dizin Yöneticisi yalnızca bu izin düzeyini gerektiren uygulamalar için izin verebilirsiniz. Kullanıcı veya yönetici onay verdiğinde, her iki web API'leri, dizinde kayıtlı.

## <a name="token-expiration"></a>Belirteç süre sonu

İlk uygulama, JWT erişim belirteci almak için yetkilendirme kodunu kullandığında, ayrıca JWT yenileme belirtecini alır. Erişim belirtecinin süresi dolduğunda yenileme belirteci kullanıcı kimlik bilgileri istemeden yeniden kimlik doğrulaması için kullanılabilir. Bu yenileme belirteci daha sonra kullanıcının kimliğini doğrulamak için kullanılan bir yeni bir erişim belirteci ve yenileme belirteci sonuçlanır.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer hakkında daha fazla bilgi [uygulama türleri ve senaryolar](app-types.md)
- Azure AD hakkında bilgi edinin [kimlik doğrulaması temelleri](authentication-scenarios.md)

