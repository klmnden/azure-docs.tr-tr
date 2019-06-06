---
title: 'Hızlı Başlangıç: Standart yük dengeleyici - Azure PowerShell oluşturma'
titlesuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, PowerShell kullanarak bir Standart Load Balancer’ın nasıl oluşturulacağı gösterilmektedir
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard Load balancer so that I can load balance internet traffic to VMs.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/07/2019
ms.author: kumud
ms:custom: seodec18
ms.openlocfilehash: 04db0232e14ccac7938d7062d77c36a54526489c
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730482"
---
# <a name="quickstart-create-a-standard-load-balancer-using-azure-powershell"></a>Hızlı Başlangıç: Azure PowerShell kullanarak bir Standard Load Balancer oluşturma

Bu hızlı başlangıçta, Azure PowerShell kullanarak bir Standart Load Balancer’ın nasıl oluşturulacağı gösterilmektedir. Yük Dengeleyiciyi test etmek için üç Windows server çalıştıran sanal makine (VM) dağıtın ve Yük Dengelemesi VM'ler arasında bir web uygulaması. Standart Load Balancer hakkında daha fazla bilgi için bkz. [Standart Load Balancer nedir](load-balancer-standard-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Yük dengeleyicinizi oluşturmadan önce bir kaynak grubu oluşturmanız gerekir [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroupSLB* içinde *EastUS* konumu:

