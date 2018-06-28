---
title: Birden çok NIC - Azure PowerShell ile bir VM'ye (Klasik) oluşturun | Microsoft Docs
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
ms.date: 05/22/2018
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca4e9e77d0e0ca62c04fbbfe132a41fb3e01df46
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "34658783"
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a>PowerShell kullanarak birden çok NIC ile VM (Klasik) oluşturun

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Azure'da sanal makineleri (VM'ler) oluşturun ve her Vm'leriniz için birden çok ağ arabirimlerine (NIC'ler) ekleyin. Birden çok NIC NIC'ler arasında trafik türlerini ayrılması etkinleştirin. Örneğin, yalnızca Internet'e bağlı iç kaynaklara kurduğu başka bir sırada bir NIC Internet ile iletişim kuran. Birden çok NIC'ler arasında ağ trafiğini ayırmak olanağı uygulama sunma ve WAN iyileştirmesi çözümleri gibi birçok ağ sanal uygulamaları için gereklidir.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Kullanarak bu adımların nasıl gerçekleştirileceğini öğrenin [Resource Manager dağıtım modeli](../virtual-machines/windows/multiple-nics.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Adlı bir kaynak grubu aşağıdaki adımları kullanın *IaaSStory* WEB sunucuları ve bir kaynak grubu için adlı *IaaSStory arka uç* DB sunucuları için.

## <a name="prerequisites"></a>Önkoşullar

DB sunucuları oluşturabilmeniz için önce oluşturmanız gerekir *IaaSStory* bu senaryo için gereken tüm kaynakların kaynak grubuyla. Bu kaynakları oluşturmak için izlediği adımları tamamlayın. İçindeki adımları izleyerek bir sanal ağ oluşturma [bir sanal ağ oluşturma](virtual-networks-create-vnet-classic-netcfg-ps.md) makalesi.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a>Arka uç VM'ler oluşturma
Arka uç VM'ler aşağıdaki kaynaklara oluşturulmasını bağlıdır:

* **Arka uç alt**. Veritabanı sunucuları trafiği kurabilmeleri için ayrı bir alt parçası olacak. Aşağıdaki komut dosyası bu alt ağ adlı bir vnet mevcut bekliyor *WTestVnet*.
* **Veri diskleri için depolama hesabı**. Daha iyi performans için veritabanı sunucularında veri diskler bir premium depolama hesabı gerektirir katı hal sürücüsü (SSD) teknolojisi kullanır. Premium depolama desteklemek için dağıtmanız Azure konumu emin olun.
* **Kullanılabilirlik kümesi**. Tüm veritabanı sunucuları, en az bir sanal makineleri çalışır durumda emin olmak için ayarlayın ve bakım sırasında çalıştıran tek bir kullanılabilirlik eklenir.

### <a name="step-1---start-your-script"></a>1. adım - kodunuzu Başlat
Kullanılan tam PowerShell komut dosyası indirebilirsiniz [burada](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Ortamınızda çalışması için komut dosyasını değiştirmek için aşağıdaki adımları izleyin.

1. Yukarıdaki dağıtılmış varolan kaynak grubunuz temelinde değişkenleri aşağıdaki değerlerini değiştirmek [Önkoşullar](#Prerequisites).

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. Arka uç dağıtımınız için kullanmak istediğiniz değerleri temel alarak aşağıdaki değişkenlerinin değerlerini değiştirin.

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

### <a name="step-2---create-necessary-resources-for-your-vms"></a>2. adım - Vm'leriniz için gerekli kaynakları oluşturma
Tüm VM'ler için yeni bir bulut hizmeti ve veri diskleri için bir depolama hesabı oluşturmanız gerekir. Ayrıca VM'ler için bir görüntü ve yerel yönetici hesabı belirtmeniz gerekir. Bu kaynakları oluşturmak için aşağıdaki adımları tamamlayın:

1. Yeni bir bulut hizmeti oluşturun.

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. Yeni bir premium depolama hesabı oluşturun.

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. Aboneliğiniz için geçerli depolama hesabı, yukarıda oluşturduğunuz depolama hesabı ayarlayın.

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

### <a name="step-3---create-vms"></a>3. adım - VMs oluşturma
Ve gerekli NIC ve sanal makineleri döngüye içinde gibi birçok VM oluşturmak için bir döngü kullanmanız gerekir. NIC ve sanal makineleri oluşturmak için aşağıdaki adımları yürütün.

1. Başlangıç bir `for` değerine göre bir VM ve gerektiği pek çok, iki NIC oluşturmak için komutları yinelemek için döngü `$numberOfVMs` değişkeni.

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. Oluşturma bir `VMConfig` görüntüsü, boyutu ve VM için ayarlanmış kullanılabilirlik belirtme nesnesi.

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

4. NIC varsayılan ayarlayabilir ve statik bir IP adresi atayabilirsiniz.

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. Her VM için ikinci bir NIC ekleyin.

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

7. Her bir VM oluşturun ve döngü sona erdirmek.

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-the-script"></a>Adım 4 - komut dosyasını çalıştır
İndirilen ve değiştirilen betik gereksinimlerinize bağlı olarak birden çok NIC VM'ler arka uç veritabanı oluşturmak için komut dosyası çalışma temel.

1. Komut dosyanızı kaydedin ve çalıştırın **PowerShell** komut istemi veya **PowerShell ISE**. Aşağıda gösterildiği gibi ilk çıkış göreceksiniz.

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. Kimlik bilgileri istemi ve tıklatın gerekli bilgileri doldurun **Tamam**. Aşağıdaki çıkış döndürülür.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded

### <a name="step-5---configure-routing-within-the-vms-operating-system"></a>5. adım - sanal makinenin işletim sistemi içinde yönlendirmeyi yapılandırma

Azure DHCP, sanal makineye bağlı ilk (birincil) ağ arabirimi için varsayılan ağ geçidi atar. Azure, bir sanal makineye bağlı ek (ikincil) ağ arabirimlerine varsayılan ağ geçidi atamaz. Bu nedenle varsayılan olarak, alt ağın dışında kalan ve ikincil bir ağ arabirimi içeren kaynaklarla iletişim kurulamaz. İkincil ağ arabirimlerine ancak, kendi alt ağı dışından kaynaklarla iletişim kurabilirsiniz. İkincil ağ arabirimleri için yönlendirmeyi yapılandırmak için aşağıdaki makalelere bakın:

- [Bir Windows VM için birden çok NIC yapılandırın](../virtual-machines/windows/multiple-nics.md#configure-guest-os-for-multiple-nics
)

- [Bir Linux VM birden çok NIC için yapılandırma](../virtual-machines/linux/multiple-nics.md#configure-guest-os-for-multiple-nics
)
