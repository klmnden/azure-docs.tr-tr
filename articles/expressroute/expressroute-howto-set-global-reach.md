---
title: 'Küresel erişim - ExpressRoute yapılandırın: Azure | Microsoft Docs'
description: Bu makalede, yardımcı birlikte özel ağ arasında şirket içi ağlarınız ve Global erişim etkinleştirme yapmak için ExpressRoute bağlantı hattına bağlayın.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: jaredro
ms.custom: seodec18
ms.openlocfilehash: aff283da27197b11ee496faecdd8b69571d3547e
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821345"
---
# <a name="configure-expressroute-global-reach"></a>ExpressRoute Global Reach’i yapılandırma
Bu makalede PowerShell kullanarak ExpressRoute Global erişim yapılandırmanıza yardımcı olur. Daha fazla bilgi için [ExpressRouteRoute Global erişim](expressroute-global-reach.md).

Yapılandırmaya başlamadan önce aşağıdakileri onaylayın:

* Azure PowerShell'in en son sürümünü yüklediğinizi. Daha fazla bilgi için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/azurerm/install-azurerm-ps).
* ExpressRoute bağlantı hattı sağlama anladığınızı [iş akışları](expressroute-workflows.md).
* ExpressRoute devreleri, sağlanan bir durumda olduğunu.
* Bu Azure özel eşleme, ExpressRoute bağlantı hatları üzerinde yapılandırılır.  

### <a name="sign-in-to-your-azure-account"></a>Azure hesabınızda oturum açma
Yapılandırmayı başlatmak için Azure hesabınızda oturum açın. 

PowerShell Konsolunuzu yükseltilmiş ayrıcalıklarla açın ve ardından hesabınıza bağlanın. Komut için Azure hesabınızda oturum açma kimlik bilgileri ister.  

```powershell
Connect-AzureRmAccount
```

Birden çok Azure aboneliğiniz varsa, hesabın aboneliklerini denetleyin.

```powershell
Get-AzureRmSubscription
```

Kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

### <a name="identify-your-expressroute-circuits-for-configuration"></a>Yapılandırma için ExpressRoute devreleri tanımlayın
Desteklenen ülkeler yerleşime ve eşleme farklı konumlarda oluşturulan sürece her iki ExpressRoute bağlantı hatları arasında ExpressRoute Global erişim etkinleştirebilirsiniz. Her iki bağlantı hatları, aboneliğin sahibi, aşağıdaki bölümlerde yapılandırması çalıştırmak için her iki bağlantı hattı seçebilirsiniz. 

İki bağlantı hatlarının farklı Azure aboneliklerinde ise bir Azure aboneliği onayı gerekir. Diğer Azure aboneliğinde yapılandırma komutu çalıştırdığınızda yetkilendirme anahtar geçirin.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınızı arasındaki bağlantıyı etkinleştir

Bağlantı hattı 1 ve 2 bağlantı hattı almak için aşağıdaki komutları kullanın. İki bağlantı hatları, aynı abonelikte yer alan.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

1 bağlantı hattı karşı aşağıdaki komutu çalıştırın ve 2 numaralı bağlantı hattının özel eşleme kimliği geçirebilirsiniz. Komutu çalıştırdığınızda aşağıdakilere dikkat edin:

* Özel Eşleme Kimliği, aşağıdaki örneğe benzer: 

  ```
  /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering
  ```
* *-AddressPrefix* bir/29 IPv4 olmalıdır alt ağ, örneğin, "10.0.0.0/29". IP adreslerini bu alt ağda iki ExpressRoute bağlantı hatları arasında bağlantı kurmak için kullanırız. Azure sanal ağlarınıza veya şirket içi ağınızdaki bu alt ağda adresleri kullanmamalısınız.

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

Yapılandırma 1 bağlantı hattı üzerinde şu şekilde kaydedin:
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Önceki işlemi tamamlandıktan sonra iki ExpressRoute bağlantı hatları aracılığıyla her iki tarafında, şirket içi ağlar arasında bağlantı olmalıdır.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute işlem hatları

İki bağlantı hatlarının aynı Azure aboneliğinde emin değilseniz, yetki vermeniz gerekir. Aşağıdaki yapılandırma, yetkilendirme bağlantı hattı 2 abonelikte oluşturulur ve yetkilendirme anahtar 1 işlem hattına geçirilen.

Bir yetkilendirme anahtar oluşturun. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Yetkilendirme anahtarı yanı sıra, bağlantı hattı 2 özel eşleme Kimliğini not edin.

1 bağlantı hattı karşı aşağıdaki komutu çalıştırın. Bağlantı hattının özel eşleme kimliği 2 ve yetkilendirme anahtarını geçirirsiniz.
```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

1 bağlantı hattındaki yapılandırmayı kaydedin.
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Önceki işlemi tamamlandıktan sonra iki ExpressRoute bağlantı hatları aracılığıyla her iki tarafında, şirket içi ağlar arasında bağlantı olmalıdır.

## <a name="get-and-verify-the-configuration"></a>Alma ve yapılandırmayı doğrulama

(Örneğin, önceki örnekte devre 1) yapılandırma yapıldığı devredeki yapılandırmasını doğrulamak için aşağıdaki komutu kullanın.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Yalnızca çalıştırırsanız *$ckt1* PowerShell'de gördüğünüz *CircuitConnectionStatus* çıktı. Bu, bağlantı olup olmadığı belirlenen, "Bağlı" veya "Bağlantısı" bildirir. 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınız arasında bağlantı devre dışı bırak

Bağlantı devre dışı bırakmak için (örneğin, önceki örnekte devre 1) yapılandırma yapıldığı karşı bağlantı hattını komutları çalıştırın.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Durumu doğrulamak için alma işlemini çalıştırabilirsiniz. 

Önceki işlem tamamlandıktan sonra artık bağlantı, ExpressRoute bağlantı hatları aracılığıyla şirket içi ağınız arasında yok. 


## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Global erişim hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
2. [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
3. [Bir Azure sanal ağı ExpressRoute bağlantı hattına bağlama](expressroute-howto-linkvnet-arm.md)


