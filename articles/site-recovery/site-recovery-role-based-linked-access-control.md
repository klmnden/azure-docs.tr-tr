---
title: Rol tabanlı erişim denetimi (RBAC) ile Azure Site Recovery erişimini yönetme | Microsoft Docs
description: Bu makalede rol tabanlı erişim denetimi (RBAC) Azure Site Recovery erişimi yönetmek için nasıl uygulanacağını açıklar.
ms.service: site-recovery
ms.date: 04/08/2019
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 33fc2cd19152fb6cbbffb106aa058948d39555f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61471443"
---
# <a name="manage-site-recovery-access-with-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) Site Recovery erişimini yönetme

Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kendi sorumluluklarını ayırabilir ve belirli işlerini gerçekleştirmek için gereken şekilde kullanıcılara yalnızca belirli erişim izinleri verme.

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için 3 yerleşik rol sağlar. [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

* [Site Recovery Katkıda Bulunanı](../role-based-access-control/built-in-roles.md#site-recovery-contributor) - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol etkinleştirebilen ve uygulamalar veya kuruluşların tamamı için olağanüstü durum kurtarma gibi durumda may olmak yönetme olağanüstü durum kurtarma yöneticileri için idealdir.
* [Site Recovery Operatörü](../role-based-access-control/built-in-roles.md#site-recovery-operator) - Bu rol, Yük Devretme ve Yeniden Çalışma işlemlerini yürütme ve yönetme izinlerine sahiptir. Bu role sahip bir kullanıcı etkinleştiremez veya çoğaltmayı devre dışı bırak, oluşturma veya kasa silme, yeni altyapılar kaydedebilir veya diğer kullanıcılara erişim hakkı atayabilirsiniz. Bu rol yük devretme sanal makinelerini bir olağanüstü durum kurtarma operatörü için idealdir veya uygulama sahipleri ve BT yöneticileri bir DR gibi bir gerçek veya sanal bir olağanüstü durumda tarafından istendiğinde uygulamaları detayına gidin. POST çözümleme olağanüstü DR operatörü yeniden koruma altına alabilir ve yeniden çalışma sanal makineler.
* [Site Recovery Okuyucusu](../role-based-access-control/built-in-roles.md#site-recovery-reader) - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol kullanan mevcut koruma durumunu izleyebilir ve gerekirse destek biletleri oluşturabilen BT izleme Yöneticisi için idealdir.

Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, bkz. nasıl [özel roller oluşturma](../role-based-access-control/custom-roles.md) azure'da.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Yeni sanal makineler için çoğaltmayı etkinleştirmek için gereken izinler
Yeni bir sanal makine Azure Site Recovery kullanılarak Azure'a çoğaltılırken, ilişkili kullanıcının erişim düzeyi kullanıcının Site Recovery için sağlanan Azure kaynakları için gerekli izinlere sahip olduğundan emin olmak doğrulanır.

Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için bir kullanıcı olması gerekir:
* Seçilen kaynak grubunda bir sanal makine oluşturma izni
* Seçilen sanal ağda bir sanal makine oluşturma izni
* Seçili depolama hesabına yazma izni

Bir kullanıcı yeni bir sanal makine çoğaltmayı tamamlamak için aşağıdaki izinler gerekiyor.

> [!IMPORTANT]
>İlgili izinler dağıtım modeli eklendiğinden emin olun (Resource Manager / Klasik) kaynak dağıtımı için kullanılır.

| **Kaynak Türü** | **Dağıtım modeli** | **İzni** |
| --- | --- | --- |
| İşlem | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Klasik | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Ağ | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Klasik | Microsoft.ClassicNetwork/virtualNetworks/read |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Depolama | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Klasik | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Kaynak Grubu | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

'Sanal makine Katılımcısı' ve 'Klasik sanal makine Katılımcısı' kullanmayı düşünün [yerleşik roller](../role-based-access-control/built-in-roles.md) için Resource Manager ve klasik dağıtım modelleri sırasıyla.

## <a name="next-steps"></a>Sonraki adımlar
* [Rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md): Azure portalında RBAC ile çalışmaya başlayın.
* Erişim ile yönetmeyi öğrenin:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Rol tabanlı erişim denetimi sorunlarını giderme](../role-based-access-control/troubleshooting.md): Yaygın sorunları çözme için öneriler alın.
