---
title: Privileged Identity Management'ı kullanarak Azure kaynaklarında Azure çok faktörlü kimlik doğrulamasını zorunlu | Microsoft Docs
description: Bu belge, PIM kaynaklar için çok faktörlü kimlik doğrulamasını etkinleştirmek açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: b4f3fb0b25787711294d09faf65c2856d57709bc
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234422"
---
# <a name="enforce-azure-multi-factor-authentication-in-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure çok faktörlü kimlik doğrulaması Azure kaynaklarında zorla

Kaynak yöneticileri ve kimlik yöneticileri kritik Azure altyapısı zaman sınırlı üyelik ve tam zamanı erişimi ile korumak Azure kaynak rolleri için ayrıcalıklı Kimlik Yönetimi (PIM) sağlar. Ayrıca, PIM iki farklı senaryolar için Azure multi-Factor Authentication isteğe bağlı zorunluluğu sağlar.

## <a name="require-multi-factor-authentication-to-activate"></a>Multi-Factor Authentication'ı etkinleştirmek gerekli

Kaynak yöneticileri etkinleştirebilmek için önce Azure çok faktörlü kimlik doğrulama çalıştırmak uygun rolü üyelerinin gerektirebilir. Bu işlem, etkinleştirme kim makul kesin olarak oldukları söyleyin isteyen kullanıcının sağlar. Kullanıcı hesabının tehlikede, bu seçenek zorlamayı durumlarda kritik kaynaklara korur. 

Bu gereksinim zorlamak için yönetilen kaynaklar listesinden bir kaynak seçin. Gelen [genel bakış Panosu'na](pim-resource-roles-overview-dashboards.md), ekranın sağ alt bölümünde rollerinde listesinden bir rol seçin.

Ayrıca, rol ayarlarını herhangi birinden alabileceğiniz **rolleri** veya **rol ayarlarını** sol bölmedeki sekmeleri.

>[!Note]
>Sol bölmede seçenekler gri olması ve sayfanın en üstündeki "Etkinleştirilebilmesi için uygun rollere sahip" bildiren bir başlık görüyorsanız, etkin bir yönetici değilsiniz. Bu, gerektiği anlamına gelir [etkinleştirme](pim-resource-roles-activate-your-roles.md) devam etmeden önce.

!["Rol" ve "Rol ayarları" sekmeleri ](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Bir rolün üyeliğini görüntülemek için seçin **rol ayarlarını** çubuğundan açmak için ekranın üstünde **rol ayarı ayrıntısı**.

Rol ayarlarını değiştirmek için seçin **Düzenle** üstündeki düğmesi.

Bölümünde altında **etkinleştirme**, onay kutusunu işaretleyin **gerektiren çok faktörlü kimlik doğrulamasını etkinleştirme**. Daha sonra **Kaydet**’e tıklayın.

![Etkinleştirme yapılırken Multi-Factor Authentication'ı gerekli kılın](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-multi-factor-authentication-on-assignment"></a>Atama üzerinde çok faktörlü kimlik doğrulaması gerektirir

Bazı durumlarda, bir kaynak yöneticisi (örneğin bir gün) kısa bir süre için role üye atama isteyebilirsiniz. Bu durumda, etkinleştirme isteği atanan üyeleri gerek yoktur. Üye kendi rol ataması kullandığında zaten atanmış olan andan rolündeki etkin olduğu Bu senaryoda, çok faktörlü kimlik doğrulaması PIM zorunlu kılamaz.

Atama gerçekleşmesine kaynak yöneticisi kim olduklarını söyleyin olduğundan emin olmak için atamada çok faktörlü kimlik doğrulamasını zorunlu kılabilir.

Aynı rol ayarı ayrıntıları ekranından, kutuyu **doğrudan atamada çok faktörlü kimlik doğrulaması iste**.

![Doğrudan atamada çok faktörlü kimlik doğrulaması gerektirir](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

[Etkinleştirmek için onay iste](pim-resource-roles-approval-workflow.md)

[Denetim günlüğü nasıl kullanma](pim-resource-roles-use-the-audit-log.md)



