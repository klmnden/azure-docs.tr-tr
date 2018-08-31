---
title: PIM'de yönetmek için Azure kaynaklarını bulun | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) yönetmek için Azure kaynaklarını keşfedin. öğrenin.
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
ms.openlocfilehash: b5d48b3f854afaa79574e0ec13cff91f60396ac6
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190667"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>PIM'de yönetmek için Azure kaynaklarını keşfedin

Bulma ve Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kullandığınızda, Azure kaynaklarını yönetme hakkında bilgi edinin. Bu bilgiler zaten yönetici kaynakları korumak için PIM kullanan kuruluşlar ve üretim kaynaklarını arayan bir abonelik sahipleri için yararlı olabilir.

Azure kaynakları için PIM ayarlamanız, bulmak ve PIM ile korunacak kaynakların seçmek gerekir. PIM ile yönetebileceğiniz kaynakları sayısı sınırı yoktur. Ancak, en kritik (üretim) kaynaklarınız ile başlamanızı öneririz.

> [!NOTE]
> Yalnızca bulup PIM kullanarak yönetmek için yönetim grubuna veya aboneliğe kaynakları seçin. Bir yönetim grubu veya abonelik PIM yönetme, ayrıca alt kaynakları yönetebilir.

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

- [PIM'de Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
