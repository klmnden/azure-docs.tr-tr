---
title: Azure Active Directory kimlik koruması çok faktörlü kimlik doğrulaması kayıt ilkesi yapılandırma | Microsoft Docs
description: Azure AD kimlik koruması çok faktörlü kimlik doğrulaması kayıt ilkesi yapılandırmayı öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: e626260dba3155ef56ee4a784aab2c6fd6897295
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581366"
---
# <a name="how-to-configure-the-multi-factor-authentication-registration-policy"></a>Nasıl yapılır: çok faktörlü kimlik doğrulaması kayıt ilkesi yapılandırma

Azure AD kimlik koruması bir ilkesi yapılandırarak multi factor authentication (MFA) kaydı üretimi yönetmenize yardımcı olur. Bu makalede, hangi ilke ne amaçla kullanılacağını açıklanmaktadır. nasıl yapılandırılacağına ilişkin bir.

## <a name="what-is-the-multi-factor-authentication-registration-policy"></a>Çok faktörlü kimlik doğrulaması kayıt ilkesi nedir?

Azure çok faktörlü kimlik doğrulaması yalnızca bir kullanıcı adı ve parola kullanılmasını gerektiren kim olduğunuzu doğrulama bir yöntemdir. Bu, ikinci bir kullanıcı oturum açma ve işlemler için güvenlik katmanı sağlar.  
Öneririz çünkü kullanıcı oturum açma işlemleri için Azure multi-Factor authentication gerektirmesine, onu:

* Güçlü kimlik doğrulaması ile çok sayıda kolay doğrulama seçeneği sunar
* Hazırlanması kuruluşunuz korumak ve hesabı ödün kurtarmak için önemli bir rol oynar

![Kullanıcı riski İlkesi](./media/howto-mfa-policy/1019.png "kullanıcı riski İlkesi")

Daha fazla ayrıntı için [Azure multi-Factor Authentication nedir?](../authentication/multi-factor-authentication.md)

## <a name="configuration"></a>Yapılandırma

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD kimlik koruması** dikey penceresindeki **yapılandırma** bölümünde **çok faktörlü kimlik doğrulaması kaydı**.

    ![MFA ilkesini](./media/howto-mfa-policy/1019.png "MFA İlkesi")

### <a name="settings"></a>Ayarlar

* Kullanıcıları ve grupları ilkenin uygulanacağı ayarlayın:

    ![MFA ilkesini](./media/howto-mfa-policy/1020.png "MFA İlkesi")
* İlke tetiklendiğinde zorlanacak denetimleri ayarlayın:  

    ![MFA ilkesini](./media/howto-mfa-policy/1021.png "MFA İlkesi")
* İlke durumu anahtarı:

    ![MFA ilkesini](./media/howto-mfa-policy/403.png "MFA İlkesi")
* Geçerli kayıt durumunu görüntüleyin:

    ![MFA ilkesini](./media/howto-mfa-policy/1022.png "MFA İlkesi")

İlgili kullanıcı deneyimini genel bakış için bkz:

* [Çok faktörlü kimlik doğrulaması kayıt akışını](flows.md#multi-factor-authentication-registration).  
* [Azure AD kimlik koruması ile karşılaştığında oturum açma](flows.md).  



## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview.md).
