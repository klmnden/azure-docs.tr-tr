---
title: "Şirket içi ağınızı bir Azure sanal ağına bağlama: Siteden Siteye VPN: PowerShell | Microsoft Docs"
description: "Şirket içi ağınız ile bir Azure sanal ağı arasında genel İnternet üzerinden bir IPSec bağlantısı oluşturma adımları. Bu adımlar PowerShell kullanarak Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı oluşturmanıza yardımcı olur."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fcc2fda5-4493-4c15-9436-84d35adbda8e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/01/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: f485dc6a52488b44bbd0e68432d3fd2bcdb060a9
ms.contentlocale: tr-tr
ms.lasthandoff: 05/02/2017


---
# <a name="create-a-vnet-with-a-site-to-site-vpn-connection-using-powershell"></a>PowerShell kullanarak Siteden Siteye VPN bağlantısı ile sanal ağ oluşturma

Bu makalede, PowerShell kullanarak şirket içi ağınızdan VNet’e Siteden Siteye VPN ağ geçidi bağlantısı oluşturma işlemi gösterilir. Bu makaledeki adımlar Resource Manager dağıtım modeli için geçerlidir. Ayrıca aşağıdaki listeden farklı bir seçenek belirtip farklı bir dağıtım aracı veya dağıtım modeli kullanarak da bu yapılandırmayı oluşturabilirsiniz:

> [!div class="op_single_selector"]
> * [Resource Manager - Azure portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager - CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Klasik - Azure portalı](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Klasik - klasik portal](vpn-gateway-site-to-site-create.md)
> 
>


