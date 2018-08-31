---
title: Azure kaynak rolleri için Denetim geçmişi PIM'de görüntüleme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM içinde) Azure kaynak rolleri için denetim geçmişini görüntülemeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: c0536423e9640f78149b612ec66b0a07cdcf24bb
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189320"
---
# <a name="view-audit-history-for-azure-resource-roles-in-pim"></a>PIM Azure kaynak rolleri için denetim geçmişini görüntüleme

Kaynak Denetim tüm rol etkinliklerin kaynağın görünüm sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgileri filtreleyebilirsiniz.
![Filtre bilgileri](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Kaynak Denetim, ayrıca bir kullanıcının Etkinlik Ayrıntısı hızlı erişim sağlar. Altında **denetim türü**seçin **etkinleştirme**. Seçin **(etkinlik)** Azure kaynaklarında kullanıcının eylemleri görmek için.
![Etkinlik Ayrıntısı](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Daha fazla etkinlik ayrıntısı](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

## <a name="my-audit"></a>Denetimim

Denetimim, bir kullanıcının kişisel rol etkinlik bir görünümünü sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgileri filtreleyebilirsiniz.
![Kişisel rol etkinliği](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Etkinleştirme ve Azure kaynak etkinliği görüntüle

Belirli bir kullanıcı çeşitli kaynakları sürdü hangi eylemleri görmek için verilen etkinleştirme nokta ile ilişkili Azure kaynak etkinliği gözden geçirebilirsiniz. Bir kullanıcıdan seçerek Başlat **üyeleri** görünümü veya belirli bir roldeki üyelerin listesi. Sonucu, Azure kaynaklarında tarihe göre kullanıcının eylemleri grafik bir görünümünü gösterir. Ayrıca, aynı süre boyunca yeni rol etkinleştirmeleri gösterir.

![Kullanıcı ayrıntıları](media/azure-pim-resource-rbac/rbac-user-details.png)

Belirli rol etkinleştirme seçerek, bu kullanıcı etkin olduğu sırada gerçekleşen karşılık gelen Azure kaynak etkinliği ve rol etkinleştirme ayrıntıları gösterir.

![Rol etkinleştirme'yi seçin](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD dizin rollerini PIM denetim geçmişini görüntüle](pim-how-to-use-audit-log.md)
