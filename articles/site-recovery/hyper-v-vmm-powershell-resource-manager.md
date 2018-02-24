---
title: "PowerShell (Azure Resource Manager) ile ikincil siteye VMM bulutlarındaki Hyper-V Vm'lerini çoğaltma | Microsoft Docs"
description: "PowerShell (Resource Manager) kullanarak bir ikincil VMM sitesi için VMM bulutlarındaki Hyper-V Vm'lerini çoğaltma açıklar"
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: a5e36546494223b20844303f3f76782746658411
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-using-powershell-resource-manager"></a>Hyper-V Vm'lerini (Resource Manager) PowerShell kullanarak ikincil bir siteye çoğaltma

System Center Virtual Machine Manager (VMM) bulutlarında Hyper-V sanal makineleri çoğaltma için ikincil bir şirket içi sitede VMM bulutu için adımları otomatikleştirmek için bu makalede gösterilmektedir kullanarak [Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Önkoşullar

- Gözden geçirme [senaryo mimarisi ve bileşenleri](hyper-v-vmm-architecture.md).
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-sec-site.md) gözden geçirin.
- VMM sunucuları ve Hyper-V konakları ile uyumlu olduğundan emin olun [destek gereksinimleri](site-recovery-support-matrix-to-sec-site.md).
- Çoğaltmak istediğiniz sanal makineleri uyması onay [çoğaltılan makine desteği](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)


## <a name="prepare-for-network-mapping"></a>Ağ eşlemesi için hazırlanma

[Ağ eşlemesi](hyper-v-vmm-network-mapping.md) arasındaki eşlemeleri şirket içi VMM VM ağları kaynak ve hedef bulutlarındaki. Eşleme şunları yapar:

- Sanal makineleri yük devretme sonrasında uygun hedef VM ağına bağlanır. 
- En iyi şekilde çoğaltılan VM'ler hedef Hyper-V ana bilgisayar sunucuları üzerinde yerleştirir. 
- Ağ eşlemesini yapılandırmazsanız, çoğaltma sanal makineleri yük devretme sonrasında bir VM ağa bağlanmayacaktır.

VMM aşağıdaki gibi hazırlayın:

