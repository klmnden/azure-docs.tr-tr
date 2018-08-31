---
title: PIM Azure kaynak rolleri atama | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri atama hakkında bilgi edinin.
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
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 7019a6f97a9590d3b652584015f3077f4ed075af
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188929"
---
# <a name="assign-azure-resource-roles-in-pim"></a>PIM Azure kaynak Rolleri Ata

Azure AD PIM (ancak bunlarla sınırlı olmayan) özel roller yanı sıra yerleşik Azure kaynağı rolleri yönetebilirsiniz:

- Sahip
- Kullanıcı Erişimi Yöneticisi
- Katılımcı
- Güvenlik Yöneticisi
- Güvenlik Yöneticisi ve daha fazlası

>[!NOTE]
Kullanıcılar veya sahibi ya da kullanıcı erişimi yöneticisi rolü ve Azure AD'de abonelik yönetimini etkinleştirme genel Yöneticiler atanmış bir grubun üyelerinin kaynağa yöneticilerdir. Bu yöneticileri, Rolleri Ata, rol ayarlarını yapılandırma ve Azure kaynakları için PIM kullanarak erişimi gözden geçirin. Listesini görüntülemek [Azure kaynakları için yerleşik roller](../../role-based-access-control/built-in-roles.md).

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

## <a name="next-steps"></a>Sonraki adımlar

- [PIM'de Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Azure AD dizin rollerini PIM atayın](pim-how-to-add-role-to-user.md)
