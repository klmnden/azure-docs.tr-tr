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
ms.date: 04/03/2018
ms.author: mabrigg
ms.openlocfilehash: e37b63580d8cea4b5772bc54f7b2f79980afc0bc
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
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
Destek almak devam etmek için geçerli Azure yığın dağıtımınızı tutmalısınız. Erteleme güncelleştirme için ilke Azure yığın'ın desteği kalmasına onu gerekir en yakın zamanda piyasaya çıkan güncelleştirme sürümünü çalıştırana veya iki önceki önemli güncelleştirme sürümleri birini çalıştırın olması.  Düzeltmeleri önemli güncelleştirme sürümleri olarak kabul edilmez.  Azure yığın bulut arkasında tarafından ise *ikiden fazla güncelleştirmeleri*, uyumsuz olarak kabul edilir ve en az destek almak için desteklenen minimum sürümü güncelleştirmeniz gerekir. 

Örneğin, en son kullanılabilir güncelleştirme sürüm 1805 ve önceki iki güncelleştirme paketleri 1804 ve 1803 sürümleri olan 1803 ve 1804 desteği kalır. Ancak, 1802 dışında desteğidir. İlke olduğunda hiçbir sürüm ayda bir veya iki için geçerlidir. Örneğin, geçerli sürümde 1805 ise ve hiçbir 1804 yayın vardı, önceki iki güncelleştirme paketleri 1803 ve 1802 desteği kalır.

Microsoft yazılım güncelleştirme paketleri, toplu olmayan ve bir önkoşul olarak önceki güncelleştirme paketini gerektirir. Bir veya daha fazla güncelleştirmelerinin erteleneceği karar verirseniz, en son sürümünü almak genel çalışma zamanı göz önünde bulundurun. 


## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığınında güncelleştirmelerini yönetme](azure-stack-updates.md)


