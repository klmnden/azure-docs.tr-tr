---
title: Lisans Self Servis parola sıfırlama - Azure Active Directory
description: Azure AD Self Servis parola sıfırlama lisans gereksinimleri
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: e185b67ae73b86b5f1c3b6cda884de05eb89c6fd
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049093"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Lisans gereksinimleri için Azure AD Self Servis parola sıfırlama

Azure Active Directory (Azure AD) parola sıfırlama işlevi için sırada, *kuruluşunuzda atanmış en az bir lisansı olması gerekir* söz konusu kullanıcı için. Uygun bir lisans, bir kullanıcının bu lisansın kapsadığı özelliklerden doğrudan veya dolaylı olarak faydalandığı durumlarda gereklidir.

* **Yalnızca bulut kullanıcılarına**: Office 365 SKU veya Azure AD temel Ücretli
* **Bulut** veya **şirket kullanıcıları**: Azure AD Premium P1 veya P2, Enterprise Mobility + Security (EMS) veya Microsoft 365

## <a name="licensing-requirements-for-password-writeback"></a>Parola geri yazma için lisans gereksinimleri

**Self Servis parola sıfırlama/değiştirme/kilidini açma şirket içi geri yazma ile Azure AD premium özelliğidir**. Lisanslama hakkında daha fazla bilgi için bkz. [Azure Active Directory site fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

Parola geri yazma özelliğini kullanmak için kiracınızda atanan aşağıdaki lisanslardan birine sahip olmalıdır:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 veya A3
* Enterprise Mobility + Security E5'e veya A5
* Microsoft 365 E3 veya A3
* Microsoft 365 E5 veya A5
* Microsoft 365 F1

> [!WARNING]
> Tek başına Office 365 planları lisanslama *parola geri yazma özelliğini desteklemeyen* ve çalışmak bu işlev için önceki planlardan birine sahip olması gerekir.
>

Maliyetleri de dahil olmak üzere ek lisans bilgileri şu sayfalarda yer bulunabilir:

* [Site fiyatlandırma Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory özellikleri ve yetenekleri](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Grup veya kullanıcı tabanlı lisanslama etkinleştir

Artık Azure AD grup tabanlı lisanslama destekler. Yöneticiler, bir kerede atamak yerine kullanıcılar bir grup toplu lisansları atayabilir. Daha fazla bilgi için [atamak, doğrulamak ve lisans sorunları çözmeniz](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Bazı Microsoft hizmetleri tüm konumlarda kullanılamaz. Yönetici bir kullanıcıya lisans atanabilmesi için önce belirtmelisiniz **kullanım konumu** kullanıcı özelliği. Lisans atama altında yapılabilir **kullanıcı** > **profili** > **ayarları** bölümünde Azure portalında. *Kullanım konumu belirtilmemiş olmadan herhangi bir kullanıcı, Grup lisans ataması kullandığınızda, dizin konumunu devralır.*

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)
