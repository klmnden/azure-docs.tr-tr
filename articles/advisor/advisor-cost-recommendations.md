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
ms.openlocfilehash: 188a79df99a174436808acd3d964abf9357cf4c0
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699339"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>Azure Danışmanı'nı kullanarak hizmet maliyetlerini azaltın

En iyi duruma getirmek ve genel Azure azaltmak advisor yardımcı olur, boş ve az kullanılan kaynakları belirleyerek ayırın. Önerileri maliyet **maliyet** Danışman Panosu sekmesinde.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>En iyi duruma getirme sanal makine harcama yeniden boyutlandırabilir veya az kullanılan örneklerini kapatılıyor 

Uygulama belirli senaryolar tasarım gereği düşük kullanımı neden olabilir ancak, genellikle, sanal makinelerin sayısını ve boyutunu yöneterek tasarruf sağlayabilirsiniz. Advisor için 7 gün, sanal makine kullanımını izler ve ardından kullanımı düşük sanal makineleri tanımlar. Sanal makineler, düşük kullanımı, CPU kullanımı % 5'ise veya daha az olarak kabul edilir ve kendi ağ kullanımı % 2'den az veya daha küçük bir sanal makine boyutu tarafından yerleştirilebilecek geçerli iş yükünü.

Advisor da kapatın veya yeniden boyutlandırın seçebilir, sanal makinenizi çalıştırmaya devam tahmini maliyeti gösterir.

Az kullanılan sanal makineleri saptamayı daha ısrarlı olmasını istiyorsanız, ortalama CPU kullanımı Kural başına abonelik temelinde ayarlayabilirsiniz.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Sağlaması yapılmamış bir ExpressRoute bağlantı hatları ortadan kaldırarak maliyetleri azaltın

Advisor tanımlayan sağlayıcı durumu içinde olan ExpressRoute bağlantı hatları *sağlanmadı* birden fazla ay ve bağlantınızı ile bağlantı hattını sağlamasını planlamıyorsanız devreyi önerir Sağlayıcı.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Silme veya boşta olan sanal ağ geçitlerini yeniden maliyetleri azaltın

Advisor 90 gün boyunca boşta sanal ağ geçitleri tanımlar. Bu ağ geçidi saatlik olarak faturalandırılır ve sonra yeniden yapılandırma ya da artık kullanmayı düşünmüyorsanız, silerek düşünmelisiniz. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Kullandıkça Öde maliyetlerinden tasarruf sağlamak için ayrılmış sanal makine örnekleri satın alın

Danışman son 30 gün içindeki sanal makine kullanımınızı gözden geçirin ve bir Azure rezervasyon satın alarak para tasarrufu, belirleyebilirsiniz. Danışman, potansiyel olarak çoğu tasarruf sahip ve rezervasyon satın alma gelen tahmini tasarruf gösterecektir boyutları ve bölgeler gösterilir. Azure ayırma ile taban maliyetlerini sanal makineleriniz için önceden satın alabilirsiniz. İndirimler aynı büyüklük ve bölge ayırmalarınızın yeni veya mevcut Vm'leri otomatik olarak uygulanır. [Azure ayrılmış VM örnekleri hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/reserved-vm-instances/)

Danışman, sonraki 30 gün içinde sona erecek olan ayrılmış örnekleri de bildirir. Kullandıkça Öde fiyatlandırması ödeme yapmaktan kaçınmak üzere yeni ayrılmış örnekler satın önerir.

## <a name="delete-unassociated-public-ip-addresses-to-save-money"></a>İlişkilendirilmemiş genel IP adreslerini tasarruf Sil

Danışman, yük Dengeleyiciler veya VM'ler gibi Azure kaynakları için şu anda ilişkili olmayan genel IP adresleri tanımlar. Bu genel IP adreslerinin nominal bir ücret ile gelir. Bunları kullanmayı planlamıyorsanız silerek maliyet tasarruf sağlayabilir.

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
