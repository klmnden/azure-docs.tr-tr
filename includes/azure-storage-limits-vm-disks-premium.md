---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: e7e57c6a821731874dcb1d99a3133b6ede1da26e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66148173"
---
**Premium yönetilmeyen sanal makine diskleri: Hesap başına limitler**

| Resource | Varsayılan limit |
| --- | --- |
| Hesap başına toplam disk kapasitesi |35 TB |
| Hesap başına toplam anlık görüntü kapasitesi |10 TB |
| En yüksek bant genişliği (giriş ve çıkış) hesabı başına<sup>1</sup> |En fazla 50 Gbps |

<sup>1</sup>*giriş* bir depolama hesabına gönderilen istekleri gelen tüm verileri ifade eder. *Çıkış* yanıtlardan bir depolama hesabından alınan tüm verilere başvuruda bulunmaktadır.

**Premium yönetilmeyen sanal makine diskleri: Disk başına limitler**

| Premium depolama diski türü | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| Disk boyutu |128 GiB |512 GiB |1\.024 giB (1 TB) |2\.048 giB (2 TB)|4\.095 giB (4 TB)|
| Disk başına maksimum IOPS |500 |2,300 |5\.000 |7\.500 |7,500 |
| Disk başına en fazla aktarım hızı |100 MB/sn | 150 MB/sn |200 MB/sn |250 MB/sn |250 MB/sn |
| Depolama hesabı başına disk sayısı |280 |70 |35 | 17 | 8 |

**Premium yönetilmeyen sanal makine diskleri: VM başına limitler**

| Resource | Varsayılan limit |
| --- | --- |
| VM başına maksimum IOPS |GS5 VM ile 80.000 IOPS |
| VM başına en fazla aktarım hızı |GS5 VM ile 2.000 MB/sn |

