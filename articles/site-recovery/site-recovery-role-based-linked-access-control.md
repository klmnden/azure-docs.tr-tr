---
title: "Azure Site Recovery yönetmek için rol tabanlı erişim denetimini kullanma | Microsoft Docs"
description: "Bu makalede nasıl uygulanacağını ve Azure Site Recovery dağıtımlarınızı yönetmek için rol tabanlı erişim denetimi (RBAC) kullanın"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: ce579bc2844d321e4fbc70726b57120e17e4788d
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="use-role-based-access-control-to-manage-azure-site-recovery-deployments"></a>Azure Site Recovery dağıtımları yönetmek için rol tabanlı erişim denetimi kullanma

Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, ekibiniz içinde sorumlulukları kurabilmeleri ve belirli işlerini gerçekleştirmek için gereken şekilde kullanıcılara yalnızca belirli erişim izinleri verin.

Azure Site Recovery Site Recovery yönetim işlemlerini denetlemek için 3 yerleşik roller sağlar. Daha fazla bilgi almak [Azure RBAC yerleşik rolleri](../active-directory/role-based-access-built-in-roles.md)

* [Site kurtarma katkıda bulunan](../active-directory/role-based-access-built-in-roles.md#site-recovery-contributor) -bu rol bir kurtarma Hizmetleri kasası Azure Site Recovery işlemlerini yönetmek için gerekli tüm izinlere sahiptir. Bu rolüne sahip bir kullanıcı ancak oluşturmak veya bir kurtarma Hizmetleri kasası silemez veya diğer kullanıcıların erişim hakları atayın. Bu rol etkinleştirin ve olağanüstü durum kurtarma uygulamaları ya da tüm kuruluşlar için olarak durumda may olmak yönetmek olağanüstü durum kurtarma yöneticileri için uygundur.
* [Site kurtarma işleci](../active-directory/role-based-access-built-in-roles.md#site-recovery-operator) -yürütme izni ve yük devretme ve yeniden çalışma operations manager bu rolü vardır. Bu rolüne sahip bir kullanıcı etkinleştirmek veya çoğaltma devre dışı bırakmak, oluşturmak veya kasalarını silmek, yeni altyapı kaydedilemiyor veya diğer kullanıcıların erişim hakları atayın. Bu rol kimin yük devretme sanal makinelerin olağanüstü durum kurtarma işleci için uygundur veya uygulama sahipleri ve gerçek veya sanal olağanüstü durumda bir DR gibi BT yöneticileri tarafından istendiğinde uygulamaları ayrıntıya gidin. POST olağanüstü durum çözümlemesi, DR işleci yeniden koruyabilir ve geri dönme sanal makineler.
* [Site kurtarma okuyucu](../active-directory/role-based-access-built-in-roles.md#site-recovery-reader) -bu rol tüm Site Recovery yönetim işlemlerini görüntülemek için gerekli izinlere sahip. Bu rol kimin koruma geçerli durumunu izleyebilir ve Destek biletlerini gerekli olursa BT izleme yöneticinin için uygundur.

Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, bkz: nasıl yapılır [özel roller yapı](../active-directory/role-based-access-control-custom-roles.md) Azure.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Yeni sanal makineler için çoğaltmayı etkinleştirme için gereken izinler
Yeni bir sanal makine için Azure Site Kurtarma'yı kullanarak Azure çoğaltıldığında ilişkili kullanıcının erişim düzeyleri kullanıcı için Site Recovery sağlanan Azure kaynaklarını kullanmak için gerekli izinlere sahip olduğundan emin olmak için doğrulanır.

Yeni bir sanal makine için çoğaltmayı etkinleştirmek için bir kullanıcı olması gerekir:
* Seçili kaynak grubuna bir sanal makine oluşturma izni
* Seçilen sanal ağ içinde bir sanal makine oluşturma izni
* Seçili depolama hesabına yazma izni

Bir kullanıcının yeni bir sanal makinenin tam çoğaltma aşağıdaki izinleri gerekir.

> [!IMPORTANT]
>İlgili izinleri dağıtım modeli eklendiğinden emin olun (Resource Manager / Klasik) kaynak dağıtımı için kullanılır.

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

'Sanal makine Katılımcısı' ve 'Klasik sanal makine Katılımcısı' kullanmayı düşünün [yerleşik roller](../active-directory/role-based-access-built-in-roles.md) için Resource Manager ve klasik dağıtım modelleri sırasıyla.

## <a name="next-steps"></a>Sonraki adımlar
* [Rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md): Azure portalında RBAC ile çalışmaya başlama.
* Erişimle yönetmeyi öğrenin:
  * [PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  * [Azure CLI](../active-directory/role-based-access-control-manage-access-azure-cli.md)
  * [REST API](../active-directory/role-based-access-control-manage-access-rest.md)
* [Rol tabanlı erişim denetimi sorun giderme](../active-directory/role-based-access-control-troubleshooting.md): sık karşılaşılan sorunları düzeltmek için öneriler alın.
