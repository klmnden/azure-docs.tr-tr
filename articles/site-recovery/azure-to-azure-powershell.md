---
title: Azure Site Recovery - Kurulum ve test Azure PowerShell kullanarak Azure sanal makineleri için olağanüstü durum kurtarma | Microsoft Docs
description: Azure PowerShell kullanarak Azure Site Recovery ile Azure sanal makineleri için olağanüstü durum kurtarma ayarlanacağını öğrenin.
services: site-recovery
author: bsiva
manager: abhemraj
editor: raynew
ms.service: site-recovery
ms.topic: article
ms.date: 05/31/2018
ms.author: bsiva
ms.openlocfilehash: 3fa9ee27a1b9717d8011b7b46a1116f1f1ac1df5
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34716338"
---
# <a name="set-up-disaster-recovery-for-azure-virtual-machines-using-azure-powershell"></a>Olağanüstü durum kurtarma için Azure PowerShell kullanarak Azure sanal makineleri ayarlama

Bu makalede, Kurulum ve test etme gördüğünüz Azure PowerShell kullanarak Azure sanal makineleri için olağanüstü durum kurtarma. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> - Kurtarma Hizmetleri kasası oluşturun.
> - Kasa bağlamı için PowerShell oturumu ayarlayın.
> - Azure sanal makineleri çoğaltma işlemi başlatma kasasının hazırlayın.
> - Ağ eşleşmelerini oluşturun.
> - Sanal makineleri çoğaltmak için depolama hesapları oluşturun.
> - Azure sanal makineleri kurtarma bölge olağanüstü durum kurtarma için çoğaltılır.
> - Yük devretme testi gerçekleştirmek, doğrulama ve temizleme yük devretme sınaması.
> - Yük devretme kurtarma bölge.

