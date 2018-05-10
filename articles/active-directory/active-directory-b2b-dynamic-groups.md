---
title: Dinamik gruplar ve Azure Active Directory B2B işbirliği | Microsoft Docs
description: Azure AD dinamik grupların Azure Active Directory B2B işbirliği ile nasıl kullanılacağını gösterir
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 12/14/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 751502c2be84e9454c507f09a47b609d003ce2c5
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
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

- [B2B işbirliği kullanıcı özellikleri](active-directory-b2b-user-properties.md)
- [Bir role B2B işbirliği kullanıcı ekleme](active-directory-b2b-add-guest-to-role.md)
- [B2B işbirliği kullanıcılar için koşullu erişim](active-directory-b2b-mfa-instructions.md)

