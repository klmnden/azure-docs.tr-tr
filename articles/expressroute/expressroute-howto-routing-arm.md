---
title: 'Yönlendirme (bir ExpressRoute için eşliği) hattı yapılandırma: Resource Manager: PowerShell: Azure | Microsoft Docs'
description: Bu makalede, bir ExpressRoute bağlantı hattı için özel, ortak ve Microsoft eşlemesinin nasıl oluşturulduğu ve sağlandığı adım adım anlatılmaktadır. Bu makalede ayrıca bağlantı hattınızın durumunu denetleme, bağlantı hattını güncelleştirme veya silme işlemlerinin nasıl yapıldığı da anlatılmaktadır.
documentationcenter: na
services: expressroute
author: osamazia
manager: jonor
editor: ''
tags: azure-resource-manager
ms.assetid: 0a036d51-77ae-4fee-9ddb-35f040fbdcdf
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/3/2018
ms.author: osamaz, jaredr80
ms.openlocfilehash: d4f8f0e6c8fab5dfb4efcc4f1659ba90336c8bf0
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a>Oluşturma ve PowerShell kullanarak bir ExpressRoute bağlantı hattı için eşlemesini değiştirme

Bu makalede, Resource Manager dağıtım modelinde PowerShell kullanarak bir expressroute bağlantı hattı için yönlendirme yapılandırması oluşturma ve yönetme yardımcı olur. Ayrıca, durumu, güncelleştirme veya silme denetleyin ve bir expressroute bağlantı hattı için eşlemeler sağlamayı sonlandırın. Bağlantı hattınız ile çalışmak için farklı bir yöntem kullanmak istiyorsanız, bir makale aşağıdaki listeden seçin:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - özel eşliği](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - ortak eşleme](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft eşlemesi](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Azure Resource Manager PowerShell cmdlet'lerinin en yeni sürümünü gerekir. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview). 
* Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Expressroute bağlantı hattı, bu makaledeki cmdlet'leri çalıştırılabilmesi sağlanmış ve etkin durumda olması gerekir.

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle gibi bir IPVPN MPLS), bağlantı sağlayıcınız yapılandırmak ve yönetmek için yönlendirme.

> [!IMPORTANT]
> Şu anda hizmet yönetim portalı aracılığıyla hizmet sağlayıcılar tarafından yapılandırılan eşlemeleri tanıtmıyoruz. Bu özelliği yakında etkinleştirmek için çalışıyoruz. BGP eşlemelerini yapılandırmadan önce hizmet sağlayıcınıza başvurun.
> 
> 

Bir ExpressRoute bağlantı hattı için bir, iki veya üç eşlemenin tamamını (Azure özel, Azure ortak ve Microsoft) yapılandırabilirsiniz. Eşlemeleri seçtiğiniz herhangi bir sırayla yapılandırabilirsiniz. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. Yönlendirme etki alanları ve eşlemeleri hakkında daha fazla bilgi için bkz: [ExpressRoute Yönlendirme etki alanları](expressroute-circuit-peerings.md).

## <a name="msft"></a>Microsoft eşlemesi

Bu bölümde, oluşturma, alma, güncelleştirme ve bir expressroute bağlantı hattı için Microsoft eşleme yapılandırmasını silme yardımcı olur.