> [!NOTE]
> Portalı aracılığıyla kullanılabilen tüm senaryosu özellikleri Azure PowerShell aracılığıyla bulunabilir. Şu anda Azure PowerShell aracılığıyla desteklenmeyen senaryo özelliklerden bazıları şunlardır:
> - Yönetilen diskler kullanan Azure sanal makinelerini çoğaltma olanağı.
> - Bir sanal makinenin tüm diskleri sanal makinenin her disk açıkça belirtmek zorunda kalmadan çoğaltılacak belirtme olanağı.  

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](azure-to-azure-support-matrix.md) gözden geçirin.
- Sürüm 5.7.0 veya AzureRm PowerShell modülünün daha büyük. Yüklemek veya Azure PowerShell yükseltmek gerekiyorsa, bu izleyin [Azure PowerShell'i yükleme ve yapılandırma için kılavuz](/powershell/azureps-cmdlets-docs).

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğiniz için oturum açın

Connect-AzureRmAccount cmdlet'i kullanılarak Azure aboneliğinizde oturum açın

```azurepowershell
Connect-AzureRmAccount
```
Azure aboneliğinizi seçin. Erişim için sahip olduğunuz Azure abonelikleri listesini almak için Get-AzureRmSubscription cmdlet'ini kullanın. Select-AzureRmSubscription cmdlet'ini kullanarak ile çalışmak için Azure aboneliğini seçin.

```azurepowershell
Select-AzureRmSubscription -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

## <a name="get-details-of-the-virtual-machines-to-be-replicated"></a>Çoğaltılacak sanal makineleri ayrıntılarını alma

Bu makalede bir örnekte, Doğu ABD bölgesindeki bir sanal makine çoğaltılan ve olması Batı ABD 2 bölgede kurtarıldı. Çoğaltılmasını sanal makine bir tek veri diski ve işletim sistemi diski ile bir sanal makinedir. AzureDemoVM örnekte kullanılan sanal makinenin adıdır.

```azurepowershell
# Get details of the virtual machine
$VM = Get-AzureRmVM -ResourceGroupName "A2AdemoRG" -Name "AzureDemoVM"

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

Disk, sanal makine diskleri için ayrıntıları. Disk ayrıntıları, daha sonra sanal makine için çoğaltma başlatırken kullanılır.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası oluşturmak bir kaynak grubu oluşturun. 

> [!IMPORTANT]
> * Kurtarma Hizmetleri kasası ve korunan, sanal makineleri farklı Azure konumlarda olması gerekir.
> * Kaynak grubu, Kurtarma Hizmetleri kasası ve korunan, sanal makineleri farklı Azure konumlarda olması gerekir.
> * Kurtarma Hizmetleri kasası ve ait olduğu, kaynak grubu aynı Azure konumuna olabilir.
 
Bu makalede örnekte korunan sanal makine Doğu ABD bölgesinde ' dir. Olağanüstü durum kurtarma için seçilen kurtarma bölge Batı ABD 2 bölgedir. Kurtarma Hizmetleri kasası, kasasının kaynak grubu hem de (Batı ABD 2) kurtarma bölgede olması

```azurepowershell
#Create a resource group for the recovery services vault in the recovery Azure region
New-AzureRmResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"
```
```
ResourceGroupName : a2ademorecoveryrg
Location          : westus2
ProvisioningState : Succeeded
Tags              : 
ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/a2ademorecoveryrg
```
   
Bir kurtarma Hizmetleri kasası oluşturun. A2aDemoRecoveryVault adlı bir kurtarma Hizmetleri kasası aşağıdaki örnekte Batı ABD 2 bölgede oluşturulur.

```azurepowershell
#Create a new Recovery services vault in the recovery region
$vault = New-AzureRmRecoveryServicesVault -Name "a2aDemoRecoveryVault" -ResourceGroupName "a2ademorecoveryrg" -Location "West US 2"

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
> Azure Site Recovery PowerShell Modülü (AzureRm.RecoveryServices.SiteRecovery Modülü) çoğu cmdlet'leri için kullanımı kolay diğer adları ile birlikte gelir. Modüldeki cmdlet'ler biçiminde  *\<işlemi >-**AzureRmRecoveryServicesAsr**\<nesnesi >* ve biçiminde eşdeğer diğer adlar  *\<İşlemi >-**ASR**\<nesnesi >*. Bu makalede, Okuma Kolaylığı için cmdlet diğer adlar kullanılmaktadır.

Kasa içeriği kullanmak için PowerShell oturumunda ayarlayın. Bunu yapmak için kasa ayarları dosyasına indirin ve kasa bağlam ayarlamak için PowerShell oturumunda indirilen dosya alma. 

Bir kez ayarlandıktan sonra sonraki Azure Site kurtarma işlemleri PowerShell oturumunda seçilen kasa bağlamında gerçekleştirilir. 

 ```azurepowershell
#Download the vault settings file for the vault.
$Vaultsettingsfile = Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteRecovery -Path C:\users\user\Documents\

#Import the downloaded vault settings file to set the vault context for the PowerShell session.
Import-AzureRmRecoveryServicesAsrVaultSettingsFile -Path $Vaultsettingsfile.FilePath

```
```
ResourceName         ResourceGroupName ResourceNamespace          ResouceType
------------         ----------------- -----------------          -----------
a2aDemoRecoveryVault a2ademorecoveryrg Microsoft.RecoveryServices Vaults     
```

```azurepowershell
#Delete the downloaded vault settings file
Remove-Item -Path $Vaultsettingsfile.FilePath
```
## <a name="prepare-the-vault-to-start-replicating-azure-virtual-machines"></a>Azure sanal makineleri çoğaltma işlemi başlatma kasasının hazırlama

####<a name="1-create-a-site-recovery-fabric-object-to-represent-the-primarysource-region"></a>1. Primary(source) bölge temsil etmek için bir Site Recovery doku nesnesi oluşturun

Kasa doku nesnesinde bir Azure bölgesi temsil eder. Birincil doku nesnesidir kasaya korunan sanal makineleri ait Azure bölgesini temsil etmek üzere oluşturulan doku nesnesi. Bu makalede örnekte korunan sanal makine Doğu ABD bölgesinde ' dir.

> [!NOTE]
> Azure Site Recovery işlemlerini zaman uyumsuz olarak çalıştırılır. Bir işlem başlattığınızda, bir Azure Site Recovery iş gönderildiğinde ve izleme nesnesi bir işi döndürülür. İzleme nesnesi işi (Get-ASRJob) iş için en son durumunu almak için ve işlemin durumunu izlemek için kullanın.

```azurepowershell
#Create Primary ASR fabric
$TempASRJob = New-ASRFabric -Azure -Location 'East US'  -Name "A2Ademo-EastUS" 

# Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        #If the job hasn't completed, sleep for 10 seconds before checking the job status again
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State

$PrimaryFabric = Get-AsrFabric -Name "A2Ademo-EastUS"
```
Aynı kasaya birçok Azure bölgesinde sanal makinelerden korumalı varsa, her kaynağı Azure bölgesi için bir doku nesnesi oluşturun.

####<a name="2-create-a-site-recovery-fabric-object-to-represent-the-recovery-region"></a>2. Kurtarma bölgeyi temsil etmek için bir Site Recovery doku nesnesi oluşturun

Kurtarma doku nesne kurtarma Azure konumu temsil eder. Sanal makineler için çoğaltılır ve kurtarma fabric tarafından temsil edilen kurtarma bölge kurtarılmadan (bir yük devretme durumunda). Bu örnekte kullanılan Azure bölgesi kurtarma Batı ABD 2 ' dir.

```azurepowershell
#Create Recovery ASR fabric
$TempASRJob = New-ASRFabric -Azure -Location 'West US 2'  -Name "A2Ademo-WestUS" 

# Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State

$RecoveryFabric = Get-AsrFabric -Name "A2Ademo-WestUS"

```

####<a name="3-create-a-site-recovery-protection-container-in-the-primary-fabric"></a>3. Birincil yapıda Site Recovery koruma kapsayıcısı oluşturma

Koruma kapsayıcısı bir yapı içinde çoğaltılmış öğeleri gruplandırmak için kullanılan bir kapsayıcıdır.

```azurepowershell
#Create a Protection container in the primary Azure region (within the Primary fabric)
$TempASRJob = New-AzureRmRecoveryServicesAsrProtectionContainer -InputObject $PrimaryFabric -Name "A2AEastUSProtectionContainer"

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

Write-Output $TempASRJob.State

$PrimaryProtContainer = Get-ASRProtectionContainer -Fabric $PrimaryFabric -Name "A2AEastUSProtectionContainer"
```
####<a name="4-create-a-site-recovery-protection-container-in-the-recovery-fabric"></a>4. Kurtarma yapıda Site Recovery koruma kapsayıcısı oluşturma

```azurepowershell
#Create a Protection container in the recovery Azure region (within the Recovery fabric)
$TempASRJob = New-AzureRmRecoveryServicesAsrProtectionContainer -InputObject $RecoveryFabric -Name "A2AWestUSProtectionContainer"

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"

Write-Output $TempASRJob.State

$RecoveryProtContainer = Get-ASRProtectionContainer -Fabric $RecoveryFabric -Name "A2AWestUSProtectionContainer"
```

####<a name="5-create-a-replication-policy"></a>5. Çoğaltma ilkesi oluşturma

```azurepowershell
#Create replication policy
$TempASRJob = New-ASRPolicy -AzureToAzure -Name "A2APolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State

$ReplicationPolicy = Get-ASRPolicy -Name "A2APolicy"
```
####<a name="6-create-a-protection-container-mapping-between-the-primary-and-recovery-protection-container"></a>6. Birincil ve kurtarma koruma kapsayıcısında koruma kapsayıcısı eşlemesini oluşturma

Koruma kapsayıcısı eşlemesini birincil koruma kapsayıcısı'Kurtarma koruma kapsayıcısı ve bir çoğaltma ilkesi ile eşler. Koruma kapsayıcısı çifti arasında sanal makineleri çoğaltmak için kullanacağınız her bir çoğaltma ilkesi için bir eşleme oluşturun.

```azurepowershell
#Create Protection container mapping between the Primary and Recovery Protection Containers with the Replication policy
$TempASRJob = New-ASRProtectionContainerMapping -Name "A2APrimaryToRecovery" -Policy $ReplicationPolicy -PrimaryProtectionContainer $PrimaryProtContainer -RecoveryProtectionContainer $RecoveryProtContainer

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State

$EusToWusPCMapping = Get-ASRProtectionContainerMapping -ProtectionContainer $PrimaryProtContainer -Name "A2APrimaryToRecovery"
```

####<a name="7-create-a-protection-container-mapping-for-failback-reverse-replication-after-a-failover"></a>7. Koruma kapsayıcısı eşlemesini yeniden çalışma (yük devretme sonrasında çoğaltmayı tersine çevirme) için oluşturma

Sanal makine üzerinde geri özgün bir Azure bölgesine, yeniden çalışma başarısız getirmek hazır olduğunuzda, bir yük devretme sonrasında. Yeniden çalışma, başarısız için başarısız çoğaltılan sanal makine üzerinde ters olan özgün bölge bölgesine üzerinden. Çoğaltmayı tersine çevirme için özgün bölgesi ve kurtarma bölgesi rollerini geçin. Özgün bölge şimdi yeni kurtarma bölgesi olur ve özgün kurtarma bölge şimdi neydi birincil bölge olur. Çoğaltmayı tersine çevirme için koruma kapsayıcısı eşlemesini özgün ve kurtarma bölgeler anahtarlı rollerini temsil eder.

```azurepowershell
#Create Protection container mapping (for failback) between the Recovery and Primary Protection Containers with the Replication policy 
$TempASRJob = New-ASRProtectionContainerMapping -Name "A2ARecoveryToPrimary" -Policy $ReplicationPolicy -PrimaryProtectionContainer $RecoveryProtContainer -RecoveryProtectionContainer $PrimaryProtContainer

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}

