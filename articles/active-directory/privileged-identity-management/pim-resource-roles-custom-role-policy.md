---
title: Azure kaynakları - özel rollere hedef PIM ayarları kullanmak için Privileged Identity Management | Microsoft Docs
description: Özel roller PIM içinde Azure kaynaklarının için nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/30/2018
ms.author: billmath
ms.openlocfilehash: 6336d99df1bbdd71c66a9757af1d9fb356a91bf6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-custom-roles-to-target-pim-settings"></a>Özel rollere hedef PIM ayarları kullanın

Diğerleri için büyük otonomisi sağlarken bazı bir rol, üyelerine katı PIM ayarları uygulamak gerekli olabilir. Burada, kuruluşunuzun sevral sözleşme ilişkilendirilmiş bir Azure aboneliği içinde çalışacak bir uygulamanın geliştirilmesi yardımcı olmak için anlaşır bir senaryo düşünün. 

Bir kaynak yöneticisi olarak onay gerekli uygun erişim sağlamak için kuruluşunuzun çalışanları istediğiniz, ancak etkinleştirme istediklerinde, tüm sözleşme ilişkilendirilmiş onay arama gerekir. Anahat işlemi Azure kaynak rolleri için hedeflenen PIM ayarlarını etkinleştirmek için aşağıdaki adımları izleyin.

## <a name="create-the-custom-role"></a>Özel rol oluşturun

[Bir kaynak için özel bir rol oluşturmak için bu kılavuzu kullanın](../../role-based-access-control/custom-roles.md).

Çoğaltmak için hedeflenen hangi yerleşik rol kolayca anımsamasını sağlayabilirsiniz için açıklayıcı bir ad içerir.

>[!NOTE]
>Özel rol amaçladığınız rol yinelemesi ve yerleşik rol kapsamı eşleştiğinden emin olun.

## <a name="apply-pim-settings"></a>PIM ayarlarını uygula

Rol kiracınızda oluşturulduktan sonra PIM için gidin ve rol kapsamına kaynağı seçin.

![](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[PIM rol ayarlarını yapılandırmak](pim-resource-roles-configure-role-settings.md) bu üyelerine uygulamalıdır

Son olarak, [Rolleri Ata](pim-resource-roles-assign-roles.md) bu ayarlarla hedeflemek istediğiniz üyeleri ayrı grubu için.

## <a name="next-steps"></a>Sonraki adımlar

[Abonelik sahipleri gözden geçirin](pim-resource-roles-perform-access-review.md)
