---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 06/07/2019
ms.author: rogarana
ms.openlocfilehash: 368f08272173b019873dfe20e1164d6baf72ff5e
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542645"
---
#### <a name="additional-premium-file-share-level-limits"></a>Ek premium dosya paylaşımı düzeyinde sınırları

|Alan  |Hedef  |
|---------|---------|
|En küçük boyut artırma/azaltma    |1 giB      |
|Temel IOPS    |100.000 adede kadar bir GiB başına 1 IOPS|
|Patlaması IOPS    |3 x IOPS, 100.000 adede kadar GiB başına|
|Çıkış oranı         |60 MiB/sn + 0,06 * GiB sağlandı        |
|Giriş oranı| 40 MiB/sn + 0.04 * GiB sağlandı |

#### <a name="file-level-limits"></a>Dosya düzeyi sınırları

|Alan  |Premium dosyası  |Standart dosya |
|---------|---------|---------|
|Size                  |1 TiB         |1 TiB|
|Dosya başına en fazla IOPS     |5,000         |1000|
|Eşzamanlı işler    |2,000         |2,000|
|Giriş  |300 MiB/sec|      Standart dosya aktarım hızı değerleri bakın|
|Çıkış   |200 Mib/sec| Standart dosya aktarım hızı değerleri bakın|
|Aktarım hızı| Premium dosya giriş/çıkış değerleri bakın| En fazla 60 MiB/sn|