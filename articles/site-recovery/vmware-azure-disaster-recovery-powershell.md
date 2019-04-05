---
title: Azure PowerShell kullanarak Azure Site Recovery'de VMware vm'lerinin olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Çoğaltma ve yük devretme için Azure PowerShell kullanarak Azure Site Recovery'de VMware vm'lerinin olağanüstü durum kurtarma ayarlama konusunda bilgi edinin.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.date: 11/27/2018
ms.topic: conceptual
ms.author: sutalasi
ms.openlocfilehash: d70f2b2f0afb99263eaefe1122dba565231d978c
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59046937"
---
# <a name="set-up-disaster-recovery-of-vmware-vms-to-azure-with-powershell"></a>Azure PowerShell ile VMware vm'lerinin olağanüstü durum kurtarmayı ayarlama

Bu makalede, çoğaltma ve yük devretme VMware sanal makinelerini Azure PowerShell kullanarak azure'a bakın. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> - Bir kurtarma Hizmetleri kasası oluşturun ve kasa bağlamını ayarlayın.
> - Sunucu kaydı kasadaki doğrulayın.
> - Çoğaltma, çoğaltma ilkesi dahil olmak üzere ayarlayın. VCenter server'ınızı ekleyin ve Vm'leri keşfedin. 
> - Bir vCenter sunucusu eklemek ve keşfedin 
> - Çoğaltılan verileri tutmak için depolama hesapları oluşturmanız ve sanal makinelerini çoğaltma.
> - Yük devretme gerçekleştirin. Yük devretme ayarlarını yapılandırma, sanal makineleri çoğaltmak için bir ayarı gerçekleştirin.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

- [Senaryo mimarisini ve bileşenlerini ](vmware-azure-architecture.md) anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-azure.md) gözden geçirin.
- Azure PowerShell'in `Az` modülü. Yüklemek veya Azure PowerShell yükseltmek gerekirse bu izleyin [Azure PowerShell'i yükleme ve yapılandırma için kılavuz](/powershell/azure/install-az-ps).

## <a name="log-into-azure"></a>Azure'da oturum açın

Azure aboneliğinize bağlanma AzAccount cmdlet'ini kullanarak oturum açın:

```azurepowershell
Connect-AzAccount
```
VMware sanal makinelerinizin çoğaltmak istediğiniz Azure aboneliğini seçin. Sahip olduğunuz Azure aboneliklerin listesi, erişim elde etmek için Get-AzSubscription cmdlet'ini kullanın. Select AzSubscription cmdlet'ini kullanarak çalışmaya Azure aboneliğini seçin.

```azurepowershell
Select-AzSubscription -SubscriptionName "ASR Test Subscription"
```
## <a name="set-up-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası ayarlama

1. Kurtarma Hizmetleri kasası oluşturmak bir kaynak grubu oluşturun. Aşağıdaki örnekte, kaynak grubunu VMwareDRtoAzurePS adlı ve Doğu Asya bölgesinde oluşturulur.

   ```azurepowershell
   New-AzResourceGroup -Name "VMwareDRtoAzurePS" -Location "East Asia"
   ```
   ```
   ResourceGroupName : VMwareDRtoAzurePS
   Location          : eastasia
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRtoAzurePS
   ```
   
