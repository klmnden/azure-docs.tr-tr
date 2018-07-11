---
title: Azure sanal makine olağanüstü durum kurtarma ile ExpressRoute kullanılarak | Microsoft Docs
description: Azure ExpressRoute, Azure Site Recovery ile Azure sanal makine olağanüstü durum kurtarma için kullanmayı açıklar
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: 73514b524f554affb9730ba63ccd608491497af2
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920479"
---
# <a name="using-expressroute-with-azure-virtual-machine-disaster-recovery"></a>Azure sanal makine olağanüstü durum kurtarma ile ExpressRoute kullanılarak

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. Bu makalede nasıl ExpressRoute Site Recovery ile Azure sanal makineleri olağanüstü durum kurtarma için kullanabileceğiniz açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce anladığınızdan emin olun:
-   ExpressRoute [devreler](../expressroute/expressroute-circuit-peerings.md)
-   ExpressRoute [Yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains)
-   Azure sanal makine [çoğaltma mimarisi](azure-to-azure-architecture.md)
-   [Çoğaltma ayarlandıktan](azure-to-azure-tutorial-enable-replication.md) Azure sanal makineleri için
-   [Yük devretmeyi](azure-to-azure-tutorial-failover-failback.md) Azure sanal makineleri

## <a name="expressroute-and-azure-virtual-machine-replication"></a>ExpressRoute ve Azure sanal makine çoğaltma

Site Recovery ile Azure sanal makineleri korurken, çoğaltma verileri bir Azure depolama hesabı veya çoğaltma hedef yönetilen Disk olup kullanın, Azure sanal makineleri bağlı olarak bir Azure bölgesine gönderilen [Azure yönetilen diskler](../virtual-machines/windows/managed-disks-overview.md). Çoğaltma uç noktaları ortak olsa da, varsayılan olarak, Azure VM çoğaltma için çoğaltma trafiğini Internet bağımsız olarak Azure bölgesi kaynak sanal ağ içinde bulunduğu erişmez.

Çoğaltma verilerini Azure sınır bırakmaz gibi Azure VM'LERİNDE olağanüstü durum kurtarma için ExpressRoute çoğaltma için gerekli değildir. Sanal makineler üzerinde hedef Azure bölgesi başarısız olduktan sonra bunları erişebilirsiniz kullanarak [özel eşdüzey hizmet sağlama](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

## <a name="replicating-azure-deployments"></a>Azure dağıtımları çoğaltılıyor

Bir önceki [makale](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity), müşterinin şirket içi veri merkezine ExpressRoute aracılığıyla Azure sanal ağına bağlı bir basit bir kurulumla açıklanmaktadır. Tipik kurumsal dağıtımlar sahip iş yükleri birden fazla Azure sanal ağları arasında bölmek ve merkezi bağlantı hub hem internet hem de şirket içi dağıtımlar için dış bağlantı kurar.

Bu örnekte, Kurumsal dağıtımlarında ortak olan bir hub ve bağlı bileşen topolojisi açıklanmaktadır:
-   Dağıtımı **Azure Doğu Asya** bölge ve şirket içi veri Merkezinize Hong Kong iş ortağı edge'de aracılığıyla bir ExpressRoute bağlantı hattı bağlantısı vardır.
-   Uygulamaları iki uç sanal ağlarda – dağıtılan **kaynak VNet1** adres alanı 10.1.0.0/24 ile ve **kaynak vnet2'den** adres alanı 10.2.0.0/24 ile.
-   Hub sanal ağı **kaynak Hub VNet**, adres alanıyla 10.10.10.0/24 ağ geçidi davranır. Alt ağlar arasındaki tüm iletişimi hub'ı aracılığıyla gider.
-   Hub sanal ağı iki alt ağa – sahip **NVA alt ağı** adres alanı 10.10.10.0/25 ile ve **ağ geçidi alt ağı** adres alanı 10.10.10.128/25 ile.
-   **NVA alt ağı** 10.10.10.10 IP adresiyle bir ağ sanal gereci sahiptir.
-   **Ağ geçidi alt ağı** yönlendiren bir özel eşdüzey hizmet sağlama Yönlendirme etki alanı aracılığıyla müşteri şirket içi bir veri merkezine ExpressRoute bağlantısı için bir ExpressRoute ağ geçidi bağlı sahiptir.
-   Her bir uç sanal ağ merkez sanal ağa bağlandığından ve bu ağ topolojisi kapsamındaki tüm yönlendirme Azure rota tabloları (UDR) aracılığıyla denetlenir. Tüm giden trafiği bir sanal ağı diğer sanal ağa veya şirket içi veri merkezine NVA üzerinden yönlendirilir.

