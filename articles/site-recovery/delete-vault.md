---
title: "Site Recovery kasası Sil"
description: "Site Recovery senaryoyu temel bir Azure Site Recovery kasası silme hakkında bilgi edinin."
service: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: 77bc7e068629c5dbf650b39844912b1b7a6155b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="delete-a-site-recovery-vault"></a>Site Recovery kasası Sil
Bağımlılıklar, bir Azure Site Recovery kasası silmesini engelleyebilirsiniz. Size gereken eylemleri Site kurtarma senaryosunda göre farklılık gösterir: VMware azure'a, Hyper-V (ile arama ve Sistem Merkezi Sanal Makine Yöneticisi olmadan) Azure ve Azure yedekleme için. Azure Backup ile kullanılan bir kasa silmek için bkz: [Azure yedekleme kasasına silme](../backup/backup-azure-delete-vault.md).



## <a name="delete-a-site-recovery-vault"></a>Site Recovery kasası Sil 
Kasayı silmek için senaryonuz için önerilen adımları izleyin.

### <a name="vmware-vms-to-azure"></a>VMware VM'lerini Azure'a

1. İçindeki adımları izleyerek VM'ler korumalı Sil tüm [VMware için korumayı devre dışı bırakma](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).

2. İçindeki adımları izleyerek tüm çoğaltma ilkelerinin silme [çoğaltma ilkesini silmek](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).

3. İçindeki adımları izleyerek vCenter başvurular silme [bir vCenter silmek](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).

4. İçindeki adımları izleyerek yapılandırma sunucusunu silmek [yapılandırma sunucusu yetkisini](site-recovery-vmware-to-azure-manage-configuration-server.md##delete-or-unregister-a-configuration-server).

5. Kasayı silin.


### <a name="hyper-v-vms-with-virtual-machine-manager-to-azure"></a>Azure Hyper-V sanal makineleri (Sanal Makine Yöneticisi ile)
1. İçindeki adımları izleyerek VM'ler korumalı Sil tüm[bir Hyper-V sanal makine Azure senaryo için System Center VMM kullanarak azure'a çoğaltmak için korumayı devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).

2. İlişkisini & Kasası'na göz atarak tüm çoğaltma ilkelerinin silme -> **Site Recovery altyapısı** -> **için System Center VMM** -> **çoğaltma İlkeleri**

3.  Sanal Makine Yöneticisi sunucularına başvuruları içindeki adımları izleyerek silmek [bağlı bir VMM sunucusunun kaydı](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Kasayı silin.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Azure için Hyper-V Vm'lerini (olmadan Sanal Makine Yöneticisi)
1. İçindeki adımları izleyerek VM'ler korumalı Sil tüm [(Hyper-V Azure) Hyper-V sanal makine için korumayı devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. İlişkisini & Kasası'na göz atarak tüm çoğaltma ilkelerinin silme -> **Site Recovery altyapısı** -> **için Hyper-V sitelerini** -> **çoğaltma ilkeleri**

3. Hyper-V sunucuları başvurular içindeki adımları izleyerek silme [bir Hyper-V ana bilgisayar kaydı](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).

4. Hyper-V sitesi silin.

5. Kasayı silin.


## <a name="use-powershell-to-force-delete-the-vault"></a>Zorlamak için kullanım PowerShell kasa silme 

> [!Important]
> Ürün test ettiğiniz ve veri kaybı hakkında ilgili değil, kasaya ve tüm bağımlılıklarını daha kolay kaldırmaya zorla delete yöntemini kullanın.
> PowerShell komut kasasının tüm içeriği siler ve olduğu **değil ters çevrilebilir**.

Korunan öğelerin olsa bile Site Recovery kasası silmek için aşağıdaki komutları kullanın:

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault