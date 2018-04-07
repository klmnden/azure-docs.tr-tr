---
title: Privileged Identity Management bulmak ve Azure kaynaklarınızı yönetmek için Azure kaynakları - | Microsoft Docs
description: Azure kaynakları korumak açıklar.
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
ms.openlocfilehash: 78650e47ec92aa144e4ccc8c57f309240bf31ee3
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="discover-and-manage-azure-resources"></a>Keşfetmek ve Azure kaynaklarını yönetmek

Kuruluşunuz, yöneticiler dizininizde korumak için Azure AD PIM zaten kullanıyor veya üretim kaynakları güvenli hale getirmek için arayan bir abonelik sahibi olduğunuz doğru yerde demektir.

PIM için Azure kaynaklarını ilk defa etkinleştirdiğinizde bulmak ve PIM ile korunacak kaynakların seçmek gerekir. PIM ile yönetebileceğiniz kaynakları sayısına bir sınır yoktur, ancak en kritik (üretim) kaynaklarınızla başlatmayı öneririz.

> [!Note]
> Yalnızca abonelik kaynakları aranır ve yönetimi için seçtiniz. PIM bir aboneliği yönetmek seçerek de tüm alt kaynaklar için yönetim olanağı sağlar.

## <a name="discover-resources"></a>Kaynakları bulmak

Azure AD PIM gidin ve sol gezinti menüsünde Yönet bölümünde Azure kaynaklarını seçin.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resources.png)

PIM için Azure kaynaklarını kullanarak ilk kez buysa yönetmek için kaynakları bulmak için bulma işlemini çalıştırın gerekecektir.
Bulma deneyimini başlatmak için ekranın Center'da "kaynakları Bul" düğmesini tıklatın.

![](media/azure-pim-resource-rbac/aadpim_first_run_discovery.png)

Kuruluşunuzdaki başka bir kaynak veya dizin Yöneticisi zaten bir Azure kaynağı PIM ile yönetme veya bir kaynak için uygun rol atama varsa, liste görünümü iletisi içerir: "kaynakları bulmak veya bir uygun rol etkinleştirme atama"devam etmek için kullanılır. 

![](media/azure-pim-resource-rbac/aadpim_discover_eligible_not_active.png)

Eylem çubuğunda veya bulma kaynakları ekranına Orta düğmesini seçtiğinizde, Yönetim için kullanılabilir Aboneliklerin listesini görürsünüz. Bu noktada, vurgulanan abonelikleri görürseniz PIM tarafından korunan belirtir.

> [!Note]
> Başka bir kaynak yönetici PIM ayarları kaldırmasını önlemek için bir abonelik yönetilmeye başladıktan sonra yönetilmeyen olamaz.

![](media/azure-pim-resource-rbac/aadpim_discovery_some_selected.png)

PIM ile korumak ve kutusunu en solundaki satırın seçin istediğiniz bir abonelik gelin. Aynı anda birden çok abonelik seçebilirsiniz.

![](media/azure-pim-resource-rbac/aadpim_discovery_all_selected.png)

İşlem ekleme başlatmak için ekranın üstünde çubuğundaki "kaynak Yönet" düğmesini seçin.

![](media/azure-pim-resource-rbac/aadpim_discovery_click_manage.png)

Seçili kaynakları artık PIM tarafından yönetilir. Sayfanın sağ üst köşesindeki "X" kullanarak bulma ekranı kapatmak ve yönetme PIM ayarları ve atama üyeleri başlamak için Yönet Azure kaynakları ekranın üstünde çubuğundaki Yenile'yi tıklatın.

![](media/azure-pim-resource-rbac/aadpim_discovery_resources_refresh.png)

## <a name="next-steps"></a>Sonraki adımlar

[Rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)

[PIM rolleri atama](pim-resource-roles-assign-roles.md)
