---
title: Çoğaltma ve VMware Vm'lerinde Azure Site Recovery PowerShell kullanarak Azure yük | Microsoft Docs
description: Çoğaltma ve yük devretme Azure PowerShell kullanarak Azure Site Recovery VMware VM'ler için nasıl ayarlanacağını öğrenin.
services: site-recovery
author: bsiva
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: bsiva
ms.openlocfilehash: 051bc3a0e1c0126826e96b49ff0a4e8008c88006
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287863"
---
# <a name="replicate-and-fail-over-vmware-vms-to-azure-with-powershell"></a>Çoğaltabilir ve VMware Vm'lerinde Azure PowerShell ile yük devri

Bu makalede, çoğaltma ve yük devretme VMware sanal makineleri Azure PowerShell kullanarak Azure konularına bakın. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> - Kurtarma Hizmetleri kasası oluşturun ve kasa bağlamını ayarlayın.
> - Sunucu kaydı kasadaki doğrulayın.
> - Bir çoğaltma ilkesi dahil, çoğaltma ayarlayın. VCenter server'ınızı ekleyin ve Vm'leri bulma. > - Bir vCenter sunucusu eklemek ve Bul 
> - Çoğaltma verileri tutmak için depolama hesapları oluşturabilir ve Vm'lerini çoğaltma.
> - Bir yük devretme gerçekleştirin. Yük devretme ayarlarını yapılandırın, sanal makineleri çoğaltmak için e ayarlarını gerçekleştirin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

