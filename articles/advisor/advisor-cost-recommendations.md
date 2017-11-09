---
title: "Azure maliyeti Advisor önerileri | Microsoft Docs"
description: "Azure dağıtımlarınızı maliyetini en iyi duruma getirmek için Azure Danışmanı'nı kullanın."
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
ms.openlocfilehash: 7b9c7037271fabd67c1ada80420ad72c340e46bb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="advisor-cost-recommendations"></a>Advisor maliyet önerileri

En iyi duruma getirme ve genel Azure azaltmak Danışmanı yardımcı olur, boşta ve az kaynaklarını tanımlayarak ayırın. Önerileri maliyet **maliyet** Danışmanı Pano sekmesinde.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>En iyi duruma getirme sanal makine harcamanız yeniden boyutlandırma veya az örnekleri kapatılıyor 
Belirli uygulama senaryoları düşük kullanımı tasarım gereği yol açabilecek olsa da, sanal makine sayısını ve boyutunu yöneterek para genellikle kaydedebilirsiniz. Advisor 14 gün boyunca, sanal makine kullanımını izler ve düşük kullanımı sanal makineleri tanımlar. Sanal makineler, CPU kullanımı yüzde 5'idir veya daha az ve ağ kullanımını 7 MB veya daha az dört veya daha fazla gün düşük kullanımı sanal makineler olarak kabul edilir.

Advisor kapatmak veya yeniden boyutlandırmak seçebilmeleri sanal makineniz çalıştırmaya devam tahmini maliyeti gösterir.

Daha az sanal makineleri saptamayı katı olmasını istiyorsanız, abonelik başına temelinde ortalama CPU kullanımı kuralı ayarlayabilirsiniz.

## <a name="use-a-cost-effective-solution-to-manage-performance-goals-of-multiple-sql-databases"></a>Birden çok SQL veritabanlarının performans hedeflerini yönetmek için uygun maliyetli bir çözüm kullanın
Advisor esnek veritabanı havuzu oluşturma yararlanabilir SQL server örnekleri tanımlar. Esnek veritabanı havuzları değişen kullanım desenlerini sahip birden çok veritabanı performans amaçlarını yönetmek için basit ve ekonomik bir çözüm sağlar. Azure esnek havuzları hakkında daha fazla bilgi için bkz: [bir Azure esnek havuz nedir?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

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
