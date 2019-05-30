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
ms.openlocfilehash: 9e0300ec0ef4ee67b06acb85514ae898bbd0a830
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544284"
---
# <a name="public-client-and-confidential-client-applications"></a>Genel istemci ve istemci gizli uygulamalar
Microsoft Authentication Library (MSAL) iki türde istemci tanımlar: Genel ve gizli istemciler. İki istemci türlerini güvenli bir şekilde yetkilendirme sunucusunda kimlik doğrulaması ve istemci kimlik bilgilerinin gizliliğini korumak için yeteneklerini göre ayırt edilir.  Buna karşılık, Azure AD Authentication Library (ADAL) (Bu bağlantı için Azure AD) kimlik doğrulama bağlam kavramı vardır.

- **Gizli istemci uygulamaları** (Web uygulamaları, Web API'si veya hatta hizmetin/daemon uygulamalar) sunucuları üzerinde çalışan uygulamalar. Bunlar erişim zordur ve bu nedenle özellikli bir uygulama gizli tutma kabul edilir. Gizli istemciler yapılandırma zaman gizli tutmak kullanabilirsiniz. Her bir istemci örneğinin (ClientID ve gizli anahtarını dahil) ayrı bir yapılandırma var. Bu değerleri ayıklamak için son kullanıcıların zordur. En yaygın gizli istemci bir web uygulamasıdır. İstemci kimliği, web tarayıcısı üzerinden sunulur ancak parolayı yalnızca arka kanal içinde geçirilen ve hiçbir zaman doğrudan kullanıma.

    Gizli istemci uygulamalar: <BR>
    ![Web uygulaması](media/msal-client-applications/web-app.png) ![Web API](media/msal-client-applications/web-api.png) ![arka plan programı/hizmet](media/msal-client-applications/daemon-service.png)

- **Genel istemci uygulamaları** veya Masaüstü cihazlarda veya bir web tarayıcısında çalışan uygulamalar. Bunlar, uygulama gizli dizilerini güvenli bir şekilde korumak ve bu nedenle yalnızca Web API'leri (yalnızca genel istemci akışlar destekledikleri) kullanıcı adına erişim için güvenilir değildir. Genel istemciler yapılandırma zaman gizli tutmak ve bunun sonucunda hiçbir istemci gizli anahtarı alınamıyor.

    Genel istemci uygulamalar: <BR>
    ![Masaüstü uygulaması](media/msal-client-applications/desktop-app.png) ![Browserless API](media/msal-client-applications/browserless-app.png) ![mobil uygulama](media/msal-client-applications/mobile-app.png)

> [!NOTE]
> MSAL.js içinde genel ve özel istemci uygulamaları hiçbir ayrım yoktur.  MSAL.js istemci uygulamaları, istemci kodu bir web tarayıcısı gibi bir kullanıcı aracısına yürütüldü genel bir istemci kullanıcı aracısı tabanlı uygulamaları olarak temsil eder.  Tarayıcı bağlam açık yayımlanmaması erişilebildiğinden, bu istemciler, gizli depolamayın.

## <a name="comparing-the-client-types"></a>İstemci türlerini karşılaştırma
Uygulamalar bazı commonalities ve genel istemci gizli istemci arasındaki farklar vardır:

- Her iki türde uygulamalar kullanıcı belirteci önbelleği korumak ve bir belirteç (sessizce belirtecin belirteç önbellekte zaten olduğu durumlarda) elde edebilirsiniz. Gizli istemci uygulamalar uygulama belirteç önbelleği uygulaması için olan belirteçler için de vardır.
- Hem de kullanıcı hesaplarını yönetme ve kullanıcı belirteç önbellekten hesapları alın, kendi tanımlayıcıdan bir hesap almak veya hesabı kaldırmak.
- Genel istemci uygulamaları, üç gizli istemci uygulamalar varsa (ve doğrulama uç noktası kimlik sağlayıcısının URL'si hesaplamak için bir yöntem ise) bir belirteç (dört kimlik doğrulama akışları) alınırken dört yolu vardır. Belirteçlerini almak ve senaryolar daha fazla bilgi için bkz.

Geçmişte ADAL kullandıysanız, ADAL'ın kimlik doğrulaması bağlamı aykırı MSAL istemci kimliği (uygulama kimliği veya uygulama kimliği adlı) bir kez oluşturma uygulama ve artık geçirilen bir belirteç alırken yinelenmesi gerektiğini, fark edebilirsiniz. Bu durum hem ortak ve özel istemci uygulamaları için geçerlidir. Gizli bir istemci uygulama Oluşturucular, istemci kimlik bilgileri de geçirilir: Kimlik sağlayıcısı ile paylaştıkları gizli anahtarı.

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin:
- [İstemci uygulaması yapılandırma seçenekleri](msal-client-application-configuration.md)
- [İstemci uygulamaları, MSAL.NET kullanarak örnekleme](msal-net-initializing-client-applications.md).
- [İstemci uygulamaları MSAL.js kullanılarak örnekleme](msal-js-initializing-client-applications.md).
