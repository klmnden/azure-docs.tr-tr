---
title: "Nasıl yükleneceğini azure'da Windows sanal makineler Bakiye | Microsoft Docs"
description: "Üç Windows sanal makineyi yüksek oranda kullanılabilir ve güvenli bir uygulama oluşturmak için Azure yük dengeleyici kullanmayı öğrenin"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6738d88d5a0430abaf3855dbf97a618e4c83617f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Nasıl yükleneceğini yüksek oranda kullanılabilir bir uygulama oluşturmak için azure'da Windows sanal makineler Bakiye
Yük Dengeleme, birden çok sanal makine genelinde gelen istekleri yayarak daha yüksek düzeyde kullanılabilirlik sağlar. Bu öğreticide, trafiği dağıtmak ve yüksek kullanılabilirlik sağlayan farklı bileşenleri, Azure yük dengeleyici hakkında bilgi edinin. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir Azure yük dengeleyici oluşturma
> * Bir yük dengeleyici durum araştırması oluştur
> * Yük Dengeleyici trafiği kuralları oluşturma
> * Temel bir IIS sitesi oluşturmak için özel betik uzantısı kullanın
> * Sanal makineler oluşturun ve bir yük dengeleyiciye ekleyin
> * Bir yük dengeleyici eylemde görüntüleyin
> * Ekleme ve sanal makineleri yük dengeleyiciden kaldırın

Bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz: [yükleme Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps).


## <a name="azure-load-balancer-overview"></a>Azure yük dengeleyici genel bakış
Bir Azure yük dengeleyici gelen trafiği sağlıklı VM'ler arasında dağıtarak yüksek kullanılabilirlik sağlayan bir katman 4 (TCP, UDP) yük dengeleyicidir. Bir yük dengeleyici durum araştırması her VM üzerinde belirli bir bağlantı noktasını izler ve yalnızca trafiği işletimsel bir VM dağıtır.

Bir veya daha fazla ortak IP adreslerini içeren bir ön uç IP yapılandırmasını tanımlayın. Bu ön uç IP yapılandırmasını yük dengeleyici ve uygulamaların Internet üzerinden erişilebilir olmasını sağlar. 

Sanal makineler kendi sanal ağ arabirim kartı (NIC) kullanarak bir yük dengeleyiciye bağlayın. VM'ler trafiğini dağıtmak için bir arka uç adres havuzu IP içeren sanal (NIC'ler) adreslerini yük dengeleyiciye bağlı.

Trafik akışını denetlemek için belirli bağlantı noktalarını ve Vm'leriniz için eşleme protokolleri için yük dengeleyici kuralları tanımlayın.


