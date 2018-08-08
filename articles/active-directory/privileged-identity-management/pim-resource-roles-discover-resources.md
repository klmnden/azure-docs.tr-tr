---
title: Keşfetmek ve Privileged Identity Management'ı kullanarak Azure kaynaklarını yönetmek | Microsoft Docs
description: PIM kullanarak Azure kaynaklarını korumak nasıl açıklar.
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
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: c5b26c01028e2a5746132939a2058cacdcad859f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622111"
---
# <a name="discover-and-manage-azure-resources-by-using-privileged-identity-management"></a>Bulma ve Privileged Identity Management'ı kullanarak Azure kaynaklarını yönetme

Bulma ve Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kullandığınızda, Azure kaynaklarını yönetme hakkında bilgi edinin. Bu bilgiler zaten yönetici kaynakları korumak için PIM kullanan kuruluşlar ve üretim kaynaklarını arayan bir abonelik sahipleri için yararlı olabilir.

Azure kaynakları için PIM ayarlamanız, bulmak ve PIM ile korunacak kaynakların seçmek gerekir. PIM ile yönetebileceğiniz kaynakları sayısı sınırı yoktur. Ancak, en kritik (üretim) kaynaklarınız ile başlamanızı öneririz.

> [!NOTE]
> Yalnızca bulup PIM kullanarak yönetmek için abonelik kaynaklarını seçin. Bir abonelikte PIM yönetirken, abonelik alt kaynakları da yönetebilirsiniz.

## <a name="discover-resources"></a>Kaynakları bulma

Azure portalında Git **Privileged Identity Management** bölmesi. Soldaki menüde içinde **Yönet** bölümünden **Azure kaynaklarını**.

!["Privileged Identity Management - Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

Azure kaynakları için PIM kullanarak ilk kez buysa, yönetilecek kaynakları bulmak için bulma işlemini çalıştırın. İçinde **kaynakları bulmak** bölmesinde **kaynakları bulmak** bulma deneyimini başlatmak için düğme.

!["Kaynakları Bul" bölmesi](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Kuruluşunuzdaki başka bir kaynak veya dizin Yöneticisi PIM kullanarak bir Azure kaynağı zaten yönetme ya da bir uygun rol ataması için bir kaynak varsa, liste görünümü iletiyi görüntüler **kaynakları bulun veya etkinleştirme bir devam etmek için uygun rol atamasını**. 

!["Kaynakları Bul" düğmesini "ayrıcalıklı Kimlik Yöneticisi'nde - Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Seçtiğinizde, **kaynakları bulmak** düğmesi, üstteki menüden olup olmadığını veya bölmenin ortasında yönetebileceğiniz aboneliklerin listesi görüntülenir. Vurgulanan abonelikleri zaten PIM tarafından korunur.

> [!NOTE]
> PIM ayarları, yönetilen bir abonelik ayarlanır sonra kaldırma başka bir kaynak yöneticisi önlemek için abonelik yönetilmeyen olamaz.

!["Azure kaynakları - bulma" bölmesi](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

İçinde **kaynak** sütun, PIM ile korumak istediğiniz bir abonelik üzerine fare getirin. Ardından, kaynak adının sol tarafındaki onay kutusunu seçin. Aynı anda birden çok abonelik seçebilirsiniz.

!["Azure kaynakları - bulma" kaynak listesini bölmesi](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

Üst menüde, ekleme işlemini başlatmak için seçin **kaynak yönetme**.

!["Azure kaynakları - bulma" "kaynak yönetme" düğmesini bölmesi](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Seçilen kaynaklara artık PIM tarafından yönetiliyor. Bulma ekranında sağ üst köşede, kapatmak için seçin **X**. PIM ayarları yönetme ve üst kısmındaki menüde atama üyeleri başlamak için **Privileged Identity Management - Azure kaynaklarını** bölmesinde **Yenile** düğmesi.

![Üst menü "Privileged Identity Management - Azure kaynaklarını", "Yenile" düğmesini bölmesi](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [PIM Rolleri Ata](pim-resource-roles-assign-roles.md)
