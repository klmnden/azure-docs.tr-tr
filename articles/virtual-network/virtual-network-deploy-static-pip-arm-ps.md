---
title: Bir statik genel IP adresiyle - Azure PowerShell VM oluşturma | Microsoft Docs
description: PowerShell kullanarak bir statik genel IP adresiyle VM oluşturma konusunda bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68656db0b76a29e7ab36fd6fa9ad4647712233ee
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38696592"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-powershell"></a>PowerShell kullanarak bir statik genel IP adresiyle VM oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (Klasik)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makale Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft'un önerdiği Resource Manager dağıtım modelini incelemektedir.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="start-your-script"></a>Betiğinizi Başlat
Kullanılan tam PowerShell betiğini indirebilirsiniz [burada](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1). Ortamınızda çalışması için komut dosyasını değiştirmek için aşağıdaki adımları izleyin.

Dağıtımınız için kullanmak istediğiniz değerleri temel alarak aşağıdaki değişkenlerin değerlerini değiştirin. Bu makalede kullanılan senaryo aşağıdaki değerleri eşleyin:

```powershell
# Set variables resource group
$rgName                = "IaaSStory"
$location              = "West US"

# Set variables for VNet
$vnetName              = "WTestVNet"
$vnetPrefix            = "192.168.0.0/16"
$subnetName            = "FrontEnd"
$subnetPrefix          = "192.168.1.0/24"

# Set variables for storage
$stdStorageAccountName = "iaasstorystorage"

# Set variables for VM
$vmSize                = "Standard_A1"
$diskSize              = 127
$publisher             = "MicrosoftWindowsServer"
$offer                 = "WindowsServer"
$sku                   = "2012-R2-Datacenter"
$version               = "latest"
$vmName                = "WEB1"
$osDiskName            = "osdisk"
$nicName               = "NICWEB1"
$privateIPAddress      = "192.168.1.101"
$pipName               = "PIPWEB1"
$dnsName               = "iaasstoryws1"
```

## <a name="create-the-necessary-resources-for-your-vm"></a>Sanal Makineniz için gerekli kaynakları oluşturma
VM oluşturmadan önce bir kaynak grubu, sanal ağ, genel IP ve NIC VM tarafından kullanılacak gerekir.

1. Yeni bir kaynak grubu oluşturun.

    ```powershell
    New-AzureRmResourceGroup -Name $rgName -Location $location
    ```

2. VNet ve alt ağ oluşturun.

    ```powershell
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
        -AddressPrefix $vnetPrefix -Location $location

    Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
        -VirtualNetwork $vnet -AddressPrefix $subnetPrefix

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

3. Genel IP kaynağı oluşturun. 

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
        -AllocationMethod Static -DomainNameLabel $dnsName -Location $location
    ```

4. Genel IP ile yukarıda oluşturulan alt ağ içinde VM için ağ arabirimi (NIC) oluşturun. İlk cmdlet VNet Azure'dan alınırken dikkat edin, bu yana gereklidir bir `Set-AzureRmVirtualNetwork` mevcut bir VNet değiştirmek için yürütüldü.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
        -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
        -PublicIpAddress $pip
    ```

5. VM işletim sistemi sürücüsünü barındırmak için bir depolama hesabı oluşturun.

    ```powershell
    $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
    -ResourceGroupName $rgName -Type Standard_LRS -Location $location
    ```

## <a name="create-the-vm"></a>Sanal makine oluşturma
Tüm gerekli kaynakları yerinde olduğuna göre yeni bir VM oluşturabilirsiniz.

1. Sanal makine için yapılandırma nesnesi oluşturun.

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    ```

2. VM yerel yönetici hesabı için kimlik bilgilerini alın.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password for the local administrator account."
    ```

3. Bir VM yapılandırma nesnesi oluşturun.

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```

4. VM için işletim sistemi görüntüsünü ayarlayın.

    ```powershell
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
        -Offer $offer -Skus $sku -Version $version
    ```

5. İşletim sistemi diski yapılandırın.

    ```powershell
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    ```

6. NIC, sanal Makineye ekleyin.

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary
    ```

7. Bir VM oluşturun.

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location
    ```

8. Komut dosyasını kaydedin.

## <a name="run-the-script"></a>Betiği çalıştırın

Gerekli değişiklikleri yaptıktan sonra önceki betiği çalıştırın. Birkaç dakika sonra sanal makine oluşturulur.

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içinde IP adreslerini ayarlayın

Hiçbir zaman el ile bir Azure sanal makinesi sanal makinenin işletim sistemi içinde atanan genel IP adresi atamanız gerekir. Önerilen, statik olarak bir sanal makinenin işletim sistemi içinde Azure sanal makinesine atanmış özel IP sürece atamayın, gerekli olduğunda gibi [birden çok IP adresleri atama bir Windows VM'ye](virtual-network-multiple-ip-addresses-powershell.md). İşletim sistemi içinde özel IP adresini el ile ayarlamanız durumunda Azure atanmış özel IP adresiyle aynı adresi olduğundan emin olun [ağ arabirimi](virtual-network-network-interface-addresses.md#change-ip-address-settings), ya da sanal makineye bağlantı kaybedebilir. Daha fazla bilgi edinin [özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede oluşturulan VM'ye gelen ve giden ağ trafiğini akabilir. Ağ arabirimi, alt ağ veya her ikisi de gelen ve giden akış trafiğini sınırlandırmak gelen ve giden güvenlik kuralları bir ağ güvenlik grubu içinde tanımlayabilirsiniz. Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik grubu genel bakış](security-overview.md).