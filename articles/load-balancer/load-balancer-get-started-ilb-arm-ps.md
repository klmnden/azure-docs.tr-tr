---
title: "Resource Manager’da PowerShell kullanarak iç yük dengeleyici oluşturma | Microsoft Belgeleri"
description: "Resource Manager’da PowerShell kullanarak iç yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 02d32ef115a6c2d9b0bb891231f3b45051ef0675


---
# <a name="create-an-internal-load-balancer-using-powershell"></a>PowerShell kullanarak iç yük dengeleyici oluşturma
[!INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

<BR>
[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]

[klasik dağıtım modeli](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

Aşağıdaki adımlarda, Azure Resource Manager ve PowerShell kullanarak iç yük dengeleyici oluşturma işlemleri açıklanmaktadır. Azure Resource Manager ile iç yük dengeleyici oluşturmak için gerekli olan adımları ayrı ayrı tamamlamanız, ardından bunları birleştirerek yük dengeleyici oluşturmanız gerekir.

Yük dengeleyici dağıtmak için aşağıdaki nesneleri oluşturmanız ve yapılandırmanız gerekir:

* Ön uç IP yapılandırması: Gelen ağ trafiği için özel IP adresinin yapılandırılması
* Arka uç adres havuzu: Ön uç IP havuzundan gelen yük dengeli trafiği alacak ağ arabirimlerinin yapılandırılması
* Yük dengeleme kuralları: Yük dengeleyici için kaynak ve yerel bağlantı noktası yapılandırması.
* Araştırmalar: Sanal Makine örnekleri için durum araştırması yapılandırması.
* Gelen NAT kuralları: Sanal Makine örneklerinden birine doğrudan erişmek için bağlantı noktası kurallarının yapılandırılması.

Azure Resource Manager içindeki yük dengeleyici bileşenleri hakkında daha fazla bilgi için bkz. [Azure Resource Manager yük dengeleyici desteği](load-balancer-arm.md).

Aşağıdaki adımlarda iki sanal makine arasında yük dengeleyici yapılandırma işlemleri açıklanmaktadır.

## <a name="setup-powershell-to-use-resource-manager"></a>PowerShell’i Resource Manager’ı kullanacak şekilde ayarlama
PowerShell Azure modülünün son üretim sürümüne sahip olduğunuzdan ve PowerShell ayarlarının Azure aboneliğinize doğrudan erişecek şekilde yapıldığından emin olun.

### <a name="step-1"></a>1. Adım
        Login-AzureRmAccount

### <a name="step-2"></a>2. Adım
Hesapla ilişkili abonelikleri kontrol etme

        Get-AzureRmSubscription

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.<BR>

### <a name="step-3"></a>3. Adım
Hangi Azure aboneliğinizin kullanılacağını seçin. <BR>

        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Yük dengeleyici için Kaynak Grubu oluşturma
### <a name="step-4"></a>4. Adım
Yeni bir kaynak grubu oluşturma (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Tüm yük dengeleyici oluşturma komutlarının aynı kaynak grubunu kullanacağından emin olun.

Yukarıdaki örnekte, "NRP-RG" adlı "Batı ABD" konumlu bir kaynak grubu oluşturduk.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Ön uç IP havuzu için Sanal Ağ ve özel IP adresi oluşturma
### <a name="step-1"></a>1. Adım
Sanal ağ için bir alt ağ oluşturur ve $backendSubnet değişkenine atar

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Sanal ağ oluşturma:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Sanal ağı oluşturur ve lb-subnet-be alt ağını NRPVNet sanal ağına ekler ve $vnet değişkenine atar

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Ön uç IP havuzu ve arka uç adres havuzu oluşturma
Gelen yük dengeleyici ağı trafiği için ön uç IP havuzu ve yük dengeli trafiği almak için arka uç adres havuzu kurma.

### <a name="step-1"></a>1. Adım
Gelen ağ trafiği uç noktası olacak 10.0.2.0/24 alt ağı için 10.0.2.5 özel IP adresini kullanan ön uç IP havuzu oluşturma.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>2. Adım
Ön uç IP havuzundan gelen trafiği almak için kullanılacak arka uç adres havuzu kurma:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>LB kuralları, NAT kuralları, araştırma ve yük dengeleyici oluşturma
Ön uç IP havuzunu ve arka uç adres havuzunu oluşturduktan sonra yük dengeleyici kaynağına ait olacak kuralları oluşturmanız gerekir:

### <a name="step-1"></a>1. Adım
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Yukarıdaki örnek aşağıdaki öğeleri oluşturur:

* 3441 numaralı bağlantı noktasına gelen tüm trafiği 3389 numaralı bağlantı noktasına yönlendirecek NAT kuralı.
* 3442 numaralı bağlantı noktasına gelen tüm trafiği 3389 numaralı bağlantı noktasına yönlendirecek ikinci NAT kuralı.
* 80 numaralı genel bağlantı noktasına gelen tüm trafik için arka uç adres havuzundaki 80 numaralı yerel bağlantı noktasında yük dengeleme gerçekleştirecek yük dengeleyici kuralı.
* "HealthProbe.aspx" yolu için sistem durumunu kontrol edecek bir araştırma kuralı

### <a name="step-2"></a>2. Adım
Tüm nesneleri (NAT kuralları, Yük dengeleyici kuralları, araştırma yapılandırmaları) ekleyerek yük dengeleyiciyi oluşturma:

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Ağ arabirimlerini oluşturma
İç yük dengeleyiciyi oluşturduktan sonra gelen yük dengeli ağ trafiğini alacak olan ağ arabirimlerini, NAT kurallarını ve araştırmayı tanımlamanız gerekir. Bu durumda ağ arabirimi ayrı olarak yapılandırılabilir ve daha sonra bir sanal makineye atanabilir.

### <a name="step-1"></a>1. Adım
Ağ arabirimlerini oluşturmak için kaynak sanal ağını ve alt ağını edinme:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Bu adımda yük dengeleyici arka uç havuzuna ait olacak ağ birimi oluşturulur ve bu ağ arabirimi için ilk RDP NAT kuralıyla ilişkilendirilir:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>2. Adım
LB-Nic2-BE adında ikinci bir ağ arabirimi oluşturma:

Bu adımda ikinci bir ağ arabirimi oluşturulur, aynı yük dengeleyici arka uç havuzuna atanır ve RDP için oluşturulan ikinci NAT kuralıyla ilişkilendirilir:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Sonuç şunu verecektir:

    $backendnic1

Beklenen çıktı:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>3. Adım
NIC’i bir sanal makineye atamak için Add-AzureRmVMNetworkInterface komutunu kullanın.

Sanal makine oluşturma ve bir NIC’e atama ile ilgili adım adım talimatları şu belgede bulabilirsiniz: [PowerShell kullanarak Azure VM oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md).

Önceden oluşturulan bir sanal makineniz varsa aşağıdaki adımları uygulayarak ağ arabirimini etkileyebilirsiniz:

#### <a name="step-1"></a>1. Adım
Yük dengeleyici kaynağını bir değişkene yükleyin (henüz yapmadıysanız). Kullanılan değişken $lb olarak adlandırılır ve yukarıda oluşturulan yük dengeleyici kaynağıyla aynı adları kullanır.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>2. Adım
Arka uç yapılandırmasını bir değişkene yükleyin.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>3. Adım
Önceden oluşturulan ağ arabirimini bir değişkene yükleyin. $nic değişken adı kullanılır. Kullanılan ağ arabirimi adı yukarıdaki örnekle aynıdır.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>4. Adım
Ağ arabiriminde arka uç yapılandırmasını değiştirin.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>5. Adım
Ağ arabirimi nesnesini kaydedin.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Yük dengeleyici arka uç havuzuna bir ağ arabirimi eklendikten sonra ilgili yük dengeleyici kaynağı için yük dengeleme kurallarına göre ağ trafiğini almaya başlar.

## <a name="update-an-existing-load-balancer"></a>Mevcut yük dengeleyiciyi güncelleştirme
### <a name="step-1"></a>1. Adım
Yukarıdaki örnekte verilen yük dengeleyiciyi kullanarak yük dengeleyici nesnesini $slb değişkenine Get-AzureRmLoadBalancer ile atayın

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>2. Adım
Aşağıdaki örnekte mevcut bir yük dengeleyiciye ön uç için 81 numaralı bağlantı noktasını, arka uç havuzu için ise 8181 numaralı arka uç bağlantı noktasını kullanarak yeni bir Gelen NAT kuralı ekleyeceksiniz

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>3. Adım
Yeni yapılandırmayı Set-AzureLoadBalancer kullanarak kaydedin

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Yük dengeleyici kaldırma
“NRP-RG” adlı kaynak grubunda yer alan “NRP-LB” adlı önceden oluşturulmuş yük dengeleyiciyi silmek için Remove-AzureRmLoadBalancer komutunu kullanın

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

> [!NOTE]
> Silme istemini atlamak için isteğe bağlı -Force anahtarını kullanabilirsiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)




<!--HONumber=Nov16_HO2-->


