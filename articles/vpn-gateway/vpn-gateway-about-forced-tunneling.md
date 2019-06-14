---
title: 'Azure siteden siteye bağlantılar için zorlamalı tünel yapılandırma: Klasik | Microsoft Docs'
description: Nasıl yeniden yönlendirme veya 'tüm İnternet'e bağlı trafiği şirket içi konumunuza geri force'.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 0955d95ebfd9e1f72ed1da577bf3520a70b71624
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60506022"
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak zorlamalı tünel yapılandırma

Zorlamalı tünel yeniden yönlendirme veya tüm İnternet'e bağlı trafiği, inceleme ve denetim için bir siteden siteye VPN tüneli aracılığıyla şirket içi konumunuza geri "force" sağlar. Bu, çoğu kurumsal BT için kritik güvenlik gereksinimdir ilkeleri. Zorlamalı tünel olmadan, Internet'e bağlı trafik azure'daki sanal makinelerinize her zaman Azure ağ altyapısından doğrudan inceleme veya trafiği denetlemek için izin verme seçeneği olmadan Internet'e gerçekleşmedikçe. Yetkisiz Internet erişimi, bilgilerin açıklanması veya diğer tür güvenlik ihlallerini olası neden olabilir.

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu makalede, zorlamalı tünel Klasik dağıtım modeli kullanılarak oluşturulmuş sanal ağlar için nasıl yapılandıracağınız anlatılmaktadır. Zorlamalı tünel, portal üzerinden değil, PowerShell kullanarak yapılandırılabilir. Resource Manager dağıtım modeli için zorlamalı tünel yapılandırmak istiyorsanız, Resource Manager makale aşağıdaki açılır listeden seçin:

