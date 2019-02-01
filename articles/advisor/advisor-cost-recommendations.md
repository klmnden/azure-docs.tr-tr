---
title: Azure Danışmanı'nı kullanarak hizmet maliyetlerinin azaltılmasına | Microsoft Docs
description: Azure Danışmanı, Azure dağıtımlarınızı maliyetini en iyi duruma getirmek için kullanın.
services: advisor
documentationcenter: NA
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.author: kasparks
ms.openlocfilehash: c76c7bdb398184cc297831c9395063e7bf0f6bdc
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492547"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>Azure Danışmanı'nı kullanarak hizmet maliyetlerini azaltın

En iyi duruma getirmek ve genel Azure azaltmak advisor yardımcı olur, boş ve az kullanılan kaynakları belirleyerek ayırın. Önerileri maliyet **maliyet** Danışman Panosu sekmesinde.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>En iyi duruma getirme sanal makine harcama yeniden boyutlandırabilir veya az kullanılan örneklerini kapatılıyor 

Uygulama belirli senaryolar tasarım gereği düşük kullanımı neden olabilir ancak, genellikle, sanal makinelerin sayısını ve boyutunu yöneterek tasarruf sağlayabilirsiniz. Advisor için 14 gün, sanal makine kullanımını izler ve ardından kullanımı düşük sanal makineleri tanımlar. Sanal makineler, CPU kullanımı: yüzde 5 veya daha az ve ağ kullanımını 7 MB veya daha az dört veya daha fazla gün kullanımı düşük sanal makine olarak kabul edilir.

Advisor da kapatın veya yeniden boyutlandırın seçebilir, sanal makinenizi çalıştırmaya devam tahmini maliyeti gösterir.

Az kullanılan sanal makineleri saptamayı daha ısrarlı olmasını istiyorsanız, ortalama CPU kullanımı Kural başına abonelik temelinde ayarlayabilirsiniz.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Sağlaması yapılmamış bir ExpressRoute bağlantı hatları ortadan kaldırarak maliyetleri azaltın

Advisor tanımlayan sağlayıcı durumu içinde olan ExpressRoute bağlantı hatları *sağlanmadı* birden fazla ay ve bağlantınızı ile bağlantı hattını sağlamasını planlamıyorsanız devreyi önerir Sağlayıcı.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Silme veya boşta olan sanal ağ geçitlerini yeniden maliyetleri azaltın

Advisor 90 gün boyunca boşta sanal ağ geçitleri tanımlar. Bu ağ geçidi saatlik olarak faturalandırılır ve sonra yeniden yapılandırma ya da artık kullanmayı düşünmüyorsanız, silerek düşünmelisiniz. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Kullandıkça Öde maliyetlerinden tasarruf sağlamak için ayrılmış sanal makine örnekleri satın alın

Danışman son 30 gün içindeki sanal makine kullanımınızı gözden geçirin ve bir Azure rezervasyon satın alarak para tasarrufu, belirleyebilirsiniz. Danışman, potansiyel olarak çoğu tasarruf sahip ve rezervasyon satın alma gelen tahmini tasarruf gösterecektir boyutları ve bölgeler gösterilir. 

Azure ayırma ile taban maliyetlerini sanal makineleriniz için önceden satın alabilirsiniz. İndirimler aynı büyüklük ve bölge ayırmalarınızın yeni veya mevcut Vm'leri otomatik olarak uygulanır. [Azure ayrılmış VM örnekleri hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Maliyet önerileri Azure Danışmanı'nda erişme

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2.  Advisor panosunda **maliyet** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz:
* [Advisor giriş](advisor-overview.md)
* [Kullanmaya Başlama](advisor-get-started.md)
* [Danışmanı performans önerileri](advisor-cost-recommendations.md)
* [Advisor yüksek kullanılabilirlik önerisi](advisor-cost-recommendations.md)
* [Advisor güvenlik önerileri](advisor-cost-recommendations.md)
