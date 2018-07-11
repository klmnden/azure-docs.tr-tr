---
title: Virtual Machine Manager bulutlarındaki Hyper-V Vm'lerini PowerShell (Azure Resource Manager) ile ikincil bir siteye çoğaltmak | Microsoft Docs
description: Virtual Machine Manager bulutlarındaki Hyper-V Vm'lerini PowerShell (Resource Manager) kullanarak Virtual Machine Manager ikincil bir siteye çoğaltma işlemi açıklanır
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: sutalasi
ms.openlocfilehash: 0fecc7ba48daf396c3d25969cdda5891bdf08232
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917974"
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-by-using-powershell-resource-manager"></a>PowerShell (Resource Manager) aracılığıyla Hyper-V Vm'lerini ikincil bir siteye çoğaltma

Bu makalede, Hyper-V Vm'lerini ikincil bir Virtual Machine Manager buluta System Center Virtual Machine Manager bulutlarındaki çoğaltılması site kullanarak şirket için adımları otomatikleştirmek gösterilmektedir [Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Önkoşullar

- Gözden geçirme [senaryo mimarisini ve bileşenlerini](hyper-v-vmm-architecture.md).
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-sec-site.md) gözden geçirin.
- Virtual Machine Manager sunucuları ve Hyper-V konakları ile uyumlu olduğundan emin [destek gereksinimlerini](site-recovery-support-matrix-to-sec-site.md).
- Çoğaltmak istediğiniz VM'lerin uyumlu onay [makine desteği çoğaltılan](site-recovery-support-matrix-to-sec-site.md).


## <a name="prepare-for-network-mapping"></a>Ağ eşlemesi için hazırlanma

[Ağ eşlemesini](hyper-v-vmm-network-mapping.md) arasındaki eşlemeleri şirket içi kaynak ile hedef Bulutlar Virtual Machine Manager VM ağları. Eşleme şunları yapar:

- Vm'leri yük devretme sonrasında uygun hedef VM ağlarına bağlar. 
- En uygun şekilde hedef Hyper-V ana bilgisayar sunucuları üzerinde çoğaltma Vm'leri yerleştirir. 
- Ağ eşlemesini yapılandırmazsanız, çoğalma VM'ler için bir VM ağı yük devretmeden sonra bağlanması gerekmez.

Virtual Machine Manager aşağıdaki gibi hazırlayın:

