---
title: "Klasik sanal ağlar Azure Resource Manager sanal ağlara bağlanma: PowerShell | Microsoft Docs"
description: "Klasik sanal ağlar ve Resource Manager VPN ağ geçidi ve PowerShell kullanarak sanal ağlar arasında bir VPN bağlantısı oluşturun."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/13/2018
ms.author: cherylmc
ms.openlocfilehash: a3afd89a928854a1b03bfd4c5645ea12dbb638fc
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>PowerShell kullanarak farklı dağıtım modellerindeki sanal ağları birbirine bağlama

Bu makalede, Resource Manager birbirleri ile iletişim kurmak için ayrı bir dağıtım modellerindeki kaynaklara izin vermek için sanal ağlar için Klasik sanal ağlar bağlanmanıza yardımcı olur. Bu makaledeki adımları PowerShell kullanın, ancak makaleyi bu listeden seçerek Azure Portalı'nı kullanarak bu yapılandırmayı de oluşturabilirsiniz.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Bir Resource Manager Vnet'i klasik bir VNet bağlama, bir şirket içi site konumuna bir sanal ağa bağlanma benzer. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır. Farklı Aboneliklerde ve farklı bölgelerdeki sanal ağlar arasında bir bağlantı oluşturabilirsiniz. Dinamik ya da rota tabanlı ağ geçidi ile yapılandırılmamış olduğu sürece şirket içi ağlara bağlantılar zaten sanal ağlar da bağlanabilirsiniz. Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağlar arası bağlantılar hakkında SSS](#faq) bölümünü inceleyin. 

Zaten bir sanal ağ geçidi yok ve oluşturmak istemiyorsanız, bunun yerine VNet eşlemesi kullanarak, sanal ağlara bağlanma göz önünde bulundurun isteyebilirsiniz. VNet eşlemesi VPN ağ geçidini kullanmaz. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

## <a name="before"></a>Başlamadan önce

Aşağıdaki adımlar, her sanal ağ için dinamik veya rota tabanlı ağ geçidi yapılandırmak ve ağ geçitleri arasında bir VPN bağlantısı oluşturmak gerekli ayarları size yol. Bu yapılandırma, statik veya ilke tabanlı ağ geçitleri desteklemez.

### <a name="pre"></a>Önkoşullar

* Her iki Vnet'in zaten oluşturulmuş.
* Sanal ağlar için adres aralıklarını değil birbirleri ile üst üste veya ağ geçitleri bağlı olabilir diğer bağlantılar için aralıklardan herhangi biriyle çakışıyor.
* En son PowerShell cmdlet'leri yüklediniz. Bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) daha fazla bilgi için. Hizmet Yönetimi (SM) ve Kaynak Yöneticisi (RM) cmdlet'leri yüklediğinizden emin olun. 

### <a name="exampleref"></a>Örnek ayarlar

Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz.

**Klasik VNet ayarları**

Sanal ağ adı ClassicVNet = <br>
Konum Batı ABD = <br>
Sanal ağ adres alanları 10.0.0.0/24 = <br>
Alt ağ 1 10.0.0.0/27 = <br>
GatewaySubnet 10.0.0.32/29 = <br>
Yerel ağ adı RMVNetLocal = <br>
GatewayType DynamicRouting =

**Resource Manager Vnet'i ayarları**

Sanal ağ adı RMVNet = <br>
Kaynak grubu RG1 = <br>
Sanal ağ IP adresi alanları 192.168.0.0/16 = <br>
Alt ağ 1 192.168.1.0/24 = <br>
GatewaySubnet = 192.168.0.0/26 <br>
Konum Doğu ABD = <br>
Ağ geçidi genel IP adı gwpip = <br>
Yerel ağ geçidi ClassicVNetLocal = <br>
Sanal ağ geçidi adı RMGateway = <br>
Ağ geçidi IP adresleme yapılandırmasını gwipconfig =

## <a name="createsmgw"></a>1. Bölüm - Klasik VNet yapılandırın
### <a name="1-download-your-network-configuration-file"></a>1. Ağ yapılandırma dosyası indirme
1. PowerShell konsolunda yükseltilmiş haklara sahip Azure hesabınızda oturum açın. Aşağıdaki cmdlet'i Azure hesabınız için oturum açma kimlik bilgilerini ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir. Bu bölümde Klasik Hizmet Yönetimi (SM) Azure PowerShell cmdlet'leri kullanılır.

  ```powershell
  Add-AzureAccount
  ```

  Azure aboneliğiniz alın.

  ```powershell
  Get-AzureSubscription
  ```

  Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

  ```powershell
  Select-AzureSubscription -SubscriptionName "Name of subscription"
  ```
2. Azure ağı yapılandırma dosyanızda aşağıdaki komutu çalıştırarak dışa aktarın. Farklı bir konum gerekirse dışa aktarılacak dosya konumunu değiştirebilirsiniz.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
3. İndirilen düzenlemek için .xml dosyasını açın. Ağ yapılandırma dosyası örneği için bkz: [ağ yapılandırma şeması](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="2-verify-the-gateway-subnet"></a>2. Ağ geçidi alt ağını doğrulayın
İçinde **VirtualNetworkSites** öğesi değil zaten oluşturulmuş bir Vnet'inizi için bir ağ geçidi alt ağı ekleyin. Ağ yapılandırma dosyası ile çalışırken, ağ geçidi alt ağı "GatewaySubnet" adlı gerekir veya Azure algılar ve bir ağ geçidi alt ağı kullanın.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Örnek:**

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="3-add-the-local-network-site"></a>3. Yerel ağ sitesi ekleme
Eklediğiniz yerel ağ sitesine bağlanmak istediğiniz RM VNet temsil eder. Ekleme bir **LocalNetworkSites** zaten yoksa, dosyaya öğesi. Bu noktada biz henüz Resource Manager Vnet'i için ağ geçidi oluşturmadınız çünkü yapılandırmasında VPNGatewayAddress herhangi bir geçerli ortak IP adresi olabilir. Biz ağ geçidini oluşturduktan sonra Biz bu yer tutucu IP adresi RM ağ geçidine atanmış doğru ortak IP adresi ile değiştirin.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="4-associate-the-vnet-with-the-local-network-site"></a>4. VNet yerel ağ alanı ile ilişkilendirme
Bu bölümde, Vnet'e bağlanmak istediğiniz yerel ağ sitesi belirtin. Bu durumda, daha önce başvurulan Resource Manager Vnet'i olur. Eşleşen adları emin olun. Bu adım, bir ağ geçidi oluşturmaz. Ağ geçidi bağlanacağı yerel ağ belirtir.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="5-save-the-file-and-upload"></a>5. Dosyayı kaydedin ve karşıya yükleme
Dosyayı kaydedin ve ardından aşağıdaki komutu çalıştırarak Azure'a alın. Ortamınız için gerektiği gibi dosya yolu değiştirdiğinizden emin olun.

```powershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

İçeri aktarma başarılı olduğunu gösteren benzer bir sonuç görürsünüz.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="6-create-the-gateway"></a>6. Ağ geçidi oluşturma

Bu örneği çalıştırmadan önce görmek için Azure bekliyor tam adları için indirdiğiniz ağ yapılandırma dosyasına bakın. Ağ yapılandırma dosyası, Klasik sanal ağlar için değerleri içerir. Bazen adları Klasik sanal ağlar için dağıtım modelleri farklılıkları nedeniyle Azure portalında Klasik VNet ayarlarını oluştururken, ağ yapılandırma dosyasında değiştirilir. Örneğin, Klasik VNet 'Klasik VNet' adlı ve 'ClassicRG' adlı bir kaynak grubunda oluşturulan oluşturmak için Azure Portalı'nı kullandıysanız, ağ yapılandırma dosyasında yer alan adı 'Grup ClassicRG Klasik VNet' dönüştürülür. Boşluk içeren bir sanal ağ adı belirtirken, değeri tırnak işaretleri kullanın.


Dinamik yönlendirme ağ geçidi oluşturmak için aşağıdaki örneği kullanın:

```powershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

Kullanarak ağ geçidi durumunu kontrol edebilirsiniz **Get-AzureVNetGateway** cmdlet'i.

## <a name="creatermgw"></a>2. Bölüm - RM VNet ağ geçidini yapılandırma
RM VNet için VPN ağ geçidi oluşturmak için aşağıdaki yönergeleri izleyin. Klasik sanal ağınızın ağ geçidi için genel IP adresi aldıktan sonra kadar adımları başlatmayın. 

1. PowerShell konsolundaki Azure hesabınızda oturum açın. Aşağıdaki cmdlet'i Azure hesabınız için oturum açma kimlik bilgilerini ister. Oturum açtıktan sonra Azure PowerShell kullanılabilir olacak şekilde, hesap ayarlarınızı karşıdan yüklenir.

  ```powershell
  Login-AzureRmAccount
  ``` 
   
  Azure aboneliklerinizin bir listesini alın.

  ```powershell
  Get-AzureRmSubscription
  ```
   
  Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Name of subscription"
  ```
2. Bir yerel ağ geçidi oluşturun. Sanal bir ağda, yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Bu durumda, yerel ağ geçidi Klasik ağınızı anlamına gelir. Olarak Azure başvurduğu ve ayrıca adres alanı ön ekini belirtin bir ad verin. Azure, belirttiğiniz IP adresi ön ekini kullanarak hangi trafiğin şirket içi konumunuza gönderileceğini belirler. Burada yer alan bilgiler, daha sonra ağ geçidi oluşturmadan önce ayarlamanız gerekip gerekmediğine değerleri değiştirin ve örnek yeniden çalıştırın.
   
   **-Name** yerel ağ geçidine başvurmak için atamak istediğiniz addır.<br>
   **-AddressPrefix** Klasik ağınız için adres alanıdır.<br>
   **-Gatewayıpaddress** Klasik sanal ağınızın ağ geçidinin genel IP adresidir. Aşağıdaki örnek doğru IP adresini gösterecek şekilde değiştirdiğinizden emin olun.<br>

  ```powershell
  New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
  -Location "West US" -AddressPrefix "10.0.0.0/24" `
  -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
  ```
3. Resource Manager Vnet'i için sanal ağ geçidine ayrılacak genel IP adresi isteyin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, sanal ağ geçidi olarak dinamik olarak ayrılır. Ancak bu, IP adresinin değiştiği anlamına gelmez. Yalnızca bir kez sanal ağ geçidi IP adresi değişiklikleri olduğunda ağ geçidi silinip yeniden. Yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında ağ geçidi değiştirmez.

  Bu adımda, biz de bir sonraki adımda kullanılan bir değişken ayarlayın.

  ```powershell
  $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
  -ResourceGroupName RG1 -Location 'EastUS' `
  -AllocationMethod Dynamic
  ```

4. Sanal ağınızın ağ geçidi alt ağı olduğunu doğrulayın. Hiçbir ağ geçidi alt ağı varsa ekleyin. Ağ geçidi alt ağı adlandırılan emin olun *GatewaySubnet*.
5. Aşağıdaki komutu çalıştırarak ağ geçidi için kullanılan alt ağ alın. Bu adımda, biz de sonraki adımda kullanılması için bir değişken ayarlayın.
   
   **-Name** Resource Manager Vnet'i adıdır.<br>
   **-ResourceGroupName** VNet ilişkili olduğu kaynak grubu. Ağ geçidi alt ağı için bu sanal ağ zaten mevcut olmalıdır ve adlandırılmalıdır *GatewaySubnet* düzgün çalışması için.<br>

  ```powershell
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
  -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
  ``` 

6. Ağ geçidi IP adresleme yapılandırmasını oluşturun. Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Ağ geçidi yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın.

  Bu adımda, **- SubnetId** ve **- PublicIpAddressId** parametreleri gerekir bayraklarıdır ID özelliği alt ağ ve IP adresi nesneleri, sırasıyla. Basit bir dize kullanamazsınız. Adımda, bir ortak IP ve adım alt almak için istemek için bu değişkenleri ayarlayın.

  ```powershell
  $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
  -Name gwipconfig -SubnetId $subnet.id `
  -PublicIpAddressId $ipaddress.id
  ```
7. Aşağıdaki komutu çalıştırarak Resource Manager sanal ağ geçidi oluşturun. `-VpnType` Olmalıdır *RouteBased*. 45 dakika veya daha fazla ağ geçidinin oluşturulması alabilir.

  ```powershell
  New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
  -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
  -IpConfigurations $gwipconfig `
  -EnableBgp $false -VpnType RouteBased
  ```
8. VPN ağ geçidi oluşturulduktan sonra genel IP adresini kopyalayın. Klasik VNet yerel ağ ayarlarını yapılandırırken kullanın. Genel IP adresi almak için aşağıdaki cmdlet'i kullanabilirsiniz. Genel IP adresi, dönüş olarak listelenen *IPADDRESS*.

  ```powershell
  Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1
  ```

## <a name="localsite"></a>3. Bölüm - Klasik VNet yerel site ayarlarını değiştirme

Bu bölümde, Klasik VNet ile birlikte çalışır. Resource Manager Vnet'i ağ geçidine bağlanmak için kullanılan yerel site ayarlarını belirtmek için kullanılan yer tutucu IP adresini değiştirin. 

1. Ağ yapılandırma dosyasını dışarı aktarın.

  ```powershell
  Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
  ```
2. Bir metin düzenleyicisi kullanarak, değer VPNGatewayAddress için değiştirin. Resource Manager ağ geçidi genel IP adresiyle yer tutucu IP adresini değiştirin ve değişiklikleri kaydedin.

  ```
  <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
  ```
3. Değiştirilen ağ yapılandırma dosyasını Azure'a içeri aktarın.

  ```powershell
  Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
  ```

## <a name="connect"></a>4. bölüm - ağ geçitleri arasında bağlantı oluşturma
Ağ geçitleri arasında bir bağlantı oluşturmak için PowerShell gerekir. PowerShell cmdlet'leri Klasik sürümü kullanmak için Azure hesabınız eklemeniz gerekebilir. Bunu yapmak için kullanın **Add-AzureAccount**.

1. PowerShell konsolunda paylaşılan anahtarınız ayarlayın. Cmdlet'leri çalıştırmadan önce görmek için Azure bekliyor tam adları için indirdiğiniz ağ yapılandırma dosyasına bakın. Boşluk içeren bir sanal ağ adı belirtirken, değeri tek tırnak işareti kullanın.<br><br>Aşağıdaki örnekte, **- vnetname adlı** Klasik VNet adıdır ve **- LocalNetworkSiteName** yerel ağ sitesi için belirtilen ad. **- SharedKey** oluşturmak ve belirten bir değer. Örnekte 'abc123' kullandık ancak oluşturabilir ve daha karmaşık bir şey kullanın. Burada belirttiğiniz değer bağlantınızı oluştururken sonraki adımda belirttiğiniz değerle aynı değere olmalıdır önemli şeydir. Dönüş göstermelidir **durumu: başarılı**.

  ```powershell
  Set-AzureVNetGatewayKey -VNetName ClassicVNet `
  -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
  ```
2. Aşağıdaki komutları çalıştırarak VPN bağlantısı oluşturun:
   
  Değişkenleri ayarlayın.

  ```powershell
  $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
  $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
  ```
   
  Bağlantıyı oluşturun. Dikkat **- ConnectionType** IPSec, Vnet2Vnet değil.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
  -Location "East US" -VirtualNetworkGateway1 `
  $vnet02gateway -LocalNetworkGateway2 `
  $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
  ```

## <a name="verify"></a>Bölüm 5 - bağlantılarınızı doğrulayın

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Resource Manager Vnet'i Klasik VNet arasında bağlantı doğrulamak için

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a>Azure portalına

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Resource Manager Vnet'i bağlantısından Klasik vnet doğrulamak için

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a>Azure portalına

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Sanal ağlar arası bağlantılar hakkında SSS

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]