> [!div class="op_single_selector"]
> * [PowerShell - Klasik](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Gereksinimleri ve konular
Zorlamalı tünel Azure'da sanal ağ kullanıcı tanımlı yollar (UDR) yapılandırılır. Bir şirket içi siteye trafik yönlendirme Azure VPN ağ geçidi için varsayılan bir yol olarak ifade edilir. Aşağıdaki bölümde, Azure sanal ağı için yönlendirme tablosu ve yol geçerli sınırlama listelenmektedir:

* Her sanal ağ alt ağı, yerleşik bir sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosu yolların aşağıdaki üç grup vardır:

  * **Yerel sanal ağ yolları:** Doğrudan hedefe Vm'leri aynı sanal ağda.
  * **Şirket içi yolları:** Azure VPN ağ geçidi için.
  * **Varsayılan yol:** Doğrudan Internet'e. Önceki iki yol tarafından kapsandığından değil özel IP adreslerini hedefleyen paketler bırakılır.
* Kullanıcı tanımlı yollar'ın yayınlanmasıyla birlikte, varsayılan bir yol eklemek için bir yönlendirme tablosu oluşturun ve ardından bu alt ağlarda zorlamalı tüneli etkinleştirmek için sanal ağ alt ağı, başarılı için yönlendirme tablosu ilişkilendirebilirsiniz.
* "Sanal ağa bağlı bir varsayılan site" şirket içi yerel siteleri arasında ayarlamanız gerekir.
* Zorlamalı tünel dinamik yönlendirme VPN ağ geçidi (bir statik ağ geçidi sorunsuz değil) sahip bir sanal ağ ile ilişkilendirilmiş olması gerekir.
* Zorlamalı tünel ExpressRoute Bu mekanizma yapılandırılmamış, ancak bunun yerine, bir varsayılan rota üzerinden ExpressRoute BGP eşliği oturumlarını reklam tarafından etkinleştirilir. Lütfen [ExpressRoute belgeleri](https://azure.microsoft.com/documentation/services/expressroute/) daha fazla bilgi için.

## <a name="configuration-overview"></a>Yapılandırmasına genel bakış
Aşağıdaki örnekte, ön uç alt ağı değil zorlamalı tünel uygulanmaz. Frontend alt iş yüklerinin kabul etmek ve doğrudan Internet'ten müşteri isteklerine yanıt vermek devam edebilirsiniz. Orta katman ve arka uç alt ağları zorlamalı tünel. Bu iki alt ağa giden tüm bağlantılarından İnternet'e zorlamalı veya bir şirket içi sitede bir S2S VPN tünelleri aracılığıyla yeniden.

Bu kısıtlama ve sanal makinelerinizdeki Internet erişimi denetleme veya çok katmanlı bir hizmet Mimarinizi gerekli etkinleştirmek devam ederken bulut Hizmetleri, azure'da sağlar. Ayrıca, sanal ağlarınızda hiçbir Internet'e yönelik iş yükü, tüm sanal ağları için zorlamalı tünel uygulayabilirsiniz.

![Zorlamalı Tünel Oluşturma](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Başlamadan önce
Yapılandırmaya başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Yapılandırılmış bir sanal ağ. 
* Azure PowerShell cmdlet'lerinin en son sürümü. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="configure-forced-tunneling"></a>Zorlamalı tünel yapılandırma
Aşağıdaki yordam bir sanal ağ için zorlamalı tünel belirtmenize yardımcı olur. Yapılandırma adımları VNet ağ yapılandırma dosyasına karşılık gelir.

```xml
<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>
```

Bu örnekte, üç alt sanal ağ 'MultiTier-VNet' sahiptir: 'Ön uç', 'Midtier' ve 'Backend' alt ağlar, dört şirket içi ve dışı bağlantılar: 'DefaultSiteHQ' ve üç dalları. 

Adımları 'DefaultSiteHQ' varsayılan site bağlantısı olarak zorlamalı tünel ve Midtier yapılandırma ve zorlamalı tünel kullanmak için arka uç alt ağları.

1. Bir yönlendirme tablosu oluşturun. Yol tablosu oluşturmak için aşağıdaki cmdlet'i kullanın.

   ```powershell
   New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
   ```

2. Varsayılan yolu yönlendirme tablosuna ekleyin. 

   Aşağıdaki örnek, 1. adımda oluşturduğunuz yönlendirme tablosu için varsayılan bir yol ekler. Yalnızca rota desteklenen hedef ön eki için "VPNGateway" sonraki atlama "0.0.0.0/0" nottur.

   ```powershell
   Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
   ```

3. Yönlendirme tablosunu bir alt ağlara ilişkilendirebilirsiniz. 

   Bir yönlendirme tablosu oluşturup bir rota ekledikten sonra ekleme veya bir sanal ağ alt ağı için rota tablosu ilişkilendirmek için aşağıdaki örneği kullanın. Örnek Midtier ve arka uç alt ağları, MultiTier ağlar için rota tablosu "MyRouteTable" ekler.

   ```powershell
   Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
   Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
   ```

4. Zorlamalı tünel için varsayılan site atayın. 

   Önceki adımda, örnek cmdlet betikler yönlendirme tablosu oluşturuldu ve iki sanal ağ alt ağların yol tablosuna ilişkili. Kalan adım, bir yerel site çok siteli bağlantılar sanal ağ arasında tünel ve varsayılan site seçeneğini belirlemektir.

   ```powershell
   $DefaultSite = @("DefaultSiteHQ")
   Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
   ```

## <a name="additional-powershell-cmdlets"></a>Ek PowerShell cmdlet'leri
### <a name="to-delete-a-route-table"></a>Bir rota tablosu silinemedi

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a>Bir yol tablosu listelemek için

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a>Bir rota tablosundan bir rota silinemedi

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a>Bir alt ağdan bir rota kaldırmak için

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Bir alt ağ ile ilişkili yol tablosuna listelemek için

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Sanal ağ VPN ağ geçidini bir varsayılan siteyi kaldırmak için

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```
