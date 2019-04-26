---
title: Azure Active Directory kimlik koruması çok faktörlü kimlik doğrulaması kayıt ilkesi yapılandırma | Microsoft Docs
description: Azure AD kimlik koruması çok faktörlü kimlik doğrulaması kayıt ilkesi yapılandırmayı öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: abd1049551d7dbc4823636dfdc00f64afab72cdf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60294906"
---
# <a name="how-to-configure-the-multi-factor-authentication-registration-policy"></a>Nasıl Yapılır: Çok faktörlü kimlik doğrulaması kayıt ilkesi yapılandırma

Azure AD kimlik koruması üretimi multi factor authentication (MFA) kaydı MFA kayıt gerektiren bir ilkeniz yapılandırarak yönetmenize yardımcı olur. Bu makalede, hangi ilke ne amaçla kullanılacağını ve nasıl yapılandırılacağı açıklanmaktadır.

## <a name="what-is-the-multi-factor-authentication-registration-policy"></a>Çok faktörlü kimlik doğrulaması kayıt ilkesi nedir?

Azure çok faktörlü kimlik doğrulaması yalnızca bir kullanıcı adı ve parola kullanılmasını gerektiren kim olduğunuzu doğrulama bir yöntemdir. Bu, ikinci bir kullanıcı oturum açma ve işlemler için güvenlik katmanı sağlar. Kullanıcıların MFA isteklerini yanıtlamasına izin için sırada, bunlar için mfa'yı ilk kaydetmelisiniz. 

Öneririz çünkü kullanıcı oturum açma işlemleri için Azure multi-Factor authentication gerektirmesine, onu:

- Güçlü kimlik doğrulaması ile çok sayıda kolay doğrulama seçeneği sunar

- Hazırlanması kuruluşunuz korumak ve hesabı ödün kurtarmak için önemli bir rol oynar


MFA ile ilgili daha fazla ayrıntı için [Azure multi-Factor Authentication nedir?](../authentication/multi-factor-authentication.md)


## <a name="how-do-i-access-the-mfa-registration-policy"></a>MFA kayıt ilkesini nasıl erişim sağlanır?
   
MFA kayıt ilkesini bulunduğu **yapılandırma** bölümünde [Azure AD kimlik koruması sayfa](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![MFA İlkesi](./media/howto-mfa-policy/1014.png)




## <a name="policy-settings"></a>İlke ayarları

MFA kayıt ilkesini yapılandırırken ayarlamanız gerekir:

- Kullanıcılar ve ilkenin uygulandığı gruplar:

    ![Kullanıcılar ve gruplar](./media/howto-mfa-policy/11.png)

- Zorla-gereksinimi için MFA kaydı istediğiniz denetimi:  

    ![Access](./media/howto-mfa-policy/12.png)

- İlke durumu:

    ![İlke zorlama](./media/howto-mfa-policy/14.png)


İlke yapılandırma iletişim kutusu yapılandırmanızı etkisini tahmin etmek için bir seçenek sağlar.

![Tahmini etki](./media/howto-mfa-policy/15.png)




## <a name="user-experience"></a>Kullanıcı deneyimi


İlgili kullanıcı deneyimini genel bakış için bkz:

* [Çok faktörlü kimlik doğrulaması kayıt akışını](flows.md#multi-factor-authentication-registration).  
* [Azure AD kimlik koruması ile karşılaştığında oturum açma](flows.md).  



## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview.md).