![Şirket içi-Azure'a yük Devretmeden önce ExpressRoute ile](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

### <a name="hub-and-spoke-peering"></a>Hub ve bağlı bileşen eşleme

Hub eşlemesi için uç yapılandırması aşağıdaki gibidir:
-   Sanal ağ adresi izin ver: etkin
-   İletilen trafiğe izin verecek: etkin
-   Ağ geçidi aktarımına izin ver: devre dışı
-   Ağ geçitleri kaldırmak: etkin

 ![Hub için eşleme yapılandırma uç](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

Eşleme uç için hub'ı aşağıdaki yapılandırmaya sahip:
-   Sanal ağ adresi izin ver: etkin
-   İletilen trafiğe izin verecek: etkin
-   Ağ geçidi aktarımına izin ver: etkin
-   Ağ geçitleri kaldırmak: devre dışı

 ![Hub'ı uç eşleme yapılandırması](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="enabling-replication-for-the-deployment"></a>Dağıtım için çoğaltmayı etkinleştirme

Yukarıdaki kurulumu için ilk [olağanüstü durum kurtarma ayarlama](azure-to-azure-tutorial-enable-replication.md) Site Recovery kullanarak tüm sanal makineleri için. Site Recovery, çoğaltma üzerinde hedef bölge (alt ağlar ve ağ geçidi alt ağları dahil) sanal ağlar oluşturma ve kaynak ve hedef sanal ağlar arasında gerekli eşlemeler oluşturun. Ayrıca, hedef tarafı ağlar ve alt ağlar önceden oluşturabilir ve aynı çoğaltmayı etkinleştirirken kullanabilirsiniz.

Site Recovery, rota tabloları, sanal ağ geçitleri, sanal ağ geçidi bağlantısı, sanal ağ eşlemesi, veya tüm diğer ağ kaynakları veya bağlantıları çoğaltmaz. Bu ve diğer kaynakların parçası [çoğaltma işlemi](azure-to-azure-architecture.md#replication-process) sırasında ya da yük devretmeden önce oluşturulan ve ilgili kaynaklara bağlı gerekir. Azure Site Recovery'nin güçlü kullanabileceğiniz [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturma ve ek kaynaklar Otomasyon betikleri kullanarak bağlanma otomatik hale getirmek için.

Varsayılan olarak, çoğaltma trafiğini Azure sınır bırakmaz. Genellikle, NVA dağıtımları da NVA üzerinden akmasını giden Internet trafiğini zorlar varsayılan yolun (0.0.0.0/0) tanımlayın. Tüm çoğaltma trafiğinin NVA üzerinden geçiyorsa bu durumda, gereç kısıtlanmazsınız. Aynı de şirket içi dağıtımlar için tüm Azure VM trafik yönlendirme için varsayılan yollar kullanılırken geçerlidir. Öneririz [bir sanal ağ hizmet uç noktası oluşturma](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) , sanal ağ için "depolama" böylece çoğaltma trafiğini Azure sınır bırakmaz.

## <a name="failover-models-with-expressroute"></a>ExpressRoute ile yük devretme modelleri

Azure sanal makineleri farklı bir bölgeye yük devredildi, mevcut bir ExpressRoute bağlantısı kaynak sanal ağ için otomatik olarak hedef sanal ağ ile kurtarma bölgesinde aktarılmaz. Yeni bir bağlantı, ExpressRoute hedef sanal ağa bağlanmak için gereklidir.

Herhangi bir Azure bölgesine ayrıntılı olarak aynı coğrafi kümedeki Azure sanal makinelerini çoğaltma [burada](azure-to-azure-support-matrix.md#region-support). Seçtiğiniz hedef Azure bölgesi aynı jeopolitik bölgede kaynağı olarak içinde değilse, kaynak ve hedef bölge bağlantısını için tek bir ExpressRoute bağlantı hattı kullanıyorsanız, ExpressRoute Premium etkinleştirmeniz gerekir. Daha fazla ayrıntı için denetleyin [ExpressRoute konumları](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [ExpressRoute fiyatlandırması](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="two-expressroute-circuits-in-two-different-expressroute-peering-locations"></a>İki farklı ExpressRoute eşleme konumlarına iki ExpressRoute işlem hatları
-   Bu yapılandırma, birincil ExpressRoute bağlantı hattının hataya karşı ve ayrıca ExpressRoute eşleme konumlarına etkiler ve birincil ExpressRoute devreniz kesintiye büyük ölçekli bölgesel felaketlere karşı sağlamak istiyorsanız kullanışlıdır.
-   Normalde üretim ortamına bağlı devre devrenin birincil ve ikincil bağlantı hattını bir hatasız ve genellikle daha düşük bant genişliği kullanılır. İkincil bant genişliği, ikincil birincil devraldığınız gerekir zaman bir olağanüstü durum olayına artırılabilir.
-   Bu yapılandırma ile genel kurtarma zamanı azaltır ikincil ExpressRoute devreniz bağlantılardan hedef sanal ağ için yük devretme sonrası veya bağlantı kurulu ve bir olağanüstü durum bildirimi için hazır kurabilirsiniz. Hem birincil hem de eşzamanlı bağlantı ve hedef bölge sanal ağları ile şirket yönlendirme içi bağlantı yalnızca yük devretme işleminden sonra ve ikincil bağlantı hattını kullandığından emin olmak.
-   Site Recovery ile korunan VM'ler için kaynak ve hedef sanal ağlar, gereksinim başına yük devretme sırasında aynı veya farklı IP adresi olabilir. Her iki durumda da, yük devretme öncesinde ikincil bağlantı kurulabilir.

### <a name="two-expressroute-circuits-in-the-same-expressroute-peering-location"></a>İki ExpressRoute bağlantı hatları ile aynı konumda ExpressRoute eşlemesi
-   Bu yapılandırma, birincil ExpressRoute devresinin hatasına karşı güvence, ancak büyük ölçekli bölgesel felaketlere karşı ExpressRoute eşleme konumlarına etkileyebilir değil. İkinci ile birincil ve ikincil devreler etkilenmiş.
-   Diğer koşulları IP adresleri ve bağlantı için önceki durum'içinde aynı kalır. Eşzamanlı bağlantı şirket içi veri merkezinden birincil bağlantı hattını sanal ağla kaynak ve hedef sanal ağı ikincil bağlantı hattını ile olabilir. Hem birincil hem de eşzamanlı bağlantı ve hedef bölge sanal ağları ile şirket yönlendirme içi bağlantı yalnızca yük devretme işleminden sonra ve ikincil bağlantı hattını kullandığından emin olmak.
-   Eşleme aynı konumda devreler oluşturulduğunda her iki bağlantı hatları aynı sanal ağa bağlanamıyor.

### <a name="single-expressroute-circuit"></a>Tek bir ExpressRoute bağlantı hattı
-   Bu yapılandırma, hangi ExpressRoute eşleme konumu etkileyebilecek büyük ölçekli bölgesel bir olağanüstü durum karşı Sigortası değil.
-   Tek bir ExpressRoute bağlantı hattıyla, kaynağına bağlanmak ve sanal ağlar aynı anda bağlantı hattı için hedef olarak aynı IP adresi alanı üzerinde hedef bölge kullanılır.
-   Aynı IP adresi alanına hedef bölge üzerinde kullanıldığında, kaynak tarafı bağlantı kesilmesi gerekir ve bundan sonra hedef tarafı bağlantı kuruldu. Bu bağlantı değişiklik kurtarma planının bir parçası yazılabilir.
-   Birincil bölge erişilemiyorsa, bölgesel bir hata, bağlantıyı kesme işlemi başarısız olabilir. Hedef sanal ağda aynı IP adresi alanı kullanıldığında böyle bir kesinti bağlantı oluşturma için hedef bölgede etkileyebilir.
-   İki eşzamanlı bağlantı aynı adres alanına bağlanmaya çalışırsanız hedefte bağlantı oluşturma işlemi başarılı olur ve daha sonra birincil bölgeye kurtarır, paket düşme karşılaşabileceği. Paket bir bırakılanlar önlemek için birincil bağlantı hemen sonlandırılmalıdır. İkincil bağlantı bağlantısını kestikten sonra birincil bölgeye, birincil bağlantı can yeniden sanal makinelerin sonrası yeniden çalışma kurulabilir.
-   Hedef sanal ağdaki farklı bir adres alanı kullanılır yaparsanız, ardından aynı anda kaynağına bağlanmak ve hedef aynı ExpressRoute bağlantı hattına sanal ağları.

## <a name="recovering-azure-deployments"></a>Azure dağıtımları kurtarma
İki farklı eşleme konumları ve korumalı Azure sanal makineleri için özel IP adreslerinin bekletme farklı iki ExpressRoute bağlantı hatları ile yük devretme modeli göz önünde bulundurun. Hedef kurtarma bölgedir Azure Güneydoğu Asya ve bir iş ortağı uç Singapur aracılığıyla ikincil bir ExpressRoute bağlantı hattı bağlantı kurulur.

Kurtarma sanal makineler ve sanal ağlar, çoğaltmaya ek olarak tüm dağıtım işlemlerini otomatikleştirmek için diğer ilgili ağ kaynakları ve bağlantıları da oluşturulması gerekir. Adımları aşağıdaki ek eski merkez ve uç ağ topolojisi için sırasında veya sonrasında gerçekleştirilmesi gereken [yük devretme](azure-to-azure-tutorial-failover-failback.md) işlemi:
1.  Hedef bölge hub sanal ağında Azure ExpressRoute ağ geçidini oluşturun. ExpressRoute ağ geçidini, hedef hub sanal ağı ExpressRoute devresine bağlama için gereklidir.
2.  Sanal ağ bağlantısı hedef merkez sanal ağdaki hedef ExpressRoute bağlantı hattı oluşturun.
3.  Hedef bölgenin hub ve bağlı sanal ağlar arasında VNet eşlemesi ayarlayın. Hedef bölge eşleme özellikleri bulunan kaynak bölge ile aynı olacaktır.
4.  Merkez sanal ağdaki Udr'leri ve iki uç sanal ağları ayarlayın. Özellikleri kullanarak aynı IP adreslerini hedef tarafı Udr'ler kaynak tarafında aynıdır. Farklı bir hedef IP adresi ile Udr uygun şekilde değiştirilmesi gerekir.

Yukarıdaki adımları bir parçası olarak yazılabilir bir [kurtarma planı](site-recovery-create-recovery-plans.md). Uygulama bağlantı ve kurtarma zamanı gereksinimlerinize bağlı olarak yukarıdaki adımları da yük devretme başlatılmadan önce tamamlanabilir.

Kurtarma sanal makinelerin ve bağlantı adımları gönderi yapın, Kurtarma Ortamı'nı şu şekilde görünür: ![şirket-için-Azure'a yük devredildikten sonra ExpressRoute ile](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)

Basit bir topoloji ile tek ExpressRoute bağlantı hattı, hedef sanal makineler, aynı IP ile Azure VM olağanüstü durum kurtarma için ayrıntılı [burada](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity).

## <a name="recovery-time-objective-rto-considerations"></a>Kurtarma süresi hedefi (RTO) konuları
Dağıtımınız için genel kurtarma süresini kısaltmak için sağlama ve ek hedef bölgede dağıtma önerilir [ağ bileşenleri](azure-vm-disaster-recovery-with-expressroute.md#enabling-replication-for-the-deployment) önceden sanal ağ geçitleri gibi. Küçük bir kapalı kalma süresi, ek kaynaklar dağıtma ile ilişkilendirilir ve bu kapalı kalma süresi genel kurtarma süresini etkileyebilir, aksi takdirde planlaması sırasında belirlenmiştir.

Normal çalıştırmanızı öneririz [olağanüstü durum kurtarma tatbikatlarını](azure-to-azure-tutorial-dr-drill.md) korumalı dağıtımları. Tatbikat, çoğaltma stratejinizi veri kaybı veya kesinti süresi olmadan doğrular ve üretim ortamınızı etkilemez. Tatbikat çalıştıran, Kurtarma süresi hedefi olumsuz yönde etkileyebilir, geçen dakikada yapılandırma sorunlarını önler. Yük devretme testi için çoğaltmayı etkinleştirdiğinizde ayarlanmış varsayılan ağ yerine ayrı bir Azure VM ağını kullanmanızı öneririz.

Tek bir ExpressRoute bağlantı hattı kullanıyorsanız, bölgesel bir olağanüstü sırasında bağlantı kurma sorunları önlemek için hedef sanal ağ için farklı bir IP adresi alanını kullanmanızı öneririz. Farklı IP adreslerini kullanarak kurtarılan üretim ortamınız için uygun değilse, iki sanal ağ IP çakışan bağlanamıyorsunuz gibi olağanüstü durum kurtarma tatbikatı test yük devretmesi farklı IP adreslerine sahip ayrı bir test ağı yapılması Adres alanı aynı ExpressRoute bağlantı hattına.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [ExpressRoute devreleri](../expressroute/expressroute-circuit-peerings.md).
- Daha fazla bilgi edinin [ExpressRoute Yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Daha fazla bilgi edinin [ExpressRoute konumları](../expressroute/expressroute-locations.md).
- Daha fazla bilgi edinin [kurtarma planları](site-recovery-create-recovery-plans.md) uygulama yük devretme otomatikleştirmek için.
