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
ms.date: 06/28/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cff9be3b074dde4a0335675663133a8df81ae62d
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114601"
---
# <a name="operating-system-upgrade"></a>İşletim sistemi yükseltmesi
Bu belgede işletim sistemi yükseltmeleri HANA büyük örneklerinde ayrıntıları açıklar.

>[!NOTE]
>İşletim sistemi yükseltme müşteriler sorumluluk, Microsoft operations destek yükseltme sırasında dikkat edilmesi gereken önemli alanlar için yönlendirebilir. Yükseltme için planlama önce de işletim sistemi satıcınıza danışmalısınız.

Sağlama HLI birim zaman Microsoft işletim ekibi işletim sistemini yükleyin. Zamanla, işletim sistemi sürdürmek için gerekli (örnek: düzeltme eki uygulama, ayarlama, yükseltme vb.) HLI birim üzerinde.

İşletim sistemi (örneğin, SP2'ye yükseltme SP1) yapılan değişiklikler birincil önce başvurun için bir destek bileti açılarak Microsoft Operations takım başvurmanız gerekir.


Farklı Linux sürümleri ile farklı SAP HANA sürümleri için destek matrisi bkz [SAP Not #2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Bilinen sorunlar

Bazı sık karşılaşılan bilinen sorunlar yükseltme sırasında şunlardır:
- İşletim sistemi yükseltme yaptıktan sonra SKU türü II sınıfı SKU, yazılım foundation yazılımı (SFS) kaldırılır. İşletim sistemi yükseltme yaptıktan sonra uyumlu SFS yeniden yüklemeniz gerekir.
- Ethernet kartı sürücüleri (ENIC ve FNIC), eski sürüme geri alındı. Sürücüleri uyumlu bir sürümü yükseltme sonrasında yeniden yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- Başvuru [yedekleme ve geri yükleme](hana-overview-high-availability-disaster-recovery.md) işletim sistemi için yedekleme türü ı SKU sınıfı.
- Başvuru [OS yedekleme türü II SKU'ları için](os-backup-type-ii-skus.md) türü II SKU sınıfı için.