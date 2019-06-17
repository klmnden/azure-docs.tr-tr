---
title: PowerShell ve Azure Resource Manager'ı kullanarak Hyper-V Vm'leri için Azure'da olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Azure'da olağanüstü durum kurtarma Hyper-V vm'lerini PowerShell ve Azure Resource Manager kullanarak Azure Site Recovery hizmeti ile otomatik hale getirin.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: 5fbe4fd5f85026cd62f1bd10e36561b312464054
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64690562"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>PowerShell ve Azure Resource Manager'ı kullanarak Hyper-V Vm'leri için Azure'da olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) çoğaltma, yük devretme ve Azure sanal makineleri (VM'ler) kurtarma işlemlerini düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur ve şirket içi Vm'leri ve fiziksel sunucuları.

Bu makalede, Hyper-V Vm'lerini Azure'a çoğaltma için Windows PowerShell, Azure Resource Manager ile birlikte kullanmayı açıklar. Bu makalede kullanılan örnek bir Hyper-V konağına, Azure üzerinde çalışan tek bir VM'yi çoğaltma gösterilmektedir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell cmdlet'lerini Windows PowerShell kullanarak Azure yönetmenizi sağlar. Site Recovery PowerShell cmdlet, Azure PowerShell için Azure Resource Manager ile kullanılabilir korumak ve sunucularınızı azure'da kurtarmanıza yardımcı olur.

Bu makalede kullanmak için bir PowerShell uzman olmanız gerekmez, ancak modüller, cmdlet'ler ve oturumlar gibi temel kavramları anlamanız gerekir. Okuma [Windows PowerShell ile çalışmaya başlama](https://technet.microsoft.com/library/hh857337.aspx), ve [Azure PowerShell kullanarak Azure Resource Manager ile](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft iş ortaklarına bulut çözümü sağlayıcısı (CSP) programında yapılandırabilir ve ilgili CSP aboneliklerini (Kiracı abonelikleri) müşteri sunuculara koruma yönetebilirsiniz.
>
>

## <a name="before-you-start"></a>Başlamadan önce
Bu önkoşulların sağlandığından emin olun:

