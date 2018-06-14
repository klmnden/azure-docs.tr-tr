---
title: IPv6 - PowerShell ile bir Azure Internet'e yönelik Yük Dengeleyici oluşturma | Microsoft Docs
description: Internet'e yönelik Yük Dengeleyici kaynak yöneticisi için PowerShell kullanarak IPv6 oluşturmayı öğrenin
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
keywords: IPv6, azure yük dengeleyici, çift yığın, genel IP, yerel IPv6, mobil, IOT
ms.assetid: d4c649e3-84ad-4343-8b6a-0e89f0b9e518
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 1e369307fba815554b7a34fd430b2e259137c5d6
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31593517"
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Internet'e yönelik Yük Dengeleyici kaynak yöneticisi için PowerShell kullanarak IPv6 oluşturmaya başlamak

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Şablon](load-balancer-ipv6-internet-template.md)




Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük dengeleyici, gelen trafiği bulut hizmetlerindeki sağlıklı hizmet örnekleri veya bir yük dengeleyici kümesindeki sanal makineler arasında dağıtarak yüksek kullanılabilirlik sağlar. Ayrıca, Azure Load Balancer bu hizmetleri birden çok bağlantı noktasında, birden çok IP adresinde ya da her ikisinde birden sağlayabilir.

## <a name="example-deployment-scenario"></a>Örnek dağıtım senaryosu

Aşağıdaki diyagram, bu makaledeki dağıtılan çözümü dengelemesini gösterir.

![Yük dengeleyici senaryosu](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

Bu senaryoda aşağıdaki Azure kaynakları oluşturulur:

* bir IPv4 ve IPv6 genel IP adresi olan bir Internet'e yönelik Yük Dengeleyici
* iki Yük Dengeleme kuralları özel uç noktaları için ortak VIP'ler eşlemek için
* bir kullanılabilirlik kümesi, iki VM içerir
* iki sanal makine (VM)
* atanan IPv4 ve IPv6 adreslerinin her VM için bir sanal ağ arabirimi

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Azure PowerShell kullanarak çözümü dağıtma

Aşağıdaki adımlar, Internet'e yönelik Yük Dengeleyici PowerShell ile Azure Resource Manager kullanarak nasıl oluşturulacağını gösterir. Azure Resource Manager ile her bir kaynak oluşturulur ve birlikte bir kaynak oluşturmak için ENTER koy ayrı ayrı yapılandırılır.

Bir yük dengeleyici dağıtmayı oluşturun ve aşağıdaki nesneleri yapılandırın:

* Ön uç IP yapılandırması: Gelen ağ trafiği için genel IP adreslerini içerir.
* Arka uç adres havuzu: Sanal makinelerin yük dengeleyiciden ağ trafiği alması için ağ arabirimlerini (NIC’ler) içerir.
* Yük dengeleme kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki bağlantı noktasına eşleme kurallarını içerir.
* Gelen NAT kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki belirli bir sanal makineye ait bağlantı noktasına eşleme kurallarını içerir.
* Araştırmalar: Arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini kontrol etmek için kullanılan durum araştırmalarını içerir.

Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell’i Resource Manager’ı kullanacak şekilde ayarlama

PowerShell için Azure Resource Manager modülüyle en son ürün sürümüne sahip olduğunuzdan emin olun.

1. Oturum Azure açın

    ```powershell
    Connect-AzureRmAccount
    ```

    İstendiğinde kimlik bilgilerinizi girin.

2. Hesapla ilişkili abonelikleri kontrol etme

    ```powershell
    Get-AzureRmSubscription
    ```

3. Hangi Azure aboneliğinizin kullanılacağını seçin.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Bir kaynak grubu (var olan bir kaynak grubu kullanıyorsanız bu adımı atla) oluşturun

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Ön uç IP havuzu için sanal ağ ve genel IP adresi oluşturma

1. Bir sanal ağ alt ağı oluşturun.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Azure genel IP adresi (PIP) kaynakları için ön uç IP adresi havuzu oluşturun.

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > Yük Dengeleyici genel IP etki alanı etiketini FQDN'sini için önek olarak kullanır. Bu örnekte, FQDN'ler olan *lbnrpipv4.westus.cloudapp.azure.com* ve *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Bir ön uç IP yapılandırmaları ve arka uç adres havuzu oluşturma

1. Oluşturduğunuz ortak IP adresleri kullanan ön uç adresi yapılandırmasını oluşturun.

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. Arka uç adres havuzu oluşturun.

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>LB kuralları, NAT kuralları, bir araştırma ve bir yük dengeleyici oluşturma

Bu örnek aşağıdaki nesneleri oluşturur:

* bağlantı noktası 443 numaralı bağlantı noktasına 4443 tüm gelen trafiği çevirmek için NAT kuralı
* 80 numaralı bağlantı noktasına gelen tüm trafiği arka uç havuzundaki adreslerin 80 numaralı bağlantı noktasıyla dengeleyen yük dengeleyici kuralı.
* bağlantı noktası 3389 sanal makinelerin RDP bağlantılarına izin vermek için bir yük dengeleyici kuralı.
* adlı bir sayfada sistem durumunu denetlemek için bir araştırma kuralı *HealthProbe.aspx* veya bir hizmet bağlantı noktası 8080
* Bu nesneler kullanan bir yük dengeleyici

1. NAT kurallarını oluşturun.

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. Durum araştırması oluşturun. Araştırmaları iki şekilde yapılandırabilirsiniz:

    HTTP araştırma

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    veya TCP araştırması

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    Bu örnekte, biz TCP araştırmalar kullanacaksanız.

3. Yük dengeleyici kuralı oluşturun.

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. Yük Dengeleyici daha önce oluşturulan nesneleri kullanarak oluşturun.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-the-back-end-vms"></a>Arka uç VM'ler için NIC oluşturun

1. Sanal ağ ve sanal ağ alt, burada NIC'ler oluşturulması gerektiğini öğrenin.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. IP yapılandırmaları ve NIC'ler, VM'ler için oluşturun.

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Sanal makineler oluşturun ve yeni oluşturulan NIC'ler atayın

Bir VM oluşturma hakkında daha fazla bilgi için bkz: [oluşturma ve Resource Manager ve Azure PowerShell ile Windows sanal makine önceden yapılandırın](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)

1. Bir kullanılabilirlik kümesi ve depolama hesabı oluşturma

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. Her bir VM oluşturun ve önceki NIC'ler oluşturulan atayın

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type the username and password of the local administrator account."

    $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
