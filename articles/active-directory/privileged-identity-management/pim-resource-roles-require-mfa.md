---
title: Privileged Identity Management Azure kaynaklarını - MFA için | Microsoft Docs
description: Bu belge, PIM kaynaklar için çok faktörlü kimlik doğrulamasını etkinleştirmek açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 8d1c05e7f61ed76c47613bfab7bb8afd9b66cbe7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---mfa"></a>Privileged Identity Management - kaynak rolleri - MFA

Kaynak yöneticileri ve kimlik yöneticileri kritik Azure altyapısı zaman sınırlı üyelik ve tam zamanı erişimi ile korumak Azure kaynak rolleri için PIM sağlar. Ayrıca, PIM iki farklı senaryolar için isteğe bağlı zorlama Azure çok faktörlü kimlik doğrulama (MFA) sağlar.

## <a name="require-mfa-to-activate"></a>Etkinleştirme için MFA isteyin.

Kaynak yöneticileri, bunlar etkinleştirmeden önce uygun bir rolün üyeleri Azure MFA başarılı gerektirebilir. Bu işlem, kullanıcı isteyen etkinleştirme kim makul kesin olarak oldukları söyleyin sağlar. Kullanıcı hesabının tehlikede olduğunda bu seçeneği zorlamayı durumlarda kritik kaynaklara korur. 

Bu gereksinim zorlamak için yönetilen kaynaklar listesinden bir kaynak seçin. Gelen [genel bakış Panosu'na](pim-resource-roles-overview-dashboards.md), ekranın sağ rollerinin listesinden bir rol seçin.

Ayrıca, sol gezinti menüsünde sekmeleri rol ayarlarını rollerden ya da "" veya "Rol ayarları" elde edebilirsiniz.

>[!Note]
>Sol gezinti menüsünde Seçenekleri gri bir başlık görürseniz sayfanın üst kısmındaki "Etkinleştirilebilmesi için uygun roller sahip" belirten etkin bir yönetici değilseniz ve gerekir [etkinleştirme](pim-resource-roles-activate-your-roles.md) devam etmeden önce.

![](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Bir rolün üyelik görüntüleme, "rol ayarı ayrıntısı" açmak için ekranın üstünde çubuğundan "Rol ayarları" seçin.

Tıklatın **Düzenle** rol ayarlarını değiştirmek için üstündeki düğmesi.

Bölümünde altında **etkinleştirme**, onay kutusuna tıklayın **etkinleştirmek için multi-Factor Authentication iste**, Kaydet'i tıklatın.

![](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-mfa-on-assignment"></a>MFA atamada gerektirir

Bazı durumlarda, bir kaynak yöneticisi üyesi kısa bir süre (örneğin bir gün) rol atama ve etkinleştirme isteği atanan üyeleri gerekmez isteyebilirsiniz. Üye kendi rol ataması kullandığında zaten atanmış olan andan rolündeki etkin olduğu Bu senaryoda, MFA PIM zorunlu kılamaz.

Atama gerçekleşmesine kaynak yöneticisi kim olduklarını söyleyin sağlamak için MFA atamada zorunlu kılabilir.

Aynı rol ayarı ayrıntıları ekranından, "atama çok faktörlü kimlik doğrulaması gerektiren için" onay kutusunu işaretleyin.

![](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

[Etkinleştirmek için onay iste](pim-resource-roles-approval-workflow.md)

[Denetim günlüğü nasıl kullanma](pim-resource-roles-use-the-audit-log.md)



