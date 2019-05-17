---
title: Web API uygulamaları Azure Active Directory'de
description: Web API uygulamaları nelerdir ve temel Protokolü akış, kayıt ve bu uygulama türü için belirteci süre sonu açıklar.
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
ms.openlocfilehash: 484e6b4c5f0e064254c957b07b8ba15ef98f2634
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545221"
---
# <a name="web-api"></a>Web API'si

Web API uygulamaları bir web API'sini kaynakları almak için gereken web uygulamalardır. Bu senaryoda, web uygulamasının kimliğini doğrulamak ve web API'sini çağırmak için kullanabileceğiniz iki kimlik türü vardır:

- **Uygulama Kimliği** -bu senaryo uygulamanın kimliğini doğrulamak ve web API'sine erişmek için OAuth 2.0 istemci kimlik bilgileri verme kullanır. Uygulama kimliği, web API'si, yalnızca web uygulaması, aradığı algılayabilir kullanırken web kullanıcı hakkındaki tüm bilgileri API almaz. Uygulama kullanıcı hakkındaki bilgileri alırsa, uygulama protokol üzerinden gönderilir ve Azure AD tarafından imzalanmadı. Web API'si, web uygulaması kullanıcının kimliğinin güvenir. Bu nedenle, bu düzen, bir güvenilir alt sistem adı verilir.
- **Yönetici temsilcisi kullanıcı kimliğini** -bu senaryo iki şekilde gerçekleştirilebilir: İle gizli bir istemci, Openıd Connect ve OAuth 2.0 yetkilendirme kodu verme. Web uygulaması, web API'sine kullanıcı, web uygulamasını başarıyla kimlik doğrulaması ve web uygulaması web API'sini çağırmak için bir yetkilendirilmiş kullanıcının kimliği alabilmiş kanıtlar kullanıcı için bir erişim belirteci alır. Bu erişim belirteci kullanıcı yetkisi verir ve istenen kaynağa döndüren API web isteği gönderilir.

Uygulama kimliği ve yetkilendirilmiş kullanıcının kimlik türleri flow'da ele alınmıştır. Aralarındaki temel fark, kullanıcı oturum açın ve Web API'sine erişmek için önce yetkilendirilmiş kullanıcının kimliğini ilk kez bir yetkilendirme kodu almalıdır ' dir.

## <a name="diagram"></a>Diyagram

