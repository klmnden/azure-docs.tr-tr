---
title: Azure ExpressRoute eşlemeleri sıfırlama | Microsoft Docs
description: Devre dışı bırakma ve etkinleştirme peeerings ExpressRoute bağlantı hattının.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: charwen
ms.openlocfilehash: b8c9bc1944e9ed0281616062a84958c953d08694
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180897"
---
# <a name="reset-expressroute-peerings"></a>ExpressRoute eşlemeleri Sıfırla

Bu makalede devre dışı bırakma ve PowerShell kullanarak ExpressRoute bağlantı hattının eşlemeler etkinleştirin. Bir eşleme devre dışı bıraktığınızda, birincil bağlantı ve, ExpressRoute devresinin ikincil bağlantısı BGP oturumu kapatılır. Microsoft eşlemesi üzerinden bağlantınız kesilir. Bir eşleme etkinleştirdiğinizde, birincil bağlantı ve, ExpressRoute devresinin ikincil bağlantısı BGP oturumu getirdiği. Microsoft eşlemesi üzerinden bağlantı yeniden elde. Etkinleştirme ve Microsoft Peering ve Azure özel eşleme ExpressRoute devresi bağımsız olarak devre dışı bırakabilirsiniz. Eşlemeler, ExpressRoute devreniz eşlikleri ilk kez yapılandırırken, varsayılan olarak etkindir. 

Burada, ExpressRoute eşlemeleri sıfırlama yararlı birkaç senaryo vardır.
* Olağanüstü durum kurtarma tasarımı ve uygulaması test edin. Örneğin, iki ExpressRoute devreniz vardır. Bir devrenin eşlikleri devre dışı bırakabilir ve ağ trafiğinizin diğer devreye yük devretmek için zorla.
* Çift yönlü iletme algılama (BFD) Azure özel eşleme ExpressRoute bağlantı hattı, şirket etkinleştirin. ExpressRoute devreniz 1 Ağustos 2018'den sonra oluşturduysanız BFD varsayılan olarak etkindir. Bağlantı hattınız önce oluşturulduysa BFD etkin değildi. Eşlemeyi devre dışı bırakma ve bu bırakılmış BFD etkinleştirebilirsiniz. BFD Azure özel eşleme yalnızca desteklendiğini unutulmamalıdır.


## <a name="reset-a-peering"></a>Bir eşleme Sıfırla

1. Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps).

2. PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

  ```powershell
  Connect-AzureRmAccount
  ```
3. Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Kullanmak istediğiniz aboneliği belirtin.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. ExpressRoute devreniz almak için aşağıdaki komutları çalıştırın.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```
6. Devre dışı bırakmak veya etkinleştirmek istediğiniz eşlemeyi tanımlar. *Eşlemeler* bir dizidir. Aşağıdaki örnekte, Azure özel eşdüzey hizmet sağlama ve eşlemeler [1] Microsoft Peering eşlemeleri [0] olduğu.

  ```powershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
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
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
ServiceKey                       : ########-####-####-####-############
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : []
AllowClassicOperations           : False
GatewayManagerEtag               :
  ```
7. Eşleme durumunu değiştirmek için aşağıdaki komutları çalıştırın.

  ```powershell
  $ckt.Peerings[0].State = "Disabled"
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```
Eşleme, ayarladığınız bir durumda olması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute sorunu gidermek için yardıma ihtiyacınız varsa şu makalelere bakın:
* [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
* [Ağ performansı sorunlarını giderme](expressroute-troubleshooting-network-performance.md)
