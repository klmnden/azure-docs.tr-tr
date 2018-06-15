---
title: Öğretici - Windows VM’ler için Azure sanal ağları oluşturma ve yönetme | Microsoft Docs
description: Bu öğreticide, Azure PowerShell kullanarak Windows sanal makineleri için Azure sanal ağları oluşturup yönetmeyi öğrenirsiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a13163949a52503f42642c109a4fd4c1dedd837f
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33944095"
---
# <a name="tutorial-create-and-manage-azure-virtual-networks-for-windows-virtual-machines-with-azure-powershell"></a>Öğretici - Windows VM’ler için Azure sanal ağları oluşturma ve yönetme | Microsoft Docs

Azure sanal makineleri, iç ve dış ağ iletişimi için Azure ağını kullanır. Bu öğretici, iki sanal makineyi dağıtma ve bu VM’ler için Azure ağını yapılandırma konusunda rehberlik sunar. Bu öğreticideki örneklerde VM’lerde veritabanı arka ucuna sahip bir web uygulaması barındırıldığı varsayılır, ancak öğreticide uygulama dağıtılmaz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal ağ ve alt ağ oluşturma
> * Genel IP adresi oluşturma
> * Ön uç VM’si oluşturma
> * Ağ trafiğinin güvenliğini sağlama
> * Arka uç VM’si oluşturma

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="vm-networking-overview"></a>VM ağına genel bakış

Azure sanal ağları, sanal makineler ile İnternet ve Azure SQL veritabanı gibi diğer Azure hizmetleri arasında güvenli ağ bağlantıları kurulmasını sağlar. Sanal ağlar, alt ağ adı verilen mantıksal segmentlere ayrılır. Alt ağlar, ağ akışını denetlemek için ve güvenlik sınırı olarak kullanılır. Bir VM dağıtılırken, genellikle bir alt ağa eklenmiş sanal ağ arabirimine sahiptir.

Bu öğreticiyi tamamladığınızda şu kaynakların oluşturulduğunu görebilirsiniz:

