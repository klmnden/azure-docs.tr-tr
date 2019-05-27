---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8861396db6f6b680ddb55ce020e5579dc25b118e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66165413"
---
Azure'da bir kullanılabilirlik grubu dinleyicisi yapılandırmanın iki yolu olduğunu bilmeniz önemlidir. Yolu, dinleyici oluştururken kullandığınız Azure load balancer'ın türleri farklı. Aşağıdaki tabloda farklar açıklanmaktadır:

| Yük Dengeleyici türü | Uygulama | Şu durumlarda kullanın: |
| --- | --- | --- |
| **Dış** |Kullanan *genel sanal IP adresi* sanal makineleri (VM'ler) barındıran bulut hizmetinin. |Internet'ten de dahil olmak üzere sanal ağın dışında dinleyicisinden erişmeniz gerekir. |
| **İç** |Kullanan bir *iç yük dengeleyici* dinleyici için özel bir adresi. |Aynı sanal ağda yalnızca bir dinleyici erişebilirsiniz. Bu erişim, siteden siteye VPN karma senaryolar içerir. |

> [!IMPORTANT]
> Bulut hizmetinin genel VIP (Dış yük dengeleyici) istemci olarak uzun kullanan bir dinleyici için dinleyici ve veritabanları aynı Azure bölgesinde, çıkış ücreti alınmayacaktır. Aksi takdirde dinleyicisi döndürülen verileri, çıkış olarak kabul edilir ve normal veri aktarımı ücretlerine göre ücretlendirilir. 
> 
> 

Bir ILB yalnızca sanal ağların bölgesel bir kapsamla yapılandırılabilir. Bir benzeşim grubu için yapılandırılmış mevcut sanal ağlarınızı, ILB kullanamazsınız. Daha fazla bilgi için [iç yük dengeleyiciye genel bakış](../articles/load-balancer/load-balancer-internal-overview.md).

