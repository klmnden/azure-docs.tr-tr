---
title: Azure AD uygulama proxy'si için SSO yönetme | Microsoft Docs
description: Uygulama proxy'si ile çoklu oturum açma temelleri hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: dbdbe8b83af20f66ad2cc99d2a5665262479b4a9
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364157"
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Azure AD uygulama proxy'si nasıl çoklu oturum açma sağlar?

Çoklu oturum açma bir anahtar Azure AD uygulama proxy'si öğesidir.  Kullanıcılarınızın Azure Active Directory bulutta oturum açmak yalnızca olduğundan en iyi kullanıcı deneyimi sağlar. Azure Active Directory kimlik doğrulaması sonra uygulama Proxy Bağlayıcısı şirket içi uygulamaya kimlik doğrulaması gerçekleştirir. Arka uç uygulaması, uygulama ara sunucusu ve etki alanına katılmış bir cihazda normal bir kullanım aracılığıyla oturum açarken uzak bir kullanıcı arasındaki farkı bildiremez. 

Uygulamalarınızda çoklu oturum açma için Azure Active Directory kullanmak için seçmeniz gerekir **Azure Active Directory** ön kimlik doğrulama yöntemi olarak. Seçerseniz **geçiş** kullanıcılarınızın Azure Active Directory'ye hiç kimlik doğrulaması yok ancak doğrudan uygulamaya yönlendirilir. Önce bir uygulama yayımlayın veya Azure portalında uygulamanıza gidin ve uygulama proxy'si ayarları düzenleyin, bu ayarı yapılandırabilirsiniz. 

Çoklu oturum açma seçenekleri görmek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Gidin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları**.
3. Çoklu oturum açma seçenekleri, yönetmek istediğiniz uygulamayı seçin.
4. Seçin **çoklu oturum açma**.

   ![SSO açılan menüsü](./media/application-proxy-single-sign-on/single-sign-on-mode.png)

Açılan menüden uygulamanıza çoklu oturum açma için beş seçenekleri gösterir:

* Azure AD çoklu oturum açma devre dışı
* Parola tabanlı oturum açma
* Bağlantılı oturum açma
* Tümleşik Windows Kimlik Doğrulaması
* Üst bilgi tabanlı oturum açma

## <a name="azure-ad-single-sign-on-disabled"></a>Azure AD çoklu oturum açma devre dışı

Uygulamanızda çoklu oturum açma için Azure Active Directory Tümleştirmesi'ni kullanmak istemiyorsanız, seçin **Azure AD çoklu oturum açma devre dışı**. Bu seçenek ile kullanıcılarınız iki kez kimlik doğrulaması yapabilir. İlk olarak, Azure Active Directory kimlik doğrulaması ve uygulama için oturum açın. 

Bu iyi bir seçenek, şirket içi uygulamanız kullanıcıların kimlik doğrulaması gerektirmez, ancak bir uzaktan erişim için güvenlik katmanı olarak Azure Active Directory eklemek istiyorsanız seçenektir. 

## <a name="password-based-sign-on"></a>Parola tabanlı oturum açma

Azure Active Directory, şirket içi uygulamalarınız için bir parola kasası olarak kullanmak istiyorsanız belirleyin **parola tabanlı oturum açma**. Uygulamanız ile erişim belirteçleri veya üst bilgileri yerine bir kullanıcı adı/parola birleşik gerçekleştiriyorsa bu seçeneği iyi bir seçimdir. Parola tabanlı ile oturum açma, kullanıcılarınızın oldukları erişim uygulama ilk zaman oturum açmanız gerekir. Bundan sonra Azure Active Directory kullanıcı adı ve parola kullanıcı adına sağlar. 

Parola tabanlı oturum açmayı ayarlama hakkında daha fazla bilgi için bkz: [vaulting uygulama proxy'si ile çoklu oturum açma için parola](application-proxy-configure-single-sign-on-password-vaulting.md).

## <a name="linked-sign-on"></a>Bağlantılı oturum açma

Şirket içi kimliklerinizi için ayarlanan tek bir oturum açma Çözüm zaten varsa, seçin **bağlantılı oturum açma**. Bu seçenek mevcut SSO çözümleri yararlanmak Azure Active Directory sağlar, ancak yine de, kullanıcılarınızın uygulamaya uzaktan erişim sağlar. 

Bağlantılı oturum açma hakkında (resmi olarak var olan çoklu oturum açma bilinir) daha fazla bilgi için bkz: [uygulama erişimi ve Azure Active Directory ile çoklu oturum açma nedir?](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Tümleşik Windows Kimlik Doğrulaması

Çoklu oturum açma için Kerberos Kısıtlı temsilci (KCD) kullanmak istiyorsanız seçin veya tümleşik Windows Authentication(IWA) kullanıyorsanız şirket içi uygulamalarınızı **tümleşik Windows kimlik doğrulaması**. Bu seçenek ile kullanıcılarınız yalnızca Azure Active Directory kimlik doğrulaması yapmanız ve sonra uygulama Proxy Bağlayıcısı kullanıcının Kerberos belirteci alma ve uygulamada oturum temsil eder. 

Tümleşik Windows kimlik doğrulaması ayarlama hakkında daha fazla bilgi için bkz: [Kerberos kısıtlanmış temsil için çoklu oturum açma uygulama ara sunucusu ile](application-proxy-configure-single-sign-on-with-kcd.md).

## <a name="header-based-sign-on"></a>Üst bilgi tabanlı oturum açma 

Uygulamalarınız için kimlik doğrulama üst bilgileri kullanırsanız, seçin **üst bilgi tabanlı oturum açma**. Bu seçenek belirtilmişse, kullanıcılarınız yalnızca Azure Active Directory kimlik doğrulaması gerekir. Bir üçüncü taraf kimlik doğrulama hizmeti ile Microsoft iş ortakları, Azure Active Directory erişim belirteci, uygulama için bir üstbilgi biçimi çevrilir PingAccess çağrılır. 

Üst bilgi tabanlı kimlik doğrulaması ayarlama hakkında daha fazla bilgi için bkz: [üst bilgi tabanlı kimlik doğrulaması için uygulama proxy'si ile çoklu oturum açma](application-proxy-configure-single-sign-on-with-ping-access.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama proxy'si ile çoklu oturum açma için vaulting parola](application-proxy-configure-single-sign-on-password-vaulting.md)
- [Kerberos kısıtlanmış temsil için çoklu oturum açma uygulama ara sunucusu ile](application-proxy-configure-single-sign-on-with-kcd.md)
- [Üst bilgi tabanlı kimlik doğrulaması için uygulama proxy'si ile çoklu oturum açma](application-proxy-configure-single-sign-on-with-ping-access.md) 
