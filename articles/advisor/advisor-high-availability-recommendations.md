---
title: "Azure Danışmanı yüksek kullanılabilirlik önerileri | Microsoft Docs"
description: "Azure dağıtımlarınızı yüksek kullanılabilirliğini artırmak için Azure Danışmanı'nı kullanın."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: e1cd7948e1969cd4ddb926e428c09b559190a805
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="advisor-high-availability-recommendations"></a>Advisor yüksek kullanılabilirlik önerileri

Azure Danışmanı emin olun ve iş açısından kritik uygulamalarınızı sürekliliği artırmanıza yardımcı olur. Danışmanına göre yüksek kullanılabilirlik öneriler alabilirsiniz **yüksek kullanılabilirlik** Danışmanı Pano sekmesi.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Sanal makine hataya dayanıklılık sağlamak

Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Advisor bir kullanılabilirlik kümesi ve bir kullanılabilirlik kümesine taşıma önerir parçası olmayan sanal makineleri tanımlar. Bu yapılandırma ya da bir planlı veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve Azure sanal makinesi SLA karşılayan sağlar. Kullanılabilirlik kümesi için sanal makine oluşturun veya var olan bir kullanılabilirlik kümesine sanal makine eklemek için seçebilirsiniz.

> [!NOTE]
> Bir kullanılabilirlik kümesi oluşturmayı seçerseniz, en az bir daha fazla sanal makine içine eklemeniz gerekir. En az bir makine olduğundan emin olmak için iki veya daha fazla sanal makine bir kullanılabilirlik kümesi bu, Grup kesinti sırasında kullanılabilir öneririz.

## <a name="ensure-availability-set-fault-tolerance"></a>Hataya dayanıklılık kullanılabilirlik kümesi emin olun 

Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Advisor tek bir sanal makine içeren kullanılabilirlik kümeleri tanımlar ve bir veya daha fazla sanal makine eklemeyi önerir. Bu yapılandırma ya da bir planlı veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve Azure sanal makinesi SLA karşılayan sağlar. Bir sanal makine oluşturun veya var olan bir sanal makine kullanılabilirlik kümesine eklemek için seçebilirsiniz.  

## <a name="ensure-application-gateway-fault-tolerance"></a>Uygulama ağ geçidi hataya dayanıklılık sağlamak
Uygulama ağ geçidi tarafından desteklenen kritik uygulamaların iş sürekliliğini sağlamak için hataya dayanıklılık için yapılandırılmamış uygulama ağ geçidi örnekleri Danışmanı tanımlar ve yapabileceğiniz düzeltme eylemleri önerir. Orta veya büyük tek örnekli uygulama ağ geçitleri Danışmanı tanımlar ve en az bir daha fazla örneğini eklemede önerir. Ayrıca, tek veya birden çok instance küçük uygulama ağ geçitleri tanımlar ve orta veya büyük SKU'ları için geçiş önerir. Advisor, uygulama ağ geçidi örnekleri bu kaynakların geçerli SLA gereksinimlerini karşılamak için yapılandırıldığından emin olmak için bu eylemleri önerir.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>Sanal makine disklerini güvenilirliğini ve performansını geliştirmek

Advisor standart diskler ile sanal makineleri tanımlar ve premium disklere yükseltme önerir.
 
Azure Premium depolama g/Ç kullanımı yoğun iş yükleri çalıştıran sanal makineler için yüksek performanslı, düşük gecikmeli disk desteği sunar. Premium depolama hesapları kullanan sanal makine diskler katı hal sürücüleri (SSD) verileri depolar. Uygulamanız için en iyi performans için premium depolama alanına yüksek IOPS gerektiren herhangi bir sanal makine disklerini geçirmek öneririz. 

Disklerinizi yüksek IOPS ihtiyacınız yoksa, standart depolama tutarak maliyetleri sınırlayabilirsiniz. Standart depolama SSD yerine sabit disk sürücülerinin (HDD'ler) üzerinde sanal makine disk verilerini depolar. Sanal makine disklerinizi premium diskleri geçirmek seçebilirsiniz. Premium diskleri çoğu sanal makine SKU'ları üzerinde desteklenir. Premium diskleri kullanmak istiyorsanız, ancak bazı durumlarda, sanal makinenize SKU'ları da yükseltmeniz gerekebilir.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Sanal makine verilerinizi yanlışlıkla silinmeye karşı koru
Sanal makine yedekleme ayarı, iş açısından kritik verilerin kullanılabilirliğini sağlar ve yanlışlıkla silme veya bozulmasına karşı koruma sağlar.  Advisor burada yedekleme etkin değil ve yedekleme etkinleştirme önerir sanal makineleri tanımlar. 

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Yüksek kullanılabilirlik önerileri Danışmanı erişme

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2.  Advisor Panoda tıklatın **yüksek kullanılabilirlik** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

Advisor önerileri hakkında daha fazla bilgi için bkz:
* [Azure Danışmanı giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor maliyet önerileri](advisor-performance-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)

