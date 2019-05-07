---
title: Azure Active Directory kimlik koruması çok faktörlü kimlik doğrulaması kayıt ilkesi yapılandırma | Microsoft Docs
description: Azure AD kimlik koruması çok faktörlü kimlik doğrulaması kayıt ilkesi yapılandırmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: article
ms.date: 05/01/2019
author: MicrosoftGuyJFlo
manager: daveba
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f4083ddf849842358f7699badca6598e56e4dee
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65139378"
---
# <a name="how-to-configure-the-azure-multi-factor-authentication-registration-policy"></a>Nasıl Yapılır: Azure multi-Factor Authentication kayıt ilkesi yapılandırma

Azure AD kimlik koruması üretimi multi factor authentication (MFA) kaydı MFA kaydı, oturum açtığınız uygulama altyapılarını ne olursa olsun gerektirmek için koşullu erişim ilkesi yapılandırarak yönetmenize yardımcı olur. Bu makalede, hangi ilke ne amaçla kullanılacağını ve nasıl yapılandırılacağı açıklanmaktadır.

## <a name="what-is-the-azure-multi-factor-authentication-registration-policy"></a>Azure multi-Factor Authentication kayıt ilkesi nedir?

Azure multi-Factor Authentication, kullanan doğrulamak için bir yol birden fazla yalnızca bir kullanıcı adı ve parola sağlar. Bu, ikinci bir kullanıcı oturum açma işlemleri için güvenlik katmanı sağlar. Kullanıcıların MFA isteklerini yanıtlamasına izin için sırada, Azure multi-Factor Authentication için önce kaydetmelisiniz.

Azure multi-Factor Authentication kullanıcı oturum açma işlemleri için çünkü önermesinden öneririz:

- Güçlü kimlik doğrulaması ile çok sayıda kolay doğrulama seçeneği sunar
- Hazırlanması kuruluşunuz korumak ve kimlik koruması, risk olaylardan kurtarmak için önemli bir rol oynar.

MFA ile ilgili daha fazla ayrıntı için [Azure multi-Factor Authentication nedir?](../authentication/howto-mfa-getstarted.md)

## <a name="how-do-i-access-the-registration-policy"></a>Kayıt İlkesi nasıl erişim sağlanır?

MFA kayıt ilkesini bulunduğu **yapılandırma** bölümünde [Azure AD kimlik koruması sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).

![MFA İlkesi](./media/howto-mfa-policy/1014.png)

## <a name="policy-settings"></a>İlke ayarları

MFA kayıt ilkesini yapılandırırken, şu yapılandırma değişiklikleri yapmanız gerekir:

- Kullanıcılar ve ilkenin uygulandığı gruplar. Kuruluşunuzun dışlanacak unutmayın [Acil Durum erişim hesapları](../users-groups-roles/directory-emergency-access.md).

    ![Kullanıcılar ve gruplar](./media/howto-mfa-policy/11.png)

- Uygulamak istediğiniz - denetim **gerektiren Azure MFA kaydı**

    ![Access](./media/howto-mfa-policy/12.png)

- Zorlamak ilke ayarlanmalıdır **üzerinde**.

    ![İlke zorlama](./media/howto-mfa-policy/14.png)

- **Kaydet** ilkenizi

## <a name="user-experience"></a>Kullanıcı deneyimi

İlgili kullanıcı deneyimini genel bakış için bkz:

- [Çok faktörlü kimlik doğrulaması kayıt akışını](flows.md#multi-factor-authentication-registration).  
- [Azure AD kimlik koruması ile karşılaştığında oturum açma](flows.md).  

## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview.md).
