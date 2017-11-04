---
title: "Zorlamalı tünel Azure siteden siteye bağlantıları için yapılandırın: Klasik | Microsoft Docs"
description: "Yeniden yönlendirme veya 'force' tüm Internet'e bağlı trafik, şirket içi konumuna geri nasıl."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 79bf6892c823da282c3e763921e830f986419854
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak zorlamalı tünel yapılandırma

Zorlamalı tünel yeniden yönlendirme veya "tüm Internet'e bağlı trafik, denetleme ve denetim için bir siteden siteye VPN tüneli aracılığıyla şirket içi konumunuza geri zorla" olanak sağlar. Bu, çoğu kurumsal BT için kritik güvenlik gereksinimdir ilkeleri. Zorlamalı tünel olmadan, Internet'e bağlı trafik, vm'lerden azure'da her zaman Azure ağ altyapısından doğrudan inceleme veya trafiği denetlemek için izin verme seçeneği olmadan Internet'e dizinlere. Yetkisiz Internet erişimine potansiyel olarak bilgilerin açığa çıkmasına veya diğer tür güvenlik ihlallerini yol açabilir.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu makalede Klasik dağıtım modeli kullanılarak oluşturulan sanal ağlar için tünel zorlanmış'ı yapılandıracağınız anlatılmaktadır. Zorlamalı tünel, Portalı aracılığıyla değil, PowerShell kullanarak yapılandırılabilir. Resource Manager dağıtım modeli için zorlamalı tünel yapılandırmak istiyorsanız, Klasik makale aşağıdaki açılan listeden seçin:

> [!div class="op_single_selector"]
> * [PowerShell - Klasik](vpn-gateway-about-forced-tunneling.md)
> * [PowerShell - Resource Manager](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a>Gereksinimleri ve konular
Zorlamalı tünel Azure'da sanal ağ kullanıcı tanımlı yolları (UDR) yapılandırılır. Bir şirket içi siteye trafiği yönlendirerek Azure VPN ağ geçidi için varsayılan bir yol olarak ifade edilir. Aşağıdaki bölümde, bir Azure sanal ağı için yönlendirme tablosunu ve yollar geçerli sınırlaması listelenmektedir:

* Her sanal ağ alt yerleşik sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosu yolların aşağıdaki üç grup vardır:

  * **Yerel VNet yollar:** doğrudan hedefe Vm'leri aynı sanal ağda.
  * **Şirket içi yollar:** için Azure VPN ağ geçidi.
  * **Varsayılan yol:** doğrudan Internet'e. Önceki iki yoldan tarafından kapsanmayan özel IP adresleri giden paketler düşürülür.
* Kullanıcı tanımlı yollar sürümle birlikte, varsayılan yol eklemek üzere bir yönlendirme tablosu oluşturun ve ardından bu alt ağlardaki zorlamalı tüneli etkinleştirmek için sanal ağ alt ağı için yönlendirme tablosunu ilişkilendirin.
* "Sanal ağa bağlı bir varsayılan site" şirket içi yerel siteleri arasında ayarlamanız gerekir.
* Zorlamalı tünel dinamik yönlendirme VPN ağ geçidi (olmayan bir statik ağ geçidi) sahip bir sanal ağ ile ilişkilendirilmiş olması gerekir.
* Zorlanan tünel ExpressRoute bu düzenek yapılandırılmamış, ancak bunun yerine, varsayılan rota ExpressRoute BGP eşliği oturumlarını üzerinden reklam tarafından etkinleştirilir. Lütfen bakın [ExpressRoute belgeleri](https://azure.microsoft.com/documentation/services/expressroute/) daha fazla bilgi için.

## <a name="configuration-overview"></a>Yapılandırmasına genel bakış
Aşağıdaki örnekte, ön uç alt ağı değil zorlamalı tünel. Ön uç alt iş yükleri kabul etmek ve doğrudan Internet'ten müşteri isteklerine yanıt vermek devam edebilirsiniz. Orta katman ve arka uç alt zorlamalı tünel. Bu iki alt internet tüm giden bağlantılar zorlanmış veya siteye bir şirket içi S2S VPN tünelleri biri aracılığıyla yeniden yönlendirildi.

Bu kısıtlama ve Internet erişimi, sanal makinelerden inceleme veya Bulut Hizmetleri, azure'da çok katmanlı bir hizmet Mimarinizi gerekli etkinleştirmek devam ederken olanak sağlar. Ayrıca, sanal ağlarınız hiçbir Internet'e iş yükleri varsa tüm sanal ağların zorlamalı tünel uygulayabilirsiniz.

![Zorlamalı Tünel Oluşturma](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a>Başlamadan önce
Yapılandırmaya başlamadan önce aşağıdaki öğelerin bulunduğunu doğrulayın.

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa [MSDN abonelik avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Yapılandırılmış bir sanal ağ. 
* Azure PowerShell cmdlet'lerinin en son sürümü. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="configure-forced-tunneling"></a>Zorlamalı tünel yapılandırma
Aşağıdaki yordam için bir sanal ağ zorlamalı tünel belirtmenize yardımcı olur. Yapılandırma adımları sanal ağ yapılandırma dosyasına karşılık gelir.

```
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

Bu örnekte, sanal ağ 'MultiTier-VNet' üç alt ağa sahip değil: 'Ön uç', 'Midtier' ve 'Backend' alt ağlar, dört şirket içi ve dışı bağlantılar: 'DefaultSiteHQ' ve üç dalları. 

Adımları zorlanan tünel 'DefaultSiteHQ' varsayılan site bağlantısı olarak ayarlayın ve Midtier yapılandırmak ve kullanmak için arka uç alt ağlar zorlanan tünel.

1. Bir yönlendirme tablosu oluşturun. Yol tablosu oluşturmak için aşağıdaki cmdlet'i kullanın.

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. Varsayılan rota yönlendirme tablosuna ekleyin. 

  Aşağıdaki örnek varsayılan yolun 1. adımda oluşturduğunuz yönlendirme tablosuna ekler. Yalnızca rota desteklenen Not "VPNGateway" NextHop için "0.0.0.0/0" hedef önekidir.

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. Alt ağlar için yönlendirme tablosunu ilişkilendirin. 

  Bir yönlendirme tablosu oluşturulur ve bir yol eklenir sonra eklemek veya bir sanal ağ alt ağı için yol tablosu ilişkilendirmek için aşağıdaki örneği kullanın. Örnek Midtier ve arka uç alt ağlar VNet MultiTier-VNet için yol tablosu "MyRouteTable" ekler.

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. Varsayılan site zorlanan tünel için atayın. 

  Önceki adımda, örnek cmdlet komutlar yönlendirme tablosu oluşturulur ve iki sanal ağ alt ağ için yol tablosu ilişkilendirilmiş. Kalan sanal ağın çok siteli bağlantıları arasında yerel bir site varsayılan site veya tünel seçmek için bir adımdır.

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a>Ek PowerShell cmdlet'leri
### <a name="to-delete-a-route-table"></a>Bir yol tablosu silmek için

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a>Bir yol tablosu listelemek için

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a>Bir rota tablosundan bir yolu silmek için

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a>Bir alt ağdan bir rota kaldırmak için

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Bir alt ağ ile ilişkili yol tablosu listelemek için

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Sanal ağ VPN ağ geçidi'nden bir varsayılan siteyi kaldırmak için

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```