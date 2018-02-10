---
title: "Azure Site Recovery ve PowerShell (Resource Manager) kullanarak VMM bulutlarındaki Hyper-V sanal makineleri çoğaltmak | Microsoft Docs"
description: "Azure Site Recovery ve PowerShell kullanarak VMM bulutlarındaki Hyper-V sanal makinelerini çoğaltma"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: raynew
ms.assetid: 6ac509ad-5024-43d8-b621-d8fec019b9a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: rajanaki
ms.openlocfilehash: 504548abf00cf682c130af22eb29d47d0f47a82a
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>VMM bulutlarındaki Hyper-V sanal makineleri PowerShell ve Azure Resource Manager kullanarak azure'a
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell - Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell - Klasik](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Genel Bakış
Azure Site Recovery, çoğaltma, yük devretme ve kurtarma çeşitli dağıtım senaryolarında sanal makinelerin düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sağlar. Dağıtım tam listesi için bkz: senaryoları [Azure Site Recovery genel bakış](site-recovery-overview.md).

Bu makalede Azure depolama alanına System Center VMM bulutlarındaki Hyper-V sanal makineleri çoğaltmak için Azure Site Recovery ayarladığınızda, yapmanız gereken ortak görevleri otomatik hale getirmek için PowerShell kullanmayı gösterir.

Makaleyi senaryonun önkoşulları içerir ve gösterir

* Bir kurtarma Hizmetleri kasasını nasıl kurulur
* Kaynak VMM sunucusunda Azure Site kurtarma Sağlayıcısı'nı yükleme
* Sunucuyu kasaya kaydetmek, bir Azure depolama hesabı ekleme
* Azure kurtarma Hizmetleri aracısını Hyper-V ana bilgisayar sunucularına yükleyin
* Tüm korumalı sanal makinelere uygulanacak VMM Bulutları için koruma ayarlarını yapılandırın
* Bu sanal makineler için korumayı etkinleştirin.
* Her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi.

Bu senaryoyu oluşturan ayarlama sorunlarını alıyorsanız, üzerinde sorularınızı [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Resource Manager dağıtım modeli kullanılarak yer almaktadır.
>
>

## <a name="before-you-start"></a>Başlamadan önce
Bu Önkoşullar sağladığınızdan emin olun:

### <a name="azure-prerequisites"></a>Azure önkoşulları
* Bir [Microsoft Azure](https://azure.microsoft.com/) hesabınızın olması gerekir. Yoksa, başlayan bir [ücretsiz bir hesap](https://azure.microsoft.com/free). Ayrıca, hakkında bilgi edinebilirsiniz [Azure Site Recovery Manager fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/).
* Bir CSP abonelik senaryosuna çoğaltma çıkışı çalışıyorsanız, CSP aboneliğine sahip olmanız gerekir. CSP programı hakkında daha fazla bilgi [CSP programa nasıl](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
* Azure'a çoğaltılan verileri depolamak için bir Azure v2 (Resource Manager) depolama hesabınızın olması gerekir. Hesap coğrafi çoğaltmanın etkinleştirilmiş olmalıdır. Azure Site Recovery hizmeti ile aynı bölgede olması ve aynı abonelik veya CSP abonelik ile ilişkilendirilmiş olması. Azure depolama ortamını ayarlama hakkında daha fazla bilgi için bkz: [Microsoft Azure Storage'a giriş](../storage/common/storage-introduction.md) başvuru.
* Korumak istediğiniz sanal makineleri ile uyumlu olduğundan emin olmak gerekir [Azure sanal makine önkoşulları](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

> [!NOTE]
> Şu anda yalnızca VM düzeyinde işlemleri Powershell aracılığıyla mümkündür. Kurtarma planı düzeyi işlemleri için destek yakında sunulacaktır.  Şu an için başarısız-belirlemez yalnızca 'VM korumalı' ayrıntı düzeyi ve düzeyinde bir kurtarma planı gerçekleştirmeye sınırlıdır.
>
>

### <a name="vmm-prerequisites"></a>VMM önkoşulları
* System Center 2012 R2 üzerinde çalışan VMM sunucusu gerekir.
* Korumak istediğiniz sanal makineleri içeren herhangi bir VMM sunucusuna Azure Site kurtarma Sağlayıcısı'nı çalıştırması gerekir. Bu Azure Site Recovery dağıtımı sırasında yüklenir.
* Korumak istediğiniz VMM sunucusunda en az bir bulut gerekir. Bulut içermelidir:
  * Bir veya birden çok VMM ana bilgisayar grubu.
  * Her ana bilgisayar grubunda bir veya daha fazla Hyper-V sunucusu ya da kümesi.
  * Kaynak Hyper-V sunucu üzerindeki bir veya daha fazla sanal makineler.
* VMM bulutlarını ayarlama hakkında daha fazla bilgi edinin:
  * Özel VMM bulutlarındaki hakkında daha fazla bilgiyi [System Center 2012 R2 VMM ile özel bulut yenilikler](http://go.microsoft.com/fwlink/?LinkId=324952) ve [VMM 2012 ve Bulutları](http://go.microsoft.com/fwlink/?LinkId=324956).
  * Hakkında bilgi edinin [VMM bulut yapı](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
  * Bulut doku öğelerinizi hazır olduktan sonra özel bulutta oluşturma hakkında bilgi edinin [VMM'de özel bulut oluşturma](http://go.microsoft.com/fwlink/p/?LinkId=324953) ve [izlenecek yol: System Center 2012 SP1 VMM içeren özel bulut oluşturma](http://go.microsoft.com/fwlink/p/?LinkId=324954).

### <a name="hyper-v-prerequisites"></a>Hyper-V önkoşulları
* Hyper-V ana bilgisayar sunucuları en az çalıştırmalıdır **Windows Server 2012** Hyper-V rolüne sahip veya **Microsoft Hyper-V Server 2012** ve en son güncelleştirmelerin yüklü olması.
* Hyper-V'yi bir kümede çalıştırıyorsanız statik IP adresi temelli bir kümeye sahip olmanız durumunda küme aracısının otomatik olarak oluşturulamayacağını unutmayın. Küme aracısını sizin yapılandırmanız gerekir. Bitiş Tarihi
* Yönergeler için bkz [Hyper-V çoğaltma aracısı yapılandırma nasıl](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
* Herhangi bir Hyper-V konak sunucusu veya küme koruma yönetmek istediğiniz bir VMM bulutunda eklenmesi gerekir.

### <a name="network-mapping-prerequisites"></a>Ağ eşlemesi önkoşulları
Ne zaman Azure, kaynak VMM sunucusunda sanal makine ağları ağ eşlemesi eşlemeleri sanal makinelerin korunmasına ve hedef Azure ağları aşağıdaki etkinleştirmek için:

* Aynı ağda yük devri tüm makineler birbirine, hangi kurtarma planında yer aldıklarına bulundukları bağlanabilir.
* Bir ağ geçidi hedef Azure ağında ayarlanırsa sanal makineler diğer şirket içi sanal makinelere bağlanabilir.
* Ağ eşlemesini yapılandırmazsanız yalnızca aynı kurtarma planında yük devri sanal makineleri azure'a yük devretme sonrasında birbirine kuramaz.

Ağ eşlemesi dağıtmak isterseniz şunlara ihtiyaç duyarsınız:

* Kaynak VMM sunucusunda korumak istediğinizde sanal makinelerin bir VM ağına bağlanması gerekir. Bu ağın, bulutla ilişkilendirilen mantıksal bir ağ ile bağlantılı olması gerekir.
* Yük devretme çoğaltılmış sanal makinelerin bağlanabileceği bir Azure ağı. Yük devretme aynı anda bu ağ seçersiniz. Ağın Azure Site Recovery aboneliğinizle aynı bölgede olması gerekir.

Ağ eşlemesi hakkında daha fazla bilgi edinin

* [VMM'de Mantıksal ağlar yapılandırma](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [VMM'de VM ağları ve ağ geçitlerini yapılandırma](http://go.microsoft.com/fwlink/p/?LinkId=386308)
* [Nasıl yapılandırılacağı ve Azure sanal ağları izleme](https://azure.microsoft.com/documentation/services/virtual-network/)

### <a name="powershell-prerequisites"></a>PowerShell önkoşulları
Azure PowerShell davranmaya hazır olduğundan emin olun. PowerShell zaten kullanıyorsanız, 0.8.10 sürümüne yükseltme veya üstü gerekir. PowerShell ayarlama hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma için kılavuz](/powershell/azureps-cmdlets-docs). Ayarlama ve PowerShell yapılandırılmış sonra tüm kullanılabilir cmdlet'leri hizmeti görüntüleyebilirsiniz [burada](/powershell/azure/overview).

Parametre değerleri, girişleri ve çıkışları genellikle Azure PowerShell'de işlenme gibi cmdlet'leri kullanın yardımcı olabilecek ipuçları hakkında bilgi edinmek için bkz [Azure cmdlet'leri kullanmaya başlama Kılavuzu](/powershell/azure/get-started-azureps).

## <a name="step-1-set-the-subscription"></a>1. adım: Abonelik ayarlama
1. Azure powershell'den Azure hesabınızda oturum açın: aşağıdaki cmdlet'leri kullanarak

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Aboneliklerinizi listesini alın. Bu da subscriptionIDs her abonelik için listeler. Kurtarma Hizmetleri kasası oluşturmak istediğiniz aboneliği Subscriptionıd unutmayın

        Get-AzureRmSubscription
3. Kurtarma Hizmetleri kasası abonelik kimliği söz tarafından oluşturulacak olan abonelik ayarlayın

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a>2. Adım: Bir Kurtarma Hizmetleri kasası oluşturma
1. Zaten yoksa, Azure Kaynak Yöneticisi'nde bir kaynak grubu oluştur

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Yeni bir kurtarma Hizmetleri kasası oluşturun ve oluşturulan ASR kasası nesnesi (daha sonra kullanılacak) bir değişkene kaydedin. Get-AzureRMRecoveryServicesVault cmdlet'ini kullanarak ASR kasası nesne post oluşturma de alabilirsiniz:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>3. adım: Kurtarma Hizmetleri kasası bağlamını ayarlayın

Çalıştırarak kasası bağlamını ayarlayın aşağıda komutu.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>4. adım: Azure Site kurtarma Sağlayıcısı'nı yükleme
1. VMM makinesinde aşağıdaki komutu çalıştırarak bir dizin oluşturun:

       New-Item c:\ASR -type directory
2. Aşağıdaki komutu çalıştırarak indirilen sağlayıcısını kullanarak dosyaları ayıklayın

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Aşağıdaki komutları kullanarak sağlayıcısını yükleyin:

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

   Yüklemesinin tamamlanması için bekleyin.
4. Sunucu aşağıdaki komutu kullanarak kasaya kaydedin:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-an-azure-storage-account"></a>5. adım: Azure storage hesabı oluşturma

Bir Azure depolama hesabınız yoksa, bir coğrafi çoğaltmanın etkinleştirilmiş hesabı kasa ile aynı coğrafi bölgede aşağıdaki komutu çalıştırarak oluşturun:

        $StorageAccountName = "teststorageacc1"    #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"     
        $ResourceGroupName =  “myRG”             #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Depolama hesabı gerekir Azure Site Recovery hizmeti ile aynı bölgede olması ve aynı abonelikle ilişkilendirilmiş olduğunu unutmayın.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>6. adım: Azure kurtarma Hizmetleri Aracısı yükleme
1. Konumundaki Azure kurtarma Hizmetleri Aracısı'nı indirme [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) ve korumak istediğiniz VMM bulutlarında yer alan her Hyper-V ana bilgisayar sunucusunda yükleyin.
2. Tüm VMM ana bilgisayarlarda aşağıdaki komutu çalıştırın:

       marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>7. adım: Bulut yapılandırma koruma ayarları
1. Aşağıdaki komutu çalıştırarak Azure bir çoğaltma ilkesi oluşturun:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

1. Koruma kapsayıcısı aşağıdaki komutları çalıştırarak alın:

       $PrimaryCloud = "testcloud"
       $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  
2. Oluşturulan iş kullanarak ve kolay ilke adı söz bir değişkene ilke ayrıntıları alın:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Koruma kapsayıcısı ilişkisini çoğaltma ilkesiyle başlatın:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  
4. İş tamamlandıktan sonra aşağıdaki komutu çalıştırın:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }
5. İş işlemeyi bitirdikten sonra aşağıdaki komutu çalıştırın:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

İşlemin tamamlanması denetlemek için adımları [etkinliğini izleme](#monitor).

## <a name="step-8-configure-network-mapping"></a>8. adım: ağ eşlemesini yapılandırma
Ağ eşlemesine başlamadan önce kaynak VMM sunucusundaki sanal makinelerin bir VM ağına bağlı olduğunu doğrulayın. Ayrıca, bir veya daha fazla Azure sanal ağı oluşturun.

Azure Resource Manager ve PowerShell kullanarak bir sanal ağ oluşturma hakkında daha fazla bilgi [Azure Resource Manager ve PowerShell kullanarak siteden siteye VPN bağlantısı olan bir sanal ağ oluşturma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Birden çok sanal makine ağları tek bir Azure ağına eşlenebileceğini unutmayın. Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı ada sahip, ardından çoğaltma sanal makinesi yük devretme, hedef alt ağa bağlanır. Eşleşen ada sahip bir hedef alt ağ ise sanal makine ağdaki ilk alt ağa bağlanır.

1. İlk komut sunucuları için geçerli Azure Site Recovery kasası alır. Komut Microsoft Azure Site Recovery sunucuları $Servers dizi değişkeninde depolar.

        $Servers = Get-AzureRmSiteRecoveryServer
2. İkinci komut site kurtarma ağ $Servers dizinin ilk sunucusunu alır. Komut ağları $Networks değişkeninde depolar.

        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

1. Üçüncü komut $AzureVmNetworks değişkeninde Azure sanal ağlar ve bu değeri alır.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork
2. Son cmdlet birincil ağ ve Azure sanal makine ağı arasında bir eşleme oluşturur. Cmdlet $Networks ilk öğe birincil ağı belirtir. Cmdlet, bir sanal makine ağına $AzureVmNetworks ilk öğe belirtir.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]

## <a name="step-9-enable-protection-for-virtual-machines"></a>9. adım: sanal makineler için korumayı etkinleştir
Sunucular, Bulutlar ve ağlar doğru şekilde yapılandırıldıktan sonra buluttaki sanal makineler için korumayı etkinleştirebilirsiniz.

 Şunlara dikkat edin:

* Sanal makineler Azure gereksinimleri karşılaması gerekir. Bu iade [Önkoşullar ve Destek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) Planlama Kılavuzu'nda.
* Koruma, işletim sistemi ve işletim sistemi etkinleştirmek için sanal makine için disk özellikleri de ayarlanmalıdır. Sana makine şablonu kullanarak VMM'de bir sanal makine oluşturduğunuzda özelliği de ayarlayabilirsiniz. Ayrıca var olan sanal makineler için bu özellikleri, sanal makinenin **Genel** ve **Donanım Yapılandırması** sekmelerinde de ayarlayabilirsiniz. Özellikleri VMM'de ayarlamazsanız Azure Site Recovery portalından da yapılandırabilirsiniz.

1. Korumayı etkinleştirmek için koruma kapsayıcısı almak için aşağıdaki komutu çalıştırın:

          $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName
2. Korunan varlık (VM), aşağıdaki komutu çalıştırarak alın:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer
3. DR VM için aşağıdaki komutu çalıştırarak etkinleştirin:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1

## <a name="test-your-deployment"></a>Dağıtımınızı test edin
Dağıtımınızı test etmek için test yük devretme için tek bir sanal makine çalıştırmak veya birden çok sanal makine içeren bir kurtarma planı oluşturmak ve test yük devretme plan için çalıştırın. Test yük devretme, yük devretme ve kurtarma mekanizmanızı yalıtılmış bir ağda benzetimini yapar. Şunlara dikkat edin:

* Sonra Yük devretme Uzak Masaüstü'nü kullanarak azure'daki sanal makineye bağlanmak istiyorsanız, yük devretme testini çalıştırmadan önce sanal makinede Uzak Masaüstü Bağlantısı etkinleştirin.
* Yük devretme, Uzak Masaüstü'nü kullanarak azure'daki sanal makineye bağlanmak için bir ortak IP adresini kullanacaksınız. Bu işlemi gerçekleştirmek isterseniz genel bir adres kullanarak sanal makineye bağlanmanızı engelleyecek etki alanı ilkelerine sahip olmadığınızdan emin olun.

İşlemin tamamlanması denetlemek için adımları [etkinliğini izleme](#monitor).

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
- Aşağıdaki komutu çalıştırarak test yük devretme başlatın:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Planlı yük devretmeyi çalıştırma
- Aşağıdaki komutu çalıştırarak planlanmış yük devretme başlatın:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Planlanmamış bir yük devretmeyi çalıştırma
- Aşağıdaki komutu çalıştırarak planlanmamış yük devretme başlatın:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

## <a name=monitor></a>Etkinliğini izleme
Etkinliğini izlemek için aşağıdaki komutları kullanın. Bitirmek işleme işleri arasında beklemek zorunda unutmayın.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi](/powershell/module/azurerm.recoveryservices.backup/#recovery) Azure Site Recovery ile Azure Resource Manager PowerShell cmdlet'leri hakkında.
