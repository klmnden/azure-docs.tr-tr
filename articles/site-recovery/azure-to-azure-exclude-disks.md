---
title: Azure Site Recovery - Azure PowerShell kullanarak Azure sanal makinelerini çoğaltma sırasında disk dışlamaya | Microsoft Docs
description: Azure PowerShell kullanarak Azure sanal makinelerinin diskleri sırasında Azure Site Recovery dışında tutmayı öğrenin.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2019
ms.author: asgang
ms.openlocfilehash: 54a32d7f7aa4bcab73f5828da3e7eba9d25276be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66160308"
---
# <a name="exclude-disks-from-powershell-replication-of-azure-vms"></a>Diskleri Azure Vm'lerini PowerShell çoğaltmanın dışında tutma

Bu makalede, Azure Vm'lerini çoğaltma yaparken diskleri tutma açıklar. Kullanılan çoğaltma bant genişliğini veya bu diskleri kullanın hedef tarafı kaynaklarını iyileştirmek için diskleri hariç. Şu anda bu özellik yalnızca Azure PowerShell kullanılabilir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

- Anladığınızdan emin olun [olağanüstü durum kurtarma mimarisini ve bileşenlerini](azure-to-azure-architecture.md).
- Tüm bileşenler için [destek gereksinimlerini](azure-to-azure-support-matrix.md) gözden geçirin.
- AzureRm PowerShell "Az" sahip olduğunuzdan emin olun modülü. Yükleme veya PowerShell güncelleştirme hakkında bilgi için bkz: [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).
- Bir kurtarma Hizmetleri kasası oluşturdunuz ve korunan sanal makine en az bir kez emin olun. Bunları yapmadıysanız, işlemi izleyin [Azure PowerShell kullanarak Azure sanal makineler için olağanüstü durum kurtarma ayarlama](azure-to-azure-powershell.md).

## <a name="why-exclude-disks-from-replication"></a>Neden diskleri çoğaltmanın dışında tutma
Çünkü, diskleri çoğaltmanın dışında tutma gerekebilir:

- Sanal makinenizi ulaştı [veri çoğaltmak için Azure Site Recovery sınırlarını değiştirme oranları](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix).

- Dışlanan diskteki verilerin önemli değildir veya çoğaltılması gerekmez.

- Verilerin çoğaltılmaması depolama ve ağ kaynaklarından tasarruf etmek istersiniz.

## <a name="how-to-exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutmak nasıl

Bizim örneğimizde, bir işletim sistemi olan bir sanal makine ve Batı ABD 2 bölgesi için Doğu ABD bölgesinde kullanıcının üç veri diskleri çoğaltın. Sanal makine adı *AzureDemoVM*. Biz 1 diskini dışarıda tutma ve diskleri 2 ve 3 tutun.

## <a name="get-details-of-the-virtual-machines-to-replicate"></a>Çoğaltma sanal makinelerin ayrıntılarını Al

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

Sanal makinenin disklerinin hakkındaki ayrıntıları alın. Bu bilgiler daha sonra sanal makinenin çoğaltma başlattığınızda kullanılacaktır.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="replicate-an-azure-virtual-machine"></a>Bir Azure sanal makinesinde çoğaltma

Aşağıdaki örnek için zaten bir önbellek depolama hesabı, çoğaltma ilkesi ve eşlemeleri olduğu varsayılır. Bunlar yoksa, işlemi izleyin [Azure PowerShell kullanarak Azure sanal makineler için olağanüstü durum kurtarma ayarlama](azure-to-azure-powershell.md).

Bir Azure sanal makinesinin çoğaltma *yönetilen diskler*.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration).

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded, so we will provide it during the time of replication. 

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


#Start replication by creating a replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Çoğaltma başlatma işlemi başarılı olduğunda, VM veri kurtarma bölgeye çoğaltılır.

Azure portalına gidin ve çoğaltılmış VM'lerin altında "çoğaltılan öğeler" konusuna bakın.

Çoğaltma işlemi kurtarma bölgesindeki sanal makinenin çoğaltılan diskleri bir kopyasını dengeli dağıtım tarafından başlatılır. Bu aşama, ilk çoğaltma aşaması olarak adlandırılır.

İlk çoğaltma sonlandırıldıktan sonra çoğaltma fark eşitleme aşamaya geçer. Bu noktada, sanal makine korunur. Tüm diskler dışarıda bırakılır görmek için korumalı sanal makineyi seçin.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [yük devretme testi çalıştırma](site-recovery-test-failover-to-azure.md).
