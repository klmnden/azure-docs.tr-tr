---
title: include dosyası
description: include dosyası
services: virtual-network
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 04/13/2018
ms.date: 06/11/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 588aa260f2ece543445bfd4da7ef4682dab8334c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309972"
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
