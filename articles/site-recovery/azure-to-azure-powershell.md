---
title: Azure Site Recovery - Kurulum ve test edin, Azure PowerShell kullanarak Azure sanal makineler için olağanüstü durum kurtarma | Microsoft Docs
description: Azure sanal makineleri için Azure PowerShell kullanarak Azure Site Recovery ile olağanüstü durum kurtarma ayarlamayı öğrenin.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 3/29/2019
ms.author: sutalasi
ms.openlocfilehash: d11ebad3eaa629a1b03d22c6548f3b7ad591cf5b
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60003824"
---
# <a name="set-up-disaster-recovery-for-azure-virtual-machines-using-azure-powershell"></a>Azure PowerShell kullanarak Azure sanal makineler için olağanüstü durum kurtarmayı ayarlama


Kurulum ve test etmek nasıl gördüğünüz bu makalede, Azure PowerShell kullanarak Azure sanal makineler için olağanüstü durum kurtarma.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> - Kurtarma Hizmetleri kasası oluşturun.
> - PowerShell oturumunun kasa bağlamını ayarlayın.
> - Azure sanal makineleri çoğaltmaya başlamak için kasa hazırlayın.
> - Ağ eşlemeleri oluşturun.
> - Sanal makineleri çoğaltmak için depolama hesapları oluşturun.
> - Olağanüstü durum kurtarma için bir kurtarma bölgesinde Azure sanal makinelerini çoğaltma.
> - Yük devretme testi gerçekleştirmek, doğrulama ve yük devretme testini Temizle.
> - Kurtarma bölgeye yük devretme.

> [!NOTE]
> Portal aracılığıyla sunulan tüm senaryo özellikleri, Azure PowerShell aracılığıyla kullanıma sunulabilir. Azure PowerShell aracılığıyla şu anda desteklenmeyen senaryo özelliklerden bazıları şunlardır:
> - Sanal makinedeki tüm diskler her disk sanal makinenin açıkça belirtmek zorunda kalmadan çoğaltılacak belirtme olanağı.  


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](azure-to-azure-support-matrix.md) gözden geçirin.
- Azure PowerShell'in `Az` modülü. Yüklemek veya Azure PowerShell yükseltmek gerekirse bu izleyin [Azure PowerShell'i yükleme ve yapılandırma için kılavuz](/powershell/azure/install-az-ps).

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğiniz için oturum açın

Connect-AzAccount cmdlet'ini kullanarak Azure aboneliğinizde oturum açın

```azurepowershell
Connect-AzAccount
```
Azure aboneliğinizi seçin. Sahip olduğunuz Azure aboneliklerin listesi, erişim elde etmek için Get-AzSubscription cmdlet'ini kullanın. Select AzSubscription cmdlet'ini kullanarak çalışmaya Azure aboneliğini seçin.

```azurepowershell
Select-AzSubscription -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

## <a name="get-details-of-the-virtual-machines-to-be-replicated"></a>Çoğaltılacak sanal makinelerin ayrıntılarını Al

Bu makaledeki örnekte, bir sanal makine Doğu ABD bölgesinde çoğaltılır ve Batı ABD 2 bölgesinde kurtarıldı. Çoğaltılan sanal makineyi, bir işletim sistemi diski ve bir tek veri diskine sahip bir sanal makinesidir. Örnekte kullanılan sanal makine AzureDemoVM adıdır.

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

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası oluşturmak bir kaynak grubu oluşturun.

> [!IMPORTANT]
> * Kurtarma Hizmetleri kasası ve korunmakta olan sanal makineleri farklı Azure konumlarında olması gerekir.
> * Kurtarma Hizmetleri kasası korunmakta olan sanal makineler ve kaynak grubu, farklı Azure konumlarında olmalıdır.
> * Kurtarma Hizmetleri kasası ve ait olduğu, kaynak grubu, aynı Azure konumda olabilir.

Bu makaledeki örnekte Doğu ABD bölgesinde korunan sanal makine var. Olağanüstü durum kurtarma için seçilen kurtarma Batı ABD 2 bölgesinde bölgedir. Kurtarma Hizmetleri kasası ve kasa kaynak grubu (Batı ABD 2) kurtarma bölgede hem de olması

```azurepowershell
#Create a resource group for the recovery services vault in the recovery Azure region
New-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"
```
```
ResourceGroupName : a2ademorecoveryrg
Location          : westus2
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg
```

Bir kurtarma Hizmetleri kasası oluşturun. A2aDemoRecoveryVault adlı kurtarma Hizmetleri kasası aşağıdaki örnekte Batı ABD 2 bölgesinde oluşturulur.

```azurepowershell
#Create a new Recovery services vault in the recovery region
$vault = New-AzRecoveryServicesVault -Name "a2aDemoRecoveryVault" -ResourceGroupName "a2ademorecoveryrg" -Location "West US 2"

