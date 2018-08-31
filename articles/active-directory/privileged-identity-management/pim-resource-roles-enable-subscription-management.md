---
title: Kiracınızdaki - Azure abonelik yönetimini etkinleştirme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) kullanırken, kiracınızdaki etkin abonelik yönetimi etkinleştirme konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 03/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 89bb6fd48c58b7672b7a2251a172cc169093d368
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190004"
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

- [PIM'de yönetmek için Azure kaynaklarını keşfedin](pim-resource-roles-discover-resources.md)
- [PIM'de Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