## <a name="create-azure-load-balancer"></a>Azure yük dengeleyici oluşturma
Bu bölümde, nasıl oluşturma ve her bileşenin yük dengeleyicinin yapılandırma ayrıntıları. Yük Dengeleyici oluşturmadan önce bir kaynak grubuyla oluşturmanız [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroupLoadBalancer* içinde *EastUS* konumu:

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma
Uygulamanızı Internet'te erişmek için yük dengeleyici için bir ortak IP adresi gerekir. Bir ortak IP adresiyle oluşturma [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Aşağıdaki örnek adlı ortak IP adresi oluşturur *myPublicIP* içinde *myResourceGroupLoadBalancer* kaynak grubu:

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma
Bir ön uç IP adresiyle oluşturma [yeni AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Aşağıdaki örnek, adlandırılmış bir ön uç IP adresi oluşturur *myFrontEndPool*: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

Olan bir arka uç adres havuzu oluşturma [yeni AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). Aşağıdaki örnek, adlandırılmış bir arka uç adres havuzu oluşturur *myBackEndPool*:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Şimdi, olan yük dengeleyici oluşturma [yeni AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). Aşağıdaki örnek, adlandırılmış bir yük dengeleyici oluşturur *myLoadBalancer* kullanarak *myPublicIP* adresi:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Bir sistem durumu araştırması oluştur
Uygulamanızın durumunu izlemek yük dengeleyici izin vermek için bir sistem durumu araştırması kullanın. Sistem durumu araştırma dinamik olarak ekler veya bunların yanıtını durumu denetimleri için temel yük dengeleyici döndürme VM'ler kaldırır. Varsayılan olarak, bir VM yük dengeleyici dağıtımı 15 saniyelik aralıklarda iki ardışık hatadan sonra kaldırılır. Bir protokol veya belirli bir sistem onay sayfasında uygulamanız için temel bir sistem durumu araştırması oluşturun. 

Aşağıdaki örnekte bir TCP araştırması oluşturur. Daha fazla hassas sistem durumu denetimlerinin özel HTTP araştırmalara de oluşturabilirsiniz. Özel bir HTTP araştırma kullanırken, sistem durumu denetimi sayfası gibi oluşturmalısınız *healthcheck.aspx*. Araştırma döndürmelidir bir **HTTP 200 Tamam** dönüş konak tutmak yük dengeleyici için yanıt.

TCP durumu araştırması oluşturmak için kullandığınız [Ekle AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Aşağıdaki örnek adlı bir sistem durumu araştırması oluşturur *myHealthProbe* her VM izler:

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Yük Dengeleyici ile güncelleştirme [kümesi AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma
Yük Dengeleyici kuralı trafiğin Vm'lere nasıl dağıtıldığını tanımlamak için kullanılır. Gelen trafiği ve gerekli kaynak ve hedef bağlantı noktası ile birlikte trafiği almak için arka uç IP havuzu için ön uç IP yapılandırmasını tanımlayın. Yalnızca sağlıklı VM'ler trafiği aldığınızdan emin olmak için aynı zamanda kullanmak için sistem durumu araştırma tanımlar.

Yük Dengeleyici kuralı ile oluşturma [Ekle AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). Aşağıdaki örnek adlı yük dengeleyici kuralı oluşturur *myLoadBalancerRule* ve bağlantı noktası üzerinde trafiği dengeler *80*:

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Yük Dengeleyici ile güncelleştirme [kümesi AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma
Bazı sanal makineleri dağıtmak ve, dengeleyici sınayabilirsiniz önce destekleyici sanal ağ kaynakları oluşturun. Sanal ağlar hakkında daha fazla bilgi için bkz: [Azure Sanal Ağları Yönet](tutorial-virtual-network.md) Öğreticisi.

### <a name="create-network-resources"></a>Ağ kaynakları oluşturun
Bir sanal ağ ile oluşturma [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVnet* ile *mySubnet*:

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Ağ güvenlik grubu kural oluştururken [yeni AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), bir ağ güvenlik grubu oluşturma [yeni AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Alt ağ ile ağ güvenlik grubu eklemek [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) ve sanal ağ ile güncelleştirme [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). 

Aşağıdaki örnek adlı bir ağ güvenlik grubu kural oluşturur *myNetworkSecurityGroup* ve uygular *mySubnet*:

```powershell
# Create security rule config
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create the network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Sanal NIC ile oluşturulan [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Aşağıdaki örnek, üç sanal NIC oluşturur. (Her VM için bir sanal NIC için aşağıdaki adımları uygulamayı oluşturduğunuz). Ek sanal NIC ve sanal makineleri herhangi bir zamanda oluşturabilir ve bunları yük dengeleyiciye ekleyin:

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma
Uygulamanızı yüksek kullanılabilirliğini artırmak için bir kullanılabilirlik kümesine Vm'leriniz yerleştirin.

Kullanılabilirlik kümesi oluştur [yeni AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Aşağıdaki örnek, kullanılabilirlik adlandırılmış kümesi oluşturur *myAvailabilitySet*:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

Yönetici olan VM'ler için kullanıcı adı ve parola ayarlayın [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

VM'lerin oluşturabileceğiniz artık [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnekte, üç VM'ler oluşturur:

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

Oluşturun ve tüm üç sanal makineleri yapılandırmak için birkaç dakika sürer.

### <a name="install-iis-with-custom-script-extension"></a>Özel betik uzantısı ile IIS yükleme
Önceki bir öğretici içinde [Windows sanal makine özelleştirmek nasıl](tutorial-automate-vm-deployment.md), Windows için özel betik uzantısı ile VM özelleştirme otomatikleştirmek öğrendiniz. Aynı yaklaşımı yükleyip Vm'leriniz IIS yapılandırmak için kullanabilirsiniz.

Kullanım [kümesi AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) özel betik uzantısı yüklemek için. Uzantı çalışır `powershell Add-WindowsFeature Web-Server` IIS Web sunucusu ve ardından güncelleştirmeleri yüklemek için *Default.htm* sayfası ana bilgisayar VM adını göster:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Test yük dengeleyici
Yük Dengeleyici ile genel IP adresi elde [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Aşağıdaki örnek IP adresi alacağı *myPublicIP* daha önce oluşturduğunuz:

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

Bir web tarayıcısı ortak IP adresi girebilirsiniz. Yük Dengeleyici trafiği için aşağıdaki örnekteki gibi dağıtılmış VM konak adı dahil olmak üzere Web sitesi görüntülenir:

![Çalışan IIS Web sitesi](./media/tutorial-load-balancer/running-iis-website.png)

Uygulamanızı çalıştıran tüm üç VM'ler arasında trafiği dağıtmak yük dengeleyici görmek için zorla-web tarayıcınızı yenileyin.


## <a name="add-and-remove-vms"></a>Sanal makineleri ekleyip
İşletim sistemi güncelleştirmelerini yükleme gibi uygulamanızı çalıştıran VM'ler bakım yapmak gerekebilir. Uygulamanıza artan trafiği ile mücadele etmek için ek VM'ler eklemeniz gerekebilir. Bu bölümde kaldırın veya VM yük dengeleyiciden nasıl ekleneceğini gösterir.

### <a name="remove-a-vm-from-the-load-balancer"></a>VM yük dengeleyiciden kaldırın
Ağ arabirimi kartı ile [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), ardından *LoadBalancerBackendAddressPools* sanal NIC'ye özelliğinin *$null*. Son olarak, bir sanal NIC güncelleştirin:

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Uygulamanızı çalıştıran diğer iki VM arasında trafiği dağıtmak yük dengeleyici görmek için zorla-web tarayıcınızı yenileyin. İşletim sistemi güncelleştirmeleri yükleme veya VM yeniden başlatma gerçekleştirme gibi VM üzerinde bakım artık gerçekleştirebilirsiniz.

### <a name="add-a-vm-to-the-load-balancer"></a>VM yük dengeleyiciye ekleyin
Sonra VM bakım yapmak veya kapasitesini genişletmek gerekiyorsa, ayarlayın *LoadBalancerBackendAddressPools* sanal NIC'ye özelliğinin *BackendAddressPool* gelen [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

Yük Dengeleyici alın:

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir yük dengeleyici oluşturulur ve sanal makineleri bağlı. Size nasıl öğrenilen için:

> [!div class="checklist"]
> * Bir Azure yük dengeleyici oluşturma
> * Bir yük dengeleyici durum araştırması oluştur
> * Yük Dengeleyici trafiği kuralları oluşturma
> * Temel bir IIS sitesi oluşturmak için özel betik uzantısı kullanın
> * Sanal makineler oluşturun ve bir yük dengeleyiciye ekleyin
> * Bir yük dengeleyici eylemde görüntüleyin
> * Ekleme ve sanal makineleri yük dengeleyiciden kaldırın

VM ağı yönetme konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [VM’leri ve sanal ağları yönetme](./tutorial-virtual-network.md)
