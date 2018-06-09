---
title: Hedef Privileged Identity Management ayarları özel rollere Azure kaynakları için kullandığınız | Microsoft Docs
description: Azure kaynaklarıyla PIM için özel roller kullanmayı açıklar.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: 03904990d54db0dd39ed7059f57a0a13efe0aaca
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233388"
---
# <a name="use-custom-roles-to-target-privileged-identity-management-settings"></a>Özel rollere hedef Privileged Identity Management ayarları kullanın

Diğerleri için büyük otonomisi sağlarken bazı bir rol, üyelerine katı ayrıcalıklı Kimlik Yönetimi (PIM) ayarları uygulamak gerekebilir. Kuruluşunuzun bir Azure aboneliği çalışacak bir uygulamanın geliştirilmesi yardımcı olmak için birkaç sözleşme ilişkilendirir anlaşır bir senaryo düşünün.

Bir kaynak yöneticisi olarak, onay gerektirmeden erişim hakkı çalışanların olmasını istiyorsunuz. Ancak, tüm sözleşme ilişkilendirilmiş bir kuruluş kaynaklarına erişim istediklerinde onaylanmalıdır.

Azure kaynak rolleri için hedeflenen PIM ayarları ayarlamak için sonraki bölümde anlatılan adımları izleyin.

## <a name="create-the-custom-role"></a>Özel rol oluşturun

Bir kaynak için özel bir rol oluşturmak için açıklanan adımları izleyin [Azure rol tabanlı erişim denetimi için özel roller oluşturmanızı](../role-based-access-control-custom-roles.md).

Özel rol oluşturduğunuzda kolayca çoğaltmak için hedeflenen hangi yerleşik rol anımsamasını sağlayabilirsiniz için açıklayıcı bir ad ekleyin.

> [!NOTE]
> Özel rol çoğaltmak istediğiniz yerleşik rol tekrarı olduğundan ve kapsamı yerleşik rol eşleştiğinden emin olun.

## <a name="apply-pim-settings"></a>PIM ayarlarını uygula

Rol Azure portalında kiracınızın oluşturulduktan sonra Git **Privileged Identity Management - Azure kaynaklarını** bölmesi. Rol uygulandığı kaynağı seçin.

!["Privileged Identity Management - Azure kaynaklarını" bölmesi](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[PIM rol ayarlarını yapılandırmak](pim-resource-roles-configure-role-settings.md) bu rolü üyelerine uygulamalısınız.

Son olarak, [Rolleri Ata](pim-resource-roles-assign-roles.md) bu ayarlarla hedeflemek istediğiniz üyeleri ayrı grubu için.

## <a name="next-steps"></a>Sonraki adımlar

[Gözden geçirme abonelik sahipleri ve erişim](pim-resource-roles-perform-access-review.md)