* A [Microsoft Azure](https://azure.microsoft.com/) hesabı. [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz. Ayrıca, okuyabilirsiniz [Azure Site Recovery Manager fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell. Bu sürüm ve nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Azure PowerShell yükleme](/powershell/azure/install-az-ps).

Ayrıca, bu makalede açıklanan özel bir örnek, aşağıdaki önkoşulları vardır:

* Windows Server 2012 R2 ya da bir veya daha fazla Vm'leri içeren Microsoft Hyper-V Server 2012 R2 çalıştıran bir Hyper-V konağı. Hyper-V sunucularının, doğrudan veya bir ara sunucu üzerinden Internet'e bağlanmalıdır.
* Çoğaltmak istediğiniz VM'lerin karşılaması gerektiğini [bu Önkoşullar](hyper-v-azure-support-matrix.md#replicated-vms).

## <a name="step-1-sign-in-to-your-azure-account"></a>1\. adım: Azure hesabınızda oturum açma

1. Bir PowerShell konsolu açın ve Azure hesabınızda oturum açmak için şu komutu çalıştırın. Cmdlet getirir, bir web sayfası için hesap kimlik bilgilerinizi ister: **Connect AzAccount**.
    - Alternatif olarak, bir parametre olarak bir hesap kimlik bilgilerinizi içerebilir **Connect AzAccount** cmdlet'ini kullanarak **-Credential** parametresi.
    - Müşteri, Kiracı adına çalışma CSP iş ortağıysanız, Kiracı kimliği veya Kiracı birincil etki alanı adlarını kullanarak Kiracı olarak belirtin. Örneğin: **Connect AzAccount-Kiracı "fabrikam.com"**
2. Hesabınız birden fazla abonelik olabilir bu yana hesabı ile kullanmak istediğiniz aboneliği ilişkilendirin:

    `Select-AzSubscription -SubscriptionName $SubscriptionName`

3. Aboneliğiniz için kurtarma Hizmetleri ve şu komutları kullanarak Site Recovery, Azure sağlayıcılarını kullanacak şekilde kaydedildiğini doğrulayın:

    `Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`

4. Komut çıktısında, doğrulayın **RegistrationState** ayarlanır **kayıtlı**, 2. adım geçebilirsiniz. Aksi durumda, bu komutları çalıştırarak eksik sağlayıcısı, aboneliğinizde kaydolmalıdır:

    `Register-AzResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Aşağıdaki komutları kullanarak sağlayıcılar başarıyla kayıtlı doğrulayın:

    `Get-AzResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`

## <a name="step-2-set-up-the-vault"></a>2\. adım: Kasası ayarlama

1. Kasa oluşturulacağı bir Azure Resource Manager kaynak grubu oluşturun veya mevcut bir kaynak grubunu kullanın. Yeni bir kaynak grubu oluşturun. $ResourceGroupName değişkeni oluşturmak istediğiniz kaynak grubunun adını, ve $Geo değişkeni (örneğin, "Brezilya Güney") kaynak grubunun oluşturulacağı Azure bölgesi içerir.

    `New-AzResourceGroup -Name $ResourceGroupName -Location $Geo`

2. Çalıştırın, aboneliğinizdeki kaynak gruplarının bir listesini almak için **Get-AzResourceGroup** cmdlet'i.
2. Şu şekilde yeni bir Azure kurtarma Hizmetleri kasası oluşturun:

        $vault = New-AzRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    İle varolan kasaların listesini alabilirsiniz **Get-AzRecoveryServicesVault** cmdlet'i.


## <a name="step-3-set-the-recovery-services-vault-context"></a>3\. adım: Kurtarma Hizmetleri kasa bağlamını ayarlayın

Kasa bağlamını şu şekilde ayarlayın:

`Set-AsrVaultSettings -Vault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>4\. Adım: Bir Hyper-V sitesi oluşturun

1. Yeni bir Hyper-V sitesi şu şekilde oluşturun:

        $sitename = "MySite"                #Specify site friendly name
        New-AsrFabric -Type HyperVSite -Name $sitename

2. Bu cmdlet siteyi oluşturmak için bir Site kurtarma işini başlatır ve Site Recovery iş nesnesi döndürür. İş başarıyla tamamlandı tamamlamak ve doğrulamak işin tamamlanmasını bekleyin.
3. Kullanım **Get-AsrJob cmdlet'i**, iş nesnesinde alıp işinin geçerli durumunu denetleyin.
4. Oluşturma ve bir kayıt anahtarı site için aşağıdaki gibi indirin:

    ```
    $SiteIdentifier = Get-AsrFabric -Name $sitename | Select -ExpandProperty SiteIdentifier
    $path = Get-AzRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename
    ```

5. İndirilen anahtarı, Hyper-V ana bilgisayara kopyalayın. Hyper-V konağı sitesine kaydetmek için anahtarı gerekir.

## <a name="step-5-install-the-provider-and-agent"></a>5\. Adım: Sağlayıcı ve aracı yükleme

1. Yükleyici adresinden sağlayıcının en son sürümü için indirme [Microsoft](https://aka.ms/downloaddra).
2. Hyper-V konağında yükleyiciyi çalıştırın.
3. Yüklemenin sonunda kayıt adıma geçin.
4. İstendiğinde, indirilen anahtarı sağlayın ve Hyper-V konağının kaydını tamamlayın.
5. Hyper-V ana siteye şu şekilde kaydedildiğini doğrulayın:

        $server =  Get-AsrFabric -Name $siteName | Get-AsrServicesProvider -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy"></a>6\. Adım: Çoğaltma ilkesi oluşturma

Başlamadan önce belirtilen depolama hesabı kasasıyla aynı Azure bölgesinde olmalıdır ve coğrafi çoğaltmanın etkinleştirilmiş olması unutmayın.

1. Bir çoğaltma ilkesi şu şekilde oluşturun:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AsrPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Çoğaltma İlkesi oluşturma başarılı olmak için döndürülen işine bakın.

3. Siteye şu şekilde karşılık gelen koruma kapsayıcısı Al:

        $protectionContainer = Get-AsrProtectionContainer
3. Koruma kapsayıcısı gibi çoğaltma ilkesiyle ilişkilendirin:

     $Policy AsrPolicy get - FriendlyName $PolicyName $associationJob = New-AsrProtectionContainerMapping =-ad $mappingName-ilke $Policy - PrimaryProtectionContainer $protectionContainer [0]

4. Başarıyla tamamlamak ilişkilendirme işin tamamlanmasını bekleyin.

## <a name="step-7-enable-vm-protection"></a>7\. Adım: VM korumasını etkinleştirin

1. Aşağıdaki şekilde, korumak istediğiniz sanal Makineye karşılık gelen bir korunabilir öğe alınamıyor:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $ProtectableItem = Get-AsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Sanal makine koruyun. Koruduğunuz VM bağlı birden fazla disk varsa, işletim sistemi diski kullanarak belirtin *OSDiskName* parametresi.

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = New-AsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS Windows -RecoveryResourceGroupId

3. İlk çoğaltma sonrasında korumalı bir duruma ulaşmak sanal makineleri bekleyin. Bu, Azure'a çoğaltılması gereken veri miktarı ve kullanılabilir Yukarı Akış bant genişliği gibi faktörlere bağlı olarak biraz zaman alabilir. Korumalı bir duruma yerinde olduğunda, iş durumu ve StateDescription şu şekilde güncelleştirilir:

        PS C:\> $DRjob = Get-AsrJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Kurtarma özellikleri (örneğin, VM rolü boyut) ve Azure ağı, yük devretme sonrasında VM NIC eklemek güncelleştirin.

        PS C:\> $nw1 = Get-AzVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $rpi = Get-AsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AsrReplicationProtectedItem --InputObject $rpi -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AsrJob -Job $UpdateJob

        PS C:\> $UpdateJob| select -ExpandProperty state
        Get-AsrJob -Job $job | select -ExpandProperty state

        Succeeded



## <a name="step-8-run-a-test-failover"></a>8\. adım: Yük devretme testi çalıştırma
1. Yük devretme testi aşağıdaki gibi çalıştırın:

        $nw = Get-AzVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $rpi = Get-AsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        $TFjob =Start-AsrTestFailoverJob -ReplicationProtectedItem $VM -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Test, Azure'da VM oluşturulduğunu doğrulayın. Azure'da VM test oluşturduktan sonra test yük devretme işi askıya alındı.
3. Temizleme ve test yük devrini tamamlamak için şunu çalıştırın:

        $TFjob = Start-AsrTestFailoverCleanupJob -ReplicationProtectedItem $rpi -Comment "TFO done"

## <a name="next-steps"></a>Sonraki adımlar
[Daha fazla bilgi edinin](https://docs.microsoft.com/powershell/module/az.recoveryservices) Azure Site Recovery ile Azure Resource Manager PowerShell cmdlet'leri hakkında.
