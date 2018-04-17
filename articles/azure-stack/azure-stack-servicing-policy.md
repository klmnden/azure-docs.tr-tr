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
ms.date: 04/09/2018
ms.author: brenduns
ms.reviewer: harik
ms.openlocfilehash: 160ba42c5cbdd3e8b999040cba8254d4c87f7c63
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-stack-servicing-policy"></a>İlke bakım azure yığını
Bu makalede hizmet ilkesi Azure tümleşik yığını sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekir. 

## <a name="update-package-types"></a>Güncelleştirme paketi türleri

Tümleşik sistemleri için güncelleştirme paketleri iki tür vardır: 

- **Microsoft yazılım güncelleştirmelerini**. Microsoft, Microsoft yazılım güncelleştirme paketleri için uçtan uca hizmet yaşam döngüsü sorumludur. Bu paketleri en son Windows Server güvenlik güncelleştirmeleri, güvenlikle ilgili güncelleştirmeleri ve Azure yığın özellik güncelleştirmeleri içerebilir. Bu güncelleştirme paketleri doğrudan Microsoft'tan yükleyebilirsiniz.

- **OEM donanım satıcısı tarafından sağlanan güncelleştirmeleri**. Azure yığın donanım iş ortaklarından (yönergeler de dahil olmak üzere) uçtan uca hizmet yaşam döngüsü donanımla ilgili bellenim ve sürücü güncelleştirme paketleri için sorumludur. Ayrıca, Azure yığın donanım iş ortakları kendi ve tüm yazılım ve donanım yaşam döngüsü ana bilgisayardaki donanım kılavuzu sağlamak. OEM donanım satıcınızla bunlar barındıran güncelleştirme paketleri kendi indirme sitesinde.


## <a name="update-package-release-cadence"></a>Güncelleştirme paketi yayın tempoyla
Microsoft yazılım güncelleştirme paketleri aylık bir tempoyla serbest bırakmak bekliyor. Ancak, bir ay içinde birden çok veya hiçbir güncelleştirme yayımları olması mümkündür. OEM donanım satıcıları güncellemeleri gerektiği düzenli olarak bırakın. 

Belgeleri nasıl planlanacağı ve güncelleştirmeleri yönetmek ve geçerli sürümünüzde belirleme bulmak [Yönet güncelleştirmeleri genel bakış](azure-stack-updates.md). Karşıdan yükleme de dahil olmak üzere belirli bir güncelleştirmeyi hakkında bilgi için güncelleştirme sürüm notları için bkz: 
- [Azure yığın 1803 güncelleştirme](azure-stack-update-1803.md)
- [Azure yığın 1802 güncelleştirme](azure-stack-update-1802.md)
- [Azure yığın 1712 güncelleştirme](azure-stack-update-1712.md)



## <a name="hotfixes"></a>Düzeltmeleri
Bazen, Microsoft bu adrese önleyici veya zamana duyarlı genellikle belirli bir sorunun Azure yığını için düzeltmeler sağlar.  Her bir düzeltme ile sorun, nedeni ve çözümü ayrıntıları bir karşılık gelen bir Microsoft Bilgi Bankası makalesi yayımlanır. 

Düzeltmeleri indirilir ve yalnızca normal tam güncelleştirme paketleri gibi Azure yığını için yüklenir. Ancak, tam güncelleştirme, dakika cinsinden düzeltmeleri yükleyebilirsiniz. Düzeltmeleri yüklerken bakım windows Azure yığın işleçleri ayarlamanızı öneririz. Böylece düzeltme uygulanıp uygulanmadığını kolayca belirleyebilir düzeltmeleri Azure yığın bulut sürümünü güncelleştirin. Hala desteklenmediği Azure yığınının her bir sürümü için ayrı bir düzeltme sağlanır. Belirli bir yineleme için her düzeltme toplu ve aynı sürümü önceki güncelleştirmeleri içerir. Daha fazla bilgiyi hakkında bilgi bankası karşılık gelen bir düzeltmeler de belirli bir düzeltme uygulanabilirliğini makalesi.  


## <a name="keep-your-system-under-support"></a>Sisteminizin destek altında tutun
Destek almak devam etmek için geçerli Azure yığın dağıtımınızı tutmalısınız. Güncelleştirmeler için erteleme İlkesi: Azure yığın dağıtımınızı desteği kalmasına en yakın zamanda piyasaya çıkan güncelleştirme sürümünü çalıştırana veya gerekir iki önceki güncelleştirme sürümleri birini çalıştırın. Düzeltmeleri önemli güncelleştirme sürümleri olarak kabul edilmez. Azure yığın bulut arkasında tarafından ise *ikiden fazla güncelleştirmeleri*, uyumsuz olarak kabul edilir ve en az destek almak için desteklenen minimum sürümü güncelleştirmeniz gerekir. 

Örneğin, en son kullanılabilir güncelleştirme sürüm 1805 ve önceki iki güncelleştirme paketleri 1804 ve 1803 sürümleri olan 1803 ve 1804 desteği kalır. Ancak, 1802 dışında desteğidir. İlke olduğunda hiçbir sürüm ayda bir veya iki için geçerlidir. Örneğin, 1805 sürümü geçerli olduğundan ve hiçbir 1804 yayın vardı, önceki iki güncelleştirme paketleri 1803 ve 1802 desteği kalır.

Microsoft yazılım güncelleştirme paketleri, toplu olmayan ve bir önkoşul olarak önceki güncelleştirme paketini gerektirir. Bir veya daha fazla güncelleştirmelerinin erteleneceği karar verirseniz, en son sürümünü almak genel çalışma zamanı göz önünde bulundurun. 


## <a name="next-steps"></a>Sonraki adımlar

- [Azure yığınında güncelleştirmelerini yönetme](azure-stack-updates.md)


