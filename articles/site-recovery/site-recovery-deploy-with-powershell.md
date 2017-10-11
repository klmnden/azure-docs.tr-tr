---
title: "PowerShell ile klasik portalda Hyper-V Vm'lerini Azure'a çoğaltma | Microsoft Docs"
description: "Klasik portalda Site Recovery ve PowerShell kullanarak VMM bulutlarındaki Hyper-V sanal makinelerini çoğaltma otomatikleştirme"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 581daaaa5cc0cf8be782f834c6bdb3f27ee413fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-hyper-v-vms-to-azure-with-powershell-in-the-classic-portal"></a>Klasik portalda Azure PowerShell ile Hyper-V Vm'lerini çoğaltma
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell - Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klasik Portal](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell - Klasik](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Genel Bakış
Azure Site Recovery, çoğaltma, yük devretme ve kurtarma çeşitli dağıtım senaryolarında sanal makinelerin düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sağlar. Dağıtım tam listesi için bkz: senaryoları [Azure Site Recovery genel bakış](site-recovery-overview.md).

Bu makalede Azure depolama alanına System Center VMM bulutlarındaki Hyper-V sanal makineleri çoğaltmak için Azure Site Recovery ayarladığınızda, yapmanız gereken ortak görevleri otomatik hale getirmek için PowerShell kullanmayı gösterir.

Makaleyi senaryonun önkoşulları içerir ve Site Recovery kasasını oluşturup, kaynak VMM sunucusunda Azure Site kurtarma Sağlayıcısı'nı yükleme, sunucuyu kasaya kaydetmek, bir Azure depolama hesabı ekleme hakkında yükleme Azure kurtarma gösterir Hizmetleri aracısını Hyper-V konak sunucuları üzerindeki tüm korumalı sanal makineler için uygulanır ve bu sanal makineler için korumayı etkinleştir VMM Bulutları için koruma ayarlarını yapılandırın. Her şeyin istendiği gibi çalıştığından emin olmak için yük devretmeyi test ederek işlemi sonlandırın.

Bu senaryoyu oluşturan ayarlama sorunlarını alıyorsanız, üzerinde sorularınızı [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır.
>
>

## <a name="before-you-start"></a>Başlamadan önce
Bu Önkoşullar sağladığınızdan emin olun:

### <a name="azure-prerequisites"></a>Azure önkoşulları
* Bir [Microsoft Azure](https://azure.microsoft.com/) hesabınızın olması gerekir. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
* Çoğaltılan verileri depolamak için bir Azure depolama hesabınızın olması gerekir. Hesap coğrafi çoğaltmanın etkinleştirilmiş olmalıdır. Bu Azure Site Recovery kasasıyla aynı bölgede olması ve aynı abonelikle ilişkilendirilmiş olması gerekir. [Azure storage hakkında daha fazla bilgi](../storage/common/storage-introduction.md).
* Korumak istediğiniz sanal makineleri ile uyumlu olduğundan emin olmak gerekir [Azure sanal makine önkoşulları](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

### <a name="vmm-prerequisites"></a>VMM önkoşulları
* System Center 2012 R2 üzerinde çalışan VMM sunucusu gerekir.
* Korumak istediğiniz VMM sunucusunda en az bir bulut gerekir. Bulut içermelidir:
  * Bir veya birden çok VMM ana bilgisayar grubu.
  * Bir veya daha fazla Hyper-V ana bilgisayar sunucuları veya kümeleri her konak grubundaki.
  * Kaynak Hyper-V sunucu üzerindeki bir veya daha fazla sanal makineler.

### <a name="hyper-v-prerequisites"></a>Hyper-V önkoşulları
* Hyper-V ana bilgisayar sunucuları en az çalıştırmalıdır **Windows Server 2012** Hyper-V rolüne sahip veya **Microsoft Hyper-V Server 2012** ve en son güncelleştirmelerin yüklü olması.
* Hyper-V'yi bir kümede çalıştırıyorsanız statik IP adresi temelli bir kümeye sahip olmanız durumunda küme aracısının otomatik olarak oluşturulamayacağını unutmayın. Küme aracısını sizin yapılandırmanız gerekir. Bu, Sunucu Yöneticisi'nde yapmak için > Yük Devretme Kümesi Yöneticisi ' ni kümeye bağlanın, tıklatın **rolü Yapılandır** seçip **Hyper-V çoğaltma aracısı** içinde **rolü Seç** Yüksek Kullanılabilirlik Sihirbazı'nın ekranı.
* Herhangi bir Hyper-V konak sunucusu veya küme koruma yönetmek istediğiniz bir VMM bulutunda eklenmesi gerekir.

### <a name="network-mapping-prerequisites"></a>Ağ eşlemesi önkoşulları
Azure'da sanal makineleri koruduğunuzda, ağ eşlemesi kaynak VMM sunucusundaki VM ağlarını hedef Azure ağları ile eşleyerek şu işlemlere olanak sağlar:

* Aynı ağda yük devri tüm makineler birbirine, hangi kurtarma planında yer aldıklarına bulundukları bağlanabilir.
* Bir ağ geçidi hedef Azure ağında ayarlanırsa sanal makineler diğer şirket içi sanal makinelere bağlanabilir.
* Ağ eşlemesini yapılandırmazsanız yalnızca aynı kurtarma planında yük devreden sanal makineler, Azure'a yük devretme işleminin ardından birbirleriyle bağlantı kurabilir.

Ağ eşlemesi dağıtmak isterseniz şunlara ihtiyaç duyarsınız:

* Kaynak VMM sunucusunda korumak istediğinizde sanal makinelerin bir VM ağına bağlanması gerekir. Bu ağın, bulutla ilişkilendirilen mantıksal bir ağ ile bağlantılı olması gerekir.
* Yük devretme işleminin ardından çoğaltılan sanal makinelerin bağlanabileceği bir Azure ağı. Bu ağı yük devretme sırasında seçersiniz. Ağın Azure Site Recovery aboneliğinizle aynı bölgede olması gerekir.

### <a name="powershell-prerequisites"></a>PowerShell önkoşulları
Azure PowerShell davranmaya hazır olduğundan emin olun. PowerShell zaten kullanıyorsanız, 0.8.10 sürümüne yükseltme veya üstü gerekir. PowerShell ayarlama hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azureps-cmdlets-docs). Ayarlama ve PowerShell yapılandırılmış sonra tüm kullanılabilir cmdlet'leri hizmeti görüntüleyebilirsiniz [burada](/powershell/azure/overview).

Parametre değerleri, girişleri ve çıkışları genellikle Azure PowerShell'de işlenme gibi cmdlet'leri kullanın yardımcı olabilecek ipuçları hakkında bilgi edinmek için bkz [Azure cmdlet'leri ile çalışmaya başlama](/powershell/azure/get-started-azureps).

## <a name="step-1-set-the-subscription"></a>1. adım: Abonelik ayarlama
PowerShell'de bu cmdlet'leri çalıştırın:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

"< >" İçinde öğelerin size özgü bilgileri ile değiştirin.

## <a name="step-2-create-a-site-recovery-vault"></a>2. adım: Site Recovery kasası oluşturma
PowerShell, "< >" içinde öğelerin belirli bilgilerinizle değiştirin ve bu komutları çalıştırın:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>3. adım: kasa kayıt anahtarı oluşturma
Kasada bir kayıt anahtarı oluşturun. Azure Site Recovery Sağlayıcısı'nı indirdikten sonra VMM sunucusuna yükleyin, oluşturduğunuz anahtarı kasadaki VMM sunucusuna kaydolmak için kullanacaksınız.

1. Kasa ayar dosyasını alın ve bağlamını ayarlayın:

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. Aşağıdaki komutları çalıştırarak kasası bağlamını ayarlayın:

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>4. adım: Azure Site kurtarma Sağlayıcısı'nı yükleme
1. VMM makinesinde aşağıdaki komutu çalıştırarak bir dizin oluşturun:

   ```

   pushd C:\ASR\

   ```
2. Aşağıdaki komutu çalıştırarak indirilen sağlayıcısını kullanarak dosyaları ayıklayın

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. Aşağıdaki komutları kullanarak sağlayıcısını yükleyin:

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   Yüklemesinin tamamlanması için bekleyin.
4. Sunucu aşağıdaki komutu kullanarak kasaya kaydedin:

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a>5. adım: Azure storage hesabı oluşturma
Bir Azure depolama hesabınız yoksa, aşağıdaki komutu çalıştırarak bir coğrafi çoğaltmanın etkinleştirilmiş hesabı oluşturun:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Depolama hesabı gerekir Azure Site Recovery hizmeti ile aynı bölgede olması ve aynı abonelikle ilişkilendirilmiş olduğunu unutmayın.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>6. adım: Azure kurtarma Hizmetleri Aracısı yükleme
Azure portalından, korumak istediğiniz VMM bulutlarında yer alan her Hyper-V ana bilgisayar sunucusunda Azure kurtarma Hizmetleri Aracısı'nı yükleyin.

Tüm VMM ana bilgisayarlarda aşağıdaki komutu çalıştırın:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>7. adım: Bulut yapılandırma koruma ayarları
1. Azure bulut koruması profil, aşağıdaki komutu çalıştırarak oluşturun:

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. Koruma kapsayıcısı aşağıdaki komutları çalıştırarak alın:

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. Koruma kapsayıcısı ilişkisini bulutla başlatın:

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. İş tamamlandıktan sonra aşağıdaki komutu çalıştırın:

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. İş işlemeyi bitirdikten sonra aşağıdaki komutu çalıştırın:

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



İşlemin tamamlanması denetlemek için adımları [etkinliğini izleme](#monitor).

## <a name="step-8-configure-network-mapping"></a>8. adım: ağ eşlemesini yapılandırma
Ağ eşlemesine başlamadan önce kaynak VMM sunucusundaki sanal makinelerin bir VM ağına bağlı olduğunu doğrulayın. Ayrıca, bir veya daha fazla Azure sanal ağı oluşturun. Birden çok VM ağının tek bir Azure ağına eşlenebileceğini unutmayın.

Hedef ağın birden çok alt ağı varsa ve bu alt ağlardan biri kaynak sanal makinenin bulunduğu alt ağ ile aynı adı taşıyorsa çoğaltılan sanal makinenin, yük devretme işleminin ardından hedef alt ağa bağlandığını unutmayın. Eşleşen bir ada sahip bir hedef alt ağa değilse, sanal makine ağdaki ilk alt ağa bağlanır.

İlk komut sunucuları için geçerli Azure Site Recovery kasası alır. Komut Microsoft Azure Site Recovery sunucuları $Servers dizi değişkeninde depolar.

    $Servers = Get-AzureSiteRecoveryServer


İkinci komut site kurtarma ağ $Servers dizinin ilk sunucusunu alır. Komut ağları $Networks değişkeninde depolar.

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Üçüncü komut Get-AzureSubscription cmdlet'ini kullanarak, Azure aboneliklerinize alır ve ardından bu değer $Subscriptions değişkeninde depolar.

    $Subscriptions = Get-AzureSubscription



Dördüncü komut $AzureVmNetworks değişkeninde Get-AzureVNetSite cmdlet'i ve bu değer'ı kullanarak Azure sanal ağlar alır.

    $AzureVmNetworks = Get-AzureVNetSite



Son cmdlet birincil ağ ve Azure sanal makine ağı arasında bir eşleme oluşturur. Cmdlet $Networks ilk öğe birincil ağı belirtir. Cmdlet'in kendi kimliğini kullanarak bir sanal makine ağ $AzureVmNetworks ilk öğe belirtir Bu komut Azure abonelik kimliğinizi içerir

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>9. adım: sanal makineler için korumayı etkinleştir
Sunucular, bulutlar ve ağlar doğru şekilde yapılandırıldıktan sonra buluttaki sanal makineler için korumayı etkinleştirebilirsiniz. Şunlara dikkat edin:

Sanal makineler karşılamalıdır [Azure sanal makine önkoşulları](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

Korumayı etkinleştirmek için işletim sisteminin ve işletim sistemi diski özelliklerinin sanal makineye göre ayarlanmış olması gerekir. Sana makine şablonu kullanarak VMM'de bir sanal makine oluşturduğunuzda özelliği de ayarlayabilirsiniz. Ayrıca var olan sanal makineler için bu özellikleri, sanal makinenin **Genel** ve **Donanım Yapılandırması** sekmelerinde de ayarlayabilirsiniz. Özellikleri VMM'de ayarlamazsanız Azure Site Recovery portalından da yapılandırabilirsiniz.

1. Korumayı etkinleştirmek için koruma kapsayıcısı almak için aşağıdaki komutu çalıştırın:

     $ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-ad $CloudName
2. Korunan varlık (VM), aşağıdaki komutu çalıştırarak alın:

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. DR VM için aşağıdaki komutu çalıştırarak etkinleştirin:

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a>Dağıtımınızı test edin
Dağıtımınızı test etmek için bir yük devretme sınaması için tek bir sanal makine çalıştırmak veya birden çok sanal makine içeren bir kurtarma planı oluşturmak ve bir yük devretme sınaması için plan çalıştırın. Yük devretme testi, yük devretme işleminizi ve kurtarma mekanizmanızı yalıtılmış bir ortamda benzetimli olarak gerçekleştirir. Şunlara dikkat edin:

* Yük devretmenin ardından Azure'daki sanal makineye Uzak Masaüstü kullanarak bağlanmak isterseniz yük devretme testini çalıştırmadan önceden sanal makinenizde Uzak Masaüstü Bağlantısı'nı etkinleştirin.
* Yük devretme sonrasında, Uzak Masaüstü'nü kullanarak azure'daki sanal makineye bağlanmak için bir ortak IP adresini kullanacaksınız. Bu işlemi gerçekleştirmek isterseniz genel bir adres kullanarak sanal makineye bağlanmanızı engelleyecek etki alanı ilkelerine sahip olmadığınızdan emin olun.

İşlemin tamamlanması denetlemek için adımları [etkinliğini izleme](#monitor).

### <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma
1. Verileri kullanarak kurtarma planınız için bir şablon olarak bir .xml dosyası oluşturun ve ardından "C:\RPTemplatePath.xml" kaydedin.
2. RecoveryPlan düğüm kimliği, ad, PrimaryServerId ve SecondaryServerId değiştirin.
3. ProtectionEntity düğüm PrimaryProtectionEntityId (vmm'den vmıd) değiştirin.
4. Daha fazla VM'ler daha fazla ProtectionEntity düğüm ekleyerek ekleyebilirsiniz.

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. Şablondaki veri doldurun:

        $TemplatePath = "C:\RPTemplatePath.xml";



1. RecoveryPlan oluşturun:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
1. Aşağıdaki komutu çalıştırarak RecoveryPlan nesnesini alın:

     $RPObject = get-AzureSiteRecoveryRecoveryPlan-ad $RPName;
2. Aşağıdaki komutu çalıştırarak test yük devretme başlatın:

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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
[Daha fazla bilgi](/powershell/azure/overview) Azure Site Recovery PowerShell cmdlet'leri hakkında. </a>.
