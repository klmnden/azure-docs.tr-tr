---
title: Privileged Identity Management'ı kullanarak Azure kaynakları için roller atama | Microsoft Docs
description: PIM rolleri atamak açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: da5a0e41c476a75f230e2d2645e7f5befd644a93
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441439"
---
# <a name="assign-roles-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynakları için roller atama

## <a name="assign-roles"></a>Roller atama

Görüntülediğiniz zaman bir kullanıcı veya grup için bir rol atamak için **rolleri** bölmesinde rolü seçin ve ardından **Kullanıcı Ekle**. 

!["Kullanıcı Ekle" düğmesi "Rolleri" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

Belirleyebilirsiniz **Kullanıcı Ekle** gelen **üyeleri** bölmesi.

!["Üye" bölmesindeki "Kullanıcı Ekle" düğmesi](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Bir kullanıcı ekliyoruz ya da Gruplandır **üyeleri** bölmesinde gerekir: 

1. Bir rolden seçin **bir rol seçin** bir kullanıcı veya grup seçmeden önce bölmesi.

   !["Rol Seç" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Bir kullanıcı veya grup, dizinden seçin.

3. Aşağı açılan menüden uygun atama türü seçin: 

   - **Zamanında**: kullanıcı veya grup üyeleri belirli bir süre için veya süresiz olarak rol için uygun ancak değil kalıcı erişim sağlar (Rol ayarlarında yapılandırılmışsa). 
   - **Doğrudan**: (kalıcı erişim bilinir) rol ataması etkinleştirmek kullanıcı veya grup üyeleri gerektirmez. Doğrudan atamayı kısa süreli kullanım için görev tamamlandığında burada erişim gerekli olmayacak kullanmanızı öneririz. Nöbet kaydırmalar ve zamana duyarlı etkinlikleri verilebilir.

4. Atama gerekiyorsa kalıcı (tam zamanında atama için uygun kalıcı olarak veya doğrudan atama kalıcı olarak etkin), aşağıdaki onay kutusunu seçin **atama türü** kutusu.

   !["Atama türü" ve ilgili onay kutularını ile "Üyeliği ayarları" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

   >[!NOTE]
   >Onay kutusunu başka bir yöneticinin her bir atama türü için en fazla atama süresi rol ayarlarında belirtilen değiştirilemeyen olabilir.

   Belirli atama süresi belirtmek için onay kutusunu temizleyin ve başlangıç ve/veya bitiş tarihi ve saati kutularını değiştirin.

   ![Başlangıç tarihi, başlangıç zamanı, bitiş tarihi ve bitiş zamanı kutusuyla "Üyeliği ayarları" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Rol atamalarını yönetme

Yöneticiler, ya da seçerek rol atamalarını yönetebilir **rolleri** veya **üyeleri** sol bölmeden. Seçme **rolleri** yöneticilerin yönetim görevlerini belirli bir role kapsam olanak sağlar. Seçme **üyeleri** kaynak için tüm kullanıcı ve Grup rol atamaları görüntülenir.

!["Rolleri" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

!["Üye" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Rol etkinleştirme bekleyen varsa, üyelik görüntülerken bölmenin en üstünde bir bildirim başlığı görüntülenir.


## <a name="modify-existing-assignments"></a>Mevcut atamalarını değiştirin

Kullanıcı/Grup ayrıntı görünümü mevcut atamaları değiştirmek için seçin **ayarlarını değiştir** eylem çubuğunda. Atama türü için değiştirme **tam zamanlı** veya **doğrudan**.

!["Kullanıcı" Ayrıntılar "Ayarları değiştir" düğmesi](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
