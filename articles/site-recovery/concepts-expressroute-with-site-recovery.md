---
title: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery ile Azure ExpressRoute kullanarak hakkında | Microsoft Docs
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery hizmeti ile Azure ExpressRoute kullanmayı açıklar.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 6/27/2019
ms.author: mayg
ms.openlocfilehash: 35fa26112a6026ab05bd59b38621de7ee802c715
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491905"
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>Azure Site Recovery ile Azure ExpressRoute

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.

Bu makalede, nasıl Azure ExpressRoute Azure Site Recovery ile olağanüstü durum kurtarma ve geçiş için kullanabileceğiniz açıklanır.

## <a name="expressroute-circuits"></a>ExpressRoute devreleri

Bir ExpressRoute bağlantı hattı, şirket içi altyapınızı ve bağlantı sağlayıcı üzerinden Microsoft bulut hizmetleri arasında mantıksal bağlantıyı temsil eder. Birden çok ExpressRoute bağlantı hattına sipariş edebilirsiniz. Her bağlantı hattı aynı veya farklı bölgelerde olabilir ve farklı bağlantı sağlayıcıları aracılığıyla şirket içinde bağlanabilir. ExpressRoute bağlantı hatları hakkında daha fazla bilgi [burada](../expressroute/expressroute-circuit-peerings.md).

