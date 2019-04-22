---
title: "Öğretici: Azure PowerShell kullanarak hibrit bir ağda Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma"
description: Bu öğreticide, dağıtma ve Azure Azure PowerShell kullanarak güvenlik duvarı yapılandırma hakkında bilgi edinin.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 3/18/2019
ms.author: victorh
customer intent: As an administrator, I want to control network access from an on-premises network to an Azure virtual network.
ms.openlocfilehash: 7beb3d986b016688c4ee0a512b9406dbf3dfbb40
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59051708"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-in-a-hybrid-network-using-azure-powershell"></a>Öğretici: Azure PowerShell kullanarak hibrit bir ağda Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma

Şirket içi ağınızı bir hibrit ağ oluşturmak için bir Azure sanal ağına bağlama, Azure ağ kaynaklarınıza erişimi denetleme olanağı genel bir güvenlik planı önemli bir parçasıdır.

İzin verilen ve reddedilen ağ trafiğini tanımlayan kuralları kullanarak hibrit ağ içindeki ağ erişimi denetlemek için Azure Güvenlik Duvarı'nı kullanabilirsiniz.

Bu öğreticide, üç sanal ağlar oluşturun:

- **VNet-Hub** -bu sanal ağda güvenlik duvarıdır.
- **VNet-uç** -uç sanal ağ, Azure üzerinde bulunan iş yükünü temsil eder.
- **VNet-Onprem** -şirket içi sanal ağı şirket içi ağı temsil eder. Gerçek bir dağıtımda bulunan bir VPN ya da ExpressRoute bağlantısı yoluyla bağlanabilir. Kolaylık olması için Bu öğretici, bir VPN gateway bağlantısı kullanır ve Azure bulunan bir sanal ağ ile şirket içi ağ temsil etmek için kullanılır.

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


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide PowerShell'i yerel olarak çalıştırmanızı gerektirir. Azure PowerShell Modülü yüklü olmalıdır. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps). PowerShell sürümünü doğruladıktan sonra, Azure ile bağlantı oluşturmak için `Login-AzAccount` komutunu çalıştırın.

Bu senaryonun doğru çalışması için üç önemli gereksinimi vardır:

- Bir kullanıcı tanımlı yol (UDR) varsayılan ağ geçidi Azure güvenlik duvarı IP adresine işaret eden uç alt ağında. Bu yol tablosunda BGP yol yaymanın **Devre Dışı** olması gerekir.
- Hub ağ geçidi alt ağı üzerinde bir UDR bileşen ağlarını için sonraki atlama olarak güvenlik duvarı IP adresine işaret etmelidir.

   BGP yolları öğrenir gibi hiçbir UDR Azure güvenlik duvarı alt ağda gereklidir.
- VNet-Hub'ı VNet-Spoke'a eşlerken **AllowGatewayTransit** ayarladığınızdan ve VNet-Spoke'u VNet-Hub'a eşlerken de **UseRemoteGateways** ayarladığınızdan emin olun.

