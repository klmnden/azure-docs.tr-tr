---
title: Kayıtlı cihaz Azure AD'ye nelerdir?
description: Cihaz Kimlik Yönetimi ortamınızdaki kaynakların erişen cihazların yönetmenize nasıl yardımcı olabileceğini öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e2a8cad7cd4410a95a6ebd60ada22de456737bf
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462765"
---
# <a name="azure-ad-registered-devices"></a>Azure AD kayıtlı cihazlar

Kayıtlı Azure AD cihazları amacı kendi cihazını getir (KCG) veya mobil cihaz senaryoları desteği ile kullanıcılarınızın sağlamaktır. Bu senaryolarda, bir kullanıcı kişisel bir cihaz kullanarak, kuruluşunuzun Azure Active Directory denetlenen kaynaklarına erişebilir.

|   | Azure AD kayıtlı |
| --- | --- |
| **Tanım** | Cihaza oturum açmak için Kurumsal hesap gerek kalmadan Azure AD'ye kayıtlı |
| **Birincil hedef kitlesi** | Aşağıdaki ölçütlere sahip tüm kullanıcılar için geçerlidir: |
|   | Kendi Cihazını Getir (KCG) |
|   | Mobil cihazlar |
| **Cihaz sahipliği** | Kullanıcı veya kuruluş |
| **İşletim sistemleri** | Windows 10, iOS, Android ve MacOS |
| **Sağlama** | Windows 10 ayarları |
|   | iOS/Android – Şirket portalı veya Microsoft Authenticator uygulaması |
|   | MacOS – Şirket portalı |
| **Cihaz oturum açma seçenekleri** | Son kullanıcı yerel kimlik bilgileri |
|   | Parola |
|   | Windows Hello |
|   | PIN |
|   | Biyometri veya diğer cihazlar için desen |
| **Cihaz yönetimi** | Mobil cihaz Yönetimi (örnek: Microsoft Intune) |
|   | Mobil Uygulama Yönetimi |
| **Anahtar özellikleri** | Bulut kaynaklarına SSO |
|   | Intune'a kaydedildiğinde koşullu erişim |
|   | Uygulama koruma İlkesi aracılığıyla koşullu erişim |
|   | Microsoft Authenticator uygulamasını telefon oturum sağlar |

![Azure AD kayıtlı cihazlar](./media/concept-azure-ad-register/azure-ad-registered-device.png)

Azure AD kayıtlı cihazları Windows 10 cihazında bulunan bir Microsoft hesabı gibi yerel bir hesap kullanarak oturum açtınız ancak ayrıca kurumsal kaynaklara erişim için bağlı bir Azure AD hesabınız yok. Bu Azure AD hesabına göre daha sınırlı ve koşullu erişim ilkeleri için cihaz kimliği uygulanan kuruluşunda bulunan kaynaklara erişimi olabilir.

Yöneticiler, güvenli ve daha fazla Microsoft Intune gibi mobil cihaz Yönetimi (MDM) araçlarını kullanarak bu kayıtlı Azure AD cihazları denetleme. MDM şifrelenmiş depolama, parola karmaşıklığını ve güncelleştirilmiş tutulan güvenlik yazılımı gerektiren gibi kuruluş gerekli yapılandırmaları zorlamak için bir yol sağlar. 

Azure AD kaydı, bir iş uygulamasını ilk kez veya el ile Windows 10 ayarlar menüsünü kullanarak erişirken gerçekleştirilebilir. 

## <a name="scenarios"></a>Senaryolar

Raporlama kapatma zaman ve ev Bilgisayarlarını avantajları kayıt e-posta için araçlara erişmek, kuruluşunuzdaki bir kullanıcı istiyor. Kuruluşunuz, Intune ile uyumlu bir CİHAZDAN erişim gerektiren bir koşullu erişim ilkesi arkasında bu araçlara sahiptir. Kullanıcı, kuruluş hesabı ekler ve ev Bilgisayarlarını Azure AD'ye kaydeder ve gerekli Intune ilkeleri, kaynaklara kullanıcı erişimini vermiş uygulanır.

Başka bir kullanıcı, kök kendi kişisel Android telefon Kurumsal e-postaya erişmek istiyor. Şirketiniz uyumlu cihaz gerektirir ve herhangi bir kökü belirtilmiş cihazları engellemek için Intune uyumluluk İlkesi oluşturdu. Bu cihazda kurumsal kaynaklara erişimi çalışan durduruldu.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak cihaz kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD'de eski cihazları yönetme](manage-stale-devices.md)
