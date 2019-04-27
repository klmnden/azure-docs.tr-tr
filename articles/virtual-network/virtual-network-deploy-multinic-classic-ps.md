---
title: Birden çok NIC ile - Azure PowerShell (Klasik) VM oluşturma | Microsoft Docs
description: PowerShell kullanarak birden çok NIC ile VM (Klasik) oluşturmayı öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 6e50f39a-2497-4845-a5d4-7332dbc203c5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 087b52bd603e8aed6078ab340e84c1f6bd0e8082
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60748520"
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a>PowerShell kullanarak birden çok NIC ile VM (Klasik) oluşturma

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Azure'da sanal makineler (VM) oluşturma ve birden çok ağ arabirimine (NIC) sanal makinelerinizin her birine ekleyin. Birden çok NIC NIC'ler arasında trafik türlerinin ayrımı sağlar. Örneğin, yalnızca iç kaynaklara Internet'e bağlı olmayan başka bir iletişim kurar, ancak bir NIC Internet ile iletişim kuran. Birden çok NIC arasında ağ trafiğini ayırmak olanağı, uygulama teslimi ve WAN iyileştirme çözümleri gibi birçok ağ sanal Gereçleri için gereklidir.

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bu adımları kullanarak gerçekleştirmeyi öğrenin [Resource Manager dağıtım modeli](../virtual-machines/windows/multiple-nics.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Adlı bir kaynak grubu aşağıdaki adımları kullanın *IaaSStory* WEB sunucuları ve bir kaynak grubu için adlı *IaaSStory arka uç* DB sunucuları için.

## <a name="prerequisites"></a>Önkoşullar

DB sunucuları oluşturabilmeniz için önce oluşturmanız gerekir *IaaSStory* bu senaryo için gerekli tüm kaynakları kaynak grubu. Bu kaynakları oluşturmak için aşağıdaki adımları tamamlayın. İçindeki adımları izleyerek bir sanal ağ oluşturma [sanal ağ oluşturma](virtual-networks-create-vnet-classic-netcfg-ps.md) makalesi.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a>Arka uç VM oluşturma
Arka uç sanal makinelerin oluşturulmasını aşağıdaki kaynaklar üzerinde bağlıdır:

* **Arka uç alt ağını**. Veritabanı sunucuları, trafiği ayırmak için ayrı bir alt bölümü olacaktır. Aşağıdaki betiği bu alt ağ adlı bir sanal ağda bulunmasını bekliyor *WTestVnet*.
* **Veri diskleri için depolama hesabı**. Daha iyi performans için veritabanı sunucularında veri diskleri gerektiren bir premium depolama hesabı, katı hal sürücüsü (SSD) teknolojisi kullanır. Premium depolamayı destekler dağıttığınız Azure konumu emin olun.
* **Kullanılabilirlik kümesi**. Tüm veritabanı sunucuları, sanal makinelerin en az biri çalışır durumda emin olmak için ayarlayabilir ve bakım sırasında çalışan tek bir kullanılabilirlik eklenir.

### <a name="step-1---start-your-script"></a>1. adım - betiğinizi Başlat
Kullanılan tam PowerShell betiğini indirebilirsiniz [burada](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Ortamınızda çalışması için komut dosyasını değiştirmek için aşağıdaki adımları izleyin.

1. Yukarıda dağıtılmış mevcut kaynak grubunuzun göre aşağıdaki değişkenlerin değerlerini değiştirmek [önkoşulları](#prerequisites).

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. Arka uç dağıtımınız için kullanmak istediğiniz değerleri temel alarak aşağıdaki değişkenlerin değerlerini değiştirin.

    ```powershell
    $backendCSName         = "IaaSStory-Backend"
    $prmStorageAccountName = "iaasstoryprmstorage"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $diskSize              = 127
    $vmNamePrefix          = "DB"
    $dataDiskSuffix        = "datadisk"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>2. adım - sanal makineleriniz için gerekli kaynakları oluşturma
Tüm VM'ler için yeni bir bulut hizmeti ve veri diskleri için depolama hesabı oluşturmanız gerekir. VM'ler için bir görüntü ve yerel yönetici hesabı belirtmeniz gerekir. Bu kaynakları oluşturmak için aşağıdaki adımları tamamlayın:

1. Yeni bir bulut hizmeti oluşturun.

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. Yeni bir premium depolama hesabı oluşturun.

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. Aboneliğiniz için geçerli bir depolama hesabı, yukarıda oluşturduğunuz depolama hesabını ayarlayın.

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. VM için bir görüntü seçin.

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. Yerel yönetici hesabının kimlik bilgilerini ayarlayın.

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a>3. adım - VM oluşturma
Bir döngü ve döngü içinde gerekli NIC'ler ve VM'ler oluşturma gibi birçok VM oluşturmak için kullanmanız gerekir. NIC'ler ve VM'ler oluşturmak için aşağıdaki adımları uygulayın.

1. Başlangıç bir `for` değerine göre bir VM ile gerekli sayıda iki NIC oluşturmak için komutları yinelemek için döngü `$numberOfVMs` değişkeni.

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. Oluşturma bir `VMConfig` görüntüsü, boyutu ve VM'nin kullanılabilirlik kümesini belirten nesne.

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. VM, bir Windows VM sağlayın.

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. ' % S'varsayılan NIC ve statik IP adresi atayın.

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. Her VM için ikinci NIC ekleyin.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```
    
6. Veri diskleri için her bir VM oluşturun.

    ```powershell
    $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk1Name `
    -LUN 0

    $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk2Name `
    -LUN 1
    ```

7. Her VM oluşturma ve döngü bitmelidir.

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-the-script"></a>Adım 4 - betiği çalıştırın
İndirilen ve değiştirilmiş betiği, birden çok NIC içeren VM'ler arka uç veritabanı oluşturmak için komut dosyası çalışma ihtiyaçlarınıza.

1. Kodunuzu kaydedin ve çalıştırın **PowerShell** komut istemi veya **PowerShell ISE**. İlk çıkış, aşağıda gösterildiği gibi göreceksiniz.

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. ' A tıklayın ve kimlik bilgileri istemi gerekli bilgileri doldurun **Tamam**. Aşağıdaki çıktı döndürülür.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded

### <a name="step-5---configure-routing-within-the-vms-operating-system"></a>5. adım - sanal makinenin işletim sistemi içinde yönlendirmeyi yapılandırma

Azure DHCP, varsayılan bir ağ geçidi sanal makinesine bağlı ilk (birincil) ağ arabirimine atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimlerine varsayılan ağ geçidi atamaz. Bu nedenle varsayılan olarak, alt ağın dışında kalan ve ikincil bir ağ arabirimi içeren kaynaklarla iletişim kurulamaz. Ancak, ikincil ağ arabirimleri kendi alt ağlarının dışında kalan kaynaklarla iletişim kurabilir. İkincil ağ arabirimleri için yönlendirmeyi yapılandırmak için aşağıdaki makalelere bakın:

- [Bir Windows VM için birden çok NIC yapılandırın](../virtual-machines/windows/multiple-nics.md#configure-guest-os-for-multiple-nics
)

- [Bir Linux VM birden çok NIC için yapılandırma](../virtual-machines/linux/multiple-nics.md#configure-guest-os-for-multiple-nics
)
