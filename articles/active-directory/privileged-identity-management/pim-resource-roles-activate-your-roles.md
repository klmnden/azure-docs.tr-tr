---
title: Privileged Identity Management kullanarak rolleri Azure kaynakları için etkinleştirme | Microsoft Docs
description: PIM rolleri etkinleştirmeyi açıklar.
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
ms.openlocfilehash: a985e67cc566cc45b3ee6b8dc98e91a8f34abd1b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="activate-roles-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management kullanarak rolleri Azure kaynakları için etkinleştirme
Ayrıcalıklı Kimlik Yönetimi (PIM) Azure kaynakları için rol etkinleştirme içinde yeni bir deneyim sunar. Uygun Rol üyeleri etkinleştirme gelecekteki bir tarih ve saat için zamanlayabilirsiniz. Bunlar ayrıca belirli etkinleştirme süresi (yöneticiler tarafından yapılandırılmış) maksimum içinde seçebilirsiniz. Daha fazla bilgi için bkz: [etkinleştirmek veya Azure AD Privileged Identity Management rollerinde devre dışı bırakma](../active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="activate-roles"></a>Rollerini etkinleştir
Gözat **My rolleri** sol bölmede bölümü. Seçin **etkinleştirme** , etkinleştirmek istediğiniz bir rol.

!["Uygun roller" sekmesinde "Rolleri My" bölmesi.](media/azure-pim-resource-rbac/rbac-roles.png)

Gelen **etkinleştirmeleri** menüsünden başlangıç tarihini girin ve rolünü etkinleştirmek için saat. İsteğe bağlı olarak, etkinleştirme süresini azaltmak (rolü etkin olduğu süre uzunluğu) ve gerekirse bir gerekçe girin. Ardından, seçin **etkinleştirme**.

Başlangıç tarihi ve saati değiştirilmez, rol saniye içinde etkinleştirilir. İçinde **My rolleri** bölmesinde, bir başlık iletisi gösterilir rol etkinleştirmesi için kuyruğa alındı. Bu iletiyi temizlemek için Yenile düğmesini seçin.

![Bir başlık iletisi ve onay bekliyor hakkında bir bildirim "Rolleri my" bölmesi](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Bekleyen isteği etkinleştirme gelecekteki bir tarih ve zaman için planlandıysa görünür **bekleyen istekler** sol bölmenin sekmesi. Rol etkinleştirme artık gerekli değilse, istek seçerek iptal edebilirsiniz **iptal** düğmesi.

![Bekleyen istekler "İptal" düğmelerle listesi](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="apply-just-enough-administration-practices"></a>Yalnızca yeterli yönetim uygulamalarından Uygula

Kaynak rol atamalarınızı ile yalnızca yetecek kadar Yönetim (JEA) en iyi yöntemler kullanarak Azure kaynakları ile PIM basit bir işlemdir. Kullanıcı ve Grup üyeleri ile Azure Abonelikleriniz veya kaynak grupları atamalarını kendi mevcut rol ataması azaltılmış bir kapsamda etkinleştirebilirsiniz. 

Arama sayfasından yönetmeniz gereken bağımlı kaynak bulunamıyor.

![Bir kaynak seçme](media/azure-pim-resource-rbac/azure-resources-02.png)

Seçin **My rolleri** sol bölmeden ve etkinleştirmek için uygun rolü seçin. Atama türü **devralınan** rolü abonelik yerine kaynak grubuna atandığından.

![Vurgulanan atama türüyle uygun rol atamalarını listesi](media/azure-pim-resource-rbac/my-roles-02.png)
