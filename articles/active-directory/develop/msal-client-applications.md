---
title: İstemci uygulamaları (Microsoft kimlik doğrulama kitaplığı) | Azure
description: Genel istemci ve gizli istemci uygulamaların Microsoft Authentication Library (MSAL) öğrenin.
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
ms.openlocfilehash: 9d09436b9a2ac38e7b07a51f01d65769ed19d08e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66430833"
---
# <a name="public-client-and-confidential-client-applications"></a>Genel istemci ve istemci gizli uygulamalar
Microsoft Authentication Library (MSAL) iki türde istemci tanımlar: Genel ve gizli istemciler. İki istemci türlerini güvenli bir şekilde yetkilendirme sunucusunda kimlik doğrulaması ve istemci kimlik bilgilerinin gizliliğini korumak için yeteneklerini göre ayırt edilir. Buna karşılık, ne adlı Azure AD Authentication Library (ADAL) kullanan *kimlik doğrulaması bağlamı* (Azure AD bağlantısı olmayan).

- **Gizli istemci uygulamaları** sunucuları (web uygulamaları, Web API apps veya hatta hizmetin/daemon uygulamalar) üzerinde çalışan uygulamalardır. Bunlar erişmek ve bu nedenle zor özellikli bir uygulama gizli tutmak kabul edilmeleri. Gizli istemciler configuration zamanı gizli dizileri içerebilir. Her bir istemci örneğinin (istemci kimliği ve istemci gizli anahtarını dahil) ayrı bir yapılandırma var. Bu değerleri ayıklamak için son kullanıcıların zordur. En yaygın gizli istemci bir web uygulamasıdır. İstemci kimliği, web tarayıcısı üzerinden sunulur ancak parolayı yalnızca arka kanal içinde geçirilen ve hiçbir zaman doğrudan kullanıma.

    Gizli istemci uygulamalar: <BR>
    ![Web uygulaması](media/msal-client-applications/web-app.png) ![Web API](media/msal-client-applications/web-api.png) ![arka plan programı/hizmet](media/msal-client-applications/daemon-service.png)

- **Genel istemci uygulamaları** masaüstü bilgisayarlar veya cihazlarda veya bir web tarayıcısında çalışan uygulamalardır. Bunlar yalnızca Web API'lerine kullanıcının adına erişim için güvenli bir şekilde uygulama parolalarını korumak için güvenilir değil. (Bunlar, yalnızca ortak istemci akışları desteklemiyor.) İstemci gizli dizileri zorunluluğunu genel istemcilere yapılandırma zamanı gizli tutamıyor.

    Genel istemci uygulamaları: <BR>
    ![Masaüstü uygulaması](media/msal-client-applications/desktop-app.png) ![Browserless API](media/msal-client-applications/browserless-app.png) ![mobil uygulama](media/msal-client-applications/mobile-app.png)

> [!NOTE]
> MSAL.js içinde genel ve özel istemci uygulamaları hiçbir ayrım yoktur.  MSAL.js istemci uygulamalar kullanıcı aracı tabanlı uygulamaları, genel istemciler, istemci kodu bir web tarayıcısı gibi bir kullanıcı aracısına yürütüldü olarak temsil eder. Tarayıcı bağlam açık yayımlanmaması erişilebilir olduğundan bu istemciler, gizli dizileri depolamayın.

## <a name="comparing-the-client-types"></a>İstemci türlerini karşılaştırma
Uygulamaları bazı benzerlikler ve genel istemci gizli istemci arasındaki farklar şunlardır:

- Her iki tür uygulama, bir kullanıcı belirteci önbelleği korumak ve sessizce (belirteç zaten belirteç önbelleğe olduğunda) bir belirteç elde edebilirsiniz. Gizli istemci uygulamalar, uygulama için olan belirteçleri için bir uygulama belirteç önbelleği de sahiptir.
- Her iki tür uygulama kullanıcı hesaplarını yönetme ve kullanıcı belirteç önbellekten bir hesap alın, kendi tanımlayıcıdan bir hesap alın veya hesabı kaldırmak.
- Genel istemci uygulamalar (dört kimlik doğrulama akışları) belirteç almak için dört yolu vardır. Gizli istemci uygulamalar bir belirteç almak için üç yolu vardır (ve kimlik sağlayıcısının URL'si işlem yollarından biri yetkilendirme uç noktası). Daha fazla bilgi için [belirteçlerini almak](msal-acquire-cache-tokens.md).

ADAL kullandıysanız, MSAL istemci kimliği, ADAL'ın kimlik doğrulaması bağlamı aksine görebilirsiniz (olarak da adlandırılan *uygulama kimliği* veya *uygulama kimliği*) uygulamasının yapımı sırasında bir kez geçirilir. Uygulama bir belirteç alır, tekrar geçirilmesi gerekmez. Bu, hem ortak ve özel istemci uygulamaları için geçerlidir. Gizli bir istemci uygulama oluşturucuları, istemci kimlik bilgileri de geçirilir: Kimlik sağlayıcısı ile paylaştıkları gizli anahtarı.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin:
- [İstemci uygulaması yapılandırma seçenekleri](msal-client-application-configuration.md)
- [İstemci uygulamaları, MSAL.NET kullanarak örnekleme](msal-net-initializing-client-applications.md)
- [İstemci uygulamaları, MSAL.js kullanılarak örnekleme](msal-js-initializing-client-applications.md)
