---
title: Azure AD uygulama proxy'sini kullanarak şirket içi uygulamaya oturum açmada sorun | Microsoft Docs
description: Azure AD uygulama ara sunucusu kullanarak Azure AD ile tümleştirilmiş bir şirket içi uygulamaya oturum açamıyor genişlettiklerinde karşılaştığı yaygın sorunları giderme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: asteen
ms.openlocfilehash: 9c94f25d415e3ef5ae03488ca117a846d2d1c4e6
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964704"
---
# <a name="problems-signing-in-to-an-on-premises-application-using-the-azure-ad-application-proxy"></a>Azure AD uygulama proxy'sini kullanarak şirket içi uygulamaya oturum açmada sorun

Şirket içi uygulamada oturum sorun yaşıyorsanız, sorunu çözmek için aşağıdaki adımları izleyerek deneyebilirsiniz.

## <a name="i-can-load-my-application-but-something-on-the-page-looks-broken"></a>Ben uygulamamı yükleyebiliyorum ama sayfadaki bir şey bozuk görünüyor

Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.

  * [Uygulamama ulaşabiliyorum ama uygulama sayfası doğru görüntülenmiyor](application-proxy-page-appearance-broken-problem.md)
  * [Uygulamama ulaşabiliyorum ama uygulamanın yüklenmesi fazla uzun sürüyor](application-proxy-page-load-speed-problem.md)
  * [Uygulamama ulaşabiliyorum ama uygulama sayfasındaki bağlantılar çalışmıyor](application-proxy-page-links-broken-problem.md)

## <a name="im-having-a-connectivity-problem-my-application"></a>Uygulamamda bağlantı sorunu yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * [Uygulamam için hangi bağlantı noktalarını açacağımı bilmiyorum](application-proxy-connectivity-ports-how-to.md)
  * [Uygulamam için bağlayıcı grubunda çalışan bağlayıcı olmadığından sorunla karşılaştım](application-proxy-connectivity-no-working-connector.md)

## <a name="im-having-a-problem-configuring-the-azure-ad-application-proxy-in-the-admin-portal"></a>Yönetim Portalı'nda Azure AD uygulama proxy'sini yapılandırırken sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * [Uygulama Proxy’si uygulamasını yapılandırırken zorluk sorun yaşıyorum](application-proxy-config-how-to.md)
  * [Uygulama Proxy’si uygulamamda çoklu oturum açmayı yapılandırmayı bilmiyorum](application-proxy-config-sso-how-to.md)
  * [Yönetim portalında uygulamamı oluştururken bir sorunla karşılaştım](application-proxy-config-problem.md)

## <a name="im-having-a-problem-setting-up-back-end-authentication-to-my-application"></a>Uygulamama arka uç kimlik doğrulaması ayarlarken sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * [Kerberos Sınırlı Temsilini yapılandırmayı bilmiyorum](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
  * [Uygulamamı PingAccess’le nasıl yapılandıracağımı bilmiyorum](application-proxy-back-end-ping-access-how-to.md)

## <a name="im-having-a-problem-when-signing-in-to-my-application"></a>Uygulamam için oturum açarken sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * ["Bu Şirket Uygulamasına Erişilemiyor" hatası alıyorum](application-proxy-sign-in-bad-gateway-timeout-error.md)

## <a name="im-having-a-problem-with-the-application-proxy-agent-connector"></a>Uygulama proxy'si aracı Bağlayıcısı ile ilgili bir sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * [Uygulama Proxy’si Aracı Bağlayıcısı’nı yüklerken sorun yaşıyorum.](application-proxy-connector-installation-problem.md)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi uygulamalara güvenli uzaktan erişim sağlama](application-proxy.md)
