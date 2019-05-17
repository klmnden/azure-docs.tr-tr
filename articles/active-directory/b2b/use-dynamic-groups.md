---
title: Dinamik gruplar ve B2B işbirliği - Azure Active Directory | Microsoft Docs
description: Azure AD dinamik grupları Azure Active Directory B2B işbirliği ile kullanma işlemini gösterir
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f3cbdfa590583da59a5083f52595d54cc7f4f86
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65767210"
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Dinamik gruplar ve Azure Active Directory B2B işbirliği

## <a name="what-are-dynamic-groups"></a>Dinamik gruplar nelerdir?
Azure Active Directory (Azure AD) için güvenlik grubu üyeliğinin dinamik yapılandırması kullanılabilir [Azure portalında](https://portal.azure.com). Yöneticiler kullanıcı özniteliklerini (örneğin, userType, bölüme veya ülke/bölge) temel alarak Azure AD'de oluşturulan grupları doldurmak için kurallar ayarlayabilirsiniz. Üyeleri otomatik olarak eklenen veya kendi özniteliklerine dayalı güvenlik grubundan kaldırıldı. Bu gruplar, uygulamalara veya Bulut kaynaklarına (SharePoint siteleri, belgeler) ve üyelerine lisansları atamak için erişim sağlayabilir. Dinamik grupları hakkında daha fazla bilgiyi [adanmış grupları Azure Active Directory'de](../active-directory-accessmanagement-dedicated-groups.md).

Uygun [Azure AD Premium P1 veya P2 lisansı](https://azure.microsoft.com/pricing/details/active-directory/) dinamik grupları oluşturma ve kullanma için gereklidir. Bu makalede daha fazla bilgi edinin [Azure Active Directory'de dinamik grup üyeliği için öznitelik tabanlı kurallar oluşturma](../users-groups-roles/groups-dynamic-membership.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Yerleşik dinamik gruplar nelerdir?
**Tüm kullanıcılar** dinamik Grup tek bir tıklamayla kiracıdaki tüm kullanıcılar içeren bir grup oluşturmak Kiracı yöneticileri etkinleştirir. Varsayılan olarak, **tüm kullanıcılar** Grup üyeleri ve konuklar dahil dizindeki tüm kullanıcıları içerir.
Yeni Azure Active Directory Yönetici portalında etkinleştirmeyi seçebilirsiniz **tüm kullanıcılar** Grup Grup ayarlarını görüntüleme.

![Evet olarak ayarlanırsa tüm kullanıcılar grubunda gösterir etkinleştir](media/use-dynamic-groups/enable-all-users-group.png)

## <a name="hardening-the-all-users-dynamic-group"></a>Tüm kullanıcıların dinamik Grup sağlamlaştırma
Varsayılan olarak, **tüm kullanıcılar** grubu, B2B işbirliği (konuk) kullanıcılarınızın de içerir. Daha fazla güvenli, **tüm kullanıcılar** Konuk kullanıcıları kaldırmak için bir kural kullanarak göre gruplandırın. Aşağıdaki çizimde gösterildiği **tüm kullanıcılar** grubunu değiştiren Konukları dışlanacak.

![Gösterir kural burada kullanıcı türü Konuk eşit değildir](media/use-dynamic-groups/exclude-guest-users.png)

Ayrıca Konuk kullanıcılar yalnızca kendilerine içerir ve böylece (örneğin, Azure AD koşullu erişim ilkeleri) ilkeleri uygulayabilirsiniz yeni dinamik bir grup oluşturmak yararlı.
Bu tür bir gruba hangi gibi görünmelidir:

![Burada Konuk kullanıcı türü eşittir gösterir kuralı](media/use-dynamic-groups/only-guest-users.png)

## <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği kullanıcı özellikleri](user-properties.md)
- [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)
- [B2B işbirliği kullanıcıları için koşullu erişim](conditional-access.md)

