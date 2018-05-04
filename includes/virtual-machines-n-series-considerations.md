---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3267f649e360c512a5523ce1d5948719a1969934
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
## <a name="deployment-considerations"></a>Dağıtma konuları

* N-serisi VM'ler kullanılabilirlik için bkz: [bölgeye göre ürünleri](https://azure.microsoft.com/regions/services/).

* N-serisi VM'ler yalnızca Resource Manager dağıtım modelinde dağıtılabilir.

* Azure için kendi diskleri destekleyen depolama türünde N-serisi VM'ler farklı. Yalnızca NC ve NV VM'ler standart Disk Depolama (HDD) tarafından desteklenen VM diskleri destekler. NCv2, ND ve NCv3 VM'ler yalnızca Premium Disk Depolama (SSD) tarafından desteklenen VM diskleri destekler.

* Birden fazla birkaç N-serisi sanal makineleri dağıtmak istiyorsanız, Kullandıkça Öde aboneliğine veya diğer satın alma seçenekleri göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* Azure aboneliğinizde (her bölge) çekirdeğini Kotayı artırmak ve NC, NCv2, NCv3, ND veya NV çekirdekleri ayrı Kotayı artırmak gerekebilir. Bir kota artışı isteği göndermek üzere [bir çevrimiçi müşteri destek isteği açma](../articles/azure-supportability/how-to-create-azure-support-request.md) herhangi bir ücret alınmaz. Varsayılan sınır abonelik kategoriye göre farklılık gösterebilir.




