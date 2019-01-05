---
title: Lisans Azure Active Directory Self Servis parola
description: Azure AD Self Servis parola sıfırlama lisans gereksinimleri
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 6da0bddc3f6c90d0ecd3a554988f510e1063caac
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54043048"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Lisans gereksinimleri için Azure AD Self Servis parola sıfırlama

Azure Active Directory (Azure AD) dört sürüm halinde sunulur: Ücretsiz, temel, Premium P1 ve Premium P2. Yaptığınız değişikliği de dahil olmak üzere, Self Servis parola sıfırlamayı sıfırlama, kilidini birçok farklı özelliği ve Azure AD'ye farklı sürümlerde kullanılabilir olan geri yazma vardır. Bu makalede farklar açıklanmaktadır dener. Her Azure AD sürümünde bulunan özellikler hakkında daha fazla ayrıntı bulunabilir [Azure Active Directory fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Sürümleri ve özellikleri karşılaştırın

Azure AD Self Servis parola sıfırlama, kullanıcı başına lisanslanır, uyum sağlamak için kuruluşlar, kullanıcılara uygun lisansları atamak için gereklidir.

* Bulut kullanıcıları için Self Servis Parola Değiştirme
   * Ben bir **yalnızca bulut kullanıcı** ve parolamı biliyorum.
      * İstiyorum **değiştirme** parolamı yeni bir şey.
   * Bu işlev, Azure AD tüm sürümlerinde dahildir.

* Bulut kullanıcıları için Self Servis Parola Sıfırlama
   * Ben bir **yalnızca bulut kullanıcı** ve parolamı unutmuş.
      * İstiyorum **sıfırlama** parolamı biliyorum bir şey.
   * Bu işlev, Azure AD temel, Premium P1 veya Premium P2 sürümlerinde bulunur.

* Self Servis parola sıfırlama/değiştirme/kilidini açma **ile şirket içi geri yazma**
   * Ben bir **karma kullanıcı** my şirket içi Active Directory kullanıcı hesabı Azure AD Connect kullanarak Azure AD hesabımla eşitlenir. Parolamı değiştirme, parolamı unuttunuz veya kilitli istiyorum.
      * Parolamı değiştirme veya bir şeye miyim bilmiyorsanız veya Hesabımı kilidini sıfırlama istiyorum **ve** geri eşitlenmiş değişiklik Active Directory şirket olduğunu.
   * Bu işlev, Azure AD Premium P1 veya Premium P2 sürümleri dahildir.

> [!WARNING]
> Tek başına Office 365 planları lisanslama *"Self Servis parola sıfırlama/değiştirme/kilidini açma şirket içi geri yazma ile" desteklemeyen* ve Azure AD Premium P1 veya Premium P2 sürümleri için bu işlevleri içeren bir planı gerekir İş.
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
