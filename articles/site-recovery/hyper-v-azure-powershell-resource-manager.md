---
title: Hyper-V Vm'lerini PowerShell ve Azure Resource Manager ile çoğaltma | Microsoft Docs
description: Hyper-V Vm'lerini PowerShell ve Azure Resource Manager kullanarak Azure Site Recovery ile azure'a çoğaltılmasını otomatikleştirin.
services: site-recovery
author: bsiva
manager: abhiag
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 13456dc53f85f6f26aab222ab0cb499aabb7d1cc
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916240"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>PowerShell ve Azure Resource Manager'ı kullanarak Hyper-V Vm'leri için Azure'da olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) çoğaltma, yük devretme ve Azure sanal makineleri (VM'ler) kurtarma işlemlerini düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur ve şirket içi Vm'leri ve fiziksel sunucuları.

Bu makalede, Hyper-V Vm'lerini Azure'a çoğaltma için Windows PowerShell, Azure Resource Manager ile birlikte kullanmayı açıklar. Bu makalede kullanılan örnek bir Hyper-V konağına, Azure üzerinde çalışan tek bir VM'yi çoğaltma gösterilmektedir.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell cmdlet'lerini Windows PowerShell kullanarak Azure yönetmenizi sağlar. Site Recovery PowerShell cmdlet, Azure PowerShell için Azure Resource Manager ile kullanılabilir korumak ve sunucularınızı azure'da kurtarmanıza yardımcı olur.

Bu makalede kullanmak için bir PowerShell uzman olmanız gerekmez, ancak modüller, cmdlet'ler ve oturumlar gibi temel kavramları anlamanız gerekir. Okuma [Windows PowerShell ile çalışmaya başlama](http://technet.microsoft.com/library/hh857337.aspx), ve [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft iş ortaklarına bulut çözümü sağlayıcısı (CSP) programında yapılandırabilir ve ilgili CSP aboneliklerini (Kiracı abonelikleri) müşteri sunuculara koruma yönetebilirsiniz.
>
>

## <a name="before-you-start"></a>Başlamadan önce
Bu önkoşulların sağlandığından emin olun:

* A [Microsoft Azure](https://azure.microsoft.com/) hesabı. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz. Ayrıca, okuyabilirsiniz [Azure Site Recovery Manager fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Bu sürüm ve nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) ve [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modüller. Bu modüllerden en son sürümlerini alabilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/)

Ayrıca, bu makalede açıklanan özel bir örnek, aşağıdaki önkoşulları vardır:

* Windows Server 2012 R2 ya da bir veya daha fazla Vm'leri içeren Microsoft Hyper-V Server 2012 R2 çalıştıran bir Hyper-V konağı. Hyper-V sunucularının, doğrudan veya bir ara sunucu üzerinden Internet'e bağlanmalıdır.
* Çoğaltmak istediğiniz VM'lerin karşılaması gerektiğini [bu Önkoşullar](hyper-v-azure-support-matrix.md#replicated-vms).

## <a name="step-1-sign-in-to-your-azure-account"></a>1. adım: Azure hesabınızda oturum açın

1. Bir PowerShell konsolu açın ve Azure hesabınızda oturum açmak için şu komutu çalıştırın. Cmdlet getirir, bir web sayfası için hesap kimlik bilgilerinizi ister: **Connect-AzureRmAccount**.
    - Alternatif olarak, bir parametre olarak bir hesap kimlik bilgilerinizi içerebilir **Connect-AzureRmAccount** cmdlet'ini kullanarak **-Credential** parametresi.
    - Müşteri, Kiracı adına çalışma CSP iş ortağıysanız, Kiracı kimliği veya Kiracı birincil etki alanı adlarını kullanarak Kiracı olarak belirtin. Örneğin: **Connect-AzureRmAccount-Kiracı "fabrikam.com"**
2. Hesabınız birden çok abonelik olabileceği için bir hesap ile kullanmak istediğiniz aboneliği ilişkilendirin:

    `Select-AzureRmSubscription -SubscriptionName $SubscriptionName`

3. Aboneliğiniz için kurtarma Hizmetleri ve şu komutları kullanarak Site Recovery, Azure sağlayıcılarını kullanacak şekilde kaydedildiğini doğrulayın:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

4. Komut çıktısında, doğrulayın **RegistrationState** ayarlanır **kayıtlı**, 2. adım geçebilirsiniz. Aksi durumda, bu komutları çalıştırarak eksik sağlayıcısı, aboneliğinizde kaydolmalıdır:

    `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery` `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Aşağıdaki komutları kullanarak sağlayıcılar başarıyla kayıtlı doğrulayın:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-vault"></a>2. adım: kasası ayarlama

1. Kasa oluşturulacağı bir Azure Resource Manager kaynak grubu oluşturun veya mevcut bir kaynak grubunu kullanın. Yeni bir kaynak grubu oluşturun. $ResourceGroupName değişkeni oluşturmak istediğiniz kaynak grubunun adını, ve $Geo değişkeni (örneğin, "Brezilya Güney") kaynak grubunun oluşturulacağı Azure bölgesi içerir.

    `New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo` 

2. Çalıştırın, aboneliğinizdeki kaynak gruplarının bir listesini almak için **Get-AzureRmResourceGroup** cmdlet'i.
2. Şu şekilde yeni bir Azure kurtarma Hizmetleri kasası oluşturun:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    İle varolan kasaların listesini alabilirsiniz **Get-AzureRmRecoveryServicesVault** cmdlet'i.


## <a name="step-3-set-the-recovery-services-vault-context"></a>3. adım: Kurtarma Hizmetleri kasa bağlamını ayarlayın

Kasa bağlamını şu şekilde ayarlayın:

`Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>4. adım: Hyper-V sitesi oluşturma

1. Yeni bir Hyper-V sitesi şu şekilde oluşturun:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

2. Bu cmdlet siteyi oluşturmak için bir Site kurtarma işini başlatır ve Site Recovery iş nesnesi döndürür. İş başarıyla tamamlandı tamamlamak ve doğrulamak işin tamamlanmasını bekleyin.
3. Kullanım **Get-AzureRmSiteRecoveryJob cmdlet'i**, iş nesnesinde alıp işinin geçerli durumunu denetleyin.
4. Oluşturma ve bir kayıt anahtarı site için aşağıdaki gibi indirin:

    ```
    $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path
    ```

5. İndirilen anahtarı, Hyper-V ana bilgisayara kopyalayın. Hyper-V konağı sitesine kaydetmek için anahtarı gerekir.

## <a name="step-5-install-the-provider-and-agent"></a>5. adım: sağlayıcı ve aracı yükleme

1. Yükleyici adresinden sağlayıcının en son sürümü için indirme [Microsoft](https://aka.ms/downloaddra).
2. Yükleyiciyi theHyper-V konağı üzerinde çalıştırın.
3. Yüklemenin sonunda kayıt adıma geçin.
4. İstendiğinde, indirilen anahtarı sağlayın ve Hyper-V konağının kaydını tamamlayın.
5. Hyper-V ana siteye şu şekilde kaydedildiğini doğrulayın:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy"></a>6. adım: çoğaltma ilkesi oluşturma

Başlamadan önce belirtilen depolama hesabı kasasıyla aynı Azure bölgesinde olmalıdır ve coğrafi çoğaltmanın etkinleştirilmiş olması unutmayın.

1. Bir çoğaltma ilkesi şu şekilde oluşturun:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Çoğaltma İlkesi oluşturma başarılı olmak için döndürülen işine bakın.

3. Siteye şu şekilde karşılık gelen koruma kapsayıcısı Al:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Koruma kapsayıcısı gibi çoğaltma ilkesiyle ilişkilendirin:

     $Policy AzureRmSiteRecoveryPolicy get - FriendlyName $PolicyName $associationJob = başlangıç AzureRmSiteRecoveryPolicyAssociationJob =-ilke $Policy - PrimaryProtectionContainer $protectionContainer

4. Başarıyla tamamlamak ilişkilendirme işin tamamlanmasını bekleyin.

## <a name="step-7-enable-vm-protection"></a>7. adım: Sanal makine korumayı etkinleştirin

1. Aşağıdaki şekilde, korumak istediğiniz sanal Makineye karşılık gelen koruma varlık alma:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Sanal makine koruyun. Koruduğunuz VM bağlı birden fazla disk varsa, işletim sistemi diski kullanarak belirtin *OSDiskName* parametresi.

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

3. İlk çoğaltma sonrasında korumalı bir duruma ulaşmak sanal makineleri bekleyin. Bu, Azure'a çoğaltılması gereken veri miktarı ve kullanılabilir Yukarı Akış bant genişliği gibi faktörlere bağlı olarak biraz zaman alabilir. Korumalı bir duruma yerinde olduğunda, iş durumu ve StateDescription şu şekilde güncelleştirilir: 

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Kurtarma özellikleri (örneğin, VM rolü boyut) ve Azure ağı, yük devretme sonrasında VM NIC eklemek güncelleştirin.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>8. adım: yük devretme testi çalıştırma
1. Yük devretme testi aşağıdaki gibi çalıştırın:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Test, Azure'da VM oluşturulduğunu doğrulayın. Azure'da VM test oluşturduktan sonra test yük devretme işi askıya alındı.
3. Temizleme ve test yük devrini tamamlamak için şunu çalıştırın:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure Site Recovery ile Azure Resource Manager PowerShell cmdlet'leri hakkında.
