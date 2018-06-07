---
title: SAP HANA azure'da (büyük örnekler) için işletim sistemi yükseltme | Microsoft Docs
description: SAP HANA azure'da (büyük örnekler) için işletim sistemi yükseltme gerçekleştirin.
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f6af1a6612360c2433c05a7add79d2e7b3b9d754
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658269"
---
# <a name="operating-system-upgrade"></a>İşletim sistemi yükseltmesi
Bu belgede işletim sistemi yükseltmeleri HANA büyük örneklerinde ayrıntıları açıklar.

>[!NOTE]
>İşletim sistemi yükseltme müşteriler sorumluluk, Microsoft operations destek yükseltme sırasında dikkat edilmesi gereken önemli alanlar için yönlendirebilir. Yükseltme için planlama önce de işletim sistemi satıcınıza danışmalısınız.

Sağlama HLI birim zaman Microsoft işletim ekibi işletim sistemini yükleyin. Zamanla, işletim sistemi sürdürmek için gerekli (örnek: düzeltme eki uygulama, ayarlama, yükseltme vb.) HLI birim üzerinde.

İşletim sistemi (örneğin, bir işletim sistemi yükseltme), değişiklikleri birincil önce **gerekir** aşağıdaki uyumluluk matrisi göz önünde bulundurun. **Gerekir** da yükseltme gibi önemli işletim sistemi etkinlikler başlamadan önce danışın için bir destek bileti açılarak Microsoft Operations ekibine başvurun.

Farklı Linux sürümleri ile farklı SAP HANA sürümleri için destek matrisi bkz [SAP Not #2235581](https://launchpad.support.sap.com/#/notes/2235581).

Aşağıdaki uyumluluk HLIs için test edilmiştir. HLI sunucunuz dışında uyumluluk matrisi ise, Microsoft işlemi Destek'e başvurun.

## <a name="for-type-i-class-sku-category"></a>T türü için SKU kategori sınıfı

| Yapılandırma | SUSE12 SP1 | SUSE12 SP2 | RHEL 7.2 | RHEL 7.3|
| --- | --- | --- | --- | --- |
| Sunucu bellenim | 3.1(2B) | 3.1(2B) | 3.1(2B) | 3.1(2B) |
| ENIC sürüm | 2.3.0.44 | 2.3.0.44 | 2.3.0.30 | 2.3.0.44 |
| FNIC sürüm | 1.6.0.34 | 1.6.0.34 | 1.6.0.27 | 1.6.0.36 |
| EDAC | Devre dışı | Devre dışı | Devre dışı | Devre dışı |
| Çekirdek sürümü | 4.4.21-69-default | 3.12.49-11-default | 3.10.0-327.el7.x86_64 | 3.10.0-693.17.1 |


## <a name="for-type-ii-class-sku-category"></a>Tür II SKU kategori sınıfı

| Yapılandırma | SUSE12 SP1 | SUSE12 SP2 | RHEL 7.2 | RHEL 7.3|
| --- | --- | --- | --- | --- |
| RMC bellenim sürümü | 1.1.121  | 1.1.121  | 1.1.121  | 1.1.121 |
| BMC bellenim sürümü | 1.0.43   | 1.0.43   | 1.0.43   | 1.0.43  |
| Yazılım Foundation Server (SFS) sürümü | 2.16    | 2.16    | 2.14/2.16   | 2.16   |
| BIOS | 5.2.6    | 5.2.6    | 5.2.6    | 5.2.6   |
| i40e sürüm    | 2.0.19     | 2.0.19     | 1.5.10-k    | 1.5.10-k   |
| Çekirdek sürümü    | 3.12.49-11.1     | 4.4.21-69.1     | 3.10.0-327    | 3.10.0-693.17.1   |


## <a name="known-issues"></a>Bilinen sorunlar

Bazı sık karşılaşılan bilinen sorunlar yükseltme sırasında şunlardır:
- İşletim sistemi yükseltme yaptıktan sonra SKU türü II sınıfı SKU, yazılım foundation yazılımı (SFS) kaldırılır. İşletim sistemi yükseltme yaptıktan sonra uyumlu SFS yeniden yüklemeniz gerekir.
- Ethernet kartı sürücüleri (ENIC ve FNIC), eski sürüme geri alındı. Sürücüleri uyumlu bir sürümü yükseltme sonrasında yeniden yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- Başvuru [yedekleme ve geri yükleme](hana-overview-high-availability-disaster-recovery.md) işletim sistemi için yedekleme türü ı SKU sınıfı.
- Başvuru [OS yedekleme türü II SKU'ları için](os-backup-type-ii-skus.md) türü II SKU sınıfı için.