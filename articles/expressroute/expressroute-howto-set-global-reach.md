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
ms.openlocfilehash: de9cbd9cfac766e2a67274684d3fb6b447e45200
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64572745"
---
# <a name="configure-expressroute-global-reach"></a>ExpressRoute Global Reach’i yapılandırma

Bu makalede PowerShell kullanarak ExpressRoute Global erişim yapılandırmanıza yardımcı olur. Daha fazla bilgi için [ExpressRouteRoute Global erişim](expressroute-global-reach.md).

 ## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmaya başlamadan önce aşağıdakileri onaylayın:

* ExpressRoute bağlantı hattı sağlama anlamak [iş akışları](expressroute-workflows.md).
* Sağlanan bir durumda, ExpressRoute bağlantı hatları var.
* Azure özel eşdüzey hizmet sağlama, ExpressRoute bağlantı hatları üzerinde yapılandırılır.
* PowerShell'i yerel olarak çalıştırmak istiyorsanız, Azure PowerShell'in en son sürümünü bilgisayarınızda yüklü olduğunu doğrulayın.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="identify-circuits"></a>Bağlantı hatlarını belirleyin

1. Yapılandırmayı başlatmak için Azure hesabınızda oturum açın ve kullanmak istediğiniz aboneliği seçin.

   [!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]
2. Kullanmak istediğiniz ExpressRoute bağlantı hatlarını belirleyin. Desteklenen ülkeler/bölgeler yerleşime ve eşleme farklı konumlarda oluşturulan sürece her iki ExpressRoute bağlantı hatları arasında ExpressRoute Global erişim etkinleştirebilirsiniz. 

   * Her iki bağlantı hatları, aboneliğin sahibi, aşağıdaki bölümlerde yapılandırması çalıştırmak için her iki bağlantı hattı seçebilirsiniz.
   * İki bağlantı hatlarının farklı Azure aboneliklerinde ise bir Azure aboneliği onayı gerekir. Diğer Azure aboneliğinde yapılandırma komutu çalıştırdığınızda yetkilendirme anahtar geçirin.

## <a name="enable-connectivity"></a>Bağlantı etkinleştir

Şirket içi ağlarınız arasında bağlantı sağlar. Aynı Azure aboneliğinde bulunan devreler ve farklı Aboneliklerde bulunan bağlantı hatları için yönerge ayrı ayarlar vardır.

### <a name="expressroute-circuits-in-the-same-azure-subscription"></a>Aynı Azure aboneliğinde ExpressRoute devreleri

1. Bağlantı hattı 1 ve 2 bağlantı hattı almak için aşağıdaki komutları kullanın. İki bağlantı hatları, aynı abonelikte yer alan.

   ```azurepowershell-interactive
   $ckt_1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
   $ckt_2 = Get-AzExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
   ```
2. 1 bağlantı hattı karşı aşağıdaki komutu çalıştırın ve 2 numaralı bağlantı hattının özel eşleme kimliği geçirebilirsiniz. Komutu çalıştırdığınızda aşağıdakilere dikkat edin:

   * Özel Eşleme Kimliği, aşağıdaki örneğe benzer: 

     ```
     /subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering
     ```
   * *-AddressPrefix* bir/29 IPv4 olmalıdır alt ağ, örneğin, "10.0.0.0/29". IP adreslerini bu alt ağda iki ExpressRoute bağlantı hatları arasında bağlantı kurmak için kullanırız. Azure sanal ağlarınıza veya şirket içi ağınızdaki bu alt ağda adresleri kullanmamalısınız.

     ```azurepowershell-interactive
     Add-AzExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
     ```
3. Yapılandırma 1 bağlantı hattı üzerinde şu şekilde kaydedin:

   ```azurepowershell-interactive
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
   ```

Önceki işlemi tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantısına sahip olur.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute işlem hatları

İki bağlantı hatlarının aynı Azure aboneliğinde emin değilseniz, yetki vermeniz gerekir. Aşağıdaki yapılandırma, yetkilendirme bağlantı hattı 2 abonelikte oluşturulur ve yetkilendirme anahtar 1 işlem hattına geçirilen.

1. Bir yetkilendirme anahtar oluşturun.

   ```azurepowershell-interactive
   $ckt_2 = Get-AzExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
   Add-AzExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_2
   ```

   Yetkilendirme anahtarı yanı sıra, bağlantı hattı 2 özel eşleme Kimliğini not edin.
2. 1 bağlantı hattı karşı aşağıdaki komutu çalıştırın. Bağlantı hattının özel eşleme kimliği 2 ve yetkilendirme anahtarını geçirirsiniz.

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
   ```
3. 1 bağlantı hattındaki yapılandırmayı kaydedin.

   ```azurepowershell-interactive
   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
   ```

Önceki işlemi tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantısına sahip olur.

## <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama

(Örneğin, önceki örnekte devre 1) yapılandırma yapıldığı devredeki yapılandırmasını doğrulamak için aşağıdaki komutu kullanın.
```azurepowershell-interactive
$ckt1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Yalnızca çalıştırırsanız *$ckt1* PowerShell'de gördüğünüz *CircuitConnectionStatus* çıktı. Bu, bağlantı olup olmadığı belirlenen, "Bağlı" veya "Bağlantısı" bildirir. 

## <a name="disable-connectivity"></a>Bağlantı devre dışı bırak

Yapılandırma yapıldığı karşı bağlantı hattını komutları çalıştırmak, şirket içi ağlar, (örneğin, bağlantı hattı 1 önceki örnekte) arasında bağlantı devre dışı bırakmak için.

```azurepowershell-interactive
$ckt1 = Get-AzExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Durumu doğrulamak için alma işlemini çalıştırabilirsiniz.

Önceki işlem tamamlandıktan sonra artık bağlantı, ExpressRoute bağlantı hatları aracılığıyla şirket içi ağınız arasında yok.

## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Global erişim hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
2. [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
3. [Bir Azure sanal ağı ExpressRoute bağlantı hattına bağlama](expressroute-howto-linkvnet-arm.md)
