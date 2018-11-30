---
title: "Öğretici: Azure PowerShell kullanarak hibrit bir ağda Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma"
description: Bu öğreticide Azure portalı kullanarak Azure Güvenlik Duvarı'nı dağıtmayı ve yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 10/27/2018
ms.author: victorh
ms.openlocfilehash: d69bd055c95592961216f5da1efaedc4a642fd63
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316408"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-in-a-hybrid-network-using-azure-powershell"></a>Öğretici: Azure PowerShell kullanarak hibrit bir ağda Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma

Şirket içi ağınızı bir hibrit ağ oluşturmak için bir Azure sanal ağına bağlama, Azure ağ kaynaklarınıza erişimi denetleme olanağı genel bir güvenlik planı önemli bir parçasıdır.

İzin verilen ve reddedilen ağ trafiğini tanımlayan kuralları kullanarak hibrit ağ içindeki ağ erişimi denetlemek için Azure Güvenlik Duvarı'nı kullanabilirsiniz.

Bu öğreticide, üç sanal ağlar oluşturun:

- **VNet-Hub** -bu sanal ağda güvenlik duvarıdır.
- **VNet-uç** -uç sanal ağ, Azure üzerinde bulunan iş yükünü temsil eder.
- **VNet-Onprem** -şirket içi sanal ağı şirket içi ağı temsil eder. Gerçek bir dağıtımda, VPN veya Express Route bağlantısıyla bağlanabilir. Kolaylık olması için Bu öğretici, bir VPN gateway bağlantısı kullanır ve Azure bulunan bir sanal ağ ile şirket içi ağ temsil etmek için kullanılır.

