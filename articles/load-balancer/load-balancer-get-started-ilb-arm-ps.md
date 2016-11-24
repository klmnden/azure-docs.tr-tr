---
title: "Resource Manager’da PowerShell kullanarak iç yük dengeleyici oluşturma | Microsoft Belgeleri"
description: "Resource Manager’da PowerShell kullanarak iç yük dengeleyici oluşturmayı öğrenin"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
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
ms.sourcegitcommit: 1a1c3c15c51b1e441f21158510e92cc8de057352
ms.openlocfilehash: b9a8b333d688730b5a7773940201178876988ae4

---

# <a name="create-an-internal-load-balancer-using-powershell"></a>PowerShell kullanarak iç yük dengeleyici oluşturma

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Şablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md).  Bu makale, Microsoft’un çoğu yeni dağıtım için [klasik dağıtım modeli](load-balancer-get-started-ilb-classic-ps.md) yerine önerdiği Resource Manager dağıtım modelini açıklamaktadır.

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

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>2. Adım

Hesapla ilişkili abonelikleri kontrol etme

```powershell
Get-AzureRmSubscription
```

Kimlik bilgilerinizle kimliğinizi doğrulamanız istenir.

### <a name="step-3"></a>3. Adım

Hangi Azure aboneliğinizin kullanılacağını seçin.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a>Yük dengeleyici için Kaynak Grubu oluşturma

Yeni bir kaynak grubu oluşturma (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın)

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Tüm yük dengeleyici oluşturma komutlarının aynı kaynak grubunu kullanacağından emin olun.

Yukarıdaki örnekte, "NRP-RG" adlı "Batı ABD" konumlu bir kaynak grubu oluşturduk.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Ön uç IP havuzu için Sanal Ağ ve özel IP adresi oluşturma

Sanal ağ için bir alt ağ oluşturur ve $backendSubnet değişkenine atar

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

Sanal ağ oluşturma:

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

Sanal ağı oluşturur ve lb-subnet-be alt ağını NRPVNet sanal ağına ekler ve $vnet değişkenine atar

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Ön uç IP havuzu ve arka uç adres havuzu oluşturma

Gelen yük dengeleyici ağı trafiği için ön uç IP havuzu ve yük dengeli trafiği almak için arka uç adres havuzu kurma.

### <a name="step-1"></a>1. Adım

Gelen ağ trafiği uç noktası olacak 10.0.2.0/24 alt ağı için 10.0.2.5 özel IP adresini kullanan ön uç IP havuzu oluşturma.

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a>2. Adım

Ön uç IP havuzundan gelen trafiği almak için kullanılacak arka uç adres havuzu kurma:

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>LB kuralları, NAT kuralları, araştırma ve yük dengeleyici oluşturma

Ön uç IP havuzunu ve arka uç adres havuzunu oluşturduktan sonra yük dengeleyici kaynağına ait olacak kuralları oluşturmanız gerekir:

### <a name="step-1"></a>1. Adım

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

Yukarıdaki örnek aşağıdaki öğeleri oluşturur:

* 3441 numaralı bağlantı noktasına gelen tüm trafiği 3389 numaralı bağlantı noktasına yönlendirecek NAT kuralı.
* 3442 numaralı bağlantı noktasına gelen tüm trafiği 3389 numaralı bağlantı noktasına yönlendirecek ikinci NAT kuralı.
* 80 numaralı genel bağlantı noktasına gelen tüm trafik için arka uç adres havuzundaki 80 numaralı yerel bağlantı noktasında yük dengeleme gerçekleştirecek yük dengeleyici kuralı.
* "HealthProbe.aspx" yolu için sistem durumunu kontrol edecek bir araştırma kuralı

### <a name="step-2"></a>2. Adım

Tüm nesneleri (NAT kuralları, Yük dengeleyici kuralları, araştırma yapılandırmaları) ekleyerek yük dengeleyiciyi oluşturma:

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a>Ağ arabirimlerini oluşturma

İç yük dengeleyiciyi oluşturduktan sonra gelen yük dengeli ağ trafiğini alacak olan ağ arabirimlerini, NAT kurallarını ve araştırmayı tanımlamanız gerekir. Bu durumda ağ arabirimi ayrı olarak yapılandırılabilir ve daha sonra bir sanal makineye atanabilir.

### <a name="step-1"></a>1. Adım

Ağ arabirimlerini oluşturmak için kaynak sanal ağını ve alt ağını edinme:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

Bu adımda yük dengeleyici arka uç havuzuna ait olacak ağ birimi oluşturulur ve bu ağ arabirimi için ilk RDP NAT kuralıyla ilişkilendirilir:

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a>2. Adım

LB-Nic2-BE adında ikinci bir ağ arabirimi oluşturma:

Bu adımda ikinci bir ağ arabirimi oluşturulur, aynı yük dengeleyici arka uç havuzuna atanır ve RDP için oluşturulan ikinci NAT kuralıyla ilişkilendirilir:

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

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

Sanal makine oluşturma ve bir NIC’e atama ile ilgili adım adım talimatları şu belgede bulabilirsiniz: [PowerShell kullanarak Azure VM oluşturma](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-the-network-interface"></a>Ağ arabirimini ekleme

Önceden oluşturulan bir sanal makineniz varsa aşağıdaki adımları uygulayarak ağ arabirimini etkileyebilirsiniz:

### <a name="step-1"></a>1. Adım

Yük dengeleyici kaynağını bir değişkene yükleyin (henüz yapmadıysanız). Kullanılan değişken $lb olarak adlandırılır ve yukarıda oluşturulan yük dengeleyici kaynağıyla aynı adları kullanır.

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a>2. Adım

Arka uç yapılandırmasını bir değişkene yükleyin.

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a>3. Adım

Önceden oluşturulan ağ arabirimini bir değişkene yükleyin. $nic değişken adı kullanılır. Kullanılan ağ arabirimi adı yukarıdaki örnekle aynıdır.

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a>4. Adım

Ağ arabiriminde arka uç yapılandırmasını değiştirin.

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a>5. Adım

Ağ arabirimi nesnesini kaydedin.

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Yük dengeleyici arka uç havuzuna bir ağ arabirimi eklendikten sonra ilgili yük dengeleyici kaynağı için yük dengeleme kurallarına göre ağ trafiğini almaya başlar.

## <a name="update-an-existing-load-balancer"></a>Mevcut yük dengeleyiciyi güncelleştirme

### <a name="step-1"></a>1. Adım
Yukarıdaki örnekte verilen yük dengeleyiciyi kullanarak yük dengeleyici nesnesini $slb değişkenine Get-AzureRmLoadBalancer ile atayın

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a>2. Adım

Aşağıdaki örnekte mevcut bir yük dengeleyiciye ön uç için 81 numaralı bağlantı noktasını, arka uç havuzu için ise 8181 numaralı arka uç bağlantı noktasını kullanarak yeni bir Gelen NAT kuralı ekleyeceksiniz

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a>3. Adım

Yeni yapılandırmayı Set-AzureLoadBalancer kullanarak kaydedin

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a>Yük dengeleyici kaldırma

“NRP-RG” adlı kaynak grubunda yer alan “NRP-LB” adlı önceden oluşturulmuş yük dengeleyiciyi silmek için Remove-AzureRmLoadBalancer komutunu kullanın

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Silme istemini atlamak için isteğe bağlı -Force anahtarını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Nov16_HO3-->


