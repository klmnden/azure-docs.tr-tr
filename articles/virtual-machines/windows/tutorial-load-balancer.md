---
title: Öğretici - Azure’da Windows sanal makinelerinde yük dengeleme | Microsoft Docs
description: Bu öğreticide, üç Windows sanal makinesi arasında yüksek kullanılabilirliğe sahip ve güvenli uygulama için bir yük dengeleyici oluşturmak üzere Azure PowerShell kullanmayı öğreneceksiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: zr-msft
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: b8e71a8874ae3ccee74723b8670d177d31f96d25
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49470838"
---
# <a name="tutorial-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application-with-azure-powershell"></a>Öğretici: Azure PowerShell ile yüksek oranda kullanılabilir bir uygulama oluşturmak için Azure’da Windows sanal makinelerinde yük dengeleme
Yük dengeleme, gelen istekleri birden çok sanal makineye dağıtarak yüksek düzeyde kullanılabilirlik sunar. Bu öğreticide, Azure yük dengeleyicisinin trafiği dağıtan ve yüksek kullanılabilirlik sağlayan farklı bileşenleri hakkında bilgi edinebilirsiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure yük dengeleyici oluşturma
> * Yük dengeleyici durum yoklaması oluşturma
> * Yük dengeleyici trafik kuralları oluşturma
> * Temel bir IIS sitesi oluşturmak için Özel Betik Uzantısı kullanma
> * Sanal makineler oluşturma ve yük dengeleyiciye ekleme
> * Çalışan yük dengeleyiciyi görüntüleme
> * VM’leri yük dengeleyiciye ekleme ve kaldırma

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.


## <a name="azure-load-balancer-overview"></a>Azure yük dengeleyiciye genel bakış
Azure yük dengeleyici, gelen trafiği iyi durumdaki VM'ler arasında dağıtmak için yüksek kullanılabilirlik sağlayan bir 4. Katman (TCP, UDP) yük dengeleyicidir. Yük dengeleyici durum araştırması, her VM'deki belirli bir bağlantı noktasını izler ve trafiği yalnızca çalışır durumdaki VM'lere yönlendirir.

Bir veya daha fazla genel IP adresi içeren bir ön uç IP yapılandırması tanımlayın. Bu ön uç IP yapılandırması yük dengeleyicinize ve uygulamalarınıza İnternet üzerinden erişilmesine izin verir. 

Sanal makineler, sanal ağ arabirim kartını (NIC) kullanarak bir yük dengeleyiciye bağlanır. Trafiği VM’lere dağıtmak için, bir arka uç adres havuzunda yük dengeleyiciye bağlı sanal NIC’lerin IP adresleri barındırılır.

Trafiğin akışını denetlemek için VM’lerinizle eşlenen belirli bağlantı noktaları ve protokoller için yük dengeleyici kuralları tanımlayın.


## <a name="create-azure-load-balancer"></a>Azure yük dengeleyici oluşturma
Bu bölümde yük dengeleyicinin her bir bileşenini nasıl oluşturacağınız ve yapılandıracağınız açıklanmaktadır. Yük dengeleyici oluşturabilmek için [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *EastUS* konumunda *myResourceGroupLoadBalancer* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
New-AzureRmResourceGroup `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS"
```

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma
Uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) ile genel IP adresi oluşturun. Aşağıdaki örnek, *myResourceGroupLoadBalancer* kaynak grubunda *myPublicIP* adlı bir genel IP adresi oluşturur:

```azurepowershell-interactive
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS" `
  -AllocationMethod "Static" `
  -Name "myPublicIP"
```

### <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma
[New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) ile ön uç IP havuzu oluşturun. Aşağıdaki örnek, *myFrontEndPool* adlı bir ön uç IP havuzu oluşturur ve *myPublicIP* adresini ekler: 

```azurepowershell-interactive
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
```

[New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) ile bir arka uç adres havuzu oluşturun. Kalan adımlarda VM’ler bu arka uç havuzuna eklenir. Aşağıdaki örnek, *myBackEndPool* adlı bir arka uç adres havuzu oluşturur:

```azurepowershell-interactive
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"
```

