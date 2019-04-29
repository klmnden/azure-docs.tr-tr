---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 563849d3875ed0156d81770f58340633d90d515b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60755256"
---
Azure nesnelerin yazılabilir boyutları aşağıda verilmiştir. Yüklenen tüm dosyalar bu sınırlara uyacak emin olun.

| Azure nesne türü | Karşıya yükleme sınırı                                             |
|-------------------|-----------------------------------------------------------|
| Blok Blobu        | ~ 4,75 TB                                                 |
| Sayfa Blobu         | 1 TB <br> Sayfa blobu biçiminde yüklenen her dosya 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var. |
| Azure Dosyaları         | 1 TB <br> Sayfa blobu biçiminde yüklenen her dosya 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var. |

> [!IMPORTANT]
> (Bağımsız olarak depolama türü) dosyaları oluşturma, en çok 5 TB izin verilir. Bir dosya boyutu, yukarıdaki tabloda tanımlanan karşıya yükleme sınırı büyüktür oluşturursanız, ancak dosyayı karşıya. Alan kazanmak için dosyayı el ile silmeniz gerekir.