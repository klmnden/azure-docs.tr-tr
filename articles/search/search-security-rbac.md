---
title: Portal - Azure Search RBAC rolleri için Azure yönetim erişimi ayarlayın
description: Rol tabanlı yönetim denetimi (RBAC) ve Azure arama yönetimi için yönetim görevleri için temsilci seçme denetlemek için Azure portalında.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 38b8e8a0e413f367d34a4ccf5dbd87817891b8ea
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53313029"
---
# <a name="set-rbac-roles-for-administrative-access"></a>Yönetici erişimi için RBAC rollerini ayarlama

Azure sağlayan bir [genel rol tabanlı yetkilendirme modeli](../role-based-access-control/role-assignments-portal.md) tüm hizmetler için portal veya Resource Manager API'leri aracılığıyla yönetilen. Sahip, katkıda bulunan ve okuyucu rolleri düzeyini belirlemek *Hizmet Yönetim* Active Directory Kullanıcıları, grupları ve her role atanmış güvenlik sorumluları için. 

> [!Note]
> Dizin bölümlerini veya bir alt belgelerin güvenliğini sağlamak için rol tabanlı erişim denetim yoktur. Kimlik tabanlı arama sonuçları üzerinden erişim için istek sahibine erişim olmamalıdır belgelerin kaldırılması kimlik tarafından sonuçları kırpmak için güvenlik filtreler oluşturabilirsiniz. Daha fazla bilgi için [güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve [Active Directory ile güvenli](search-security-trimming-for-azure-search-with-aad.md).

## <a name="management-tasks-by-role"></a>Rol yönetim görevleri

Azure arama için rolleri aşağıdaki yönetim görevlerini desteklemek izin düzeyleri ile ilişkilidir:

| Rol | Görev |
| --- | --- |
| Sahip |Oluşturma veya hizmet veya API anahtarları, dizin, dizin oluşturucular, Dizin Oluşturucu veri kaynakları ve dizin oluşturucu zamanlamaları ekleme gibi herhangi bir nesne silme.<p>Boyut sayısı ve depolama da dahil olmak üzere, hizmet durumunu görüntüleyin.<p>Ekleyin veya rol üyeliğini (yalnızca bir sahip rolü üyeliği yönetebilirsiniz) silin.<p>Abonelik yöneticileri ve hizmet sahipleri otomatik üyelik sahipleri rolüne sahiptir. |
| Katılımcı |Aynı düzeyde erişime sahip RBAC rolü yönetim eksi. Örneğin, bir katkıda bulunan oluşturma veya silme nesneleri görüntülemek veya yeniden [api anahtarlarını](search-security-api-keys.md), rol üyeliklerini değiştiremezsiniz ancak. |
| [Arama hizmeti Katılımcısı yerleşik rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | Katkıda bulunan rolü ile eşdeğerdir. |
| Okuyucu |Görünüm hizmet temel bileşenleri ve ölçümleri. Bu rolün üyeleri, dizin, dizin oluşturucu, veri kaynağı veya anahtar bilgileri görüntüleyemezsiniz.  |

Rolleri, hizmet uç noktasına erişim hakları vermeyin. Arama hizmeti, dizin yönetimi ve dizin oluşturma işlemi sorgular gibi veriler üzerinde işlem arama, api anahtarlarını, olmayan roller denetlenir. Daha fazla bilgi için [API anahtarları Yönet](search-security-api-keys.md).

## <a name="see-also"></a>Ayrıca bkz.

+ [PowerShell’i kullanarak yönetme](search-manage-powershell.md) 
+ [Performans ve iyileştirme Azure Search'te](search-performance-optimization.md)
+ [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](../role-based-access-control/overview.md).