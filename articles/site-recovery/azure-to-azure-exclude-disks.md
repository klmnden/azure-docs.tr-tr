---
title: Azure Site Recovery - Azure PowerShell kullanarak Azure sanal makinelerini çoğaltma sırasında disk dışlama | Microsoft Docs
description: Azure PowerShell kullanarak Azure Site Recovery ile Azure sanal makineler için disk dışında tutmayı öğrenin.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2019
ms.author: asgang
ms.openlocfilehash: 1c278d810df7e5ba8701529a59987c9bb16fa40c
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044134"
---
# <a name="exclude-disks-from-replication-of-azure-vms-to-azure-using-azure-powershell"></a>Azure PowerShell kullanarak Azure'a geçişte diskleri Azure vm'leri bir çoğaltmanın dışında tutma

Bu makalede, Azure Vm'lerini çoğaltma yaparken diskleri tutma açıklar. Bu dışında tutma, kullanılan çoğaltma bant genişliğini iyileştirebilir veya bu gibi disklerin kullandığı hedef tarafı kaynakları iyileştirebilir. Şu anda bu özellik yalnızca Azure PowerShell kullanıma sunulur.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](azure-to-azure-support-matrix.md) gözden geçirin.
- Azure PowerShell'in `Az` modülü. Yüklemek veya Azure PowerShell yükseltmek gerekirse bu izleyin [Azure PowerShell'i yükleme ve yapılandırma için kılavuz](/powershell/azure/install-az-ps).
- Kurtarma Hizmetleri kasası oluşturmuş olmanız ve sanal makineler en az bir kez koruma yaptıktan. Olmayan sonra bunu yaparsanız bahsedilen belgeleri kullanarak [burada](azure-to-azure-powershell.md).

## <a name="why-exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma nedenleri nelerdir?
Disklerin çoğaltmanın dışında tutulması, çoğu zaman aşağıdaki nedenlerden dolayı gereklidir:

- Sanal makinenizi ulaştı [veri çoğaltmak için Azure Site Recovery sınırlarını değiştirme oranları](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix)

- Hariç tutulan diskteki değişen veriler önemli değildir veya çoğaltılmaları gerekmez.

- Bu dalgalanmayı çoğaltmayarak depolama ve ağ kaynaklarından tasarruf sağlamak istersiniz.


## <a name="how-to-exclude-disks-from-replication"></a>Diskler nasıl çoğaltmanın dışında tutulur?

Bu makalede, 1 olan bir sanal makine örnekte Doğu ABD bölgesindeki işletim sistemi ile 3 veri çoğaltılabilir Batı ABD 2 bölgesinde. Örnekte kullanılan sanal makine AzureDemoVM adıdır. Disk 1 dışında bırakır ve disk 2 ve 3 tutar

## <a name="get-details-of-the-virtual-machines-to-be-replicated"></a>Çoğaltılacak sanal makinelerin ayrıntılarını Al

```azurepowershell
# Get details of the virtual machine
$VM = Get-AzVM -ResourceGroupName "A2AdemoRG" -Name "AzureDemoVM"

Write-Output $VM     
```

```
ResourceGroupName  : A2AdemoRG
Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/A2AdemoRG/providers/Microsoft.Compute/virtualMachines/AzureDemoVM
VmId               : 1b864902-c7ea-499a-ad0f-65da2930b81b
Name               : AzureDemoVM
Type               : Microsoft.Compute/virtualMachines
Location           : eastus
Tags               : {}
DiagnosticsProfile : {BootDiagnostics}
HardwareProfile    : {VmSize}
NetworkProfile     : {NetworkInterfaces}
OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
ProvisioningState  : Succeeded
StorageProfile     : {ImageReference, OsDisk, DataDisks}
```


Sanal makine diskleri için disk ayrıntıları alın. Disk ayrıntıları daha sonra sanal makine için çoğaltmayı başlatırken kullanılacak.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="replicate-azure-virtual-machine"></a>Azure sanal makine çoğaltma

İçinde aşağıdaki örnekte zaten bir önbellek depolama hesabı, çoğaltma ilkesi ve eşlemeleri sahip biz kabul. Olmayan sonra bunu yaparsanız bahsedilen belgeleri kullanarak [burada](azure-to-azure-powershell.md) 


Azure sanal makinesinin çoğaltma **yönetilen diskler**.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded so we will provide it during the time of replication 

# Data disk 2
$datadiskId2  = $vm.StorageProfile.DataDisks[1].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[1]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[1]. StorageAccountType

$DataDisk2ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId2 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

# Data Disk 3

$datadiskId3  = $vm.StorageProfile.DataDisks[2].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[2]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[2]. StorageAccountType

$DataDisk3ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId3 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk2ReplicationConfig, $DataDisk3ReplicationConfig


#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Başlangıç çoğaltma işlemi başarılı olduktan sonra sanal makine verilerini kurtarma bölgeye çoğaltılır.

Azure portalına gidebilir ve çoğaltılan öğeler çoğaltılan sanal makineler görebilirsiniz.
Başlangıçta bir kurtarma bölgesindeki sanal makinenin çoğaltılan diskleri kopyasını dengeli dağıtım çoğaltma işlemi başlatır. Bu aşama, ilk çoğaltma aşaması olarak adlandırılır.

İlk çoğaltma tamamlandıktan sonra çoğaltma fark eşitleme aşamaya taşır. Bu noktada, sanal makine korunur. Korumalı sanal makineye tıklayın > diskler, disk veya dışlanırsa, görmek için.

## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](site-recovery-test-failover-to-azure.md) yük devretme testi çalıştırma hakkında.
