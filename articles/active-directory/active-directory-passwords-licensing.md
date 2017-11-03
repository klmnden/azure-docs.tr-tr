---
title: 'Lisans: Azure AD SSPR''yi | Microsoft Docs'
description: "Lisans gereksinimlerini Azure AD Self Servis parola sıfırlama"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: db849cd1e9da634064f79fbc041098542580ad02
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Lisans gereksinimleri için Azure AD Self Servis parola sıfırlama

Azure AD parola sıfırlama işlevi için sırayla, **kuruluşunuzda atanan en az bir Lisansı olmalıdır**. Kullanıcı başına parola sıfırlama deneyimi lisans uygulamaz. Microsoft lisans sözleşmenize ile uyumluluğu korumak için premium özellikleri kullanan kullanıcılara lisans atamanız gerekir.

* **Yalnızca bulut kullanıcıları** -Office 365 (O365) herhangi bir SKU veya Azure AD temel Ücretli
* **Bulut** ve/veya **şirket içi kullanıcıların** -Azure AD Premium P1 veya P2, Enterprise Mobility + Security (EMS) veya güvenli üretken Enterprise (Parametreyi)

## <a name="licenses-required-for-password-writeback"></a>Parola geri yazma için gerekli lisansları

Parola geri yazma özelliğini kullanmak için aşağıdaki lisansları kiracınızda atanmış olması gerekir.

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3
* Enterprise Mobility + Security E5
* Microsoft 365 E3
* Microsoft 365 E5

> [!WARNING]
> Tek başına Office 365 planları lisans **parola geri yazma desteklemeyen** ve bu işlevlerin çalışması önceki planlardan gerektirir.

Maliyetleri de dahil olmak üzere ek lisans bilgileri şu sayfalarda bulunabilir.

* [Azure Active Directory fiyatlandırma site](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory özellikleri ve yetenekleri](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Grup veya kullanıcı tabanlı lisans etkinleştirme

Artık Azure AD, Grup tabanlı bir kullanıcılar grubuna toplu lisansları atamak için izin verme Yöneticiler lisans yerine bunları birer birer atama destekler. [Ata, doğrulayın ve lisans sorunları gidermek](active-directory-licensing-group-assignment-azure-portal.md#step-1-assign-the-required-licenses)

Bazı Microsoft Hizmetleri tüm konumlarda kullanılabilir değil. Bir kullanıcıya bir lisans atanması önce yönetici kullanıcıya "Kullanım konumu" özelliği belirtmeniz gerekir. Lisans atama kullanıcı altında yapılabilir > Profil > Azure Portalı'ndaki ayarları. **Grup lisans atamasını kullanırken, belirtilen bir kullanım konumu olmayan tüm kullanıcılar dizininin konumunu devralır.**

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR başarılı bir sunum nasıl tamamlamak?](active-directory-passwords-best-practices.md)
* [Sıfırlama veya parolanızı değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self Servis parola sıfırlama için kaydetme](active-directory-passwords-reset-register.md).
* [Hangi verilerin SSPR tarafından kullanılır ve hangi verilerin, kullanıcılarınız için doldurmanız gerekir?](active-directory-passwords-data.md)
* [Hangi kimlik doğrulama yöntemlerinin kullanıcıların var mı?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile ilkesi seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden t hakkında önemli?](active-directory-passwords-writeback.md)
* [SSPR etkinliğinde üzerinde nasıl rapor edebilirim?](active-directory-passwords-reporting.md)
* [Tüm SSPR seçeneklerinde nedir ve ne anlama geldiklerini?](active-directory-passwords-how-it-works.md)
* [Bir şey bozuk düşünüyorum. SSPR nasıl sorun giderme?](active-directory-passwords-troubleshoot.md)
* [Herhangi bir yerde else kapsanmayan bir soru sahip](active-directory-passwords-faq.md)
