---
title: "Yönetilen bir VM görüntüden Azure VM oluşturma | Microsoft Docs"
description: "Resource Manager dağıtım modelinde Azure PowerShell kullanarak genelleştirilmiş yönetilen VM görüntüsü Windows sanal makine oluşturun."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.openlocfilehash: 8157b77976a2152cc0b012fe6ad5fa116223bef6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Yönetilen bir görüntüden bir VM oluşturma

Azure yönetilen bir VM görüntüsünden birden çok VM oluşturabilirsiniz. Yönetilen bir VM görüntüsü, işletim sistemi ve veri diskleri de dahil olmak üzere, bir VM oluşturmak gerekli bilgileri içerir. İşletim sistemi ve tüm veri disklerle hem de dahil olmak üzere görüntüyü oluşturan, yönetilen diskleri olarak depolanan VHD'ler. 


## <a name="prerequisites"></a>Ön koşullar

Zaten olmasına gerek [yönetilen bir VM görüntüsü oluşturulan](capture-image-resource.md) yeni VM oluşturmak için kullanılacak. 

AzureRM.Compute ve AzureRM.Network PowerShell modülleri en son sürümüne sahip olduğunuzdan emin olun. PowerShell komut istemini yönetici olarak açın ve bunları yüklemek için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Install-Module AzureRM.Compute,AzureRM.Network
```
Daha fazla bilgi için bkz: [Azure PowerShell sürüm](/powershell/azure/overview).



## <a name="collect-information-about-the-image"></a>Görüntü ile ilgili bilgileri toplama

İlk olarak kimliğinizi görüntü ile ilgili temel bilgileri toplamak ve görüntü için bir değişken oluşturmak gerekiyor. Bu örnek adında yönetilen bir VM görüntüsü kullanan **myImage** alanında **myResourceGroup** kaynak grubunda **Batı Orta ABD** konumu. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
VNet ve alt ağı oluşturmak [sanal ağ](../../virtual-network/virtual-networks-overview.md).

1. Alt ağ oluşturun. Bu örnek adlı bir alt ağı oluşturur **mySubnet** adres öneki ile **10.0.0.0/24**.  
   
    ```azurepowershell-interactive
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Sanal ağı oluşturun. Bu örnek adlı bir sanal ağ oluşturur **myVnet** adres öneki ile **10.0.0.0/16**.  
   
    ```azurepowershell-interactive
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Ortak IP adresi ve ağ arabirimi oluşturma

Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Bir ortak IP adresi oluşturun. Bu örnek adlı ortak IP adresi oluşturur **myPip**. 
   
    ```azurepowershell-interactive
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. NIC oluşturun Bu örnek, adlandırılmış bir NIC oluşturur **myNic**. 
   
    ```azurepowershell-interactive
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma

RDP kullanarak VM'nizi oturum açmak bağlantı noktası 3389 üzerinde RDP erişimine izin veren bir ağ güvenlik kuralı (NSG) olması gerekir. 

Bu örnekte adlı bir NSG oluşturulur **myNsg** adlı kuralı içeren **myRdpRule** 3389 numaralı bağlantı noktası RDP trafiğine izin veren. Nsg'ler hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Azure'da bir VM için bağlantı noktaları açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```azurepowershell-interactive
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Sanal ağ için bir değişken oluşturun

Tamamlanan sanal ağ için bir değişken oluşturun. 

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a>VM için kimlik bilgilerini alma

Aşağıdaki cmdlet'i yeni bir kullanıcı adı ve parola VM uzaktan erişmek için yerel yönetici hesabı olarak kullanılacak gireceğiniz bir pencere açılır. 

```azurepowershell-interactive
$cred = Get-Credential
```

## <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>VM adı, bilgisayar adını ve VM boyutu değişkenlerini ayarlama

1. VM adı ve bilgisayar adı için değişkenler oluşturun. Bu örnek VM adı olarak ayarlar **myVM** ve bilgisayar adı olarak **Bilgisayarım**.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Sanal makine boyutunu ayarlayın. Bu örnekte **Standard_DS1_v2** VM boyuta sahip. Bkz: [VM boyutları](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) daha fazla bilgi için.

    ```azurepowershell-interactive
    $vmSize = "Standard_DS1_v2"
    ```

3. Sanal makine adını ve boyutunu VM yapılandırmasına ekleyin.

```azurepowershell-interactive
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>VM görüntüsü yeni VM için kaynak görüntü olarak ayarlayın

Yönetilen VM görüntüsü Kimliğini kullanarak kaynak görüntüsü ayarlayın.

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>İşletim sistemi yapılandırması ayarlayın ve NIC ekleyin

Depolama türü (PremiumLRS veya StandardLRS) ve işletim sistemi disk boyutu girin. Bu örnekte hesap türünü ayarlar **PremiumLRS**, disk boyutu **128 GB** ve disk önbelleğe alma **ReadWrite**.

```azurepowershell-interactive
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>Sanal makine oluşturma

Biz oluşturulan ve depolanan yapılandırmayı kullanarak yeni Vm oluşturma **$vm** değişkeni.

```azurepowershell-interactive
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>VM oluşturulduğunu doğrulayın
Tamamlandığında, yeni oluşturulan VM görmelisiniz [Azure portal](https://portal.azure.com) altında **Gözat** > **sanal makineleri**, veya aşağıdaki PowerShell komutlarını kullanarak:

```azurepowershell-interactive
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizi Azure PowerShell ile yönetmek için bkz: [oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

