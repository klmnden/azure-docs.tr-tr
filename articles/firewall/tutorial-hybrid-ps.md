---
title: Azure PowerShell kullanarak hibrit bir ağda Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma
description: Bu öğreticide Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 10/2/2018
ms.author: victorh
ms.openlocfilehash: 27221ac4b23f52dd6976a959e6e5529eb0cc89fa
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856080"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-in-a-hybrid-network-using-azure-powershell"></a>Öğretici: Azure PowerShell kullanarak hibrit bir ağda Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ağ ortamını oluşturma
> * Güvenlik duvarını yapılandırma ve dağıtma
> * Yolları oluşturma
> * Sanal makineleri oluşturma
> * Güvenlik duvarını test etme

Bu öğretici için üç sanal ağ oluşturursunuz:
- **VNet-Hub** - güvenlik duvarı bu sanal ağ içindedir.
- **VNet-Spoke** - Uç sanal ağ, Azure'da bulunan iş yükünü temsil eder.
- **VNet-Onprem** - OnPrem sanal ağ, şirket içi ağ temsil eder. Gerçek bir dağıtımda, VPN veya Express Route bağlantısıyla bağlanabilir. Kolaylık olması için, bu öğreticide VPN ağ geçidi bağlantısı kullanılır ve şirket içi ağı temsil etmesi için de Azure'da bulunan bir sanal ağ kullanılır.

![Hibrit ağda güvenlik duvarı](media/tutorial-hybrid-ps/hybrid-network-firewall.png)

## <a name="key-requirements"></a>Önemli gereksinimler

Bu senaryonun doğru çalışması için üç önemli gereksinimi vardır:

- Uç alt ağda varsayılan ağ geçidi olarak Azure Güvenlik Duvarı IP adresine işaret eden Kullanıcı Tanımlı Yol. Bu yol tablosunda BGP yol yaymanın **Devre Dışı** olması gerekir.
- Hub ağ geçidi alt ağındaki Kullanıcı Tanımlı Yol, uç ağlara yönelik sonraki atlama olarak güvenlik duvarı IP adresine işaret etmelidir.
- Azure Güvenlik Duvarı alt ağında Kullanıcı Tanımlı Yol gerekmez çünkü bu alt ağ yolları BGP'den öğrenir.
- VNet-Hub'ı VNet-Spoke'a eşlerken **AllowGatewayTransit** ayarladığınızdan ve VNet-Spoke'u VNet-Hub'a eşlerken de **UseRemoteGateways** ayarladığınızdan emin olun.

