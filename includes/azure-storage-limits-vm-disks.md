---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: 09b4f94db3464943a8367bfb3ca89f9a88446193
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554182"
---
Bir Azure sanal makinesine veri diski sayısı ekleyebilirsiniz. Bir sanal makinenin veri diskleri için ölçeklenebilirlik ve performans hedefleri bağlı olarak, performans ve kapasite gereksinimlerini karşılamak için gereken disk türü ve numarası belirleyebilirsiniz.

> [!IMPORTANT]
> En iyi performans için olası azalmayı önlemek için sanal makineye bağlı, yüksek oranda kullanılan disk sayısını sınırlayın. Eklenen tüm diskler aynı anda yüksek oranda kullanılan değil, sanal makine diskleri daha fazla sayıda destekleyebilir.

* **İçin Azure yönetilen diskler:** 

> | Kaynak | Varsayılan limit | Üst sınır |
> | --- | --- | --- |
> | Standart yönetilen diskler | 10,000 | 50,000 |
> | Standart SSD yönetilen diskler | 10,000 | 50,000 |
> | Premium yönetilen diskler | 10,000 | 50,000 |
> | Standard_LRS anlık görüntüleri | 10,000 | 50,000 |
> | Standard_ZRS anlık görüntüleri | 10,000 | 50,000 |
> | Yönetilen bir görüntü | 10,000 | 50,000 |

* **Standart depolama hesapları için:** Bir standart depolama hesabı, 20.000 IOPS toplam en fazla istek oranını sahiptir. Tüm standart depolama hesabı, sanal makine disklerinizde toplam IOPS bu sınırı aşmamalıdır.
  
    İstek oranı sınırına göre tek bir standart depolama hesabı tarafından desteklenen yüksek kullanımlı disk sayısını kabaca hesaplayabilirsiniz. Örneğin, bir temel katman için sanal makine, disk başına 20.000/300 IOPS olduğu sayısı yüksek kullanımlı disk 66 hakkında sayısıdır. Bir standart katman sanal makine için yüksek oranda kullanılan disk sayısı, disk başına 20.000/500 IOPS olduğu yaklaşık 40, olur. 

* **Premium depolama hesapları için:** Premium depolama hesabı en fazla aktarım hızı 50 Gbps'dir sahiptir. Tüm sanal makinelerdeki toplam aktarım hızı bu sınırı aşmamalıdır.