#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State
```

## <a name="create-cache-storage-accounts-and-target-storage-accounts"></a>Önbellek depolama hesapları ve hedef depolama hesapları oluşturma

Önbellek depolama hesabı aynı Azure bölgesinde çoğaltılmasını sanal makine olarak bir standart depolama hesabıdır. Önbellek depolama hesabı değişiklikleri kurtarma Azure bölgesi taşımadan önce çoğaltma değişikliklerini geçici olarak tutmak için kullanılır. (Ancak gerekli değildir) seçebileceğiniz bir sanal makinenin farklı diskler için farklı önbellek depolama hesaplarını belirtin.

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$EastUSCacheStorageAccount = New-AzureRmStorageAccount -Name "a2acachestorage" -ResourceGroupName "A2AdemoRG" -Location 'East US' -SkuName Standard_LRS -Kind Storage
```

Yönetilen diskleri kullanmayan sanal makineler için hedef depolama hesabı diskleri sanal makinenin çoğaltılan sanal kurtarma bölgede depolama hesapları ' dir. Hedef depolama hesabı, bir standart depolama hesabı ya da premium depolama hesabı olabilir. (G/ç yazma oranı) veri değişikliği hızını diskler ve depolama türü için Azure Site Recovery desteklenen karmaşası sınırları için temel gerekli depolama hesabı türünü seçin.