![İki alt ağ içeren sanal ağ](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* - VM’lerin birbirleriyle ve İnternet’le iletişim kurmak için kullandığı sanal ağ.
- *myFrontendSubnet* - Ön uç kaynakları tarafından kullanılan *myVNet*’teki alt ağ.
- *myPublicIPAddress* - İnternet’ten *myFrontendVM*’ye erişmek için kullanılan genel IP adresi.
- *myFrontentNic* - *myBackendVM* ile iletişim kurmak için *myFrontendVM* tarafından kullanılan ağ arabirimi.
- *myFrontendVM* - İnternet ile *myBackendVM* arasında iletişim kurmak için kullanılan VM.
- *myBackendNSG* - *myFrontendVM* ile *myBackendVM* arasındaki iletişimi denetleyen ağ güvenlik grubu.
- *myBackendSubnet* - *myBackendNSG* ile ilişkilendirilmiş ve arka uç kaynakları tarafından kullanılan alt ağ.
- *myBackendNic* - *myFrontendVM* ile iletişim kurmak için *myBackendVM* tarafından kullanılan ağ arabirimi.
- *myBackendVM* - 1433 numaralı bağlantı noktasını kullanarak *myFrontendVM* ile iletişim kuran VM.


## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma

Bu öğreticide iki alt ağa sahip tek bir sanal ağ oluşturulur. Web uygulamasını barındırmak için bir ön uç alt ağı ve veritabanı sunucusunu barındırmak için bir arka uç alt ağı.

Sanal ağ oluşturabilmek için önce [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnekte *EastUS* konumunda *myRGNetwork* adlı bir kaynak grubu oluşturulur:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

### <a name="create-subnet-configurations"></a>Alt ağ yapılandırmaları oluşturma

[New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) komutunu kullanarak *myFrontendSubnet* adlı bir alt ağ yapılandırması oluşturun:

```azurepowershell-interactive
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Sonra, *myBackendSubnet* adlı bir alt ağ yapılandırması oluşturun:

```azurepowershell-interactive
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
```

### <a name="create-virtual-network"></a>Sanal ağ oluşturma

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) komutunu kullanarak *myFrontendSubnet* ve *myBackendSubnet* ile *myVNet* adlı bir sanal ağ oluşturun:

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet, $backendSubnet
```

Bu noktada ağ oluşturulur ve biri ön uç hizmetlerine, diğeri ise arka uç hizmetlerine yönelik olan iki alt ağ segmentine ayrılır. Sonraki bölümde sanal makineler oluşturulacak ve bu alt ağlara bağlanacak.

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Genel IP adresi, Azure kaynaklarına İnternet’ten erişilmesine izin verir. Genel IP adresi ayırma yöntemi dinamik veya statik olarak yapılandırılabilir. Genel IP adresi varsayılan olarak dinamik biçimde ayrılır. Bir VM serbest bırakıldığında dinamik IP adresleri de serbest bırakılır. Bu davranış, VM’nin serbest bırakılmasını içeren tüm işlemlerde IP adresinin değişmesine neden olur.

Ayırma yöntemi statik olarak ayarlanabilir; bu yöntem, VM serbest bırakılsa bile IP adresinin VM’ye atanmış olarak kalmasını sağlar. Statik olarak ayrılan bir IP adresi kullanılırken IP adresi belirtilemez. Bunun yerine IP adresi, kullanılabilen adresler havuzundan ayrılır.

[New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) komutunu kullanarak *myPublicIPAddress* adlı genel bir IP adresi oluşturun:

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

Statik bir genel IP adresi atamak için AllocationMethod parametresini `Static` olarak değiştirebilirsiniz.

## <a name="create-a-front-end-vm"></a>Ön uç VM’si oluşturma

Bir VM’nin sanal ağ içerisinde iletişim kurabilmesi için bir sanal ağ arabirimine (NIC) sahip olması gerekir. [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) komutunu kullanarak bir NIC oluşturun:

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontend `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

[Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) komutunu kullanarak VM’de yönetici hesabı için gereken kullanıcı adı ve parolasını ayarlayın. Ek adımlarda VM’ye bağlanmak için bu kimlik bilgilerini kullanacaksınız:

```azurepowershell-interactive
$cred = Get-Credential
```

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) komutunu kullanarak VM’leri oluşturun.

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myFrontend `
   -PublicIpAddressName myPublicIPAddress `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -Size Standard_D1 `
   -SubnetName myFrontendSubnet `
   -VirtualNetworkName myVNet
```

## <a name="secure-network-traffic"></a>Ağ trafiğinin güvenliğini sağlama

Ağ güvenlik grubu (NSG), Azure Sanal Ağlara (VNet) bağlı kaynaklara ağ trafiğine izin veren veya reddeden güvenlik kurallarının listesini içerir. NSG’ler alt ağlarla veya tek tek ağ arabirimleriyle ilişkilendirilebilir. Bir NSG ağ arabirimiyle ilişkilendirildiğinde, yalnızca ilişkili VM için geçerli olur. Bir NSG bir alt ağ ile ilişkilendirildiğinde kurallar alt ağa bağlı tüm kaynaklar için geçerli olur.

### <a name="network-security-group-rules"></a>Ağ güvenlik grubu kuralları

NSG kuralları trafiğe izin verilen veya trafiğin engellendiği ağ bağlantı noktalarını tanımlar. Trafiğin belirli sistemler veya alt ağlar arasında denetlenmesi için kurallar, kaynak ve hedef IP adresi aralıkları içerebilir. Ayrıca NSG kuralları öncelik (1 ile 4.096 arasında) içerir. Kurallar öncelik sırasına göre değerlendirilir. 100 önceliğine sahip bir kural, 200 önceliğine sahip kuraldan önce değerlendirilir.

Tüm NSG'ler bir varsayılan kurallar kümesini içerir. Varsayılan kurallar silinemez ancak en düşük önceliğe atanmış oldukları için sizin oluşturduğunuz kurallar tarafından geçersiz kılınabilirler.

- **Sanal ağ** - Kaynağı bir sanal ağ olan ve bir sanal ağda biten trafiğe hem gelen hem de giden yönlerde izin verilir.
- **İnternet** - Giden trafiğe izin verilir, ancak gelen trafik engellenir.
- **Yük dengeleyici** - VM’lerinizin ve rol örneklerinizin sistem durumunu araştıran Azure yük dengeleyicisine izin verir. Yük dengeli bir küme kullanmıyorsanız bu kuralı geçersiz kılabilirsiniz.

### <a name="create-network-security-groups"></a>Ağ güvenlik grupları oluşturma

[New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) komutunu kullanarak *myFrontendVM* üzerinde gelen web trafiğine izin vermek için *myFrontendNSGRule* adlı bir gelen kuralı oluşturun:

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

Arka uç alt ağı için bir NSG oluşturarak *myBackendVM*’ye gelen iç trafiği yalnızca *myFrontendVM*’den gelecek şekilde sınırlayabilirsiniz. Aşağıdaki örnekte *myBackendNSGRule* adlı bir kural oluşturulur:

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

[New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) komutunu kullanarak *myFrontendNSG* adlı bir ağ güvenlik grubu oluşturun:

```azurepowershell-interactive
$nsgFrontend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule
```

Şimdi de New-AzureRmNetworkSecurityGroup komutunu kullanarak *myBackendNSG* adlı bir ağ güvenlik grubu ekleyin:

```azurepowershell-interactive
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```

Ağ güvenlik gruplarını alt ağlara ekleyin:

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

## <a name="create-a-back-end-vm"></a>Arka uç VM’si oluşturma

Bu öğretici için gerekli arka uç VM’yi oluşturmanın en kolay yolu bir SQL Server görüntüsü kullanmaktır. Bu öğreticide yalnızca veritabanı sunucusu ile VM oluşturulurken, veritabanına erişim hakkında bilgi sağlanmaz.

*myBackendNic* öğesini oluşturun:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackend `
  -SubnetId $vnet.Subnets[1].Id
```

Get-Credential komutuyla VM’de yönetici hesabı için gereken kullanıcı adı ve parolasını ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

*myBackendVM* öğesini oluşturun.

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myBackend `
   -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -SubnetName MyBackendSubnet `
   -VirtualNetworkName myVNet
```

Kullanılan görüntüde SQL Server yüklüdür, ancak bu öğreticide kullanılmamıştır. Bir VM’yi nasıl web trafiğini işleyecek ve bir VM’yi veritabanı yönetim işlemlerini gerçekleştirecek şekilde yapılandırabileceğinizi göstermek için dahil edilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide sanal makinelerle ilgili Azure ağlarını oluşturup ve güvenliğini sağladınız. 

> [!div class="checklist"]
> * Sanal ağ ve alt ağ oluşturma
> * Genel IP adresi oluşturma
> * Ön uç VM’si oluşturma
> * Ağ trafiğinin güvenliğini sağlama
> * Arka uç VM’si oluşturma

Azure Backup kullanarak sanal makinelerdeki verilerin güvenliğini izlemeyi öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Windows sanal makinelerini Azure’da yedekleyin](./tutorial-backup-vms.md)
