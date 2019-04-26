---
title: PIM - Azure Active Directory Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüle | Microsoft Docs
description: Etkinliğini görüntülemek ve denetim geçmişi Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri için.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 74607f6a746558238ead65036d708b515d370035
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60441453"
---
# <a name="view-activity-and-audit-history-for-azure-resource-roles-in-pim"></a>PIM Azure kaynak rolleri için etkinlik ve denetim geçmişini görüntüle

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) ile kuruluşunuz içinde etkinlik etkinleştirmeleri ve Azure kaynak rolleri için denetim geçmişini görüntüleyebilirsiniz. Bu abonelik, kaynak grupları ve bile sanal makineleri içerir. Azure rol tabanlı erişim denetimi (RBAC) işlevselliği yararlanan Azure portalındaki herhangi bir kaynağa güvenlik ve yaşam döngüsü yönetimi özellikleri PIM'de yararlanabilirsiniz.

## <a name="view-activity-and-activations"></a>Etkinlikleri görüntüle ve etkinleştirme

Belirli bir kullanıcı çeşitli kaynakları sürdü hangi eylemleri görmek için verilen etkinleştirme nokta ile ilişkili Azure kaynak etkinliği görüntüleyebilirsiniz.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

1. Etkinlik ve etkinleştirmeleri için görüntülemek istediğiniz kaynağa tıklayın.

1. Tıklayın **rolleri** veya **üyeleri**.

1. Bir kullanıcı tıklayın.

    Tarihe göre Azure kaynaklarına kullanıcının eylemleri grafik bir görünümünü görürsünüz. Ayrıca, aynı süre boyunca yeni rol etkinleştirmeleri gösterir.

    ![Kullanıcı ayrıntıları](media/azure-pim-resource-rbac/rbac-user-details.png)

1. Özel rol etkinleştirme ayrıntıları ve bu kullanıcı etkin olduğu sırada gerçekleşen karşılık gelen Azure kaynak etkinliği görmek için tıklayın.

    ![Rol etkinleştirme'yi seçin](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="export-role-assignments-with-children"></a>Alt öğeleri olan rolü atamalarını dışarı aktarma

Bir uyumluluk gereksinimini burada denetçilerine rol atamaları tam listesi sağlamalısınız olabilir. PIM için sorgu rol atamaları tüm alt kaynaklar için rol atamalarını içeren belirli bir kaynak olarak sağlar. Daha önce yöneticilerin bir abonelik için rol atamalarını tam bir listesini almak için zordu ve her belirli bir kaynak rolü atamalarını dışarı aktarma gerekiyordu. PIM kullanarak, bir Abonelikteki tüm kaynak grupları ve kaynaklar için rol atamalarını dahil olmak üzere tüm etkin ve uygun rol atamalarını sorgulayabilirsiniz.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

1. Bir aboneliği gibi rol atamalarını dışarı aktarmak istediğiniz kaynağa tıklayın.

1. Tıklayın **üyeleri**.

1. Tıklayın **dışarı** dışarı aktarma üyelik bölmesini açmak için.

    ![Dışarı aktarma üyelik bölmesi](media/azure-pim-resource-rbac/export-membership.png)

1. Tıklayın **tüm üyeleri dışarı** bir CSV dosyasındaki tüm rolü atamalarını dışarı aktarma.

    ![CSV dosyasını dışarı aktar](media/azure-pim-resource-rbac/export-csv.png)

## <a name="view-resource-audit-history"></a>Kaynak denetim geçmişini görüntüleme

Kaynak Denetim, bir kaynak için tüm rol etkinliği bir görünümünü sağlar.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

1. Denetim geçmişini görüntülemek istediğiniz kaynağa tıklayın.

1. Tıklayın **kaynak denetim**.

1. Önceden tanımlanmış tarih veya özel aralığı kullanarak geçmiş filtreleyin.

    ![Kaynak Denetim filtresi](media/azure-pim-resource-rbac/rbac-resource-audit.png)

1. İçin **denetim türü**seçin **etkinleştir (atanan + etkinleştirildi)**.

    ![Etkinlik Ayrıntısı](media/azure-pim-resource-rbac/rbac-audit-activity.png)

1. Altında **eylem**, tıklayın **(etkinlik)** kullanıcının etkinlik ayrıntı Azure kaynakları bir kullanıcı için.

    ![Kullanıcı Etkinlik Ayrıntısı](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="view-my-audit"></a>Denetimim görüntüleyin

Denetimim kişisel rol etkinlik görüntülemenizi sağlar.

1. Açık **Azure AD Privileged Identity Management**.

1. Tıklayın **Azure kaynaklarını**.

1. Denetim geçmişini görüntülemek istediğiniz kaynağa tıklayın.

1. Tıklayın **Denetimim**.

1. Önceden tanımlanmış tarih veya özel aralığı kullanarak geçmiş filtreleyin.

    ![Kişisel rol etkinliği](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
- [Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri](pim-resource-roles-approval-workflow.md)
- [Azure AD PIM rolleri için denetim geçmişini görüntüleme](pim-how-to-use-audit-log.md)
