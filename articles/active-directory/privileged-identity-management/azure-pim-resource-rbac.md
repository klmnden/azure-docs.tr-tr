---
title: Azure kaynağı rolleri PIM'de olan Görünüm | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM içinde) olan Azure kaynak rolleri görüntüleyin.
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
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: ce7c96d92938c4e3b4cc0b53271df48350083754
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465240"
---
# <a name="view-who-has-azure-resource-roles-in-pim"></a>Azure kaynağı rolleri PIM'de olan görüntüle

Azure Active Directory Privileged Identity Management (PIM ile), yönetebilir, denetleyebilir ve kuruluşunuzda Azure kaynaklara erişimi izleyin. Bu abonelik, kaynak grupları ve bile sanal makineleri içerir. Azure rol tabanlı erişim denetimi (RBAC) işlevselliği yararlanan Azure portalındaki herhangi bir kaynağa güvenlik ve yaşam döngüsü yönetimi özellikleri Azure AD PIM'de yararlanabilirsiniz. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>Azure kaynakları için PIM kaynak yöneticileri yardımcı olur.

- Hangi kullanıcıların ve grupların yönettiğiniz Azure kaynakları için rolleri atanmış bakın
- İsteğe bağlı, "yalnızca zamanında" abonelik, kaynak grupları ve diğer kaynakları yönetmek için erişimi etkinleştir
- Yeni zaman sınırı atama ayarları ile otomatik olarak atanan kullanıcılar/gruplar kaynak erişim süre sonu
- Hızlı Görevler veya nöbet zamanlamalar için geçici kaynak erişim atama
- Herhangi bir yerleşik veya özel rolü şirket kaynak erişimi için multi-Factor Authentication yürürlüğe 
- Etkin bir kullanıcının oturumu sırasında kaynak bağıntılı erişim kaynak etkinliği hakkında raporlar alın
- Yeni kullanıcılar veya gruplar kaynak erişimi atandığında ve bunlar uygun atamalar'ı etkinleştirdiğinizde uyarılar alın

## <a name="view-activation-and-azure-resource-activity"></a>Etkinleştirme ve Azure kaynak etkinliği görüntüle

Belirli bir kullanıcı çeşitli kaynaklardaki sürdü hangi eylemleri görmek için ihtiyacınız olayda bir belirtilen etkinleştirme süresi (için uygun olan kullanıcılar) ile ilişkili Azure kaynak etkinliği gözden geçirebilirsiniz. Bir kullanıcı üye görünümünden veya belirli bir roldeki üyelerin listesi seçerek başlatın. Sonuç, tarihe göre Azure kaynaklarına kullanıcı eylemleri ve bu aynı süre boyunca yeni rol etkinleştirmeleri grafik bir görünümünü görüntüler.

![](media/azure-pim-resource-rbac/user-details.png)

Özel rol etkinleştirme seçerek rol etkinleştirme ayrıntıları ve bu kullanıcı etkin olduğu sırada gerçekleşen karşılık gelen Azure kaynak etkinliği gösterir.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="review-who-has-access-in-a-subscription"></a>Bir abonelikte erişimi olan gözden geçirme

Aboneliğinizdeki rol atamalarını gözden geçirmesini üyeler sekmesini seçin sol gezinti bölmesinden veya rolleri seçin ve üyeleri gözden geçirmek için belirli bir rol seçin. 

Gözden geçirme mevcut erişim gözden geçirmeleri görüntülemek ve yeni bir gözden geçirme oluşturmak için Ekle'yi seçin için Eylem çubuğu seçin.

![](media/azure-pim-resource-rbac/owner.png)

[Erişim gözden geçirmeleri hakkında daha fazla bilgi edinin](pim-how-to-perform-security-review.md)

>[!NOTE]
İncelemeler, şu anda yalnızca abonelik kaynak türleri için desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM Azure kaynak Rolleri Ata](pim-resource-roles-assign-roles.md)
- [Onaylayın veya reddedin PIM Azure kaynak rolleri için istekleri](pim-resource-roles-approval-workflow.md)
- [Azure'da yerleşik roller](../../role-based-access-control/built-in-roles.md)
