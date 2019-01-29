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
ms.subservice: pim
ms.date: 01/23/2019
ms.author: rolyon
ms.openlocfilehash: e6548c2635afd8e1ac784a1930c2277548647cfd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55192766"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>PIM'de yönetmek için Azure kaynaklarını keşfedin

Azure AD Privileged Identity Management (PIM) kullanarak, Azure kaynaklarınızın korumasını artırabilirsiniz. Bu, zaten korumak Azure AD Dizin rolleri için PIM kullanan kuruluşlar ve üretim kaynaklarını arayan yönetim grubu ve abonelik sahipleri için yararlıdır.

Azure kaynakları için PIM ayarlamanız, bulmak ve PIM ile korumak istediğiniz kaynakları seçin gerekir. PIM ile yönetebileceğiniz kaynakları sayısı sınırı yoktur. Ancak, en kritik (üretim) kaynaklarınız ile başlamanızı öneririz.

## <a name="discover-resources"></a>Kaynakları bulma

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

    Azure kaynakları için PIM kullanarak ilk kez varsa, bir bulma kaynakları bölmesinde görürsünüz.

    ![Kaynaklar - ilk kez keşfedin](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Kuruluşunuzdaki başka bir kaynak veya dizin Yöneticisi zaten PIM Azure kaynaklarını yönetmek, yönetilmekte olan kaynakların bir listesini görürsünüz.

    ![Kaynakları bölmesinde keşfedin](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Tıklayın **kaynakları bulmak** bulma deneyimini başlatmak için.

    ![Bulma bölmesi](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. Bulma bölmeyi üzerinde **kaynak durumu filtresi** ve **kaynak türünü seçin** yönetim filtrelemek için grubu veya sahip olduğunuz abonelik için yazma izni. Başlamak muhtemelen en kolay yöntemdir **tüm** başlangıçta.

    Yalnızca bulup PIM kullanarak yönetmek için yönetim grubuna veya aboneliğe kaynakları seçin. Bir yönetim grubu veya abonelik PIM yönetme, ayrıca alt kaynakları yönetebilir.

1. Yönetmek istediğiniz herhangi bir yönetilmeyen kaynağa yanında bir onay işareti ekleyin.

1. Tıklayın **kaynak yönetme** seçili kaynakları yönetmeye başlamak için.

    > [!NOTE]
    > Bir yönetim grubuna veya aboneliğe yönetilen ayarlandıktan sonra yönetilmeyen olamaz. Bu, başka bir Kaynak Yöneticisi'nin PIM ayarları çıkarmasını engeller.

    ![Bulma - Kaynak yönetme](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Seçili kaynak Yönetim için hazırlama onaylamak için bir ileti görürseniz, tıklayın **Evet**.

    ![Bulma - Kaynak ileti yönetme](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM'de Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
