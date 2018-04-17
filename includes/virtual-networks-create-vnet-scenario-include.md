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
ms.openlocfilehash: 29b2ac1a5dc128028dbd40e683c1b45e6208d124
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
## <a name="scenario"></a>Senaryo

VNet ve alt ağları oluşturma göstermek için bu belge aşağıdaki senaryoyu kullanır:

![VNet senaryosu](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Bu senaryoda, bir VNet oluşturma **TestVNet**, ayrılmış CIDR bloğu ile **192.168.0.0./16**. VNet aşağıdaki alt ağlar içerir: 

* CIDR bloğu olarak **192.168.1.0/24** kullanan **Ön Uç**.
* CIDR bloğu olarak **192.168.2.0/24** kullanan **rka Uç**.

