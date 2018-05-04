---
title: Azure Site Recovery ile Azure ExpressRoute | Microsoft Docs
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery ile Azure ExpressRoute kullanmayı açıklar
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/30/2018
ms.author: manayar
ms.openlocfilehash: ffdceeba829cc77d506236274ec1d1cc160eb525
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>Azure Site Recovery ile Azure ExpressRoute

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.

Bu makalede, nasıl Azure ExpressRoute Azure Site Recovery ile olağanüstü durum kurtarma ve geçiş için kullanabileceğiniz açıklanır.

## <a name="expressroute-circuits"></a>ExpressRoute bağlantı hatları

Bir expressroute bağlantı hattı şirket içi altyapınızı ve bağlantı sağlayıcı üzerinden Microsoft bulut hizmetleri arasında mantıksal bağlantıyı temsil eder. Birden çok ExpressRoute bağlantı hatları sıralayabilirsiniz. Her bağlantı hattı, aynı veya farklı bölgelerde olabilir ve şirketinizde farklı bağlantı sağlayıcıları üzerinden bağlanır. ExpressRoute bağlantı hatları hakkında daha fazla bilgi [burada](../expressroute/expressroute-circuit-peerings.md).

## <a name="expressroute-routing-domains"></a>ExpressRoute yönlendirme etki alanları

Bir expressroute bağlantı hattı kendisiyle ilişkili birden fazla Yönlendirme etki alanları şunlardır:
-   [Azure özel eşleme](../expressroute/expressroute-circuit-peerings.md#azure-private-peering) - Azure işlem Hizmetleri, yani sanal makineler (Iaas) ve bir sanal ağ içindeki dağıtılan bulut hizmetlerini (PaaS) üzerinden özel eşleme etki alanına bağlı. Özel Eşleme etki alanı Microsoft Azure'da çekirdek ağınızı, güvenilen bir uzantısı olarak kabul edilir.
-   [Azure ortak eşleme](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) -Azure Storage, SQL veritabanları ve Web siteleri gibi hizmetleri ortak IP adreslerini sunulur. Özel olarak, bulut hizmetleriyle ortak eşleme Yönlendirme etki alanı VIP'ler dahil olmak üzere, genel IP adresleri barındırılan hizmetlere bağlanabilir. Ortak eşleme yeni oluşturmaları için kullanım dışı bırakıldı ve Microsoft Peering Azure PaaS Hizmetleri için bunun yerine kullanılmalıdır.
-   [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoft-peering) -Microsoft Çevrimiçi Hizmetleri (Office 365, Dynamics 365 ve Azure PaaS hizmetler) için bağlantı olduğu Microsoft eşleme aracılığıyla. Microsoft eşlemesi Azure PaaS hizmetlerine bağlanmak için önerilen Yönlendirme etki alanı adıdır.

Daha fazla bilgi edinmek ve ExpressRoute Yönlendirme etki alanları karşılaştırma [burada](../expressroute/expressroute-circuit-peerings.md#routing-domain-comparison).

## <a name="on-premises-to-azure-replication-with-expressroute"></a>Şirket içi ExpressRoute ile Azure çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma ve şirket içi Azure geçiş [Hyper-V sanal makineleri](hyper-v-azure-architecture.md), [VMware sanal makineleri](vmware-azure-architecture.md), ve [fiziksel sunucuları](physical-azure-architecture.md). Tüm şirket için Azure senaryoları için çoğaltma verilerinin gönderilen ve bir Azure depolama hesabında depolanır. Çoğaltma sırasında herhangi bir sanal makine ücret ödemeniz gerekmez. Azure'a yük devretme çalıştırdığınızda, Site Recovery Azure Iaas sanal makineleri otomatik olarak oluşturur.

Site Recovery, genel bir uç nokta bir Azure depolama hesabı verilerini çoğaltır. Site Recovery çoğaltma için ExpressRoute kullanmak için kullanabileceği [ortak eşleme](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) veya [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoft-peering). Microsoft eşlemesi çoğaltma için önerilen Yönlendirme etki alanıdır. Üzerinden sanal makineleri veya sunucuları Azure sanal ağını başarısız olduktan sonra bunlara erişebileceği kullanarak [özel eşleme](../expressroute/expressroute-circuit-peerings.md#azure-private-peering). Çoğaltma özel eşleme üzerinden desteklenmiyor.

Birleşik senaryo aşağıdaki çizimde gösterilen: ![şirket içi-Azure'a ExpressRoute ile](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>ExpressRoute ile Azure için Azure'a çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma [Azure sanal makineleri](azure-to-azure-architecture.md). Kullanım olup, Azure sanal makineleri bağlı olarak [Azure yönetilen diskleri](../virtual-machines/windows/managed-disks-overview.md), çoğaltma verilerinin bir Azure depolama hesabı ya da çoğaltma hedef Azure bölgesi yönetilen diskte gönderilir. Çoğaltma uç noktaları ortak olsa da, varsayılan olarak, Azure VM çoğaltma için çoğaltma trafiğini bağımsız olarak Azure bölgesi kaynak sanal ağ içinde bulunduğu Internet'e erişmez. 0.0.0.0/0 adres ön eki ile için Azure'un varsayılan sistem yolu geçersiz bir [özel rota](../virtual-network/virtual-networks-udr-overview.md#custom-routes) ve bir şirket içi ağ sanal gereç (NVA) VM trafiğine yönlendir, ancak bu yapılandırma için Site Recovery önerilmez Çoğaltma. Özel yollar kullanıyorsanız yapmanız gerekenler [bir sanal ağ hizmet uç noktası oluşturma](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) , sanal ağ "Depolama için" böylece çoğaltma trafiğini Azure sınır bırakmaz.

Çoğaltma verilerini Azure sınır bırakmaz sürece Azure VM olağanüstü durum kurtarma için ExpressRoute çoğaltma için gerekli değildir. Sanal makineler hedef Azure bölgesi yük devri sonra bunları erişebilirsiniz kullanarak [özel eşleme](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

Şirket içi veri merkezinden kaynak bölge Azure Vm'lerinde bağlanmak için ExpressRoute zaten kullanıyorsanız, yük devretme hedef bölgesi, ExpressRoute bağlantı yeniden kurmak için planlayabilirsiniz. Yeni bir sanal ağ bağlantısı üzerinden hedef bölgesi bağlanmak veya ayrı expressroute bağlantı hattı ve olağanüstü durum kurtarma için bağlantı kullanmak için aynı expressroute bağlantı hattı kullanabilirsiniz. Farklı olası senaryolar açıklanan [burada](azure-vm-disaster-recovery-with-expressroute.md#failover-models-with-expressroute).

Ayrıntılı olarak aynı coğrafi kümedeki herhangi bir Azure bölgesine Azure sanal makineleri çoğaltabilirsiniz [burada](../site-recovery/azure-to-azure-support-matrix.md#region-support). Seçilen hedef Azure bölgesi kaynağı olarak aynı coğrafi bölge içindeki değilse, ExpressRoute Premium etkinleştirmeniz gerekebilir. Daha fazla ayrıntı için [ExpressRoute konumları](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [ExpressRoute fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/expressroute/).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [ExpressRoute bağlantı hatları](../expressroute/expressroute-circuit-peerings.md).
- Daha fazla bilgi edinmek [ExpressRoute Yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Daha fazla bilgi edinmek [ExpressRoute konumları](../expressroute/expressroute-locations.md).
- Olağanüstü durum kurtarma hakkında daha fazla bilgi [ExpressRoute Azure sanal makinelerle ](azure-vm-disaster-recovery-with-expressroute.md).
