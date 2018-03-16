---
title: "PowerShell (Azure Resource Manager) ile ikincil siteye Sanal Makine Yöneticisi bulutlarındaki Hyper-V Vm'lerini çoğaltma | Microsoft Docs"
description: "PowerShell (Resource Manager) kullanarak sanal makine yöneticisi bulutlarındaki Hyper-V Vm'lerini ikincil bir sanal makine yöneticisi siteye çoğaltmak açıklar"
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: ea4c2ed287619b92dba1b9b966cc0d52e0eb89c5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-by-using-powershell-resource-manager"></a>PowerShell (Resource Manager) kullanarak Hyper-V Vm'lerini ikincil bir siteye çoğaltma

Bu makale, Hyper-V Vm'lerini ikincil bir sanal makine yöneticisi bulutta System Center Virtual Machine Manager bulutlarında çoğaltmasını site kullanarak şirket için adımları otomatikleştirmek gösterilmiştir [Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Önkoşullar

- Gözden geçirme [senaryo mimarisi ve bileşenleri](hyper-v-vmm-architecture.md).
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-to-sec-site.md) gözden geçirin.
- Sanal Makine Yöneticisi sunucuları ve Hyper-V konakları ile uyumlu olduğundan emin olun [destek gereksinimleri](site-recovery-support-matrix-to-sec-site.md).
- Çoğaltmak istediğiniz sanal makineleri uyması onay [makine desteği çoğaltılan](site-recovery-support-matrix-to-sec-site.md).


## <a name="prepare-for-network-mapping"></a>Ağ eşlemesi için hazırlanma

[Ağ eşlemesi](hyper-v-vmm-network-mapping.md) arasındaki eşlemeleri şirket içi Sanal Makine Yöneticisi'ni VM ağları kaynak ve hedef bulutlarındaki. Eşleme şunları yapar:

- Sanal makineleri yük devretme sonrasında uygun hedef VM ağına bağlanır. 
- En iyi şekilde çoğaltılan VM'ler hedef Hyper-V ana bilgisayar sunucuları üzerinde yerleştirir. 
- Ağ eşlemesini yapılandırmazsanız, çoğaltma sanal makineleri yük devretme sonrasında bir VM ağa bağlanmayacaktır.

Sanal Makine Yöneticisi aşağıdaki gibi hazırlayın:

