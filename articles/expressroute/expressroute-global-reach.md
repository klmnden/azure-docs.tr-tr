---
title: Azure ExpressRoute küresel erişim | Microsoft Docs
description: Bu makalede, yardımcı birlikte özel ağ arasında şirket içi ağlarınız ve Global erişim etkinleştirme yapmak için ExpressRoute bağlantı hattına bağlayın.
documentationcenter: na
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 90f34da10e0193e31a0f70187463799013f52be8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966824"
---
# <a name="link-expressroute-circuits-to-enable-expressroute-global-reach-preview"></a>ExpressRoute Global erişim (Önizleme) etkinleştirmek için ExpressRoute bağlantı hattına bağlama

ExpressRoute, Microsoft Cloud, şirket içi ağlara bağlanmak için özel ve esnek bir yoludur. Özel veri Merkezinize veya şirket ağınızda Azure, Office 365 ve Dynamics 365 gibi pek çok Microsoft bulut hizmetlerine erişebilir. Örneğin, bir ExpressRoute bağlantı hattı Silikon ve başka bir şube ofisinde Baltimore ile ExpressRoute devresi, Washington DC, San Francisco bir şube ofisi olabilir. 

Her iki şube ofisleri yüksek hızlı bağlantı hem Doğu ABD ve Batı ABD Azure kaynaklarına sahip olabilir. Ancak, şube ofislerinde doğrudan birbirleri ile veri değişimi olamaz. Diğer bir deyişle, 10.0.1.0/24 10.0.3.0/24 ve 10.0.4.0/24 ancak değil 10.0.2.0/24 ile veri alışverişinde bulunabilir.

![Olmadan][1]

İle **ExpressRoute Global erişim**, birlikte şirket içi ağlarınız arasında bir özel veri ağı yapmak için ExpressRoute bağlantı hattına bağlayabilirsiniz. Yukarıdaki örnekte, ek olarak, ExpressRoute Global erişim, İstanbul ofis (10.0.1.0/24) verilerle Baltimore office (10.0.2.0/24) var olan ExpressRoute bağlantı hatları aracılığıyla ve global Microsoft ağı üzerinden doğrudan değiştirebilir. 

![ile][2]

ExpressRoute Global erişim şu anda aşağıdaki ülkelerde desteklenir:

* Amerika Birleşik Devletleri
* Birleşik Krallık 
* Japonya

Konumunda, ExpressRoute devreleri oluşturulması [ExpressRoute eşleme konumlarına](expressroute-locations.md) yukarıdaki ülkede. ExpressRoute Global erişim arasında etkinleştirmek için [farklı coğrafi bölgeler](expressroute-locations.md), Premium SKU, devreler olması gerekir.


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

## <a name="to-enable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınız arasında bağlantıyı etkinleştirmek için

Bağlantı hattı 1 ve 2 bağlantı hattı almak için aşağıdaki komutları kullanın. İki bağlantı hatları, aynı abonelikte yer alan.

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
```

1 bağlantı hattı karşı aşağıdaki komutu çalıştırın ve bağlantı hattı 2 özel ait eşleme kimliği geçirin

Özel ait eşleme kimliği şu şekilde görünür olduğunu unutmayın:

`/subscriptions/{your_subscription_id}/resourceGroups/{your_resource_group}/providers/Microsoft.Network/expressRouteCircuits/{your_circuit_name}/peerings/AzurePrivatePeering`


```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering $ckt_2.Peerings[0].Id -AddressPrefix '__.__.__.__/29'
```

*- AddressPrefix* bir/29 IPv4 olmalıdır alt ağ, örneğin "10.0.0.0/29". IP adreslerini bu alt ağda iki ExpressRoute bağlantı hatları arasında bağlantı kurmak için kullanacağız. Adresleri bu alt ağda Azure Vnet'ler veya şirket içi ağlarınızı kullanmamanız gerekir. 


1 bağlantı hattındaki yapılandırmayı kaydedin.

```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Önceki işlemi tamamlandıktan sonra iki ExpressRoute bağlantı hatları aracılığıyla her iki tarafında, şirket içi ağlar arasında bağlantı olmalıdır.

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>Farklı Azure aboneliklerinde ExpressRoute işlem hatları

İki bağlantı hatlarının aynı Azure aboneliğinde emin değilseniz, yetkilendirme gerekir. Aşağıdaki yapılandırmasında, yetkilendirme, bağlantı hattı 2 abonelikte oluşturulur ve yetkilendirme anahtarını 1 işlem hattına geçirilen.

Bir yetkilendirme anahtar oluşturun. 
```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```
Bağlantı hattı 2 özel ait eşleme kimliği yanı sıra, yetkilendirme anahtarını not edin.

1 bağlantı hattı karşı aşağıdaki komutu çalıştırın. Bağlantı hattı 2 özel ait eşleme kimliği ve yetkilendirme anahtarını geçirin

```powershell
Add-AzureRmExpressRouteCircuitConnectionConfig -Name 'Your_connection_name' -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix '__.__.__.__/29' -AuthorizationKey '########-####-####-####-############'
```

1 bağlantı hattındaki yapılandırmayı kaydedin.

```powershell
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Önceki işlem tamamlandığında, şirket içi ağlarınız ile iki ExpressRoute bağlantı hatları her iki tarafında arasında bağlantı olmalıdır.

## <a name="to-get-and-verify-the-configuration"></a>Alma ve yapılandırmayı doğrulamak için

Yapılandırma yapıldığı devredeki yapılandırmasını doğrulamak için aşağıdaki komutu kullanın. Bu örnekte, bağlantı hattı 1 önceki örnekte kullanılmaktadır.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
```

Yalnızca çalıştırırsanız *$ckt1* PowerShell'de göreceğiniz *CircuitConnectionStatus* çıktı. Bu size olup, veya değil, "bağlı" ile bağlantı bildirir "Bağlantısı kesildi". 

## <a name="to-disable-connectivity-between-your-on-premises-networks"></a>Şirket içi ağlarınız arasında bağlantı devre dışı bırakmak için

Devre dışı bırakmak için yapılandırma yapıldığı karşı bağlantı hattını komutları çalıştırın. Bu örnek, önceki örneklerle devre 1 kullanır.

```powershell
$ckt1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Remove-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Durumu doğrulamak için alma işlemini çalıştırabilirsiniz. 

Yukarıdaki işlem tamamlandıktan sonra ExpressRoute bağlantı hatları aracılığıyla şirket içi ağınız arasında bağlantı artık yoktur. 

## <a name="next-steps"></a>Sonraki adımlar
1. [ExpressRoute Global erişim hakkında daha fazla bilgi edinin](expressroute-faqs.md#globalreach)
2. [ExpressRoute bağlantısını doğrulama](expressroute-troubleshooting-expressroute-overview.md)
3. [ExpressRoute bağlantı hattı için Azure sanal ağı bağlama](expressroute-howto-linkvnet-arm.md)


<!--Image References-->
[1]: ./media/expressroute-global-reach/1.png "Küresel erişim olmadan diyagramı"
[2]: ./media/expressroute-global-reach/2.png "Küresel erişime sahip diyagramı"