---
title: Azure maliyeti Advisor önerileri | Microsoft Docs
description: Azure dağıtımlarınızı maliyetini en iyi duruma getirmek için Azure Danışmanı'nı kullanın.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: ade6ef996c00c0c06d5b8e44815520e6e4ab7e9f
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34735876"
---
# <a name="advisor-cost-recommendations"></a>Advisor maliyet önerileri

En iyi duruma getirme ve genel Azure azaltmak Danışmanı yardımcı olur, boşta ve az kaynaklarını tanımlayarak ayırın. Önerileri maliyet **maliyet** Danışmanı Pano sekmesinde.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>En iyi duruma getirme sanal makine harcamanız yeniden boyutlandırma veya az örnekleri kapatılıyor 
Belirli uygulama senaryoları düşük kullanımı tasarım gereği yol açabilecek olsa da, sanal makine sayısını ve boyutunu yöneterek para genellikle kaydedebilirsiniz. Advisor 14 gün boyunca, sanal makine kullanımını izler ve düşük kullanımı sanal makineleri tanımlar. Sanal makineler, CPU kullanımı yüzde 5'idir veya daha az ve ağ kullanımını 7 MB veya daha az dört veya daha fazla gün düşük kullanımı sanal makineler olarak kabul edilir.

Advisor kapatmak veya yeniden boyutlandırmak seçebilmeleri sanal makineniz çalıştırmaya devam tahmini maliyeti gösterir.

Daha az sanal makineleri saptamayı katı olmasını istiyorsanız, abonelik başına temelinde ortalama CPU kullanımı kuralı ayarlayabilirsiniz.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Sağlaması ExpressRoute bağlantı hatları ortadan kaldırarak maliyetlerini azaltma
Advisor tanımlar sağlayıcı durumu silinmiş ExpressRoute bağlantı hatları *sağlanmadı* birden fazla ay ve bağlantı hattı bağlantınızı ile sağlamak planlama olmayan bağlantı hattı silme önerir Sağlayıcı.

## <a name="buy-virtual-machine-reserved-instances-to-save-money-over-pay-as-you-go-costs"></a>"Kullandıkça öde" maliyetlerinden tasarruf sağlamak için ayrılmış sanal makine örnekleri satın alın
Advisor, sanal makine kullanımını son 30 gün içinde gözden geçirin ve ayrılmış örnekler satın alarak paradan tasarruf, belirleyin. Advisor bölgeler ve burada olası çoğu tasarrufları sahip ve ayrılmış örnekler satın gelen tahmini tasarrufları gösterecektir boyutları gösterir. 

Ayrılmış örnekler ile temel maliyetleri sanal makineleriniz için önceden satın alabilirsiniz. İndirim, aynı boyut ve bölge ayrılmış örneklerinizi sahip yeni veya var olan sanal makineleri otomatik olarak uygulanır. [Azure ayrılmış VM örnekleri hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Maliyet önerileri Azure Danışmanı erişme

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2.  Advisor Panoda tıklatın **maliyet** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

Advisor önerileri hakkında daha fazla bilgi için bkz:
* [Advisor giriş](advisor-overview.md)
* [Kullanmaya Başlama](advisor-get-started.md)
* [Advisor performans önerileri](advisor-cost-recommendations.md)
* [Advisor yüksek kullanılabilirlik önerileri](advisor-cost-recommendations.md)
* [Advisor güvenlik önerileri](advisor-cost-recommendations.md)
