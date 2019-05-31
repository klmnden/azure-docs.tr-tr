---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: eb12adf8f8523686b1d8deda2776eb203a76e954
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244622"
---
Azure nesnelerin yazılabilir boyutları aşağıda verilmiştir. Yüklenen tüm dosyalar bu sınırlara uyacak emin olun.

| Azure nesne türü | Varsayılan limit                                             |
|-------------------|-----------------------------------------------------------|
| Blok blobu        | ~ 4,75 TiB                                                 |
| Sayfa blobu         | 8 TiB <br> Sayfa blob biçiminde yüklenen her dosya 512 bayt hizalı (tamsayı katı) olmalıdır, aksi takdirde yükleme başarısız olur. <br> VHD ve VHDX 512 bayt hizalı var. |
| Azure Dosyaları        | 1 TiB                                                      |
| Yönetilen diskler     | 4 TiB <br> Boyutu ve sınırları hakkında daha fazla bilgi için bkz: <li>[Standart SSD ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#standard-ssd)</li><li>[Premium SSD ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#standard-hdd)</li><li>[Standart hdd'lerin ölçeklenebilirlik hedefleri](../articles/virtual-machines/windows/disks-types.md#premium-ssd)</li><li>[Fiyatlandırma ve faturalama yönetilen diskleri](../articles/virtual-machines/windows/disks-types.md#billing)</li>  
