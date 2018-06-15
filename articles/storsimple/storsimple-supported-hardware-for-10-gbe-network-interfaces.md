---
title: StorSimple 10 GbE için donanım arabirimleri | Microsoft Docs
description: Desteklenen küçük form Faktörlü Takılabilir (SFP) vericilerinin, kablolar ve anahtarlar 10 GbE ağ arabirimleri için StorSimple Cihazınızda açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: df8d40c7-f5ad-4f84-93eb-779fbd5f7243
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: db03b3cd668bf8e35913872ac4225de6d4d3edd1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23927757"
---
# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>StorSimple Cihazınızda 10 GbE ağ arabirimleri için desteklenen donanım
## <a name="overview"></a>Genel Bakış
Bu makalede, Microsoft Azure StorSimple cihazınızla çalışır tamamlayıcı donanım hakkında bilgi sağlar.

## <a name="list-of-devices-tested-by-microsoft"></a>Microsoft tarafından test aygıtların listesi
Microsoft aşağıdaki küçük form Faktörlü Takılabilir (SFP) vericilerinin, kablolar ve anahtarlar en iyi şekilde aygıtlarla çalıştıklarının emin olmak için test. (Yeni donanım sınanırken aşağıdaki tablolarda güncelleştirilir.)

### <a name="sfp-transceivers"></a>SFP + ileticileri
| Yapma | modeli |
| --- | --- |
| Cisco |SFP 10G SR |

### <a name="cables"></a>Kabloları
| S. Hayır. | Yapma | modeli |
| --- | --- | --- |
| 1. |Cisco |SFP H10GB CU1M |
| 2. |Cisco |SFP H10GB CU2M |
| 3. |Cisco |SFP H10GB CU3M |
| 4. |Tripp Lite |N820 - 05M (OM3) |

### <a name="switches"></a>Anahtarları
| S. Hayır. | Yapma | modeli |
| --- | --- | --- |
| 1. |Cisco |N3K C3172PQ 10GE |
| 2. |Cisco |N3K-C3048-ZM-F |
| 3. |Cisco |N5K C5596UP FA |

## <a name="list-of-devices-tested-in-the-field"></a>Aygıtların listesi alanında test
Bu bölüm alanında StorSimple müşteriler tarafından dağıtılan başarıyla aygıtların listesini içerir. Bunlar, Microsoft tarafından test edilmemiştir ancak StorSimple cihazınızla çalışması olasıdır.

| Parametre | Değer |
| --- | --- |
| Geçiş yapmak |Juniper |
| Anahtar modeli |ex4550 32F |
| Anahtar işletim sistemi sürümü |JunOS 12.3R9.4 |
| Dikey modeli |Yerleşik bağlantı noktaları (PIC 0) |
| Alıcı oluşturma |Juniper |
| Alıcı modeli |Parça numarası 740 021308 <br></br> Parça numarası 740 030658 |
| Alıcı bellenim sürümü |01 sürüm (bildirilen) 0,0 rev |
| Kablo modeli |Çift yönlü anahtar LC/LC 50/125µ, OM3, LSZH |
| StorSimple modeli |8600 |
| StorSimple yazılım sürümü |6.3.9600.17491 |

## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>OEM sağlayıcısı (Mellanox) tarafından test aygıtların listesi
Mellanox aşağıdaki küçük form Faktörlü Takılabilir (SFP) vericilerinin, kablolar ve anahtarlar en iyi şekilde 10 GbE ağ arabirimleri, StorSimple Cihazınızda gibi Mellanox ağ arabirimleri ile çalıştıklarının emin olmak için test.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kablo ve Mellanox tarafından desteklenen modülleri
Aşağıdaki tabloda, kablo ve Mellanox tarafından desteklenen modüllerini listeler. Bunlar, Microsoft tarafından test edilmemiştir ancak StorSimple cihazınızla çalışması olasıdır.

