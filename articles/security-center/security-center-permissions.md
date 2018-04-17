---
title: Azure Güvenlik Merkezi'nde izinleri | Microsoft Docs
description: Bu makalede, rol tabanlı erişim denetimini Azure Güvenlik Merkezi kullanıcılara izinler atamak için nasıl kullandığını açıklar ve her rol için izin verilen eylemleri tanımlar.
services: security-center
cloud: na
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: terrylan
ms.openlocfilehash: f85f49bd54eacbca67143b35eaf555cfb744a41d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="permissions-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde izinleri

Azure Güvenlik Merkezi kullanan [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md), sağlayan [yerleşik roller](../role-based-access-control/built-in-roles.md) kullanıcıları, grupları ve Azure Hizmetleri için atanabilir.

Güvenlik Merkezi güvenlik sorunları ve güvenlik açıklarını tanımlamak için kaynaklarınızı yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca kaynak sahibi, katkıda bulunan veya okuyucu rolü abonelik veya kaynak ait olduğu kaynak grubu için atandığında ilgili bilgileri görebilirsiniz.

Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

* **Güvenlik okuyucu**: Bu role ait olan bir kullanıcı haklarını Güvenlik Merkezi'ne görüntüleme sahiptir. Kullanıcı öneriler, uyarılar, bir güvenlik ilkesi ve güvenlik durumları görüntüleyebilir ancak değişiklik yapamazsınız.
* **Güvenlik Yöneticisi**: Bu role ait bir kullanıcı güvenlik okuyucu aynı haklarına sahiptir ve ayrıca güvenlik ilkesini güncelleştirin ve uyarısı ve öneri yok sayın.

> [!NOTE]
> Güvenlik rolleri, güvenlik okuyucu ve Güvenlik Yöneticisi, yalnızca Güvenlik Merkezi'nde erişebilirsiniz. Güvenlik rolleri, Azure depolama, Web ve mobil veya nesnelerin interneti gibi diğer hizmet alanlara erişimi yoktur.
>
>

## <a name="roles-and-allowed-actions"></a>Rol ve izin verilen eylemleri

Aşağıdaki tabloda rollerini görüntüler ve Güvenlik Merkezi'nde Eylemler izin. Bir X eylemi bu rol için izin verildiğini gösterir.

| Rol | Güvenlik ilkesini Düzenle | Bir kaynak için güvenlik önerilerini uygulama | Uyarılar ve öneriler kapatın | Uyarıları görüntüle ve öneriler |
|:--- |:---:|:---:|:---:|:---:|
| Abonelik sahibi | X | X | X | X |
| Abonelik katkıda bulunan | X | X | X | X |
| Kaynak grubunun sahibi | -- | X | -- | X |
| Kaynak grubu katkıda bulunan | -- | X | -- | X |
| Okuyucu | -- | -- | -- | X |
| Güvenlik Yöneticisi | X | -- | X | X |
| Güvenlik Okuyucu | -- | -- | -- | X |

> [!NOTE]
> Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, yalnızca bir kaynak güvenlik durumu hakkındaki bilgileri görüntüleyebilir, ancak önerileri uygulamak veya ilkeleri düzenleme gibi bir eylemde bulunmamasını gereken kullanıcılar için okuyucu rolüne atayın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede RBAC Güvenlik Merkezi kullanıcılara izinler atamak için nasıl kullandığı açıklanmıştır ve her rol için izin verilen eylemleri tanımlanır. Aboneliğinizi güvenlik durumunu izlemek için gerekli rol atamalarını ile tanıdık, güvenlik ilkeleri, düzenleme ve önerileri geçerlidir, öğrenme nasıl yapılır:

- [Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md)
- [Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md)
- [Azure kaynaklarınızın güvenlik durumunu izleme](security-center-monitoring.md)
- [Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
- [İş ortağı güvenlik çözümlerini izleme](security-center-partner-solutions.md)
