---
title: "Dinamik gruplar ve Azure Active Directory B2B işbirliği | Microsoft Docs"
description: "Azure AD dinamik grupların Azure Active Directory B2B işbirliği ile nasıl kullanılacağını gösterir"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 12/14/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: d60b7f70332d6183a67003c1dceb96241fc6bae4
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dinamik gruplar ve Azure Active Directory B2B işbirliği

## <a name="what-are-dynamic-groups"></a>Dinamik grupların nelerdir?
Azure Active Directory (Azure AD) için dinamik yapılandırma, güvenlik grubunun üyeliği kullanılabilir [Azure portalı](https://portal.azure.com). Yöneticiler (örneğin, userType, bölüme veya ülke) kullanıcı özniteliklerini temel alarak Azure AD'de oluşturulan grupları doldurmak için kurallar ayarlayabilir. Üyeler otomatik olarak eklenecek veya bunların özniteliklerini temel alarak bir güvenlik grubu kaldırıldı. Bu grupları, uygulamalar veya Bulut kaynakları (SharePoint siteleri, belgeleri) ve üyelerine lisansları atamak için erişim sağlayabilir. Dinamik grupları hakkında daha fazla bilgiyi [ayrılmış Azure Active Directory'deki grupları](active-directory-accessmanagement-dedicated-groups.md).

Uygun [lisans Azure AD Premium P1 veya P2](https://azure.microsoft.com/pricing/details/active-directory/) dinamik grupları oluşturma ve kullanma için gereklidir. Makalede daha fazla bilgi edinin [öznitelik tabanlı kurallar dinamik grup üyeliği için Azure Active Directory'de oluşturmak](active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Yerleşik dinamik grupların nelerdir?
**Tüm kullanıcılar** dinamik Grup tek bir tıklatmayla kiracıdaki tüm kullanıcıları içeren bir grup oluşturmak Kiracı yöneticileri sağlar. Varsayılan olarak, **tüm kullanıcılar** Grup üyeleri ve konuklar dahil olmak üzere dizinde tüm kullanıcıları içerir.
Yeni Azure Active Directory Yönetim portalını etkinleştirmeyi seçebilirsiniz **tüm kullanıcılar** grup ayarları görünümünde grubu.

![Evet olarak ayarlanmış tüm kullanıcıları grubu gösterir etkinleştir](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

## <a name="hardening-the-all-users-dynamic-group"></a>Tüm kullanıcılar dinamik Grup sağlamlaştırma
Varsayılan olarak, **tüm kullanıcılar** grubu B2B işbirliği (konuk) kullanıcılarınızın de içerir. Daha fazla güvenli, **tüm kullanıcılar** Konuk kullanıcılar kaldırmak için bir kural kullanarak göre gruplandırın. Aşağıdaki çizimde gösterildiği **tüm kullanıcılar** grubunu değiştiren konuklar dışlanacak.

![Konuk kullanıcı girildiği değil gösterir kural eşittir](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

Ayrıca Konuk kullanıcılar yalnızca kendilerine içerir ve böylece (örneğin, Azure AD koşullu erişim ilkeleri) ilkeleri uygulayabilirsiniz yeni dinamik bir grup oluşturmak yararlı.
Ne tür bir grup aşağıdaki gibi görünmelidir:

![Konuk kullanıcı türü burada eşittir gösterir kuralı](media/active-directory-b2b-dynamic-groups/only-guest-users.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [B2B işbirliği kullanıcı özellikleri](active-directory-b2b-user-properties.md)
* [Bir role B2B işbirliği kullanıcı ekleme](active-directory-b2b-add-guest-to-role.md)
* [B2B işbirliği davetleri temsilci seçme](active-directory-b2b-delegate-invitations.md)
* [B2B işbirliği kodu ve PowerShell örnekleri](active-directory-b2b-code-samples.md)
* [SaaS uygulamaları B2B işbirliği için yapılandırma](active-directory-b2b-configure-saas-apps.md)
* [B2B işbirliği kullanıcı belirteçleri](active-directory-b2b-user-token.md)
* [B2B işbirliği kullanıcı taleplerini eşleme](active-directory-b2b-claims-mapping.md)
* [Office 365 dış paylaşım](active-directory-b2b-o365-external-user.md)
* [B2B işbirliği geçerli sınırlamalar](active-directory-b2b-current-limitations.md)
