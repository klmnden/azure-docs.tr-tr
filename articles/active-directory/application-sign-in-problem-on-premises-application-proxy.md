---
title: Azure AD uygulama proxy'si kullanarak bir şirket içi uygulama oturum açma sorunları | Microsoft Docs
description: Azure AD uygulama proxy'si kullanarak Azure AD ile tümleşik bir şirket içi uygulamasında oturum kaldıramadığınızda karşılaştığı yaygın sorunları giderme
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: afd5460ef3e286cd3d8b550e7d7e54b3e09f2990
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="problems-signing-in-to-an-on-premises-application-using-the-azure-ad-application-proxy"></a>Azure AD uygulama proxy'si kullanarak bir şirket içi uygulama oturum açma sorunları

Bir şirket içi uygulamanızı imzalama sorun yaşıyorsanız, sorunu çözmek için aşağıdaki adımları izleyerek deneyebilirsiniz.

## <a name="i-can-load-my-application-but-something-on-the-page-looks-broken"></a>Uygulamam yük yükleyebilirsiniz, ancak sayfasında bir şey bozuk görünüyor

Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.

  * [Uygulamama ulaşabiliyorum ama uygulama sayfası doğru görüntülenmiyor](https://docs.microsoft.com/azure/active-directory/application-proxy-page-appearance-broken-problem/)
  * [Uygulamama ulaşabiliyorum ama uygulamanın yüklenmesi fazla uzun sürüyor](https://docs.microsoft.com/azure/active-directory/application-proxy-page-load-speed-problem/)
  * [Uygulamama ulaşabiliyorum ama uygulama sayfasındaki bağlantılar çalışmıyor](https://docs.microsoft.com/azure/active-directory/application-proxy-page-links-broken-problem/)

## <a name="im-having-a-connectivity-problem-my-application"></a>Uygulamam bağlantı sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * [Uygulamam için hangi bağlantı noktalarını açacağımı bilmiyorum](https://docs.microsoft.com/azure/active-directory/application-proxy-connectivity-ports-how-to/)
  * [Uygulamam için bağlayıcı grubunda çalışan bağlayıcı olmadığından sorunla karşılaştım](https://docs.microsoft.com/azure/active-directory/application-proxy-connectivity-no-working-connector/)

## <a name="im-having-a-problem-configuring-the-azure-ad-application-proxy-in-the-admin-portal"></a>Yönetim Portalı'nda Azure AD uygulama proxy'si yapılandırma bir sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * [Uygulama Proxy’si uygulamasını yapılandırırken zorluk sorun yaşıyorum](https://docs.microsoft.com/azure/active-directory/application-proxy-config-how-to/)
  * [Uygulama Proxy’si uygulamamda çoklu oturum açmayı yapılandırmayı bilmiyorum](https://docs.microsoft.com/azure/active-directory/application-proxy-config-sso-how-to/)
  * [Yönetim portalında uygulamamı oluştururken bir sorunla karşılaştım](https://docs.microsoft.com/azure/active-directory/application-proxy-config-problem/)

## <a name="im-having-a-problem-setting-up-back-end-authentication-to-my-application"></a>Uygulamam için arka uç kimlik doğrulaması kurmada sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * [Kerberos Sınırlı Temsilini yapılandırmayı bilmiyorum](https://docs.microsoft.com/azure/active-directory/application-proxy-back-end-kerberos-constrained-delegation-how-to/)
  * [Uygulamamı PingAccess’le nasıl yapılandıracağımı bilmiyorum](https://docs.microsoft.com/azure/active-directory/application-proxy-back-end-ping-access-how-to/)

## <a name="im-having-a-problem-when-signing-in-to-my-application"></a>Uygulamam için oturum açarken sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * ["Bu Şirket Uygulamasına Erişilemiyor" hatası alıyorum](https://docs.microsoft.com/azure/active-directory/application-proxy-sign-in-bad-gateway-timeout-error/)

## <a name="im-having-a-problem-with-the-application-proxy-agent-connector"></a>Uygulama Proxy Aracısı Bağlayıcısı ile ilgili bir sorun yaşıyorum
  Aşağıdaki belgeler, bu kategoriye giren yaygın sorunların çoğunu çözmenize yardımcı olabilir.
  * [Uygulama Proxy’si Aracı Bağlayıcısı’nı yüklerken sorun yaşıyorum.](https://docs.microsoft.com/azure/active-directory/application-proxy-connector-installation-problem/)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi uygulamalara güvenli uzaktan erişim sağlama](manage-apps/application-proxy.md)