```azurepowershell
#Create Target storage account in the recovery region. In this case a Standard Storage account
$WestUSTargetStorageAccount = New-AzureRmStorageAccount -Name "a2atargetstorage" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -SkuName Standard_LRS -Kind Storage

```

## <a name="create-network-mappings"></a>Ağ eşlemeleri oluşturma

Ağ eşlemesi birincil bölge sanal ağlarda sanal ağlara kurtarma bölgede eşler. Ağ eşlemesini Azure sanal ağı yük devretme için birincil sanal ağ içindeki bir sanal makineye gereken kurtarma bölgede belirtir. Bir Azure sanal ağı yalnızca tek bir Azure sanal ağa bir kurtarma bölgede eşlenebilir.

- Yük devretme için kurtarma bölgede bir Azure sanal ağ oluşturma

   ```azurepowershell
    #Create a Recovery Network in the recovery region
    $WestUSRecoveryVnet = New-AzureRmVirtualNetwork -Name "a2arecoveryvnet" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -AddressPrefix "10.0.0.0/16"

    Add-AzureRmVirtualNetworkSubnetConfig -Name "default" -VirtualNetwork $WestUSRecoveryVnet -AddressPrefix "10.0.0.0/20" | Set-AzureRmVirtualNetwork

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

    #Get network interface details using the extracted resource group name and resourec name
    $NIC = Get-AzureRmNetworkInterface -ResourceGroupName $NICRG -Name $NICname

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
    
    #Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
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
        
    #Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
    Write-Output $TempASRJob.State
    ```

## <a name="replicate-azure-virtual-machine"></a>Azure sanal makinesi çoğaltma

Azure sanal makinesine çoğaltılır.

