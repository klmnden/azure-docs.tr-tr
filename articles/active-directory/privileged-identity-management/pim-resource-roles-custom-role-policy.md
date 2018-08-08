---
title: Azure kaynakları için özel roller için hedef Privileged Identity Management ayarlarını kullanma | Microsoft Docs
description: PIM ile Azure kaynakları için özel roller kullanmayı açıklar.
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
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: 03091b50eb4fb368ae0277344317b261991c4bd1
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622696"
---
# <a name="use-custom-roles-to-target-privileged-identity-management-settings"></a>Özel roller için hedef Privileged Identity Management ayarları kullanın

Başkaları için büyük özerkliği sağlamanın bir rolün bazı üyeler için katı Privileged Identity Management (PIM) ayarlarını uygulamak gerekebilir. Kuruluşunuzun bir Azure aboneliğinde çalıştırılan bir uygulamanın geliştirilmesi konusunda yardımcı olmak için birkaç sözleşme ilişkilendirir işe alma bir senaryo düşünün.

Bir kaynak yöneticisi olarak, çalışanlarınızın onay gerek kalmadan erişim hakkı olmasını istersiniz. Ancak, kuruluşunuzun kaynaklarına erişimi istediklerinde, tüm sözleşme ilişkilendirir onaylanmalıdır.

Azure kaynak rolleri için hedeflenen PIM ayarları ayarlamak için sonraki bölümde verilen adımları izleyin.

## <a name="create-the-custom-role"></a>Özel rol oluşturma

Bir kaynak için özel bir rol oluşturmak için açıklanan adımları izleyin. [Azure rol tabanlı erişim denetimi için özel roller oluşturma](../role-based-access-control-custom-roles.md).

Özel rol oluşturduğunuzda, yinelenen yönelik yerleşik hangi rolü kolayca hatırlayabileceğiniz için açıklayıcı bir ad ekleyin.

> [!NOTE]
> Özel rol çoğaltmak istediğiniz yerleşik rolü kopyası olduğunu ve kapsamı yerleşik rolü eşleştiğinden emin olun.

## <a name="apply-pim-settings"></a>PIM ayarları uygula

Kiracınızda Azure portalında rolü oluşturduktan sonra Git **Privileged Identity Management - Azure kaynaklarını** bölmesi. Rolün için geçerli bir kaynak seçin.

!["Privileged Identity Management - Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[PIM rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md) bu rolü üyelerine uygulamalıdır.

Son olarak, [Rolleri Ata](pim-resource-roles-assign-roles.md) bu ayarlarla hedeflemek istediğiniz üyeleri farklı grubu için.

## <a name="next-steps"></a>Sonraki adımlar

[Gözden geçirme abonelik sahipleri ve erişim](pim-resource-roles-perform-access-review.md)