![Siteden Siteye şirket içi ve dışı karışık VPN Gateway bağlantısı diyagramı](./media/vpn-gateway-create-site-to-site-rm-powershell/site-to-site-connection-diagram.png)

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN ağ geçidi hakkında](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Resource Manager dağıtım modeliyle çalışmak istediğinizi doğrulayın. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Uyumlu bir VPN cihazı ve bu cihazı yapılandırabilecek biri. Uyumlu VPN cihazları ve cihaz yapılandırması hakkında daha fazla bilgi için bkz.[VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md).
* VPN cihazınız için dışarıya yönelik genel bir IPv4 adresi. Bu IP adresi bir NAT’nin arkasında olamaz.
* Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir. Bu yapılandırmayı oluşturduğunuzda, Azure’un şirket içi konumunuza yönlendireceği IP adres aralığı ön eklerini oluşturmanız gerekir. Şirket içi ağınızın alt ağlarından hiçbiri, bağlanmak istediğiniz sanal ağ alt ağlarıyla çakışamaz.
* Azure Resource Manager PowerShell cmdlet'lerinin son sürümü. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

### <a name="example-values"></a>Örnek değerler

Bu makaledeki örneklerde aşağıdaki değerler kullanılır. Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz.

```
#Example values

VnetName                = testvnet 
ResourceGroup           = testrg 
Location                = West US 
AddressSpace            = 10.0.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.0.1.0/28 
GatewaySubnet           = 10.0.0.0/27
LocalNetworkGatewayName = LocalSite
LNG Public IP           = <VPN device IP address> 
Local Address Prefixes  = 10.0.0.0/24','20.0.0.0/24
Gateway Name            = vnetgw1
PublicIP                = gwpip
Gateway IP Config       = gwipconfig1 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = myGWConnection
```

## <a name="Login"></a>1. Aboneliğinize bağlanma

[!INCLUDE [vpn-gateway-ps-login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="VNet"></a>2. Sanal ağ ve ağ geçidi alt ağı oluşturma

Sanal ağınız yoksa bir sanal ağ oluşturun. Sanal ağ oluştururken, belirlediğiniz adres alanlarının şirket içi ağınızdaki adres alanlarından herhangi biriyle çakışmadığından emin olun. Bu yapılandırma için, bir ağ geçidi alt ağına da ihtiyacınız olacaktır. Sanal ağın ağ geçidi, VPN ağ geçidi hizmetleri tarafından kullanılan IP adreslerinin bulunduğu ağ geçidi alt ağını kullanır. Ağ geçidi alt ağını oluşturduğunuzda, bu alt ağ 'GatewaySubnet' olarak adlandırılmalıdır. Başka bir ad kullanırsanız alt ağ oluşturulur ancak Azure bunu bir ağ geçidi alt ağı olarak değerlendirmez.

Belirttiğiniz ağ geçidi alt ağının boyutu, oluşturmak istediğiniz VPN ağ geçidi yapılandırmasına bağlıdır. /29 kadar küçük bir ağ geçidi alt ağı oluşturmak mümkün olsa da, /27 veya /28’i seçerek daha fazla adres içeren büyük bir alt ağ oluşturmanızı öneririz. Daha büyük bir ağ geçidi alt ağı kullanmak, olası gelecek yapılandırmaları barındırmak için yeterli IP adresi bulunmasını sağlar.

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Sanal ağ ve ağ geçidi alt ağı oluşturmak için

Bu örnekte, sanal ağ ve ağ geçidi alt ağı oluşturulur. Ağ geçidi alt ağı eklemeniz gereken bir sanal ağınız zaten varsa, bkz. [Önceden oluşturduğunuz bir sanal ağa, bir ağ geçidi alt ağı eklemek için](#gatewaysubnet).

Kaynak grubu oluşturun:

```powershell
New-AzureRmResourceGroup -Name testrg -Location 'West US'
```

Sanal ağınızı oluşturun. 

1. Değişkenleri ayarlayın.

  ```powershell
  $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/27
  $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'
  ```
2. VNet'i oluşturun.

  ```powershell
  New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
  -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2
  ```

### <a name="gatewaysubnet"></a>Önceden oluşturduğunuz bir sanal ağa, bir ağ geçidi alt ağı eklemek için

1. Değişkenleri ayarlayın.

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet
  ```
2. Ağ geçidi alt ağını oluşturun.

  ```powershell
  Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/27 -VirtualNetwork $vnet
  ```
3. Yapılandırmayı ayarlayın.

  ```powershell
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

## 3. <a name="localnet"></a>Yerel ağ geçidi oluşturma

Yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Siteye Azure’un başvuruda bulunmak için kullanabileceği bir ad verir, ardından bağlantı oluşturacağınız şirket içi VPN cihazının IP adresini belirtirsiniz. Ayrıca, VPN ağ geçidi üzerinden VPN cihazına yönlendirilecek IP adresi ön eklerini de belirtirsiniz. Belirttiğiniz adres ön ekleri, şirket içi adresinizde yer alan ön eklerdir. Şirket içi ağınız değişirse, ön ekleri kolayca güncelleştirebilirsiniz.

Aşağıdaki değerleri kullanın:

* *GatewayIPAddress* şirket içi VPN cihazınızın IP adresidir. VPN cihazınız bir NAT’nin arkasında olamaz.
* *AddressPrefix* şirket içi adres alanınızdır.

- Tek bir adres ön ekine sahip bir yerel ağ geçidi eklemek için:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
  -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix   '10.0.0.0/24'
  ```

- Birden çok adres ön ekine sahip bir yerel ağ geçidi eklemek için:

  ```powershell
  New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
  -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')
  ```

- Yerel ağ geçidinizin IP adresi ön eklerini değiştirmek için:<br>
Bazen yerel ağ geçidi ön ekleriniz değişir. IP adresi ön eklerinizi değiştirmek için uygulayacağınız adımlar bir VPN ağ geçidi bağlantısı oluşturup oluşturmadığınıza göre değişir. Bu makalenin [Yerel bir ağ geçidinin IP adresi ön eklerini değiştirme](#modify) bölümüne bakın.

## <a name="PublicIP"></a>4. Genel IP adresi isteme

Bir VPN ağ geçidinin genel bir IP adresi olmalıdır. İlk olarak IP adresi kaynağını istemeniz, sonra sanal ağ geçidinizi oluştururken bu kaynağa başvurmanız gerekir. VPN ağ geçidi oluşturulurken, IP adresi kaynağa dinamik olarak atanır. VPN Gateway hizmeti şu anda yalnızca *Dinamik* Genel IP adresi ayırmayı desteklemektedir. Statik bir Genel IP adresi ataması isteğinde bulunamazsınız. Ancak, bu durum IP adresinin VPN ağ geçidinize atandıktan sonra değiştiği anlamına gelmez. Genel IP adresi, yalnızca ağ geçidi silinip yeniden oluşturulduğunda değişir. VPN ağ geçidiniz üzerinde gerçekleştirilen yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında değişmez.

Sanal ağ VPN ağ geçidinize bir Genel IP adresinin atanmasını isteyin.

```powershell
$gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic
```

## <a name="GatewayIPConfig"></a>5. Ağ geçidi IP adresleme yapılandırmasını oluşturma

Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Ağ geçidi yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
```

## <a name="CreateGateway"></a>6. VPN ağ geçidini oluşturma

Sanal ağ VPN ağ geçidini oluşturun. VPN ağ geçidi oluşturma işleminin tamamlanması 45 dakika veya daha uzun sürebilir.

Aşağıdaki değerleri kullanın:

* Siteden Siteye bir yapılandırma için *-GatewayType* değeri *Vpn* olmalıdır. Ağ geçidi türü her zaman, uygulamakta olduğunuz yapılandırmaya özeldir. Örneğin, başka ağ geçidi yapılandırmalarında -GatewayType değerinin ExpressRoute olması gerekebilir.
* *-VpnType*, *RouteBased* (bazı belgelerde Dinamik Ağ Geçidi olarak adlandırılır) veya *PolicyBased* (bazı belgelerde Statik Ağ Geçidi olarak adlandırılır) olabilir. VPN ağ geçidi türleri hakkında daha fazla bilgi için bkz. [VPN Gateway Hakkında](vpn-gateway-about-vpngateways.md).
* *-GatewaySku* değeri Basic, Standard veya HighPerformance olabilir. Bazı SKU’larda yapılandırma sınırlamaları vardır. Daha fazla bilgi için bkz. [Ağ geçidi SKU'ları](vpn-gateway-about-vpngateways.md#gateway-skus).

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased -GatewaySku Standard
```

## <a name="ConfigureVPNDevice"></a>7. VPN cihazınızı yapılandırma

[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

PowerShell kullanarak sanal ağ geçidinizin Genel IP adresini bulmak için şu örneği kullanın:

```powershell
Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
```

## <a name="CreateConnection"></a>8. VPN bağlantısını oluşturma

Bir sonraki adımda, sanal ağ geçidiniz ile VPN cihazınız arasındaki Siteden Siteye VPN bağlantısını oluşturacaksınız. Değerlerin kendinizinkilerle değiştirildiğinden emin olun. Paylaşılan anahtar, VPN cihazınızın yapılandırması için kullandığınız değerin aynısı olmalıdır. Siteden Siteye bağlantı için ‘-ConnectionType’ değerinin *IPsec* olduğunu unutmayın.

1. Değişkenleri ayarlayın.
  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
  $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
  ```

2. Bağlantıyı oluşturun.
  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName testrg `
  -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

Kısa bir süre içerisinde bağlantı kurulur.

## <a name="toverify"></a>9. VPN bağlantısını doğrulama
VPN bağlantınızı doğrulamanın birkaç farklı yolu vardır.

[!INCLUDE [Verify connection](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="connectVM"></a>Sanal makineye bağlanma

[!INCLUDE [Connect to VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]


## <a name="modify"></a>Yerel bir ağ geçidinin IP adresi ön eklerini değiştirme

Şirket içi konumunuza yönlendirilmesini istediğiniz IP adresi ön ekleri değişirse, yerel ağ geçidini değiştirebilirsiniz. İki ayrı yönerge grubu sunulmuştur. Hangi yönergeleri seçeceğiniz, ağ geçidi bağlantınızı önceden oluşturup oluşturmadığınıza bağlıdır.

[!INCLUDE [Modify prefixes](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Yerel bir ağ geçidinin ağ geçidi IP adresini değiştirme

[!INCLUDE [Modify gw IP](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

*  Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için bkz. [Sanal Makineler](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* BGP hakkında bilgi edinmek için [BGP’ye Genel Bakış](vpn-gateway-bgp-overview.md) ve [BGP’yi yapılandırma](vpn-gateway-bgp-resource-manager-ps.md) makalelerine bakın.