Şimdi [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) ile yük dengeleyiciyi oluşturun. Aşağıdaki örnek, önceki adımlarda oluşturulan ön uç ve arka uç IP havuzlarını kullanarak *myLoadBalancer* adlı bir yük dengeleyici oluşturur:

```azurepowershell-interactive
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myLoadBalancer" `
  -Location "EastUS" `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma
Yük dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. VM, 15 saniyelik aralıklarda art arda iki kez başarısız olursa varsayılan olarak yük dengeleyici dağıtımından kaldırılır. Bir protokolü temel alan bir durum araştırması veya uygulamanız için belirli bir sistem durumu denetim sayfası oluşturun. 

Aşağıdaki örnek bir TCP araştırması oluşturur. Ayrıca daha ayrıntılı sistem durumu denetimleri için özel HTTP araştırmaları oluşturabilirsiniz. Özel bir HTTP yoklaması kullanırken *healthcheck.aspx* gibi bir durum denetimi sayfası oluşturmanız gerekir. Yük dengeleyicinin konağı rotasyonda tutması için yoklamanın **HTTP 200 OK** yanıtını döndürmesi gerekir.

TCP durumu araştırması oluşturmak için [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) komutunu kullanın. Aşağıdaki örnek her VM’yi *80* numaralı *TCP* bağlantı noktasında izleyen *myHealthProbe* adında bir durum yoklaması oluşturur:

```azurepowershell-interactive
Add-AzureRmLoadBalancerProbeConfig `
  -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Durum araştırmasını uygulamak için, [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) ile yük dengeleyiciyi güncelleştirin:

```azurepowershell-interactive
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma
Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Yalnızca durumu iyi olan VM’lerin trafik almasını sağlamak için kullanılacak durum araştırmasını da tanımlamanız gerekir.

[Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) ile bir yük dengeleyici kuralı oluşturun. Aşağıdaki örnek, *myLoadBalancerRule* adlı bir yük dengeleyici kuralı oluşturur ve *80* numaralı *TCP* bağlantı noktasında trafiği dengeler:

```azurepowershell-interactive
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name "myHealthProbe"

Add-AzureRmLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

[Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) ile yük dengeleyiciyi güncelleştirin:

```azurepowershell-interactive
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma
VM’leri dağıtmadan ve dengeleyicinizi sınamadan önce yardımcı sanal ağ kaynaklarını oluşturun. Sanal ağlar hakkında daha fazla bilgi edinmek için [Azure Sanal Ağlarını Yönetme](tutorial-virtual-network.md) öğreticisine gözatın.

### <a name="create-network-resources"></a>Ağ kaynakları oluşturma
[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek *mySubnet* alt ağına sahip *myVnet* adında bir sanal ağ oluşturur:

```azurepowershell-interactive
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS" `
  -Name "myVnet" `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Sanal NIC’ler [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) ile oluşturulur. Aşağıdaki örnek üç sanal NIC oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC). İstediğiniz zaman ek sanal NIC’ler ve VM’ler oluşturabilir ve bunları yük dengeleyiciye ekleyebilirsiniz:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName "myResourceGroupLoadBalancer" `
     -Name myVM$i `
     -Location "EastUS" `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```


## <a name="create-virtual-machines"></a>Sanal makineler oluşturma
Uygulamanızın yüksek oranda kullanılabilir olmasını sağlamak için VM’lerinizi bir kullanılabilirlik kümesine yerleştirin.

[New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) ile kullanılabilirlik kümesi oluşturun. Aşağıdaki örnek *myAvailabilitySet* adında bir kullanılabilirlik kümesi oluşturur:

```azurepowershell-interactive
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myAvailabilitySet" `
  -Location "EastUS" `
  -Sku aligned `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2
```

VM’ler için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Artık [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile VM’leri oluşturabilirsiniz. Aşağıdaki örnek, üç VM ve mevcut değilse gerekli olan sanal ağ bileşenlerini oluşturur:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
    New-AzureRmVm `
        -ResourceGroupName "myResourceGroupLoadBalancer" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -OpenPorts 80 `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred `
        -AsJob
}
```

PowerShell komut istemlerinin size döndürülmesi için `-AsJob` parametresi VM’yi arka plan görevi olarak oluşturur. Arka plan işlerinin ayrıntılarını `Job` cmdlet'i ile görüntüleyebilirsiniz. Üç VM’nin tümünü oluşturup yapılandırmak birkaç dakika sürer.


### <a name="install-iis-with-custom-script-extension"></a>Özel Betik Uzantısı ile IIS yükleme
[Windows sanal makinesini özelleştirme](tutorial-automate-vm-deployment.md) konulu önceki bir öğreticide, Windows için Özel Betik Uzantısı ile VM özelleştirmeyi nasıl otomatikleştirebileceğinizi öğrendiniz. Aynı yaklaşımı, VM'lerinizde IIS yüklemek ve yapılandırmak için kullanabilirsiniz.

Özel Betik Uzantısı’nı yüklemek için [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) komutunu kullanın. Uzantı, IIS web sunucusunu yüklemek için `powershell Add-WindowsFeature Web-Server` komutunu çalıştırır ve ardından VM’nin ana bilgisayar adını göstermek için *Default.htm* sayfasını güncelleştirir:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName "myResourceGroupLoadBalancer" `
     -ExtensionName "IIS" `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.8 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Yük dengeleyiciyi test etme
[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ile yük dengeleyicinizin genel IP adresini alın. Aşağıdaki örnek, daha önce oluşturulan *myPublicIP* için IP adresini alır:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myPublicIP" | select IpAddress
```