![Web uygulaması Web API'si diyagramı](./media/authentication-scenarios/web_app_to_web_api.png)

## <a name="protocol-flow"></a>Akış Protokolü

### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Uygulama kimliği ile OAuth 2.0 istemci kimlik bilgileri verme

1. Bir kullanıcı Azure AD web uygulamasında oturum açmış (bkz **Web uygulamaları** bölümünde daha fazla bilgi için).
1. Web API'si için kimlik doğrulaması yapmak ve almak istediğiniz kaynak bir erişim belirteci almak web uygulaması gerekir. Kimlik bilgisi, uygulama kimliği ve web API'SİNİN uygulama kimliği URI'si sağlama Azure AD'nin belirteç uç noktası için bir talep gönderir.
1. Azure AD uygulama ve web API'sini çağırmak için kullanılan bir JWT erişim belirtecini döndürür.
1. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle isteğin yetkilendirme üst bilgisinde web API'sine eklemek için döndürülen JWT erişim belirtecini kullanır. Web API'si daha sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

### <a name="delegated-user-identity-with-openid-connect"></a>Openıd Connect ile yetkilendirilmiş kullanıcının kimliği

1. Bir kullanıcının Azure AD kullanarak bir web uygulamasına oturum açtığı (yukarıdaki bölümde Web uygulaması için Web tarayıcısı bakın). Web uygulamasının kullanıcı henüz web uygulaması, kendi adına web API'sini çağırmak izin verme durumuna değil etmişse, kullanıcı onayı gerekir. Uygulamanın gerektirdiği izinleri görüntülenir ve aşağıdakilerden herhangi biri yönetici düzeyi izinlerini varsa, normal bir kullanıcı dizinde onay verme mümkün olmayacaktır. Uygulamanın zaten gerekli izinlere sahip şekilde bu onayı işlemi yalnızca değil tek kiracılı uygulamalar, çok kiracılı uygulamalar için geçerlidir. Kullanıcının oturum açtığı, web uygulaması bir yetkilendirme kodu yanı sıra, kullanıcı hakkında bilgi içeren bir kimlik belirteci alındı.
1. Azure AD tarafından verilen yetkilendirme kodunu kullanarak, web uygulaması bir isteği ayrıntılarını (uygulama kimliği ve yeniden yönlendirme URI'si) istemci uygulaması ve istenen kaynağa (uygulama kimliği yetkilendirme kodu içeren Azure AD'nin belirteç uç noktası gönderir URI web API'si için).
1. Yetkilendirme kodunu ve web uygulaması ve web API'si hakkında bilgi, Azure AD tarafından doğrulanır. Doğrulama başarıyla tamamlandıktan sonra Azure AD iki belirteçleri döndürür: JWT erişim belirteci ve JWT yenileme belirteci.
1. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle isteğin yetkilendirme üst bilgisinde web API'sine eklemek için döndürülen JWT erişim belirtecini kullanır. Web API'si daha sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Yetkilendirilmiş kullanıcının kimliği OAuth 2.0 yetkilendirme kodu verme ile

1. Bir kullanıcı zaten bir web uygulaması için Azure AD, kimlik doğrulama mekanizması bağımsızdır imzalanır.
1. Web uygulamasının uygulama kimliği sağlayarak Azure AD'nin yetkilendirme uç noktasına, tarayıcı üzerinden bir istek sorunları için bir erişim belirteci almak ve başarılı kimlik doğrulamadan sonra web uygulaması için yeniden yönlendirme URI'si bir yetkilendirme kodu gerektirir. Azure AD'de oturum açtığında kullanıcı.
1. Web uygulamasının kullanıcı henüz web uygulaması, kendi adına web API'sini çağırmak izin verme durumuna değil etmişse, kullanıcı onayı gerekir. Uygulamanın gerektirdiği izinleri görüntülenir ve aşağıdakilerden herhangi biri yönetici düzeyi izinlerini varsa, normal bir kullanıcı dizinde onay verme mümkün olmayacaktır. Bu onay, hem tek hem de çok kiracılı uygulama için geçerlidir. Tek kiracılı durumunda yönetici kullanıcıları adına yönetici onayı için onay gerçekleştirebilirsiniz. Bu yapılabilir kullanarak `Grant Permissions` düğmesine [Azure portalında](https://portal.azure.com). 
1. Kullanıcının seçtiği sonra web uygulaması, bir erişim belirteci almak için gereken yetkilendirme kodunu alır.
1. Azure AD tarafından verilen yetkilendirme kodunu kullanarak, web uygulaması bir isteği ayrıntılarını (uygulama kimliği ve yeniden yönlendirme URI'si) istemci uygulaması ve istenen kaynağa (uygulama kimliği yetkilendirme kodu içeren Azure AD'nin belirteç uç noktası gönderir URI web API'si için).
1. Yetkilendirme kodunu ve web uygulaması ve web API'si hakkında bilgi, Azure AD tarafından doğrulanır. Doğrulama başarıyla tamamlandıktan sonra Azure AD iki belirteçleri döndürür: JWT erişim belirteci ve JWT yenileme belirteci.
1. HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle isteğin yetkilendirme üst bilgisinde web API'sine eklemek için döndürülen JWT erişim belirtecini kullanır. Web API'si daha sonra JWT belirteci doğrular ve doğrulama başarılı olursa, istenen kaynak döndürür.

## <a name="code-samples"></a>Kod örnekleri

Web API senaryoları için Web uygulaması için kod örneklere bakın. Ve sıkça tekrar kontrol edin; yeni örnekleri sık eklenir. Web [Web API'si uygulamaya](sample-v1-code.md#web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity).

## <a name="app-registration"></a>Uygulama kaydı

Bir uygulamayı Azure AD'ye v1.0 uç noktası ile kaydetmek için bkz: [bir uygulamayı kaydetme](quickstart-register-app.md).

* -Uygulama kimliği ve yetkilendirilmiş kullanıcının kimliğinin servis talepleri, web uygulaması ve web API'si hem tek bir kiracının aynı dizinde Azure AD'de kayıtlı olması gerekir. Web API'si, bir web uygulamasının kaynaklarına erişimi sınırlamak için kullanılan bir izin kümesi kullanıma sunmak için yapılandırılabilir. Web uygulaması bir yetkilendirilmiş kullanıcının kimlik türü kullanılıyorsa, istenen izinlere seçmesi gerekir **diğer uygulamalara izinler** Azure Portalı'nda açılan menüsü. Uygulama kimlik türü kullanılıyorsa bu adım gerekli değildir.
* Çok kiracılı-ilk olarak, web uygulaması, işlev olmasını gerektiren izinleri belirtmek için yapılandırılır. Bir kullanıcının veya yöneticinin hedef dizinde kuruluşları için kullanılabilir hale getirir uygulamaya izin verilirse bu gerekli izinlerin listesi iletişim kutusunda gösterilir. Bazı uygulamalar, yalnızca kuruluşunuzdaki herhangi bir kullanıcı onay verebildiği kullanıcı düzeyi izinleri gerektirir. Diğer uygulamalar, kuruluşunuzdaki bir kullanıcı onay veremez yönetici düzeyi izinlerini gerektirir. Dizin Yöneticisi yalnızca bu izin düzeyini gerektiren uygulamalar için izin verebilirsiniz. Kullanıcı veya yönetici onay verdiğinde, web uygulaması ve web API'si hem de onların dizinde kaydedilir.

## <a name="token-expiration"></a>Belirteç süre sonu

Web uygulaması, bir JWT erişim belirteci almak için yetkilendirme kodunu kullandığında, ayrıca JWT yenileme belirtecini alır. Erişim belirtecinin süresi dolduğunda yenileme belirtecini yeniden oturum açmak için gerek kalmadan kullanıcı yeniden kimlik doğrulamaya zorlayabilir kullanılabilir. Bu yenileme belirteci daha sonra kullanıcının kimliğini doğrulamak için kullanılan bir yeni bir erişim belirteci ve yenileme belirteci sonuçlanır.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer hakkında daha fazla bilgi [uygulama türleri ve senaryolar](app-types.md)
- Azure AD hakkında bilgi edinin [kimlik doğrulaması temelleri](authentication-scenarios.md)