2. Bir kurtarma Hizmetleri kasası oluşturun. Aşağıdaki örnekte, Kurtarma Hizmetleri kasası VMwareDRToAzurePs adlı ve Doğu Asya bölgesi ve önceki adımda oluşturduğunuz kaynak grubunda oluşturulur.

   ```azurepowershell
   New-AzRecoveryServicesVault -Name "VMwareDRToAzurePs" -Location "East Asia" -ResourceGroupName "VMwareDRToAzurePs"
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

3. Kasa için kasa kayıt anahtarını indirin. Kasa kayıt anahtarını, şirket içi yapılandırma sunucusunu bu kasaya kaydetmek için kullanılır. Kayıt, yapılandırma sunucusuna yazılım yükleme işleminin parçasıdır.

   ```azurepowershell
   #Get the vault object by name and resource group and save it to the $vault PowerShell variable 
   $vault = Get-AzRecoveryServicesVault -Name "VMwareDRToAzurePS" -ResourceGroupName "VMwareDRToAzurePS"

   #Download vault registration key to the path C:\Work
   Get-AzRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   ```
   ```
   FilePath
   --------
   C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials
   ```

4. İndirilen kasa kayıt anahtarını kullanın ve tam yükleme ve yapılandırma sunucusunun kaydı için aşağıda verilen makalelerindeki adımları izleyin.
   - [Koruma hedeflerinizi seçme](vmware-azure-set-up-source.md#choose-your-protection-goals)
   - [Kaynak ortamı ayarlama](vmware-azure-set-up-source.md#set-up-the-configuration-server) 

### <a name="set-the-vault-context"></a>Kasa bağlamını ayarlayın

Set-ASRVaultContext cmdlet'ini kullanarak kasa bağlamını ayarlayın. Bir kez ayarlandıktan sonra sonraki Azure Site Recovery işlemlerini bir PowerShell oturumunda seçili kasa bağlamında gerçekleştirilir.

> [!TIP]
> Azure Site Recovery PowerShell Modülü (Az.RecoveryServices Modülü), çoğu cmdlet için kullanımı kolay diğer adları ile birlikte gelir. Modüldeki cmdlet'ler biçiminde  *\<işlemi >-**AzRecoveryServicesAsr**\<Nesne >* ve biçiminde eşdeğer diğer adlar  *\< İşlem >-**ASR**\<Nesne >*. Bu makalede, kolay okunması için cmdlet diğer adları kullanır.

Aşağıdaki örnekte, kasa ayrıntıları $vault değişkeni kasa bağlamını PowerShell oturumunun belirtmek için kullanılır.

   ```azurepowershell
   Set-ASRVaultContext -Vault $vault
   ```
   ```
   ResourceName      ResourceGroupName ResourceNamespace          ResourceType
   ------------      ----------------- -----------------          -----------
   VMwareDRToAzurePs VMwareDRToAzurePs Microsoft.RecoveryServices vaults
   ```

Set-ASRVaultContext cmdlet'e alternatif olarak, bir de alma AzRecoveryServicesAsrVaultSettingsFile cmdlet kasa bağlamını ayarlamak için kullanabilirsiniz. Kasa kayıt anahtarı dosyasını içeri aktarma AzRecoveryServicesAsrVaultSettingsFile cmdlet'ine path parametresini olarak yer olduğu için yolu belirtin. Örneğin:

   ```azurepowershell
   Get-AzRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
   Import-AzRecoveryServicesAsrVaultSettingsFile -Path "C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials"
   ```
Bu makalenin sonraki bölümlerinde, Azure Site kurtarma işlemleri için kasa bağlamını ayarlamanız gerektiğini varsayalım.

## <a name="validate-vault-registration"></a>Kasa kayıt doğrulama

Bu örnek için şunları sunuyoruz:

- Yapılandırma sunucusunu (**ConfigurationServer**) bu kasaya kayıtlı.
- Bir ek işlem sunucusu (**genişletme dosya**) için kayıtlı *ConfigurationServer*
- Hesapları (**vCenter_account**, **WindowsAccount**, **LinuxAccount**) yapılandırma sunucusunda ayarlanmış. Bu hesaplar, sanal makineleri keşfetmek için ve çoğaltılması için Windows ve Linux sunucuları üzerinde mobility hizmet yazılımı göndermeli yükleme için vCenter server eklemek için kullanılır.

1. Kayıtlı bir yapılandırma sunucusu, Site recovery'de bir doku nesnesi tarafından temsil edilir. Fabric listesini nesneleri kasaya alma ve yapılandırma sunucusu belirleyin.

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

   Yukarıdaki çıktıda ***$ProcessServers [0]*** karşılık gelen *genişletme dosya* ve ***$ProcessServers [1]*** işlemsunucusurolünekarşılıkgelen*ConfigurationServer*

3. Yapılandırma sunucusunda ayarlanan hesapları tanımlayın.

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

   Yukarıdaki çıktıda ***$AccountHandles [0]*** hesaba karşılık gelen *vCenter_account*, ***$AccountHandles [1]*** hesabına *WindowsAccount*, ve ***$AccountHandles [2]*** hesabına *LinuxAccount*

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

Bu adımda, iki çoğaltma ilkeleri oluşturulur. Azure ve diğer çoğaltılacak VMware sanal makinelerini çoğaltma için bir ilke yük devretti Azure'da çalışan sanal makineleri yedeklemek için şirket içi VMware sitesi.

> [!NOTE]
> Çoğu Azure Site Recovery işlemlerini zaman uyumsuz olarak yürütülür. Bir işlem başlattığınızda, Azure Site Recovery iş gönderilir ve nesne izleme iş döndürülür. İzleme nesnesi bu proje, işlemin durumunu izlemek için kullanılabilir.

1. Adlı çoğaltma ilkesi oluşturma *ReplicationPolicy* VMware sanal makinelerini, belirtilen özelliklerle Azure'a çoğaltmak için.

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

2. Azure'dan şirket içi VMware sitesi için kullanılacak bir çoğaltma ilkesi oluşturun.

   ```azurepowershell
   $Job_FailbackPolicyCreate = New-ASRPolicy -AzureToVMware -Name "ReplicationPolicy-Failback" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60
   ```

   İş ayrıntılarında kullanın *$Job_FailbackPolicyCreate* işlemi tamamlanana kadar izlemek için.

   * Çoğaltma ilkeleri yapılandırma sunucusu ile eşlemek için bir koruma kapsayıcısı eşlemesi oluşturun.

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

## <a name="add-a-vcenter-server-and-discover-vms"></a>Bir vCenter sunucusu eklemek ve Vm'leri keşfedin

Bir ' % s'vCenter sunucusu tarafından IP adresi veya konak ekleyin. **-Bağlantı noktası** parametresi bağlanmak vCenter sunucusundaki bağlantı noktasını belirtir **-adı** parametresi, vCenter sunucusu için kullanılacak bir kolay adı belirtir ve **-hesap** parametresi vCenter sunucusu tarafından yönetilen sanal makineleri bulmak için kullanılacak yapılandırma sunucusu üzerindeki hesabı tanıtıcı belirtir.

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

## <a name="create-storage-accounts-for-replication"></a>Çoğaltma için depolama hesapları oluşturun

Bu adımda, çoğaltma için kullanılacak depolama hesabı oluşturulur. Bu depolama hesaplarından daha sonra sanal makineleri çoğaltmak için kullanılır. Depolama hesapları kasayla aynı Azure bölgesinde oluşturduğunuzdan emin olun. Çoğaltma için mevcut bir depolama hesabı kullanmayı planlıyorsanız, bu adımı atlayabilirsiniz.

> [!NOTE]
> Premium depolama hesabı için şirket içi sanal makineleri çoğaltılırken, ek bir standart depolama hesabı (günlük depolama hesabı) belirtmeniz gerekir. Günlükleri premium Depolama Hedef uygulanabilir kadar günlük depolama hesabına çoğaltma günlükleri Ara bir depo olarak korur.
>

```azurepowershell