* Olduğundan emin olun [Virtual Machine Manager Mantıksal ağlar](https://docs.microsoft.com/system-center/vmm/network-logical) kaynak ve hedef Virtual Machine Manager sunuculara:

    - Mantıksal ağ kaynak sunucudaki Hyper-V konaklarının bulunduğu kaynak bulutla ilişkili olmalıdır.
    - Hedef sunucudaki mantıksal ağı hedef bulutla ilişkili olmalıdır.
* Olduğundan emin olun [VM ağları](https://docs.microsoft.com/system-center/vmm/network-virtual) kaynak ve hedef Virtual Machine Manager sunucularında. VM ağları her konumdaki mantıksal ağ bağlantılı olması gerekir.
* Kaynak VM ağına kaynak Hyper-V konaklarında VM'ler bağlanın. 

## <a name="prepare-for-powershell"></a>PowerShell için hazırlama

Azure PowerShell kullanıma hazır olduğundan emin olun:

- PowerShell, 0.8.10 sürüme yükseltin veya üzerini kullanıyorsanız. [Daha fazla bilgi edinin](/powershell/azureps-cmdlets-docs) PowerShell'i ayarlama hakkında.
- Ayarlama ve PowerShell yapılandırma sonra gözden [hizmet cmdlet'leri](/powershell/azure/overview).
- Parametre değerlerini, girdileri ve çıktıları içinde PowerShell kullanma hakkında daha fazla bilgi edinmek için [başlama](/powershell/azure/get-started-azureps) Kılavuzu.

## <a name="set-up-a-subscription"></a>Bir aboneliği ayarlama
1. Powershell'den Azure hesabınızda oturum açın.

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Connect-AzureRmAccount #-Credential $Cred
2. Abonelik kimlikleri ile aboneliklerinizin bir listesini alın. Kurtarma Hizmetleri kasası oluşturmak istediğiniz abonelik Kimliğini not alın. 

        Get-AzureRmSubscription
3. Kasa için abonelik ayarlayın.

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
1. Yoksa, bir Azure Resource Manager kaynak grubu oluşturun.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Yeni bir kurtarma Hizmetleri kasası oluşturun. Kasa nesnesi daha sonra kullanılmak üzere bir değişkene kaydedin. 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Get-AzureRMRecoveryServicesVault cmdlet'ini kullanarak oluşturduktan sonra kasayı nesnesi alabilirsiniz.

## <a name="set-the-vault-context"></a>Kasa bağlamını ayarlayın
1. Mevcut bir kasayı alın.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Kasa bağlamını ayarlayın.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-site-recovery-provider"></a>Site Recovery Sağlayıcısı'nı yükleyin
1. Virtual Machine Manager makinesinde aşağıdaki komutu çalıştırarak bir dizin oluşturun:

       New-Item c:\ASR -type directory
2. İndirilen sağlayıcı kurulum dosyasını kullanarak dosyaları ayıklayın.

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Sağlayıcıyı yükleyin ve yüklemenin tamamlanmasını bekleyin.

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

4. Sunucuyu kasaya kaydedin.

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Oluşturma ve bir çoğaltma ilkesi ilişkilendirme
1. Bir çoğaltma ilkesi, bu durumda Hyper-V 2012 R2 için şu şekilde oluşturun:

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
    > Windows Server'ın farklı sürümlerini çalıştıran Hyper-V konaklarını Virtual Machine Manager cloud içerebilir, ancak çoğaltma ilkesi belirli bir işletim sistemi sürümü için olan. Farklı işletim sistemleri üzerinde çalışan farklı konaklarınız varsa, her sistemi için ayrı bir çoğaltma ilkesi oluşturun. Örneğin, Windows Server 2012 ve Windows Server 2012 R2 üzerinde çalışan üç konakları üzerinde çalışan beş konaklarınız varsa, iki çoğaltma ilkesi oluşturun. Her işletim sistemi türü için bir tane oluşturun.

2. Birincil koruma kapsayıcısı (birincil Virtual Machine Manager Bulutu) ve kurtarma koruma kapsayıcısı (Kurtarma Virtual Machine Manager cloud) alın.

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
3. Kolay adı kullanılarak oluşturulan çoğaltma ilkesini alın.

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
4. Çoğaltma İlkesi ile ilişkisini koruma kapsayıcısı (Virtual Machine Manager Bulutu) başlatın.

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
5. Bitirmek ilke ilişkilendirmesi işin tamamlanmasını bekleyin. İş tamamlandıktan denetlemek için aşağıdaki PowerShell kod parçacığını kullanın:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

6. İşleme işi tamamlandıktan sonra aşağıdaki komutu çalıştırın:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

İşlemin tamamlanması denetlemek için adımları [etkinliğini İzle](#monitor-activity).

##  <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma
1. Geçerli kasa için sunucuları almak için bu komutu kullanın. Komut, Site Recovery sunucuları $Servers dizi değişkeninde depolar.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Kaynak Virtual Machine Manager sunucusu ve hedef Virtual Machine Manager sunucusu için ağları almak için şu komutu çalıştırın.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Kaynak Virtual Machine Manager sunucusu, sunucu dizideki ilk veya ikinci bir olabilir. Virtual Machine Manager sunucu adlarını denetleyin ve ağları uygun şekilde alın.


3. Bu cmdlet, birincil ağ ile kurtarma ağı arasında bir eşleme oluşturur. Bu birincil ağ $PrimaryNetworks ilk öğesi belirtir. Bu kurtarma ağı $RecoveryNetworks ilk öğesi belirtir.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>VM'ler için korumayı etkinleştirin
Sunucular, Bulutlar ve ağlar doğru şekilde yapılandırıldıktan sonra buluttaki sanal makineler için korumayı etkinleştirin.

1. Korumayı etkinleştirmek için koruma kapsayıcısı almak için aşağıdaki komutu çalıştırın:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Korunan varlık (VM), aşağıdaki gibi alın:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Sanal makine için çoğaltmayı etkinleştirin.

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Dağıtımı test etmek için tek bir sanal makine bir yük devretme testi çalıştırın. Ayrıca birden çok VM içeren bir kurtarma planı oluşturabilir ve planı için bir yük devretme testi çalıştırın. Yük devretme testi, yük devretme işleminizi ve kurtarma mekanizmanızı yalıtılmış bir ortamda benzetimli olarak gerçekleştirir.

1. Al, hangi sanal makinelerin VM'ye üzerinde başarısız olur.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2. Yük devretme testi gerçekleştirin.

   Tek bir VM için:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
   Kurtarma planı için:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

İşlemin tamamlanması denetlemek için adımları [etkinliğini İzle](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Planlanmış ve planlanmamış yük devretme çalıştırma

1. Planlı yük devretme gerçekleştirin.

   Tek bir VM için:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Kurtarma planı için:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

2. Planlanmamış yük devretme gerçekleştirin.

   Tek bir VM için:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Kurtarma planı için:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Etkinliğini izleme
Yük devretme etkinliğini izlemek için aşağıdaki komutları kullanın. İşleme işleri arasında bitmesini bekleyin.

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

[Daha fazla bilgi edinin](/powershell/module/azurerm.recoveryservices.backup/#recovery) Resource Manager PowerShell cmdlet'leri ile Site Recovery hakkında.
