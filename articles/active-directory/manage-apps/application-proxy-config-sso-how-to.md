---
title: Uygulama proxy'si uygulaması için çoklu oturum açma yapılandırma | Microsoft Docs
description: Nasıl çoklu oturum açma uygulama proxy uygulamanıza hızlı bir şekilde yapılandırabilirsiniz
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: mimart
ms.reviewer: japere, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: f26b28b34a569673b397fa4700c5332c3550500f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65825860"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Uygulama proxy'si uygulaması için çoklu oturum açma yapılandırma

Çoklu oturum açma (SSO) kullanıcılarınızın birden çok kez kimlik doğrulaması olmadan bir uygulamaya erişmesine izin verir. Bu bulutta, Azure Active Directory karşı gerçekleşmesi tek bir kimlik doğrulaması ve hizmet veya uygulamadaki herhangi ek bir kimlik doğrulama sınaması tamamlamak için kullanıcının kimliğine bürünmek için bağlayıcı olanak sağlar.

## <a name="how-to-configure-single-sign-on"></a>Çoklu oturum yapılandırma
SSO yapılandırmak için ilk uygulamanızı Azure Active Directory üzerinden ön kimlik doğrulaması için yapılandırılmış olduğundan emin olun. Bu yapılandırmayı gerçekleştirmek için Git **Azure Active Directory**  - &gt; **kurumsal uygulamalar**  - &gt; **tüm uygulamalar**   - &gt; Uygulamanızı  **- &gt; uygulama proxy'si**. Bu sayfada, "Ön kimlik doğrulaması" alanına bakın ve "Azure Active Directory için ayarlanmış olduğundan emin olun 

Ön kimlik doğrulama yöntemleri hakkında daha fazla bilgi için bkz. 4. adımı [uygulama yayımlama belge](application-proxy-add-on-premises-application.md).

   ![Azure portalında ön kimlik doğrulama yöntemi](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Çoklu oturum açma modları için uygulama Proxy uygulamaları yapılandırma
Çoklu oturum açma belirli türünü yapılandırın. Oturum açma yöntemleri, hangi kimlik doğrulaması türü arka uç uygulamanın kullandığı şirket göre sınıflandırılır. Uygulama Proxy uygulamaları, üç tür oturum açmayı destekler:

-   **Parola tabanlı oturum açma**: Parola tabanlı oturum açma için oturum açma için kullanıcı adı ve parola alanları kullanan tüm uygulamaları kullanılabilir. Yapılandırma adımları bulunduğunuz [parola çoklu oturum açma Azure AD galeri uygulaması için yapılandırma](configure-password-single-sign-on-gallery-applications.md).

-   **Tümleşik Windows kimlik doğrulaması**: Kerberos Kısıtlı temsilci (KCD) aracılığıyla çoklu oturum açma tümleşik Windows kimlik doğrulaması (IWA) kullanarak uygulamalar için etkinleştirilir. Bu yöntem uygulama Proxy Bağlayıcılarına, kullanıcıların kimliğine bürünmek ve göndermek ve kendileri adına belirteçlerini almak için Active Directory'de sağlar. KCD yapılandırma ayrıntıları bulunabilir [KCD belgeleri ile çoklu oturum açmayı](application-proxy-configure-single-sign-on-with-kcd.md).

-   **Üst bilgi tabanlı oturum açma**: Üst bilgi tabanlı oturum açma bir iş ortaklığı etkinleştirilir ve bazı ek yapılandırma gerektirir. Üst bilgileri için kimlik doğrulaması kullanan bir uygulama için çoklu oturum açma yapılandırmaya yönelik adım adım yönergeler ve iş ortaklığı hakkında daha fazla bilgi için bkz: [belgeleri Azure AD için PingAccess](application-proxy-configure-single-sign-on-with-ping-access.md).

-   **SAML çoklu oturum açma**: SAML çoklu oturum açma ile Azure AD kullanıcının Azure AD hesabı kullanarak uygulamaya kimliğini doğrular. Azure AD oturum açma bilgileri uygulamaya bir bağlantı protokolü üzerinden iletişim kurar. SAML tabanlı çoklu oturum açma ile kullanıcılar, SAML Taleplerde tanımladığınız kurallarına göre belirli uygulama rolleri eşleyebilirsiniz. SAML çoklu oturum açmayı ayarlama hakkında daha fazla bilgi için bkz: [uygulama proxy'si ile çoklu oturum açma için SAML](application-proxy-configure-single-sign-on-on-premises-apps.md).

Bu seçeneklerin her biri "Kurumsal uygulamalar" uygulamanızda giderek ve ardından açarak bulunabilir **çoklu oturum açma** sol menüde sayfası. Uygulamanız eski portalda oluşturulmuş olsa bile, tüm bu seçenekler göremeyebilirsiniz unutmayın.

Bu sayfada, ayrıca bir gördüğünüz ek oturum açma seçeneği: Bağlantılı oturum açma. Bu seçenek, uygulama proxy'si tarafından da desteklenir. Ancak, bu seçeneği çoklu oturum açma uygulamaya eklemez. Bu uygulamanın tek Active Directory Federasyon Hizmetleri gibi başka bir hizmet kullanılarak uygulanan oturum zaten olabilir belirtti. 

Bu seçenek, bir uygulamaya bir bağlantı, kullanıcıların ilk land uygulamaya erişirken oluşturmak için bir yönetici sağlar. Active Directory Federasyon Hizmetleri 2.0 kullanan kullanıcıların kimliğini doğrulamak üzere yapılandırılmış bir uygulama ise, örneğin, bir yönetici "bağlantılı oturum açma" seçeneği erişim panelinde bir bağlantı oluşturmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Uygulama proxy'si ile çoklu oturum açma için vaulting parola](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Kerberos kısıtlanmış temsil için çoklu oturum açma uygulama ara sunucusu ile](application-proxy-configure-single-sign-on-with-kcd.md)
- [Üst bilgi tabanlı kimlik doğrulaması için uygulama proxy'si ile çoklu oturum açma](application-proxy-configure-single-sign-on-with-ping-access.md) 
- [Uygulama proxy'si ile çoklu oturum açma için SAML](application-proxy-configure-single-sign-on-on-premises-apps.md).
