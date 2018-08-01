---
title: Uygulama proxy'si uygulaması için çoklu oturum açma yapılandırma | Microsoft Docs
description: Nasıl çoklu oturum açma uygulama proxy uygulamanıza hızlı bir şekilde yapılandırabilirsiniz
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 178737a5dec1aace43de9ddb8dcfac73530a250f
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363186"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Uygulama proxy'si uygulaması için çoklu oturum açma yapılandırma

Çoklu oturum açma (SSO) kullanıcılarınızın birden çok kez kimlik doğrulaması olmadan bir uygulamaya erişmesine izin verir. Bu bulutta, Azure Active Directory karşı gerçekleşmesi tek bir kimlik doğrulaması ve hizmet veya uygulamadaki herhangi ek bir kimlik doğrulama sınaması tamamlamak için kullanıcının kimliğine bürünmek için bağlayıcı olanak sağlar.

## <a name="how-to-configure-single-sign-on"></a>Çoklu oturum yapılandırma
SSO yapılandırmak için ilk uygulamanızı Azure Active Directory üzerinden ön kimlik doğrulaması için yapılandırılmış olduğundan emin olun. Bu yapılandırmayı gerçekleştirmek için Git **Azure Active Directory**  - &gt; **kurumsal uygulamalar**  - &gt; **tüm uygulamalar**   - &gt; Uygulamanızı  **- &gt; uygulama proxy'si**. Bu sayfada, "Ön kimlik doğrulaması" alanına bakın ve "Azure Active Directory için ayarlanmış olduğundan emin olun 

Ön kimlik doğrulama yöntemleri hakkında daha fazla bilgi için bkz. 4. adımı [uygulama yayımlama belge](manage-apps/application-proxy-publish-azure-portal.md).

   ![Azure portalında ön kimlik doğrulama yöntemi](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Çoklu oturum açma modları için uygulama Proxy uygulamaları yapılandırma
Çoklu oturum açma belirli türünü yapılandırın. Oturum açma yöntemleri, hangi kimlik doğrulaması türü arka uç uygulamanın kullandığı şirket göre sınıflandırılır. Uygulama Proxy uygulamaları, üç tür oturum açmayı destekler:

-   **Parola tabanlı oturum açma**: parola tabanlı oturum açma için kullanılabilir oturum açma için kullanıcı adı ve parola alanları kullanan tüm uygulamaları. Yapılandırma adımları bulunduğunuz [parola SSO yapılandırma belgelerine](active-directory-enterprise-apps-whats-new-azure-portal.md#bring-your-own-password-sso-applications).

-   **Tümleşik Windows kimlik doğrulaması**: tümleşik Windows kimlik doğrulaması (IWA) kullanarak uygulamalar için çoklu oturum açma Kerberos Kısıtlı temsilci (KCD) aracılığıyla etkinleştirilir. Bu yöntem uygulama Proxy Bağlayıcılarına, kullanıcıların kimliğine bürünmek ve göndermek ve kendileri adına belirteçlerini almak için Active Directory'de sağlar. KCD yapılandırma ayrıntıları bulunabilir [KCD belgeleri ile çoklu oturum açmayı](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md).

-   **Üst bilgi tabanlı oturum açma**: üst bilgi tabanlı oturum açma, bir iş ortaklığı etkinleştirilir ve bazı ek yapılandırma gerektirir. Üst bilgileri için kimlik doğrulaması kullanan bir uygulama için çoklu oturum açma yapılandırmaya yönelik adım adım yönergeler ve iş ortaklığı hakkında daha fazla bilgi için bkz: [belgeleri Azure AD için PingAccess](manage-apps/application-proxy-configure-single-sign-on-with-ping-access.md).

Bu seçeneklerin her biri "Kurumsal uygulamalar" uygulamanızda giderek ve ardından açarak bulunabilir **çoklu oturum açma** sol menüde sayfası. Uygulamanız eski portalda oluşturulmuş olsa bile, tüm bu seçenekler göremeyebilirsiniz unutmayın.

Bu sayfada, ayrıca bir gördüğünüz ek oturum açma seçeneği: bağlantılı oturum açma. Bu seçenek, uygulama proxy'si tarafından da desteklenir. Ancak, bu seçeneği çoklu oturum açma uygulamaya eklemez. Bu uygulamanın tek Active Directory Federasyon Hizmetleri gibi başka bir hizmet kullanılarak uygulanan oturum zaten olabilir belirtti. 

Bu seçenek, bir uygulamaya bir bağlantı, kullanıcıların ilk land uygulamaya erişirken oluşturmak için bir yönetici sağlar. Active Directory Federasyon Hizmetleri 2.0 kullanan kullanıcıların kimliğini doğrulamak üzere yapılandırılmış bir uygulama ise, örneğin, bir yönetici "bağlantılı oturum açma" seçeneği erişim panelinde bir bağlantı oluşturmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