- [Senaryo mimarisini ve bileşenlerini ](vmware-azure-architecture.md) anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-azure.md) gözden geçirin.
- Sürüm 5.0.1 veya AzureRm PowerShell modülünün daha büyük. Yüklemek veya Azure PowerShell yükseltmek gerekiyorsa, bu izleyin [Azure PowerShell'i yükleme ve yapılandırma için kılavuz](/powershell/azureps-cmdlets-docs).

## <a name="log-into-azure"></a>Azure'da oturum açın

Connect-AzureRmAccount cmdlet'i kullanılarak Azure aboneliğinize oturum:

```azurepowershell
Connect-AzureRmAccount
```
VMware sanal makinelerinizi çoğaltmak istediğiniz Azure aboneliğini seçin. Erişim için sahip olduğunuz Azure abonelikleri listesini almak için Get-AzureRmSubscription cmdlet'ini kullanın. Select-AzureRmSubscription cmdlet'ini kullanarak ile çalışmak için Azure aboneliğini seçin.

```azurepowershell
Select-AzureRmSubscription -SubscriptionName "ASR Test Subscription"
```
## <a name="set-up-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası ayarlama

1. Kurtarma Hizmetleri kasası oluşturmak bir kaynak grubu oluşturun. Aşağıdaki örnekte, kaynak grubu VMwareDRtoAzurePS olarak adlandırılır ve Doğu Asya bölgede oluşturulur.

   ```azurepowershell
   New-AzureRmResourceGroup -Name "VMwareDRtoAzurePS" -Location "East Asia"
   ```
   ```
   ResourceGroupName : VMwareDRtoAzurePS
   Location          : eastasia
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRtoAzurePS
```
   
2. Bir kurtarma Hizmetleri kasası oluşturun. Aşağıdaki örnekte, Kurtarma Hizmetleri kasası VMwareDRToAzurePs olarak adlandırılır ve Doğu Asya bölgesinde ve önceki adımda oluşturduğunuz kaynak grubunda oluşturulur.

   ```azurepowershell
   New-AzureRmRecoveryServicesVault -Name "VMwareDRToAzurePs" -Location "East Asia" -ResourceGroupName "VMwareDRToAzurePs"
   ```
   ```
   Name              : VMwareDRToAzurePs
   ID                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs
   Type              : Microsoft.RecoveryServices/vaults
   Location          : eastasia
   ResourceGroupName : VMwareDRToAzurePs
   SubscriptionId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
   Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
   ``` 

3. Kasa için kasa kayıt anahtarını indirin. Kasa kayıt anahtarını şirket içi yapılandırma sunucusu bu kasaya kaydetmek için kullanılır. Kayıt yapılandırma sunucusu yazılım yükleme işleminin bir parçası değil.

   ```azurepowershell
   #Get the vault object by name and resource group and save it to the $vault PowerShell variable 
   $vault = Get-AzureRmRecoveryServicesVault -Name "VMwareDRToAzurePS" -ResourceGroupName "VMwareDRToAzurePS"

   #Download vault registration key to the path C:\Work
   Get-AzureRmRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   ```
   ```
   FilePath
   --------
   C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials
   ```

4. İndirilen kasa kayıt anahtarını kullanın ve tam yükleme ve yapılandırma sunucusunun kaydı için aşağıda verilen makaleleri'ndaki adımları izleyin.
   - [Koruma hedeflerinizi seçme](vmware-azure-set-up-source.md#choose-your-protection-goals)
   - [Kaynak ortamını ayarlama](vmware-azure-set-up-source.md#set-up-the-configuration-server) 

### <a name="set-the-vault-context"></a>Kasa bağlamını ayarlayın

Set-ASRVaultContext cmdlet'ini kullanarak kasası bağlamını ayarlayın. Bir kez ayarlandıktan sonra sonraki Azure Site kurtarma işlemleri PowerShell oturumunda seçilen kasa bağlamında gerçekleştirilir.

> [!TIP]
> Azure Site Recovery PowerShell Modülü (AzureRm.RecoveryServices.SiteRecovery Modülü) çoğu cmdlet'leri için kullanımı kolay diğer adları ile birlikte gelir. Modüldeki cmdlet'ler biçiminde  *\<işlemi >-**AzureRmRecoveryServicesAsr**\<nesnesi >* ve biçiminde eşdeğer diğer adlar  *\<İşlemi >-**ASR**\<nesnesi >*. Bu makalede, Okuma Kolaylığı için cmdlet diğer adlar kullanılmaktadır.

Aşağıdaki örnekte, $vault kasası ayrıntılarının değişkeni PowerShell oturumu için kasa bağlamı belirtmek için kullanılır.

   ```azurepowershell
   Set-ASRVaultContext -Vault $vault
   ```
   ```
   ResourceName      ResourceGroupName ResourceNamespace          ResouceType
   ------------      ----------------- -----------------          -----------
   VMwareDRToAzurePs VMwareDRToAzurePs Microsoft.RecoveryServices vaults
   ```

Set-ASRVaultContext cmdlet'i alternatif olarak, kasası bağlam ayarlamak için alma AzureRmRecoveryServicesAsrVaultSettingsFile cmdlet de kullanabilirsiniz. Kasa kayıt anahtarı dosyasını Import-AzureRmRecoveryServicesAsrVaultSettingsFile cmdlet'i için path parametresi olarak bulunduğu olduğu yolunu belirtin. Örneğin:

   ```azurepowershell
   Get-AzureRmRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   Import-AzureRmRecoveryServicesAsrVaultSettingsFile -Path "C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials"
   ```
Bu makalenin sonraki bölümlerinde, Azure Site Recovery işlemlerini kasası bağlamının ayarlanmış varsayalım.

## <a name="validate-vault-registration"></a>Kasa kayıt doğrula

Bu örnek için şunları sunuyoruz:

- Yapılandırma sunucusu (**ConfigurationServer**) bu kasaya kayıtlı.
- Bir ek işlem sunucusu (**genişletme dosya**) için kayıtlı *ConfigurationServer*
- Hesapları (**vCenter_account**, **WindowsAccount**, **LinuxAccount**) yapılandırma sunucusunda ayarlanmış. Bu hesapları, sanal makineleri bulmak ve göndermeli yükleme mobility hizmeti yazılım sunucularında çoğaltılacak olan Windows ve Linux için vCenter sunucusu eklemek için kullanılır.

1. Kayıtlı yapılandırma sunucuları, Site Recovery doku nesnesinde temsil edilir. Doku listesini nesneleri kasaya almak ve yapılandırma sunucusu belirleyin.

   ```azurepowershell
   # Verify that the Configuration server is successfully registered to the vault
   $ASRFabrics = Get-ASRFabric
   $ASRFabrics.count
   ```
   ```
   1
   ```
   ```azurepowershell
   #Print details of the Configuration Server
   $ASRFabrics[0]
   ```
   ```
   Name                  : 2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
   FriendlyName          : ConfigurationServer
   ID                    : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationFabrics
                           /2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
   Type                  : Microsoft.RecoveryServices/vaults/replicationFabrics
   FabricType            : VMware
   SiteIdentifier        : ef7a1580-f356-4a00-aa30-7bf80f952510
   FabricSpecificDetails : Microsoft.Azure.Commands.RecoveryServices.SiteRecovery.ASRVMWareSpecificDetails
   ```

2. Makineleri çoğaltmak için kullanılan işlem sunucularını belirleyin.

   ```azurepowershell
   $ProcessServers = $ASRFabrics[0].FabricSpecificDetails.ProcessServers
   for($i=0; $i -lt $ProcessServers.count; $i++) {
    "{0,-5} {1}" -f $i, $ProcessServers[$i].FriendlyName
   }
   ```
   ```
   0     ScaleOut-ProcessServer
   1     ConfigurationServer
   ```

   Yukarıdaki çıktıya ***$ProcessServers [0]*** karşılık gelen *genişletme dosya* ve ***$ProcessServers [1]*** işlemsunucusurolüiçinkarşılıkgelen*ConfigurationServer*

3. Yapılandırma sunucusunda ayarlanmış hesapları tanımlayın.

   ```azurepowershell
   $AccountHandles = $ASRFabrics[0].FabricSpecificDetails.RunAsAccounts
   #Print the account details
   $AccountHandles
   ```
   ```
   AccountId AccountName
   --------- -----------
   1         vCenter_account
   2         WindowsAccount
   3         LinuxAccount
   ```

   Yukarıdaki çıktıya ***$AccountHandles [0]*** hesaba karşılık gelen *vCenter_account*, ***$AccountHandles [1]*** hesabına *WindowsAccount*, ve ***$AccountHandles [2]*** hesabına *LinuxAccount*

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Bu adımda, iki çoğaltma ilkesi oluşturulur. Azure ve diğer çoğaltmak için VMware sanal makineleri çoğaltmak için bir ilke başarısız üzerinden Azure'da çalışan sanal makineler şirket içi VMware siteye yedekleyin.

> [!NOTE]
> Azure Site kurtarma işlemlerinin çoğu zaman uyumsuz olarak çalıştırılır. Bir işlem başlattığınızda, bir Azure Site Recovery iş gönderildiğinde ve izleme nesnesi bir işi döndürülür. İzleme nesnesi bu iş, işlemin durumunu izlemek için kullanılabilir.

1. Adlı bir çoğaltma ilkesi oluşturmak *ReplicationPolicy* VMware sanal makineleri belirtilen özelliklerle Azure'a çoğaltmak için.

   ```azurepowershell
   $Job_PolicyCreate = New-ASRPolicy -VMwareToAzure -Name "ReplicationPolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60

   # Track Job status to check for completion
   while (($Job_PolicyCreate.State -eq "InProgress") -or ($Job_PolicyCreate.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_PolicyCreate = Get-ASRJob -Job $Job_PolicyCreate
   }

   #Display job status
   $Job_PolicyCreate
   ```
   ```
   Name             : 8d18e2d9-479f-430d-b76b-6bc7eb2d0b3e
   ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/8d18e2d
                      9-479f-430d-b76b-6bc7eb2d0b3e
   Type             :
   JobType          : AddProtectionProfile
   DisplayName      : Create replication policy
   ClientRequestId  : a162b233-55d7-4852-abac-3d595a1faac2 ActivityId: 9895234a-90ea-4c1a-83b5-1f2c6586252a
   State            : Succeeded
   StateDescription : Completed
   StartTime        : 11/24/2017 2:49:24 AM
   EndTime          : 11/24/2017 2:49:23 AM
   TargetObjectId   : ab31026e-4866-5440-969a-8ebcb13a372f
   TargetObjectType : ProtectionProfile
   TargetObjectName : ReplicationPolicy
   AllowedActions   :
   Tasks            : {Prerequisites check for creating the replication policy, Creating the replication policy}
   Errors           : {}
   ```

2. Azure'dan şirket içi VMware sitesi için kullanılmak üzere bir çoğaltma ilkesi oluşturun.

   ```azurepowershell
   $Job_FailbackPolicyCreate = New-ASRPolicy -AzureToVMware -Name "ReplicationPolicy-Failback" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60
   ```

   İş ayrıntılarını kullanmak *$Job_FailbackPolicyCreate* işlem tamamlanıncaya kadar izlemek için.

   * Yapılandırma sunucusu çoğaltma ilkeleriyle eşleştirmek için koruma kapsayıcısı eşlemesini oluşturun.

   ```azurepowershell
   #Get the protection container corresponding to the Configuration Server
   $ProtectionContainer = Get-ASRProtectionContainer -Fabric $ASRFabrics[0]

   #Get the replication policies to map by name.
   $ReplicationPolicy = Get-ASRPolicy -Name "ReplicationPolicy"
   $FailbackReplicationPolicy = Get-ASRPolicy -Name "ReplicationPolicy-Failback"

   # Associate the replication policies to the protection container corresponding to the Configuration Server. 

   $Job_AssociatePolicy = New-ASRProtectionContainerMapping -Name "PolicyAssociation1" -PrimaryProtectionContainer $ProtectionContainer -Policy $ReplicationPolicy

   # Check the job status
   while (($Job_AssociatePolicy.State -eq "InProgress") -or ($Job_AssociatePolicy.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_AssociatePolicy = Get-ASRJob -Job $Job_AssociatePolicy
   }
   $Job_AssociatePolicy.State

   <# In the protection container mapping used for failback (replicating failed over virtual machines 
      running in Azure, to the primary VMware site.) the protection container corresponding to the 
      Configuration server acts as both the Primary protection container and the recovery protection
      container
   #>
    $Job_AssociateFailbackPolicy = New-ASRProtectionContainerMapping -Name "FailbackPolicyAssociation" -PrimaryProtectionContainer $ProtectionContainer -RecoveryProtectionContainer $ProtectionContainer -Policy $FailbackReplicationPolicy

   # Check the job status
   while (($Job_AssociateFailbackPolicy.State -eq "InProgress") -or ($Job_AssociateFailbackPolicy.State -eq "NotStarted")){ 
           sleep 10; 
           $Job_AssociateFailbackPolicy = Get-ASRJob -Job $Job_AssociateFailbackPolicy
   }
   $Job_AssociateFailbackPolicy.State

   ```

## <a name="add-a-vcenter-server-and-discover-vms"></a>Bir vCenter sunucusu eklemek ve sanal makineleri Bul

IP adresi veya ana bilgisayar adı bir vCenter sunucusu ekleyin. **-Bağlantı noktası** parametresi bağlanmak için vCenter sunucusu üzerindeki bağlantı noktasını belirtir **-adı** parametresi, vCenter sunucusu için kullanılacak kolay bir ad belirtir ve **-hesap** parametresi, vCenter sunucusu tarafından yönetilen sanal makinelerin bulmak için kullanılacak yapılandırma sunucusundaki hesap tanıtıcı belirtir.

```azurepowershell
# The $AccountHandles[0] variable holds details of vCenter_account

$Job_AddvCenterServer = New-ASRvCenter -Fabric $ASRFabrics[0] -Name "MyvCenterServer" -IpOrHostName "10.150.24.63" -Account $AccountHandles[0] -Port 443

#Wait for the job to complete and ensure it completed successfully

while (($Job_AddvCenterServer.State -eq "InProgress") -or ($Job_AddvCenterServer.State -eq "NotStarted")) { 
        sleep 30; 
        $Job_AddvCenterServer = Get-ASRJob -Job $Job_AddvCenterServer
}
$Job_AddvCenterServer
```
```
Name             : 0f76f937-f9cf-4e0e-bf27-10c9d1c252a4
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/0f76f93
                   7-f9cf-4e0e-bf27-10c9d1c252a4
Type             :
JobType          : DiscoverVCenter
DisplayName      : Add vCenter server
ClientRequestId  : a2af8892-5686-4d64-a528-10445bc2f698 ActivityId: 7ec05aad-002e-4da0-991f-95d0de7a9f3a
State            : Succeeded
StateDescription : Completed
StartTime        : 11/24/2017 2:41:47 AM
EndTime          : 11/24/2017 2:44:37 AM
TargetObjectId   : 10.150.24.63
TargetObjectType : VCenter
TargetObjectName : MyvCenterServer
AllowedActions   :
Tasks            : {Adding vCenter server}
Errors           : {}
```

## <a name="create-storage-accounts-for-replication"></a>Çoğaltma için depolama hesabı oluşturma

Bu adımda, çoğaltma için kullanılacak depolama hesapları oluşturulur. Bu depolama hesaplarından daha sonra sanal makineleri çoğaltmak için kullanılır. Depolama hesapları kasa ile aynı Azure bölgesinde oluşturduğunuzdan emin olun. Çoğaltma için mevcut bir depolama hesabını kullanmayı planlıyorsanız, bu adımı atlayabilirsiniz.

> [!NOTE]
> Premium depolama hesabı için şirket içi sanal makineleri çoğaltılırken, ek bir standart depolama hesabı (günlük depolama hesabı) belirtmeniz gerekir. Premium Depolama Hedef günlükleri uygulanabilir kadar günlük depolama hesabı çoğaltma günlükleri bir ara deposu olarak tutar.
>

```azurepowershell

$PremiumStorageAccount = New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "premiumstorageaccount1" -Location "East Asia" -SkuName Premium_LRS

$LogStorageAccount = New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "logstorageaccount1" -Location "East Asia" -SkuName Standard_LRS

$ReplicationStdStorageAccount= New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "replicationstdstorageaccount1" -Location "East Asia" -SkuName Standard_LRS
```

## <a name="replicate-vmware-vms"></a>VMware Vm'lerini çoğaltma

VCenter sunucudan bulunmak sanal makineler için yaklaşık 15-20 dakika sürer. Bulunan bir kez korunabilir öğe nesnesi bulunan her sanal makine için Azure Site Recovery oluşturulur. Bu adımda, önceki adımda oluşturduğunuz Azure depolama hesapları için üç bulunan sanal makinelerin çoğaltılır.

Bulunan bir sanal makineyi korumak için aşağıdaki ayrıntıları gerekir:

* Çoğaltılacak korunabilir öğe.
* Sanal makine için çoğaltmak için depolama hesabı. Ayrıca, bir günlük depolama, premium depolama hesabı için sanal makineleri koruma için gereklidir.
* İşlem çoğaltma için kullanılacak sunucu. Kullanılabilir işlem sunucularının listesini alınan ve kaydedilen ***$ProcessServers [0]****(dosya genişletme)* ve ***$ProcessServers [1]*** *(ConfigurationServer)* değişkenleri.
* Mobility hizmeti yazılım makinelere göndermeli yükleme için kullanılacak hesabı. Kullanılabilir hesaplar listesindeki alınan ve depolanan ***$AccountHandles*** değişkeni.
* Koruma kapsayıcısı eşlemesini çoğaltma için kullanılacak çoğaltma ilkesi için.
* Kaynak grubu sanal makine yük devretme oluşturulması gerekir.
* İsteğe bağlı olarak, Azure sanal ağ ve alt ağı başarısız sanal makine bağlanması gerekir.

Şimdi bu tabloda belirtilen ayarlarla şu sanal makineleri çoğaltmak


|Sanal makine  |İşlem sunucusu        |Depolama Hesabı              |Günlük depolama hesabı  |İlke           |Mobility hizmeti yükleme hesabı|Hedef kaynak grubu  | Hedef sanal ağ  |Hedef alt ağ  |
|-----------------|----------------------|-----------------------------|---------------------|-----------------|-----------------------------------------|-----------------------|-------------------------|---------------|
|Win2K12VM1       |Genişletme dosya|premiumstorageaccount1       |logstorageaccount1   |ReplicationPolicy|WindowsAccount                           |VMwareDRToAzurePs      |ASR sanal ağ                 |Alt ağ 1       |
|CentOSVM1       |ConfigurationServer   |replicationstdstorageaccount1| Yok                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |ASR sanal ağ                 |Alt ağ 1       |   
|CentOSVM2       |ConfigurationServer   |replicationstdstorageaccount1| Yok                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |ASR sanal ağ                 |Alt ağ 1       |   

 
```azurepowershell

#Get the target resource group to be used
$ResourceGroup = Get-AzureRmResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzureRmVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap  = Get-ASRProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine Win2K12VM1
$VM1 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "Win2K12VM1"

# Enable replication for virtual machine Win2K12VM1
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableRepication1 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $PremiumStorageAccount.Id -LogStorageAccountId $LogStorageAccount.Id -ProcessServer $ProcessServers[0] -Account $AccountHandles[1] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM2 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1
$Job_EnableRepication2 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM2 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

#Get the protectable item corresponding to the virtual machine CentOSVM2
$VM3 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM2"

# Enable replication for virtual machine CentOSVM2
$Job_EnableRepication3 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM3 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

```

Etkinleştirme çoğaltma işi başarıyla tamamlandıktan sonra ilk çoğaltma sanal makineler için başlatılır. İlk çoğaltma, çoğaltma için çoğaltılacak veri miktarı ve kullanılabilir bant genişliğine bağlı olarak biraz zaman alabilir. İlk çoğaltma tamamlandıktan sonra sanal makinenin korumalı bir duruma taşır. Sanal makine bir yük devretme sınaması için sanal makine gerçekleştirebileceğiniz korumalı bir duruma ulaştığında, Kurtarma planları vb. ekleyin.

Get-ASRReplicationProtectedItem cmdlet ile sanal makinenin çoğaltma durumunu ve çoğaltma durumunu kontrol edebilirsiniz.

```azurepowershell
Get-ASRReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState                 ReplicationHealth
------------ ---------------                 -----------------
CentOSVM1    Protected                       Normal
CentOSVM2    InitialReplicationInProgress    Normal
Win2K12VM1   Protected                       Normal
```

## <a name="configure-failover-settings"></a>Yük devretme ayarlarını yapılandırın

Korumalı makineler için yük devretme ayarları kümesi ASRReplicationProtectedItem cmdlet'ini kullanarak güncelleştirilebilir. Bu cmdlet'i güncelleştirilebilir ayarlardan bazıları şunlardır:
* Yük devretme oluşturulacak sanal makinenin adı
* Sanal makinenin Yük Devretmesini oluşturulacak VM boyutu
* Azure sanal ağ ve NIC'ler sanal makinenin Yük Devretmesini bağlanması gerekir alt
* Yönetilen diskleri için yük devretme
* Azure karma kullanımı avantajı Uygula
* Yük devretme sanal makineye atanacak hedef sanal ağlardan statik bir IP adresi atayın.

Bu örnekte, sanal makinenin sanal makine için yük devretme üzerinde oluşturulacak VM boyutu güncelleştiriyoruz *Win2K12VM1* ve yönetilen sanal makine kullanımını belirtin yük devretme disklerde.

```azurepowershell
$ReplicatedVM1 = Get-ASRReplicationProtectedItem -FriendlyName "Win2K12VM1" -ProtectionContainer $ProtectionContainer

Set-ASRReplicationProtectedItem -InputObject $ReplicatedVM1 -Size "Standard_DS11" -UseManagedDisk True
```
```
Name             : cafa459c-44a7-45b0-9de9-3d925b0e7db9
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/cafa459
                   c-44a7-45b0-9de9-3d925b0e7db9
Type             :
JobType          : UpdateVmProperties
DisplayName      : Update the virtual machine
ClientRequestId  : b0b51b2a-f151-4e9a-a98e-064a5b5131f3 ActivityId: ac2ba316-be7b-4c94-a053-5363f683d38f
State            : InProgress
StateDescription : InProgress
StartTime        : 11/24/2017 2:04:26 PM
EndTime          :
TargetObjectId   : 88bc391e-d091-11e7-9484-000c2955bb50
TargetObjectType : ProtectionEntity
TargetObjectName : Win2K12VM1
AllowedActions   :
Tasks            : {Update the virtual machine properties}
Errors           : {}
```

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. DR detayı (yük devretme testi) aşağıdaki gibi çalıştırın:

   ```azurepowershell
   #Test failover of Win2K12VM1 to the test virtual network "V2TestNetwork"

   #Get details of the test failover virtual network to be used
   TestFailovervnet = Get-AzureRmVirtualNetwork -Name "V2TestNetwork" -ResourceGroupName "asrrg" 

   #Start the test failover operation
   $TFOJob = Start-ASRTestFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -AzureVMNetworkId $TestFailovervnet.Id -Direction PrimaryToRecovery
   ```
2. Test yük devretme işlemi başarıyla tamamlandığında, bir sanal makine ile sonekine fark edeceksiniz *"-Test"* (Bu durumda Win2K12VM1 Test) adının Azure içinde oluşturulur.
3. Şimdi devredilen sanal makine üzerinde test bağlanmak ve yük devretme sınaması doğrulayın.
4. Start-ASRTestFailoverCleanupJob cmdlet'ini kullanarak yük devretmeyi temizleyin. Bu işlem, test yük devretme işleminin bir parçası olarak oluşturulan sanal makinesini siler.

   ```azurepowershell
   $Job_TFOCleanup = Start-ASRTestFailoverCleanupJob -ReplicationProtectedItem $ReplicatedVM1
   ```

## <a name="fail-over-to-azure"></a>Azure'a yük

Bu adımda, biz belirli bir kurtarma noktası sanal makineye Win2K12VM1 yük devri.

1. Yük devretme için kullanılacak kullanılabilir kurtarma noktalarının bir listesini alın:
   ```azurepowershell
   # Get the list of available recovery points for Win2K12VM1
   $RecoveryPoints = Get-ASRRecoveryPoint -ReplicationProtectedItem $ReplicatedVM1
   "{0} {1}" -f $RecoveryPoints[0].RecoveryPointType, $RecoveryPoints[0].RecoveryPointTime
   ```
   ```
   CrashConsistent 11/24/2017 5:28:25 PM
   ```
   ```azurepowershell

   #Start the failover job
   $Job_Failover = Start-ASRUnplannedFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -Direction PrimaryToRecovery -RecoveryPoint $RecoveryPoints[0]
   do {
           $Job_Failover = Get-ASRJob -Job $Job_Failover;
           sleep 60;
   } while (($Job_Failover.State -eq "InProgress") -or ($JobFailover.State -eq "NotStarted"))
   $Job_Failover.State
   ```
   ```
   Succeeded
   ```

2. Başarıyla, başarısız sonra Yük devretme işlemi yürütme ve azure'dan çoğaltmayı tersine çevirme ayarlayın şirket içi VMware siteye yedekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Kullanarak daha fazla görevleri otomatikleştirmeyi öğrenin [Azure Site Recovery PowerShell başvurusu ](https://docs.microsoft.com/powershell/module/AzureRM.RecoveryServices.SiteRecovery).
