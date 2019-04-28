---
title: 'Klasik sanal ağlar, Azure Resource Manager sanal ağlarına bağlama: PowerShell | Microsoft Docs'
description: Klasik sanal ağlar ile Resource Manager VPN Gateway ve PowerShell kullanarak sanal ağlar arasında VPN bağlantısı oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/17/2018
ms.author: cherylmc
ms.openlocfilehash: 2263996b84b17f7de9826c07eb28e4b7668cd915
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62095599"
---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>PowerShell kullanarak farklı dağıtım modellerindeki sanal ağları birbirine bağlama

Bu makalede Klasik sanal ağları Resource Manager sanal ağlarına birbirleri ile iletişim kurmak üzere ayrı dağıtım modellerindeki kaynaklara izin verecek şekilde erişmenize yardımcı olur. Bu makaledeki adımlarda PowerShell kullanın, ancak ayrıca makalede bu listeden seçerek Azure portalını kullanarak bu yapılandırmayı oluşturabilirsiniz.

> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

İçin Resource Manager Vnet'i klasik bir VNet bağlama VNet bir şirket içi site konumuna bağlamakla aynıdır. Her iki bağlantı türü de IPsec/IKE kullanarak güvenli bir tünel sunmak üzere bir VPN ağ geçidi kullanır. Farklı Aboneliklerdeki ve farklı bölgelerdeki sanal ağlar arasında bir bağlantı oluşturabilirsiniz. Dinamik ya da rota tabanlı ağ geçidi ile yapılandırılmış olduğu sürece şirket içi ağlara bağlantıları olan sanal ağlar da bağlanabilirsiniz. Sanal ağlar arası bağlantılar hakkında daha fazla bilgi için bu makalenin sonunda yer alan [Sanal ağlar arası bağlantılar hakkında SSS](#faq) bölümünü inceleyin. 

Zaten bir sanal ağ geçidi yok ve oluşturmak istemiyorsanız, bunun yerine kullanarak VNet eşlemesi, sanal ağları bağlama düşünün isteyebilirsiniz. VNet eşlemesi VPN ağ geçidini kullanmaz. Daha fazla bilgi için bkz. [VNet eşlemesi](../virtual-network/virtual-network-peering-overview.md).

## <a name="before"></a>Başlamadan önce

Aşağıdaki adımlar, her sanal ağ için dinamik ya da rota tabanlı ağ geçidi yapılandırma ve ağ geçitleri arasında bir VPN bağlantısı oluşturmak gereken ayarları yol. Bu yapılandırma, statik ya da ilke tabanlı ağ geçitleri desteklemez.

### <a name="pre"></a>Önkoşullar

* Her iki Vnet'in zaten oluşturdunuz. Resource manager sanal ağı oluşturmak için ihtiyacınız varsa bkz [bir kaynak grubunu ve sanal ağ oluşturma](../virtual-network/quick-create-powershell.md#create-a-resource-group-and-a-virtual-network). Klasik bir sanal ağ oluşturmak için bkz [Klasik sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/create-virtual-network-classic).
* Sanal ağlar için adres aralıklarını değil birbiriyle çakışma veya ağ geçitlerinin bağlı diğer bağlantılar aralıklardan herhangi biriyle çakışıyor.
* En son PowerShell cmdlet'leri yüklediniz. Bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview) daha fazla bilgi için. Hizmet Yönetimi (SM) hem de kaynak yöneticisi (RM) cmdlet'leri yüklediğinizden emin olun. 

### <a name="exampleref"></a>Örnek ayarlar

Bu değerleri kullanarak bir test ortamı oluşturabilir veya bu makaledeki örnekleri daha iyi anlamak için bunlara bakabilirsiniz.

**Klasik sanal ağ ayarları**

VNet adı ClassicVNet = <br>
Konum Batı ABD = <br>
Sanal ağ adres alanları 10.0.0.0/24 = <br>
Alt ağ-1 10.0.0.0/27 = <br>
GatewaySubnet 10.0.0.32/29 = <br>
Yerel ağ adı RMVNetLocal = <br>
GatewayType DynamicRouting =

**Resource Manager Vnet'i ayarları**

VNet adı RMVNet = <br>
Kaynak grubu RG1 = <br>
Sanal ağ IP adresi alanları 192.168.0.0/16 = <br>
Alt ağ-1 = 192.168.1.0/24 <br>
GatewaySubnet 192.168.0.0/26 = <br>
Konumu Doğu ABD = <br>
Ağ geçidi genel IP adı gwpip = <br>
Yerel ağ geçidi ClassicVNetLocal = <br>
Sanal ağ geçidi adı RMGateway = <br>
Ağ geçidi IP adresleme yapılandırması gwipconfig =

