---
title: 'Hızlı Başlangıç: Temel yük dengeleyici - Azure PowerShell oluşturma'
titlesuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, PowerShell kullanarak bir Temel Yük Dengeleyicinin nasıl oluşturulacağı gösterilmektedir
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Basic Load balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2019
ms.author: kumud
ms:custom: seodec18
ms.openlocfilehash: 5b77087557bc4ca1bea3cb4a36a50d787a5edf4d
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729838"
---
# <a name="get-started"></a>Hızlı Başlangıç: Azure PowerShell kullanarak bir genel yük dengeleyici oluşturma

Bu hızlı başlangıçta, Azure PowerShell kullanarak Temel Yük Dengeleyici oluşturma işlemi gösterilmektedir. Yük dengeleyiciyi test etmek için, Windows sunucusu çalıştıran iki sanal makine (VM) dağıtın ve sanal makineler arasında bir web uygulamasının yük dengelemesini yapın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure PowerShell modülü 5.4.1 veya sonraki bir sürümünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Yük dengeleyicinizi oluşturmadan önce bir kaynak grubu oluşturmanız gerekir [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnek *EastUS* konumunda *myResourceGroupLB* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
New-AzResourceGroup `
  -ResourceGroupName "myResourceGroupLB" `
  -Location "EastUS"
```

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma
Uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. Bir genel IP adresiyle oluşturma [yeni AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Aşağıdaki örnek, *myResourceGroupLB* kaynak grubunda *myPublicIP* adlı bir genel IP adresi oluşturur:

```azurepowershell-interactive
$publicIP = New-AzPublicIpAddress `
  -ResourceGroupName "myResourceGroupLB" `
  -Location "EastUS" `
  -AllocationMethod "Static" `
  -Name "myPublicIP"
```

## <a name="create-basic-load-balancer"></a>Temel Yük Dengeleyici Oluşturma

Bu bölümde, ön uç IP ve yük dengeleyici için arka uç adres havuzu yapılandırmanız ve ardından temel Azure Load Balancer oluşturun.

### <a name="create-front-end-ip"></a>Ön uç IP oluşturma

Bir ön uç IP ile oluşturma [yeni AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig). Aşağıdaki örnek, *myFrontEnd* adlı bir ön uç IP yapılandırması oluşturur ve *myPublicIP* adresini ekler:

```azurepowershell-interactive
$frontendIP = New-AzLoadBalancerFrontendIpConfig `
  -Name "myFrontEnd" `
  -PublicIpAddress $publicIP
```

### <a name="configure-back-end-address-pool"></a>Arka uç adres havuzu Yapılandır

İle bir arka uç adres havuzu oluşturun [yeni AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig). Bu arka uç havuzu kalan adımlarda VM'ler iliştirin. Aşağıdaki örnekte adlı bir arka uç adres havuzu oluşturur *myBackEndPool*:

```azurepowershell-interactive
$backendPool = New-AzLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"
```

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma
Yük dengeleyicinin uygulamanızın durumunu izlemesine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. VM, 15 saniyelik aralıklarda art arda iki kez başarısız olursa varsayılan olarak yük dengeleyici dağıtımından kaldırılır. Bir protokolü temel alan bir durum araştırması veya uygulamanız için belirli bir sistem durumu denetim sayfası oluşturun. 

Aşağıdaki örnek bir TCP araştırması oluşturur. Ayrıca daha ayrıntılı sistem durumu denetimleri için özel HTTP araştırmaları oluşturabilirsiniz. Özel bir HTTP yoklaması kullanırken *healthcheck.aspx* gibi bir durum denetimi sayfası oluşturmanız gerekir. Yük dengeleyicinin konağı rotasyonda tutması için yoklamanın **HTTP 200 OK** yanıtını döndürmesi gerekir.

TCP durum araştırması oluşturmak için kullandığınız [Ekle AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig). Aşağıdaki örnek her VM’yi *80* numaralı *HTTP* bağlantı noktasında izleyen *myHealthProbe* adında bir durum yoklaması oluşturur:

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig `
  -Name "myHealthProbe" `
  -RequestPath healthcheck2.aspx `
  -Protocol http `
  -Port 80 `
  -IntervalInSeconds 16 `
  -ProbeCount 2
  ```

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Yalnızca durumu iyi olan VM’lerin trafik almasını sağlamak için kullanılacak durum araştırmasını da tanımlamanız gerekir.

İle bir yük dengeleyici kuralı oluşturma [Ekle AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig). Aşağıdaki örnek, *myLoadBalancerRule* adlı bir yük dengeleyici kuralı oluşturur ve *80* numaralı *TCP* bağlantı noktasında trafiği dengeler:

```azurepowershell-interactive
$lbrule = New-AzLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

### <a name="create-the-nat-rules"></a>NAT kurallarını oluşturma

NAT kuralları ile oluşturma [Ekle AzLoadBalancerRuleConfig](/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig). Aşağıdaki örnek, 4221 ve 4222 numaralı bağlantı noktaları ile arka uç sunucularına RDP bağlantıları kurulmasına izin veren *myLoadBalancerRDP1* ve *myLoadBalancerRDP2* adlı NAT kuralları oluşturur:

```azurepowershell-interactive
$natrule1 = New-AzLoadBalancerInboundNatRuleConfig `
-Name 'myLoadBalancerRDP1' `
-FrontendIpConfiguration $frontendIP `
-Protocol tcp `
-FrontendPort 4221 `
-BackendPort 3389

$natrule2 = New-AzLoadBalancerInboundNatRuleConfig `
-Name 'myLoadBalancerRDP2' `
-FrontendIpConfiguration $frontendIP `
-Protocol tcp `
-FrontendPort 4222 `
-BackendPort 3389
```

### <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

Basic Load Balancer ile oluşturma [AzLoadBalancer yeni](/powershell/module/az.network/new-azloadbalancer). Aşağıdaki örnek, önceki adımlarda oluşturduğunuz ön uç IP yapılandırmasını, arka uç havuzunu, sistem durumu araştırmasını, yük dengeleme kuralını ve NAT kurallarını kullanarak myLoadBalancer adlı genel bir Temel Yük Dengeleyici oluşturur:

```azurepowershell-interactive
$lb = New-AzLoadBalancer `
-ResourceGroupName 'myResourceGroupLB' `
-Name 'MyLoadBalancer' `
-Location 'eastus' `
-FrontendIpConfiguration $frontendIP `
-BackendAddressPool $backendPool `
-Probe $probe `
-LoadBalancingRule $lbrule `
-InboundNatRule $natrule1,$natrule2
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

Bazı VM’leri dağıtabilmeniz ve yük dengeleyicinizi test edebilmeniz için destekleyici ağ kaynakları (sanal ağ ve sanal NIC’ler) oluşturmanız gerekir. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Aşağıdaki örnek *mySubnet* alt ağına sahip *myVnet* adında bir sanal ağ oluşturur:

```azurepowershell-interactive
# Create subnet config
$subnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.2.0/24

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName "myResourceGroupLB" `
  -Location "EastUS" `
  -Name "myVnet" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturma

Sanal ağınıza gelen bağlantıları tanımlamak için ağ güvenlik grubu oluşturun.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>3389 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

Bağlantı noktası 3389 ile RDP bağlantılarına izin verecek şekilde bir ağ güvenlik grubu kuralı oluşturma [yeni AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell-interactive
$rule1 = New-AzNetworkSecurityRuleConfig `
-Name 'myNetworkSecurityGroupRuleRDP' `
-Description 'Allow RDP' `
-Access Allow `
-Protocol Tcp `
-Direction Inbound `
-Priority 1000 `
-SourceAddressPrefix Internet `
-SourcePortRange * `
-DestinationAddressPrefix * `
-DestinationPortRange 3389
```

#### <a name="create-a-network-security-group-rule-for-port-80"></a>80 numaralı bağlantı noktası için ağ güvenlik grubu kuralı oluşturma

Bağlantı noktası 80 üzerinden gelen bağlantılara izin vermek için bir ağ güvenlik grubu kuralı oluşturma [yeni AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell-interactive
$rule2 = New-AzNetworkSecurityRuleConfig `
-Name 'myNetworkSecurityGroupRuleHTTP' `
-Description 'Allow HTTP' `
-Access Allow `
-Protocol Tcp `
-Direction Inbound `
-Priority 2000 `
-SourceAddressPrefix Internet `
-SourcePortRange * `
-DestinationAddressPrefix * `
-DestinationPortRange 80
```

#### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu oluşturun [yeni AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

```azurepowershell-interactive
$nsg = New-AzNetworkSecurityGroup `
-ResourceGroupName 'myResourceGroupLB' `
-Location 'EastUS' `
-Name 'myNetworkSecurityGroup' `
-SecurityRules $rule1,$rule2
```

### <a name="create-nics"></a>NIC’leri oluşturma

Sanal NIC ile oluşturulan oluşturma [yeni AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface). Aşağıdaki örnek iki sanal NIC oluşturur. (Sonraki adımlarda uygulamanız için oluşturduğunuz her bir VM için bir sanal NIC). İstediğiniz zaman ek sanal NIC’ler ve VM’ler oluşturabilir ve bunları yük dengeleyiciye ekleyebilirsiniz:

```azurepowershell-interactive
# Create NIC for VM1
$nicVM1 = New-AzNetworkInterface `
-ResourceGroupName 'myResourceGroupLB' `
-Location 'EastUS' `
-Name 'MyVM1' `
-LoadBalancerBackendAddressPool $backendPool `
-NetworkSecurityGroup $nsg `
-LoadBalancerInboundNatRule $natrule1 `
-Subnet $vnet.Subnets[0]

# Create NIC for VM2
$nicVM2 = New-AzNetworkInterface `
-ResourceGroupName 'myResourceGroupLB' `
-Location 'EastUS' `
-Name 'MyVM2' `
-LoadBalancerBackendAddressPool $backendPool `
-NetworkSecurityGroup $nsg `
-LoadBalancerInboundNatRule $natrule2 `
-Subnet $vnet.Subnets[0]
```

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Uygulamanızın yüksek oranda kullanılabilir olmasını sağlamak için VM’lerinizi bir kullanılabilirlik kümesine yerleştirin.

Bir kullanılabilirlik kümesi oluşturma [yeni AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset). Aşağıdaki örnek *myAvailabilitySet* adında bir kullanılabilirlik kümesi oluşturur:

```azurepowershell-interactive
$availabilitySet = New-AzAvailabilitySet `
  -ResourceGroupName "myResourceGroupLB" `
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

İle Vm'leri oluşturabilirsiniz artık [New-AzVM](/powershell/module/az.compute/new-azvm). Aşağıdaki örnek, zaten mevcut değilse iki VM ve gerekli olan sanal ağ bileşenlerini oluşturur. Bu örnekte, NIC'ler (*VM1* ve *VM2*) önceki adımda oluşturduğunuz sanal makinelere otomatik olarak atanmasını *VM1* ve *VM2*aynı adlara sahip ve aynı sanal ağda atanır (*myVnet*) ve alt ağ (*mySubnet*). Ayrıca, NIC'ler için yük dengeleyicinin arka uç havuzu ilişkili olduğundan, sanal makineleri otomatik olarak arka uç havuzuna eklenir.

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzVm `
        -ResourceGroupName "myResourceGroupLB" `
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

PowerShell komut istemlerinin size döndürülmesi için `-AsJob` parametresi VM’yi arka plan görevi olarak oluşturur. Arka plan işlerinin ayrıntılarını `Job` cmdlet'i ile görüntüleyebilirsiniz. İki VM’yi oluşturup yapılandırmak birkaç dakika sürer.

### <a name="install-iis-with-custom-web-page"></a>Özel web sayfası ile IIS yükleme
 
Her iki arka uç VM’ye de aşağıdaki gibi bir özel web sayfası ile IIS yükleyin:

1. Yük Dengeleyicinin Genel IP adresini alın. `Get-AzPublicIPAddress` kullanarak, Yük Dengeleyicinin Genel IP adresini alın.

   ```azurepowershell-interactive
    Get-AzPublicIPAddress `
    -ResourceGroupName "myResourceGroupLB" `
    -Name "myPublicIP" | select IpAddress
   ```
2. Önceki adımda elde ettiğiniz Genel IP adresini kullanarak VM1 ile bir uzak masaüstü bağlantısı oluşturun. 

   ```azurepowershell-interactive

      mstsc /v:PublicIpAddress:4221  
  
   ```
3. RDP oturumunu başlatmak için *VM1* kimlik bilgilerini girin.
4. VM1’de Windows PowerShell’i başlatın ve IIS sunucusunu yükleyip varsayılan htm dosyasını güncelleştirmek için aşağıdaki komutları kullanın.
    ```azurepowershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello from" + $env:computername)
    ```
5. *myVM1* ile RDP bağlantısını kapatın.
6. `mstsc /v:PublicIpAddress:4222` komutunu çalıştırarak *myVM2* ile bir RDP bağlantısı oluşturun ve *VM2* için adım 4’ü yineleyin.

## <a name="test-load-balancer"></a>Yük dengeleyiciyi test etme
İle yük dengeleyicinizin genel IP adresini [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress). Aşağıdaki örnek, daha önce oluşturulan *myPublicIP* için IP adresini alır:

```azurepowershell-interactive
Get-AzPublicIPAddress `
  -ResourceGroupName "myResourceGroupLB" `
  -Name "myPublicIP" | select IpAddress
```

Sonra da genel IP adresini bir web tarayıcısına girebilirsiniz. Aşağıdaki örnekteki gibi yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adının dahil olduğu web sitesi görüntülenir:

![Yük dengeleyiciyi test etme](media/quickstart-create-basic-load-balancer-powershell/load-balancer-test.png)

Yük dengeleyicinin trafiği, uygulamanızı çalıştıran her iki VM’ye de dağıtmasını görmek için web tarayıcınızı yenilemeye zorlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kaynak grubunu, VM'yi ve tüm ilgili kaynakları.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupLB
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Temel Load Balancer oluşturdunuz, buna VM’ler eklediniz, yük dengeleyici trafik kuralını ve durum araştırmasını yapılandırdınız ve yük dengeleyiciyi test ettiniz. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-basic-internal-portal.md)