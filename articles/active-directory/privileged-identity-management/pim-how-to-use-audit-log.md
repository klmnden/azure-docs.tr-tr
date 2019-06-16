---
title: Denetim geçmişi Azure AD rolleri için PIM - Azure Active Directory görüntüleme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure AD rolleri için denetim geçmişini görüntülemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/10/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8061cff8d39db66cb22a5650c7688657aa8b3554
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053946"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>Azure AD PIM rolleri için denetim geçmişini görüntüleme

Tüm ayrıcalıklı roller için son 30 gün içinde rol atamalarını ve etkinleştirmeler görmek için Azure Active Directory (Azure AD) Privileged Identity Management (PIM) denetim Geçmişi'ni kullanabilirsiniz. Etkinlik, dizininizdeki yönetici ve son kullanıcı eşitleme etkinliği de dahil olmak üzere tam denetim geçmişini görmek istiyorsanız kullanabileceğiniz [Azure Active Directory güvenlik ve Etkinlik raporlarını](../reports-monitoring/overview-reports.md).

## <a name="view-audit-history"></a>Denetim geçmişini görüntüleme

Azure AD rolleri denetim geçmişini görüntülemek için aşağıdaki adımları izleyin.

1. Oturum [Azure portalında](https://portal.azure.com/) üyesi olan bir kullanıcı ile [ayrıcalıklı Rol Yöneticisi](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) rol.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure AD rolleri**.

1. Tıklayın **Dizin rolleri denetim geçmişi**.

    Denetim geçmişinizi bağlı olarak bir sütun grafik toplam etkinleştirmelerin, günlük en fazla etkinleştirme ve günlük ortalama etkinleştirme ile birlikte görüntülenir.

    ![Dizin rolleri denetim geçmişi](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    Sayfanın en altında her bir eylemin kullanılabilir denetim geçmişi hakkında bilgi içeren bir tablo görüntülenir. Sütun, aşağıdaki anlamlara sahiptir:

    | Sütun | Açıklama |
    | --- | --- |
    | Zaman | Eylemin gerçekleştiği. |
    | İstek sahibi | Rol etkinleştirme ya da değişiklik isteyen kullanıcı. Değer ise **Azure sistem**, daha fazla bilgi için Azure denetim geçmişini kontrol edin. |
    | Eylem | İstek sahibi tarafından gerçekleştirilen eylemler. Ata, Atamayı Kaldır, etkinleştirme, devre dışı bırak veya AddedOutsidePIM eylemler ekleyebilirsiniz. |
    | Üyesi | Etkinleştiriyor veya bir role atanmış kullanıcı. |
    | Rol | Rol atanmış veya kullanıcı tarafından etkinleştirildi. |
    | Mantık yürütme | Neden alana etkinleştirme sırasında girilen metni. |
    | süre sonu | Etkin bir rol süresinin sona erdiği. Yalnızca uygun rol atamaları için geçerlidir. |

1. Denetim geçmişi sıralamak için tıklayın **zaman**, **eylem**, ve **rol** düğmeleri.

## <a name="filter-audit-history"></a>Denetim geçmişi Filtrele

1. Denetim geçmişi sayfasının üst kısmında tıklayın **filtre** düğmesi.

    **Grafik parametrelerini güncelleştir** bölmesi görünür.

1. İçinde **zaman aralığı**, bir zaman aralığı seçin.

1. İçinde **rolleri**, görüntülemek istediğiniz rolleri için onay işaretleri koyacağız ekleyin.

    ![Grafik parametreleri bölmesini güncelleştir](media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. Tıklayın **Bitti** filtre uygulanmış bir denetim geçmişini görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüle](azure-pim-resource-rbac.md)