## <a name="createsmgw"></a>1. Bölüm - Klasik sanal ağ yapılandırma
### <a name="1-download-your-network-configuration-file"></a>1. Ağ yapılandırma dosyanızı indirin
1. PowerShell konsolunda yükseltilmiş haklara sahip Azure hesabınızda oturum açın. Aşağıdaki cmdlet'i Azure hesabınıza ilişkin oturum açma kimlik bilgilerini ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir. Bu bölümde, Klasik Hizmet Yönetimi (SM) Azure PowerShell cmdlet'leri kullanılır.

   ```azurepowershell
   Add-AzureAccount
   ```

   Azure aboneliğinizi alın.

   ```azurepowershell
   Get-AzureSubscription
   ```

   Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin.

   ```azurepowershell
   Select-AzureSubscription -SubscriptionName "Name of subscription"
   ```
2. Azure ağ yapılandırma dosyanız, aşağıdaki komutu çalıştırarak dışarı aktarın. Farklı bir konuma gerekirse dışa aktarılacak dosyanın konumunu değiştirebilirsiniz.

   ```azurepowershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
3. Düzenlemek için indirdiğiniz .xml dosyasını açın. Ağ yapılandırma dosyası örneği için bkz: [ağ yapılandırma şeması](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="2-verify-the-gateway-subnet"></a>2. Ağ geçidi alt ağı doğrulayın
İçinde **VirtualNetworkSites** öğesi değil zaten oluşturulmuş bir ağ geçidi alt ağı, sanal ağa ekleyin. Ağ yapılandırma dosyası ile çalışırken, ağ geçidi alt ağı "GatewaySubnet" adlı gerekir veya Azure'nın tanımak ve bir ağ geçidi alt ağı olarak kullanın.

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

### <a name="3-add-the-local-network-site"></a>3. Yerel ağ sitesi ekleyin
Eklediğiniz yerel ağ alanına RM bağlanmak istediğiniz sanal ağı temsil eder. Ekleme bir **LocalNetworkSites** zaten yoksa, dosyaya öğesi. Bu noktada size henüz Resource Manager sanal ağı için ağ geçidi oluşturmadıysanız çünkü Yapılandırması'nda herhangi bir geçerli genel IP adresi alanının VPNGatewayAddress olabilir. Ağ geçidi biz oluşturduktan sonra Biz bu yer tutucu IP adresi RM ağ geçidine atanan doğru genel IP adresi ile değiştirin.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="4-associate-the-vnet-with-the-local-network-site"></a>4. VNet yerel ağ alanı ile ilişkilendirin
Bu bölümde, biz sanal ağa bağlamak istediğiniz yerel ağ alanı belirtin. Bu durumda, daha önce başvurulan Resource Manager Vnet'i olur. Eşleşen adları emin olun. Bu adım, bir ağ geçidi oluşturmaz. Bu, ağ geçidi bağlanacağı yerel ağ belirtir.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="5-save-the-file-and-upload"></a>5. Dosyayı kaydedin ve karşıya yükleme
Dosyayı kaydedin ve ardından aşağıdaki komutu çalıştırarak Azure'a aktarın. Ortamınız için gerektiği gibi dosya yolu değiştirdiğinizden emin olun.

```azurepowershell
Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
```

İçeri aktarma başarılı olduğunu gösteren benzer bir sonuç göreceksiniz.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="6-create-the-gateway"></a>6. Ağ geçidi oluşturma

Bu örneği çalıştırmadan önce indirdiğiniz ağ yapılandırma dosyasını görmek için Azure bekliyor tam adları için bakın. Ağ yapılandırma dosyasını, Klasik sanal ağlar için değerleri içerir. Bazen adlarını Klasik Vnet'ler için Klasik sanal ağ ayarlarını dağıtım modelleri arasındaki farklılıklar nedeniyle Azure portalında oluşturulurken ağ yapılandırma dosyasında değiştirilir. Örneğin, Azure portalında bir Klasik sanal ağ 'Klasik sanal ağ' adlı ve 'ClassicRG' adlı bir kaynak grubunda oluşturulan oluşturmak için kullanılan, ağ yapılandırma dosyasında yer alan adı 'Grup ClassicRG Klasik VNet' dönüştürülür. Boşluk içeren bir Vnet'in adı belirtirken, değeri tırnak işareti kullanın.


Dinamik yönlendirme ağ geçidi oluşturmak için aşağıdaki örneği kullanın:

```azurepowershell
New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting
```

Kullanarak ağ geçidinin durumunu kontrol edebilirsiniz **Get-AzureVNetGateway** cmdlet'i.

## <a name="creatermgw"></a>2. Bölüm - RM sanal ağ geçidi yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Önkoşullar, RM VNet zaten oluşturduğunuzu varsayalım. Bu adımda, RM VNet için bir VPN ağ geçidi oluşturun. Klasik sanal ağın ağ geçidi için genel IP adresi aldıktan sonra kadar bu adımları başlamaz. 

1. PowerShell konsolundaki Azure hesabınızda oturum açın. Aşağıdaki cmdlet'i Azure hesabınıza ilişkin oturum açma kimlik bilgilerini ister. Azure PowerShell için kullanılabilir olacak şekilde oturum açtıktan sonra hesap ayarlarınızı indirilir. İsteğe bağlı olarak, tarayıcıda Azure Cloud Shell'i başlatmak için "Try It" özelliğini kullanabilirsiniz.

   Azure Cloud Shell kullanıyorsanız, aşağıdaki cmdlet'i atla:

   ```azurepowershell
   Connect-AzAccount
   ``` 
   Doğru abonelik kullandığını doğrulamak için aşağıdaki cmdlet'i çalıştırın:  

   ```azurepowershell-interactive
   Get-AzSubscription
   ```
   
   Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "Name of subscription"
   ```
