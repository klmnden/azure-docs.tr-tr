---
author: WenJason
ms.service: load-balancer
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/31/2018
ms.author: v-jay
ms.openlocfilehash: f0e575af51f952a80fe42102b033727713c75cf9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60398825"
---
## <a name="configuration-scenario"></a>Yapılandırma senaryosu

Bu senaryoda aşağıdaki şekilde gösterildiği gibi bir sanal ağ içinde iç yük dengeleyici oluşturacağız:

![İç yük dengeleyici senaryosu](./media/load-balancer-get-started-ilb-scenario-include/figure1.png)

Bu senaryo için yapılandırma aşağıdaki şekildedir:

* **DB1** ve **DB2** adlı iki sanal makine
* İç yük dengeleyici için uç noktalar
* Bir iç yük dengeleyici
