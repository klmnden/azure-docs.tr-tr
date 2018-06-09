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
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 211b8c69a1462f7efdcb4002269d96d1d5cf2ae6
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233793"
---
# <a name="audit-resource-roles-for-azure-resources-by-using-privileged-identity-management"></a>Privileged Identity Management'ı kullanarak Azure kaynakları için kaynak rolleri denetleme 

Kaynak Denetim tüm rol etkinlik kaynak için bir görünümünü sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgi filtreleyebilirsiniz.
![Filtre bilgileri](media/azure-pim-resource-rbac/rbac-resource-audit.png)

Kaynak Denetim ayrıca kullanıcının Etkinlik Ayrıntısı hızlı erişim sağlar. Altında **denetim türü**seçin **etkinleştirme**. Seçin **(etkinlik)** kullanıcının Eylemler Azure kaynaklarını görmeyi.
![Etkinlik Ayrıntısı](media/azure-pim-resource-rbac/rbac-audit-activity.png)

![Daha fazla etkinlik ayrıntısı](media/azure-pim-resource-rbac/rbac-audit-activity-details.png)

# <a name="my-audit"></a>Denetimim

My denetim, kullanıcının kişisel rol etkinliği bir görünümünü sağlar. Önceden tanımlanmış tarih veya özel aralığı kullanarak bilgi filtreleyebilirsiniz.
![Kişisel rol etkinliği](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="view-activation-and-azure-resource-activity"></a>Etkinleştirme ve Azure kaynak etkinliği görüntüleme

Belirli bir kullanıcı çeşitli kaynaklarında sürdü hangi eylemleri görmek için belirtilen etkinleştirme süresi ile ilişkili Azure kaynak etkinliği gözden geçirebilirsiniz. Başlangıç bir kullanıcıdan seçerek **üyeleri** görünüm veya belirli bir roldeki üyelerin listesi. Sonuç kullanıcının Eylemler grafik bir görünümünü Azure kaynaklarında tarihe göre görüntüler. Ayrıca yeni rol etkinleştirmeleri aynı bu süre boyunca gösterir.

![Kullanıcı ayrıntıları](media/azure-pim-resource-rbac/rbac-user-details.png)

Belirli bir rol etkinleştirme seçmek, rol etkinleştirme ayrıntıları ve bu kullanıcının etkinken oluşan karşılık gelen Azure kaynak etkinliği gösterir.

![Rol etkinleştirme seçin](media/azure-pim-resource-rbac/rbac-user-resource-activity.png)

