---
title: SSO için Azure AD uygulama proxy'si yönetme | Microsoft Docs
description: Çoklu oturum açma uygulama proxy'si ile uygulama temelleri hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: barbkess
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 24578ba933b56517053ecfa434a0a58ba6c863a6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a>Azure AD uygulama proxy'si nasıl çoklu oturum açma sağlar?

Çoklu oturum açma bir anahtar Azure AD uygulama proxy'si öğesidir.  Kullanıcılarınız yalnızca bulut Azure Active Directory'de oturum açmak zorunda olduğu için en iyi kullanıcı deneyimi sağlar. Azure Active Directory kimlik doğrulaması sonra uygulama ara sunucusu Bağlayıcısı'nı şirket içi uygulama için kimlik doğrulaması gerçekleştirir. Arka uç uygulaması uzak bir kullanıcı uygulama proxy'si ve etki alanına katılmış bir cihazda normal kullanım aracılığıyla oturum açmayı arasındaki farkı bildiremez. 

Çoklu oturum açma uygulamalarınızda Azure Active Directory kullanmak için seçmeniz gerekir **Azure Active Directory** ön kimlik doğrulama yöntemi olarak. Seçerseniz **geçiş** kullanıcılarınızın Azure Active Directory'ye hiç kimlik doğrulaması yok ancak düz uygulamaya yönlendirilir. Bu ayar, önce bir uygulama yayımlamak veya Azure portalında uygulamanıza gidin ve uygulama Proxy ayarlarını düzenleyin yapılandırabilirsiniz. 

Çoklu oturum açma seçenekleri görmek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Gidin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları**.
3. Çoklu oturum açma seçenekleri yönetmek istediğiniz uygulamayı seçin.
4. Seçin **çoklu oturum açma**.

   ![SSO açılır menü](./media/application-proxy-sso-overview/single-sign-on-mode.png)

Açılır menüsünde, uygulamanız için çoklu oturum açma için beş seçenekleri gösterir:

* Azure AD çoklu oturum açma devre dışı
* Parola tabanlı oturum açma
* Bağlantılı oturum açma
* Tümleşik Windows Kimlik Doğrulaması
* Üstbilgi tabanlı oturum açma

## <a name="azure-ad-single-sign-on-disabled"></a>Azure AD çoklu oturum açma devre dışı

Çoklu oturum açma uygulamanız için Azure Active Directory Tümleştirme kullanmak istemiyorsanız, seçin **Azure AD çoklu oturum açma devre dışı özelliğini**. Bu seçenek ile kullanıcılarınızın iki kez doğrulanabilir. İlk olarak, Azure Active Directory kimlik doğrulaması ve uygulama için oturum açın. 

Bu seçenek iyi bir şirket içi uygulamanız kullanıcıların kimlik doğrulaması yapmasına gerektirmez, ancak bir uzaktan erişim için güvenlik katmanı olarak Azure Active Directory eklemek istediğiniz seçimdir. 

## <a name="password-based-sign-on"></a>Parola tabanlı oturum açma

Azure Active Directory parola kasası olarak şirket içi uygulamalarınız için kullanmak istiyorsanız, tercih **parola tabanlı oturum açma**. Uygulamanızı erişim belirteçleri veya üstbilgileri yerine bir kullanıcı adı/parola bileşik kimlik doğrulamasını, bu seçenek iyi bir seçimdir. Parola tabanlı ile oturum açma, kullanıcılarınızın erişen uygulama ilk zaman oturum açmanız. Bundan sonra Azure Active Directory kullanıcı adı ve parola kullanıcı adına sağlar. 

Parola tabanlı oturum açmayı ayarlama hakkında daha fazla bilgi için bkz: [tek oturum açma için uygulama proxy'si ile vaulting parola](application-proxy-sso-azure-portal.md).

## <a name="linked-sign-on"></a>Bağlantılı oturum açma

Şirket içi kimliklerinizi için ayarlanmış bir tek oturum açma çözümü zaten varsa, seçin **bağlantılı oturum açma**. Bu seçenek varolan SSO çözümleri yararlanmak Azure Active Directory sağlar, ancak hala kullanıcılarınızın uygulamaya uzaktan erişim sağlar. 

Bağlantılı oturum açma (varolan çoklu oturum açma resmi olarak bilinir) özelliğini hakkında daha fazla bilgi için bkz: [uygulama erişimi ve çoklu oturum açma ile Azure Active Directory nedir?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="integrated-windows-authentication"></a>Tümleşik Windows Kimlik Doğrulaması

Çoklu oturum açma için Kerberos Kısıtlı temsilci (KCD) kullanmak istiyorsanız seçin veya tümleşik Windows Authentication(IWA) kullanıyorsanız, şirket içi uygulamalarınızı **tümleşik Windows kimlik doğrulaması**. Bu seçenek ile kullanıcılarınız yalnızca Azure Active Directory kimlik doğrulaması yapmanız ve kullanıcının Kerberos belirteci almak ve uygulamada oturum açmak için uygulama ara sunucusu Bağlayıcısı'nı temsil eder. 

Tümleşik Windows kimlik doğrulaması ayarlama hakkında daha fazla bilgi için bkz: [Kerberos Kısıtlı temsilci çoklu oturum açma için uygulama proxy'si ile](active-directory-application-proxy-sso-using-kcd.md).

## <a name="header-based-sign-on"></a>Üstbilgi tabanlı oturum açma 

Uygulamalarınız için kimlik doğrulama üstbilgileri kullanıyorsa seçin **üstbilgi tabanlı oturum açma**. Bu seçenek ile kullanıcılarınız için kimlik doğrulaması yalnızca Azure Active Directory gerekir. Bir üçüncü taraf kimlik doğrulama hizmetiyle Microsoft iş ortakları, uygulama için bir üstbilgi biçime Azure Active Directory'ye erişim belirteci çevrilen PingAccess çağrılır. 

Üstbilgi tabanlı kimlik doğrulaması ayarlama hakkında daha fazla bilgi için bkz: [üstbilgi tabanlı kimlik doğrulaması için çoklu oturum açma uygulama proxy'si ile](application-proxy-ping-access.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Çoklu oturum açma için uygulama proxy'si ile vaulting parola](application-proxy-sso-azure-portal.md)
- [Kerberos Kısıtlı temsilci çoklu oturum açma için uygulama proxy'si ile uygulama](active-directory-application-proxy-sso-using-kcd.md)
- [Uygulama proxy'si ile çoklu oturum açma için üstbilgi tabanlı kimlik doğrulaması](application-proxy-ping-access.md) 
