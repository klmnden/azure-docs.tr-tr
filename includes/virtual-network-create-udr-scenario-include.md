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
ms.openlocfilehash: 40b81904daabfdad7e45571d8ab86cf32cac8964
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170859"
---
## <a name="scenario"></a>Senaryo
Udr'ler oluşturmak nasıl daha iyi anlamak için bu belge aşağıdaki senaryoyu kullanır:

![GÖRÜNTÜ AÇIKLAMASI](./media/virtual-network-create-udr-scenario-include/figure1.png)

Bu senaryoda, bir UDR için oluşturduğunuz *ön uç alt ağına* ve başka bir UDR için *arka uç alt ağı*gibi: 

* **UDR ön uç**. Ön uç UDR uygulanan *ön uç* alt ağ ve bir yol içerir:    
  * **RouteToBackend**. Bu rota tüm trafiği arka uç alt ağına gönderir **FW1** sanal makine.
* **UDR arka uç**. Arka uç UDR uygulanan *arka uç* alt ağ ve bir yol içerir:    
  * **RouteToFrontend**. Bu yol ön uç alt ağına tüm trafiği gönderir **FW1** sanal makine.

Bu yolları birleşimi bir alt ağdan diğerine hedefleyen tüm trafiği için yönlendirilmesini sağlar **FW1** sanal makine, bir sanal gereç kullanılır. Ayrıca için NIC'de IP etkinleştirmek gereken **FW1** diğer Vm'lere hedefleyen trafiği aldığınızdan emin olmak için VM.

