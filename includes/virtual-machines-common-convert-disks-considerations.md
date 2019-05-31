---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 9f7e2760ef8bf06a2e680dce90c323672ca9d491
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416073"
---
* Dönüştürme işlemi VM’nin yeniden başlatılmasını gerektirir, bu nedenle VM'lerinizin geçişini önceden var olan bir bakım penceresi sırasında zamanlayın. 

* Bu dönüştürme geri alınamaz. 

* Herhangi bir unutmayın kullanıcılarla [sanal makine Katılımcısı](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol (ön dönüştürme verebilir gibi) VM boyutunu değiştirmek mümkün olmayacaktır. Yönetilen disklere sahip VM'ler işletim sistemi disklerinde Microsoft.Compute/disks/write izninin gerektirmek olmasıdır.

* Dönüştürmeyi test ettiğinizden emin olun. Üretimde geçişi gerçekleştirmeden önce bir sınama sanal makinesini geçirin.

* Dönüştürme sırasında VM’yi serbest bırakın. VM, dönüştürmeden sonra başlatıldığında yeni bir IP adresi alır. Gerekirse VM’ye [statik bir IP adresi atayabilirsiniz](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

* Azure VM Aracısı dönüştürme işlemini desteklemek için gereken en düşük sürümünü gözden geçirin. Denetleyin ve aracınızın sürümü güncelleştirme konusunda daha fazla bilgi için bkz: [Azure VM aracıları için Minimum sürüm desteği](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)