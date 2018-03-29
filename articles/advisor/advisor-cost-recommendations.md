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
ms.openlocfilehash: 7a8807a580f1a7f1fe67e026a8fbd4cc0e96c41c
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="advisor-cost-recommendations"></a>Advisor maliyet önerileri

En iyi duruma getirme ve genel Azure azaltmak Danışmanı yardımcı olur, boşta ve az kaynaklarını tanımlayarak ayırın. Önerileri maliyet **maliyet** Danışmanı Pano sekmesinde.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>En iyi duruma getirme sanal makine harcamanız yeniden boyutlandırma veya az örnekleri kapatılıyor 
Belirli uygulama senaryoları düşük kullanımı tasarım gereği yol açabilecek olsa da, sanal makine sayısını ve boyutunu yöneterek para genellikle kaydedebilirsiniz. Advisor 14 gün boyunca, sanal makine kullanımını izler ve düşük kullanımı sanal makineleri tanımlar. Sanal makineler, CPU kullanımı yüzde 5'idir veya daha az ve ağ kullanımını 7 MB veya daha az dört veya daha fazla gün düşük kullanımı sanal makineler olarak kabul edilir.

Advisor kapatmak veya yeniden boyutlandırmak seçebilmeleri sanal makineniz çalıştırmaya devam tahmini maliyeti gösterir.

Daha az sanal makineleri saptamayı katı olmasını istiyorsanız, abonelik başına temelinde ortalama CPU kullanımı kuralı ayarlayabilirsiniz.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Sağlaması ExpressRoute bağlantı hatları ortadan kaldırarak maliyetlerini azaltma
Advisor tanımlar sağlayıcı durumu silinmiş ExpressRoute bağlantı hatları *sağlanmadı* birden fazla ay ve bağlantı hattı bağlantınızı ile sağlamak planlama olmayan bağlantı hattı silme önerir Sağlayıcı.

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