```azurepowershell
#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#Disk replication configuration for the OS disk
$OSDiskReplicationConfig = New-AzureRmRecoveryServicesAsrAzureToAzureDiskReplicationConfig -VhdUri $OSDiskVhdURI.Uri -LogStorageAccountId $EastUSCacheStorageAccount.Id -RecoveryAzureStorageAccountId $WestUSTargetStorageAccount.Id

#Disk replication configuration for data disk
$DataDisk1ReplicationConfig = New-AzureRmRecoveryServicesAsrAzureToAzureDiskReplicationConfig -VhdUri $DataDisk1VhdURI.Uri -LogStorageAccountId $EastUSCacheStorageAccount.Id -RecoveryAzureStorageAccountId $WestUSTargetStorageAccount.Id

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzureRmResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.  
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId

#Track Job status to check for completion
while (($TempASRJob.State -eq "InProgress") -or ($TempASRJob.State -eq "NotStarted")){ 
        sleep 10; 
        $TempASRJob = Get-ASRJob -Job $TempASRJob
}


#Check if the Job completed successfully. The updated job state of a successfuly completed job should be "Succeeded"
Write-Output $TempASRJob.State
```

Başlangıç çoğaltma işlemi başarılı olduktan sonra sanal makine verilerini kurtarma bölgeye çoğaltılır. 

Başlangıçta çoğaltılan diskleri sanal makinenin kurtarma bölgede bir kopyasını dengeli dağıtarak çoğaltma işlemini başlatır. Bu aşama, ilk çoğaltma aşaması olarak adlandırılır. 

İlk çoğaltma işlemi tamamlandıktan sonra çoğaltma için fark eşitleme aşaması taşır. Bu noktada, sanal makine korunuyor ve bir test yük devretme işlemi üzerinde gerçekleştirilebilir. İlk çoğaltma tamamlandıktan sonra sanal makineyi temsil eden çoğaltılmış öğesinin çoğaltma durumu "Korumalı" durumuna geçer.

Sanal makine için çoğaltma durumunu ve çoğaltma durumu kendisine karşılık gelen korumalı çoğaltma öğesinde ayrıntılarını alarak izleyin.
```azurepowershell
 Get-ASRReplicationProtectedItem -ProtectionContainer $PrimaryProtContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState ReplicationHealth
------------ --------------- -----------------
AzureDemoVM  Protected       Normal           
```

## <a name="perform-a-test-failover-validate-and-cleanup-test-failover"></a>Yük devretme testi gerçekleştirmek, doğrulama ve yük devretme testi temizleme

Sanal makinede (korumalı çoğaltma öğesinde sanal makinenin.) üzerinde çoğaltma sanal makine için korumalı bir duruma ulaştı sonra bir test yük devretme işlemi gerçekleştirilebilir

```azurepowershell
#Create a seperate network for test failover (not connected to my DR network)
$TFOVnet = New-AzureRmVirtualNetwork -Name "a2aTFOvnet" -ResourceGroupName "a2ademorecoveryrg" -Location 'West US 2' -AddressPrefix "10.3.0.0/16"

Add-AzureRmVirtualNetworkSubnetConfig -Name "default" -VirtualNetwork $TFOVnet -AddressPrefix "10.3.0.0/20" | Set-AzureRmVirtualNetwork

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
Test yük devretme işlemi başarıyla tamamlandığında, yükü devredilen sanal makine üzerinde test bağlanmak ve yük devretme sınaması doğrulayın.

Yedek temizleme başlatarak temiz test kopya sınama devredilen sanal makine üzerinde test tamamlandıktan sonra Yük devretme işlemi test edin. Bu işlem, yük devretme sınaması tarafından oluşturulan sanal makinenin test kopyasını siler.

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

Belirli bir kurtarma noktası için sanal makine yük devretme.

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

Başarılı bir şekilde devredilir sonra Yük devretme işlemini tamamlayabilir.
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

Özgün bölgesine geri dönmek hazır olduğunuzda, bir yük devretme sonrasında çoğaltmayı tersine çevirme için güncelleştirme AzureRmRecoveryServicesAsrProtectionDirection cmdlet'ini kullanarak korumalı çoğaltma öğesinde başlatın.

## <a name="next-steps"></a>Sonraki adımlar
Görünüm [Azure Site Recovery PowerShell başvurusu ](https://docs.microsoft.com/powershell/module/AzureRM.RecoveryServices.SiteRecovery) nasıl kurtarma planları oluşturma ve PowerShell aracılığıyla kurtarma planı yük devretme sınaması gibi diğer görevleri gerçekleştirebilir öğrenin.
