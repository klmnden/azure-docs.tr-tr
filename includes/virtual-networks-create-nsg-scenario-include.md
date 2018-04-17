---
title: include dosyası
description: include dosyası
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 09c6871fc5243296da2f2defd594afb80c62ac95
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
## <a name="scenario"></a>Senaryo
Nsg'ler oluşturmak nasıl daha iyi anlamak için bu belge aşağıdaki senaryoyu kullanır:

![VNet senaryosu](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Bu senaryoda, her alt ağ için bir NSG oluşturma **TestVNet** aşağıdaki gibi sanal ağ: 

* **NSG ön uç**. Ön uç NSG uygulanır *ön uç* alt ağı ve iki kural içerir:    
  * **RDP kural**. RDP trafiğine izin verir *ön uç* alt ağ.
  * **Web kuralı**. HTTP trafiğine izin verir *ön uç* alt ağ.
* **NSG arka uç**. Arka uç NSG uygulanır *arka uç* alt ağı ve iki kural içerir:    
  * **SQL kural**. Yalnızca SQL trafiğe izin veren *ön uç* alt ağ.
  * **Web kuralı**. Tüm Internet trafiği bağlı engellediği *arka uç* alt ağ.

Bu kurallar birleşimi burada arka uç alt ağ yalnızca SQL için ön uç alt ağından gelen trafik alabilir ve ön uç alt Internet ile iletişim kurmak ve alma hiçbir Internet erişimi DMZ benzeri senaryosu, oluşturun yalnızca gelen HTTP isteklerini için.