Bu yolların nasıl oluşturulduğunu görmek için [Yolları Oluşturma](#create-the-routes) bölümüne bakın.

>[!NOTE]
>Azure güvenlik duvarı, doğrudan internet bağlantısı olması gerekir. Varsayılan olarak, bir UDR 0.0.0.0/0 ile yalnızca AzureFirewallSubnet sağlamalıdır **NextHopType** değer kümesini olarak **Internet**.
>
>Şirket içi ExpressRoute veya uygulama ağ geçidi aracılığıyla zorlamalı tünel etkinleştirirseniz, açıkça NextHopType değeri olarak ayarlanmış bir UDR 0.0.0.0/0 yapılandırmanız gerekebilir **Internet** , AzureFirewallSubnet ile ilişkilendirin. Kuruluşunuz için Azure güvenlik duvarı trafiğine zorlamalı tünel gerektiriyorsa, beyaz liste aboneliğiniz olabilir ve gerekli güvenlik duvarı Internet bağlantısı korunduğundan emin olmak için lütfen desteğe başvurun.

>[!NOTE]
>Azure Güvenlik Duvarı varsayılan ağ geçidi için bir UDR işaret ediyor olsa bile doğrudan eşlenmiş sanal ağlar arasındaki trafiği doğrudan yönlendirilir. Bu senaryoda bir güvenlik duvarı alt ağ için alt ağ trafiği göndermek için bir UDR her iki alt ağ üzerinde açıkça hedef alt ağ ön eki içermesi gerekir.

İlgili Azure PowerShell başvuru belgeleri gözden geçirmek için bkz: [Azure PowerShell başvurusu](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall).

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
  New-AzResourceGroup -Name $RG1 -Location $Location1
  ```

Alt ağlar sanal ağ içinde dahil edilecek tanımlayın:

```azurepowershell
$FWsub = New-AzVirtualNetworkSubnetConfig -Name $SNnameHub -AddressPrefix $SNHubPrefix
$GWsub = New-AzVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNGWHubPrefix
```

Şimdi, güvenlik duvarı hub sanal ağı oluşturun:

```azurepowershell
$VNetHub = New-AzVirtualNetwork -Name $VNetnameHub -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetHubPrefix -Subnet $FWsub,$GWsub
```

Sanal ağınız için oluşturacağınız VPN ağ geçidine ayrılacak genel IP adresi isteyin. *AllocationMethod* değerinin **Dynamic** olduğuna dikkat edin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, VPN ağ geçidinize dinamik olarak ayrılır. 

  ```azurepowershell
  $gwpip1 = New-AzPublicIpAddress -Name $GWHubpipName -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
```

## <a name="create-the-spoke-virtual-network"></a>Uç sanal ağ oluşturma

Uç sanal ağ dahil edilecek alt ağları tanımlayın:

```azurepowershell
$Spokesub = New-AzVirtualNetworkSubnetConfig -Name $SNnameSpoke -AddressPrefix $SNSpokePrefix
$GWsubSpoke = New-AzVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNSpokeGWPrefix
```

Uç sanal ağ oluşturun:

```azurepowershell
$VNetSpoke = New-AzVirtualNetwork -Name $VnetNameSpoke -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetSpokePrefix -Subnet $Spokesub,$GWsubSpoke
```

## <a name="create-the-on-premises-virtual-network"></a>Şirket içi sanal ağ oluşturma

Alt ağlar sanal ağ içinde dahil edilecek tanımlayın:

```azurepowershell
$Onpremsub = New-AzVirtualNetworkSubnetConfig -Name $SNNameOnprem -AddressPrefix $SNOnpremPrefix
$GWOnpremsub = New-AzVirtualNetworkSubnetConfig -Name $SNnameGW -AddressPrefix $SNGWOnpremPrefix
```

Artık, şirket içi sanal ağ oluşturun:

```azurepowershell
$VNetOnprem = New-AzVirtualNetwork -Name $VNetnameOnprem -ResourceGroupName $RG1 `
-Location $Location1 -AddressPrefix $VNetOnpremPrefix -Subnet $Onpremsub,$GWOnpremsub
```

Sanal ağ için oluşturacağınız ağ geçidine ayrılacak genel IP adresi isteyin. *AllocationMethod* değerinin **Dynamic** olduğuna dikkat edin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, ağ geçidinize dinamik olarak ayrılır. 

  ```azurepowershell
  $gwOnprempip = New-AzPublicIpAddress -Name $GWOnprempipName -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Dynamic
```

## <a name="configure-and-deploy-the-firewall"></a>Güvenlik duvarını yapılandırma ve dağıtma

Şimdi merkez sanal ağa Güvenlik Duvarı'nı dağıtın.

```azurepowershell
# Get a Public IP for the firewall
$FWpip = New-AzPublicIpAddress -Name "fw-pip" -ResourceGroupName $RG1 `
  -Location $Location1 -AllocationMethod Static -Sku Standard
# Create the firewall
$Azfw = New-AzFirewall -Name AzFW01 -ResourceGroupName $RG1 -Location $Location1 -VirtualNetworkName $VNetnameHub -PublicIpName fw-pip

#Save the firewall private IP address for future use

$AzfwPrivateIP = $Azfw.IpConfigurations.privateipaddress
$AzfwPrivateIP

```

### <a name="configure-network-rules"></a>Ağ kurallarını yapılandırma

<!--- $Rule3 = New-AzFirewallNetworkRule -Name "AllowPing" -Protocol ICMP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort *--->

```azurepowershell
$Rule1 = New-AzFirewallNetworkRule -Name "AllowWeb" -Protocol TCP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort 80

$Rule2 = New-AzFirewallNetworkRule -Name "AllowRDP" -Protocol TCP -SourceAddress $SNOnpremPrefix `
   -DestinationAddress $VNetSpokePrefix -DestinationPort 3389

$NetRuleCollection = New-AzFirewallNetworkRuleCollection -Name RCNet01 -Priority 100 `
   -Rule $Rule1,$Rule2 -ActionType "Allow"
$Azfw.NetworkRuleCollections = $NetRuleCollection
Set-AzFirewall -AzureFirewall $Azfw
```

## <a name="create-and-connect-the-vpn-gateways"></a>VPN ağ geçitlerini oluşturma ve bağlama

Hub ve şirket içi sanal ağlara VPN ağ geçitleri bağlanır.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Merkez sanal ağ için VPN gateway oluşturma

VPN ağ geçidi yapılandırmasını oluşturun. VPN ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar.

  ```azurepowershell
  $vnet1 = Get-AzVirtualNetwork -Name $VNetnameHub -ResourceGroupName $RG1
  $subnet1 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
  $gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfNameHub `
  -Subnet $subnet1 -PublicIpAddress $gwpip1
  ```

Şimdi hub sanal ağ için VPN ağ geçidi oluşturun. Ağ ve ağ yapılandırmaları, RouteBased bir VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

```azurepowershell
New-AzVirtualNetworkGateway -Name $GWHubName -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku basic
```

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Şirket içi sanal ağ için VPN gateway oluşturma

VPN ağ geçidi yapılandırmasını oluşturun. VPN ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar.

  ```azurepowershell
$vnet2 = Get-AzVirtualNetwork -Name $VNetnameOnprem -ResourceGroupName $RG1
$subnet2 = Get-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gwipconf2 = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfNameOnprem `
  -Subnet $subnet2 -PublicIpAddress $gwOnprempip
  ```

Şimdi şirket içi sanal ağ için VPN ağ geçidi oluşturun. Ağ ve ağ yapılandırmaları, RouteBased bir VpnType gerektirir. VPN ağ geçidinin oluşturulması, seçili VPN ağ geçidi SKU’suna bağlı olarak 45 dakika veya daha uzun sürebilir.

```azurepowershell
New-AzVirtualNetworkGateway -Name $GWOnpremName -ResourceGroupName $RG1 `
-Location $Location1 -IpConfigurations $gwipconf2 -GatewayType Vpn `
-VpnType RouteBased -GatewaySku basic
```

### <a name="create-the-vpn-connections"></a>VPN bağlantılarını oluşturma

Hub ve şirket içi ağ geçitleri arasındaki VPN bağlantıları artık oluşturabilirsiniz

#### <a name="get-the-vpn-gateways"></a>VPN ağ geçitlerini alma

```azurepowershell
$vnetHubgw = Get-AzVirtualNetworkGateway -Name $GWHubName -ResourceGroupName $RG1
$vnetOnpremgw = Get-AzVirtualNetworkGateway -Name $GWOnpremName -ResourceGroupName $RG1
```

#### <a name="create-the-connections"></a>Bağlantıları oluşturma

Bu adımda, bağlantı şirket içi sanal ağa hub sanal ağı oluşturun. Örneklerde paylaşılan bir anahtar göreceksiniz. Paylaşılan anahtar için kendi değerlerinizi kullanabilirsiniz. Paylaşılan anahtarın her iki bağlantıyla da eşleşiyor olması önemlidir. Bir bağlantı oluşturmak çok zaman almaz.

```azurepowershell
New-AzVirtualNetworkGatewayConnection -Name $ConnectionNameHub -ResourceGroupName $RG1 `
-VirtualNetworkGateway1 $vnetHubgw -VirtualNetworkGateway2 $vnetOnpremgw -Location $Location1 `
-ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
```
Şirket içi hub sanal ağ bağlantısı oluşturun. VNet-hub'ına VNet-Onprem gelen bağlantı oluşturma dışında bu Öncekine, benzer bir adımdır. Paylaşılan anahtarların eşleştiğinden emin olun. Bağlantı birkaç dakika içerisinde kurulacaktır.

  ```azurepowershell
  New-AzVirtualNetworkGatewayConnection -Name $ConnectionNameOnprem -ResourceGroupName $RG1 `
  -VirtualNetworkGateway1 $vnetOnpremgw -VirtualNetworkGateway2 $vnetHubgw -Location $Location1 `
  -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'
  ```

#### <a name="verify-the-connection"></a>Bağlantıyı doğrulama

Başarılı bir bağlantı kullanarak doğrulayabilirsiniz *Get-AzVirtualNetworkGatewayConnection* cmdlet'i ile bağlantınızın *-hata ayıklama*. Aşağıdaki cmdlet örneğini kullanın ve değerleri, kendi değerlerinizle eşleşecek şekilde yapılandırın. İstendiğinde, **Tümü**'nü çalıştırmak için **A** seçin. Örnekte bulunan *-Name*, test etmek istediğiniz bağlantının adıdır.

```azurepowershell
Get-AzVirtualNetworkGatewayConnection -Name $ConnectionNameHub -ResourceGroupName $RG1
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
Add-AzVirtualNetworkPeering -Name HubtoSpoke -VirtualNetwork $VNetHub -RemoteVirtualNetworkId $VNetSpoke.Id -AllowGatewayTransit

# Peer spoke to hub
Add-AzVirtualNetworkPeering -Name SpoketoHub -VirtualNetwork $VNetSpoke -RemoteVirtualNetworkId $VNetHub.Id -AllowForwardedTraffic -UseRemoteGateways
```

## <a name="create-the-routes"></a>Yolları oluşturma

Şimdi birkaç yol oluşturun:

- Güvenlik duvarı IP adresi üzerinden hub ağ geçidi alt ağından uç alt ağına giden bir yol
- Güvenlik duvarı IP adresi üzerinden uç alt ağından gelen varsayılan yol

```azurepowershell
#Create a route table
$routeTableHubSpoke = New-AzRouteTable `
  -Name 'UDR-Hub-Spoke' `
  -ResourceGroupName $RG1 `
  -location $Location1

#Create a route
Get-AzRouteTable `
  -ResourceGroupName $RG1 `
  -Name UDR-Hub-Spoke `
  | Add-AzRouteConfig `
  -Name "ToSpoke" `
  -AddressPrefix $VNetSpokePrefix `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress $AzfwPrivateIP `
 | Set-AzRouteTable

#Associate the route table to the subnet

Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $VNetHub `
  -Name $SNnameGW `
  -AddressPrefix $SNGWHubPrefix `
  -RouteTable $routeTableHubSpoke | `
Set-AzVirtualNetwork

#Now create the default route

#Create a table, with BGP route propagation disabled
$routeTableSpokeDG = New-AzRouteTable `
  -Name 'UDR-DG' `
  -ResourceGroupName $RG1 `
  -location $Location1 `
  -DisableBgpRoutePropagation

#Create a route
Get-AzRouteTable `
  -ResourceGroupName $RG1 `
  -Name UDR-DG `
  | Add-AzRouteConfig `
  -Name "ToSpoke" `
  -AddressPrefix 0.0.0.0/0 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress $AzfwPrivateIP `
 | Set-AzRouteTable

#Associate the route table to the subnet

Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $VNetSpoke `
  -Name $SNnameSpoke `
  -AddressPrefix $SNSpokePrefix `
  -RouteTable $routeTableSpokeDG | `
Set-AzVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Artık uç iş yükü ve şirket içi sanal makineleri oluşturun ve bunları uygun alt ağlarına yerleştirin.

### <a name="create-the-workload-virtual-machine"></a>İş yükü sanal makinesi oluşturma

Uç sanal ağdaki hiçbir genel IP adresi ile IIS çalıştıran bir sanal makine oluşturun ve ping de sağlar.
İstendiğinde, sanal makine için bir kullanıcı adı ve parola yazın.

```azurepowershell
# Create an inbound network security group rule for ports 3389 and 80
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name Allow-RDP  -Protocol Tcp `
  -Direction Inbound -Priority 200 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix $SNSpokePrefix -DestinationPortRange 3389 -Access Allow
$nsgRuleWeb = New-AzNetworkSecurityRuleConfig -Name Allow-web  -Protocol Tcp `
  -Direction Inbound -Priority 202 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix $SNSpokePrefix -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $RG1 -Location $Location1 -Name NSG-Spoke02 -SecurityRules $nsgRuleRDP,$nsgRuleWeb

#Create the NIC
$NIC = New-AzNetworkInterface -Name spoke-01 -ResourceGroupName $RG1 -Location $Location1 -SubnetId $VnetSpoke.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id

#Define the virtual machine
$VirtualMachine = New-AzVMConfig -VMName VM-Spoke-01 -VMSize "Standard_DS2"
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName Spoke-01 -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2016-Datacenter' -Version latest

#Create the virtual machine
New-AzVM -ResourceGroupName $RG1 -Location $Location1 -VM $VirtualMachine -Verbose

#Install IIS on the VM
Set-AzVMExtension `
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
Set-AzVMExtension `
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
New-AzVm `
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

Set-AzFirewall -AzureFirewall $azfw
```

Şimdi testleri yeniden çalıştırın. Bu kez tümü başarısız olmalıdır. Değişen kuralları test etmeden önce var olan tüm uzak masaüstlerini kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Güvenlik duvarı kaynaklarını bir sonraki öğretici için tutabilirsiniz veya artık gerekli değilse **FW-Hybrid-Test** kaynak grubunu silerek güvenlik duvarıyla ilgili tüm kaynakları silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Azure Güvenlik Duvarı günlüklerini izleyebilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: Azure güvenlik duvarı günlüklerini izleyin](./tutorial-diagnostics.md)
