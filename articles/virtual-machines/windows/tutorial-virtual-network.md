---
title: "Azure sanal ağlar ve Windows sanal makineleri | Microsoft Docs"
description: "Öğretici - Azure sanal ağlar ve Azure PowerShell ile Windows sanal makineleri yönetme"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/12/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 21f2d586a4c468071bec55c65005b35baf323fe7
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Azure sanal ağlar ve Azure PowerShell ile Windows sanal makineleri yönetme

Azure sanal makineler, iç ve dış ağ iletişimi için Azure ağ kullanın. Bu öğreticide iki sanal makine dağıtma ve bu VM'ler için Azure ağı yapılandırma açıklanmaktadır. Bir uygulama öğreticide dağıtılmamış ancak bu öğreticide örneklerde, sanal makineleri bir veritabanı arka uç, web uygulamasıyla barındırıyorsanız varsayılmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir sanal ağ ve alt ağ oluşturun
> * Genel IP adresi oluşturma
> * Bir ön uç VM oluşturma
> * Ağ trafiğinin güvenliğini sağlayın
> * Arka uç VM oluşturma

Bu öğreticiyi tamamlamak sırasında oluşturulan bu kaynakları görebilirsiniz:

![İki alt ağ ile sanal ağ](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* -birbirine ve internet ile iletişim kurmak için sanal makineleri kullanan sanal ağ.
- *myFrontendSubnet* -alt ağda *myVNet* ön uç kaynaklar tarafından kullanılır.
- *myPublicIPAddress* -kullanılan genel IP adresine erişimi *myFrontendVM* internet'ten.
- *myFrontentNic* -tarafından kullanılan ağ arabirimini *myFrontendVM* ile iletişim kurmak için *myBackendVM*.
- *myFrontendVM* -VM Internet arasında iletişim kurmak için kullanılır ve *myBackendVM*.
- *myBackendNSG* -arasındaki iletişimi denetleyen ağ güvenlik grubu *myFrontendVM* ve *myBackendVM*.
- *myBackendSubnet* -alt ağ ile ilişkili *myBackendNSG* ve arka uç kaynaklar tarafından kullanılır.
- *myBackendNic* -tarafından kullanılan ağ arabirimini *myBackendVM* ile iletişim kurmak için *myFrontendVM*.
- *myBackendVM* -bağlantı noktası 1433 ile iletişim kurmak için kullandığı VM *myFrontendVM*.

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="vm-networking-overview"></a>VM ağ genel bakış

Azure sanal ağlar arasında sanal makineleri, internet ve diğer Azure hizmetleriyle Azure SQL veritabanı gibi güvenli ağ bağlantıları etkinleştirin. Sanal ağlar alt ağ olarak adlandırılan mantıksal parçalara bölünür. Alt ağ akış denetimi ve güvenlik sınırı olarak kullanılır. Bir VM dağıtırken, genellikle bir alt ağa bağlı bir sanal ağ arabirimi içerir.

## <a name="create-a-virtual-network-and-subnet"></a>Bir sanal ağ ve alt ağ oluşturun

Bu öğreticide, tek bir sanal ağı iki alt ağ ile oluşturulur. Bir web uygulamasını barındırmak için bir ön uç alt ağı ve bir veritabanı sunucusunu barındırmak için bir arka uç alt ağ.

Bir kaynak grubunu kullanarak bir sanal ağ oluşturmadan önce oluşturmanız [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu oluşturur *myRGNetwork* içinde *EastUS* konumu:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

### <a name="create-subnet-configurations"></a>Alt ağ yapılandırmaları oluşturma

Adlı bir alt ağ yapılandırması oluşturma *myFrontendSubnet* kullanarak [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Adlı bir alt ağ yapılandırması oluşturun *myBackendSubnet*:

```azurepowershell-interactive
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
```

### <a name="create-virtual-network"></a>Sanal ağ oluşturma

Adlı VNET oluşturma *myVNet* kullanarak *myFrontendSubnet* ve *myBackendSubnet* kullanarak [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet, $backendSubnet
```

Bu noktada, ağ oluşturulduğundan ve iki alt ağ, ön uç Hizmetleri için bir tane ve arka uç hizmetlerini için başka bir içine bölümlenmiş. Sonraki bölümde, sanal makineleri oluşturulur ve bu alt ağlara bağlı.

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Bir ortak IP adresi Internet üzerinden erişilebilir olması Azure kaynaklarını sağlar. Genel IP adresi ayırma yöntemi dinamik veya statik olarak yapılandırılmış olmalıdır. Varsayılan olarak, bir ortak IP adresi dinamik olarak ayrılır. Bir VM serbest bırakıldığında dinamik IP adresleri serbest bırakılır. Bu davranış VM ayırmayı kaldırma içeren herhangi bir işlem sırasında değiştirmek IP adresi neden olur.

IP adresi deallocated durumundayken bile bir VM için atanan kalmasını sağlar statik ayırma yöntemi ayarlanabilir. Statik olarak ayrılmış bir IP adresi kullanıldığında, IP adresi belirtilemez. Bunun yerine, kullanılabilir adresler havuzundan tahsis edilir.

Adlı bir ortak IP adresi oluşturma *myPublicIPAddress* kullanarak [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

-AllocationMethod parametresi değişebilir `Static` bir statik genel IP adresi atamak için.

## <a name="create-a-front-end-vm"></a>Bir ön uç VM oluşturma

Bir VM sanal ağ içinde iletişim kurmak bir sanal ağ arabirimi (NIC) gerekir. Kullanarak bir NIC oluşturun [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Kullanıcı adı ve parola kullanarak VM üzerinde yönetici hesabı için gerekli ayarlayın [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential). Ek adımlar VM'yi bağlanmak için bu kimlik bilgileri kullanın:

```azurepowershell-interactive
$cred = Get-Credential
```

Kullanarak sanal makineleri oluşturmak [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [kümesi AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [kümesi AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Ekleme AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), ve [AzureRmVM yeni](/powershell/module/azurerm.compute/new-azurermvm):

```azurepowershell-interactive
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="secure-network-traffic"></a>Ağ trafiğinin güvenliğini sağlayın

Ağ güvenlik grubu (NSG), Azure Sanal Ağlara (VNet) bağlı kaynaklara ağ trafiğine izin veren veya reddeden güvenlik kurallarının listesini içerir. Nsg'ler alt ağları veya tek tek ağ arabirimleri için ilişkili olabilir. Bir NSG'yi bir ağ arabirimi ile ilişkili olduğunda, yalnızca ilişkili VM geçerlidir. Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur.

### <a name="network-security-group-rules"></a>Ağ güvenlik grubu kuralları

NSG kuralları üzerinden trafik izin verilen veya reddedilen ağ bağlantı noktalarını tanımlar. Böylece belirli sistemleri veya alt ağlar arasında trafiği denetlenir kuralları kaynak ve hedef IP adresi aralıklarını içerebilir. NSG kuralları da dahil bir öncelik (1 arasında — ve 4096). Kurallar öncelik sırasına göre değerlendirilir. 100 önceliğine sahip bir kural 200 önceliğine sahip bir kural önce değerlendirilir.

Tüm NSG'ler bir varsayılan kurallar kümesini içerir. Varsayılan kurallar silinemez ancak en düşük önceliğe atanmış oldukları için sizin oluşturduğunuz kurallar tarafından geçersiz kılınabilirler.

- **Sanal ağ** - kaynaklanan trafiği ve sanal ağ içinde bitiş hem gelen ve giden yönlerde izin verilir.
- **Internet** - giden trafiğe izin verilir, ancak gelen trafik engellenir.
- **Yük Dengeleyici** -VM'ler ve rol örneklerinin durumunu araştırma için izin Azure'nın yük dengeleyici. Yük dengelenmiş bir küme kullanmıyorsanız bu kuralı geçersiz kılabilirsiniz.

### <a name="create-network-security-groups"></a>Ağ güvenlik grupları oluşturma

Adlı bir gelen kuralı oluşturma *myFrontendNSGRule* gelen web trafiği sağlamak için *myFrontendVM* kullanarak [yeni AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

```azurepowershell-interactive
$nsgFrontendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myFrontendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

İç trafiği sınırlayabilirsiniz *myBackendVM* yalnızca gelen *myFrontendVM* arka uç alt ağı için bir NSG oluşturarak. Aşağıdaki örnek, adlandırılmış bir NSG kuralı oluşturur *myBackendNSGRule*:

```azurepowershell-interactive
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Adlı ağ güvenlik grubu Ekle *myFrontendNSG* kullanarak [yeni AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsgFrontend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule
```

Şimdi, adlı ağ güvenlik grubu Ekle *myBackendNSG* yeni AzureRmNetworkSecurityGroup kullanarak:

```azurepowershell-interactive
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```

Ağ güvenlik grupları için alt ağlar ekleyin:

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
$frontendSubnet = $vnet.Subnets[0]
$backendSubnet = $vnet.Subnets[1]
$frontendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myFrontendSubnet `
  -AddressPrefix $frontendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgFrontend
$backendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myBackendSubnet `
  -AddressPrefix $backendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

## <a name="create-a-back-end-vm"></a>Bir arka uç VM oluşturma

Bu öğretici için arka uç VM oluşturmak için en kolay yolu, bir SQL Server görüntüsü kullanmaktır. Bu öğretici yalnızca VM veritabanı sunucusuyla oluşturur, ancak veritabanı erişme hakkında bilgi sağlamaz.

Oluşturma *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Kullanıcı adı ve parola Get-Credential ile VM üzerinde yönetici hesabı için gerekli ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Oluşturma *myBackendVM*:

```azurepowershell-interactive
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

Kullanılan görüntü SQL Server'ın yüklü, ancak bu öğreticide kullanılmaz. Web trafiğini işlemek için bir VM ve veritabanı yönetimi işlemek için bir VM nasıl yapılandırabileceğiniz göstermek için dahil edilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide oluşturduğunuz ve Azure ağları sanal makinelerle ilgili olarak güvenli. 

> [!div class="checklist"]
> * Bir sanal ağ ve alt ağ oluşturun
> * Genel IP adresi oluşturma
> * Bir ön uç VM oluşturma
> * Ağ trafiğinin güvenliğini sağlayın
> * Bir arka uç VM oluşturma

Azure Yedekleme'yi kullanarak sanal makinelerde güvenli hale getirme verileri izleme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure'da Windows sanal makineleri yedekleyin](./tutorial-backup-vms.md)
