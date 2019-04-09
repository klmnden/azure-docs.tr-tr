---
title: Şema dağıtımının aşamaları
description: Azure şema Hizmetleri dağıtımı sırasında adımlarını öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: d7000813b51fb9c9aae9a21cbded3ae0028e83f4
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59261431"
---
# <a name="stages-of-a-blueprint-deployment"></a>Şema dağıtımının aşamaları

Bir şema dağıtıldığında, bir dizi eylem blueprint'te tanımlanan kaynakları dağıtmak için Azure şemaları hizmet tarafından alınır. Bu makalede, her bir adımın ne içerir ilişkin ayrıntılar sağlanır.

Şema dağıtımı, bir abonelik için bir şema atayarak tetiklenir veya [mevcut bir atamanın güncelleştirilmesi](../how-to/update-existing-assignments.md). Dağıtım sırasında aşağıdaki üst düzey adımları şemaları alır:

> [!div class="checklist"]
> - Sahibi haklarına şemaları
> - Blueprint ataması nesnesi oluşturulur
> - İsteğe bağlı - şemaları oluşturur **sistem tarafından atanan** yönetilen kimlik
> - Blueprint yapıtları yönetilen kimlik dağıtır
> - Blueprint hizmet ve **sistem tarafından atanan** yönetilen kimlik hakları iptal

## <a name="blueprints-granted-owner-rights"></a>Sahibi haklarına şemaları

Blueprint Azure hizmet sorumlusu atanan abonelik ya da abonelikler için sahip hakları verilir. Verilen rol oluşturmak ve daha sonra iptal etmek planlar sağlayan [yönetilen sistem tarafından atanan kimliği](../../../active-directory/managed-identities-azure-resources/overview.md).

Hakları atama portal üzerinden yapılır, otomatik olarak verilir. Atama hakları olması gerekir verme REST API aracılığıyla gerçekleştirilir, ancak ayrı bir API ile çağırın. Azure şema uygulama kimliği `f71766dc-90d9-4b7d-bd9d-4499c4331c3f`, ancak Kiracı tarafından hizmet sorumlusu değişir. Kullanım [Azure Active Directory Graph API'si](../../../active-directory/develop/active-directory-graph-api.md) ve REST uç noktasını [servicePrincipals](/graph/api/resources/serviceprincipal) hizmet sorumlusu alınamıyor. Ardından, Azure şemaları verin _sahibi_ rolü aracılığıyla [portalı](../../../role-based-access-control/role-assignments-portal.md), [Azure CLI](../../../role-based-access-control/role-assignments-cli.md), [Azure PowerShell](../../../role-based-access-control/role-assignments-powershell.md), [REST API](../../../role-based-access-control/role-assignments-rest.md), veya bir [Resource Manager şablonu](../../../role-based-access-control/role-assignments-template.md).

Blueprint hizmet kaynakları doğrudan dağıtmaz.

## <a name="the-blueprint-assignment-object-is-created"></a>Blueprint ataması nesnesi oluşturulur

Bir kullanıcı, Grup veya hizmet sorumlusu bir şema için bir abonelik atar. Burada şema atanan abonelik düzeyinde atama nesnesi yok. Dağıtım tarafından oluşturulan kaynakları dağıtma varlık bağlamında yapılır değildir.

Şema atamasını türü oluşturulurken [yönetilen kimliği](../../../active-directory/managed-identities-azure-resources/overview.md) seçilir. Varsayılan bir **sistem tarafından atanan** yönetilen kimliği. A **kullanıcı tarafından atanan** yönetilen kimlik seçilebilir. Kullanırken bir **kullanıcı tarafından atanan** yönetilen kimliği, tanımlanan ve şema atamasını oluşturulmadan önce izni gerekir.

## <a name="optional---blueprints-creates-system-assigned-managed-identity"></a>Yönetilen kimlik sistem tarafından atanan isteğe bağlı - şemaları oluşturur

Zaman [yönetilen sistem tarafından atanan kimliği](../../../active-directory/managed-identities-azure-resources/overview.md) seçili Blueprint ataması sırasında kimliği oluşturur ve yönetilen kimlik verir [sahibi](../../../role-based-access-control/built-in-roles.md#owner) rol. Varsa bir [mevcut atamanın yükseltildiğinde](../how-to/update-existing-assignments.md), daha önce oluşturulan yönetilen kimlik şemaları kullanır.

Blueprint ataması ile ilgili bir yönetilen kimlik blueprint'te tanımlanan kaynakları yeniden dağıtın veya dağıtmak için kullanılır. Bu tasarım, yanlışlıkla birbiriyle engellemesini atamaları önler.
Bu tasarım da destekler [kaynak kilitleme](./resource-locking.md) tarafından dağıtılan her blueprint kaynak güvenliğini denetleme özelliği.

## <a name="the-managed-identity-deploys-blueprint-artifacts"></a>Blueprint yapıtları yönetilen kimlik dağıtır

Yönetilen kimlik ardından içinde tanımlanan şema içindeki tüm öğelerle Resource Manager dağıtımları tetikleyen [sıralama sipariş](./sequencing-order.md). Sipariş yapıtları diğer yapıları üzerine bağımlı doğru sırayla dağıtılır emin olmak için ayarlanabilir.

Bir dağıtım tarafından bir erişim hatası genellikle yönetilen kimlik için verilen erişim düzeyini sonucudur. Şemaları hizmeti güvenlik yaşam döngüsünü yöneten **sistem tarafından atanan** yönetilen kimliği. Ancak, kullanıcı hakları ve yaşam döngüsünü yönetmek için sorumlu bir **kullanıcı tarafından atanan** yönetilen kimliği.

## <a name="blueprint-service-and-system-assigned-managed-identity-rights-are-revoked"></a>Blueprint hizmeti ve yönetilen kimlik sistemi atanmış hakları iptal edildi

Dağıtım tamamlandığında, şemalar iptal eder haklarını **sistem tarafından atanan** abonelikten yönetilen kimliği. Ardından, şemalar hizmet abonelikten kendi rights iptal eder. Hakları kaldırma şemaları, bir abonelikte bir kalıcı sahip hale gelmesini engeller.

## <a name="next-steps"></a>Sonraki adımlar

- [Statik ve dinamik parametrelerin](parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.