* Olduğundan emin olun [Sanal Makine Yöneticisi Mantıksal ağlar](https://docs.microsoft.com/system-center/vmm/network-logical) kaynak ve hedef Sanal Makine Yöneticisi sunucularda:

    - Mantıksal ağ kaynak sunucu üzerindeki Hyper-V konaklarının bulunduğu kaynak Bulutu ile ilişkili olmalıdır.
    - Hedef sunucudaki mantıksal ağ hedef Bulutu ile ilişkili olmalıdır.
* Olduğundan emin olun [VM ağları](https://docs.microsoft.com/system-center/vmm/network-virtual) kaynak ve hedef Sanal Makine Yöneticisi sunucularında. VM ağları her konumdaki mantıksal ağ bağlantılı olması gerekir.
* Vm'leri kaynak Hyper-V konaklarının kaynak VM ağına bağlayın. 

## <a name="prepare-for-powershell"></a>PowerShell için hazırlama

Azure PowerShell davranmaya hazır olduğundan emin olun:

- PowerShell, 0.8.10 sürümüne yükseltme veya üstünü kullanıyorsanız. [Daha fazla bilgi edinin](/powershell/azureps-cmdlets-docs) PowerShell ayarlama hakkında.
- Ayarlama ve PowerShell yapılandırdıktan sonra gözden [hizmet cmdlet'leri](/powershell/azure/overview).
- Parametre değerleri, giriş ve çıkış PowerShell'de kullanma hakkında daha fazla bilgi edinmek için okuma [başlama](/powershell/azure/get-started-azureps) Kılavuzu.

## <a name="set-up-a-subscription"></a>Bir aboneliği ayarlama
1. Powershell'den, Azure hesabınızda oturum açın.

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Abonelik kimlikleri ile Aboneliklerin listesini alın. Kurtarma Hizmetleri kasası oluşturmak istediğiniz abonelik Kimliğini not alın. 

        Get-AzureRmSubscription
3. Kasa için abonelik ayarlayın.

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma
1. Yoksa, bir Azure Resource Manager kaynak grubu oluşturun.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Yeni bir kurtarma Hizmetleri kasası oluşturun. Kasa nesnesi daha sonra kullanılmak üzere bir değişkene kaydedin. 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Get-AzureRMRecoveryServicesVault cmdlet'ini kullanarak oluşturduktan sonra kasa nesne alabilirsiniz.

## <a name="set-the-vault-context"></a>Kasa bağlamını ayarlayın
1. Varolan bir kasa alın.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Kasa bağlamını ayarlayın.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-site-recovery-provider"></a>Site Recovery Sağlayıcısı'nı yüklemek
1. Sanal Makine Yöneticisi makinede aşağıdaki komutu çalıştırarak bir dizin oluşturun:

       New-Item c:\ASR -type directory
2. İndirilen sağlayıcı kurulum dosyasını kullanarak dosyaları ayıklayın.

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Bir sağlayıcı yükleyin ve yüklemesinin tamamlanması için bekleyin.

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

## <a name="create-and-associate-a-replication-policy"></a>Oluşturun ve bir çoğaltma ilkesi ilişkilendirin
1. Bir çoğaltma ilkesi, bu durumda Hyper-V 2012 R2 için aşağıdaki gibi oluşturun:

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
    > Virtual Machine Manager Bulutu Windows Server'ın farklı sürümlerini çalıştıran Hyper-V konakları içerebilir, ancak çoğaltma ilkesi belirli bir işletim sistemi için sürümüdür. Farklı işletim sistemlerinde çalışan farklı ana bilgisayarınız varsa, her sistem için ayrı çoğaltma ilkeleri oluşturun. Örneğin, Windows Server 2012 ve Windows Server 2012 R2 üzerinde çalışan üç ana çalışan beş ana bilgisayarınız varsa, iki çoğaltma ilkesi oluşturun. Her tür işletim sistemi için bir tane oluşturun.

2. Birincil koruma kapsayıcısı (birincil Sanal Makine Yöneticisi bulut) ve kurtarma koruma kapsayıcısı (Kurtarma Virtual Machine Manager Bulutu) alın.

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
3. Kolay adı kullanılarak oluşturulan çoğaltma ilkesini alır.

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
4. Koruma kapsayıcısı (Sanal Makine Yöneticisi bulut) ilişkilendirme çoğaltma ilkesiyle başlatın.

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
5. İlke ilişkilendirme işi bitmesini bekleyin. İş tamamlandıktan denetlemek için aşağıdaki PowerShell parçacığını kullanın:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

6. İşleme iş tamamlandıktan sonra aşağıdaki komutu çalıştırın:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

İşlemin tamamlanması denetlemek için adımları [etkinliğini İzle](#monitor-activity).

##  <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma
1. Geçerli kasa için sunucuları almak için bu komutu kullanın. Komut, Site Recovery sunucuları $Servers dizi değişkeninde depolar.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Kaynak Sanal Makine Yöneticisi sunucunun ve hedef Sanal Makine Yöneticisi sunucunun ağları almak için şu komutu çalıştırın.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > Kaynak Sanal Makine Yöneticisi sunucunun ilk veya ikinci bir sunucu dizisindeki olabilir. Sanal Makine Yöneticisi'ni sunucu adlarını denetleyin ve ağlar uygun şekilde alın.


3. Bu cmdlet, birincil ağ ve kurtarma ağ arasında bir eşleme oluşturur. Birincil Ağ $PrimaryNetworks ilk öğe belirtir. Kurtarma ağ $RecoveryNetworks ilk öğe belirtir.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>VM'ler için korumayı etkinleştir
Sunucular, Bulutlar ve ağlar doğru şekilde yapılandırıldıktan sonra buluttaki VM'ler için korumayı etkinleştirin.

1. Korumayı etkinleştirmek için koruma kapsayıcısı almak için aşağıdaki komutu çalıştırın:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Korunan varlık (VM), aşağıdaki gibi alın:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Sanal makine için çoğaltmayı etkinleştirin.

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Dağıtımınızı test etmek için bir yük devretme sınaması için tek bir sanal makine çalıştırın. Ayrıca birden çok VM içeren bir kurtarma planı oluşturmak ve bir yük devretme sınaması için plan çalıştırın. Yük devretme testi, yük devretme işleminizi ve kurtarma mekanizmanızı yalıtılmış bir ortamda benzetimli olarak gerçekleştirir.

1. Sanal makineleri içine üzerinden başarısız olur VM alın.

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

## <a name="run-planned-and-unplanned-failovers"></a>Planlanmış ve planlanmamış yük devretmeler çalıştırabilirsiniz

1. Planlanmış bir yük devretme gerçekleştirin.

   Tek bir VM için:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Kurtarma planı için:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

2. Planlanmamış bir yük devretme gerçekleştirin.

   Tek bir VM için:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Kurtarma planı için:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Etkinliğini izleme
Yük devretme etkinliğini izlemek için aşağıdaki komutları kullanın. İşleme işleri arasında tamamlamak bekleyin.

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

[Daha fazla bilgi edinin](/powershell/module/azurerm.recoveryservices.backup/#recovery) Site Recovery Resource Manager PowerShell cmdlet'leri ile ilgili.