![Hibrit ağda güvenlik duvarı](media/tutorial-hybrid-ps/hybrid-network-firewall.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Değişkenleri tanımlama
> * Güvenlik Duvarı hub sanal ağ oluşturma
> * Uç sanal ağ oluşturma
> * Şirket içi sanal ağ oluşturma
> * Güvenlik duvarını yapılandırma ve dağıtma
> * VPN ağ geçitlerini oluşturma ve bağlama
> * Eş merkez ve uç sanal ağları
> * Yolları oluşturma
> * Sanal makineleri oluşturma
> * Güvenlik duvarını test etme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide PowerShell'i yerel olarak çalıştırmanızı gerektirir. Sonraki bir sürümünün yüklü veya Azure PowerShell modülü sürüm 6.12.0 olmalıdır. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzureRmAccount` komutunu çalıştırın.

Bu senaryonun doğru çalışması için üç önemli gereksinimi vardır:

- Uç alt ağda varsayılan ağ geçidi olarak Azure Güvenlik Duvarı IP adresine işaret eden Kullanıcı Tanımlı Yol. Bu yol tablosunda BGP yol yaymanın **Devre Dışı** olması gerekir.
- Hub ağ geçidi alt ağındaki Kullanıcı Tanımlı Yol, uç ağlara yönelik sonraki atlama olarak güvenlik duvarı IP adresine işaret etmelidir.
- Azure Güvenlik Duvarı alt ağında Kullanıcı Tanımlı Yol gerekmez çünkü bu alt ağ yolları BGP'den öğrenir.
- VNet-Hub'ı VNet-Spoke'a eşlerken **AllowGatewayTransit** ayarladığınızdan ve VNet-Spoke'u VNet-Hub'a eşlerken de **UseRemoteGateways** ayarladığınızdan emin olun.

Bu yolların nasıl oluşturulduğunu görmek için [Yolları Oluşturma](#create-routes) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="declare-the-variables"></a>Değişkenleri tanımlama

Aşağıdaki örnekte bu öğreticiye yönelik değerleri kullanan değişkenler bildirilmektedir. Bazı durumlarda, bazı değerler, aboneliğinize çalışacak şekilde kendi değerlerinizle değiştirmeniz gerekebilir. Gerekirse değişkenleri değiştirin, daha sonra kopyalayın ve PowerShell konsolunuza yapıştırın.

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

# Variables for the spoke virtual network

$VnetNameSpoke = "VNet-Spoke"
$SNnameSpoke = "SN-Workload"
$VNetSpokePrefix = "10.6.0.0/16"
$SNSpokePrefix = "10.6.0.0/24"
$SNSpokeGWPrefix = "10.6.1.0/24"

# Variables for the on-premises virtual network

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


## <a name="create-the-firewall-hub-virtual-network"></a>Güvenlik Duvarı hub sanal ağ oluşturma

İlk olarak, Bu öğretici için kaynakları içeren kaynak grubunu oluşturun:

```azurepowershell
  New-AzureRmResourceGroup -Name $RG1 -Location $Location1
  ```

Alt ağlar sanal ağ içinde dahil edilecek tanımlayın:

```azurepowershell
$FWsub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameHub -AddressPrefix $SNHubPrefix
$GWsub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNGWHubPrefix
```

Şimdi, güvenlik duvarı hub sanal ağı oluşturun:

```azurepowershell
$VNetHub = New-AzureRmVirtualNetwork -Name $VNetnameHub -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetHubPrefix -Subnet $FWsub,$GWsub
```

Sanal ağınız için oluşturacağınız VPN ağ geçidine ayrılacak genel IP adresi isteyin. *AllocationMethod* değerinin **Dynamic** olduğuna dikkat edin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, VPN ağ geçidinize dinamik olarak ayrılır. 

  ```azurepowershell
  $gwpip1 = New-AzureRmPublicIpAddress -Name $GWHubpipName -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
```

## <a name="create-the-spoke-virtual-network"></a>Uç sanal ağ oluşturma

Uç sanal ağ dahil edilecek alt ağları tanımlayın:

```azurepowershell
$Spokesub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameSpoke -AddressPrefix $SNSpokePrefix
$GWsubSpoke = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNSpokeGWPrefix
```

Uç sanal ağ oluşturun:

```azurepowershell
$VNetSpoke = New-AzureRmVirtualNetwork -Name $VnetNameSpoke -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetSpokePrefix -Subnet $Spokesub,$GWsubSpoke
```

## <a name="create-the-on-premises-virtual-network"></a>Şirket içi sanal ağ oluşturma

Alt ağlar sanal ağ içinde dahil edilecek tanımlayın:

```azurepowershell
$Onpremsub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNNameOnprem -AddressPrefix $SNOnpremPrefix
$GWOnpremsub = New-AzureRmVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNGWOnpremPrefix
```

Artık, şirket içi sanal ağ oluşturun:

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

Şimdi merkez sanal ağa Güvenlik Duvarı'nı dağıtın.

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

<!--- $Rule3 = New-AzureRmFirewallNetworkRule -Name "AllowPing" -Protocol ICMP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort *--->

```azurepowershell
$Rule1 = New-AzureRmFirewallNetworkRule -Name "AllowWeb" -Protocol TCP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort 80

$Rule2 = New-AzureRmFirewallNetworkRule -Name "AllowRDP" -Protocol TCP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort 3389

$NetRuleCollection = New-AzureRmFirewallNetworkRuleCollection -Name RCNet01 -Priority 100 `
   -Rule $Rule1,$Rule2 -ActionType "Allow"
$Azfw.NetworkRuleCollections = $NetRuleCollection
Set-AzureRmFirewall -AzureFirewall $Azfw
```

## <a name="create-and-connect-the-vpn-gateways"></a>VPN ağ geçitlerini oluşturma ve bağlama

Hub ve şirket içi sanal ağlara VPN ağ geçitleri bağlanır.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Merkez sanal ağ için VPN gateway oluşturma

VPN ağ geçidi yapılandırmasını oluşturun. VPN ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar.

  ```azurepowershell
  $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetnameHub -ResourceGroupName $RG1
  $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfNameHub `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```

Şimdi hub sanal ağ için VPN ağ geçidi oluşturun. Ağ ve ağ yapılandırmaları, RouteBased bir VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

```azurepowershell
New-AzureRmVirtualNetworkGateway -Name $GWHubName -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku basic
```

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Şirket içi sanal ağ için VPN gateway oluşturma

VPN ağ geçidi yapılandırmasını oluşturun. VPN ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar.

  ```azurepowershell
$vnet2 = Get-AzureRmVirtualNetwork -Name $VNetnameOnprem -ResourceGroupName $RG1
$subnet2 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfNameOnprem `
  -Subnet $subnet2 -PublicIpAddress $gwOnprempip
  ```

Şimdi şirket içi sanal ağ için VPN ağ geçidi oluşturun. Ağ ve ağ yapılandırmaları, RouteBased bir VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

```azurepowershell
New-AzureRmVirtualNetworkGateway -Name $GWOnpremName -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gwipconf2 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku basic
```

### <a name="create-the-vpn-connections"></a>VPN bağlantılarını oluşturma

Hub ve şirket içi ağ geçitleri arasındaki VPN bağlantıları artık oluşturabilirsiniz

#### <a name="get-the-vpn-gateways"></a>VPN ağ geçitlerini alma

```azurepowershell
$vnetHubgw = Get-AzureRmVirtualNetworkGateway -Name $GWHubName -ResourceGroupName $RG1
$vnetOnpremgw = Get-AzureRmVirtualNetworkGateway -Name $GWOnpremName -ResourceGroupName $RG1
```

#### <a name="create-the-connections"></a>Bağlantıları oluşturma

Bu adımda, bağlantı şirket içi sanal ağa hub sanal ağı oluşturun. Örneklerde paylaşılan bir anahtar göreceksiniz. Paylaşılan anahtar için kendi değerlerinizi kullanabilirsiniz. Paylaşılan anahtarın her iki bağlantıyla da eşleşiyor olması önemlidir. Bir bağlantı oluşturmak çok zaman almaz.

```azurepowershell
New-AzureRmVirtualNetworkGatewayConnection -Name $ConnectionNameHub -ResourceGroupName $RG1 `
-VirtualNetworkGateway1 $vnetHubgw -VirtualNetworkGateway2 $vnetOnpremgw -Location $Location1 `
-ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
```
Şirket içi hub sanal ağ bağlantısı oluşturun. VNet-hub'ına VNet-Onprem gelen bağlantı oluşturma dışında bu Öncekine, benzer bir adımdır. Paylaşılan anahtarların eşleştiğinden emin olun. Bağlantı birkaç dakika içerisinde kurulacaktır.

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

## <a name="peer-the-hub-and-spoke-virtual-networks"></a>Eş merkez ve uç sanal ağları

Artık eş hub ve uç sanal ağları.

```azurepowershell
# Peer hub to spoke
Add-AzureRmVirtualNetworkPeering -Name HubtoSpoke -VirtualNetwork $VNetHub -RemoteVirtualNetworkId $VNetSpoke.Id -AllowGatewayTransit

# Peer spoke to hub
Add-AzureRmVirtualNetworkPeering -Name SpoketoHub -VirtualNetwork $VNetSpoke -RemoteVirtualNetworkId $VNetHub.Id -AllowForwardedTraffic -UseRemoteGateways
```

## <a name="create-the-routes"></a>Yolları oluşturma

Şimdi birkaç yol oluşturun:

- Güvenlik duvarı IP adresi üzerinden hub ağ geçidi alt ağından uç alt ağına giden bir yol
- Güvenlik duvarı IP adresi üzerinden uç alt ağından gelen varsayılan yol

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

Artık uç iş yükü ve şirket içi sanal makineleri oluşturun ve bunları uygun alt ağlarına yerleştirin.

### <a name="create-the-workload-virtual-machine"></a>İş yükü sanal makinesi oluşturma

Uç sanal ağdaki hiçbir genel IP adresi ile IIS çalıştıran bir sanal makine oluşturun ve ping de sağlar.
İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.

```azurepowershell
# Create an inbound network security group rule for ports 3389 and 80
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

### <a name="create-the-on-premises-virtual-machine"></a>Şirket içi sanal makine oluşturma

Genel IP adresi için Uzak Masaüstü kullanarak bağlanmak için kullandığınız basit bir sanal makine budur. Buradan sonra şirket içi sunucunun güvenlik duvarı üzerinden bağlanır. İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.

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

İlk olarak, alma ve özel IP adresini not edin **VM-uç-01** sanal makine.

```azurepowershell
$NIC.IpConfigurations.privateipaddress
```

Azure portalından, **VM-Onprem** sanal makinesine bağlanın.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
Bir web tarayıcısı açmak **VM-Onprem**ve http:// Gözat\<VM-uç-01 özel IP\>.

Internet Information Services varsayılan sayfasını görmelisiniz.

**VM-Onprem** sanal makinesinden özel IP adresinde **VM-spoke-01**'e uzak masaüstü açın.

Bağlantınız başarılı olmalı ve seçtiğiniz kullanıcı adıyla parolayı kullanarak oturum açabilmelisiniz.

Güvenlik duvarı kurallarının çalıştığını doğruladığınıza göre:

<!---- You can ping the server on the spoke VNet.--->
- Web sunucusu uç sanal ağ üzerinde göz atabilirsiniz.
- Sunucu üzerinde RDP kullanarak uç sanal ağa bağlanabilir.

Ardından, güvenlik duvarı kurallarının beklendiği gibi çalıştığını doğrulamak için güvenlik duvarı ağ kuralı koleksiyonu eylemini **Reddet** olarak değiştirin. Kural koleksiyonu eylemini **Reddet** olarak değiştirmek için aşağıdaki betiği çalıştırın.

```azurepowershell
$rcNet = $azfw.GetNetworkRuleCollectionByName("RCNet01")
$rcNet.action.type = "Deny"

Set-AzureRmFirewall -AzureFirewall $azfw
```

Şimdi testleri yeniden çalıştırın. Bu kez tümü başarısız olmalıdır. Değişen kuralları test etmeden önce var olan tüm uzak masaüstlerini kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **FW-Hybrid-Test** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: Azure Güvenlik Duvarı günlüklerini izleme](./tutorial-diagnostics.md)
