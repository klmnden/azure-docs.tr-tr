---
title: "Azure İnternet’e yönelik yük dengeleyicisi oluşturma - PowerShell | Microsoft Docs"
description: "PowerShell kullanarak Resource Manager’da İnternet’e yönelik yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 0347e90fa97a083865e8b783c96034d30a859031
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---

# <a name="get-started"></a>PowerShell kullanarak Resource Manager’da İnternet’e yönelik yük dengeleyici oluşturma

> [!div class="op_single_selector"]
> * [Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Şablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makalede Resource Manager dağıtım modeli anlatılmaktadır. [Klasik dağıtım modelini kullanarak İnternet’e yönelik yük dengeleyici oluşturma](load-balancer-get-started-internet-classic-cli.md) sayfasını da inceleyebilirsiniz.

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Çözümü Azure PowerShell kullanarak dağıtma

Aşağıdaki yordamlarda, Azure Resource Manager ve PowerShell kullanarak İnternet’e yönelik yük dengeleyici oluşturma işlemleri açıklanmaktadır. Azure Resource Manager ile her bir kaynak ayrı ayrı oluşturulup yapılandırıldıktan sonra yük dengeleyici oluşturmak için bir araya getirilir.

Yük dengeleyici dağıtmak için aşağıdaki nesneleri oluşturmanız ve yapılandırmanız gerekir:

* Ön uç IP yapılandırması: Gelen ağ trafiği için genel IP (PIP) adreslerini içerir.
* Arka uç adres havuzu: Sanal makinelerin yük dengeleyiciden ağ trafiği alması için ağ arabirimlerini (NIC’ler) içerir.
* Yük dengeleme kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki bağlantı noktasına eşleyen kuralları içerir.
* Gelen NAT kuralları: Yük dengeleyici üzerindeki bir genel bağlantı noktasını arka uç adres havuzundaki belirli bir sanal makineye ait bağlantı noktasına eşleyen kuralları içerir.
* Araştırmalar: Arka uç adres havuzundaki sanal makine örneklerinin kullanılabilirliğini kontrol etmek için kullanılan durum araştırmalarını içerir.

Daha fazla bilgi için bkz. [Yük Dengeleyici için Azure Resource Manager desteği](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>PowerShell’i Resource Manager’ı kullanacak şekilde ayarlama

PowerShell için Azure Resource Manager’ın en güncel üretim sürümüne sahip olduğunuzdan emin olun:

1. Azure'da oturum açın.

    ```powershell
    Login-AzureRmAccount
    ```

    İstendiğinde kimlik bilgilerinizi girin.

2. Hesapla ilişkili abonelikleri kontrol edin.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Hangi Azure aboneliğinizin kullanılacağını seçin.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Bir kaynak grubu oluşturun. (Mevcut bir kaynak grubunu kullanıyorsanız bu adımı atlayın.)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Ön uç IP havuzu için sanal ağ ve genel IP adresi oluşturma

1. Alt ağ ve sanal ağ oluşturun.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. **loadbalancernrp.westus.cloudapp.azure.com** DNS adına sahip ön uç IP havuzu tarafından kullanılacak **PublicIP** adlı bir Azure genel IP adresi kaynağı oluşturun. Aşağıdaki komutta statik ayırma türü kullanılmaktadır.

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > Yük dengeleyici, FQDN ön eki olarak genel IP’nin etki alanı etiketini kullanır. Bu, yük dengeleyici FQDN değeri olarak bulut hizmeti kullanan klasik dağıtım modelinden farklıdır.
   > Bu örnekte FQDN: **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Ön uç IP havuzu ve arka uç adres havuzu oluşturma

1. **PublicIp** kaynağını kullanan **LB-Frontend** adlı bir ön uç IP havuzu oluşturun.

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. **LB-backend** adlı bir arka uç adres havuzu oluşturun.

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>NAT kuralları, yük dengeleyici kuralı, araştırma ve yük dengeleyici oluşturma

Bu örnek aşağıdaki nesneleri oluşturur:

* 3441 numaralı bağlantı noktasına gelen tüm trafiği 3389 numaralı bağlantı noktasına yönlendiren NAT kuralı
* 3442 numaralı bağlantı noktasına gelen tüm trafiği 3389 numaralı bağlantı noktasına yönlendiren NAT kuralı
* **HealthProbe.aspx** adlı sayfanın durumunu denetleyen araştırma kuralı
* 80 numaralı bağlantı noktasına gelen tüm trafiği arka uç havuzundaki adreslerin 80 numaralı bağlantı noktasıyla dengeleyen yük dengeleyici kuralı
* Tüm bu nesneleri kullanan yük dengeleyici

Şu adımları uygulayın:

1. NAT kurallarını oluşturun.

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. Durum araştırması oluşturun. Araştırmaları iki şekilde yapılandırabilirsiniz:

    HTTP araştırma

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    TCP araştırma

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. Yük dengeleyici kuralı oluşturun.

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. Önceden oluşturulan nesneleri kullanarak yük dengeleyiciyi oluşturun.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a>NIC’leri oluşturma

Ağ arabirimleri oluşturun (veya var olanları düzenleyin) ve bunları NAT kuralları, yük dengeleyici kuralları ve araştırmalarla ilişkilendirin:

1. NIC’lerin oluşturulacağı sanal ağı ve sanal ağ alt ağını belirleyin.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. **lb-nic1-be** adlı bir NIC oluşturup ilk NAT kuralı ve ilk (ve tek) arka uç adres havuzuyla ilişkilendirin.

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. **lb-nic2-be** adlı bir NIC oluşturup ikinci NAT kuralı ve ilk (ve tek) arka uç adres havuzuyla ilişkilendirin.

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. NIC’leri denetleyin.

        $backendnic1

    Beklenen çıktı:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. NIC’leri farklı VM’lere atamak için `Add-AzureRmVMNetworkInterface` cmdlet’ini kullanın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Sanal makine oluşturma ve NIC atama talimatları için bkz. [PowerShell kullanarak Azure VM oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Ağ arabirimini yük dengeleyiciye ekleme

1. Yük dengeleyiciyi Azure’dan alın.

    Yük dengeleyici kaynağını bir değişkene yükleyin (henüz yapmadıysanız). Değişken adı: **$lb**. Daha önce oluşturduğunuz yük dengeleyici kaynağındaki adları kullanın.

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. Arka uç yapılandırmasını bir değişkene yükleyin.

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. Önceden oluşturulan ağ arabirimini bir değişkene yükleyin. Değişken adı: **$nic**. Ağ arabirimi adı önceki örnekle aynıdır.

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. Ağ arabiriminde arka uç yapılandırmasını değiştirin.

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. Ağ arabirimi nesnesini kaydedin.

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    Yük dengeleyici arka uç havuzuna bir ağ arabirimi eklendikten sonra ilgili yük dengeleyici kaynağı için yük dengeleme kurallarına göre ağ trafiğini almaya başlar.

## <a name="update-an-existing-load-balancer"></a>Mevcut yük dengeleyiciyi güncelleştirme

1. Önceki örnekte verilen yük dengeleyiciyi kullanarak **$slb** değişkenine `Get-AzureLoadBalancer` aracılığıyla yük dengeleyici nesnesi atayın.

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. Aşağıdaki örnekte mevcut bir yük dengeleyiciye ön uç için 81 numaralı bağlantı noktasını, arka uç havuzu için ise 8181 numaralı arka uç bağlantı noktasını kullanarak yeni bir Gelen NAT kuralı ekleyin.

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. `Set-AzureLoadBalancer` kullanarak yeni yapılandırmayı kaydedin.

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a>Yük dengeleyici kaldırma

**NRP-RG** adlı kaynak grubunda yer alan **NRP-LB** adlı önceden oluşturulmuş yük dengeleyiciyi silmek için `Remove-AzureLoadBalancer` komutunu kullanın.

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Silme istemini atlamak için isteğe bağlı **-Force** anahtarını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)

