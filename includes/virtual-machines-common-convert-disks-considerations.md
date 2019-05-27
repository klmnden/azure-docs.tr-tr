---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: bf26af7fa4b1b31514fb82c5e28a85154b2e274a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158476"
---
* Dönüştürme işlemi VM’nin yeniden başlatılmasını gerektirir, bu nedenle VM'lerinizin geçişini önceden var olan bir bakım penceresi sırasında zamanlayın. 

* Bu dönüştürme geri alınamaz. 

* Herhangi bir unutmayın kullanıcılarla [sanal makine Katılımcısı](../articles/role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol (ön dönüştürme verebilir gibi) VM boyutunu değiştirmek mümkün olmayacaktır. Yönetilen disklere sahip VM'ler işletim sistemi disklerinde Microsoft.Compute/disks/write izninin gerektirmek olmasıdır.

* Dönüştürmeyi test ettiğinizden emin olun. Üretimde geçişi gerçekleştirmeden önce bir sınama sanal makinesini geçirin.

* Dönüştürme sırasında VM’yi serbest bırakın. VM, dönüştürmeden sonra başlatıldığında yeni bir IP adresi alır. Gerekirse VM’ye [statik bir IP adresi atayabilirsiniz](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

* Özgün VHD’ler ve dönüştürme öncesinde VM tarafından kullanılan depolama hesabı silinmez. Ücretler uygulanmaya devam eder. Bunlar için ücret alınmasını önlemek istiyorsanız, dönüştürmenin tamamlandığını doğruladıktan sonra özgün VHD bloblarını silin.

* Azure VM Aracısı dönüştürme işlemini desteklemek için gereken en düşük sürümünü gözden geçirin. Denetleyin ve aracınızın sürümü güncelleştirme konusunda daha fazla bilgi için bkz: [Azure VM aracıları için Minimum sürüm desteği](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)