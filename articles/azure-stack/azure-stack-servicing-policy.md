---
title: İlke bakım azure yığın | Microsoft Docs
description: Bakım İlkesi ve desteklenen bir duruma tümleşik bir sistem korumak nasıl Azure yığın hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: f495ca12e7cdb1bf61f09bd2d4a8a21654745d8a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-stack-servicing-policy"></a>İlke bakım azure yığını
Bu makalede hizmet ilkesi Azure tümleşik yığını sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekir. 

## <a name="update-package-types"></a>Güncelleştirme paketi türleri

Tümleşik sistemleri için güncelleştirme paketleri iki tür vardır; Microsoft yazılım güncelleştirmeleri ve özgün donanım üreticisi (OEM) donanım satıcınıza, sürücüler ve bellenim gibi belirli güncelleştirmeler. Bu güncelleştirmeler ayrı Azure yığın güncelleştirme paketleri teslim edilir ve bağımsız olarak yönetilir.

- **Microsoft yazılım güncelleştirmelerini**. Microsoft, Microsoft yazılım güncelleştirme paketleri için uçtan uca hizmet yaşam döngüsü sorumludur. Bu paketleri en son Windows Server güvenlik güncelleştirmeleri, güvenlikle ilgili güncelleştirmeleri ve Azure yığın özellik güncelleştirmeleri içerebilir. Bu güncelleştirme paketleri doğrudan Microsoft'tan yükleyebilirsiniz.
- **OEM donanım satıcısı tarafından sağlanan güncelleştirmeleri**. Azure yığın donanım iş ortaklarından (yönergeler de dahil olmak üzere) uçtan uca hizmet yaşam döngüsü donanımla ilgili bellenim ve sürücü güncelleştirme paketleri için sorumludur. Ayrıca, Azure yığın donanım iş ortakları kendi ve tüm yazılım ve donanım yaşam döngüsü ana bilgisayardaki donanım kılavuzu sağlamak. OEM donanım satıcınızla bunlar barındıran güncelleştirme paketleri kendi indirme sitesinde.

## <a name="update-package-release-cadence"></a>Güncelleştirme paketi yayın tempoyla

Microsoft yazılım güncelleştirme paketleri aylık bir tempoyla serbest bırakmak bekliyor. Ancak, bir ay içinde birden çok veya hiçbir güncelleştirme yayımları olması mümkündür. OEM donanım satıcıları güncellemeleri gerektiği düzenli olarak bırakın.

Bir Microsoft güncelleştirme paketi kolayca yayın tarihi tanımlamanıza yardımcı olması için aşağıdaki adlandırma kuralını sahiptir:

*MajorProductVersion.MinorProductVersion.YYMMDD.BuildNumber*

Örneğin, 15 Haziran 2017 üzerinde yayımlanan bir Microsoft yazılım güncelleştirmesi "1.0.170615.1" sürüm gerekir.

## <a name="keep-your-system-under-support"></a>Sisteminizin destek altında tutun

Sisteminiz için destek almak için belirli bir zaman aralığı içinde güncelleştirilmiş Azure yığın tutmanız gerekir. Bizim erteleme Microsoft yazılım güncelleştirmeleri için üç ayda bir ilkedir. Sisteminiz üç aydan daha eski ise, uyumsuz olarak kabul. Sistem için en az güncellemeniz gerekir desteklenen minimum sürüm desteği alabilirsiniz. 

Microsoft yazılım güncelleştirme paketleri, toplu olmayan ve bir önkoşul olarak önceki güncelleştirme paketini gerektirir. Bir veya daha fazla güncelleştirmelerinin erteleneceği karar verirseniz, en son sürümünü almak genel çalışma zamanı göz önünde bulundurun.

Aşağıdaki tabloda, örnek güncelleştirme paketi sürümleri, kendi önkoşul ve sisteminizin destek korumak için en gereken desteklenen minimum sürümü gösterir. Tablonun ilk güncelleştirme paketi sürümünde (1709) Eylül 2017 ilk sürümünden (1708 yapı) Azure tümleşik yığını sistemlerinin temel alır. 

| Son güncelleştirme paketini (*örnek*) | Önkoşul | Desteklenen en düşük sürüm |
| -- | -- | -- |
| 1710 | 1709 | Yok |
| 1711 | 1710 | 1709 |
| 1712 | 1711 | 1710 |
| 1802 | 1801 | 1712 |
| 1803 | 1802 | 1801 |
| 1804 | 1803 | 1802 |
| 1805 | 1804 | 1803 |
| | | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığınında güncelleştirmelerini yönetme](azure-stack-updates.md)


