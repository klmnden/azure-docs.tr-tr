---
author: kumudD
ms.service: load-balancer
ms.topic: include
ms.date: 11/09/2018
ms.author: kumud
ms.openlocfilehash: f0e575af51f952a80fe42102b033727713c75cf9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66122264"
---
## <a name="configuration-scenario"></a>Yapılandırma senaryosu

Bu senaryoda aşağıdaki şekilde gösterildiği gibi bir sanal ağ içinde iç yük dengeleyici oluşturacağız:

![İç yük dengeleyici senaryosu](./media/load-balancer-get-started-ilb-scenario-include/figure1.png)

Bu senaryo için yapılandırma aşağıdaki şekildedir:

* **DB1** ve **DB2** adlı iki sanal makine
* İç yük dengeleyici için uç noktalar
* Bir iç yük dengeleyici
