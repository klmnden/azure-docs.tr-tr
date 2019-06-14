---
title: Yük Dengeleme birden fazla IP yapılandırması - Azure CLI hakkında
titlesuffix: Azure Load Balancer
description: Birincil ve ikincil IP yapılandırmaları arasında yük dengelemeyi.
services: load-balancer
documentationcenter: na
author: anavinahar
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: annahar
ms.openlocfilehash: 9654fd66faa1f745f25494e8b54625a92eb1745b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66111621"
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a>PowerShell kullanarak birden çok IP yapılandırmalarında Yük Dengelemesi

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [CLI](load-balancer-multiple-ip-cli.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)


Bu makalede, bir ikincil ağ arabirimi (NIC) üzerinde birden çok IP adresi ile Azure Load Balancer kullanmayı açıklar. Bu senaryoda, her birincil ve ikincil bir NIC ile Windows çalıştıran iki sanal makine sunuyoruz İkincil ağ arabirimlerine her iki IP yapılandırması vardır. Her VM, hem Web sitesi contoso.com ve fabrikam.com barındırır. Her Web sitesi IP yapılandırmaları birine ikincil NIC üzerinde bağlı Azure Load Balancer iki ön uç IP adresi, bir Web sitesi için ilgili IP yapılandırması trafiği dağıtmak için her Web sitesi için kullanıma sunmak için kullanırız. Bu senaryo aynı bağlantı noktası numarasını hem ön uçlar, hem de arasında her iki arka uç havuz IP adreslerini kullanır.

![LB senaryo görüntüsü](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="steps-to-load-balance-on-multiple-ip-configurations"></a>Yük dengelemek birden fazla IP yapılandırması için adımları

Bu makalede açıklanan senaryo elde etmek için aşağıdaki adımları izleyin:

1. Azure PowerShell'i yükleyin. Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).
2. Aşağıdaki ayarları kullanarak bir kaynak grubu oluşturun:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    Daha fazla bilgi için bkz. 2. adım [bir kaynak grubu oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

3. [Bir kullanılabilirlik kümesi oluşturma](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) Vm'lerinizi içerecek. Bu senaryo için aşağıdaki komutu kullanın:

    ```powershell
    New-AzAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. Yönergeler 3 ile 5'te adımları [Windows VM oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) makalede tek bir NIC ile VM oluşturma hazırlamak için Adım 6.1 yürütün ve 6.2 adım yerine aşağıdaki kullanın:

    ```powershell
    $availset = Get-AzAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    Ardından tamamlayın [Windows VM oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) 6,8 aracılığıyla 6.3 adımları.

5. İkinci bir IP yapılandırması sanal makinelerin her birine ekleyin. Bölümündeki yönergeleri [birden çok IP adresi için sanal makinelere Atayabilmenizi](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) makalesi. Aşağıdaki yapılandırma ayarları kullanın:

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    Bu öğreticinin amacı doğrultusunda genel IP'ler ikincil IP yapılandırmaları ilişkilendirmek gerekmez. Genel IP ilişkilendirme bölümü kaldırmak için komutu düzenleyin.

6. Adım 4-6 Bu makalenin, VM2 için yeniden tamamlayın. Bunu yaparken VM2 VM adıyla değiştirdiğinizden emin olun. İkinci sanal makine için sanal ağ oluşturma gerekmez unutmayın. Alabilir veya sizin kullanım durumunu temel alarak yeni bir alt ağ oluşturabilirsiniz.

7. İki ortak IP adresi oluşturun ve bunları uygun değişkenlerde gösterildiği depolamak:

    ```powershell
    $publicIP1 = New-AzPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. İki ön uç IP yapılandırmasını oluşturun:

    ```powershell
    $frontendIP1 = New-AzLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. Arka uç adres havuzları, araştırma ve Yük Dengeleme kuralları oluşturun:

    ```powershell
    $beaddresspool1 = New-AzLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. Load balancer'ınız şu kaynakların oluşturulduğunu oluşturduktan sonra oluşturun:

    ```powershell
    $mylb = New-AzLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. İkinci arka uç adres havuzu ve ön uç IP yapılandırması, yeni oluşturulan bir yük dengeleyiciye ekleyin:

    ```powershell
    $mylb = Get-AzLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzLoadBalancer

    $mylb | Add-AzLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzLoadBalancer
    
    Add-AzLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzLoadBalancer
    ```

12. Aşağıdaki komutları NIC'ler alın ve ardından her iki her ikincil NIC IP yapılandırmaları yük dengeleyicinin arka uç adres havuzuna ekleyin:

    ```powershell
    $nic1 = Get-AzNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzLoadBalancer

    $nic1 | Set-AzNetworkInterface
    $nic2 | Set-AzNetworkInterface
    ```

13. Son olarak, DNS kaynak kayıtlarını yük dengeleyicinin ilgili ön uç IP adresine işaret edecek şekilde yapılandırmanız gerekir. Azure DNS etki alanlarınızı barındırabilir. Load Balancer ile Azure DNS kullanma hakkında daha fazla bilgi için bkz. [kullanarak Azure DNS diğer Azure hizmetleriyle](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Sonraki adımlar
- Yük Dengeleme hizmetlerini Azure kapsamında birleştirme hakkında daha fazla bilgi [Azure'da Yük Dengeleme hizmetlerini kullanarak](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Günlükleri farklı türde Azure'da yönetmek ve yük dengeleyicide sorunlarını gidermek için nasıl kullanacağınızı öğrenin [Azure İzleyici, Azure Load Balancer için günlükleri](../load-balancer/load-balancer-monitor-log.md).
