---
title: Privileged Identity Management Azure kaynaklarını için-abonelik yönetimini etkinleştirin | Microsoft Docs
description: Bilgi edinmek nasıl genel yöneticileri Kiracı aboneliklerini yönetebilirsiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 03/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 628ee70f7eb59673d4229441e3c4242e1ef8e0d3
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234281"
---
# <a name="enable-subscription-management-in-your-tenant"></a>Kiracı abonelik yönetimini etkinleştirme

Dizininizin genel Yöneticisi olarak, tüm abonelik kaynaklarına varsayılan erişim kiracınızda sahip olmayabilir. Bu makalede kendiniz kiracınızda tüm abonelikleri erişmesini sağlamak için gereken adımları özetler. Ayrıca, erişim aldıktan sonra kuruluşunuza tüm güvenlik denetimleri ile uyumlu kalan bir önerilen yaklaşım sağlar.

## <a name="who-can-enable-management-of-subscriptions-in-my-directory"></a>Kimin Dizinim Aboneliklerde yönetim etkinleştirebilir?

Genel yönetici rolüne atanan her bir kullanıcı abonelik yönetimini etkinleştirmek için aşağıdaki adımları izlemelisiniz. Kendiniz için abonelik yönetimi etkinleştirdikten sonra kaynak gerekebilecek başka genel Yöneticiler ekleyebilirsiniz de erişim. Genel yönetici rolüne tüm üyeleri için erişim sağlayan hiçbir dizin ayarı yoktur.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure portalına genel Yönetici rolüne uygun veya etkin bir üyesi olan bir hesapla oturum açın. Uygun bir genel yönetici hesabı ise, sonraki adıma geçmeden önce ilk olarak rolü etkinleştirmeniz gerekir.

## <a name="access-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim Merkezi erişim

Azure portalına genel yönetici olarak oturum, Azure abonelikleri erişim sağlayan ayarları düzenleyebilirsiniz. Azure Active Directory (Azure AD) Yönetim Merkezi'ne gözatın ve seçin **özellikleri**.

![Vurgulanan özellikleriyle ekran Azure AD Yönetim Merkezi](media/azure-pim-resource-rbac/aad_properties.png)

Özellikler listesinde altında **genel yönetici Azure Aboneliklerini yönetmek**seçin **Evet**.

![Ekran özelliklerin sayfasıyla geçiş Evet olarak ayarlayın](media/azure-pim-resource-rbac/aad_properties_save.png)

Artık hesabınızı her Kiracı aboneliği kaynak için kullanıcı erişimi yöneticisi rolü için otomatik olarak eklenir.

## <a name="browse-to-azure-ad-pim"></a>Azure AD PIM Gözat

 Buradan, Azure AD Privileged Identity Management (PIM için) gidin. Altında **Yönet**seçin **Azure kaynaklarını**.

![Ekran görüntüsü, PIM, vurgulanmış Azure kaynakları ile](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

## <a name="manage-and-discover-resources"></a>Kaynakları bulmak ve yönetme

Kuruluşunuz zaten yöneticileri Azure AD'de korumak için Azure AD PIM kullanıyorsa, dikey yüklediğinde Aboneliklerin listesini görebilirsiniz.

![Ekran görüntüsü, PIM, dikey penceresinde gösterilen Aboneliklerin listesini ile](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

> [!NOTE]
> Herhangi bir kaynağa görmüyorsanız onaylayın:
>- Genel yönetici rolü'nüz süresi geçmedi. 
>- Kuruluşunuzda bir Azure aboneliğiniz varsa.

![Ekran görüntüsü, PIM, boş bir kaynak listesi](media/azure-pim-resource-rbac/aadpim_rbac_empty_resource_list.png)

## <a name="configure-assignments"></a>Atamaları yapılandırın

Listeden bir abonelik seçin ve uygun bir kaynağın sahibi olarak kendiniz (veya bir üyesi olan bir grup) atayın. 
[Rol atama hakkında daha fazla okuma](pim-resource-roles-assign-roles.md).

Sonraki adıma geçmeden önce her bir kaynak için bu işlemi yineleyin.

## <a name="clean-up-standing-access"></a>Durumu erişimi Temizle

Önemli abonelikler için uygun atamaları, kuruluşunuzda sahip olduğunuza göre dizin özelliklerini seçeneği devre dışı bırakarak durumu erişimi temizleyebilirsiniz.

![Ekran görüntüsü, Özellikler sayfasında, iki durumlu Hayır olarak ayarlayın](media/azure-pim-resource-rbac/aad_properties_no.png)

## <a name="next-steps"></a>Sonraki adımlar

[Kaynakları bulma](pim-resource-roles-discover-resources.md)

[Rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)