1. Olduğundan emin olun [VMM mantıksal ağlarına](https://docs.microsoft.com/system-center/vmm/network-logical) kaynak ve hedef VMM sunucularında.
    - Mantıksal ağ kaynak sunucu üzerindeki Hyper-V konaklarının bulunduğu kaynak Bulutu ile ilişkili olmalıdır.
    - Hedef sunucudaki mantıksal ağ hedef Bulutu ile ilişkili olmalıdır.
1. Olduğundan emin olun [VM ağları](https://docs.microsoft.com/system-center/vmm/network-virtual) kaynak ve hedef VMM sunucularında. VM ağları her konumdaki mantıksal ağ bağlantılı olması gerekir.
2. Vm'leri kaynak Hyper-V konaklarının kaynak VM ağına bağlayın. 

## <a name="prepare-for-powershell"></a>PowerShell için hazırlama

Azure PowerShell davranmaya hazır olduğundan emin olun.

- PowerShell zaten kullanıyorsanız, 0.8.10 sürümüne yükseltme veya üstü gerekir.  [Daha fazla bilgi edinin](/powershell/azureps-cmdlets-docs) PowerShell için ayarlama hakkında.
- Ayarlama ve PowerShell yapılandırdıktan sonra gözden [hizmet cmdlet'leri](/powershell/azure/overview).
- Parametre değerleri, giriş ve çıkış Azure PowerShell kullanma hakkında daha fazla bilgi edinmek için okuma [Başlarken](/powershell/azure/get-started-azureps) Kılavuzu.

## <a name="set-up-a-subscription"></a>Bir aboneliği ayarlama
1. Azure PowerShell ' Azure hesabınızda oturum:

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Abonelik kimlikleri ile Aboneliklerin listesini alın. Kurtarma hizmeti kasasının oluşturmak istediğiniz abonelik kimliği unutmayın.   

        Get-AzureRmSubscription
3. Kasa için abonelik ayarlayın:

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
1. Yoksa, bir Azure Resource Manager kaynak grubu oluşturun.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Yeni bir kurtarma Hizmetleri kasası oluşturun ve daha sonra kullanılacak bir değişkende kasası nesne kaydedin: 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Get-AzureRMRecoveryServicesVault cmdlet'ini kullanarak oluşturduktan sonra kasa nesne alabilirsiniz.

## <a name="set-the-vault-context"></a>Kasa bağlamını ayarlayın
1. Varolan bir kasa Al:

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Kasa bağlamını ayarlayın:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-azure-site-recovery-provider"></a>Azure Site kurtarma Sağlayıcısı'nı yüklemek
1. VMM makinesinde aşağıdaki komutu çalıştırarak bir dizin oluşturun:

       New-Item c:\ASR -type directory
2. İndirilen sağlayıcı kurulum dosyasını kullanarak dosyaları ayıklayın: pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Bir sağlayıcı yükleyin ve yüklemesinin tamamlanması için bekleyin:

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

4. Sunucuyu kasaya kaydedin:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Oluşturun ve bir çoğaltma ilkesi ilişkilendirin
1. Bir çoğaltma ilkesi (Bu durumda Hyper-V 2012 R2 için) gibi oluşturun:

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
    > VMM bulut Windows Server'ın farklı sürümlerini çalıştıran Hyper-V konakları içerebilir, ancak çoğaltma ilkesi belirli bir işletim sistemi için sürümüdür. Farklı işletim sistemlerinde çalışan farklı ana bilgisayarınız varsa, her sistem için ayrı çoğaltma ilkeleri oluşturun. Örneğin, Windows sunucuları 2012 ve Windows Server 2012 R2'de üç çalışan beş ana bilgisayarınız varsa, iki çoğaltma ilkeleri – her tür işletim sistemi için bir tane oluşturun.

2. Birincil koruma kapsayıcısı (birincil VMM c) ve kurtarma koruma kapsayıcısı (Kurtarma VMM bulut) Al:

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Kolay adı kullanılarak oluşturulan çoğaltma ilkesi Al:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Koruma kapsayıcısı (VMM Bulutu) ilişkilendirme çoğaltma ilkesiyle başlatın:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. İlke ilişkilendirme işin tamamlanması için bekleyin. Aşağıdaki PowerShell parçacığını kullanarak iş tamamlanıp tamamlanmadığını denetleyebilirsiniz:

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

İşlemin tamamlanması denetlemek için adımları [etkinliğini İzle](#monitor-activity).

##  <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma
1. Geçerli kasa için sunucuları almak için bu komutu kullanın. Komut Azure Site Recovery sunucuları $Servers dizi değişkeninde depolar.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Bu komuta retrievet ağları için kaynak VMM sunucusunu ve hedef VMM sunucusunda çalıştırın.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Kaynak VMM sunucusunu sunucuları dizisindeki ilk veya ikinci bir olabilir. VMM Sunucu adlarını denetleyin ve ağlar uygun şekilde alın.


3. Bu cmdlet, birincil ağ ve kurtarma ağ arasında bir eşleme oluşturur. $PrimaryNetworks ilk öğe olarak birincil ağ ve kurtarma ağ $RecoveryNetworks ilk öğe belirtir.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>VM'ler için korumayı etkinleştir
Sunucular, Bulutlar ve ağlar doğru şekilde yapılandırıldıktan sonra buluttaki VM'ler için korumayı etkinleştirebilirsiniz.

1. Korumayı etkinleştirmek için koruma kapsayıcısı almak için aşağıdaki komutu çalıştırın:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Korunan varlık (VM) aşağıdaki gibi alın:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Sanal makine için çoğaltmayı etkinleştirin:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Bir yük devretme sınaması için tek bir sanal makine çalıştırmak veya birden çok VM içeren bir kurtarma planı oluşturun, dağıtımı test etmek ve ve bir yük devretme sınaması için plan çalıştırın. Yük devretme testi, yük devretme işleminizi ve kurtarma mekanizmanızı yalıtılmış bir ortamda benzetimli olarak gerçekleştirir.

1. Sanal makineleri içine üzerinden başarısız olur VM Al:

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Yük devretme testi aşağıdaki gibi gerçekleştirin:
    - Tek bir VM için

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
    - Kurtarma planı için:

        $recoveryplanname "test kurtarma planı" =

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

İşlemin tamamlanması denetlemek için adımları [etkinliğini İzle](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Planlanmış ve planlanmamış yük devretmeler çalıştırabilirsiniz

1. Planlanmış bir yük devretme aşağıdaki gibi gerçekleştirin:
    -  Tek bir VM için:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
    - Bir kurtarma planı

        $recoveryplanname "test kurtarma planı" =

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan
2. Planlanmamış bir yük devretme aşağıdaki gibi gerçekleştirin:
    - Tek bir VM için
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

    - Kurtarma planı için:

        $recoveryplanname "test kurtarma planı" =

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Etkinliğini izleme
Failver etkinliğini izlemek için aşağıdaki komutları kullanın. Bitirmek işleme işleri arasında beklemek zorunda unutmayın.

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

[Daha fazla bilgi edinin](/powershell/module/azurerm.recoveryservices.backup/#recovery) Site Recovery ile Azure Resource Manager PowerShell cmdlet'leri hakkında.