```azurepowershell
$rgName='MyResourceGroupSLB'
$location='eastus'
New-AzResourceGroup -Name $rgName -Location $location
```

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. Bir genel IP adresiyle oluşturma [yeni AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Aşağıdaki örnekte adlı bir genel IP adresi oluşturur *Mypublicıp* içinde *myResourceGroupSLB* kaynak grubu:

```azurepowershell
$publicIp = New-AzPublicIpAddress `
 -ResourceGroupName $rgName `
 -Name 'myPublicIP' `
 -Location $location `
 -AllocationMethod static `
 -SKU Standard
```

## <a name="create-standard-load-balancer"></a>Standart Load Balancer oluşturma

Bu bölümde, ön uç IP'sini ve yük dengeleyici için arka uç adres havuzu yapılandırmanız ve ardından standart Load Balancer oluşturun.

### <a name="create-front-end-ip"></a>Ön uç IP oluşturma

Bir ön uç IP ile oluşturma [yeni AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig). Aşağıdaki örnekte adlı bir ön uç IP yapılandırmasını oluşturur *myFrontEnd* ve ekler *Mypublicıp* adresi:

```azurepowershell
$feip = New-AzLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
```

### <a name="configure-back-end-address-pool"></a>Arka uç adres havuzu Yapılandır

İle bir arka uç adres havuzu oluşturun [yeni AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig). Bu arka uç havuzu kalan adımlarda VM'ler iliştirin. Aşağıdaki örnekte adlı bir arka uç adres havuzu oluşturur *myBackEndPool*:

```azurepowershell-interactive
$bepool = New-AzLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma
Yük dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. VM, 15 saniyelik aralıklarda art arda iki kez başarısız olursa varsayılan olarak yük dengeleyici dağıtımından kaldırılır. Bir protokolü temel alan bir durum araştırması veya uygulamanız için belirli bir sistem durumu denetim sayfası oluşturun.

Aşağıdaki örnek bir TCP araştırması oluşturur. Ayrıca daha ayrıntılı sistem durumu denetimleri için özel HTTP araştırmaları oluşturabilirsiniz. Özel bir HTTP yoklaması kullanırken *healthcheck.aspx* gibi bir durum denetimi sayfası oluşturmanız gerekir. Yük dengeleyicinin konağı rotasyonda tutması için yoklamanın **HTTP 200 OK** yanıtını döndürmesi gerekir.

TCP durum araştırması oluşturmak için kullandığınız [Ekle AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig). Aşağıdaki örnek her VM’yi *80* numaralı *HTTP* bağlantı noktasında izleyen *myHealthProbe* adında bir durum yoklaması oluşturur:

```azurepowershell
$probe = New-AzLoadBalancerProbeConfig `
 -Name 'myHealthProbe' `
 -Protocol Http -Port 80 `
 -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma
Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Yalnızca durumu iyi olan VM’lerin trafik almasını sağlamak için kullanılacak durum araştırmasını da tanımlamanız gerekir.

İle bir yük dengeleyici kuralı oluşturma [Ekle AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig). Aşağıdaki örnek, *myLoadBalancerRule* adlı bir yük dengeleyici kuralı oluşturur ve *80* numaralı *TCP* bağlantı noktasında trafiği dengeler:

```azurepowershell
$rule = New-AzLoadBalancerRuleConfig `
  -Name 'myLoadBalancerRuleWeb' -Protocol Tcp `
  -Probe $probe -FrontendPort 80 -BackendPort 80 `
  -FrontendIpConfiguration $feip `
  -BackendAddressPool $bePool
```

### <a name="create-the-nat-rules"></a>NAT kurallarını oluşturma

NAT kuralları ile oluşturma [Ekle AzLoadBalancerRuleConfig](/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig). Aşağıdaki örnekte adlı NAT kuralları oluşturur *myLoadBalancerRDP1* ve *myLoadBalancerRDP2* 4221 ve 4222 bağlantı noktası ile arka uç sunucularına RDP bağlantılarına izin verecek şekilde:

```azurepowershell
$natrule1 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP1' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp -FrontendPort 4221 `
  -BackendPort 3389

$natrule2 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP2' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp `
  -FrontendPort 4222 `
  -BackendPort 3389

$natrule3 = New-AzLoadBalancerInboundNatRuleConfig `
  -Name 'myLoadBalancerRDP3' `
  -FrontendIpConfiguration $feip `
  -Protocol tcp `
  -FrontendPort 4223 `
  -BackendPort 3389
```

### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

Standard Load Balancer ile oluşturma [AzLoadBalancer yeni](/powershell/module/az.network/new-azloadbalancer). Aşağıdaki örnek, genel standart yük adlı ön uç IP yapılandırmasını kullanarak myLoadBalancer dengeleyici oluşturur. arka uç havuzu, sistem durumu araştırması, Yük Dengeleme kuralı ve önceki adımlarda oluşturduğunuz NAT kuralları:

```azurepowershell
$lb = New-AzLoadBalancer `
  -ResourceGroupName $rgName `
  -Name 'MyLoadBalancer' `
  -SKU Standard `
  -Location $location `
  -FrontendIpConfiguration $feip `
  -BackendAddressPool $bepool `
  -Probe $probe `
  -LoadBalancingRule $rule `
  -InboundNatRule $natrule1,$natrule2,$natrule3
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma
Bazı VM’leri dağıtabilmeniz ve yük dengeleyicinizi test edebilmeniz için destekleyici ağ kaynakları (sanal ağ ve sanal NIC’ler) oluşturmanız gerekir. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Aşağıdaki örnek *mySubnet* alt ağına sahip *myVnet* adında bir sanal ağ oluşturur:

```azurepowershell
# Create subnet config
$subnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.2.0/24

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName "myResourceGroupSLB" `
  -Location $location `
  -Name "myVnet" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnetConfig
```
### <a name="create-public-ip-addresses-for-the-vms"></a>VM'ler için genel IP adresi oluşturma

RDP bağlantısı kullanarak sanal makinelerinizi erişmek için VM'ler için genel IP adresi gereklidir. Bu senaryoda bir Standard Load Balancer kullanıldığından sahip VM'ler için standart genel IP adresleri oluşturmalısınız [yeni AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress).

```azurepowershell
$RdpPublicIP_1 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_1" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static
 

$RdpPublicIP_2 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_2" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static


$RdpPublicIP_3 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_3" `
  -ResourceGroupName $RgName `
  -Location $location  `
  -SKU Standard `
  -AllocationMethod static

```
### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma
Sanal ağınıza gelen bağlantıları tanımlamak için ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>3389 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma
Bağlantı noktası 3389 ile RDP bağlantılarına izin verecek şekilde bir ağ güvenlik grubu kuralı oluşturma [yeni AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell

$rule1 = New-AzNetworkSecurityRuleConfig -Name 'myNetworkSecurityGroupRuleRDP' -Description 'Allow RDP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 1000 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 3389
```

#### <a name="create-a-network-security-group-rule-for-port-80"></a>80 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma
Bağlantı noktası 80 üzerinden gelen bağlantılara izin vermek için bir ağ güvenlik grubu kuralı oluşturma [yeni AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell
$rule2 = New-AzNetworkSecurityRuleConfig -Name 'myNetworkSecurityGroupRuleHTTP' -Description 'Allow HTTP' `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 2000 `
  -SourceAddressPrefix Internet -SourcePortRange * `
  -DestinationAddressPrefix * -DestinationPortRange 80
```

#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu oluşturun [yeni AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

```azurepowershell
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $RgName -Location $location `
-Name 'myNetworkSecurityGroup' -SecurityRules $rule1,$rule2
```

### <a name="create-nics"></a>NIC’leri oluşturma
Sanal NIC'ler oluşturun ve genel IP adresi ile ilişkilendirmek ve ağ güvenlik grupları ile önceki adımlarda oluşturulan [yeni AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface). Aşağıdaki örnek üç sanal NIC oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC). İstediğiniz zaman ek sanal NIC’ler ve VM’ler oluşturabilir ve bunları yük dengeleyiciye ekleyebilirsiniz:

```azurepowershell
# Create NIC for VM1
$nicVM1 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic1' -PublicIpAddress $RdpPublicIP_1 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule1 -Subnet $vnet.Subnets[0]

$nicVM2 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic2' -PublicIpAddress $RdpPublicIP_2 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule2 -Subnet $vnet.Subnets[0]

$nicVM3 = New-AzNetworkInterface -ResourceGroupName $rgName -Location $location `
  -Name 'MyNic3' -PublicIpAddress $RdpPublicIP_3 -LoadBalancerBackendAddressPool $bepool -NetworkSecurityGroup $nsg `
  -LoadBalancerInboundNatRule $natrule3 -Subnet $vnet.Subnets[0]
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM’ler için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell
$cred = Get-Credential
```

İle Vm'leri oluşturabilirsiniz artık [New-AzVM](/powershell/module/az.compute/new-azvm). Aşağıdaki örnek, zaten mevcut değilse iki VM ve gerekli olan sanal ağ bileşenlerini oluşturur. Bu örnekte, NIC'ler (*MyNic1*, *MyNic2*, ve *MyNic3*) önceki adımda oluşturduğunuz sanal makinelere atanan *myVM1* , *myVM2*, ve *VM3*. Ayrıca, NIC'ler için yük dengeleyicinin arka uç havuzu ilişkili olduğundan, sanal makineleri otomatik olarak arka uç havuzuna eklenir.

```azurepowershell

# ############## VM1 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM1' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM1' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
 | Add-AzVMNetworkInterface -Id $nicVM1.Id

# Create a virtual machine
$vm1 = New-AzVM -ResourceGroupName $rgName -Zone 1 -Location $location -VM $vmConfig

# ############## VM2 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM2' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM2' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
 | Add-AzVMNetworkInterface -Id $nicVM2.Id

# Create a virtual machine
$vm2 = New-AzVM -ResourceGroupName $rgName -Zone 2 -Location $location -VM $vmConfig

# ############## VM3 ###############

# Create a virtual machine configuration
$vmConfig = New-AzVMConfig -VMName 'myVM3' -VMSize Standard_DS1_v2 `
 | Set-AzVMOperatingSystem -Windows -ComputerName 'myVM3' -Credential $cred `
 | Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2019-Datacenter -Version latest `
| Add-AzVMNetworkInterface -Id $nicVM3.Id

# Create a virtual machine
$vm3 = New-AzVM -ResourceGroupName $rgName -Zone 3 -Location $location -VM $vmConfig
```

Bu, oluşturmak ve üç VM yapılandırmak için birkaç dakika sürer.

### <a name="install-iis-with-a-custom-web-page"></a>IIS ile özel bir web sayfası yükleme

IIS gibi iki arka uç VM'nin özel bir web sayfasıyla yükleyin:

1. Genel IP adreslerini kullanarak bir üç VM alma `Get-AzPublicIPAddress`.

   ```azurepowershell
     $vm1_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_1").IpAddress
     $vm2_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_2").IpAddress
     $vm3_rdp_ip = (Get-AzPublicIPAddress -ResourceGroupName $rgName -Name "RdpPublicIP_3").IpAddress
    ```
2. Uzak Masaüstü bağlantılarıyla oluşturma *myVM1*, *myVM2*, ve *myVM3* kullanarak aşağıdaki gibi sanal makinelerin genel IP adresleri: 

   ```azurepowershell    
     mstsc /v:$vm1_rdp_ip
     mstsc /v:$vm2_rdp_ip
     mstsc /v:$vm3_rdp_ip
   
    ```

3. RDP oturumu başlatmak her bir VM için kimlik bilgilerini girin.
4. Her VM'de Windows PowerShell'i başlatın ve aşağıdaki komutları kullanarak IIS sunucusu yüklemek ve varsayılan htm dosyasını güncelleştirin.

    ```azurepowershell
    # Install IIS
     Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from host " + $env:computername)
    ```

5. RDP bağlantıları ile kapatmak *myVM1*, *myVM2*, ve *myVM3*.


## <a name="test-load-balancer"></a>Yük dengeleyiciyi test etme
İle yük dengeleyicinizin genel IP adresini [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress). Aşağıdaki örnek, daha önce oluşturulan *myPublicIP* için IP adresini alır:

```azurepowershell
Get-AzPublicIPAddress `
  -ResourceGroupName "myResourceGroupSLB" `
  -Name "myPublicIP" | select IpAddress
```

Sonra da genel IP adresini bir web tarayıcısına girebilirsiniz. Aşağıdaki örnekteki gibi yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adının dahil olduğu web sitesi görüntülenir:

![Yük dengeleyiciyi test etme](media/quickstart-create-basic-load-balancer-powershell/load-balancer-test.png)

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran üç VM’ye dağıtmasını görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kaynak grubunu, VM'yi ve tüm ilgili kaynakları.

```azurepowershell
Remove-AzResourceGroup -Name myResourceGroupSLB
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Standart Yük Dengeleyici oluşturdunuz, buna sanal makineler eklediniz, yük dengeleyici trafik kuralını ve durum araştırmasını yapılandırdınız ve yük dengeleyiciyi test ettiniz. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticilerine geçin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-basic-internal-portal.md)