Bu yolların nasıl oluşturulduğunu görmek için [Yolları Oluşturma](#create-routes) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="declare-the-variables"></a>Değişkenleri tanımlama
Aşağıdaki örnekte bu öğreticiye yönelik değerleri kullanan değişkenler bildirilmektedir. Çoğu durumda değerleri kendi değerlerinizle değiştirmeniz gerekir. Ancak, bu tür yapılandırmaları tanımaya başlamak için adımları gözden geçiriyorsanız bu değişkenleri kullanabilirsiniz. Gerekirse değişkenleri değiştirin, daha sonra kopyalayın ve PowerShell konsolunuza yapıştırın.

```azurepowershell
$RG1 = "FW-Hybrid-Test"
$Location1 = "East US"

# Variables for the firewall hub VNet

$VNetnameHub = "VNet-hub"
$SNnameHub = "AzureFirewallSubnet"
$VNetHubPrefix = "10.5.0.0/16"
$SNHubPrefix = "10.5.0.0/24"
$SNGWHubPrefix = "10.5.1.0/24"
$GWHubName = "GW-hub"
$GWHubpipName = "VNet-hub-GW-pip"
$GWIPconfNameHub = "GW-ipconf-hub"
$ConnectionNameHub = "hub-to-Onprem"

# Variables for the spoke VNet

$VnetNameSpoke = "VNet-Spoke"
$SNnameSpoke = "SN-Workload"
$VNetSpokePrefix = "10.6.0.0/16"
$SNSpokePrefix = "10.6.0.0/24"
$SNSpokeGWPrefix = "10.6.1.0/24"

# Variables for the OnPrem VNet

$VNetnameOnprem = "Vnet-Onprem"
$SNNameOnprem = "SN-Corp"
$VNetOnpremPrefix = "192.168.0.0/16"
$SNOnpremPrefix = "192.168.1.0/24"
$SNGWOnpremPrefix = "192.168.2.0/24"
$GWOnpremName = "GW-Onprem"
$GWIPconfNameOnprem = "GW-ipconf-Onprem"
$ConnectionNameOnprem = "Onprem-to-hub"
$GWOnprempipName = "VNet-Onprem-GW-pip"

$SNnameGW = "GatewaySubnet"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Bu öğreticide gereken tüm kaynakları içerecek bir kaynak grubu oluşturun:

```azurepowershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```

## <a name="create-and-configure-the-firewall-hub-vnet"></a>Güvenlik duvarı hub'ı sanal ağını oluşturma ve yapılandırma

Sanal ağa eklenecek alt ağları tanımlayın:

```azurepowershell
$FWsub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameHub -AddressPrefix $SNHubPrefix
$GWsub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNGWHubPrefix
```

Şimdi güvenlik duvarı hub'ı sanal ağını oluşturun:

```azurepowershell
$VNetHub = New-AzureRmVirtualNetwork -Name $VNetnameHub -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetHubPrefix -Subnet $FWsub,$GWsub
```
Sanal ağınız için oluşturacağınız VPN ağ geçidine ayrılacak genel IP adresi isteyin. *AllocationMethod* değerinin **Dynamic** olduğuna dikkat edin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, VPN ağ geçidinize dinamik olarak ayrılır. 

  ```azurepowershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWHubpipName -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
```
## <a name="create-and-configure-the-spoke-vnet"></a>Uç sanal ağı oluşturma ve yapılandırma

Uç sanal ağa eklenecek alt ağları tanımlayın:

```azurepowershell
$Spokesub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameSpoke -AddressPrefix $SNSpokePrefix
$GWsubSpoke = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNSpokeGWPrefix
```

Uç sanal ağı oluşturun:

```azurepowershell
$VNetSpoke = New-AzureRmVirtualNetwork -Name $VnetNameSpoke -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetSpokePrefix -Subnet $Spokesub,$GWsubSpoke
```

## <a name="create-and-configure-the-onprem-vnet"></a>OnPrem sanal ağını oluşturma ve yapılandırma

Sanal ağa eklenecek alt ağları tanımlayın:

```azurepowershell
$Onpremsub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNNameOnprem -AddressPrefix $SNOnpremPrefix
$GWOnpremsub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNGWOnpremPrefix
```

Şimdi OnPrem sanal ağını oluşturun:

```azurepowershell
$VNetOnprem = New-AzureRmVirtualNetwork -Name $VNetnameOnprem -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetOnpremPrefix -Subnet $Onpremsub,$GWOnpremsub
```
Sanal ağ için oluşturacağınız ağ geçidine ayrılacak genel IP adresi isteyin. *AllocationMethod* değerinin **Dynamic** olduğuna dikkat edin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, ağ geçidinize dinamik olarak ayrılır. 

  ```azurepowershell
  $gwOnprempip = New-AzureRmPublicIpAddress -Name $GWOnprempipName -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
```

## <a name="configure-and-deploy-the-firewall"></a>Güvenlik duvarını yapılandırma ve dağıtma

Şimdi güvenlik duvarını hab sanal ağına dağıtın.

```azurepowershell
# Get a Public IP for the firewall
$FWpip = New-AzureRmPublicIpAddress -Name "fw-pip" -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Static -Sku Standard
# Create the firewall
$Azfw = New-AzureRmFirewall -Name AzFW01 -ResourceGroupName $RG1 -Location $Location1 -VirtualNetworkName $VNetnameHub -PublicIpName fw-pip

#Save the firewall private IP address for future use

$AzfwPrivateIP = $Azfw.IpConfigurations.privateipaddress
$AzfwPrivateIP

```

### <a name="configure-network-rules"></a>Ağ kurallarını yapılandırma

<!--- $Rule2 = New-AzureRmFirewallNetworkRule -Name "AllowPing" -Protocol ICMP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort *--->

```azurepowershell
$Rule1 = New-AzureRmFirewallNetworkRule -Name "AllowWeb" -Protocol TCP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort 80

$Rule3 = New-AzureRmFirewallNetworkRule -Name "AllowRDP" -Protocol TCP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort 3389

$NetRuleCollection = New-AzureRmFirewallNetworkRuleCollection -Name RCNet01 -Priority 100 `
   -Rule $Rule1,$Rule2,$Rule3 -ActionType "Allow"
$Azfw.NetworkRuleCollections = $NetRuleCollection
Set-AzureRmFirewall -AzureFirewall $Azfw
```
### <a name="configure-an-application-rule"></a>Uygulama kuralı yapılandırma

```azurepowershell
$Rule4 = New-AzureRmFirewallApplicationRule -Name "AllowBing" -Protocol "Http:80","Https:443" `
   -SourceAddress $SNOnpremPrefix -TargetFqdn "bing.com"

$AppRuleCollection = New-AzureRmFirewallApplicationRuleCollection -Name RCApp01 -Priority 100 `
   -Rule $Rule4 -ActionType "Allow"
$Azfw.ApplicationRuleCollections = $AppRuleCollection
Set-AzureRmFirewall -AzureFirewall $Azfw

```

## <a name="create-and-connect-the-vpn-gateways"></a>VPN ağ geçitlerini oluşturma ve bağlama

Hub ve OnPrem sanal ağları VPN ağ geçitleri üzerinden bağlanır.

### <a name="create-a-vpn-gateway-for-the-hub-vnet"></a>Hub sanal ağı için VPN ağ geçidini oluşturma

VPN ağ geçidi yapılandırmasını oluşturun. VPN ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar.

  ```azurepowershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetnameHub -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfNameHub `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```

Şimdi hub sanal ağı için VPN ağ geçidini oluşturun. Sanal Ağdan Sanal Ağa yapılandırmaları, RouteBased bir VPNType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

```azurepowershell
New-AzureRmVirtualNetworkGateway -Name $GWHubName -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku basic
```

### <a name="create-a-vpn-gateway-for-the-onprem-vnet"></a>OnPrem sanal ağı için VPN ağ geçidini oluşturma

VPN ağ geçidi yapılandırmasını oluşturun. VPN ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar.

  ```azurepowershell
$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetnameOnprem -ResourceGroupName $RG1
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfNameOnprem `
  -Subnet $subnet2 -PublicIpAddress $gwOnprempip
  ```

Şimdi OnPrem sanal ağı için VPN ağ geçidini oluşturun. Sanal Ağdan Sanal Ağa yapılandırmaları, RouteBased bir VPNType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

```azurepowershell
New-AzureRmVirtualNetworkGateway -Name $GWOnpremName -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gwipconf2 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku basic
```
### <a name="create-the-vpn-connections"></a>VPN bağlantılarını oluşturma
Şimdi hub ile OnPrem ağ geçitleri arasında VPN bağlantılarını oluşturabilirsiniz

#### <a name="get-the-vpn-gateways"></a>VPN ağ geçitlerini alma
```azurepowershell
$vnetHubgw = Get-AzureRmVirtualNetworkGateway -Name $GWHubName -ResourceGroupName $RG1
$vnetOnpremgw = Get-AzureRmVirtualNetworkGateway -Name $GWOnpremName -ResourceGroupName $RG1
```

#### <a name="create-the-connections"></a>Bağlantıları oluşturma

Bu adımda, hub sanal ağından OnPrem sanal ağına bağlantı oluşturursunuz. Örneklerde paylaşılan bir anahtar göreceksiniz. Paylaşılan anahtar için kendi değerlerinizi kullanabilirsiniz. Paylaşılan anahtarın her iki bağlantıyla da eşleşiyor olması önemlidir. Bir bağlantı oluşturmak çok zaman almaz.

```azurepowershell
New-AzureRmVirtualNetworkGatewayConnection -Name $ConnectionNameHub -ResourceGroupName $RG1 `
-VirtualNetworkGateway1 $vnetHubgw -VirtualNetworkGateway2 $vnetOnpremgw -Location $Location1 `
-ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
```
OnPrem sanal ağından hub sanal ağına bağlantı oluşturun. Bu adım bir öncekine benzer; tek farkı bağlantıyı Vnet-Onprem'den VNet-hub'a oluşturmanızdır. Paylaşılan anahtarların eşleştiğinden emin olun. Bağlantı birkaç dakika içerisinde kurulacaktır.

  ```azurepowershell
  New-AzureRmVirtualNetworkGatewayConnection -Name $ConnectionNameOnprem -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnetOnpremgw -VirtualNetworkGateway2 $vnetHubgw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```
#### <a name="verify-the-connection"></a>Bağlantıyı doğrulama
 
*Get-AzureRmVirtualNetworkGatewayConnection* cmdlet’ini *-Debug* ile veya bu parametre olmadan kullanarak başarılı bir bağlantıyı doğrulayabilirsiniz. Aşağıdaki cmdlet örneğini kullanın ve değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. İstendiğinde, **Tümü**'nü çalıştırmak için **A** seçin. Örnekte bulunan *-Name*, test etmek istediğiniz bağlantının adıdır.

```azurepowershell
Get-AzureRmVirtualNetworkGatewayConnection -Name $ConnectionNameHub -ResourceGroupName $RG1
```

Cmdlet tamamlandıktan sonra değerleri görüntüleyin. Aşağıdaki örnekte, bağlantı durumu *Bağlandı* olarak gösterilir; ayrıca giriş ve çıkış baytlarını görebilirsiniz.

```
"connectionStatus": "Connected",
"ingressBytesTransferred": 33509044,
"egressBytesTransferred": 4142431
```



## <a name="peer-the-hub-and-spoke-vnets"></a>Hub ve uç sanal ağlarını eşleme

Artık uç ve hub sanal ağlarını eşleyin.

```azurepowershell
# Peer hub to spoke
Add-AzureRmVirtualNetworkPeering -Name HubtoSpoke -VirtualNetwork $VNetHub -RemoteVirtualNetworkId $VNetSpoke.Id -AllowGatewayTransit

# Peer spoke to hub
Add-AzureRmVirtualNetworkPeering -Name SpoketoHub -VirtualNetwork $VNetSpoke -RemoteVirtualNetworkId $VNetHub.Id -AllowForwardedTraffic -UseRemoteGateways
```
## <a name="create-routes"></a>Yolları oluşturma

Şimdi birkaç yol oluşturun: 
- Güvenlik duvarı IP adresi üzerinden hub ağ geçidi alt ağından uç alt ağına giden bir yol
- Güvenlik duvarı IP adresi üzerinden uç alt ağından gelen varsayılan yol

> [!NOTE]
> Azure Güvenlik Duvarı, BGP kullanarak şirket içi ağlarınızı öğrenir. Buna İnternet trafiğini şirket içi ağınızdan yönlendirecek varsayılan rota dahil olabilir. Bunun yerine İnternet trafiğinin güvenlik duvarından doğrudan İnternete gönderilmesini isterseniz AzureFirewallSubnet üzerine **İnternet** atlama türüne sahip kullanıcı tanımlı varsayılan rota (0.0.0.0/0) ekleyin. Hedefi şirket içi ortam olan trafik BGP ile öğrenilen yollar kullanılarak zorla VPN/ExpressRoute ağ geçidinden geçirilir.

```azurepowershell
#Create a route table
$routeTableHubSpoke = New-AzureRmRouteTable `
  -Name 'UDR-Hub-Spoke' `
  -ResourceGroupName $RG1 `
  -location $Location1

#Create a route
Get-AzureRmRouteTable `
  -ResourceGroupName $RG1 `
  -Name UDR-Hub-Spoke `
  | Add-AzureRmRouteConfig `
  -Name "ToSpoke" `
  -AddressPrefix $VNetSpokePrefix `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress $AzfwPrivateIP `
 | Set-AzureRmRouteTable

#Associate the route table to the subnet

Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VNetHub `
  -Name $SNnameGW `
  -AddressPrefix $SNGWHubPrefix `
  -RouteTable $routeTableHubSpoke | `
Set-AzureRmVirtualNetwork

#Now create the default route

#Create a table, with BGP route propagation disabled
$routeTableSpokeDG = New-AzureRmRouteTable `
  -Name 'UDR-DG' `
  -ResourceGroupName $RG1 `
  -location $Location1 `
  -DisableBgpRoutePropagation

#Create a route
Get-AzureRmRouteTable `
  -ResourceGroupName $RG1 `
  -Name UDR-DG `
  | Add-AzureRmRouteConfig `
  -Name "ToSpoke" `
  -AddressPrefix 0.0.0.0/0 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress $AzfwPrivateIP `
 | Set-AzureRmRouteTable

#Associate the route table to the subnet

Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VNetSpoke `
  -Name $SNnameSpoke `
  -AddressPrefix $SNSpokePrefix `
  -RouteTable $routeTableSpokeDG | `
Set-AzureRmVirtualNetwork
```
## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Şimdi uç iş yükü ve OnPrem sanal makinelerini oluşturup bunları uygun alt ağlara yerleştirin.

### <a name="create-the-workload-virtual-machine"></a>İş yükü sanal makinesi oluşturma
Uç sanal ağda IIS çalıştıran, genel IP adresi olmayan ve içeri ping işlemlerine izin veren bir sanal makine oluşturun.
İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.

```azurepowershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name Allow-RDP  -Protocol Tcp `
  -Direction Inbound -Priority 200 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix $SNSpokePrefix -DestinationPortRange 3389 -Access Allow
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name Allow-web  -Protocol Tcp `
  -Direction Inbound -Priority 202 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix $SNSpokePrefix -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $RG1 -Location $Location1 -Name NSG-Spoke02 -SecurityRules $nsgRuleRDP,$nsgRuleWeb

#Create the NIC
$NIC = New-AzureRmNetworkInterface -Name spoke-01 -ResourceGroupName $RG1 -Location $Location1 -SubnetId $VnetSpoke.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id

#Define the virtual machine
$VirtualMachine = New-AzureRmVMConfig -VMName VM-Spoke-01 -VMSize "Standard_DS2"
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName Spoke-01 -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2016-Datacenter' -Version latest

#Create the virtual machine
New-AzureRmVM -ResourceGroupName $RG1 -Location $Location1 -VM $VirtualMachine -Verbose

#Install IIS on the VM
Set-AzureRmVMExtension `
    -ResourceGroupName $RG1 `
    -ExtensionName IIS `
    -VMName VM-Spoke-01 `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
    -Location $Location1
```

<!---#Create a host firewall rule to allow ping in
Set-AzureRmVMExtension `
    -ResourceGroupName $RG1 `
    -ExtensionName IIS `
    -VMName VM-Spoke-01 `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4"}' `
    -Location $Location1--->


### <a name="create-the-onprem-virtual-machine"></a>OnPrem sanal makinesi oluşturma
Bu, Uzak Masaüstü kullanarak genel IP adresine bağlanmak için kullanılabilecek basit bir sanal makinedir. Buradan, güvenlik duvarı aracılığıyla OnPrem sunucusuna bağlanabilirsiniz. İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.
```azurepowershell
New-AzureRmVm `
    -ResourceGroupName $RG1 `
    -Name "VM-Onprem" `
    -Location $Location1 `
    -VirtualNetworkName $VNetnameOnprem `
    -SubnetName $SNNameOnprem `
    -OpenPorts 3389 `
    -Size "Standard_DS2"
```

## <a name="test-the-firewall"></a>Güvenlik duvarını test etme
İlk olarak, **VM-spoke-01** sanal makinesi için özel IP adresini alın ve not edin.

```azurepowershell
$NIC.IpConfigurations.privateipaddress
```

1. Azure portalından, **VM-Onprem** sanal makinesine bağlanın.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
2. **VM-Onprem** sanal makinesinde web tarayıcısını açın ve http://\<VM-spoke-01 private IP\> adresine göz atın

   Internet Information Services varsayılan sayfasını görmelisiniz.

3. **VM-Onprem** sanal makinesinden özel IP adresinde **VM-spoke-01**'e uzak masaüstü açın.

   Bağlantınız başarılı olmalı ve seçtiğiniz kullanıcı adıyla parolayı kullanarak oturum açabilmelisiniz.

Güvenlik duvarı kurallarının çalıştığını doğruladığınıza göre:

<!---- You can ping the server on the spoke VNet.--->
- Uç sanal ağda web sunucusuna göz atabilirsiniz.
- RDP kullanarak uç sanal ağda sunucuya bağlanabilirsiniz.

Ardından, güvenlik duvarı kurallarının beklendiği gibi çalıştığını doğrulamak için güvenlik duvarı ağ kuralı koleksiyonu eylemini **Reddet** olarak değiştirin. Kural koleksiyonları eylemini **Reddet** olarak değiştirmek için aşağıdaki betiği çalıştırın.

```azurepowershell
$rcNet = $azfw.GetNetworkRuleCollectionByName("RCNet01")
$rcNet.action.type = "Deny"

$rcApp = $azfw.GetApplicationRuleCollectionByName("RCApp01")
$rcApp.action.type = "Deny"

Set-AzureRmFirewall -AzureFirewall $azfw
```
Şimdi testleri yeniden çalıştırın. Bu kez tümü başarısız olmalıdır. Değişen kuralları test etmeden önce var olan tüm uzak masaüstlerini kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **FW-Hybrid-Test** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Ağ ortamını oluşturma
> * Güvenlik duvarını yapılandırma ve dağıtma
> * Yolları oluşturma
> * Sanal makineleri oluşturma
> * Güvenlik duvarını test etme

Şimdi Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
