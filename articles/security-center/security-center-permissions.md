---
title: Azure Güvenlik Merkezi'nde izinler | Microsoft Docs
description: Bu makalede rol tabanlı erişim denetimi Azure Güvenlik Merkezi kullanıcılara izinler atamak için nasıl kullandığını açıklar ve her rol için izin verilen eylemleri tanımlar.
services: security-center
cloud: na
documentationcenter: na
author: rkarlin
manager: barbkess
ms.assetid: ''
ms.service: security-center
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: aa6c154e14fbf5291e2493f4f27b9d4501ba18f4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60905628"
---
# <a name="permissions-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde izinler

Azure Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanır.

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, yalnızca bir kaynağa sahip, katkıda bulunan veya okuyucu rolü bir kaynağın ait olduğu kaynak grubu veya abonelik atandığında ilgili bilgiler görürsünüz.

Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

* **Güvenlik okuyucusu**: Bu role ait bir kullanıcı, Güvenlik Merkezi hakları görüntüleme sahiptir. Kullanıcı, öneriler, uyarılar, bir güvenlik ilkesi ve güvenlik durumları görüntüleyebilir ancak değişiklik yapamaz.
* **Güvenlik Yöneticisi**: Bu role ait bir kullanıcı güvenlik okuyucu aynı haklarına sahiptir ve güvenlik ilkesini güncelleştirme da uyarıları ve öneriler yok sayın.

> [!NOTE]
> Güvenlik rollerini, güvenlik okuyucu ve Güvenlik Yöneticisi, yalnızca Güvenlik Merkezi'ndeki erişebilir. Güvenlik rolleri, Azure depolama, Web ve mobil veya nesnelerin interneti gibi diğer hizmet alanlarına erişimi yoktur.
>
>

## <a name="roles-and-allowed-actions"></a>Rolleri ve izin verilen eylemleri

Aşağıdaki tabloda rollerini görüntüler ve Güvenlik Merkezi'nde eylemlerine izin. Bir X eylemi o rol için izin verildiğini gösterir.

| Rol | Güvenlik ilkesini Düzenle | Bir kaynak için güvenlik önerilerini uygulama | Uyarıları ve öneriler Kapat | Uyarıları görüntüleme ve öneriler |
|:--- |:---:|:---:|:---:|:---:|
| Abonelik sahibi | X | X | X | X |
| Abonelik katkıda bulunanı | -- | X | X | X |
| Kaynak grubu sahibi | -- | X | -- | X |
| Kaynak grubu Katılımcısı | -- | X | -- | X |
| Okuyucu | -- | -- | -- | X |
| Güvenlik Yöneticisi | X | -- | X | X |
| Güvenlik Okuyucusu | -- | -- | -- | X |

> [!NOTE]
> Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, önerileri uygulamak veya ilkeleri düzenleme eyleme geçmeyecek ancak bir kaynak güvenlik durumu hakkındaki bilgileri görüntülemek için yalnızca ihtiyaç duyan kullanıcılar okuyucu rolüne atayın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede RBAC Güvenlik Merkezi kullanıcılara izinler atamak için nasıl kullandığı açıklanmıştır ve her rol için izin verilen eylemleri belirledik. Aboneliğiniz güvenlik durumunu izlemek için gereken rol atamaları ile ilgili bilgi sahibi olduğunuz, güvenlik ilkelerini düzenleme ve öneriler geçerlidir, öğrenme nasıl yapılır:

- [Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md)
- [Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md)
- [Azure kaynaklarınızın güvenlik durumunu izleme](security-center-monitoring.md)
- [Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
- [İş ortağı güvenlik çözümlerini izleme](security-center-partner-solutions.md)
