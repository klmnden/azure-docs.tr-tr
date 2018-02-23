---
title: "PowerShell ve Azure Resource Manager ile Hyper-V Vm'lerini çoğaltma | Microsoft Docs"
description: "Hyper-V sanal makineleri PowerShell ve Azure Resource Manager kullanarak Azure Site Recovery ile azure'a çoğaltma otomatikleştirin."
services: site-recovery
author: bsiva
manager: abhiag
ms.service: site-recovery
ms.topic: article
ms.date: 02/14/2018
ms.author: bsiva
ms.openlocfilehash: 5269fa528b6c32576b9cf1fb945ebf85b41ce819
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>Hyper-V PowerShell ve Azure Resource Manager kullanarak VM'ler için olağanüstü durum kurtarma Azure ayarlama

[Azure Site Recovery](site-recovery-overview.md) çoğaltma, yük devretme ve kurtarma Azure sanal makineleri (VM'ler) düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı ve sanal makineleri ve fiziksel sunucuları şirket.

Bu makalede, Hyper-V Vm'lerini Azure'a çoğaltma için Windows PowerShell, Azure Resource Manager ile birlikte kullanmayı açıklar. Bu makalede kullanılan örnek bir Hyper-V konağına, Azure üzerinde çalışan tek bir VM çoğaltmak nasıl gösterir.

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell cmdlet'lerini Windows PowerShell kullanarak Azure yönetmenizi sağlar. Site Recovery PowerShell cmdlet, Azure PowerShell için Azure Resource Manager ile kullanılabilir korumak ve sunucularınızı Azure kurtarmak yardımcı olur.

Bu makalede kullanmak için uzman PowerShell olması gerekmez, ancak modüller, cmdlet'ler ve oturumlar gibi temel kavramları anlamanız gerekir. Okuma [Windows PowerShell ile çalışmaya başlama](http://technet.microsoft.com/library/hh857337.aspx), ve [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft iş ortaklarının bulut çözümü sağlayıcısı (CSP) programı yapılandırın ve bunların ilgili CSP aboneliklere (Kiracı abonelikleri) müşteri sunucuların korumasını yönetin.
>
>

## <a name="before-you-start"></a>Başlamadan önce
Bu Önkoşullar sağladığınızdan emin olun:

* A [Microsoft Azure](https://azure.microsoft.com/) hesabı. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz. Ayrıca, hakkında bilgi edinebilirsiniz [Azure Site Recovery Manager fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Bu sürüm ve nasıl yükleneceği hakkında daha fazla bilgi için bkz: [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) ve [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modüller. Bu modüllerden en son sürümlerini alabilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/)

Ayrıca, bu makalede açıklanan belirli örnek aşağıdaki önkoşullar vardır:

* Windows Server 2012 R2 veya bir veya daha fazla sanal makineleri içeren Microsoft Hyper-V Server 2012 R2 çalıştıran bir Hyper-V ana bilgisayarı. Hyper-V sunucuları Internet'e doğrudan veya bir proxy üzerinden bağlı.
* Çoğaltmak istediğiniz sanal makineleri karşılaması gerektiğini [bu Önkoşullar](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="step-1-sign-in-to-your-azure-account"></a>1. adım: Azure hesabınızda oturum açın

1. Bir PowerShell konsolu açın ve Azure hesabınızda oturum açmak için bu komutu çalıştırın. Cmdlet getirir bir web sayfası için hesap kimlik bilgilerinizi ister: **Login-AzureRmAccount**.
    - Alternatif olarak, bir parametresi olarak hesabı kimlik bilgilerinizi içerebilir **Login-AzureRmAccount** cmdlet'ini kullanarak **-kimlik bilgisi** parametresi.
    - Kiracı adına çalışma CSP ortağı varsa, müşteri, Tenantıd veya Kiracı birincil etki alanı adlarını kullanarak bir kiracı olarak belirtin. Örneğin: **Login-AzureRmAccount-Kiracı "fabrikam.com"**
2. Bir hesap birkaç abonelikleri olabileceği için hesap ile kullanmak istediğiniz aboneliği ilişkilendirin:

    `Select-AzureRmSubscription -SubscriptionName $SubscriptionName`

3. Aboneliğiniz Azure sağlayıcıları kurtarma Hizmetleri ve bu komutları kullanarak Site kurtarma için kullanılacak kayıtlı olduğunu doğrulayın:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

4. Komut çıktısında doğrulayın **RegistrationState** ayarlanır **kayıtlı**, adım 2'ye devam edebilirsiniz. Değilse, eksik sağlayıcısını şu komutları çalıştırarak, aboneliğinizde kayıt:

    `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery` `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Aşağıdaki komutları kullanarak sağlayıcıları başarıyla kayıtlı doğrulayın:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-vault"></a>2. adım: Kasa ayarlama

1. Kasa oluşturulacağı bir Azure Resource Manager kaynak grubu oluşturun veya varolan bir kaynak grubunu kullanın. Aşağıdaki gibi yeni bir kaynak grubu oluşturun. $ResourceGroupName değişkeni oluşturmak istediğiniz kaynak grubunun adını içerir ve $Geo değişkeni (örneğin, "Brezilya Güney") kaynak grubunun oluşturulacağı Azure bölgesi içerir.

    `New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo` 

2. Çalıştırmak, aboneliğinizdeki kaynak gruplarının bir listesini almak için **Get-AzureRmResourceGroup** cmdlet'i.
2. Yeni bir Azure kurtarma Hizmetleri kasası gibi oluşturun:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    İle varolan kasalarının listesi alabilirsiniz **Get-AzureRmRecoveryServicesVault** cmdlet'i.


## <a name="step-3-set-the-recovery-services-vault-context"></a>3. adım: Kurtarma Hizmetleri kasası bağlamını ayarlayın

Kasa bağlam aşağıdaki gibi ayarlayın:

`Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>4. adım: bir Hyper-V sitesi oluşturun

1. Yeni bir Hyper-V sitesi gibi oluşturun:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

2. Bu cmdlet siteyi oluşturmak için bir Site kurtarma işini başlatır ve Site Recovery iş nesnesi döndürür. İşi tamamlamak ve doğrulamak iş başarıyla tamamlandı bekleyin.
3. Kullanım **Get-AzureRmSiteRecoveryJob cmdlet'i**iş nesnesini almak ve işinin geçerli durumunu denetlemek için.
4. Oluştur ve site için bir kayıt anahtarı aşağıdaki gibi indirin:

    ```
    $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path
    ```

5. İndirilen anahtarı Hyper-V ana bilgisayara kopyalayın. Hyper-V ana siteye kaydetmek için anahtarını gerekir.

## <a name="step-5-install-the-provider-and-agent"></a>5. adım: sağlayıcı ve aracı yükleme

1. Yükleyici Sağlayıcısı'ndan en son sürümü karşıdan [Microsoft](https://aka.ms/downloaddra).
2. Yükleyici theHyper-V ana bilgisayarda çalıştırın.
3. Yükleme sonunda kayıt adıma devam edin.
4. İstendiğinde, indirilen anahtarı sağlayın ve Hyper-V ana bilgisayar kaydını tamamlayın.
5. Hyper-V ana siteye şu şekilde kaydedildiğini doğrulayın:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy"></a>6. adım: bir çoğaltma ilkesi oluşturma

Başlamadan önce belirtilen depolama hesabı kasa ile aynı Azure bölgesinde olmalıdır ve coğrafi çoğaltmanın etkinleştirilmiş olması unutmayın.

1. Bir çoğaltma ilkesi şu şekilde oluşturun:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Çoğaltma İlkesi oluşturma başarılı olduğundan emin olmak için döndürülen işini denetleyin.

3. Siteye gibi karşılık gelen koruma kapsayıcısı Al:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Koruma kapsayıcısı gibi çoğaltma ilkesiyle ilişkilendirin:

     $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName   $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

4. İlişkilendirme işi başarıyla tamamlanmasını bekleyin.

## <a name="step-7-enable-vm-protection"></a>7. adım: VM korumasını etkinleştir

1. Aşağıdaki gibi korumak istediğiniz VM karşılık gelen koruma varlık Al:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. VM koruyun. Koruduğunuz VM ekli birden çok disk varsa, işletim sistemi diski kullanarak belirtin *OSDiskName* parametresi.

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

3. İlk çoğaltma sonrasında korumalı bir duruma ulaşması VM'ler için bekleyin. Bu Azure'a çoğaltılması için veri miktarını ve Yukarı Akış bant gibi etkenlere bağlı olarak biraz zaman alabilir. Korumalı bir duruma yerinde olduğunda, iş durumu ve StateDescription şu şekilde güncelleştirilmiştir: 

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Kurtarma özellikleri (örneğin, VM rolü boyut) ve yük devretme sonrasında VM NIC ekleneceği Azure ağı güncelleştirin.

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
1. Yük devretme testi çalıştırmak için şunları yapın:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Test Azure'da VM oluşturulduğunu doğrulayın. Test yük devretme işini Azure'da VM test oluşturduktan sonra askıya alınır.
3. Temizleme ve yük devretme sınaması tamamlamak için çalıştırın:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure Site Recovery ile Azure Resource Manager PowerShell cmdlet'leri hakkında.