$PremiumStorageAccount = New-AzStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "premiumstorageaccount1" -Location "East Asia" -SkuName Premium_LRS

$LogStorageAccount = New-AzStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "logstorageaccount1" -Location "East Asia" -SkuName Standard_LRS

$ReplicationStdStorageAccount= New-AzStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "replicationstdstorageaccount1" -Location "East Asia" -SkuName Standard_LRS
```

## <a name="replicate-vmware-vms"></a>VMware Vm'lerini çoğaltma

VCenter sunucusundaki bulunmak sanal makineler için yaklaşık 15-20 dakika sürer. Bulunan bir kez korunabilir öğe nesnesi bulunan her sanal makine için Azure Site Recovery'de oluşturulur. Bu adımda, önceki adımda oluşturduğunuz Azure depolama hesapları için üç bulunan sanal makinelerin çoğaltılır.

Bulunan bir sanal makineyi korumak için aşağıdaki ayrıntıları ihtiyacınız olacak:

* Çoğaltılacak korunabilir öğe.
* Sanal makine için çoğaltmak için depolama hesabı. Ayrıca, bir günlük depolama, premium depolama hesabı için sanal makineleri korumak için gereklidir.
* Çoğaltma için kullanılan işlem sunucusu. Kullanılabilir işlem sunucularının listesini alınan ve kaydedilen ***$ProcessServers [0]***  *(genişletme dosya)* ve ***$ProcessServers [1]*** *(ConfigurationServer)* değişkenleri.
* Mobility hizmet yazılımı makinelere göndermeli yükleme için kullanılacak hesabı. Kullanılabilir hesaplar listesi alınan ve depolanan ***$AccountHandles*** değişkeni.
* Koruma kapsayıcısı eşlemesini çoğaltma için kullanılacak çoğaltma ilkesi için.
* Kaynak grubu, sanal makinelerin yük devretme sırasında oluşturulmalıdır.
* İsteğe bağlı olarak, Azure sanal ağını ve alt ağı başarısız yük devredilen sanal makinenin bağlı olmalıdır.

Artık bu tabloda belirtilen ayarları kullanarak şu sanal makineleri çoğaltma


|Sanal makine  |Process Server        |Depolama Hesabı              |Günlük depolama hesabına  |İlke           |Mobility hizmeti yüklemesi için hesabı|Hedef kaynak grubu  | Hedef sanal ağ  |Hedef alt ağ  |
|-----------------|----------------------|-----------------------------|---------------------|-----------------|-----------------------------------------|-----------------------|-------------------------|---------------|
|Win2K12VM1       |Genişletme dosya|premiumstorageaccount1       |logstorageaccount1   |ReplicationPolicy|WindowsAccount                           |VMwareDRToAzurePs      |ASR sanal ağ                 |Alt ağ-1       |
|CentOSVM1       |ConfigurationServer   |replicationstdstorageaccount1| Yok                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |ASR sanal ağ                 |Alt ağ-1       |   
|CentOSVM2       |ConfigurationServer   |replicationstdstorageaccount1| Yok                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |ASR sanal ağ                 |Alt ağ-1       |   

 
```azurepowershell

