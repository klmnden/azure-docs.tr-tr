---
author: WenJason
ms.service: load-balancer
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/31/2018
ms.author: v-jay
ms.openlocfilehash: 459c99d1b45af9c98cc1a6d0d7dd7a9a04c824ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516972"
---
Azure Load Balancer bir Katman 4 (TCP, UDP) yük dengeleyicidir. Yük dengeleyici, gelen trafiği bulut hizmetlerindeki sağlıklı hizmet örnekleri veya bir yük dengeleyici kümesindeki sanal makineler arasında dağıtarak yüksek kullanılabilirlik sağlar. Ayrıca, Azure Load Balancer bu hizmetleri birden çok bağlantı noktasında, birden çok IP adresinde ya da her ikisinde birden sağlayabilir.

Bir yük dengeleyiciyi aşağıdakileri yapacak şekilde yapılandırabilirsiniz:

* Gelen İnternet trafiği yükünü sanal makineler (VM’ler) arasında dengeleme. Bu senaryodaki bir yük dengeleyiciye [İnternet’e yönelik yük dengeleyici](../articles/load-balancer/load-balancer-internet-overview.md) diyoruz.
* Trafiği bir sanal ağdaki (VNet) VM’ler arasında, bulut hizmetlerindeki VM’ler arasında veya şirket içi bilgisayarlar ile şirketler arası bir sanal ağdaki VM’ler arasında dengeleme. Bu senaryodaki bir yük dengeleyiciye [iç yük dengeleyici (ILB)](../articles/load-balancer/load-balancer-internal-overview.md) diyoruz.
* Dış trafiği belirli bir VM örneğine iletme.