2. Yerel ağ geçidi oluşturma. Sanal bir ağda, yerel ağ geçidi genellikle şirket içi konumunuz anlamına gelir. Bu durumda, yerel ağ geçidi Klasik sanal ağınıza anlamına gelir. Bu, tarafından Azure başvurduğu ve aynı zamanda adres alanı ön ekini belirtin. bir ad verin. Azure, belirttiğiniz IP adresi ön ekini kullanarak hangi trafiğin şirket içi konumunuza gönderileceğini belirler. Buradaki bilgiler, daha sonra ağ geçidini oluşturmadan önce ayarlamanız gerekirse, değerleri değiştirebilir ve örneği tekrar çalıştırın.
   
   **-Ad** yerel ağ geçidine başvurmak için atamak istediğiniz addır.<br>
   **-AddressPrefix** Klasik sanal ağınıza ait adres alanıdır.<br>
   **-Gatewayıpaddress** Klasik sanal ağın ağ geçidi genel IP adresidir. Aşağıdaki örnek metni doğru IP adresini yansıtacak şekilde "n.n.n.n" değiştirdiğinizden emin olun.<br>

   ```azurepowershell-interactive
   New-AzLocalNetworkGateway -Name ClassicVNetLocal `
   -Location "West US" -AddressPrefix "10.0.0.0/24" `
   -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
   ```
3. Resource Manager sanal ağı için sanal ağ geçidine ayrılacak genel IP adresi isteyin. Kullanmak istediğiniz IP adresini belirtemezsiniz. IP adresi, sanal ağ geçidi için dinamik olarak ayrılır. Ancak bu, IP adresinin değiştiği anlamına gelmez. Yalnızca bir kez sanal ağ geçidi IP adresi değişiklikleri olduğunda ağ geçidi silinip yeniden. Yeniden boyutlandırma, sıfırlama veya diğer iç bakım/yükseltme işlemleri sırasında ağ geçidi değiştirmez.

   Bu adımda, biz de sonraki adımlardan birinde kullanılan bir değişken ayarlayın.

   ```azurepowershell-interactive
   $ipaddress = New-AzPublicIpAddress -Name gwpip `
   -ResourceGroupName RG1 -Location 'EastUS' `
   -AllocationMethod Dynamic
   ```