#Get the target resource group to be used
$ResourceGroup = Get-AzResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap  = Get-ASRProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine Win2K12VM1
$VM1 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "Win2K12VM1"

# Enable replication for virtual machine Win2K12VM1
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableReplication1 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $PremiumStorageAccount.Id -LogStorageAccountId $LogStorageAccount.Id -ProcessServer $ProcessServers[0] -Account $AccountHandles[1] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM2 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1
$Job_EnableReplication2 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM2 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

#Get the protectable item corresponding to the virtual machine CentOSVM2
$VM3 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM2"

# Enable replication for virtual machine CentOSVM2
$Job_EnableReplication3 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM3 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

```

Etkinleştirme çoğaltma işi başarıyla tamamlandıktan sonra ilk çoğaltma sanal makineler için başlatılır. İlk çoğaltma, çoğaltma için çoğaltılması gereken veri miktarı ve kullanılabilir bant genişliğine bağlı olarak biraz sürebilir. İlk çoğaltma tamamlandıktan sonra sanal makinenin korumalı bir duruma taşınır. Sanal makine yük devretme testi sanal makinesi için gerçekleştirebileceğiniz korumalı bir duruma ulaştığında, Kurtarma planlarına vb. ekleyin.

Get-ASRReplicationProtectedItem cmdlet'i ile sanal makine çoğaltma durumu ve çoğaltma durumunu kontrol edebilirsiniz.

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

## <a name="configure-failover-settings"></a>Yük devretme ayarlarını yapılandırma

Korumalı makineler için yük devretme ayarları kümesi ASRReplicationProtectedItem cmdlet'i kullanılarak güncelleştirilebilir. Bu cmdlet ile güncelleştirilebilir ayarlardan bazıları şunlardır:
* Yük devretme sırasında oluşturulacak sanal makinenin adı
* Sanal makine yük devretme sırasında oluşturulacak sanal makine boyutu
* Azure sanal ağ ve NIC sanal makinenin yük devretme sırasında bağlanması gereken alt
* Yönetilen disklere
* Azure hibrit kullanım teklifi Uygula
* Yük devretme sanal makineye atanacak hedef sanal ağdan statik bir IP adresi atayın.

Bu örnekte, sanal makine için yük devretme sırasında oluşturulacak sanal makinenin VM boyutunu güncelleştiriyoruz *Win2K12VM1* ve sanal makine kullanımı yönetilen diskler üzerinde yük devretme.

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

1. DR tatbikatı (yük devretme testi) aşağıdaki gibi çalıştırın:

   ```azurepowershell
   #Test failover of Win2K12VM1 to the test virtual network "V2TestNetwork"

   #Get details of the test failover virtual network to be used
   TestFailovervnet = Get-AzVirtualNetwork -Name "V2TestNetwork" -ResourceGroupName "asrrg" 

   #Start the test failover operation
   $TFOJob = Start-ASRTestFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -AzureVMNetworkId $TestFailovervnet.Id -Direction PrimaryToRecovery
   ```
2. Test yük devretme işi başarıyla tamamlandıktan sonra bir sanal makine ile olark fark edeceksiniz *"-Test"* (Bu durumda, Win2K12VM1-Test) adının Azure içinde oluşturulur.
3. Şimdi, sınama yük devredilen sanal makinenin başarısız bağlanın ve test yük devretmesi doğrulayın.
4. Başlangıç ASRTestFailoverCleanupJob cmdlet'ini kullanarak devretmeyi temizleyin. Bu işlem, sanal makine test yük devretme işleminin bir parçası olarak oluşturulan siler.

   ```azurepowershell
   $Job_TFOCleanup = Start-ASRTestFailoverCleanupJob -ReplicationProtectedItem $ReplicatedVM1
   ```

## <a name="fail-over-to-azure"></a>Azure'a yük devretme

Bu adımda, size belirli bir kurtarma noktasını sanal makineye Win2K12VM1 yük devretme.

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

2. Başarıyla, üzerinde başarısız oldu. sonra Yük devretme işlemini tamamlama ve azure'dan çoğaltmayı tersine çevirme ayarlamak için şirket içi VMware sitesi yedekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Kullanarak daha fazla görevleri otomatikleştirmeyi öğrenin [Azure Site Recovery PowerShell başvurusu](https://docs.microsoft.com/powershell/module/Az.RecoveryServices).
