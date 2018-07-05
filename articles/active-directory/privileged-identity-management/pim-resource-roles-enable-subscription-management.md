---
title: -Azure kaynakları için Privileged Identity Management abonelik yönetimini etkinleştirme | Microsoft Docs
description: Nasıl genel Yöneticiler öğrenin Kiracı aboneliklerini yönetebilirsiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 03/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 6aeb82ff1feb3521f3a09dc1b28186754568bb27
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443006"
---
# <a name="enable-subscription-management-in-your-tenant"></a>Kiracınızdaki abonelik yönetimini etkinleştirme

Dizininizin genel Yöneticisi olarak, varsayılan tüm abonelik kaynaklarına kiracınızda erişemeyebilirsiniz. Bu makalede kendiniz kiracınızdaki tüm abonelikler için erişim vermek için gereken adımları özetler. Ayrıca, tüm güvenlik denetimleri erişim aldıktan sonra kuruluşunuzun ihtiyaç uyumlu kalan önerilen bir yaklaşım sağlar.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Kimlerin yönetim dizinimdeki aboneliği etkinleştirebilir miyim?

Her kullanıcı için genel Yönetici rolüne atanan abonelik yönetimini etkinleştirmek için aşağıdaki adımları izlemelisiniz. Abonelik Yönetimi için kendiniz etkinleştirdikten sonra kaynak gerekebilecek diğer genel Yöneticiler ekleyebilirsiniz de erişim. Genel yönetici rolü üyeleri için erişim sağlayan bir dizin ayar yoktur.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure portalına genel Yönetici rolüne uygun ya da etkin bir üyesi olan bir hesapla oturum açın. Uygun bir genel yönetici hesabı ise sonraki adıma geçmeden önce ilk rolünü etkinleştirmeniz gerekir.

## <a name="access-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim Merkezine erişim

Azure portalına genel yönetici olarak oturum açtıktan sonra Azure abonelikleri erişmeyi ayarları düzenleyebilirsiniz. Azure Active Directory (Azure AD) Yönetim Merkezi'ne gözatın ve seçin **özellikleri**.

![Vurgulanan özellikleriyle ekran Azure AD Yönetim Merkezi](media/azure-pim-resource-rbac/aad_properties.png)

Özellikler listesinde altında **genel yönetici Azure aboneliklerini yönetebilir**seçin **Evet**.

![Ekran görüntüsü, Özellikler sayfasının, Evet olarak ayarlanırsa Aç/Kapat](media/azure-pim-resource-rbac/aad_properties_save.png)

Artık hesabınız, her Kiracı aboneliği kaynak için kullanıcı erişim Yöneticisi rolüne otomatik olarak eklenir.

## <a name="browse-to-azure-ad-pim"></a>Azure AD PIM için Gözat

 Buradan, Azure AD Privileged Identity Management (PIM için) gidin. Altında **Yönet**seçin **Azure kaynaklarını**.

![Ekran görüntüsü, PIM, vurgulandığı Azure kaynaklarıyla](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Kaynakları bulma ve yönetme

Kuruluşunuz zaten Azure AD'de yöneticileri korumak için Azure AD PIM kullanıyorsa, dikey penceresi yüklendiğinde Aboneliklerin listesini görebilirsiniz.

![Ekran görüntüsü, PIM, dikey pencerede gösterilen Aboneliklerin listesini ile](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Tüm kaynaklarını görmüyorsanız onaylayın:
>- Genel yönetici rolünüzün süresi dolmadı. 
>- Kuruluşunuz bir Azure aboneliğine sahip.

![Ekran görüntüsü, PIM, boş bir kaynak listesi](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Atamaları yapılandırın

Listeden bir abonelik seçin ve kendinizi (veya üyesi olduğunuz bir grubu) uygun bir kaynak sahibi olarak atayın. 
[Rol atama hakkında daha fazla okuma](pim-resource-roles-assign-roles.md).

Sonraki adıma geçmeden önce her bir kaynak için bu işlemi yineleyin.

## <a name="clean-up-standing-access"></a>Ayakta erişimi Temizle

Kuruluşunuzdaki uygun atamalar önemli aboneliklere sahip olduğunuza göre dizin özellikleri seçeneği devre dışı bırakarak ayakta erişimi temizleyebilirsiniz.

![Ekran görüntüsü, Özellikler sayfasının, Hayır olarak ayarlanırsa Aç/Kapat](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Sonraki adımlar

[Kaynakları bulma](pim-resource-roles-discover-resources.md)

[Rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)








