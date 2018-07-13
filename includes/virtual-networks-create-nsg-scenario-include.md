---
title: include dosyası
description: include dosyası
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 873549442284ede2e9f020bd90879f721b9c1a18
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38760377"
---
## <a name="scenario"></a>Senaryo
Nsg'ler oluşturma daha iyi anlamak için bu belge aşağıdaki senaryoyu kullanır:

![VNet senaryosu](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

Bu senaryoda, her alt ağ için bir NSG oluşturmak **TestVNet** gösterildiği gibi sanal ağ: 

* **NSG ön uç**. Ön uç NSG'si uygulanan *ön uç* alt ağ ve iki kural içerir:    
  * **RDP kuralının**. RDP trafiğine izin verir *ön uç* alt ağ.
  * **Web kuralı**. HTTP trafiğe izin veren *ön uç* alt ağ.
* **NSG arka uç**. Arka uç NSG'si uygulanan *arka uç* alt ağ ve iki kural içerir:    
  * **SQL-rule**. Yalnızca SQL trafiğe izin veren *ön uç* alt ağ.
  * **Web kuralı**. Tüm İnternet'e bağlı trafiği engeller *arka uç* alt ağ.

Bu kurallar birleşimi oluşturma bir DMZ gibi senaryosu, burada arka uç alt ağı yalnızca SQL için ön uç alt ağından gelen trafiği alabilir ve ön uç alt ağına Internet ile iletişim kurmak ve almak, İnternet'e erişim yok yalnızca gelen HTTP istekleri için.

