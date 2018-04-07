---
title: Kaynak Denetim Azure kaynakları için - Privileged Identity Management | Microsoft Docs
description: Tüm rol etkinliği için bir görünümünü elde açıklanmaktadır belirli bir kaynak.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: b8fa7d5600c0de8a3319ea4de785281372959937
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---audit"></a>Privileged Identity Management - kaynak rolleri - denetim

Kaynak Denetim tüm rol etkinlik kaynak için bir görünümünü sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgi filtreleyebilirsiniz.
![](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Kaynak Denetim ayrıca kullanıcının Etkinlik ayrıntıları görüntülemek için hızlı erişim sağlar. "Denetim türü" altında "Etkinleştir" seçin. "(Faaliyete)" Azure kaynaklarına kullanıcının eylemleri görmek için tıklatın.
![](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Denetimim

My denetim, kullanıcının kişisel rol etkinliği bir görünümünü sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgi filtreleyebilirsiniz.
![](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Etkinleştirme ve Azure kaynak etkinliği görüntüleme

Belirli bir kullanıcı çeşitli kaynaklara sürdü hangi eylemleri görmek gerekmesi durumunda belirtilen etkinleştirme süresi (uygun kullanıcılar için) ile ilişkili Azure kaynak etkinliği gözden geçirebilirsiniz. Bir kullanıcı üyeleri görünümünden veya belirli bir roldeki üyelerin listesi seçerek başlatın. Sonuç tarihe göre Azure kaynaklarına kullanıcının eylemleri ve aynı bu süre boyunca yeni rol etkinleştirmeleri grafik bir görünümünü görüntüler.

![](media/azure-pim-resource-rbac/rbac-user-details.png)

Belirli bir rol etkinleştirme seçerek rol etkinleştirme ayrıntıları ve bu kullanıcının etkinken oluşan karşılık gelen Azure kaynak etkinliği gösterir.

![](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