Write-Output $vault
```
```
Name              : a2aDemoRecoveryVault
ID                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg/providers/Microsoft.RecoveryServices/vaults/a2aDemoRecoveryVault
Type              : Microsoft.RecoveryServices/vaults
Location          : westus2
ResourceGroupName : a2ademorecoveryrg
SubscriptionId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```
## <a name="set-the-vault-context"></a>Kasa bağlamını ayarlayın

> [!TIP]
> Azure Site Recovery PowerShell Modülü (Az.RecoveryServices Modülü), çoğu cmdlet için kullanımı kolay diğer adları ile birlikte gelir. Modüldeki cmdlet'ler biçiminde  *\<işlemi >-**AzRecoveryServicesAsr**\<Nesne >* ve biçiminde eşdeğer diğer adlar  *\< İşlem >-**ASR**\<Nesne >*. Bu makalede, kolay okunması için cmdlet diğer adları kullanır.

PowerShell oturumunda kullanmak için kasa bağlamını ayarlayın. Bunu yapmak için kasa ayarları dosyasına indirin ve kasa bağlamını ayarlamak için PowerShell oturumunda indirilen dosyayı içeri aktarın.

Bir kez ayarlandıktan sonra sonraki Azure Site Recovery işlemlerini bir PowerShell oturumunda seçili kasa bağlamında gerçekleştirilir.

 ```azurepowershell
#Download the vault settings file for the vault.
$Vaultsettingsfile = Get-AzRecoveryServicesVaultSettingsFile -Vault $vault -SiteRecovery -Path C:\users\user\Documents\

#Import the downloaded vault settings file to set the vault context for the PowerShell session.
Import-AzRecoveryServicesAsrVaultSettingsFile -Path $Vaultsettingsfile.FilePath

```
```
ResourceName         ResourceGroupName ResourceNamespace          ResourceType
------------         ----------------- -----------------          -----------
a2aDemoRecoveryVault a2ademorecoveryrg Microsoft.RecoveryServices Vaults     
```

```azurepowershell
#Delete the downloaded vault settings file
Remove-Item -Path $Vaultsettingsfile.FilePath
```
## <a name="prepare-the-vault-to-start-replicating-azure-virtual-machines"></a>Azure sanal makineleri çoğaltmaya başlamak için kasa hazırlama

### <a name="create-a-site-recovery-fabric-object-to-represent-the-primary-source-region"></a>Birincil (kaynak) bölgeyi temsil eden bir Site Recovery doku nesnesi oluşturun

Kasadaki doku nesnesi, bir Azure bölgesi temsil eder. Kasa için korunan sanal makinelerle ait Azure bölgesini temsil etmek için birincil doku nesnesi oluşturulur. Bu makaledeki örnekte Doğu ABD bölgesinde korunan sanal makine var.

- Bölge başına yalnızca bir doku nesnesi oluşturulabilir.
- Azure portalında bir VM için Site Recovery çoğaltma daha önce etkinleştirdiyseniz, Site Recovery otomatik olarak bir doku nesnesi oluşturur. Bir bölge için bir doku nesnesi varsa, yeni bir tane oluşturamazsınız.


Başlamadan önce Site Recovery işlemlerini zaman uyumsuz olarak yürütülür. Bir işlem başlattığınızda, Azure Site Recovery iş gönderilir ve nesne izleme iş döndürülür. Nesne izleme iş (Get-ASRJob) iş için en son durumu almak ve işlemin durumunu izlemek için kullanın.

```azurepowershell
#Create Primary ASR fabric
$TempASRJob = New-ASRFabric -Azure -Location 'East US'  -Name "A2Ademo-EastUS"

# Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State

$PrimaryFabric = Get-AsrFabric -Name "A2Ademo-EastUS"
```
Aynı kasaya birden fazla Azure bölgesini sanal makineleri korumalı bir doku nesnesi için her kaynak Azure bölgesini oluşturun.

### <a name="create-a-site-recovery-fabric-object-to-represent-the-recovery-region"></a>Kurtarma bölgesi temsil etmek için bir Site Recovery doku nesnesi oluşturun

Kurtarma doku nesnesi, Azure konum kurtarma temsil eder. Sanal makineler için çoğaltılır ve için kurtarma fabric tarafından temsil edilen kurtarma bölgesi (yük devretme durumunda) kurtarıldı. Bu örnekte kullanılan bir Azure bölgesine kurtarma Batı ABD 2 ' dir.

```azurepowershell
#Create Recovery ASR fabric
$TempASRJob = New-ASRFabric -Azure -Location 'West US 2'  -Name "A2Ademo-WestUS"

# Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State

$RecoveryFabric = Get-AsrFabric -Name "A2Ademo-WestUS"

```

### <a name="create-a-site-recovery-protection-container-in-the-primary-fabric"></a>Birincil dokusunda bir Site Recovery koruma kapsayıcısı oluşturma

Koruma kapsayıcısı, çoğaltılan öğeler bir yapı içinde gruplamak için kullanılan bir kapsayıcıdır.

```azurepowershell
#Create a Protection container in the primary Azure region (within the Primary fabric)
$TempASRJob = New-AzRecoveryServicesAsrProtectionContainer -InputObject $PrimaryFabric -Name "A2AEastUSProtectionContainer"

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

Write-Output $TempASRJob.State

$PrimaryProtContainer = Get-ASRProtectionContainer -Fabric $PrimaryFabric -Name "A2AEastUSProtectionContainer"
```
### <a name="create-a-site-recovery-protection-container-in-the-recovery-fabric"></a>Kurtarma dokusunda bir Site Recovery koruma kapsayıcısı oluşturma

```azurepowershell
#Create a Protection container in the recovery Azure region (within the Recovery fabric)
$TempASRJob = New-AzRecoveryServicesAsrProtectionContainer -InputObject $RecoveryFabric -Name "A2AWestUSProtectionContainer"

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"

Write-Output $TempASRJob.State

$RecoveryProtContainer = Get-ASRProtectionContainer -Fabric $RecoveryFabric -Name "A2AWestUSProtectionContainer"
```

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

```azurepowershell
#Create replication policy
$TempASRJob = New-ASRPolicy -AzureToAzure -Name "A2APolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State

$ReplicationPolicy = Get-ASRPolicy -Name "A2APolicy"
```
### <a name="create-a-protection-container-mapping-between-the-primary-and-recovery-protection-container"></a>Bir koruma kapsayıcısı eşlemesine arasındaki birincil ve kurtarma koruma kapsayıcısı oluşturma

Bir koruma kapsayıcısı eşlemesine kurtarma koruma kapsayıcısı ve bir çoğaltma ilkesi ile birincil koruma kapsayıcısı eşlemeleri. Koruma kapsayıcısı çifti arasında sanal makineleri çoğaltmak için kullanacağınız her bir çoğaltma ilkesi için bir eşleme oluşturun.

```azurepowershell
#Create Protection container mapping between the Primary and Recovery Protection Containers with the Replication policy
$TempASRJob = New-ASRProtectionContainerMapping -Name "A2APrimaryToRecovery" -Policy $ReplicationPolicy -PrimaryProtectionContainer $PrimaryProtContainer -RecoveryProtectionContainer $RecoveryProtContainer

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State

