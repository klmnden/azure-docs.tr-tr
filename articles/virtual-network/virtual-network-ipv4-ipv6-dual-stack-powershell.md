---
title: Azure sanal ağında - PowerShell IPv6 ikili yığını uygulama dağıtma
titlesuffix: Azure Virtual Network
description: Bu makalede nasıl dağıtılacağı Azure Powershell kullanarak Azure sanal ağı bir IPv6 ikili yığını uygulamada gösterir.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: f26391e36e3208996160fffad01e39ec2f182318
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130967"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure---powershell-preview"></a>Azure - PowerShell (Önizleme) IPv6 ikili yığını uygulama dağıtma

Bu makalede, azure'da çift yığın sanal ağ ve alt ağ, bir yük dengeleyici ön uç çift (IPv4 + IPv6) yapılandırmalar, ikili bir IP yapılandırmasına sahip NIC ile VM içeren bir ikili yığın (IPv4 + IPv6) uygulamasının nasıl dağıtılacağı gösterir ağ güvenlik grubu ve genel IP'ler.

> [!Important]
> Azure sanal ağ için IPv6 desteği şu anda genel Önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure PowerShell modülü sürüm 6.9.0 gerekir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="prerequisites"></a>Önkoşullar
Azure'da bir çift yığın uygulaması dağıtmadan önce aboneliğiniz için aşağıdaki Azure PowerShell kullanarak bu önizleme özelliğini yapılandırmanız gerekir:

Şu şekilde kaydedin:
```azurepowershell
Register-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
```
Özellik kaydı tamamlanması 30 dakika kadar sürer. Aşağıdaki Azure PowerShell komutunu çalıştırarak, kayıt durumunu kontrol edebilirsiniz: Kayıt üzerinde aşağıdaki gibi denetleyin:
```azurepowershell
Get-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
```
Kayıt tamamlandıktan sonra aşağıdaki komutu çalıştırın:

```azurepowershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

İkili yığın sanal ağ oluşturabilmeniz için önce bir kaynak grubu oluşturmanız gerekir [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myRGDualStack* içinde *Doğu ABD* konumu:

```azurepowershell-interactive
   $rg = New-AzResourceGroup `
  -ResourceGroupName "dsRG1"  `
  -Location "east us"
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses"></a>IPv4 ve IPv6 genel IP adresi oluşturma
Sanal makinelerinize Internet'ten erişmek için yük dengeleyicinin genel IP adresleri IPv4 ve IPv6 gereklidir. Genel IP adresi ile oluşturma [yeni AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Aşağıdaki örnekte adlı IPv4 ve IPv6 genel IP adresi oluşturur *dsPublicIP_v4* ve *dsPublicIP_v6* içinde *dsRG1* kaynak grubu:

```azurepowershell-interactive
$PublicIP_v4 = New-AzPublicIpAddress `
  -Name "dsPublicIP_v4" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv4
  
$PublicIP_v6 = New-AzPublicIpAddress `
  -Name "dsPublicIP_v6" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv6
```
RDP bağlantısı kullanarak sanal makinelerinizde erişmek için bir IPv4 genel IP adresleri ile sanal makineler için oluşturma [yeni AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress).

```azurepowershell-interactive
  $RdpPublicIP_1 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_1" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv4
  
  $RdpPublicIP_2 = New-AzPublicIpAddress `
   -Name "RdpPublicIP_2" `
   -ResourceGroupName $rg.ResourceGroupName `
   -Location $rg.Location  `
   -AllocationMethod Dynamic `
   -IpAddressVersion IPv4
```

## <a name="create-basic-load-balancer"></a>Temel Yük Dengeleyici Oluşturma

Bu bölümde, çift ön uç IP (IPv4 ve IPv6) ve yük dengeleyici için arka uç adres havuzu yapılandırmanız ve ardından bir temel yük dengeleyici oluşturun.

### <a name="create-front-end-ip"></a>Ön uç IP oluşturma

Bir ön uç IP ile oluşturma [yeni AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig). Aşağıdaki örnek, IPv4 ve IPv6 ön uç IP yapılandırmaları adlı oluşturur *dsLbFrontEnd_v4* ve *dsLbFrontEnd_v6*:

```azurepowershell-interactive
$frontendIPv4 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v4" `
  -PublicIpAddress $PublicIP_v4