ExpressRoute devresi, kendisiyle ilişkili birden fazla Yönlendirme etki alanları vardır. Daha fazla bilgi edinin ve ExpressRoute Yönlendirme etki alanları karşılaştırın [burada](../expressroute/expressroute-circuit-peerings.md#peeringcompare).

## <a name="on-premises-to-azure-replication-with-expressroute"></a>Şirket içi ExpressRoute ile Azure'a çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma ve azure'a geçiş için şirket içi [Hyper-V sanal makinelerini](hyper-v-azure-architecture.md), [VMware sanal makinelerini](vmware-azure-architecture.md), ve [fiziksel sunucuları](physical-azure-architecture.md). Tüm şirket içi Azure senaryoları için çoğaltma verileri gönderilen ve bir Azure depolama hesabında depolanır. Çoğaltma sırasında herhangi bir sanal makine ücreti ödeme yapmayın. Azure'a yük devretme çalıştırdığınızda, Site Recovery, Azure Iaas sanal makineleri otomatik olarak oluşturur.

Site Recovery veri genel bir uç nokta bir Azure depolama hesabı veya çoğaltma yönetilen Disk hedef Azure bölgesi çoğaltır. Site Recovery çoğaltma trafiği için ExpressRoute kullanmak için kullanabileceği [Microsoft eşlemesi](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) veya varolan [genel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#publicpeering) (yeni oluşturma işlemleri için kullanım dışı). Microsoft eşlemesi, çoğaltma için önerilen Yönlendirme etki alanıdır. Özel eşdüzey hizmet sağlama üzerinden yineleme desteklenmiyor unutmayın.

Emin [ağ gereksinimleri](vmware-azure-configuration-server-requirements.md#network-requirements) için yapılandırma sunucusunu da karşılandığından. Belirli URL'lere bağlantısı yapılandırma sunucusu tarafından Site Recovery çoğaltma düzenleme için gereklidir. ExpressRoute için bağlantı kullanılamaz. 

Şirket içi proxy kullanın ve çoğaltma trafiği için Expressroute'u kullanmak istiyorsanız durumunda, Proxy atlama listesi yapılandırma sunucusu ve işlem sunucusu yapılandırmanız gerekir. Aşağıdaki adımları izleyin:

- PsExec aracını indirin [burada](https://aka.ms/PsExec) sistem kullanıcı bağlamı erişmek için.
- Internet Explorer açın sistem kullanıcı bağlamında aşağıdaki komut satırı psexec -s çalıştırarak -i "%ProgramFiles%\Internet Explorer\iexplore.exe"
- IE'de proxy ayarları Ekle
- Atlama listesi içinde Azure depolama URL'si ekleme *. blob.core.windows.net

Bu proxy üzerinden iletişime geçebilirsiniz ancak çoğaltma trafiği yalnızca ExpressRoute aracılığıyla akar garanti eder.

Sanal makine ya da sunucuları için bir Azure sanal ağı yük devretme sonra erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering). 

Birleşik senaryo, aşağıdaki diyagramda gösterilmiştir: ![Şirket içi-Azure'a ExpressRoute ile](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>ExpressRoute ile azure'dan Azure'a çoğaltma

Azure Site kurtarma sağlayan olağanüstü durum kurtarma [Azure sanal makineleri](azure-to-azure-architecture.md). İster kullanın, Azure sanal makineleri bağlı olarak [Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md), çoğaltma verileri bir Azure depolama hesabı veya çoğaltma yönetilen Disk üzerinde ' % s'hedef Azure bölgeniz için gönderilir. Çoğaltma uç noktaları ortak olsa da, varsayılan olarak, Azure VM çoğaltma için çoğaltma trafiğini Internet bağımsız olarak Azure bölgesi kaynak sanal ağ içinde bulunduğu erişmez. Azure'nın varsayılan sistem yolu 0.0.0.0/0 adres ön eki ile geçersiz kılabilirsiniz bir [özel rota](../virtual-network/virtual-networks-udr-overview.md#custom-routes) ve bir şirket içi ağ sanal Gereci (NVA) VM trafiğine yöneltmektir, ancak bu yapılandırma, Site Recovery için önerilmez Çoğaltma. Özel yollar kullanıyorsanız yapmanız gerekenler [bir sanal ağ hizmet uç noktası oluşturma](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) , sanal ağ için "depolama" böylece çoğaltma trafiğini Azure sınır bırakmaz.

Azure VM'LERİNDE olağanüstü durum kurtarma için varsayılan olarak, ExpressRoute çoğaltma için gerekli değildir. Sanal makineler üzerinde hedef Azure bölgesi başarısız olduktan sonra bunları erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#privatepeering).

Kaynak bölgesi Azure Vm'lerinde, şirket içi veri merkezinizden bağlanmak için zaten ExpressRoute kullanıyorsanız, yük devretme hedef bölgede ExpressRoute bağlantınızın yeniden kurmak için planlayabilirsiniz. Hedef bölgede yeni bir sanal ağ bağlantısı üzerinden bağlanmak veya ayrı bir ExpressRoute bağlantı hattı ve olağanüstü durum kurtarma için bağlantı kullanmak için aynı ExpressRoute bağlantı hattı kullanabilirsiniz. Farklı olası senaryolar açıklanmıştır [burada](azure-vm-disaster-recovery-with-expressroute.md#fail-over-azure-vms-when-using-expressroute).

Herhangi bir Azure bölgesine ayrıntılı olarak aynı coğrafi kümedeki Azure sanal makinelerini çoğaltma [burada](../site-recovery/azure-to-azure-support-matrix.md#region-support). Seçtiğiniz hedef Azure bölgesi aynı jeopolitik bölgede kaynağı olarak içinde değilse, ExpressRoute Premium etkinleştirmeniz gerekebilir. Daha fazla ayrıntı için denetleyin [ExpressRoute konumları](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [ExpressRoute fiyatlandırması](https://azure.microsoft.com/pricing/details/expressroute/).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [ExpressRoute devreleri](../expressroute/expressroute-circuit-peerings.md).
- Daha fazla bilgi edinin [ExpressRoute Yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#peeringcompare).
- Daha fazla bilgi edinin [ExpressRoute konumları](../expressroute/expressroute-locations.md).
- Olağanüstü durum kurtarma hakkında daha fazla bilgi [ExpressRoute ile Azure sanal makineleri](azure-vm-disaster-recovery-with-expressroute.md).
