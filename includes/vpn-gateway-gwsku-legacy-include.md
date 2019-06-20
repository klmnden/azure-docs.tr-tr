---
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/09/2018
ms.author: cherylmc
ms.openlocfilehash: f2ee442d0d6fecf34449ad28f058615a1274bbea
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188259"
---
Eski VPN ağ geçidi SKU'ları şunlardır:

* Temel
* Standart
* HighPerformance

VPN Gateway, UltraPerformance ağ geçidi SKU’sunu kullanmaz. UltraPerformance SKU’su hakkında bilgi edinmek için [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md) belgelerine bakın.

Eski SKU'lar ile çalışırken aşağıdakileri göz önünde bulundurun:

* PolicyBased VPN türünü kullanmak istiyorsanız Temel SKU'yu kullanmanız gerekir. PolicyBased VPN'ler (daha önce Statik Yönlendirme olarak biliniyordu), diğer SKU’larda desteklenmez.
* BGP, Temel SKU’da desteklenmez.
* ExpressRoute-VPN Gateway ağ geçidi bir arada var olabilen yapılandırmaları, temel SKU'da desteklenmez.
* Etkin-etkin S2S VPN Gateway bağlantıları, yalnızca HighPerformance değerine sahip SKU'larda yapılandırılabilir.