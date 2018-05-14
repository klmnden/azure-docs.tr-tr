---
title: Azure sanal makinesi olağanüstü durum kurtarma ile ExpressRoute kullanarak | Microsoft Docs
description: Azure ExpressRoute, Azure Site Recovery ile Azure sanal makinesi olağanüstü durum kurtarma için kullanılacak açıklar
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 44ecbcc51cb53f4d7b68f5c5e24e7d81c5a4208c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="using-expressroute-with-azure-virtual-machine-disaster-recovery"></a>Azure sanal makinesi olağanüstü durum kurtarma ile ExpressRoute kullanma

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. Bu makalede nasıl ExpressRoute Site Recovery ile Azure sanal makinelerin olağanüstü durum kurtarma için kullanabileceğiniz açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce anladığınızdan emin olun:
-   ExpressRoute [bağlantı hatları](../expressroute/expressroute-circuit-peerings.md)
-   ExpressRoute [Yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains)
-   Azure sanal makinesi [çoğaltma mimarisi](azure-to-azure-architecture.md)
-   [Çoğaltma ayarı](azure-to-azure-tutorial-enable-replication.md) Azure sanal makineleri için
-   [Devretmek](azure-to-azure-tutorial-failover-failback.md) Azure sanal makineler

## <a name="expressroute-and-azure-virtual-machine-replication"></a>ExpressRoute ve Azure sanal makine çoğaltma

Site Recovery ile Azure sanal makineleri korurken, çoğaltma verilerinin bir Azure depolama hesabı ya da çoğaltma hedef yönetilen diskte mi kullanın, Azure sanal makineleri bağlı olarak Azure bölgesi gönderilir [Azure yönetilen diskleri](../virtual-machines/windows/managed-disks-overview.md). Çoğaltma uç noktaları ortak olsa da, varsayılan olarak, Azure VM çoğaltma için çoğaltma trafiğini bağımsız olarak Azure bölgesi kaynak sanal ağ içinde bulunduğu Internet'e erişmez.