$EusToWusPCMapping = Get-ASRProtectionContainerMapping -ProtectionContainer $PrimaryProtContainer -Name "A2APrimaryToRecovery"
```

### <a name="create-a-protection-container-mapping-for-failback-reverse-replication-after-a-failover"></a>Koruma kapsayıcısı eşlemesini yeniden çalışma (yük devretme sonrasında çoğaltmayı tersine çevirme) için oluşturma

Bir sanal makineyi geri özgün Azure bölgesindeki için yeniden çalışma başarısız duruma getirmeye hazır olduğunda yük devretme sonrasında. Yeniden çalışma, başarısız başarısız olarak çoğaltılmış sanal makine üzerinde ters olan özgün bölgeye bölge üzerinden. Çoğaltmayı tersine çevirme için özgün bölgesi ve kurtarma bölgesi rollerini geçin. Özgün bölge, yeni kurtarma bölgesi artık hale gelir ve ilk kurtarma bölgesi artık neydi birincil bölgeye olur. Çoğaltmayı tersine çevirme için koruma kapsayıcısı eşlemesini özgün ve kurtarma bölgelerin anahtarlı rolleri temsil eder.

```azurepowershell
#Create Protection container mapping (for failback) between the Recovery and Primary Protection Containers with the Replication policy
$TempASRJob = New-ASRProtectionContainerMapping -Name "A2ARecoveryToPrimary" -Policy $ReplicationPolicy -PrimaryProtectionContainer $RecoveryProtContainer -RecoveryProtectionContainer $PrimaryProtContainer

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State
```

## <a name="create-cache-storage-accounts-and-target-storage-accounts"></a>Önbellek depolama hesapları ve hedef depolama hesabı oluşturma

Bir önbellek depolama hesabına çoğaltılan sanal makine ile aynı Azure bölgesindeki bir standart depolama hesabıdır. Önbellek depolama hesabı, değişiklikleri Azure bölgesine kurtarma taşınır önce çoğaltma değişiklikleri geçici olarak tutmak için kullanılır. (Ancak gerek kalmayacak şekilde) seçebileceğiniz bir sanal makinenin farklı diskler için farklı bir önbellek depolama hesapları belirtin.

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$EastUSCacheStorageAccount = New-AzStorageAccount -Name "a2acachestorage" -ResourceGroupName "A2AdemoRG" -Location 'East US' -SkuName Standard_LRS -Kind Storage
```

Sanal makineler için **yönetilen diskleri kullanmayan**, sanal makinenin diskleri yinelenir kurtarma bölgesinde bir depolama hesabı hedef depolama hesabıdır. Hedef depolama hesabı, standart depolama hesabı veya bir premium depolama hesabı olabilir. Depolama hesabı gereklidir (GÇ Yazma oranı) veri değişim hızı için diskler ve depolama türü için Azure Site Recovery tarafından desteklenen değişim sınırları temel türü seçin.

```azurepowershell
#Create Target storage account in the recovery region. In this case a Standard Storage account
$WestUSTargetStorageAccount = New-AzStorageAccount -Name "a2atargetstorage" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -SkuName Standard_LRS -Kind Storage

```

## <a name="create-network-mappings"></a>Ağ eşlemeleri oluşturma

Ağ eşlemesi, Kurtarma bölgesindeki sanal ağlar için birincil bölgedeki sanal ağlar eşler. Ağ eşlemesi, birincil sanal ağdaki bir sanal makine yük devretme için gereken kurtarma bölgesinde Azure sanal ağı belirtir. Bir Azure sanal ağı, yalnızca tek bir Azure sanal ağına kurtarma bölgesindeki eşlenebilir.

- Yük devretme için kurtarma bölgede bir Azure sanal ağı oluşturun

   ```azurepowershell
    #Create a Recovery Network in the recovery region
    $WestUSRecoveryVnet = New-AzVirtualNetwork -Name "a2arecoveryvnet" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -AddressPrefix "10.0.0.0/16"

    Add-AzVirtualNetworkSubnetConfig -Name "default" -VirtualNetwork $WestUSRecoveryVnet -AddressPrefix "10.0.0.0/20" | Set-AzVirtualNetwork

    $WestUSRecoveryNetwork = $WestUSRecoveryVnet.Id
   ```