> [!IMPORTANT]
> 1 Ağustos 2017 önce yapılandırılmış olan ExpressRoute bağlantı hatları, Microsoft eşlemesi yol filtreleri tanımlanmamış olsa bile eşleme, Microsoft tarafından tanıtılan tüm hizmet ön eklerin sahip olur. Ve 1 Ağustos 2017 sonrasında yapılandırılmış ExpressRoute bağlantı hatları, Microsoft eşlemesi sahip olmaz tüm ön eklerin rota filtre devresine bağlanana kadar tanıtılan. Daha fazla bilgi için bkz: [Microsoft eşliği için rota filtresini Yapılandır](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. ExpressRoute için PowerShell modülünü içeri aktarın.

  ExpressRoute cmdlet’lerini kullanmaya başlamak için [PowerShell Galerisi](http://www.powershellgallery.com/)’nden en yeni PowerShell yükleyicisini yüklemeniz ve Azure Resource Manager modüllerini PowerShell oturumuna aktarmanız gerekir. PowerShell'i Yönetici olarak çalıştırmanız gerekir.

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  Tüm bilinen semantik sürüm aralığı içinde AzureRM.* modüllerini içeri aktarın.

  ```powershell
  Import-AzureRM
  ```

  Aynı zamanda yalnızca bilinen semantik sürüm aralığı içinde seçili bir modülü içeri aktarabilirsiniz.

  ```powershell
  Import-Module AzureRM.Network
  ```

  Hesabınızda oturum açın.

  ```powershell
  Connect-AzureRmAccount
  ```

  Expressroute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. ExpressRoute bağlantı hattı oluşturun.

  Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-arm.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. f bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunar, bağlantı sağlayıcınızdan sizin için eşlemeyi Microsoft etkinleştirmesini isteyin. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için hattınızı oluşturduktan sonra yönetmiyorsa yapılandırmanızı sonraki adımlarla devam edin. 

3. Sağlanan ve ayrıca etkin emin olmak için expressroute bağlantı hattı denetleyin. Şu örneği kullanın:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  Yanıt aşağıdaki örneğe benzer:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Bağlantı hattı için Microsoft eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

  * Bir/30 veya /126 alt ağı birincil bağlantı için. Bu size ait ve bir RIR kayıtlı bir geçerli ortak IPv4 veya IPv6 öneki olmalıdır / IRR.
  * Bir/30 veya /126 alt ağı ikincil bağlantı için. Bu size ait ve bir RIR kayıtlı bir geçerli ortak IPv4 veya IPv6 öneki olmalıdır / IRR.
  * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
  * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
  * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm öneklerin bir listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Bir ön ek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu ön ekler size bir RIR / IRR içinde kaydedilmiş olmalıdır. IPv4, IPv6 önekleri tanıtılan önekler ve IPv6 BGP oturumları iste tanıtılan IPv4 BGP oturumları gerektirir. 
  * Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.
  * İsteğe bağlı:
    * Müşteri ASN’si: Eşleme AS numarasına kayıtlı olmayan önekler tanıtıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz.
    * Kullanmayı seçerseniz bir MD5 karma değeri.

  Microsoft bağlantı hattınız için eşlemesini yapılandırmak için aşağıdaki örneği kullanın:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv4 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv6 -PrimaryPeerAddressPrefix "3FFE:FFFF:0:CD30::/126" -SecondaryPeerAddressPrefix "3FFE:FFFF:0:CD30::4/126" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "3FFE:FFFF:0:CD31::/120" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="getmsft"></a>Microsoft eşlemesi ayrıntılarını almak için

Aşağıdaki örnek kullanarak yapılandırma ayrıntılarını alabilirsiniz:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="updatemsft"></a>Microsoft eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki örnek kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz:

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv4 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv6 -PrimaryPeerAddressPrefix "3FFE:FFFF:0:CD30::/126" -SecondaryPeerAddressPrefix "3FFE:FFFF:0:CD30::4/126" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "3FFE:FFFF:0:CD31::/120" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="deletemsft"></a>Microsoft eşlemesini silmek için

Aşağıdaki cmdlet'i çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz:

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="private"></a>Azure özel eşleme

Bu bölümde, oluşturma, alma, güncelleştirme ve bir expressroute bağlantı hattı için Azure özel eşleme yapılandırmasını silme yardımcı olur.

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. ExpressRoute için PowerShell modülünü içeri aktarın.

  ExpressRoute cmdlet’lerini kullanmaya başlamak için [PowerShell Galerisi](http://www.powershellgallery.com/)’nden en yeni PowerShell yükleyicisini yüklemeniz ve Azure Resource Manager modüllerini PowerShell oturumuna aktarmanız gerekir. PowerShell'i Yönetici olarak çalıştırmanız gerekir.

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  Tüm bilinen semantik sürüm aralığı içinde AzureRM.* modüllerini içeri aktarın.

  ```powershell
  Import-AzureRM
  ```

  Aynı zamanda yalnızca bilinen semantik sürüm aralığı içinde seçili bir modülü içeri aktarabilirsiniz.

  ```powershell
  Import-Module AzureRM.Network 
  ```

  Hesabınızda oturum açın.

  ```powershell
  Connect-AzureRmAccount
  ```

  Expressroute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. ExpressRoute bağlantı hattı oluşturun.

  Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-arm.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 Hizmetleri sunuyorsa, bağlantı sağlayıcınızdan özel sizin için Azure eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için hattınızı oluşturduktan sonra yönetmiyorsa yapılandırmanızı sonraki adımlarla devam edin.

3. Sağlanan ve ayrıca etkin emin olmak için expressroute bağlantı hattı denetleyin. Şu örneği kullanın:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  Yanıt aşağıdaki örneğe benzer:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Bağlantı hattı için Azure özel eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

  * Birincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
  * İkincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
  * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
  * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. Bu eşleme için özel bir AS numarası kullanabilirsiniz. 65515’i kullanmadığınızdan emin olun.
  * İsteğe bağlı:
    * Kullanmayı seçerseniz bir MD5 karma değeri.

  Bağlantı hattınız için Azure özel eşlemesini yapılandırmak için aşağıdaki örneği kullanın:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  Bir MD5 karma değeri kullanmayı seçerseniz, aşağıdaki örneği kullanın:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > AS numaranızı müşteri ASN’si değil eşleme ASN’si olarak belirttiğinizden emin olun.
  > 
  >

### <a name="getprivate"></a>Azure özel eşleme ayrıntılarını almak için

Aşağıdaki örnek kullanarak yapılandırma ayrıntılarını alabilirsiniz:

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
```

### <a name="updateprivate"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki örnek kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Bu örnekte, bağlantı hattının VLAN kimliği 100'den 500'e güncelleştiriliyor.

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="deleteprivate"></a>Azure özel eşlemesini silmek için

Aşağıdaki örnek çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz:

> [!WARNING]
> Bu örneği çalıştırmadan önce tüm sanal ağları expressroute bağlantı hattı bağlantısız olduğundan emin olmalısınız. 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="public"></a>Azure ortak eşleme

Bu bölümde, oluşturma, alma, güncelleştirme ve bir expressroute bağlantı hattı için Azure ortak eşleme yapılandırmasını silme yardımcı olur.

### <a name="to-create-azure-public-peering"></a>Azure ortak eşlemesi oluşturmak için

1. ExpressRoute için PowerShell modülünü içeri aktarın.

  ExpressRoute cmdlet’lerini kullanmaya başlamak için [PowerShell Galerisi](http://www.powershellgallery.com/)’nden en yeni PowerShell yükleyicisini yüklemeniz ve Azure Resource Manager modüllerini PowerShell oturumuna aktarmanız gerekir. PowerShell'i Yönetici olarak çalıştırmanız gerekir.

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  Tüm bilinen semantik sürüm aralığı içinde AzureRM.* modüllerini içeri aktarın.

  ```powershell
  Import-AzureRM
  ```

  Aynı zamanda yalnızca bilinen semantik sürüm aralığı içinde seçili bir modülü içeri aktarabilirsiniz.

  ```powershell
  Import-Module AzureRM.Network
```

  Hesabınızda oturum açın.

  ```powershell
  Connect-AzureRmAccount
  ```

  Expressroute bağlantı hattı oluşturmak istediğiniz aboneliği seçin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. ExpressRoute bağlantı hattı oluşturun.

  Bir [ExpressRoute bağlantı hattı](expressroute-howto-circuit-arm.md) oluşturmak için yönergeleri izleyin ve bağlantı sağlayıcısından bağlantı hattını sağlamasını isteyin. Bağlantı sağlayıcınız yönetilen Katman 3 Hizmetleri sunuyorsa, bağlantı sağlayıcınızdan Azure ortak sizin için eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için hattınızı oluşturduktan sonra yönetmiyorsa yapılandırmanızı sonraki adımlarla devam edin.

3. Sağlanan ve ayrıca etkin emin olmak için expressroute bağlantı hattı denetleyin. Şu örneği kullanın:

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  Yanıt aşağıdaki örneğe benzer:

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                      "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. Bağlantı hattı için Azure ortak eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

  * Birincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
  * İkincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
  * Bu eşlemenin kurulacak geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
  * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
  * İsteğe bağlı:
    * Kullanmayı seçerseniz bir MD5 karma değeri.

  Bağlantı hattınız için Azure ortak eşlemesini yapılandırmak için aşağıdaki örneği çalıştırma

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  Bir MD5 karma değeri kullanmayı seçerseniz, aşağıdaki örneği kullanın:

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > AS numaranızı müşteri ASN’si değil eşleme ASN’si olarak belirttiğinizden emin olun.
  > 
  >

### <a name="getpublic"></a>Azure ortak eşleme ayrıntılarını almak için

Aşağıdaki cmdlet'i kullanarak yapılandırma ayrıntılarını alabilirsiniz:

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="updatepublic"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Aşağıdaki örnek kullanarak yapılandırmanın herhangi bir bölümünü güncelleştirebilirsiniz. Bu örnekte, bağlantı hattının VLAN kimliği 200'den 600 olarak güncelleştiriliyor.

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="deletepublic"></a>Azure ortak eşlemesini silmek için

Aşağıdaki örnek çalıştırarak eşleme yapılandırmanızı kaldırabilirsiniz:

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, [ExpressRoute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-arm.md).

* ExpressRoute iş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
* Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md).
