---
title: Privileged Identity Management'ı kullanarak Azure kaynakları için rolleri Ata | Microsoft Docs
description: PIM rolleri atamak açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 246467d5eeebd43b8d89d98a30fdfc1c5ca0909a
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233664"
---
# <a name="assign-roles-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynakları için rolleri Ata

## <a name="assign-roles"></a>Rolleri Ata

Görüntülediğiniz zaman bir kullanıcı veya grup için bir rol atamasını **rolleri** bölmesinde rolü seçin ve ardından **Kullanıcı Ekle**. 

!["Rol" bölmesi "Kullanıcı Ekle" düğmesi](media/azure-pim-resource-rbac/rbac-assign-roles-1.png)

Öğesini de seçebilirsiniz **Kullanıcı Ekle** gelen **üyeleri** bölmesi.

!["Kullanıcı Ekle" düğmesi "Üye" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-2.png)


Kullanıcı ekliyoruz ya da Gruplandır **üyeleri** bölmesinde gerekir: 

1. Bir rol seçin **bir rol seçin** bir kullanıcı veya grup seçebilmeniz için önce bölmesi.

   !["Bir rol seçin" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-select-role.png)

2. Bir kullanıcı veya grup dizinden seçin.

3. Aşağı açılan menüsünden uygun atama türünü seçin: 

   - **Tam zamanında**: kullanıcı veya grup üyeleri rol süresiz olarak veya belirli bir süre için uygun ancak kalıcı değil erişmenizi sağlar (Rol ayarlarında yapılandırılmışsa). 
   - **Doğrudan**: (sürekli erişim olarak da bilinir) rol ataması etkinleştirmek kullanıcının veya grubun üyeleri gerektirmez. Görev tamamlandığında, burada erişim gerekli olmayacak kısa süreli kullanım için doğrudan atanmasına kullanmanızı öneririz. Çağrısı kaydırmalar ve zamana duyarlı etkinlikleri örnektir.

4. Atama gerekiyorsa kalıcı (yalnızca zaman atama için uygun kalıcı olarak veya doğrudan bir atama için kalıcı olarak etkin), aşağıdaki onay kutusunu seçin **atama türü** kutusu.

   !["Atama türü" kutusu ve ilgili onay kutusu "Üyeliği ayarları" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-settings.png)

   >[!NOTE]
   >Onay kutusu başka bir yöneticinin her atama türü yüksek atama süresince rol ayarlarında belirtilen, değiştirilemeyen olabilir.

   Belirli atama süresi belirtmek için onay kutusunu temizleyin ve başlangıç ve/veya son tarih ve saat kutularını değiştirin.

   ![Başlangıç tarihi, başlangıç zamanı, bitiş tarihi ve bitiş zamanı kutularının "Üyeliği ayarları" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-duration.png)


## <a name="manage-role-assignments"></a>Rol atamalarını yönetme

Yöneticiler, rol atamaları ya da seçerek yönetebilir **rolleri** veya **üyeleri** sol bölmeden. Seçme **rolleri** yönetim görevlerini belirli bir rol için kapsamı sağlar. Seçme **üyeleri** kaynak için tüm kullanıcı ve Grup rol atamalarını görüntüler.

!["Rol" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-roles.png)

!["Üye" bölmesi](media/azure-pim-resource-rbac/rbac-assign-roles-members.png)

>[!NOTE]
Rol etkinleştirme bekleyen varsa, üyelik görüntülerken bölmenin en üstünde bir bildirim başlığı görüntülenir.


## <a name="modify-existing-assignments"></a>Varolan atamalarını değiştirin

Kullanıcı/Grup ayrıntı görünümünden varolan atamalarını değiştirmek için seçin **Ayarları Değiştir** eylem çubuğunda. Atama türü değiştirme **zaman sadece** veya **doğrudan**.

!["Kullanıcı" Ayrıntılar "Ayarlarını değiştir" düğmesi](media/azure-pim-resource-rbac/rbac-assign-role-manage.png)