- Birincil sanal ağ (sanal makine bağlı vnet) alma
   ```azurepowershell
    #Retrieve the virtual network that the virtual machine is connected to

    #Get first network interface card(nic) of the virtual machine
    $SplitNicArmId = $VM.NetworkProfile.NetworkInterfaces[0].Id.split("/")

    #Extract resource group name from the ResourceId of the nic
    $NICRG = $SplitNicArmId[4]

    #Extract resource name from the ResourceId of the nic
    $NICname = $SplitNicArmId[-1]

    #Get network interface details using the extracted resource group name and resource name
    $NIC = Get-AzNetworkInterface -ResourceGroupName $NICRG -Name $NICname

    #Get the subnet ID of the subnet that the nic is connected to
    $PrimarySubnet = $NIC.IpConfigurations[0].Subnet

    # Extract the resource ID of the Azure virtual network the nic is connected to from the subnet ID
    $EastUSPrimaryNetwork = (Split-Path(Split-Path($PrimarySubnet.Id))).Replace("\","/")  
   ```
- Birincil sanal ağ ve kurtarma sanal ağ arasında ağ eşlemesi oluşturma
   ```azurepowershell
    #Create an ASR network mapping between the primary Azure virtual network and the recovery Azure virtual network
    $TempASRJob = New-ASRNetworkMapping -AzureToAzure -Name "A2AEusToWusNWMapping" -PrimaryFabric $PrimaryFabric -PrimaryAzureNetworkId $EastUSPrimaryNetwork -RecoveryFabric $RecoveryFabric -RecoveryAzureNetworkId $WestUSRecoveryNetwork

    #Track Job status to check for completion
    while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
            sleep 10;
            $TempASRJob = Get-ASRJob -Job $TempASRJob
    }

    #Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
    Write-Output $TempASRJob.State

   ```
- Ters yönde (yeniden çalışma) için Ağ eşlemesi oluşturma
    ```azurepowershell
    #Create an ASR network mapping for failback between the recovery Azure virtual network and the primary Azure virtual network
    $TempASRJob = New-ASRNetworkMapping -AzureToAzure -Name "A2AWusToEusNWMapping" -PrimaryFabric $RecoveryFabric -PrimaryAzureNetworkId $WestUSRecoveryNetwork -RecoveryFabric $PrimaryFabric -RecoveryAzureNetworkId $EastUSPrimaryNetwork

    #Track Job status to check for completion
    while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
            sleep 10;
            $TempASRJob = Get-ASRJob -Job $TempASRJob
    }

    #Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
    Write-Output $TempASRJob.State
    ```

## <a name="replicate-azure-virtual-machine"></a>Azure sanal makine çoğaltma

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

# Data disk
$datadiskId1  = $vm.StorageProfile.DataDisks[0].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[0]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[0]. StorageAccountType

$DataDisk1ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId1 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig


#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId

```

Azure sanal makinesinin çoğaltma **yönetilmeyen diskler**.

```azurepowershell
#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#Disk replication configuration for the OS disk
$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -VhdUri $OSDiskVhdURI.Uri -LogStorageAccountId $EastUSCacheStorageAccount.Id -RecoveryAzureStorageAccountId $WestUSTargetStorageAccount.Id

#Disk replication configuration for data disk
$DataDisk1ReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -VhdUri $DataDisk1VhdURI.Uri -LogStorageAccountId $EastUSCacheStorageAccount.Id -RecoveryAzureStorageAccountId $WestUSTargetStorageAccount.Id

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.  
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){
        sleep 10;
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}