$frontendIPv6 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v6" `
  -PublicIpAddress $PublicIP_v6

```

### <a name="configure-back-end-address-pool"></a>Arka uç adres havuzu Yapılandır

İle bir arka uç adres havuzu oluşturun [yeni AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig). Bu arka uç havuzu kalan adımlarda VM'ler iliştirin. Aşağıdaki örnekte adlı arka uç adres havuzları oluşturur *dsLbBackEndPool_v4* ve *dsLbBackEndPool_v6* hem IPv4 hem de IPv6 NIC yapılandırmaları ile sanal makineleri eklemek için:

```azurepowershell-interactive
$backendPoolv4 = New-AzLoadBalancerBackendAddressPoolConfig `
-Name "dsLbBackEndPool_v4"

$backendPoolv6 = New-AzLoadBalancerBackendAddressPoolConfig `
-Name "dsLbBackEndPool_v6"
```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Yalnızca sağlıklı olduğundan emin olmak için trafiği sanal makineleri almak, isteğe bağlı olarak bir sistem durumu araştırması tanımlayabilirsiniz. Temel yük dengeleyici hem IPv4 hem de IPv6 uç noktaları vm'lerde sistem durumunu değerlendirmek için bir IPv4 araştırması kullanır. Standart load balancer sistem durumu araştırmaları açıkça IPv6 için destek içerir.

İle bir yük dengeleyici kuralı oluşturma [Ekle AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig). Aşağıdaki örnekte adlı yük dengeleyici kuralları oluşturur *dsLBrule_v4* ve *dsLBrule_v6* ve noktasında trafiği dengeler *TCP* bağlantı noktası *80* için IPv4 ve IPv6 ön uç IP yapılandırması:

```azurepowershell-interactive
$lbrule_v4 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v4" `
  -FrontendIpConfiguration $frontendIPv4 `
  -BackendAddressPool $backendPoolv4 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

$lbrule_v6 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v6" `
  -FrontendIpConfiguration $frontendIPv6 `
  -BackendAddressPool $backendPoolv6 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

Basic Load Balancer ile oluşturma [AzLoadBalancer yeni](/powershell/module/az.network/new-azloadbalancer). Aşağıdaki örnek, genel bir temel yük adlı dengeleyici oluşturur *myLoadBalancer* IPv4 ve IPv6 ön uç IP yapılandırmaları arka uç havuzları, sistem durumu araştırmaları, Yük Dengeleme kuralları ve NAT kuralları'te oluşturduğunuz önceki adımlar:

```azurepowershell-interactive
$lb = New-AzLoadBalancer `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location  `
-Name "MyLoadBalancer" `
-Sku "Basic" `
-FrontendIpConfiguration $frontendIPv4,$frontendIPv6 `
-BackendAddressPool $backendPoolv4,$backendPoolv6 `
-LoadBalancingRule $lbrule_v4,$lbrule_v6

```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Vm'leri dağıtmadan ve dengeleyicinizi test edebilirsiniz önce destekleyici ağ kaynakları - oluşturmalısınız kullanılabilirlik kümesi, ağ güvenlik grubu, sanal ağ ve sanal NIC. 
### <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma
Uygulamanızın yüksek oranda kullanılabilir olmasını sağlamak için VM’lerinizi bir kullanılabilirlik kümesine yerleştirin.

Bir kullanılabilirlik kümesi oluşturma [yeni AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset). Aşağıdaki örnek *myAvailabilitySet* adında bir kullanılabilirlik kümesi oluşturur:

```azurepowershell-interactive
$avset = New-AzAvailabilitySet `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsAVset" `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2 `
  -Sku aligned
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma

Sanal ağınızdaki gelen ve giden iletişimi yöneten kurallar için ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>3389 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

Bağlantı noktası 3389 ile RDP bağlantılarına izin verecek şekilde bir ağ güvenlik grubu kuralı oluşturma [yeni AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell-interactive
$rule1 = New-AzNetworkSecurityRuleConfig `
-Name 'myNetworkSecurityGroupRuleRDP' `
-Description 'Allow RDP' `
-Access Allow `
-Protocol Tcp `
-Direction Inbound `
-Priority 100 `
-SourceAddressPrefix * `
-SourcePortRange * `
-DestinationAddressPrefix * `
-DestinationPortRange 3389
```
#### <a name="create-a-network-security-group-rule-for-port-80"></a>80 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