Çoğaltma verilerini Azure sınır bırakmaz gibi Azure VM olağanüstü durum kurtarma için ExpressRoute çoğaltma için gerekli değildir. Sanal makineler hedef Azure bölgesi yük devri sonra bunları erişebilirsiniz kullanarak [özel eşleme](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

## <a name="replicating-azure-deployments"></a>Çoğaltılan Azure dağıtımları

Daha önceki bir [makale](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity), basit bir kurulum bir Azure sanal ağı bağlı müşteri şirket içi veri merkezine ExpressRoute aracılığıyla açıklanan. Tipik kurumsal dağıtımlar birden fazla Azure sanal ağlar arasında bölme iş yükleri vardır ve bir merkezi bağlantı hub'ı hem internet hem de şirket içi dağıtımlar için harici bağlantı kurar.

Bu örnekte, kuruluş dağıtımlarında ortak olan bir hub ve bağlı bileşen topolojisi açıklanmaktadır:
-   Dağıtım bulunduğu **Azure Doğu Asya** bölge ve şirket içi veri merkezi Hong Kong, iş ortağı ucu aracılığıyla ExpressRoute bağlantı hattı bağlantısı vardır.
-   Uygulamalar, iki bağlı sanal ağlar arasında – dağıtıldığı **kaynak VNet1** adres alanı 10.1.0.0/24 ile ve **kaynak vnet2'yi** adres alanı 10.2.0.0/24 ile.
-   Hub sanal ağ **kaynak Hub VNet**, adres alanıyla 10.10.10.0/24 ağ geçidi davranır. Alt ağlar arasında tüm iletişimi hub'ı aracılığıyla gider.
-   Hub sanal ağı iki alt – sahiptir **NVA alt** adres alanı 10.10.10.0/25 ile ve **ağ geçidi alt ağı** adres alanı 10.10.10.128/25 ile.
-   **NVA alt** 10.10.10.10 IP adresiyle bir ağ sanal gereç sahiptir.
-   **Ağ geçidi alt ağı** özel eşleme Yönlendirme etki alanı ile müşterinin şirket içi veri merkezine yönlendiren bir ExpressRoute bağlantı bağlı olan bir ExpressRoute ağ geçidi.
-   Her bağlı sanal ağ hub sanal ağa bağlı ve bu ağ topolojisi kapsamındaki tüm yönlendirme Azure yol tabloları (UDR) aracılığıyla denetlenir. Giden tüm trafiği bir vnet'ten diğer vnet'e veya şirket içi veri merkezine NVA yönlendirilir.

![Şirket içi-Azure'a yük devretme önce ExpressRoute ile](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

### <a name="hub-and-spoke-peering"></a>Hub ve bağlı bileşen eşleme

Hub eşliği için bağlı bileşen yapılandırması aşağıdaki gibidir:
-   Sanal ağ adresi izin ver: etkin
-   İletilen trafiğe izin: etkin
-   Ağ geçidi transit izin ver: devre dışı
-   Ağ geçitleri kaldırmak: etkin

 ![Hub eşleme yapılandırmasını bağlı bileşen](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

Hub'ın eşlemesi bağlı bileşen yapılandırması aşağıdaki gibidir:
-   Sanal ağ adresi izin ver: etkin
-   İletilen trafiğe izin: etkin
-   Ağ geçidi transit izin ver: etkin
-   Ağ geçitleri kaldırmak: devre dışı

 ![Hub'ın eşleme yapılandırmasını bağlı bileşen](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="enabling-replication-for-the-deployment"></a>Dağıtım için çoğaltmayı etkinleştirme

Yukarıdaki kurulumu için ilk [olağanüstü durum kurtarma ayarlamak](azure-to-azure-tutorial-enable-replication.md) Site RECOVERY'yi kullanarak her bir sanal makine için. Site Recovery, çoğaltma üzerinde hedef bölgesi (alt ağlar ve ağ geçidi alt ağları dahil) sanal ağlar oluşturmak ve kaynak ve hedef sanal ağlar arasında gerekli eşlemeleri oluşturma. Ayrıca, hedef tarafı ağları ve alt ağlar önceden oluşturabilir ve aynı çoğaltma etkinleştirirken kullanın.

Site Recovery, yol tablolarını, sanal ağ geçitlerini, sanal ağ geçidi bağlantıları, sanal ağ eşlemesi, veya herhangi diğer ağ kaynaklarına veya bağlantıları yinelemez. Bunlar ve diğer kaynakların parçası [çoğaltma işlemi](azure-to-azure-architecture.md#replication-process) sırasında veya yük devretme önce oluşturulan ve ilgili kaynaklarına bağlı olması gerekir. Azure Site Recovery'nin güçlü kullanabilirsiniz [kurtarma planlarına](site-recovery-create-recovery-plans.md) oluşturma ve ek kaynaklar Otomasyon betikleri kullanarak bağlanma otomatik hale getirmek için.

Varsayılan olarak, çoğaltma trafiği Azure sınır bırakmaz. Genellikle, NVA dağıtımları NVA giden Internet akışına zorlar varsayılan yol (0.0.0.0/0) tanımlayın. Tüm çoğaltma trafiği NVA geçerse bu durumda, Gereci kısıtlanan. Aynı Ayrıca varsayılan yolların şirket dağıtımları için tüm Azure VM trafik yönlendirme için kullanıldığında uygulanır. Öneririz [bir sanal ağ hizmeti uç noktası oluşturulurken](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) , sanal ağ "Depolama için" böylece çoğaltma trafiğini Azure sınır bırakmaz.

## <a name="failover-models-with-expressroute"></a>ExpressRoute ile yük devretme modelleri

Azure sanal makineleri farklı bir bölgeye yük devredildi, mevcut kaynak sanal ağ ExpressRoute bağlantısı otomatik olarak hedef sanal ağ üzerinde kurtarma bölge aktarılmaz. Yeni bir bağlantı, ExpressRoute hedef sanal ağa bağlanmak için gereklidir.

Ayrıntılı olarak aynı coğrafi kümedeki herhangi bir Azure bölgesine Azure sanal makineleri çoğaltabilirsiniz [burada](azure-to-azure-support-matrix.md#region-support). Seçilen hedef Azure bölgesi kaynağı olarak aynı coğrafi bölge içindeki değilse, kaynak ve hedef bölgesini bağlantısı için tek bir expressroute bağlantı hattı kullanıyorsanız, ExpressRoute Premium etkinleştirmeniz gerekir. Daha fazla ayrıntı için [ExpressRoute konumları](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) ve [ExpressRoute fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="two-expressroute-circuits-in-two-different-expressroute-peering-locations"></a>İki farklı konumlarda ExpressRoute eşleme iki ExpressRoute bağlantı hatları
-   Bu yapılandırma, birincil expressroute bağlantı hattı arızasına karşı ve ayrıca ExpressRoute eşleme konumlarına etkisi ve birincil expressroute bağlantı hattı kesintiye büyük ölçekli bölgesel afetler karşı güvence altına almaya istiyorsanız yararlıdır.
-   Normalde üretim ortamına bağlı hattı birincil hattı ve ikincil hattı olduğu gibi bir hatasız ve daha düşük bant genişliği genellikle kullanılır. Bant genişliğini ikincil ikincil birincil olarak gerekir zaman bir olağanüstü durum olayı artırılabilir.
-   Bu yapılandırma ile genel kurtarma zamanı azaltır, ikincil bir expressroute bağlantı hattı bağlantılardan hedef sanal ağ için yük devretme sonrası veya yerleşik ve olağanüstü durum bildirimi için hazır bağlantınız kurabilirsiniz. Hem birincil hem de eşzamanlı bağlantıların ve hedef bölgesini sanal ağlar ile yönlendirme şirket içi ikincil hattı ve yalnızca yük devretme sonrasında bağlantı kullandığından emin olmak.
-   Site Recovery ile korunan VM'ler için kaynak ve hedef sanal ağlar aynı veya farklı IP adreslerini, gereksinimi başına yük devretme sırasında olabilir. Her iki durumda da, yük devretme öncesinde ikincil bağlantıları kurulabilir.

### <a name="two-expressroute-circuits-in-the-same-expressroute-peering-location"></a>İki ExpressRoute bağlantı hatları ile aynı konumda ExpressRoute eşleme
-   Bu yapılandırma, birincil expressroute bağlantı hattı hatasına karşı güvence altına, ancak büyük ölçekli bölgesel afetler karşı ExpressRoute eşleme konumlarına etkileyebilir değil. İkinci ile birincil ve ikincil devreler etkilenmiş.
-   IP adresleri ve bağlantıları için başka koşullar ile aynı önceki durumda kalır. Eşzamanlı bağlantılarından şirket içi veri merkezi birincil bağlantı hattı ile sanal ağ kaynak ve hedef sanal ağ ikincil hattı ile olabilir. Hem birincil hem de eşzamanlı bağlantıların ve hedef bölgesini sanal ağlar ile yönlendirme şirket içi ikincil hattı ve yalnızca yük devretme sonrasında bağlantı kullandığından emin olmak.
-   Aynı eşleme konumunda devreler oluşturduğunuzda aynı sanal ağa her iki bağlantı hatları bağlanamıyor.

### <a name="single-expressroute-circuit"></a>Tek bir expressroute bağlantı hattı
-   Bu yapılandırma ExpressRoute eşleme konumu etkileyen bir büyük ölçekli bölgesel afet karşı güvence altına değil.
-   Tek bir expressroute bağlantı hattı ile kaynağına bağlanmak ve aynı IP adresi alanı hedef bölgesi üzerinde kullanılırsa, aynı anda bağlantı hattı için sanal ağlar hedef.
-   Aynı IP adresi alanı hedef bölgesi üzerinde kullanıldığında, kaynak tarafı bağlantı kesilmesi gerekir ve bundan sonra hedef tarafı bağlantı kuruldu. Bu bağlantı değişiklik kurtarma planının bir parçası betiği yazılabilir.
-   Birincil bölge erişilemiyorsa bölgesel bir hata, bağlantıyı kesme işlemi başarısız olabilir. Hedef sanal ağda aynı IP adresi alanı kullanıldığında, bu tür bir kesinti bağlantı oluşturma hedef bölgeye etkileyebilir.
-   İki eşzamanlı bağlantı aynı adres alanına bağlanmaya çalışırsanız hedefte bağlantı oluşturma işlemi başarılı olur ve daha sonra birincil bölge kurtarır, paket düşme karşılaşıyor. Paket düşme önlemek için birincil bağlantı hemen sonlandırılmalıdır. Birincil bağlantı can yeniden birincil bölge için sanal makinelerin POST geri dönme ikincil bağlantıyı kesmeden sonra kurulabilir.
-   Farklı bir adres alanı hedef sanal ağda kullanılırsa, ardından aynı anda kaynağına bağlanabilir ve sanal ağ aynı expressroute bağlantı hattı bağlantılarını hedef.

## <a name="recovering-azure-deployments"></a>Azure dağıtımları kurtarma
İki farklı eşleme konumları ve korumalı Azure sanal makinelerin özel IP adreslerinin bekletme farklı iki ExpressRoute bağlantı hatları ile yük devretme modeli göz önünde bulundurun. Azure Güneydoğu Asya hedef kurtarma bölgedir ve ikincil bir ExpressRoute bağlantı hattı bağlantı Singapur bir iş ortağı edge'de üzerinden kurulur.

Sanal makineler ve sanal ağlar, çoğaltma ek olarak tüm dağıtımın kurtarma otomatikleştirmek için diğer ilgili ağ kaynakları ve bağlantıları da oluşturulması gerekir. Önceki hub ve bağlı bileşen topolojisi aşağıdaki ek ağ için adımları sırasında veya sonrasında yapılması gereken [yük devretme](azure-to-azure-tutorial-failover-failback.md) işlemi:
1.  Azure ExpressRoute ağ geçidi hedef bölge hub sanal ağ oluşturun. ExpressRoute ağ geçidini expressroute bağlantı hattı hedef hub sanal ağa bağlanmak için gereklidir.
2.  Sanal ağ bağlantısı hedef hub sanal ağdan hedef expressroute bağlantı hattı oluşturun.
3.  Hedef bölgenin hub ve bağlı bileşen sanal ağlar arasında VNet eşlemeler ayarlayın. Eşleme özelliklerini hedef bölge ile aynı kaynak bölge olacaktır.
4.  VNet hub Udr'ler ve iki uç Vnet'ler ayarlayın. Özellikleri aynı IP adresleri kullanılırken hedef tarafı Udr'ler kaynak tarafında aynıdır. Farklı bir hedef IP adresleriyle Udr'ler uygun şekilde değiştirilmesi gerekir.

Yukarıdaki adımları parçası olarak betiği yazılabilir bir [kurtarma planı](site-recovery-create-recovery-plans.md). Uygulama bağlantı ve kurtarma süresi gereksinimlerine bağlı olarak, yukarıdaki adımları yük devretmeyi başlatmadan önce de tamamlanabilir.

Sanal makineler kurtarma ve diğer bağlantı adımları tamamlama işlemleri sonrasında, Kurtarma Ortamı'nı aşağıdaki gibi görünür: ![şirket içi-Azure'a yük devretme sonrasında ExpressRoute ile](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)

Basit bir topoloji hedef sanal makinelerde aynı IP ile tek expressroute bağlantı hattı ile Azure VM olağanüstü durum kurtarma için ayrıntılı [burada](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity).

## <a name="recovery-time-objective-rto-considerations"></a>Kurtarma süresi hedefi (RTO) değerlendirmeleri
Dağıtımınız için genel kurtarma süresini azaltmak için sağlama ve ek hedef bölgesini dağıtma öneririz [ağ bileşenleri](azure-vm-disaster-recovery-with-expressroute.md#enabling-replication-for-the-deployment) önceden sanal ağ geçitleri gibi. Kısa bir kapalı kalma süresi ek kaynaklar dağıtma ile ilişkili olan ve bu kesinti genel kurtarma süresini etkileyebilir değilse planlama sırasında için hesaba.

Normal çalıştıran öneririz [olağanüstü durum kurtarma ayrıntılarını](azure-to-azure-tutorial-dr-drill.md) korumalı dağıtımları. Bir detaylandırma çoğaltma stratejinizi veri kaybı veya kapalı kalma süresi olmadan doğrular ve üretim ortamınıza etkilemez. Bir detaylandırma çalışan kurtarma süresi hedefi olumsuz yönde etkileyebilir son dakika yapılandırma sorunları önler. Çoğaltma etkin olduğunda, ayarlanmış varsayılan ağ yerine test yük devretme için ayrı bir Azure VM ağ kullanmanızı öneririz.

Tek bir expressroute bağlantı hattı kullanıyorsanız, bölgesel afetler sırasında bağlantı kurulması sorunlarını önlemek için hedef sanal ağ için farklı bir IP adresi alanını kullanmanızı öneririz. Farklı IP adreslerini kullanarak kurtarılan üretim ortamınız için uygun değilse, çakışan IP adresleri ile iki sanal ağ bağlanamıyor gibi olağanüstü durum kurtarma ayrıntıya yük devretme sınaması farklı IP adreslerine sahip ayrı bir test ağı üzerinde yapılması gerekir Adres alanı aynı expressroute bağlantı hattına.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [ExpressRoute bağlantı hatları](../expressroute/expressroute-circuit-peerings.md).
- Daha fazla bilgi edinmek [ExpressRoute Yönlendirme etki alanları](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Daha fazla bilgi edinmek [ExpressRoute konumları](../expressroute/expressroute-locations.md).
- Daha fazla bilgi edinmek [kurtarma planlarına](site-recovery-create-recovery-plans.md) uygulama yük devretmeyi otomatikleştirmek için.
