---
title: Azure Kaynakları - Ata rolleri Privileged Identity Management | Microsoft Docs
description: PIM rolleri atamak açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 9a9046afe2ee1e578333ff9d29f6fb21e95a0f22
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---assign"></a>Privileged Identity Management - kaynak rolleri - atama

## <a name="assign-roles"></a>Rolleri Ata

Bir kullanıcı veya grup için bir rol atamak için (rolleri görüntülerken) rolünü seçin, 

![](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

veya Ekle Eylem çubuğu'ndan (üyeleri görünümü varsa) tıklayın.

![](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Bir kullanıcı veya grup üyeleri sekmesinden eklemek için gerekir: 

1. Bir kullanıcı veya grup seçebilmeniz için önce Rol Ekle menüsünden seçin.

![](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Bir kullanıcı veya grup dizinden seçin.

3. Uygun atama açılır menüsünden seçin. 

    - **Yalnızca zaman ataması içinde:** kullanıcı veya grup üyeleri rolüne uygun ancak kalıcı değil erişimi olan belirli bir dönem için zaman veya süresiz olarak sağladığı (Rol ayarlarında yapılandırılmışsa). 
    - **Doğrudan atama:** (sürekli erişim olarak da bilinir) rol ataması etkinleştirmek kullanıcının veya grubun üyeleri gerektirmez. Microsoft görev tamamlandığında, burada erişim gerekli olmayacak çağrısı kaydırmalar veya zaman hassas etkinlikleri gibi kısa süreli kullanım için doğrudan atanmasına kullanılmasını önerir.

Atama türü açılır altında bir onay kutusu atama kalıcı olup olmayacağını belirtin (saat atama/kalıcı olarak etkin, sadece doğrudan ataması için etkinleştirmek kalıcı olarak uygun) sağlar.

![](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

>[!NOTE]
>Onay kutusu başka bir yöneticinin her atama türü yüksek atama süresince rol ayarlarında belirtilen, değiştirilemeyen olabilir.

 Belirli atama süresini belirtmek için onay kutusunun seçimini kaldırın ve başlangıç değiştirmek ve/veya tarih ve saat alanları bitmelidir.

![](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Rol atamalarını yönetme

Yöneticiler, sol gezinti bölmesinden rol ya da üyeleri seçerek rol atamalarını yönetebilir. Rolleri seçme üyeleri gösterirken, kaynak için tüm kullanıcı ve Grup rol atamalarını yönetim görevlerini belirli bir rol için kapsamı yöneticilerinin olanak tanır.

![](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

![](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Bir rol etkinleştirme bekleyen varsa, sayfanın en üstünde üyelik görüntülerken bir bildirim başlığı görüntülenir.


## <a name="modify-existing-assignments"></a>Varolan atamalarını değiştirin

Kullanıcı/Grup ayrıntı görünümünden varolan atamalarını değiştirmek için sayfanın üst kısmındaki eylem çubuğunda değişiklik ayarlarını seçin. Atama türü yalnızca zaman atama içinde veya doğrudan atama olarak değiştirin.

![](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
