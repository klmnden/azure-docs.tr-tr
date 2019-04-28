---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2019
ms.author: rogarana
ms.openlocfilehash: 2936fd318f08c74675f7e8b382c861f4a28319fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60386363"
---
Bir Azure sanal makinesine veri diski sayısı ekleyebilirsiniz. Bir sanal makinenin veri diskleri için ölçeklenebilirlik ve performans hedefleri bağlı olarak, performans ve kapasite gereksinimlerini karşılamak için gereken disk türü ve numarası belirleyebilirsiniz.

> [!IMPORTANT]
> En iyi performans için olası azalmayı önlemek için sanal makineye bağlı, yüksek oranda kullanılan disk sayısını sınırlayın. Eklenen tüm diskler aynı anda yüksek oranda kullanılan değil, sanal makine diskleri daha fazla sayıda destekleyebilir.

**İçin Azure yönetilen diskler:**

Aşağıdaki tabloda, varsayılan ve maksimum sınırları her Abonelikteki bölge başına kaynak sayısı gösterilmektedir.

> | Kaynak | Varsayılan limit  | Üst sınır |
> | --- | --- | --- |
> | Standart yönetilen diskler | 25,000 | 50,000 |
> | Standart SSD yönetilen diskler | 25,000 | 50,000 |
> | Premium yönetilen diskler | 25,000 | 50,000 |
> | Standard_LRS anlık görüntüleri | 25,000 | 50,000 |
> | Standard_ZRS anlık görüntüleri | 25,000 | 50,000 |
> | Yönetilen bir görüntü | 25,000 | 50,000 |

* **Standart depolama hesapları için:** Bir standart depolama hesabı, 20.000 IOPS toplam en fazla istek oranını sahiptir. Tüm standart depolama hesabı, sanal makine disklerinizde toplam IOPS bu sınırı aşmamalıdır.
  
    İstek oranı sınırına göre tek bir standart depolama hesabı tarafından desteklenen yüksek kullanımlı disk sayısını kabaca hesaplayabilirsiniz. Örneğin, bir temel katman için sanal makine, disk başına 20.000/300 IOPS olduğu sayısı yüksek kullanımlı disk 66 hakkında sayısıdır. Bir standart katman sanal makine için yüksek oranda kullanılan disk sayısı, disk başına 20.000/500 IOPS olduğu yaklaşık 40, olur. 

* **Premium depolama hesapları için:** Premium depolama hesabı en fazla aktarım hızı 50 Gbps'dir sahiptir. Tüm sanal makinelerdeki toplam aktarım hızı bu sınırı aşmamalıdır.

