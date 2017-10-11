---
title: "PowerShell ve Azure Resource Manager ile Hyper-V Vm'lerini çoğaltma | Microsoft Docs"
description: "Hyper-V sanal makineleri PowerShell ve Azure Resource Manager kullanarak Azure Site Recovery ile azure'a çoğaltma otomatikleştirin."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: dbd562b73b0caecd0feb993bd6fb796dddb0438c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>PowerShell ve Azure Resource Manager kullanarak şirket içi Hyper-V sanal makineleri ve Azure arasında çoğaltma
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell - Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Klasik Portal](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a>Genel Bakış
Azure Site Recovery, çoğaltma, yük devretme ve kurtarma çeşitli dağıtım senaryolarında sanal makinelerin düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma stratejinize katkıda bulunur. Dağıtım senaryoları tam bir listesi için bkz: [Azure Site Recovery genel bakış](site-recovery-overview.md).

Azure PowerShell cmdlet'leri Azure Windows PowerShell üzerinden yönetmenizi sağlayan bir modüldür. İki tür modülü çalışabilir: Azure profili modülü veya Azure Resource Manager modülü.

Site Recovery PowerShell cmdlet, Azure PowerShell için Azure Resource Manager ile kullanılabilir korumak ve sunucularınızı Azure kurtarmak yardımcı olur.

Bu makalede, yapılandırmak ve sunucusu Azure koruması için düzenlemek için Site Recovery dağıtmak için Windows PowerShell, Azure Resource Manager ile birlikte kullanmak açıklar. Bu makalede kullanılan örnek koruma, yük devri ve Azure Resource Manager ile Azure PowerShell'i kullanarak azure'a, Hyper-V konağı üzerinde sanal makineleri kurtarma gösterilmektedir.

> [!NOTE]
> Site Recovery PowerShell cmdlet şu anda aşağıdaki yapılandırmanıza olanak tanır: başka bir sanal makine yöneticisi sitesine, bir Azure Sanal Makine Yöneticisi siteye ve Azure Hyper-V sitesi.
>
>

Bu makalede kullanmak için uzman PowerShell olması gerekmez, ancak modüller, cmdlet'ler ve oturumlar gibi temel kavramları anlamanız gerekir. Windows PowerShell hakkında daha fazla bilgi için bkz. [Windows PowerShell ile Çalışmaya Başlama](http://technet.microsoft.com/library/hh857337.aspx).

Ayrıca daha fazla hakkında bilgiyi [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

> [!NOTE]
> Bulut çözümü sağlayıcısı (CSP) programın parçası olan Microsoft iş ortaklarının yapılandırın ve müşterilerin ilgili CSP aboneliklere (Kiracı abonelikleri) müşterilerin sunucuların korumasını yönetin.
>
>

## <a name="before-you-start"></a>Başlamadan önce
Bu Önkoşullar sağladığınızdan emin olun:

* A [Microsoft Azure](https://azure.microsoft.com/) hesabı. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz. Ayrıca, hakkında bilgi edinebilirsiniz [Azure Site Recovery Manager fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Bu sürüm ve nasıl yükleneceği hakkında daha fazla bilgi için bkz: [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) ve [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modüller. Bu modüllerden en son sürümlerini alabilirsiniz [PowerShell Galerisi](https://www.powershellgallery.com/)

Bu makalede Azure PowerShell'i Azure Resource Manager ile yapılandırmak ve koruma sunucularınızın yönetmek için nasıl kullanılacağı gösterilmektedir. Bu makalede kullanılan örnek Azure bir Hyper-V ana bilgisayarda çalışan bir sanal makine Koru gösterilmiştir. Bu örnek için aşağıdaki önkoşullar özgüdür. Daha kapsamlı birtakım çeşitli Site kurtarma senaryoları için gereksinimleri için bu senaryo ile ilgili belgelere başvurun.

* Windows Server 2012 R2 veya bir veya daha fazla sanal makine içeren Microsoft Hyper-V Server 2012 R2 çalıştıran bir Hyper-V ana bilgisayarı.
* Hyper-V sunucularının, doğrudan ya da bir proxy sunucu üzerinden Internet'e bağlı.
* Korumak istediğiniz sanal makineleri karşılaması gerektiğini [sanal makine önkoşulları](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="step-1-sign-in-to-your-azure-account"></a>1. adım: Azure hesabınızda oturum açın
1. Bir PowerShell konsolu açın ve Azure hesabınızda oturum açmak için bu komutu çalıştırın. Cmdlet için hesap kimlik bilgilerinizi ister bir web sayfasını getirir.

        Login-AzureRmAccount

    Alternatif olarak, ayrıca, hesap kimlik bilgilerini bir parametre olarak ekleyebilirsiniz `Login-AzureRmAccount` cmdlet'ini kullanarak `-Credential` parametresi.

    Kiracı adına çalışma CSP ortağı varsa, müşteri, Tenantıd veya Kiracı birincil etki alanı adlarını kullanarak bir kiracı olarak belirtin.

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. Hesap ile kullanmak istediğiniz aboneliği ilişkilendirmeniz böylece bir hesap birden fazla abonelik olabilir.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. Aboneliğinizi aşağıdaki komutları kullanarak Azure sağlayıcıları kurtarma Hizmetleri ve Site kurtarma için kullanmak üzere kayıtlı olduğunu doğrulayın:

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   Bu komutların çıktısı, varsa **RegistrationState** ayarlanır **kayıtlı**, adım 2'ye devam edebilirsiniz. Aksi durumda, aboneliğinizde eksik sağlayıcısı kaydetmeniz.

   Site kurtarma ve kurtarma Hizmetleri için Azure sağlayıcısını kaydetmek için aşağıdaki komutları çalıştırın:

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   Aşağıdaki komutları kullanarak sağlayıcıları başarıyla kaydedildiğini doğrulayın: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` ve `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-recovery-services-vault"></a>2. adım: Kurtarma Hizmetleri kasası ayarlama
1. İçinde kasası oluşturma veya varolan bir kaynak grubunu kullanın bir Azure Resource Manager kaynak grubu oluşturun. Aşağıdaki komutu kullanarak yeni bir kaynak grubu oluşturabilirsiniz:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    Burada $ResourceGroupName değişkeni oluşturmak istediğiniz kaynak grubunun adını içerir ve $Geo değişkeni (örneğin, "Brezilya Güney") kaynak grubunun oluşturulacağı Azure bölgesi içerir.

    Kaynak gruplarının bir listesini kullanarak aboneliğinizde elde edebilirsiniz `Get-AzureRmResourceGroup` cmdlet'i.
2. Yeni bir Azure kurtarma Hizmetleri kasası gibi oluşturun:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Kullanarak mevcut kasalarının listesi alabilirsiniz `Get-AzureRmRecoveryServicesVault` cmdlet'i.

> [!NOTE]
> Klasik Portalı'nı veya Azure Service Management PowerShell modülü kullanılarak oluşturulan Site Recovery kasası işlemleri gerçekleştirmek isterseniz, kullanarak bu tür kasalarının listesi alabilirsiniz `Get-AzureRmSiteRecoveryVault` cmdlet'i. Tüm yeni işlemleri için yeni bir kurtarma Hizmetleri kasası oluşturmanız gerekir. Daha önce oluşturduğunuz Site Recovery kasası desteklenir, ancak en son özelliklere sahip değilsiniz.
>
>

## <a name="step-3-set-the-recovery-services-vault-context"></a>3. adım: Kurtarma Hizmetleri kasası bağlamını ayarlayın
1. Aşağıdaki komutu çalıştırarak kasası bağlamını ayarlayın:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>4. adım: bir Hyper-V sitesi oluşturun ve site için yeni bir kasa kayıt anahtarı oluşturun.
1. Yeni bir Hyper-V sitesi gibi oluşturun:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Bu cmdlet siteyi oluşturmak için bir Site kurtarma işini başlatır ve Site Recovery iş nesnesi döndürür. İşi tamamlamak ve doğrulamak iş başarıyla tamamlandı bekleyin.

    İş nesnesini almak ve böylelikle Get-AzureRmSiteRecoveryJob cmdlet'ini kullanarak işinin geçerli durumunu denetleyin.
2. Oluştur ve site için bir kayıt anahtarı aşağıdaki gibi indirin:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    İndirilen anahtarı Hyper-V ana bilgisayara kopyalayın. Hyper-V ana siteye kaydetmek için anahtarını gerekir.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>5. adım: Azure Site Recovery sağlayıcısı ve Azure kurtarma Hizmetleri Aracısı, Hyper-V konak üzerinde yükleyin
1. Yükleyici Sağlayıcısı'ndan en son sürümü karşıdan [Microsoft](https://aka.ms/downloaddra).
2. Çalışma yükleyici, Hyper-V ana bilgisayarında ve yükleme işleminin sonunda kayıt adıma devam edin.
3. İstendiğinde, indirilen site kayıt anahtarını ve Hyper-V ana siteye tam kayıt sağlayın.
4. Aşağıdaki komutu kullanarak Hyper-V ana siteye kayıtlı olduğunu doğrulayın:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>6. adım: bir çoğaltma ilkesi oluşturun ve koruma kapsayıcısı ile ilişkilendirin
1. Bir çoğaltma ilkesi şu şekilde oluşturun:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Çoğaltma İlkesi oluşturma başarılı olduğundan emin olmak için döndürülen işini denetleyin.

   > [!IMPORTANT]
   > Belirtilen depolama hesabı, Kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde olmalıdır ve coğrafi çoğaltmanın etkinleştirilmiş olması gerekir.
   >
   > * Belirtilen Kurtarma Depolama hesabı türü Azure Storage (Klasik) ise, korunan makinelerin yük devretme Azure Iaas (Klasik) makineye kurtarın.
   > * Belirtilen Kurtarma Depolama hesabı türü Azure Storage (Azure Resource Manager) ise, korunan makinelerin yük devretme Azure Iaas (Azure Resource Manager) makineye kurtarın.
   >
   >
2. Siteye gibi karşılık gelen koruma kapsayıcısı alın:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Koruma kapsayıcısı ilişkisini çoğaltma ilkesi, aşağıda gösterildiği gibi başlatın:

     $Policy get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = başlangıç AzureRmSiteRecoveryPolicyAssociationJob =-ilke $Policy - PrimaryProtectionContainer $protectionContainer

   İlişkilendirme işi tamamlanmasını bekleyin ve başarıyla tamamlandığını doğrulayın.

## <a name="step-7-enable-protection-for-virtual-machines"></a>7. adım: sanal makineler için korumayı etkinleştir
1. Aşağıdaki gibi korumak istediğiniz VM karşılık gelen koruma varlık alın:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Sanal makine aşağıdaki gibi koruma başlatın:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > Belirtilen depolama hesabı, Kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde olmalıdır ve coğrafi çoğaltmanın etkinleştirilmiş olması gerekir.
   >
   > * Belirtilen Kurtarma Depolama hesabı türü Azure Storage (Klasik) ise, korunan makinelerin yük devretme Azure Iaas (Klasik) makineye kurtarın.
   > * Belirtilen Kurtarma Depolama hesabı türü Azure Storage (Azure Resource Manager) ise, korunan makinelerin yük devretme Azure Iaas (Azure Resource Manager) makineye kurtarın.
   >
   > Koruduğunuz VM ekli birden çok disk varsa, işletim sistemi diski kullanarak belirtin *OSDiskName* parametresi.
   >
   >
3. Sanal makinelerin ilk çoğaltma sonrasında bir korumalı durumuna ulaşmasını bekleyin. Bu çoğaltılacak veri miktarını ve Azure Yukarı Akış bant gibi etkenlere bağlı olarak biraz zaman alabilir. Aşağıdaki gibi StateDescription ve iş durumu korumalı duruma ulaşmadan VM güncelleştirilir.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. VM rol boyutu ve Itanium tabanlı sistemler için sanal makinenin ağ arabirimi kartları için yük devretme sırasında eklemek için Azure ağ gibi kurtarma özelliklerini güncelleştirir.

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
1. Bir test yük devretme işini çalıştırmak için şunları yapın:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Test Azure'da VM oluşturulduğunu doğrulayın. (Test yük devretme işini, Azure'da VM test oluşturduktan sonra askıya alındı. İş işi yeniden başlatma sırasında oluşturulan artefacts sonraki adımda gösterildiği gibi temizleniyor tarafından tamamlar.)
3. Yük devretme testi, aşağıdaki gibi tamamlayın:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Sonraki Adımlar
[Daha fazla bilgi](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure Site Recovery ile Azure Resource Manager PowerShell cmdlet'leri hakkında.
