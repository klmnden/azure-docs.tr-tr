---
title: Privileged Identity Management'ı kullanarak Azure kaynakları için rol etkinleştirme | Microsoft Docs
description: PIM rolleri etkinleştirmeyi açıklar.
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
ms.openlocfilehash: 45a2747a60df4d91c2fe1c5247e1d4ac82ff819f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39617158"
---
# <a name="activate-roles-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynakları için rollerini etkinleştir
Privileged Identity Management (PIM) Azure kaynakları için rol etkinleştirme içinde yeni bir deneyim sağlanıyor. Uygun Rol üyeleri için bir gelecek tarih ve saat etkinleştirme zamanlayabilirsiniz. Bunlar, belirli etkinleştirme süresi üst sınırı (yönetici tarafından yapılandırılır) içinde de seçebilirsiniz. Daha fazla bilgi için [etkinleştirmek veya Azure AD Privileged Identity Management içinde rol devre dışı bırakma](pim-how-to-activate-role.md).

## <a name="activate-roles"></a>Rollerini etkinleştir
Gözat **rollerim** sol bölmedeki bölümü. Seçin **etkinleştirme** etkinleştirmek istediğiniz rolü için.

!["Uygun roller" sekmesinde bulunan "Rollerim" bölmesi.](media/azure-pim-resource-rbac/rbac-roles.png)

Gelen **etkinleştirmeleri** menüsünde, başlangıç tarihini girin ve rol etkinleştirme süresi. İsteğe bağlı olarak, etkinleştirme süresini azaltmak (Rol etkin olduğu süre uzunluğu) ve gerekirse bir gerekçe girin. Ardından, **etkinleştirme**.

Başlangıç tarihi ve saati değiştirilmez, rol saniyeler içinde etkinleştirilir. İçinde **rollerim** bölmesinde, bir başlık iletisi gösteren bir rol etkinleştirme için sıraya alındı. Bu ileti temizlemek için Yenile düğmesini seçin.

![Bir başlık iletisi ve bekleyen bir onay hakkında bir bildirim ile "Rollerim" bölmesi](media/azure-pim-resource-rbac/rbac-activate-notification.png)

Bekleyen istek etkinleştirme gelecekteki bir tarih ve saat için planlandıysa görünür **bekleyen istekler** sol bölmenin sekmesi. Rol etkinleştirme artık gerekli değilse, istek seçerek iptal edebilirsiniz **iptal** düğmesi.

![Bekleyen istekler "İptal" düğmelerle listesi](media/azure-pim-resource-rbac/rbac-activate-pending.png)


## <a name="apply-just-enough-administration-practices"></a>Yeterli yönetim uygulamaları Uygula

Yeterli yönetim (JEA) en iyi uygulamalar, kaynak rol atamaları kullanarak Azure kaynakları için PIM ile basit bir işlemdir. Kullanıcı ve Grup üyeleri atamaları Azure Abonelikleriniz veya kaynak grupları ile birlikte, mevcut rol ataması azaltılmış bir kapsamda etkinleştirebilirsiniz. 

Arama sayfasından yönetmeniz gereken alt kaynak bulun.

![Bir kaynak seçmeyi](media/azure-pim-resource-rbac/azure-resources-02.png)

Seçin **rollerim** sol bölmeden ve etkinleştirmek için uygun rolü seçin. Atama türü **devralınan** rol abonelik yerine kaynak grubuna atandığından.

![Uygun rol atamaları, vurgulanan atama türü listesi](media/azure-pim-resource-rbac/my-roles-02.png)
