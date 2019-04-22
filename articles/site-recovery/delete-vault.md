---
title: Azure Site Recovery hizmeti için yapılandırılmış bir kurtarma Hizmetleri kasasını silme
description: Azure Site Recovery için yapılandırılmış bir kurtarma Hizmetleri kasasını silme hakkında bilgi edinin
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajani-janaki-ram
ms.openlocfilehash: b5d035308c50525449edf47131c4a6a8c62b750b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59784769"
---
# <a name="delete-a-site-recovery-services-vault"></a>Site kurtarma Hizmetleri kasasını silme

Bağımlılıklar Azure Site Recovery kasası silmesi engelleyebilirsiniz. Yapmanız gereken eylemler Site kurtarma senaryosuna bağlı olarak değişiklik gösterir. Azure Yedekleme'de kullanılan bir kasayı silme için bkz: [Azure bir Backup kasasını silme](../backup/backup-azure-delete-vault.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-a-site-recovery-vault"></a>Site Recovery kasasını silme 
Kasayı silmek için senaryonuz için önerilen adımları izleyin.

### <a name="vmware-vms-to-azure"></a>VMware VM'lerini Azure'a

1. İçindeki adımları izleyerek Vm'leri korumalı Sil tüm [bir VMware korumasını devre dışı bırak](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure).

2. İçindeki adımları izleyerek tüm çoğaltma ilkelerinin Sil [çoğaltma ilkesini silme](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy).

3. İçindeki adımları izleyerek vCenter başvurularını Sil [vCenter sunucusu silme](vmware-azure-manage-vcenter.md#delete-a-vcenter-server).

4. İçindeki adımları izleyerek yapılandırma sunucusunu silme [yapılandırma sunucusunu yetkisini](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server).

5. Kasayı silin.


### <a name="hyper-v-vms-with-vmm-to-azure"></a>Hyper-V VM’lerini (VMM ile) Azure’a
1. İçindeki adımları izleyerek Vm'leri korumalı Sil tüm[(VMM ile) bir Hyper-V VM korumasını devre dışı bırak](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario).

2. İlişkisini ve tüm çoğaltma ilkelerinin kasanıza göz atarak sil -> **Site Recovery altyapısı** -> **için System Center VMM** -> **çoğaltma İlkeleri**

3.  İçindeki adımları izleyerek VMM sunuculara yönelik başvuruları Sil [bağlı bir VMM sunucusunun kaydını kaldırma](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server).

4.  Kasayı silin.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Azure'a Hyper-V Vm'lerini (olmadan Virtual Machine Manager)
1. İçindeki adımları izleyerek Vm'leri korumalı Sil tüm [(Hyper-V'den azure'a) Hyper-V sanal makine korumasını devre dışı bırak](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. İlişkisini ve tüm çoğaltma ilkelerinin kasanıza göz atarak sil -> **Site Recovery altyapısı** -> **Hyper-V siteleri için** -> **çoğaltma ilkeleri**

3. İçindeki adımları izleyerek Hyper-V sunuculara yönelik başvuruları Sil [Hyper-V konağının kaydını kaldırma](site-recovery-manage-registration-and-protection.md#unregister-a-hyper-v-host-in-a-hyper-v-site).

4. Hyper-V sitesini silin.

5. Kasayı silin.


## <a name="use-powershell-to-force-delete-the-vault"></a>Zorlamak için PowerShell kullanma kasayı silme 

> [!Important]
> Ürünü test ettiğiniz ve veri kaybı hakkında endişe değil, kasayı ve tüm bağımlılıklarını hızlıca kaldırmak için zorla silme yöntemi kullanın.
> PowerShell komut kasa tüm içeriğini siler ve olduğu **değil ters çevrilebilir**.

Korumalı öğeler olsa bile Site Recovery kasasını silmek için aşağıdaki komutları kullanın:

    Connect-AzAccount

    Select-AzSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzRecoveryServicesVault -Name "vaultname"

    Remove-AzRecoveryServicesVault -Vault $vault

Daha fazla bilgi edinin [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault), ve [Remove-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault).