Bağlantı noktası 80 üzerinden internet bağlantılarına izin verecek şekilde bir ağ güvenlik grubu kuralı oluşturma [yeni AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell-interactive
$rule2 = New-AzNetworkSecurityRuleConfig `
  -Name 'myNetworkSecurityGroupRuleHTTP' `
  -Description 'Allow HTTP' `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange 80 `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80
```
#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu oluşturun [yeni AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

```azurepowershell-interactive
$nsg = New-AzNetworkSecurityGroup `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location  `
-Name "dsNSG1"  `
-SecurityRules $rule1,$rule2
```
### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Aşağıdaki örnek *mySubnet* alt ağına sahip *myVnet* adında bir sanal ağ oluşturur:

```azurepowershell-interactive
# Create dual stack subnet
$subnet = New-AzVirtualNetworkSubnetConfig `
-Name "dsSubnet" `
-AddressPrefix "10.0.0.0/24","ace:cab:deca:deed::/64"

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsVnet" `
  -AddressPrefix "10.0.0.0/16","ace:cab:deca::/48"  `
  -Subnet $subnet
```

### <a name="create-nics"></a>NIC’leri oluşturma

Sanal NIC ile oluşturma [yeni AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface). Aşağıdaki örnek iki sanal NIC hem de IPv4 ve IPv6 yapılandırmalarla oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC).

```azurepowershell-interactive
  $Ip4Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp4Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv4 `
    -LoadBalancerBackendAddressPool $backendPoolv4 `
    -PublicIpAddress  $RdpPublicIP_1
    
  $Ip6Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp6Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv6 `
    -LoadBalancerBackendAddressPool $backendPoolv6
    
  $NIC_1 = New-AzNetworkInterface `
    -Name "dsNIC1" `
    -ResourceGroupName $rg.ResourceGroupName `
    -Location $rg.Location  `
    -NetworkSecurityGroupId $nsg.Id `
    -IpConfiguration $Ip4Config,$Ip6Config 
    
  $Ip4Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp4Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv4 `
    -LoadBalancerBackendAddressPool $backendPoolv4 `
    -PublicIpAddress  $RdpPublicIP_2  

  $NIC_2 = New-AzNetworkInterface `
    -Name "dsNIC2" `
    -ResourceGroupName $rg.ResourceGroupName `
    -Location $rg.Location  `
    -NetworkSecurityGroupId $nsg.Id `
    -IpConfiguration $Ip4Config,$Ip6Config 

```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM’ler için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = get-credential -Message "DUAL STACK VNET SAMPLE:  Please enter the Administrator credential to log into the VMs."
```

İle Vm'leri oluşturabilirsiniz artık [New-AzVM](/powershell/module/az.compute/new-azvm). Aşağıdaki örnek, zaten mevcut değilse iki VM ve gerekli olan sanal ağ bileşenlerini oluşturur. 

```azurepowershell-interactive
$vmsize = "Standard_A2"
$ImagePublisher = "MicrosoftWindowsServer"
$imageOffer = "WindowsServer"
$imageSKU = "2016-Datacenter"

$vmName= "dsVM1"
$VMconfig1 = New-AzVMConfig -VMName $vmName -VMSize $vmsize -AvailabilitySetId $avset.Id 3> $null | Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent 3> $null | Set-AzVMSourceImage -PublisherName $ImagePublisher -Offer $imageOffer -Skus $imageSKU -Version "latest" 3> $null | Set-AzVMOSDisk -Name "$vmName.vhd" -CreateOption fromImage  3> $null | Add-AzVMNetworkInterface -Id $NIC_1.Id  3> $null 
$VM1 = New-AzVM -ResourceGroupName $rg.ResourceGroupName  -Location $rg.Location  -VM $VMconfig1 

$vmName= "dsVM2"
$VMconfig2 = New-AzVMConfig -VMName $vmName -VMSize $vmsize -AvailabilitySetId $avset.Id 3> $null | Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent 3> $null | Set-AzVMSourceImage -PublisherName $ImagePublisher -Offer $imageOffer -Skus $imageSKU -Version "latest" 3> $null | Set-AzVMOSDisk -Name "$vmName.vhd" -CreateOption fromImage  3> $null | Add-AzVMNetworkInterface -Id $NIC_2.Id  3> $null 
$VM2 = New-AzVM -ResourceGroupName $rg.ResourceGroupName  -Location $rg.Location  -VM $VMconfig2
```