4. Sanal ağınızı bir ağ geçidi alt ağı olduğunu doğrulayın. Hiçbir ağ geçidi alt ağı varsa, bir tane ekleyin. Ağ geçidi alt ağı adlı emin *GatewaySubnet*.
5. Aşağıdaki komutu çalıştırarak ağ geçidi için kullanılan alt ağ alın. Bu adımda, biz de sonraki adımda kullanılacak bir değişken ayarlayın.
   
   **-Ad** , Resource Manager Vnet'i adıdır.<br>
   **-ResourceGroupName** sanal ağ ile ilişkili kaynak grubu. Ağ geçidi alt ağı için bu sanal ağ zaten bulunmalı ve adlandırılmalıdır *GatewaySubnet* düzgün çalışması için.<br>

   ```azurepowershell-interactive
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name GatewaySubnet `
   -VirtualNetwork (Get-AzVirtualNetwork -Name RMVNet -ResourceGroupName RG1)
   ``` 

6. Ağ geçidi IP adresleme yapılandırması oluşturun. Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Ağ geçidi yapılandırmanızı oluşturmak için aşağıdaki örneği kullanın.

   Bu adımda, **- Subnetıd** ve **- Publicıpaddressıd** parametreleri geçirilmelidir ID özelliği alt ağ ve IP adresi nesnelerden, sırasıyla. Basit bir dize kullanamazsınız. Bu değişkenler adımda alt almak için genel bir IP ve adım istemek için ayarlanır.

   ```azurepowershell-interactive
   $gwipconfig = New-AzVirtualNetworkGatewayIpConfig `
   -Name gwipconfig -SubnetId $subnet.id `
   -PublicIpAddressId $ipaddress.id
   ```
7. Resource Manager sanal ağ geçidi, aşağıdaki komutu çalıştırarak oluşturun. `-VpnType` Olmalıdır *RouteBased*. 45 dakika veya daha fazla bilgi için ağ geçidinin oluşturulması alabilir.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
   -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
   -IpConfigurations $gwipconfig `
   -EnableBgp $false -VpnType RouteBased
   ```
8. VPN ağ geçidi oluşturulduğunda genel IP adresini kopyalayın. Klasik sanal yerel ağ ayarlarını yapılandırırken kullanın. Genel IP adresini almak için aşağıdaki cmdlet'i kullanabilirsiniz. Genel IP adresini, dönüş listelenen *IPADDRESS*.

   ```azurepowershell-interactive
   Get-AzPublicIpAddress -Name gwpip -ResourceGroupName RG1
   ```

## <a name="localsite"></a>3. Bölüm - Klasik sanal ağ yerel sitesi ayarlarını değiştirme

Bu bölümde, Klasik VNet ile birlikte çalışır. Resource Manager sanal ağ geçidine bağlanmak için kullanılacak yerel site ayarlarını belirtmek için kullanılan yer tutucu IP adresini değiştirin. Klasik sanal ağ ile çalıştığınızdan, yerel olarak bilgisayarınızda değil Azure Cloud Shell TryIt yüklü PowerShell kullanın.

1. Ağ yapılandırma dosyasını dışarı aktarın.

   ```azurepowershell
   Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
   ```
2. Bir metin düzenleyicisi kullanarak değer alanında VPNGatewayAddress için değiştirin. Resource Manager ağ geçidi genel IP adresiyle yer tutucu IP adresini değiştirin ve değişiklikleri kaydedin.

   ```
   <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
   ```
3. Değiştirilen ağ yapılandırma dosyasını Azure'a aktarın.

   ```azurepowershell
   Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml
   ```

## <a name="connect"></a>4. bölüm - ağ geçitleri arasında bağlantı oluşturma
Ağ geçitleri arasında bağlantı oluşturma PowerShell gerektirir. Azure Klasik PowerShell cmdlet'lerini kullanmanız için hesabınızı eklemeniz gerekebilir. Bunu yapmak için **Add-AzureAccount**.

1. PowerShell konsolunda paylaşılan anahtarınızın ayarlayın. Cmdlet'leri çalıştırmadan önce indirdiğiniz ağ yapılandırma dosyasını görmek için Azure bekliyor tam adları için bakın. Boşluk içeren bir Vnet'in adı belirtirken, değeri tek tırnak işaretleri kullanın.<br><br>Aşağıdaki örnekte, **- VNetName** Klasik VNet adıdır ve **- LocalNetworkSiteName** yerel ağ alanı için belirtilen adı. **- SharedKey** sizin oluşturup belirttiğiniz bir değerdir. Örnekte 'abc123' kullandık, ancak oluşturabilir ve daha karmaşık. Önemli olan, burada belirttiğiniz değerin bağlantınızı oluştururken sonraki adımda belirttiğiniz değerle aynı olması gerekliliğidir. Dönüş göstermelidir **durumu: Başarılı**.

   ```azurepowershell
   Set-AzureVNetGatewayKey -VNetName ClassicVNet `
   -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
   ```
2. Aşağıdaki komutları çalıştırarak VPN bağlantısı oluşturun:
   
   Değişkenleri ayarlayın.

   ```azurepowershell-interactive
   $vnet01gateway = Get-AzLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
   $vnet02gateway = Get-AzVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
   ```
   
   Bağlantıyı oluşturun. Dikkat **- ConnectionType** IPSec, Vnet2Vnet değil.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
   -Location "East US" -VirtualNetworkGateway1 `
   $vnet02gateway -LocalNetworkGateway2 `
   $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
   ```

## <a name="verify"></a>5. Bölüm - bağlantılarınızı doğrulama

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Resource Manager Vnet'i Klasik ağınızdan bağlantıyı doğrulamak için

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

#### <a name="azure-portal"></a>Azure portal

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


### <a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Bağlantının, Resource Manager Vnet'i Klasik sanal ağınıza doğrulamak için

#### <a name="powershell"></a>PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

#### <a name="azure-portal"></a>Azure portal

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="faq"></a>Sanal ağlar arası bağlantılar hakkında SSS

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]
