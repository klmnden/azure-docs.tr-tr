---
title: Azure Stack hizmet İlkesi | Microsoft Docs
description: İlke ve tümleşik bir sistem desteklenen bir durumda tutmak nasıl hizmet Azure Stack hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: brenduns
ms.reviewer: harik
ms.openlocfilehash: f74a4ad0507f1c1f029befff88d733ffa719a763
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44023520"
---
# <a name="azure-stack-servicing-policy"></a>Azure Stack hizmet İlkesi
Bu makalede, Azure Stack tümleşik sistemleri ve desteklenen bir duruma sisteminizi tutmak için yapmanız gerekenlere hizmet İlkesi açıklanır. 

## <a name="update-package-types"></a>Güncelleştirme paketi türleri

Tümleşik sistemler için güncelleştirme paketleri iki tür vardır: 

- **Microsoft yazılım güncelleştirmeleri**. Microsoft, Microsoft yazılım güncelleştirme paketleri için uçtan uca hizmet yaşam döngüsü sorumludur. Bu paketlerin en son Windows Server güvenlik güncelleştirmeleri, güvenlikle ilgili olmayan güncelleştirmeleri ve Azure Stack özellik güncelleştirmeleri içerebilir. Bu güncelleştirme paketleri doğrudan Microsoft'tan indirebilirsiniz.

- **OEM donanım satıcısı tarafından sağlanan güncelleştirmeleri**. Azure Stack donanım iş ortakları, (yönergeler dahil olmak üzere) uçtan uca hizmet ömrü donanım ile ilgili bellenim ve sürücü güncelleştirme paketleri sorumludur. Ayrıca, Azure Stack donanım iş ortakları kendi ve yönergeler için tüm yazılım ve donanım yaşam döngüsü konaktaki bir donanım Bakımı. OEM donanım satıcınıza bunlar barındıran güncelleştirme paketleri kendi indirme sitesinde.


## <a name="update-package-release-cadence"></a>Güncelleştirme paketi yayın sıklığı
Microsoft, yazılım güncelleştirme paketleri aylık temposu serbest bırakmak bekliyor. Ancak, bir ay içindeki birden çok veya hiçbir güncelleştirme sürümleri mümkündür. OEM donanım satıcıları güncellemeleri gerektiğinde olarak bırakın. 

Planlama ve güncelleştirmeleri yönetme ve geçerli sürümünüzde belirleme belgelere [yönetme genel bakış güncelleştirmeleri](azure-stack-updates.md). Belirli bir güncelleştirme hakkında daha fazla bilgi için indirin, güncelleştirme sürüm notları için bkz dahil olmak üzere: 
- [Azure Stack 1808 güncelleştirme](azure-stack-update-1808.md)
- [Azure Stack 1807 güncelleştirme](azure-stack-update-1807.md)
- [Azure Stack 1805 güncelleştirme](azure-stack-update-1805.md)


## <a name="hotfixes"></a>Düzeltmeler
Bazen, Microsoft bu adrese önleyici veya zamana duyarlı genellikle belirli bir sorunun Azure Stack için düzeltmeler sağlar.  Her bir düzeltme sorun, nedeni ve çözümü ayrıntıları karşılık gelen bir Microsoft Bilgi Bankası makalesiyle serbest bırakılır. 

Düzeltmeleri indirilir ve Azure Stack için yalnızca normal tam güncelleştirme paketleri gibi yüklü. Ancak, tam güncelleştirme, dakikalar içinde düzeltmeleri yükleyebilirsiniz. Azure Stack operatörleri bakım pencereleri düzeltmeleri yüklerken ayarlamanızı öneririz. Böylece düzeltmeyi uygulanıp uygulanmadığını kolayca belirleyebilir düzeltmeleri Azure Stack bulutunuza sürümünü güncelleştirin. Her Azure Stack, yine destek sürümü için ayrı bir düzeltme sağlanır. Belirli bir yineleme için her bir düzeltme toplanır ve aynı sürüme önceki güncelleştirmeleri içerir. Daha fazla bilgi bankası karşılık gelen bir düzeltmeler de belirli bir düzeltme uygulanabilirliğini hakkında makalesi.  


## <a name="keep-your-system-under-support"></a>Sisteminizi desteği altında tutun
Destek almaya devam etmek için geçerli Azure Stack dağıtımınıza tutmalısınız. Güncelleştirmeleri erteleme İlkesi: Azure Stack dağıtımınıza desteği kalmasına en kısa süre önce yayımlanan bir güncelleştirme sürümünü çalıştırana veya olmalıdır iki önceki güncelleştirme sürümleri birini çalıştırın. Düzeltmeleri önemli güncelleştirme sürümleri olarak kabul edilmez. Azure Stack bulutunuza arkasında tarafından ise *ikiden fazla güncelleştirmeleri*, uyumsuz olarak kabul edilir ve en az destek almak için desteklenen en düşük sürüme güncelleştirmeniz gerekir. 

Örneğin, 1805 en son kullanılabilir güncelleştirme sürümüdür ve iki önceki güncelleştirme paketlerini 1804 ve 1803 sürümleri olan 1803 hem 1804 desteği kalır. Ancak, destek kapsamı dışında 1802 olur. İlke olduğunda yayın ayda bir veya iki için geçerlidir. Örneğin, geçerli sürümde 1805 ve 1804 yayın vardı, 1803 1802 ve iki önceki güncelleştirme paketlerini desteği kalır.

Microsoft yazılım güncelleştirme paketleri, toplu olmayan ve bir önkoşul olarak önceki güncelleştirme paketini gerektirir. Bir veya daha fazla güncelleştirmelerini erteleme karar verirseniz, genel çalışma zamanının en son sürüme almak istiyorsanız göz önünde bulundurun. 

## <a name="get-support"></a>Destek alın
Azure Stack, aynı Azure destek süreci izler. Kurumsal müşteriler, açıklanan işlemi izleyebilirsiniz [Azure destek isteği oluşturmak nasıl](/azure/azure-supportability/how-to-create-azure-support-request). Bir müşteri bir bulut hizmeti sağlayıcısı (CSP) ise, CSP için desteğe başvurun.  Daha fazla bilgi için [Azure desteği SSS](https://azure.microsoft.com/support/faq/). 


## <a name="next-steps"></a>Sonraki adımlar

- [Azure stack'teki güncelleştirmelerini yönetme](azure-stack-updates.md)