Sonra da genel IP adresini bir web tarayıcısına girebilirsiniz. Aşağıdaki örnekteki gibi yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adının dahil olduğu web sitesi görüntülenir:

![Çalışan IIS web sitesi](./media/tutorial-load-balancer/running-iis-website.png)

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM’ye dağıtmasını görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz.


## <a name="add-and-remove-vms"></a>VM’leri ekleme ve kaldırma
Uygulamanızı çalıştıran VM’lerde işletim sistemi güncelleştirmelerini yükleme gibi bakım işlemleri gerçekleştirmeniz gerekebilir. Uygulamanıza gelen trafiği fazla trafiği yönetmek için ek VM’lere ihtiyaç duyabilirsiniz. Bu bölümde VM’yi yük dengeleyiciden nasıl kaldırabileceğiniz veya ekleyebileceğiniz gösterilmektedir.

### <a name="remove-a-vm-from-the-load-balancer"></a>VM’yi yük dengeleyiciden kaldırma
[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) ile ağ arabirim kartını alın, ardından sanal NIC’nin *LoadBalancerBackendAddressPools* özelliğini *$null* olarak ayarlayın. Son olarak, sanal NIC’yi güncelleştirin:

```azurepowershell-interactive
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName "myResourceGroupLoadBalancer" `
    -Name "myVM2"
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran kalan iki VM’ye dağıtmasını görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz. Artık VM üzerinde, işletim sistemi güncelleştirmeleri yükleme veya VM’yi yeniden başlatma gibi bakım işlemleri gerçekleştirebilirsiniz.

### <a name="add-a-vm-to-the-load-balancer"></a>Yük dengeleyiciye VM ekleme
VM bakım işlemlerini gerçekleştirdikten sonra, ya da kapasiteyi genişletmeniz gerekiyorsa, sanal NIC’nin *LoadBalancerBackendAddressPools* özelliğini [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) komutundaki *BackendAddressPool* olarak ayarlayın:

Yük dengeleyiciyi alın:

```azurepowershell-interactive
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir yük dengeleyici oluşturdunuz ve ona sanal makineler eklediniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure yük dengeleyici oluşturma
> * Yük dengeleyici durum yoklaması oluşturma
> * Yük dengeleyici trafik kuralları oluşturma
> * Temel bir IIS sitesi oluşturmak için Özel Betik Uzantısı kullanma
> * Sanal makineler oluşturma ve yük dengeleyiciye ekleme
> * Çalışan yük dengeleyiciyi görüntüleme
> * VM’leri yük dengeleyiciye ekleme ve kaldırma

VM ağının nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VM’leri ve sanal ağları yönetme](./tutorial-virtual-network.md)
