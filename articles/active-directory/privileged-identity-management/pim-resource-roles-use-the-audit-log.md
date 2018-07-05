---
title: Privileged Identity Management'ı kullanarak Azure kaynakları için kaynak rolleri denetim | Microsoft Docs
description: Tüm rol etkinliği için bir görünümünü elde açıklanmaktadır belirli bir kaynak.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2740297337e1de0d041a80c7860324175413c833
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447722"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynakları için kaynak rolleri denetleme 

Kaynak Denetim tüm rol etkinliklerin kaynağın görünüm sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgileri filtreleyebilirsiniz.
![Filtre bilgileri](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Kaynak Denetim, ayrıca bir kullanıcının Etkinlik Ayrıntısı hızlı erişim sağlar. Altında **denetim türü**seçin **etkinleştirme**. Seçin **(etkinlik)** Azure kaynaklarında kullanıcının eylemleri görmek için.
![Etkinlik Ayrıntısı](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Daha fazla etkinlik ayrıntısı](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Denetimim

Denetimim, bir kullanıcının kişisel rol etkinlik bir görünümünü sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgileri filtreleyebilirsiniz.
![Kişisel rol etkinliği](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Etkinleştirme ve Azure kaynak etkinliği görüntüle

Belirli bir kullanıcı çeşitli kaynakları sürdü hangi eylemleri görmek için verilen etkinleştirme nokta ile ilişkili Azure kaynak etkinliği gözden geçirebilirsiniz. Bir kullanıcıdan seçerek Başlat **üyeleri** görünümü veya belirli bir roldeki üyelerin listesi. Sonucu, Azure kaynaklarında tarihe göre kullanıcının eylemleri grafik bir görünümünü gösterir. Ayrıca, aynı süre boyunca yeni rol etkinleştirmeleri gösterir.

![Kullanıcı ayrıntıları](media/azure-pim-resource-rbac/rbac-user-details.png)

Belirli rol etkinleştirme seçerek, bu kullanıcı etkin olduğu sırada gerçekleşen karşılık gelen Azure kaynak etkinliği ve rol etkinleştirme ayrıntıları gösterir.

![Rol etkinleştirme'yi seçin](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

