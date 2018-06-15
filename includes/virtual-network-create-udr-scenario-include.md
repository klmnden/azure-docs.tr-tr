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
ms.openlocfilehash: b91ae155761f6357e286f4742d57b97cf96d909a
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31805173"
---
## <a name="scenario"></a>Senaryo
Udr'ler oluşturmak nasıl daha iyi anlamak için bu belge aşağıdaki senaryoyu kullanır:

![GÖRÜNTÜ AÇIKLAMASI](./media/virtual-network-create-udr-scenario-include/figure1.png)

Bu senaryoda, bir UDR için oluşturduğunuz *ön uç alt* ve için başka bir UDR *arka uç alt*aşağıdaki gibi: 

* **Ön uç UDR**. Ön uç UDR uygulanan *ön uç* alt ağı ve bir rota içerir:    
  * **RouteToBackend**. Bu yol arka uç alt ağına tüm trafiği gönderir **FW1** sanal makine.
* **Arka uç UDR**. Arka uç UDR uygulanan *arka uç* alt ağı ve bir rota içerir:    
  * **RouteToFrontend**. Bu yol ön uç alt ağına tüm trafiği gönderir **FW1** sanal makine.

Bu yollar bileşimi için bir alt ağdan diğerine giden tüm trafiği yönlendirilmesini sağlar **FW1** sanal gereç olarak kullanılan sanal makine. Ayrıca IP için iletmeyi etkinleştirmek gereken **FW1** diğer VM'ler için giden trafiğe aldığınızdan emin olmak için VM.