#Check if the Job completed successfully. The updated job state of a successfully completed job should be "Succeeded"
Write-Output $TempASRJob.State
```

Başlangıç çoğaltma işlemi başarılı olduktan sonra sanal makine verilerini kurtarma bölgeye çoğaltılır.

Başlangıçta bir kurtarma bölgesindeki sanal makinenin çoğaltılan diskleri kopyasını dengeli dağıtım çoğaltma işlemi başlatır. Bu aşama, ilk çoğaltma aşaması olarak adlandırılır.

İlk çoğaltma tamamlandıktan sonra çoğaltma fark eşitleme aşamaya taşır. Bu noktada, sanal makine korunur ve test yük devretme işlemi üzerinde gerçekleştirilebilir. İlk çoğaltma tamamlandıktan sonra çoğaltılan öğeyi temsil eden sanal makine çoğaltma durumu "Korumalı" durumuna geçer.

Çoğaltma durumu ve sanal makine için çoğaltma durumu kendisine karşılık gelen çoğaltma korumalı öğe ayrıntılarını alarak izleyin.
```azurepowershell
 Get-ASRReplicationProtectedItem -ProtectionContainer $PrimaryProtContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState ReplicationHealth
------------ --------------- -----------------
AzureDemoVM  Protected       Normal           
```

## <a name="perform-a-test-failover-validate-and-cleanup-test-failover"></a>Yük devretme testi gerçekleştirmek, doğrulama ve yük devretme testini Temizle

Sanal makine için çoğaltmayı korumalı bir duruma ulaştı. sonra bir sınama yük devretmesi işlemini sanal makinede (korumalı çoğaltma öğesinde sanal makinenin.) gerçekleştirilebilir

```azurepowershell
#Create a separate network for test failover (not connected to my DR network)
$TFOVnet = New-AzVirtualNetwork -Name "a2aTFOvnet" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -AddressPrefix "10.3.0.0/16"

Add-AzVirtualNetworkSubnetConfig -Name "default" -VirtualNetwork $TFOVnet -AddressPrefix "10.3.0.0/20" | Set-AzVirtualNetwork

$TFONetwork= $TFOVnet.Id
```

Yük devretme testi gerçekleştirin.
```azurepowershell
$ReplicationProtectedItem = Get-ASRReplicationProtectedItem -FriendlyName "AzureDemoVM" -ProtectionContainer $PrimaryProtContainer

$TFOJob = Start-ASRTestFailoverJob -ReplicationProtectedItem $ReplicationProtectedItem -AzureVMNetworkId $TFONetwork -Direction PrimaryToRecovery


```

Test yük devretme işleminin tamamlanması için bekleyin.
```azurepowershell
Get-ASRJob -Job $TFOJob
```

```
Name             : 3dcb043e-3c6d-4e0e-a42e-8d4245668547
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg/providers/Microsoft.RecoveryServices/vaults/a2aDemoR
                   ecoveryVault/replicationJobs/3dcb043e-3c6d-4e0e-a42e-8d4245668547
Type             : Microsoft.RecoveryServices/vaults/replicationJobs
JobType          : TestFailover
DisplayName      : Test failover
ClientRequestId  : 1ef8515b-b130-4452-a44d-91aaf071931c ActivityId: 907bb2bc-ebe6-4732-8b66-77d0546eaba8
State            : Succeeded
StateDescription : Completed
StartTime        : 4/25/2018 4:29:43 AM
EndTime          : 4/25/2018 4:33:06 AM
TargetObjectId   : ce86206c-bd78-53b4-b004-39b722c1ac3a
TargetObjectType : ProtectionEntity
TargetObjectName : azuredemovm
AllowedActions   :
Tasks            : {Prerequisites check for test failover, Create test virtual machine, Preparing the virtual machine, Start the virtual machine}
Errors           : {}
```
Test yük devretme işi başarıyla tamamlandıktan sonra Yük devredilen sanal makinenin başarısız test bağlanmak ve test yük devretmesi doğrulayın.

Temizleme başlatarak temiz test Kopyala'kurmak test sanal makinesi üzerinde test tamamlandıktan sonra Yük devretme işlemini test edin. Bu işlem, yük devretme testi tarafından oluşturulan sanal makinenin test kopyasını siler.

```azurepowershell
$Job_TFOCleanup = Start-ASRTestFailoverCleanupJob -ReplicationProtectedItem $ReplicationProtectedItem

