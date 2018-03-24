---
title: Azure sanal ağlar ve Windows sanal makineleri | Microsoft Docs
description: Öğretici - Azure sanal ağlar ve Azure PowerShell ile Windows sanal makineleri yönetme
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: feaef679a3090491b64c69ac69bf22153c281d31
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Azure sanal ağlar ve Azure PowerShell ile Windows sanal makineleri yönetme

Azure sanal makineleri, iç ve dış ağ iletişimi için Azure ağını kullanır. Bu öğretici, iki sanal makineyi dağıtma ve bu VM’ler için Azure ağını yapılandırma konusunda rehberlik sunar. Bu öğreticideki örneklerde VM’lerde veritabanı arka ucuna sahip bir web uygulaması barındırıldığı varsayılır, ancak öğreticide uygulama dağıtılmaz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal ağ ve alt ağ oluşturma
> * Genel IP adresi oluşturma
> * Ön uç VM’si oluşturma
> * Ağ trafiğinin güvenliğini sağlama
> * Arka uç VM’si oluşturma



Bu öğretici için AzureRM.Compute modülünün 4.3.1 veya daha sonraki bir sürümü gerekir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM.Compute` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).

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
- *myBackendVM* -bağlantı noktası 1433 ile iletişim kurmak için kullandığı VM *myFrontendVM*.


## <a name="create-a-virtual-network-and-subnet"></a>Sanal ağ ve alt ağ oluşturma

Bu öğreticide iki alt ağa sahip tek bir sanal ağ oluşturulur. Web uygulamasını barındırmak için bir ön uç alt ağı ve veritabanı sunucusunu barındırmak için bir arka uç alt ağı.

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

Bu noktada ağ oluşturulur ve biri ön uç hizmetlerine, diğeri ise arka uç hizmetlerine yönelik olan iki alt ağ segmentine ayrılır. Sonraki bölümde sanal makineler oluşturulacak ve bu alt ağlara bağlanacak.

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Genel IP adresi, Azure kaynaklarına İnternet’ten erişilmesine izin verir. Genel IP adresi ayırma yöntemi dinamik veya statik olarak yapılandırılabilir. Genel IP adresi varsayılan olarak dinamik biçimde ayrılır. Bir VM serbest bırakıldığında dinamik IP adresleri de serbest bırakılır. Bu davranış, VM’nin serbest bırakılmasını içeren tüm işlemlerde IP adresinin değişmesine neden olur.

Ayırma yöntemi statik olarak ayarlanabilir; bu yöntem, VM serbest bırakılsa bile IP adresinin VM’ye atanmış olarak kalmasını sağlar. Statik olarak ayrılan bir IP adresi kullanılırken IP adresi belirtilemez. Bunun yerine IP adresi, kullanılabilen adresler havuzundan ayrılır.

Adlı bir ortak IP adresi oluşturma *myPublicIPAddress* kullanarak [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

-AllocationMethod parametresi değişebilir `Static` bir statik genel IP adresi atamak için.

## <a name="create-a-front-end-vm"></a>Ön uç VM’si oluşturma

Bir VM sanal ağ içinde iletişim kurmak bir sanal ağ arabirimi (NIC) gerekir. Kullanarak bir NIC oluşturun [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontend `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Kullanıcı adı ve parola kullanarak VM üzerinde yönetici hesabı için gerekli ayarlayın [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential). Ek adımlar VM'yi bağlanmak için bu kimlik bilgileri kullanın:

```azurepowershell-interactive
$cred = Get-Credential
```

Kullanarak sanal makineleri oluşturmak [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

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

## <a name="create-a-back-end-vm"></a>Arka uç VM’si oluşturma

Bu öğretici için arka uç VM oluşturmak için en kolay yolu, bir SQL Server görüntüsü kullanmaktır. Bu öğretici yalnızca VM veritabanı sunucusuyla oluşturur, ancak veritabanı erişme hakkında bilgi sağlamaz.

Oluşturma *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackend `
  -SubnetId $vnet.Subnets[1].Id
```

Kullanıcı adı ve parola Get-Credential ile VM üzerinde yönetici hesabı için gerekli ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Oluşturma *myBackendVM*.

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myBackend `
   -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -SubnetName myFrontendSubnet `
   -VirtualNetworkName myVNet
```

Kullanılan görüntü SQL Server'ın yüklü, ancak bu öğreticide kullanılmaz. Web trafiğini işlemek için bir VM ve veritabanı yönetimi işlemek için bir VM nasıl yapılandırabileceğiniz göstermek için dahil edilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide sanal makinelerle ilgili Azure ağlarını oluşturup ve güvenliğini sağladınız. 

> [!div class="checklist"]
> * Sanal ağ ve alt ağ oluşturma
> * Genel IP adresi oluşturma
> * Ön uç VM’si oluşturma
> * Ağ trafiğinin güvenliğini sağlama
> * Arka uç VM’si oluşturma

Azure Yedekleme'yi kullanarak sanal makinelerde güvenli hale getirme verileri izleme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure'da Windows sanal makineleri yedekleyin](./tutorial-backup-vms.md)
