---
title: Site Recovery kasası Sil
description: Site Recovery senaryoyu temel bir Azure Site Recovery kasası silme hakkında bilgi edinin.
service: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: rajani-janaki-ram
ms.openlocfilehash: 80c479aa23da2a8471af3fd83879a2dbfc5d6195
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300580"
---
# <a name="delete-a-site-recovery-vault"></a>Site Recovery kasası Sil

Bağımlılıklar, bir Azure Site Recovery kasası silmesini engelleyebilirsiniz. Yapılması gereken eylemleri Site kurtarma senaryosunda göre farklılık gösterir. Azure Backup ile kullanılan bir kasa silmek için bkz: [Azure yedekleme kasasına silme](../backup/backup-azure-delete-vault.md).



## <a name="delete-a-site-recovery-vault"></a>Site Recovery kasası Sil 
Kasayı silmek için senaryonuz için önerilen adımları izleyin.

### <a name="vmware-vms-to-azure"></a>VMware VM'lerini Azure'a

1. İçindeki adımları izleyerek VM'ler korumalı Sil tüm [VMware için korumayı devre dışı bırakma](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).

2. İçindeki adımları izleyerek tüm çoğaltma ilkelerinin silme [çoğaltma ilkesini silmek](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy).

3. İçindeki adımları izleyerek vCenter başvurular silme [vCenter server silme](vmware-azure-manage-vcenter.md#delete-a-vcenter-server).

4. İçindeki adımları izleyerek yapılandırma sunucusunu silmek [yapılandırma sunucusu yetkisini](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server).

5. Kasayı silin.


### <a name="hyper-v-vms-with-vmm-to-azure"></a>Hyper-V VM’lerini (VMM ile) Azure’a
1. İçindeki adımları izleyerek VM'ler korumalı Sil tüm[(VMM ile) bir Hyper-V sanal makine için korumayı devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).

2. İlişkisini & Kasası'na göz atarak tüm çoğaltma ilkelerinin silme -> **Site Recovery altyapısı** -> **için System Center VMM** -> **çoğaltma İlkeleri**

3.  İçindeki adımları izleyerek VMM sunucuları başvurular silme [bağlı bir VMM sunucusunun kaydı](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Kasayı silin.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Azure için Hyper-V Vm'lerini (olmadan Sanal Makine Yöneticisi)
1. İçindeki adımları izleyerek VM'ler korumalı Sil tüm [(Hyper-V Azure) Hyper-V sanal makine için korumayı devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. İlişkisini & Kasası'na göz atarak tüm çoğaltma ilkelerinin silme -> **Site Recovery altyapısı** -> **için Hyper-V sitelerini** -> **çoğaltma ilkeleri**

3. Hyper-V sunucuları başvurular içindeki adımları izleyerek silme [bir Hyper-V ana bilgisayar kaydı](/site-recovery-manage-registration-and-protection.md#unregister-a-hyper-v-host-in-a-hyper-v-site).

4. Hyper-V sitesi silin.

5. Kasayı silin.


## <a name="use-powershell-to-force-delete-the-vault"></a>Zorlamak için kullanım PowerShell kasa silme 

> [!Important]
> Ürün test ettiğiniz ve veri kaybı hakkında ilgili değil, kasaya ve tüm bağımlılıklarını daha kolay kaldırmaya zorla delete yöntemini kullanın.
> PowerShell komut kasasının tüm içeriği siler ve olduğu **değil ters çevrilebilir**.

Korunan öğelerin olsa bile Site Recovery kasası silmek için aşağıdaki komutları kullanın:

    Connect-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmRecoveryServicesVault -Name "vaultname"

    Remove-AzureRmRecoveryServicesVault -Vault $vault

Daha fazla bilgi edinmek [Get-AzureRMRecoveryServicesVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault?view=azurermps-6.0.0), ve [Kaldır AzureRMRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/remove-azurermrecoveryservicesvault?view=azurermps-6.0.0).
