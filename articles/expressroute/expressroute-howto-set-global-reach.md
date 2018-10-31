---
title: Azure ExpressRoute küresel erişimi yapılandırma | Microsoft Docs
description: Bu makalede, yardımcı birlikte özel ağ arasında şirket içi ağlarınız ve Global erişim etkinleştirme yapmak için ExpressRoute bağlantı hattına bağlayın.
documentationcenter: na
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: mialdrid
ms.openlocfilehash: 67fbf9dc430d615efe3ef894add1a26bbce792bc
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50237987"
---
# <a name="configure-expressroute-global-reach-preview"></a>ExpressRoute küresel erişim (Önizleme) yapılandırma
Bu makalede PowerShell kullanarak ExpressRoute Global erişim yapılandırmanıza yardımcı olur. Daha fazla bilgi için [ExpressRouteRoute Global erişim](expressroute-global-reach.md).
 
## <a name="before-you-begin"></a>Başlamadan önce
> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 


Yapılandırmaya başlamadan önce aşağıdaki gereksinimleri denetlemek gerekir.

* Azure PowerShell'in en son sürümünü yükleyin. Bkz: [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/install-azurerm-ps).
* ExpressRoute bağlantı hattı sağlama anlamak [iş akışları](expressroute-workflows.md).
* ExpressRoute devreleri sağlanan durumda olduğundan emin olun.
* Azure özel eşdüzey hizmet sağlama, ExpressRoute bağlantı hatları üzerinde yapılandırıldığından emin olun.  

### <a name="log-into-your-azure-account"></a>Azure hesabınızda oturum açın
Yapılandırmayı başlatmak için Azure hesabınızda oturum açmalısınız. 

PowerShell konsolunuzu yükseltilmiş ayrıcalıklarla açın ve hesabınıza bağlanın. Komutu, Azure hesabınızda oturum açma kimlik bilgileri için istemde bulunur.  

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
ExpressRoute Global erişim herhangi iki ExpressRoute bağlantı hatları arasında eşleme farklı konumlarda oluşturuldukları ve desteklenen ülkede bulunan sürece etkinleştirebilirsiniz. Her iki bağlantı hatları, aboneliğin sahibi, aşağıdaki bölümlerde yapılandırması çalıştırmak için her iki bağlantı hattı seçebilirsiniz. İki bağlantı hatlarının farklı Azure aboneliklerinde ise bir Azure aboneliğinden yetki vermeniz gerekir ve diğer Azure aboneliğinde yapılandırma komutu çalıştırdığınızda, yetkilendirme anahtar geçirin.

## <a name="enable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınızı arasındaki bağlantıyı etkinleştir

Bağlantı hattı 1 ve 2 bağlantı hattı almak için aşağıdaki komutları kullanın. İki bağlantı hatları, aynı abonelikte yer alan.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

1 bağlantı hattı karşı aşağıdaki komutu çalıştırın ve bağlantı hattı 2 özel ait eşleme kimliği geçirin

> [!NOTE]
> Özel Eşleme Kimliği aşağıdaki gibi görünür kullanıcının */subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering*
> 
>

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

> [!IMPORTANT]
> *-AddressPrefix* bir/29 IPv4 olmalıdır alt ağ, örneğin "10.0.0.0/29". IP adreslerini bu alt ağda iki ExpressRoute bağlantı hatları arasında bağlantı kurmak için kullanacağız. Adresleri bu alt ağda Azure Vnet'ler veya şirket içi ağlarınızı kullanmamanız gerekir.
> 



1 bağlantı hattındaki yapılandırmayı kaydedin
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Yukarıdaki işlem tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantı olmalıdır.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute işlem hatları

İki bağlantı hatlarının aynı Azure aboneliğinde emin değilseniz, yetkilendirme gerekir. Aşağıdaki yapılandırmasında, yetkilendirme, bağlantı hattı 2 abonelikte oluşturulur ve yetkilendirme anahtarını 1 işlem hattına geçirilen.

Bir yetkilendirme anahtar oluşturun. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Bağlantı hattı 2 özel ait eşleme kimliği hem de yetkilendirme anahtarını not edin.

1 bağlantı hattı karşı aşağıdaki komutu çalıştırın. Bağlantı hattı 2 özel ait eşleme kimliği ve yetkilendirme anahtarını geçirin 
```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

1 bağlantı hattındaki yapılandırmayı kaydedin
```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Yukarıdaki işlem tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantı olmalıdır.

## <a name="get-and-verify-the-configuration"></a>Alma ve yapılandırmayı doğrulama

Burada yapılandırma yapıldı, yani devre 1 yukarıdaki örnekte devredeki yapılandırmasını doğrulamak için aşağıdaki komutu kullanın.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Yalnızca çalıştırırsanız *$ckt1* göreceksiniz PowerShell'de *CircuitConnectionStatus* çıktı. Bu size olup, veya değil, "bağlı" ile bağlantı bildirir "Bağlantısı kesildi". 

## <a name="disable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınız arasında bağlantı devre dışı bırak

Devre dışı bırakmak için burada yapılandırma yapıldı, yani devre 1 yukarıdaki örnekte karşı bağlantı hattını komutları çalıştırın.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Durumu doğrulamak için alma işlemini çalıştırabilirsiniz. 

Yukarıdaki işlem tamamlandıktan sonra ExpressRoute bağlantı hatları aracılığıyla şirket içi ağınız arasında bağlantı artık yoktur. 


## <a name="next-steps"></a>Sonraki Adımlar
1. [ExpressRoute Global erişim hakkında daha fazla bilgi edinin](expressroute-global-reach.md)
2. [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
3. [ExpressRoute bağlantı hattı için Azure sanal ağı bağlama](expressroute-howto-linkvnet-arm.md)