| S. Hayır. | Hız | modeli | Açıklama | Yapma |
| --- | --- | --- | --- | --- |
| 1. |10 GbE |CAB-SFP-SFP - 1M |Pasif Bakır kablo SFP + 10 Gb/sn 1m |Arista |
| 2. |10 GbE |CAB-SFP-SFP - 2M |Pasif Bakır kablo SFP + 10 Gb/sn 2m |Arista |
| 3. |10 GbE |CAB-SFP-SFP - 3M |Pasif Bakır kablo SFP + 10 Gb/sn 3m |Arista |
| 4. |10 GbE |CAB-SFP-SFP - 5M |Pasif Bakır kablo SFP + 10 Gb/sn 5m |Arista |
| 5. |10 GbE |Cisco SFP-H10GBCU1M |Cisco SFP + kablosu |Cisco |
| 6. |10 GbE |Cisco SFP-H10GBCU3M |Cisco SFP + kablosu |Cisco |
| 7. |10 GbE |Cisco SFP-H10GBCU5M |Cisco SFP + kablosu |Cisco |
| 8. |10 GbE |J9281B HP X242 10 G |SFP + SFP + 1m doğrudan Bakır kablo ekleme |HP |
| 9. |10 GbE |455883 B21 HP BLc |10Gb SR SFP + iptal et |HP |
| 10. |10 GbE |455886 B21 HP BLc |10Gb LR SFP + iptal et |HP |
| 11. |10 GbE |487649 B21 HP BLc |SFP + 0,5 m 10GbE Bakır kablosu |HP |
| 12. |10 GbE |487652 B21 HP BLc |SFP + 1m 10GbE Bakır kablosu |HP |
| 13. |10 GbE |487655 B21 HP BLc |SFP + 3m 10GbE Bakır kablosu |HP |
| 14. |10 GbE |487658 B21 HP BLc |SFP + 7m 10GbE Bakır kablosu |HP |
| 15. |10 GbE |537963 B21 HP BLc |SFP + 5m 10GbE Bakır kablosu |HP |
| 16. |10 GbE |AP784A HP |3m C-Serisi pasif Bakır SFP + kablosu |HP |
| 17. |10 GbE |AP785A HP |5m C-Serisi pasif Bakır SFP + kablosu |HP |
| 18. |10 GbE |AP818A HP |1m B-serisi etkin Bakır SFP + kablosu |HP |
| 19. |10 GbE |AP819A HP |3m B-serisi etkin Bakır SFP + kablosu |HP |
| 20. |10 GbE |J9150A HP |X132 10 G SFP + LC SR ileticisi |HP |
| 21. |10 GbE |J9151A HP |X132 10 G SFP + LC LR ileticisi |HP |
| 22. |10 GbE |J9283B HP |X242 10 G SFP + SFP + 3 m DAC kablosu |HP |
| 23. |10 GbE |J9285B HP |X242 10 G SFP + SFP + 7 m DAC kablosu |HP |
| 24. |10 GbE |JD095B HP |X240 10 G SFP + SFP + 0.65 m DAC kablosu |HP |
| 25. |10 GbE |JD096B HP |X240 10 G SFP + SFP + 1.2 m DAC kablosu |HP |
| 26. |10 GbE |JD097B HP |X240 10 G SFP + SFP + 3 m DAD kablosu |HP |
| 27. |10 GbE |MAM1Q00A QSA Mellanox |SFP + bağdaştırıcısına QSFP |Mellanox teknolojileri |
| 28. |10 GbE |MC2309124-006 Mt |Pasif Bakır kablo 1 x SFP+ QSFP 10 Gb/s 24awg için 7 m |Mellanox teknolojileri |
| 29. |10 GbE |MC2309124 007 Mt |Pasif Bakır kablo 1 x SFP+ QSFP 10 Gb/s 24awg için 7 m |Mellanox teknolojileri |
| 30. |10 GbE |MC2309130 003 Mt |Pasif Bakır kablo 1 x SFP+ QSFP 10 Gb/s 30awg için 3 m |Mellanox teknolojileri |
| 31. |10 GbE |MC2309130 00A Mt |Pasif Bakır kablo 1 x SFP+ QSFP 10 Gb/s 30awg için 0,5 m |Mellanox teknolojileri |
| 32. |10 GbE |MC3309124 005 Mt |Pasif Bakır kablo 1 x SFP+ 10 Gb/s 24awg 5 m |Mellanox teknolojileri |
| 33. |10 GbE |MC3309124 007 Mt |Pasif Bakır kablo 1 x SFP+ 10 Gb/s 24awg 7 m |Mellanox teknolojileri |
| 34. |10 GbE |MC3309130 003 Mt |Pasif Bakır kablo 1 x SFP+ 10 Gb/s 30awg 3 m |Mellanox teknolojileri |
| 35. |10 GbE |MC3309130 00A Mt |Pasif Bakır kablo 1 x SFP+ 10 Gb/s 30awg 0,5 m |Mellanox teknolojileri |

### <a name="switches-supported-by-mellanox"></a>Mellanox tarafından desteklenen anahtarları
Aşağıdaki tabloda Mellanox tarafından desteklenen anahtarları listeler. Bunlar, Microsoft tarafından test edilmemiştir ancak StorSimple cihazınızla çalışması olasıdır.

| S. Hayır. | Hız | modeli | Açıklama | Yapma |
| --- | --- | --- | --- | --- |
| 1. |10GbE |516733 B21 |HP ProCurve 6120XG 10GbE Ethernet dikey anahtarı |HP |
| 2. |10GbE |538113 B21 |HP 10GbE geçiş Modülü (PTM) |HP |
| 3. |10GbE |EN4093 |IBM PureFlex sistem doku EN4093 10 Gigabit ölçeklenebilir anahtarı Modülü |IBM |
| 4. |1GbE |3020 |Cisco Catalyst 3020 1GbE anahtar dikey penceresi |Cisco |
| 5. |1GbE |3020 X |Cisco Catalyst 3020 X 1GbE anahtar dikey penceresi |Cisco |
| 6. |1GbE |438030 B21 |HP 1GbE anahtar modül - GbE2c Katman 2/3 Ethernet dikey anahtarı |HP |
| 7. |1GbE |6120G |HP ProCurve 6120G/XG 1GbE anahtar dikey penceresi |HP |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple donanım bileşenleri ve durumu hakkında daha fazla bilgi edinmek](storsimple-monitor-hardware-status.md).

