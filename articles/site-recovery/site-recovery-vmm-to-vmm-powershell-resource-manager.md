---
title: "İkincil bir siteye PowerShell (Azure Resource Manager) ile VMM Hyper-V Vm'lerini çoğaltma | Microsoft Docs"
description: "Çoğaltma, yük devretme ve VMM bulutlarındaki Hyper-V sanal makineleri Kurtarma (Resource Manager) PowerShell kullanarak bir ikincil VMM sitesi olarak düzenlemek için Azure Site Recovery dağıtmayı açıklar"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 5a6e00877b0a2b139d5322f610c1901ad76a710f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>VMM bulutlarındaki Hyper-V sanal makinelerini (Resource Manager) PowerShell kullanarak bir ikincil VMM siteye çoğaltma
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klasik Portal](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell - Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Azure Site Recovery'ye hoş geldiniz! Şirket içi Hyper-V sanal makineleri çoğaltmak istiyorsanız bu makalede ikincil bir siteyi System Center Virtual Machine Manager (VMM) bulutlarında yönetilen kullanın.

Bu makalede PowerShell System Center VMM bulutlarında ikincil site için System Center VMM bulutlarındaki Hyper-V sanal makineleri çoğaltmak için Azure Site Recovery ayarladığınızda, yapmanız gereken ortak görevleri otomatik hale getirmek için nasıl kullanılacağı gösterilmektedir.

Makaleyi senaryonun önkoşulları içerir ve gösterir

* Bir kurtarma Hizmetleri kasasını nasıl kurulur
* Kaynak VMM Sunucu ve hedef VMM sunucusunda Azure Site kurtarma Sağlayıcısı'nı yükleyin
* VMM sunucuları kasaya kaydetmek
* Çoğaltma İlkesi VMM bulutu için yapılandırın. Tüm korumalı sanal makineler için İlkesi'ndeki çoğaltma ayarları uygulanır
* Sanal makineler için korumayı etkinleştirin.
* Ayrı ayrı veya her şeyin beklendiği gibi çalıştığından emin olmak için kurtarma planının bir parçası olarak sanal makineleri yük devretme sınamasını.
* Planlanmış veya planlanmamış bir yük devretme VM'lerin ayrı ayrı veya her şeyin beklendiği gibi çalıştığından emin olmak için kurtarma planının bir parçası olarak gerçekleştirin.

Bu senaryoyu oluşturan ayarlama sorunlarını alıyorsanız, üzerinde sorularınızı [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki [dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md) kullanır: Azure Resource Manager ve klasik model. Ayrıca Azure iki portala sahiptir: Klasik dağıtım modelini destekleyen klasik Azure portalı ve her iki dağıtım modeline de destek sağlayan Azure portalı. Bu makalede Resource Manager dağıtım modeli anlatılmaktadır.
>
>

## <a name="on-premises-prerequisites"></a>Şirket içi önkoşullar
İşte şirket içi birincil ve ikincil siteler bu senaryoyu dağıtmak için gerekenler:

| **Önkoşullar** | **Ayrıntılar** |
| --- | --- |
| **VMM** |VMM sunucusu birincil sitedeki ve ikincil sitedeki VMM sunucusu dağıtmak öneririz.<br/><br/> Ayrıca [tek bir VMM sunucusundaki Bulutlar arasında çoğaltma](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). Bunu yapmak için VMM sunucusunda yapılandırılan en az iki bulut gerekir.<br/><br/> VMM sunucuları çalışıyor. en son güncelleştirmeleri içeren System Center 2012 SP1.<br/><br/> Her bir VMM sunucusunun tek sahip olmalı veya daha fazla bulut yapılandırılmış ve tüm Bulutlar Hyper-V Kapasite profili kümesine sahip olması gerekir. <br/><br/>Bulut bir veya daha fazla VMM ana bilgisayar grubu içermesi gerekir.<br/><br/>VMM bulutlarını ayarlama hakkında daha fazla bilgi [VMM bulut doku](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), ve [izlenecek yol: System Center 2012 SP1 VMM içeren özel bulut oluşturma](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM sunucuları Internet erişimine sahip olmalıdır. |
| **Hyper-V** |Hyper-V sunucuları çalışıyor olmalıdır en az Windows Server 2012 Hyper-V rolüne sahip ve en son güncelleştirmelerin yüklü olması.<br/><br/> Bir Hyper-V sunucusunun bir veya daha fazla VM içermesi gerekir.<br/><br/>  Hyper-V ana bilgisayar sunucularının birincil ve ikincil VMM bulutlarında konak gruplarındaki bulunmalıdır.<br/><br/> Windows Server 2012 R2'de bir kümede Hyper-V çalıştırıyorsanız, yüklemelisiniz [2961977 güncelleştir](https://support.microsoft.com/kb/2961977)<br/><br/> Bir kümede küme aracısının Windows Server 2012 Not üzerinde Hyper-V çalıştırıyorsanız, bir statik IP adresi tabanlı kümeniz varsa otomatik olarak oluşturulmaz. Küme aracısını sizin yapılandırmanız gerekir. [Daha fazla bilgi](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Sağlayıcı** |Site Recovery dağıtımı sırasında VMM sunucularına Azure Site kurtarma Sağlayıcısı'nı yükleyin. Sağlayıcı, çoğaltmayı düzenlemek için HTTPS 443 üzerinden Site Recovery ile iletişim kurar. Veri çoğaltma, LAN veya bir VPN bağlantısı üzerinden birincil ve ikincil Hyper-V sunucuları arasında oluşur.<br/><br/> VMM sunucusunda çalışan sağlayıcı bu URL'leri erişmesi: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.<br/><br/> Ayrıca VMM sunucularına gelen güvenlik duvarı iletişimi izin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653) ve HTTPS (443) protokolüne izin verin. |

### <a name="network-mapping-prerequisites"></a>Ağ eşlemesi önkoşulları
Ağ eşlemesi eşlemeleri VMM VM ağ için birincil ve ikincil VMM sunucularındaki arasında:

* En iyi şekilde çoğaltılan VM'ler üzerinde ikincil Hyper-V konakları yük devretme sonrasında yerleştirin.
* Çoğaltılan VM'ler için uygun VM ağları bağlayın.
* Ağ eşlemesi çoğaltma sanal makineleri yük devretme sonrasında herhangi bir ağa bağlanmayacaktır yapılandırmazsanız.
* Ağ kurmak istiyorsanız, Site kurtarması sırasında eşleme gerekenler dağıtım burada verilmiştir:

  * Kaynak Hyper-V ana bilgisayar sunucusundaki VM'lerin bir VMM VM ağına bağlı olduğundan emin olun. Bu ağın, bulutla ilişkilendirilen mantıksal bir ağ ile bağlantılı olması gerekir.
  * Kurtarma için kullanacağınız ikincil bulut yapılandırılmış karşılık gelen VM ağı olduğundan emin olun. Bir VM ağı ikincil bulutla ilişkilendirilen bir mantıksal ağ bağlantılı olması gerekir.

VMM ağları yapılandırma hakkında daha fazla bilgi makaleleri aşağıda

* [VMM'de Mantıksal ağlar yapılandırma](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [VMM'de VM ağları ve ağ geçitlerini yapılandırma](http://go.microsoft.com/fwlink/p/?LinkId=386308)

Ağ eşlemesinin nasıl çalıştığı hakkında [daha fazla bilgi edinin](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping).

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
1. Zaten yoksa, bir Azure Resource Manager kaynak grubu oluşturun

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Yeni bir kurtarma Hizmetleri kasası oluşturun ve oluşturulan ASR kasası nesnesi (daha sonra kullanılacak) bir değişkene kaydedin. Get-AzureRMRecoveryServicesVault cmdlet'ini kullanarak ASR kasası nesne post oluşturma de alabilirsiniz:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>3. adım: Kurtarma Hizmetleri kasası bağlamını ayarlayın
1. Önceden oluşturulmuş bir kasası varsa çalıştırmak kasa için komutu aşağıdaki.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Çalıştırarak kasası bağlamını ayarlayın aşağıda komutu.

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

## <a name="step-5-create-and-associate-a-replication-policy"></a>5. adım: Oluşturun ve bir çoğaltma ilkesi ilişkilendirin
1. Aşağıdaki komutu çalıştırarak bir 2012 R2 Hyper-V çoğaltma ilkesi oluşturun:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > VMM bulut (Hyper-V önkoşullarını belirtildiği gibi) Windows Server'ın farklı sürümlerini çalıştıran Hyper-V konakları içerebilir, ancak çoğaltma ilkesi OS belirli sürümüdür. Farklı işletim sistemi sürümlerinde çalışan farklı ana bilgisayarınız varsa, her tür işletim sistemi sürümü için ayrı çoğaltma ilkeleri oluşturun. İçin örn: Windows sunucuları 2012 ve Windows Server 2012 R2'de üç çalışan beş ana bilgisayarınız varsa, iki oluşturmak çoğaltma ilkeleri – bir işletim sistemi sürümleri her tür.

1. Birincil koruma kapsayıcısı (birincil VMM Bulutu) ve kurtarma koruma kapsayıcısı (Kurtarma VMM bulut) aşağıdaki komutları çalıştırarak alın:

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. İlke kolay adını kullanarak 1. adımda oluşturduğunuz İlke Al

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Koruma kapsayıcısı (VMM Bulutu) ilişkilendirme çoğaltma ilkesiyle başlatın:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. İlke ilişkilendirme işin tamamlanması için bekleyin. Aşağıdaki PowerShell parçacığını kullanarak iş tamamlanıp tamamlanmadığını kontrol edebilirsiniz.

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   İş işlemeyi bitirdikten sonra aşağıdaki komutu çalıştırın:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

İşlemin tamamlanması denetlemek için adımları [etkinliğini izleme](#monitor).

## <a name="step-6-configure-network-mapping"></a>6. adım: ağ eşlemesini yapılandırma
1. İlk komut sunucuları için geçerli Azure Site Recovery kasası alır. Komut Microsoft Azure Site Recovery sunucuları $Servers dizi değişkeninde depolar.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Kaynak VMM sunucusunu ve hedef VMM sunucusu için aşağıdaki komutları site kurtarma ağ alın.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Kaynak VMM sunucusunu ilk veya ikinci bir sunucuları dizisindeki olabilir. VMM sunucularının adlarını denetleyin ve ağlar uygun şekilde Al


1. Son cmdlet birincil ağ ve kurtarma ağ arasında bir eşleme oluşturur. Cmdlet birincil ağ $PrimaryNetworks ve $RecoveryNetworks ilk öğe olarak kurtarma ağ ilk öğe olarak belirtir.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a>7. adım: depolama eşlemesi yapılandırma
1. Aşağıdaki komutunu depolama sınıflandırmaları $storageclassifications değişkeni içine listesini alır.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. Aşağıdaki komutları $SourceClassificaion değişkeni ve hedef sınıflandırma $TargetClassification değişkeni içine içine kaynak sınıflandırma alır.

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > Kaynak ve hedef sınıflandırmaları dizideki her öğe olabilir. Çıkışına bakın kaynak ve hedef sınıflandırmaları $storageclassifications dizisindeki dizini bulmak için komutu aşağıdaki.

    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - FriendlyName özelliği, kimliği | Tablo Biçimlendir


1. Aşağıda cmdlet'ini kaynak sınıflandırma ve hedef sınıflandırma arasında bir eşleme oluşturur.

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a>8. Adım: Sanal makinelere yönelik korumayı etkinleştirme
Sunucular, Bulutlar ve ağlar doğru şekilde yapılandırıldıktan sonra buluttaki sanal makineler için korumayı etkinleştirebilirsiniz.

1. Korumayı etkinleştirmek için koruma kapsayıcısı almak için aşağıdaki komutu çalıştırın:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Korunan varlık (VM), aşağıdaki komutu çalıştırarak alın:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Sanal makine için çoğaltmayı aşağıdaki komutu çalıştırarak etkinleştirin:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a>Dağıtımınızı test edin
Dağıtımınızı test etmek için bir yük devretme sınaması için tek bir sanal makine çalıştırmak veya birden çok sanal makine içeren bir kurtarma planı oluşturmak ve bir yük devretme sınaması için plan çalıştırın. Yük devretme testi, yük devretme işleminizi ve kurtarma mekanizmanızı yalıtılmış bir ortamda benzetimli olarak gerçekleştirir.

> [!NOTE]
> Azure Portalı'nda, uygulamanız için bir kurtarma planı oluşturabilirsiniz.
>
>

İşlemin tamamlanması denetlemek için adımları [etkinliğini izleme](#monitor).

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
1. Çalıştırma Vm'leriniz için yük devretme sınamasını istediğiniz VM ağı almak için cmdlet'leri aşağıda.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Aşağıdakileri yaparak bir VM bir sınama yük devretmesi gerçekleştirin:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. Aşağıdakileri yaparak bir kurtarma planı bir sınama yük devretmesi gerçekleştirin:

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a>Planlı yük devretmeyi çalıştırma
1. Aşağıdakileri yaparak bir VM planlanmış bir yük devretme gerçekleştirin:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. Aşağıdakileri yaparak bir kurtarma planı planlanan bir yük devretme gerçekleştirin:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Planlanmamış bir yük devretmeyi çalıştırma
1. Aşağıdakileri yaparak bir VM planlanmamış yük devretme gerçekleştirin:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2., aşağıdakileri yaparak bir kurtarma planı planlanmamış yük devretme gerçekleştirin:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

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
