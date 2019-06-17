---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 563849d3875ed0156d81770f58340633d90d515b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161267"
---
Azure nesnelerin yazılabilir boyutları aşağıda verilmiştir. Yüklenen tüm dosyalar bu sınırlara uyacak emin olun.

| Azure nesne türü | Karşıya yükleme sınırı                                             |
|-------------------|-----------------------------------------------------------|
| Blok blobu        | ~ 4,75 TB                                                 |
| Sayfa blobu         | 1 TB <br> Sayfa blobu biçiminde yüklenen her dosya 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var. |
| Azure Dosyaları         | 1 TB <br> Sayfa blobu biçiminde yüklenen her dosya 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var. |

> [!IMPORTANT]
> (Bağımsız olarak depolama türü) dosyaları oluşturma, en çok 5 TB izin verilir. Bir dosya boyutu, yukarıdaki tabloda tanımlanan karşıya yükleme sınırı büyüktür oluşturursanız, ancak dosyayı karşıya. Alan kazanmak için dosyayı el ile silmeniz gerekir.