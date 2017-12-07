---
title: "Azure yönetilen bir görüntüden VM oluşturma | Microsoft Docs"
description: "Azure PowerShell veya Azure portalında Resource Manager dağıtım modelinde kullanılarak genelleştirilmiş bir yönetilen görüntüsünden bir Windows sanal makine oluşturun."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: cynthn
ms.openlocfilehash: d8986c71fd0300af385750af898d620e6d3e1f24
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Yönetilen bir görüntüden bir VM oluşturma

Birden çok VM PowerShell veya Azure portalını kullanarak yönetilen bir VM görüntüsünü oluşturabilirsiniz. Yönetilen bir VM görüntüsü, işletim sistemi ve veri diskleri de dahil olmak üzere, bir VM oluşturmak gerekli bilgileri içerir. İşletim sistemi ve tüm veri disklerle hem de dahil olmak üzere görüntüyü oluşturan, yönetilen diskleri olarak depolanan VHD'ler. 

Zaten olmasına gerek [yönetilen bir VM görüntüsü oluşturulan](capture-image-resource.md) yeni VM oluşturmak için kullanılacak. 

## <a name="use-the-portal"></a>Portalı kullanma

1. [Azure portalı](https://portal.azure.com) açın.
2. Soldaki menüde seçin **tüm kaynakları**. Kaynaklar tarafından sıralayabilirsiniz **türü** görüntülerinizi kolayca bulmak için.
3. Listeden kullanmak istediğiniz görüntüyü seçin. Görüntü **genel bakış** sayfası açılır.
4. Tıklatın **+ Oluştur VM** menüsünde.
5. Sanal makine bilgilerini girin. Burada girilen kullanıcı adı ve parola, sanal makinede oturum açarken kullanılır. İşlem tamamlandığında **Tamam**’a tıklayın. Var olan bir Resrouce grubuna yeni bir VM oluşturun veya seçin **Yeni Oluştur** VM depolamak için yeni bir kaynak grubu oluşturmak için.
6. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. 
7. Altında **ayarları**, gerekli değişiklikleri yapın ve tıklayın **Tamam**. 
8. Özet sayfasında, görüntü adı için listelenen görmelisiniz **özel görüntü**. Tıklatın **Tamam** sanal makine dağıtımı başlatmak için.


## <a name="use-powershell"></a>PowerShell kullanma

### <a name="prerequisites"></a>Ön koşullar

AzureRM.Compute ve AzureRM.Network PowerShell modülleri en son sürümüne sahip olduğunuzdan emin olun. PowerShell komut istemini yönetici olarak açın ve bunları yüklemek için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Install-Module AzureRM.Compute,AzureRM.Network
```
Daha fazla bilgi için bkz: [Azure PowerShell sürüm](/powershell/azure/overview).



### <a name="collect-information-about-the-image"></a>Görüntü ile ilgili bilgileri toplama

İlk olarak kimliğinizi görüntü ile ilgili temel bilgileri toplamak ve görüntü için bir değişken oluşturmak gerekiyor. Bu örnek adında yönetilen bir VM görüntüsü kullanan **myImage** alanında **myResourceGroup** kaynak grubunda **Batı Orta ABD** konumu. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
VNet ve alt ağı oluşturmak [sanal ağ](../../virtual-network/virtual-networks-overview.md).

Alt ağ oluşturun. Bu örnek adlı bir alt ağı oluşturur **mySubnet** adres öneki ile **10.0.0.0/24**.  
   
```azurepowershell-interactive
$subnetName = "mySubnet"
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
    -Name $subnetName -AddressPrefix 10.0.0.0/24
```

Sanal ağı oluşturun. Bu örnek adlı bir sanal ağ oluşturur **myVnet** adres öneki ile **10.0.0.0/16**.  
   
```azurepowershell-interactive
$vnetName = "myVnet"
$vnet = New-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $rgName `
    -Location $location `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $singleSubnet
```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Ortak IP adresi ve ağ arabirimi oluşturma

Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

Bir ortak IP adresi oluşturun. Bu örnek adlı ortak IP adresi oluşturur **myPip**. 
   
```azurepowershell-interactive
$ipName = "myPip"
$pip = New-AzureRmPublicIpAddress `
    -Name $ipName `
    -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod Dynamic
```
       
NIC oluşturun Bu örnek, adlandırılmış bir NIC oluşturur **myNic**. 
   
```azurepowershell-interactive
$nicName = "myNic"
$nic = New-AzureRmNetworkInterface `
    -Name $nicName `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id
```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma

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


### <a name="create-a-variable-for-the-virtual-network"></a>Sanal ağ için bir değişken oluşturun

Tamamlanan sanal ağ için bir değişken oluşturun. 

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

### <a name="get-the-credentials-for-the-vm"></a>VM için kimlik bilgilerini alma

Aşağıdaki cmdlet'i yeni bir kullanıcı adı ve parola VM uzaktan erişmek için yerel yönetici hesabı olarak kullanılacak gireceğiniz bir pencere açılır. 

```azurepowershell-interactive
$cred = Get-Credential
```

### <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>VM adı, bilgisayar adını ve VM boyutu değişkenlerini ayarlama

VM adı ve bilgisayar adı için değişkenler oluşturun. Bu örnek VM adı olarak ayarlar **myVM** ve bilgisayar adı olarak **Bilgisayarım**.

```azurepowershell-interactive
$vmName = "myVM"
$computerName = "myComputer"
```

Sanal makine boyutunu ayarlayın. Bu örnekte **Standard_DS1_v2** VM boyuta sahip. Bkz: [VM boyutları](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) daha fazla bilgi için.

```azurepowershell-interactive
$vmSize = "Standard_DS1_v2"
```

Sanal makine adını ve boyutunu VM yapılandırmasına ekleyin.

```azurepowershell-interactive
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

### <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>VM görüntüsü yeni VM için kaynak görüntü olarak ayarlayın

Yönetilen VM görüntüsü Kimliğini kullanarak kaynak görüntüsü ayarlayın.

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

### <a name="set-the-os-configuration-and-add-the-nic"></a>İşletim sistemi yapılandırması ayarlayın ve NIC ekleyin

Depolama türü (PremiumLRS veya StandardLRS) ve işletim sistemi disk boyutu girin. Bu örnekte hesap türünü ayarlar **PremiumLRS**, disk boyutu **128 GB** ve disk önbelleğe alma **ReadWrite**.

```azurepowershell-interactive
$vm = Set-AzureRmVMOSDisk -VM $vm  `
    -StorageAccountType PremiumLRS `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

### <a name="create-the-vm"></a>Sanal makine oluşturma

Biz oluşturulan ve depolanan yapılandırmayı kullanarak yeni Vm oluşturma **$vm** değişkeni.

```azurepowershell-interactive
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

### <a name="verify-that-the-vm-was-created"></a>VM oluşturulduğunu doğrulayın
Tamamlandığında, yeni oluşturulan VM görmelisiniz [Azure portal](https://portal.azure.com) altında **Gözat** > **sanal makineleri**, veya aşağıdaki PowerShell komutlarını kullanarak:

```azurepowershell-interactive
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizi Azure PowerShell ile yönetmek için bkz: [oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

