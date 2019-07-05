---
title: Hibrit Azure AD'ye nedir, cihazı alanına?
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
ms.openlocfilehash: 5c57180ba10322cb790c05b3f8f48043ca08b545
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462752"
---
# <a name="hybrid-azure-ad-joined-devices"></a>Hibrit Azure AD’ye katılmış cihazlar

On yılı aşkın süredir pek çok kuruluş şirket içi Active Directory alanları için etki alanına katılım uygulayarak:

- BT departmanlarının merkezi bir konumdan işe ait cihazları yönetebilmesini sağlamıştır.
- Kullanıcıların Active Directory iş veya okul hesapları ile cihazlarında oturum açabilmesini sağlamıştır.

Genel olarak şirket içi ayak izi olan kuruluşlar cihazları kullanıma almak için görüntüleme yöntemlerinden yararlanır ve bu cihazları yönetmek için çoğunlukla **System Center Configuration Manager (SCCM)** veya **grup ilkesi (GP)** seçeneğini kullanır.

Ortamınızda şirket içi AD ayak izi varsa ve Azure Active Directory ile sağlanan özelliklerden yararlanmak istiyorsanız hibrit Azure AD’ye katılmış cihazları uygulayabilirsiniz. Bu cihazlar, şirket içi Active Directory'nize katılmış ve Azure Active Directory'ye kayıtlı cihazları olan.

|   | Hibrit Azure AD'ye katılma |
| --- | --- |
| **Tanım** | Şirket içi AD ile Azure AD kuruluş hesabı gerekmeden cihaza oturum açmak için birleştirilmiş |
| **Birincil hedef kitlesi** | Hibrit kuruluşlar, mevcut şirket içi için AD altyapısı uygun |
|   | Bir kuruluştaki tüm kullanıcılar için geçerlidir |
| **Cihaz sahipliği** | Kuruluş |
| **İşletim sistemleri** | Windows 10, 8.1 ve 7 |
|   | Windows Server 2008/R2, 2012/R2 2016 ve 2019 |
| **Sağlama** | Windows 10, Windows Server 2016/2019 |
|   | Etki alanına katılma ile BT ve otomatik birleştirmeyi aracılığıyla Azure AD Connect veya ADFS yapılandırma |
|   | Windows Autopilot ve Azure AD Connect veya ADFS config aracılığıyla otomatik birleştirmeyi etki alanına katılma |
|   | Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 ve Windows Server 2008 R2 - MSI gerektirir |
| **Cihaz oturum açma seçenekleri** | Kuruluş hesapları kullanarak: |
|   | Parola |
|   | Win10 için iş için Windows Hello |
| **Cihaz yönetimi** | Grup İlkesi |
|   | System Center Configuration Manager tek başına veya Intune ortak yönetim |
| **Anahtar özellikleri** | Hem bulut hem de şirket içi kaynaklara SSO |
|   | Koşullu erişim etki alanına katılmış veya Intune aracılığıyla yönetilen birlikte |
|   | Kilit ekranında Self Servis parola sıfırlama ve Windows Hello PIN sıfırlama |
|   | Kurumsal durumda Dolaşım cihazlar arasında |

![Hibrit Azure AD’ye katılmış cihazlar](./media/concept-azure-ad-join-hybrid/azure-ad-hybrid-joined-device.png)

## <a name="scenarios"></a>Senaryolar

Varsa katılmış cihazların Azure AD karma kullanın:

- Active Directory makine kimlik doğrulamasına dayalı bu cihazlara dağıtılan Win32 uygulamalarınız varsa.
- Cihaz yapılandırmasını yönetmek için Grup İlkesi'ni kullanmaya devam etmek istiyor.
- Cihazları yapılandırmak ve dağıtmak için mevcut görüntü çözümlerini kullanmaya devam etmek istiyor.
- Alt düzey Windows 7 ve Windows 10 yanı sıra 8.1 cihazları desteklemesi gerekir

## <a name="next-steps"></a>Sonraki adımlar

- [Hibrit Azure AD katılımınızı uygulamayı planlama](hybrid-azuread-join-plan.md)
- [Azure portalını kullanarak cihaz kimliklerini yönetme](device-management-azure-portal.md)
- [Azure AD'de eski cihazları yönetme](manage-stale-devices.md)