## <a name="determine-ip-addresses-of-the-ipv4-and-ipv6-endpoints"></a>IPv4 ve IPv6 bitiş IP adreslerini belirler
Bu dağıtımla kullanılan IP'ler özetlemek için kaynak grubundaki tüm ağ arabirim nesnelerini almak `get-AzNetworkInterface`. Ayrıca, IPv4 ve IPv6 uç noktaları ile Load Balancer'ın ön uç adreslerini edinin `get-AzpublicIpAddress`.

```azurepowershell-interactive
$rgName= "dsRG1"
$NICsInRG= get-AzNetworkInterface -resourceGroupName $rgName 
write-host `nSummary of IPs in this Deployment: 
write-host ******************************************
foreach ($NIC in $NICsInRG) {
 
    $VMid= $NIC.virtualmachine.id 
    $VMnamebits= $VMid.split("/") 
    $VMname= $VMnamebits[($VMnamebits.count-1)] 
    write-host `nPrivate IP addresses for $VMname 
    $IPconfigsInNIC= $NIC.IPconfigurations 
    foreach ($IPconfig in $IPconfigsInNIC) {
 
        $IPaddress= $IPconfig.privateipaddress 
        write-host "    "$IPaddress 
        IF ($IPconfig.PublicIpAddress.ID) {
 
            $IDbits= ($IPconfig.PublicIpAddress.ID).split("/")
            $PipName= $IDbits[($IDbits.count-1)]
            $PipObject= get-azPublicIpAddress -name $PipName -resourceGroup $rgName
            write-host "    "RDP address:  $PipObject.IpAddress
                 }
         }
 }
 
 
 
  write-host `nPublic IP addresses on Load Balancer:
 
  (get-AzpublicIpAddress -resourcegroupname $rgName | where { $_.name -notlike "RdpPublicIP*" }).IpAddress
```
Aşağıdaki şekilde, özel IPv4 ve IPv6 adresleri iki VM ve yük dengeleyici ön uç IPv4 ve IPv6 IP adreslerini listeleyen örnek çıktı gösterilmektedir.

![Azure'da, ikili yığın (IPv4/IPv6) uygulama dağıtımının IP özeti](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-application-summary.png)

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>IPv6 ikili yığını sanal ağ, Azure portalında görüntüleme
Azure portalında sanal ağ IPv6 ikili yığını gibi görüntüleyebilirsiniz:
1. Portalın arama çubuğunda girin *dsVnet*.
2. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin. Böylece **genel bakış** adlı çift yığın sanal ağ sayfasının *dsVnet*. Çift yığın sanal ağı iki NIC adlı çift yığın alt ağda bulunan IPv4 ve IPv6 yapılandırmaları gösterir *dsSubnet*.

  ![IPv6 ikili yığını azure'da sanal ağ](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)

> [!NOTE]
> Azure portalında Azure sanal ağ için IPv6 kullanılabilir Bu önizleme sürümü için ise salt okunurdur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kaynak grubunu, VM'yi ve tüm ilgili kaynakları.

```azurepowershell-interactive
Remove-AzResourceGroup -Name dsRG1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir çift ön uç IP yapılandırması (IPv4 ve IPv6) ile bir temel yük dengeleyici oluşturulur. Aynı zamanda yük dengeleyicinin arka uç havuzuna eklenmiş olan çift IP yapılandırmaları (IPv4 + IPv6) NIC birlikte iki sanal makine oluşturursunuz. Azure sanal ağlarda IPv6 desteği hakkında daha fazla bilgi için bkz: [Azure sanal ağ için IPv6 nedir?](ipv6-overview.md)