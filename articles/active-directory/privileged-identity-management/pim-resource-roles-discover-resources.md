---
title: Bul ve Privileged Identity Management'ı kullanarak Azure kaynaklarınızı yönetmek | Microsoft Docs
description: PIM kullanarak Azure kaynaklarınızı korumak açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 51a10ea164e8bd7650ad2823281d9ed6a4c91915
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="discover-and-manage-azure-resources-by-using-privileged-identity-management"></a>Bul ve Privileged Identity Management'ı kullanarak Azure kaynaklarınızı yönetmek

Bul ve Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kullandığınızda, Azure kaynaklarınızı yönetmek hakkında bilgi edinin. Bu bilgiler zaten yönetici kaynakları korumak için PIM kullanan kuruluşlar ve üretim kaynakları güvenli hale getirmek için arayan abonelik sahipleri için yararlı olabilir.

Azure kaynakları için ilk PIM ayarladığınızda, bulmak ve PIM ile korunacak kaynakların seçmek gerekir. PIM ile yönetebileceğiniz kaynakları sayısına bir sınır yoktur. Ancak, en kritik (üretim) kaynaklarınızı ile başlamanızı öneririz.

> [!NOTE]
> Yalnızca aramak ve PIM kullanarak yönetmek için abonelik kaynakları seçin. Bir abonelik PIM yönetirken abonelik alt kaynakları da yönetebilirsiniz.

## <a name="discover-resources"></a>Kaynakları bulmak

Azure portalında Git **Privileged Identity Management** bölmesi. Soldaki menüde içinde **Yönet** bölümünde, select **Azure kaynaklarını**.

!["Privileged Identity Management - Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Önce ilk kez PIM için Azure kaynaklarını kullanıyorsanız yönetmek için kaynakları bulmak için bulma işlemini çalıştırın. İçinde **kaynakları bulmak** bölmesinde, **kaynakları bulmak** bulma deneyimini Başlat düğmesi.

!["Kaynakları bulmak" bölmesi](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Kuruluşunuzdaki başka bir kaynak veya dizin yönetici PIM kullanarak bir Azure kaynağı zaten yönetme ya da bir kaynak için uygun rol atama varsa, liste görünümü görüntülenir **kaynakları bulmak veya etkinleştirme bir devam etmek için uygun rol ataması**. 

!["Kaynakları Bul" düğmesini "ayrıcalıklı Kimlik Yöneticisi'nde - Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Seçtiğinizde, **kaynakları bulmak** düğmesini tıklatın, yönetebileceğiniz Aboneliklerin listesini üstteki menüden olup olmadığını veya bölmesinde ortasında görünür. Vurgulanan abonelikleri zaten PIM tarafından korunur.

> [!NOTE]
> Başka bir kaynak yönetici yönetilen bir abonelik ayarlanır sonra PIM ayarları kaldırmasını önlemek için abonelik yönetilmeyen olamaz.

!["Azure kaynaklarını - bulma" bölmesi](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

İçinde **kaynak** sütun, farenizi PIM ile korumak istediğiniz bir abonelik üzerine getirin. Ardından, kaynak adı solundaki onay kutusunu seçin. Aynı anda birden çok abonelik seçebilirsiniz.

!["Azure kaynaklarını - bulma" kaynaklarında listesi bölmesi](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Üst menüde ekleme işlemini başlatmak için seçin **kaynak yönetmek**.

!["Azure kaynaklarını - bulma" "kaynak Yönet" düğmesini bölmesi](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Seçili kaynakları artık PIM tarafından yönetilir. Bulma ekranında sağ üst köşesinde, kapatmaya seçin **X**. Yönetme PIM ayarları ve üst kısmındaki menüde atama üyeleri başlamak için **Privileged Identity Management - Azure kaynaklarını** bölmesinde, **yenileme** düğmesi.

![Üstteki menüde "Privileged Identity Management - Azure kaynaklarını", "Yenile" düğmesini bölmesi](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [PIM rolleri atama](pim-resource-roles-assign-roles.md)
