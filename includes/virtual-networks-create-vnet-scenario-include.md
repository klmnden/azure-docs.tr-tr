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
ms.openlocfilehash: 2b1f9990985951a4e6ef260954968c0e1466c298
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60597028"
---
## <a name="scenario"></a>Senaryo

Bir VNet ve alt ağlar oluşturmak nasıl göstermek için bu belge aşağıdaki senaryoyu kullanır:

![VNet senaryosu](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

Bu senaryoda, bir VNet oluşturma **TestVNet**, ayrılmış CIDR bloğu ile **192.168.0.0./16**. Sanal ağ şu alt ağlar içerir: 

* CIDR bloğu olarak **192.168.1.0/24** kullanan **Ön Uç**.
* CIDR bloğu olarak **192.168.2.0/24** kullanan **rka Uç**.