Get-ASRJob -Job $Job_TFOCleanup | Select State
```
```
State    
-----    
Succeeded
```

## <a name="failover-to-azure"></a>Azure'a yük devretme

Belirli bir kurtarma noktasını sanal makineye yük devretme.

```azurepowershell
$RecoveryPoints = Get-ASRRecoveryPoint -ReplicationProtectedItem $ReplicationProtectedItem

#The list of recovery points returned may not be sorted chronologically and will need to be sorted first, in order to be able to find the oldest or the latest recovery points for the virtual machine.
"{0} {1}" -f $RecoveryPoints[0].RecoveryPointType, $RecoveryPoints[-1].RecoveryPointTime
```
```
CrashConsistent 4/24/2018 11:10:25 PM
```


```azurepowershell
#Start the failover job
$Job_Failover = Start-ASRUnplannedFailoverJob -ReplicationProtectedItem $ReplicationProtectedItem -Direction PrimaryToRecovery -RecoveryPoint $RecoveryPoints[-1]

do {
        $Job_Failover = Get-ASRJob -Job $Job_Failover;
        sleep 30;
} while (($Job_Failover.State -eq "InProgress") -or ($JobFailover.State -eq "NotStarted"))

$Job_Failover.State
```
```
Succeeded
```

Başarıyla yük devretti sonra Yük devretme işlemini yürütebilirsiniz.
```azurepowershell
$CommitFailoverJOb = Start-ASRCommitFailoverJob -ReplicationProtectedItem $ReplicationProtectedItem

Get-ASRJOb -Job $CommitFailoverJOb
```

```
Name             : 58afc2b7-5cfe-4da9-83b2-6df358c6e4ff
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg/providers/Microsoft.RecoveryServices/vaults/a2aDemoR
                   ecoveryVault/replicationJobs/58afc2b7-5cfe-4da9-83b2-6df358c6e4ff
Type             : Microsoft.RecoveryServices/vaults/replicationJobs
JobType          : CommitFailover
DisplayName      : Commit
ClientRequestId  : 10a95d6c-359e-4603-b7d9-b7ee3317ce94 ActivityId: 8751ada4-fc42-4238-8de6-a82618408fcf
State            : Succeeded
StateDescription : Completed
StartTime        : 4/25/2018 4:50:58 AM
EndTime          : 4/25/2018 4:51:01 AM
TargetObjectId   : ce86206c-bd78-53b4-b004-39b722c1ac3a
TargetObjectType : ProtectionEntity
TargetObjectName : azuredemovm
AllowedActions   :
Tasks            : {Prerequisite check, Commit}
Errors           : {}
```

## <a name="reprotect-and-failback-to-source-region"></a>Yeniden koruma ve yeniden çalışma için kaynak bölgesi

Özgün bölgesiyle geri dönmek hazır olduğunuzda, bir yük devrinden sonra güncelleştirme AzRecoveryServicesAsrProtectionDirection cmdlet'ini kullanarak çoğaltma korumalı öğe için çoğaltmayı tersine çevirmeyi Başlat.

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$WestUSCacheStorageAccount = New-AzStorageAccount -Name "a2acachestoragewestus" -ResourceGroupName "A2AdemoRG" -Location 'West US' -SkuName Standard_LRS -Kind Storage
```

```azurepowershell
#Use the recovery protection container, new cache storage accountin West US and the source region VM resource group
Update-AzRecoveryServicesAsrProtectionDirection -ReplicationProtectedItem $ReplicationProtectedItem -AzureToAzure
-ProtectionContainerMapping $RecoveryProtContainer -LogStorageAccountId $WestUSCacheStorageAccount.Id -RecoveryResourceGroupID $sourceVMResourcegroup.Id
```

Yeniden koruma tamamlandıktan sonra Yük devretme ters yönde (Doğu ABD Batı ABD) ve kaynak bölgeye yeniden başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Görünüm [Azure Site Recovery PowerShell başvurusu](https://docs.microsoft.com/powershell/module/az.RecoveryServices) kurtarma planları oluşturma ve PowerShell üzerinden kurtarma planı yük devretme testi gibi diğer görevleri nasıl gerçekleştirebileceğiniz öğrenin.
