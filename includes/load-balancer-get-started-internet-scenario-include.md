---
author: WenJason
ms.service: load-balancer
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/31/2018
ms.author: v-jay
ms.openlocfilehash: bf3aee30460e052db23cf9090f13e2af3b22628b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516976"
---
Bu senaryoda aşağıdaki görevler gerçekleştirilecek:

* 80 bağlantı noktasında ağ trafiği alan ve "web1" ve "web2" sanal makinelerine yükü dengelenmiş trafik gönderen bir yük dengeleyici oluşturma
* Uzak masaüstü erişimi için NAT kuralları/yük dengeleyicinin arkasındaki sanal makineler için SSH oluşturma
* Sistem durumu araştırmaları oluşturma

![Yük dengeleyici senaryosu](./media/load-balancer-get-started-internet-scenario-include/scenario-classic.png)